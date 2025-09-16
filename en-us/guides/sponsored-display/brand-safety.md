---
title: Brand safety developer guide 
description: Learn how to use the Amazon Ads API for Sponsored Display Brand Safety features to protect your brand.
type: guide
interface: api 
tags:
    - Sponsored Display
keywords:
    - brand
    - brand safety
    - content categories
---

# Brand safety developer guide

The Sponsored Display [Brand Safety API](sponsored-display/3-0/openapi#tag/Brand-Safety-List) is available for vendors and sellers registered within Amazon Brand Registry in the US, CA, UK, DE, ES, FR, IT, NL, BR, MX, and UAE. Sponsored Display can run ads onsite and offsite on third-party sites and mobile apps. The brand safety functionality gives you more control over where your ads serve to protect your brand and block ads from delivering on third-party sites that don’t align with your brand. You can upload a list of domains / mobile applications where you want to prevent ads from showing in your Sponsored Display campaigns. This feature will be available at the advertiser level, allowing each advertiser to control where their ads are shown off Amazon. 

> [NOTE] Amazon Ads has a global brand safety policy that takes a variety of measures to prevent your ads from appearing on webpages and apps containing sensitive content, products, and services. 

Specifically, we don't allow ads to be served on third-party sites in the following content categories:

* Accidents and disasters
* Alcohol
* Excessive profanity and obscenities 
* Extreme violence
* Hate speech
* Illegal content
* Illicit and recreational drugs
* Online gambling
* Pornographic or sexually explicit content 
* Sale of firearms and ammunition 
* Terrorism
* Tobacco

Our lists are continuously updated from independent brand safety providers to filter out content from these categories. In addition, certain products are not permitted or are subject to additional requirements under our Restricted Products policy, and our Community Guidelines. Amazon does not display ads on certain products even when permitted under the Restricted Products policy, such as sexual wellness products or sensitive historic memorabilia.

When building your brand safety list, you do not need to enumerate all possible web sites since Amazon Brand Safety will provide protection for the categories listed above. Your list should include sites that don’t align with your brand or any other business reasons.

A single brand safety list is configured per advertiser. You can upload items to the brand safety list with increments of 10,000 items per request, where items can be either website or mobile applications.

The main API available to create and manage deny lists is `/sd/brandSafety/deny`. It contains the following operations:

1. Retrieve items: `GET /sd/brandSafety/deny`
2. Append items to list: `POST /sd/brandSafety/deny`
3. Delete list: `DELETE /sd/brandSafety/deny`

> [NOTE] The POST and DELETE operations are asynchronous while the GET operation is synchronous.

We do not support updating lists at this time. If you need to modify or remove items from the list, you will need to delete the list and re-upload the list. If you don’t have a local copy of the list, you can download the list using the `GET /sd/brandSafety/deny` endpoint.

When you perform the above operations, you will get a `requestId` that acknowledges that the request has been processed.

```JSON
{
 "requestId": "<string>"
}
```

You can then use the `GET /sd/brandSafety/:requestId/status` API to retrieve the status of the API passing in your `requestId` . When this API returns `COMPLETED` then the operation has finished but this API does not tell you the actual results of the operation. 

```JSON
{
 "status": "COMPLETED",
 "statusDetails": "some details"
}
```

You will need to use the `GET /sd/brandSafety/:requestId/results` API to retrieve the actual results from the operation. When deleting a list, this operation is not available. More details on validating a deleted list are described later in the document. You can examine individual domain entries to understand whether a particular domain was updated successfully.

```JSON
{
 "results": [
  {
   "status": "SUCCESS",
   "details": "<string>",
   "domainId": "<long>",
   "name": "<string>"
  },
  {
   "status": "SUCCESS",
   "details": "<string>",
   "domainId": "<long>",
   "name": "<string>"
  }
 ]
}
```

## Building the brand safety list

You can build a brand safety list that denies the following:

* Block the entire web site (e.g., `example.com`)
* Block sub-domains from a web site (e.g., `api.example.com` or `docs.example.com` )

> [NOTE] You cannot block individual web pages, such as index.html. When a website does not include a webpage, for example, `www.example.com/someURI`, it is generally assumed that the actual link is `www.example.com/someURI/index.html`. 

The format for web sites is `example.com` or `example.net` and must follow the domain naming standards per [here](https://en.wikipedia.org/wiki/Domain_name). Furthermore, only ASCII characters are supported. The list does not support character  representation from non-ASCII character sets.

Brand Safety Lists are created using the `POST /sd/brandSafety/deny` operation by including the following payload in the request.

```JSON
{
    "domains": [
        {
            "name": "example.com",
            "type": "WEBSITE"
        },
        {
            "name": "com.example.myapp",
            "type": "APP"
        },
        {
            "name": "1234567890",
            "type": "APP"
        }
    ]
}
```

* `example.com` would deny list all sub-domains, in addition to example.com.
* **com.example.myapp** would deny list the Android mobile application with the Android Id **com.example.myapp**
* **1234567890** would deny list the IOS application with the IOS id **1234567890.** 

You can add up to 10K entries into the list when using the API. If you need to send > 10K, you will need to send the items in another request.

## Instructions

_Prerequisites_:

* You must download Postman from [here](https://www.postman.com/downloads/).
* You must import the [Brand Safety collection](https://d3a0d0y2hgofx6.cloudfront.net/openapi/en-us/sponsored-display/postman/Brand_Safety.postman_collection.json) and [environment file](https://d3a0d0y2hgofx6.cloudfront.net/openapi/en-us/sponsored-display/postman/Amazon_SD_API_postman_environment.json) into your workspace.

In this section, you will run API requests to manage brand safety lists. A postman collection has been provided that contains pre-configured requests. When you run requests, it will capture response information as variables, to be used in subsequent requests. Before you run these requests, you will need to fetch the Refresh Token using the **OAuth Refresh Token** request or manually add one to the Postman script.

For more information on setting up your environment, see [here](guides/sponsored-display/tutorials/postman).

### Use Case 1: Add Domains / Mobile Apps to Brand Safety List

Use the operation `POST /sd/brandSafety/deny` to add items to the Brand Safety List. 

> [NOTE] If you need to convert an existing domain list from a CSV file to JSON, several free tools are readily available to convert between the formats.

```JSON
{
    "domains": [
        {
            "name": "foo.com",
            "type": "WEBSITE"
        },
        {
            "name": "api.example.com",
            "type": "WEBSITE"
        },
        {
            "name": "com.example.app",
            "type": "APP"
        },
        {
            "name": "1234567890",
            "type": "APP"
        }
    ]
}
```

The response returns the following:

```JSON
{
    "requestId": "a2fd14cb81c72cb32ac071e13b105f14"
}
```

When you check the status immediately using the operation `GET /sd/brandSafety/:requestId/status.` You will likely see the following:

```JSON
{
    "status": "IN_PROGRESS",
    "statusDetails": "Your request is still in progress. Please check again later."
}
```

After a few minutes, when you try the request again (depending on the size of the list), you will see the following:

```JSON
{
    "status": "COMPLETED",
    "statusDetails": "Your request has been completed. For POST requests, you can check your request results at /sd/brandSafety/a2fd14cb81c72cb32ac071e13b105f14/results to check the status of each domain in the request. For DELETE requests you can call GET /sd/brandSafety/deny to verify that your Brand Safety Deny List is now empty."
}
```

You can examine the detailed response for the `requestId` using the operation: `GET /sd/brandSafety/:requestId/results`

```JSON
{
    "results": [
        {
            "status": "SUCCESS",
            "details": "Domain successfully added",
            "domainId": 5750546536593170213,
            "name": "foo.com"
        },
        {
            "status": "SUCCESS",
            "details": "Domain successfully added",
            "domainId": 6337651429473114930,
            "name": "api.example.com"
        },
        {
            "status": "SUCCESS",
            "details": "Domain successfully added",
            "domainId": 8394201756681458405,
            "name": "com.example.app"
        },
        {
            "status": "SUCCESS",
            "details": "Domain successfully added",
            "domainId": 8662947689235204460,
            "name": "1234567890"
        }
    ]
}
```

You can verify the full domain list using the operation: `GET /sd/brandSafety/deny`

```JSON
{
    "domains": [
        {
            "domainId": 5750546536593170213,
            "name": "foo",
            "type": "WEBSITE",
            "state": "ENABLED",
            "createdAt": "2021-04-27T18:48:20.253Z",
            "lastModified": "2021-04-27T18:48:20.253Z"
        },
        {
            "domainId": 6337651429473114930,
            "name": "api.example.com",
            "type": "WEBSITE",
            "state": "ENABLED",
            "createdAt": "2021-04-27T18:48:20.253Z",
            "lastModified": "2021-04-27T18:48:20.253Z"
        },
        {
            "domainId": 8394201756681458405,
            "name": "com.example.app",
            "type": "APP",
            "state": "ENABLED",
            "createdAt": "2021-04-27T18:48:20.253Z",
            "lastModified": "2021-04-27T18:48:20.253Z"
        },
        {
            "domainId": 8662947689235204460,
            "name": "1234567890",
            "type": "APP",
            "state": "ENABLED",
            "createdAt": "2021-04-27T18:48:20.253Z",
            "lastModified": "2021-04-27T18:48:20.253Z"
        }
    ]
}
```

### Use Case 2: Delete Brand Safety List

If you want to delete the Brand Safety List, you will simply call the operation `DELETE /sd/brandSafety/deny`

The response will return the following: 

```JSON
{
    "requestId": "011686d6c372870e46e598894b7ce832"
}
```

After a few minutes, when you check the status using the operation `GET /sd/brandSafety/:requestId/status`  (depending on the size of the list), you will see the following:

```JSON
{
    "status": "COMPLETED",
    "statusDetails": "Your request has been completed. For POST requests, you can check your request results at /sd/brandSafety/a2fd14cb81c72cb32ac071e13b105f14/results to check the status of each domain in the request. For DELETE requests you can call GET /sd/brandSafety/deny to verify that your Brand Safety Deny List is now empty."
}
```

The operation: `GET /sd/brandSafety/:requestId/results` is not available for the DELETE operation; however, you can call the `GET /sd/brandSafety/deny` operation and verify that the domain list is empty.

```JSON
{
    "domains": []
}
```

### Use Case 3: Update the Brand Safety List

If you need to update items in the list, you will need to delete the list using the operation `DELETE /sd/brandSafety/deny` and then create items using the operation `POST /sd/brandSafety/deny`.  You can follow instructions Use Case 2 for delete and then steps in Use Case 1 to create items within the brand safety list.

**Appendix**:

* When a user adds domains to their Brand Safety Deny List, the request is processed asynchronously, and a requestId is provided to the user. 
* This requestId can be used to view the request results for up to 90 days from when the request was submitted. The results provide the status of each domain in the given request. 
* Request results may contain multiple pages. This endpoint will only be available once the request has completed processing. 
* To see the status of the request you can call [GET /sd/brandSafety/{requestId}/status](sponsored-display/3-0/openapi#tag/Brand-Safety-List/operation/getRequestStatus). **Note** that this endpoint only lists the results of POST requests to [GET /sd/brandSafety/deny](sponsored-display/3-0/openapi#tag/Brand-Safety-List/operation/listDomains). It does not reflect the results of `DELETE` requests.
* Currently we do not accept Unicode domains. We do accept [Punycode](https://en.wikipedia.org/wiki/Punycode) domains.

When a user modifies their Brand Safety Deny List, the request is processed asynchronously, and a requestId is provided to the user. This requestId can be used to check the status of the request for up to 90 days from when the request was submitted.
