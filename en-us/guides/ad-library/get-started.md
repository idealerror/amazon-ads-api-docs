---
title: Ad Library getting started
description: Getting started on how to use the Ad Library API
type: guide
interface: api
tags:
    - Ad library
keywords:
    - getting started
    - ad library
---

# Getting started with the Ad Library API

This tutorial outlines how to get access and make your first call using the Ad Library API.

## Before you begin

* Complete the Amazon Ads API [Onboarding](guides/onboarding/overview) and [Getting started](guides/get-started/overview) processes to obtain your access token. Since the Ad Library is a public API, users only need to follow step 1 of the onboarding process: [Create a Login with Amazon application](guides/onboarding/create-lwa-app) and steps 1 and 2 for the getting started process: [Create an authorization grant](guides/get-started/create-authorization-grant) and [Retrieve access and refresh tokens](guides/get-started/retrieve-access-token).

>[NOTE] When creating an [authorization URL](guides/get-started/create-authorization-grant), the `scope` query parameter must be set to `profile`. This is required for the Ad Library API.

Example of the authorization URL:

```
https://www.amazon.com/ap/oa?client_id=amzn1.application-oa2-client.12345678901234567890&scope=profile&response_type=code&redirect_uri=https://amazon.com
```


## Make your first call

To test your first call, we will use the POST method of the [/adRepository/ads/list](ad-library#operation/ListAds) endpoint to query ads data.

### Query Parameters

|Name	|Description	|
|---	|---	|
| `advertiserName` <br /> string	|The person or entity on whose behalf the ad or affiliate marketing content is presented. This parameter will limit results to those matching the requested advertiser or creator name.	|
|`advertisementPurpose` <br /> string	|This parameter will limit results to those matching the requested advertisement purpose. This includes the name of the product, service, or brand within the ad or affiliate marketing content. 	|
|`siteName`<br /> string	|When specified, limits results based on their delivery to a particular Amazon EU store (e.g., amazon.de, amazon.fr).	|
|`deliveryBeforeDateUtc` <br /> string	|The specified end date of the delivered ads. The date string is specified in ISO format (YYYY-MM-DD) in UTC timezone. For example: 2020-12-16.	|
|`deliveryAfterDateUtc` <br /> string	|The specified start date of the delivered ads. The date string is specified in ISO format (YYYY-MM-DD) in UTC timezone. For example: 2020-12-16	|
|`isRestricted`<br /> boolean	|Whether the advertisement was paused, removed or suppressed based on alleged illegality or incompatibility with terms and conditions. Limits results based on their restriction status.	|
|`nameMatchType` <br /> enum <br /> {CONTAINS, EXACT_MATCH}	|Default behavior will perform a substring search (i.e., CONTAINS).	|
|`maxResults` <br /> integer	|Defaults to 10, with supported values between 1 and 1,000. If the size of the result set is larger than the limit, callers will retrieve additional results through pagination parameters.	|
|`nextToken` <br /> string	|Pagination token to retrieve the next set of results. Callers must make use of the token provided in a previous response to use this parameter.	|

### Response Schema

|Name	|Description	|
|---	|---	|
|`id` <br /> string	|Unique identifier for the ad to support lookups in the Ad Library.	|
|`advertisementPurpose` <br /> string	|Name of the product, service or brand within the ad or affiliate marketing content.	|
|`type` <br /> string	|Displays content type of the ad (AD or AFFILIATE\_MARKETING\_CONTENT).	|
|`subjectMatterUrl` <br /> string	|URL for the affiliate marketing content. 	|
|`advertiserName` <br /> string	|The person or entity on whose behalf the ad or affiliate marketing content is presented.	|
|`payerName` <br /> string	|The person or entity who paid for the ad.	|
|`deliveryBeforeDateUtc` <br /> string	|The specified end date of the delivered ads. The date string is specified in ISO format (YYYY-MM-DD) in UTC timezone. For example: 2020-12-16.	|
|`deliveryAfterDateUtc` <br /> string	|The specified start date of the delivered ads. The date string is specified in ISO format (YYYY-MM-DD) in UTC timezone. For example: 2020-12-16	|
|`targetingMethods` <br /> list of strings	|This field lists the main parameters used to determine the recipient(s) of the ad (e.g., their estimated location).	|
|`isRestricted` <br /> boolean	|Whether the ad was paused, removed or suppressed based on alleged illegality or incompatibility with terms and conditions.	|
|`restrictionCategory` <br /> string	|Reason(s) why an ad was paused, removed, or suppressed.	|
|`restrictionDetectionAutomated` <br /> boolean	|Describes whether automation was used to identify the restricted ad. 	|
|`illegalContentReport` <br /> boolean	|Describes whether the ad was removed as a result of an illegal content report. 	|
|`totalRecipientsRange` <br /> map	|Range of the number of impressions delivered by the ad. 	|
|`totalRecipientsRangeBySite` <br /> map	|Range of the number of impressions delivered by the ad by Amazon EU store (e.g., amazon.de, amazon.fr). The return value is in form of ```{siteName: {min: rangeMin, max: rangeMax}}```. For example: <br /> ``` "amazonDe":  { "max": 50000, "min": 0 } ```|
|`totalResults` <br /> number	|Total number of return results for the query input.	|
|`nextToken` <br /> string	|Operations that return paginated results include a pagination token in this field. To retrieve the next page of results, call the same operation and specify this token in the request. If the nextToken field is empty, there are no further results.	|

### Request payload

The following is an example of the message body with the parameters `advertiserName` and `deliveryAfterDateUtc` to retrieve Amazon Ads data:

```
{
  "advertiserName": "Amazon",
  "deliveryAfterDateUTC": "2024-05-01"
}
```

When executed, Ad Library data which was delivered after the desired date will be returned. The default number of results returned in a single request is 10, but you can use the `maxResults` parameter to specify the desired number of results for each query. The POST ad library endpoint can be called using one of the following approaches:

* Using cURL
* Using Postman

### Using cURL

You can use cURL to query the Amazon Ads data using the EU URL prefix.

#### Sample request

```
curl --location --request POST 'https://advertising-api-eu.amazon.com/adRepository/ads/list' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-ClientId: xxxxxx' \
--data '{
  "advertiserName": "Amazon",
  "deliveryAfterDateUTC": "2024-04-01"
}'
```

#### Sample response

```
{
  "ads": [
    {
      "advertisementPurpose": "TAKE SPORT- Kinesio, Kinesio tape, Cerotto kinesiologico, Tape kinesiologico, Kinesio taping. 1 rotolo, 5 x 5m, 95% Cotone 5% Spandex, elastico. (nero/black)",
      "advertiserName": "FCOM srl",
      "deliveryAfterDateUtc": "2023-07-30T00:00:00Z",
      "deliveryBeforeDateUtc": "2024-04-01T00:00:00Z",
      "id": "019742ba585f2b8ff1b1c047a19ac6d679c1a944",
      "illegalContentReport": false,
      "isRestricted": false,
      "payerName": "FCOM srl",
      "restrictionDetectionAutomated": false,
      "subjectMatterUrl": "NOT_AVAILABLE",
      "targetingMethods": [
        "LOCATION",
        "PAST_ACTIVITY",
        "ADS_PREFERENCES",
        "SEARCH_TERMS"
      ],
      "totalRecipientsRange": {
        "max": 50000,
        "min": 0
      },
      "totalRecipientsRangeBySite": {
        "amazonIt": {
          "max": 50000,
          "min": 0
        }
      },
      "type": "AD"
    }
  ],
  "nextToken": "[1.0,1711929600000,\" FCOM srl\",\"3eb88100f3fc53ffbaf2cb1dce0044da46dbc87c\"]",
  "totalResults": 611485476
}
```



### Using Postman

In this example, we'll be using Postman, an API platform that has an easy-to-use graphical user interface (GUI).
 
>[NOTE] Postman is not an Amazon-affiliated tool. You can use any API tool for this tutorial (such as the cURL command-line tool), but we chose Postman for its intuitive GUI.

#### Setup

1. Download and [install Postman](https://www.postman.com/downloads/).
2. [Complete the setup for the Amazon Ads API Postman collection](guides/get-started/using-postman-collection). Ensure that the user account you use to generate the authorization code has access to the advertising account you want to query.
3. Click **Add request** within the collection to create a new request. This will inherit the authorization parameters of the collection.
4. Name your request as desired (for example, Execute ad hoc workflow). By default, Postman creates a GET request. Make sure the HTTP method is set to POST.
5. In the request URL field, enter the base Amazon Ads API URL and append the endpoint path for the Ad Library API (/adRepository/ads). Using the EU prefix, your request will look something like the example below, with your unique instance being called as a variable:
    `https://advertising-api-eu.amazon.com/adRepository/ads/list`
6. If successful, you will receive a response with the HTTP status code 200 OK and the following response parameters:

```
{
  "ads": [
    {
      "advertisementPurpose": "TAKE SPORT- Kinesio, Kinesio tape, Cerotto kinesiologico, Tape kinesiologico, Kinesio taping. 1 rotolo, 5 x 5m, 95% Cotone 5% Spandex, elastico. (nero/black)",
      "advertiserName": "FCOM srl",
      "deliveryAfterDateUtc": "2023-07-30T00:00:00Z",
      "deliveryBeforeDateUtc": "2024-04-01T00:00:00Z",
      "id": "019742ba585f2b8ff1b1c047a19ac6d679c1a944",
      "illegalContentReport": false,
      "isRestricted": false,
      "payerName": "FCOM srl",
      "restrictionDetectionAutomated": false,
      "subjectMatterUrl": "NOT_AVAILABLE",
      "targetingMethods": [
        "LOCATION",
        "PAST_ACTIVITY",
        "ADS_PREFERENCES",
        "SEARCH_TERMS"
      ],
      "totalRecipientsRange": {
        "max": 50000,
        "min": 0
      },
      "totalRecipientsRangeBySite": {
        "amazonIt": {
          "max": 50000,
          "min": 0
        }
      },
      "type": "AD"
    }
  ],
  "nextToken": "[1.0,1711929600000,\"FCOM srl\",\"3eb88100f3fc53ffbaf2cb1dce0044da46dbc87c\"]",
  "totalResults": 611485476
}
```


