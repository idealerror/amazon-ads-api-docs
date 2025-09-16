---
title: Make your first call
description: How to make your first call
type: guide
interface: api
---
# Make your first call

The tutorial in this section will walk you through the steps to set up your header parameters and make your first AMC call for advertisers.

> [NOTE] If you are a sponsored ads advertiser, you can use the AMC instant self-service access API to automatically create an AMC instance associated with your sponsored ads account and access AMC within minutes. After you onboard to Amazon API, follow the instructions in [Instant self-service access to AMC](guides/amazon-marketing-cloud/get-started/instant-access-to-amc) or use this tutorial for more in-depth steps to make your first call.

Before you can begin with any of the tutorials, you must have:

* Either applied for access to AMC via your AdTech AE and Amazon has provisioned your AMC account, or have a sponsored ads account (if you are using the instant self-service access API)
* Established a client application on Amazon Ads API through Login with Amazon
* Completed all the prerequisites as described in the [Get started](guides/amazon-marketing-cloud/get-started/get-started) section, and [generated access token to gain access to Amazon Ads API](guides/account-management/authorization/access-tokens#using-access-tokens-in-the-amazon-ads-api).

To make your first call, we will use the GET method of the `amc/instances` to retrieve the instance(s) associated with your account.

## Prerequisites

To make your first call, you will need to know the following:

- [Your base URL](#ads-api-base-urls)
- [Your marketplace identifiers](#marketplace)
- [Header parameters](#header-parameters)

### Ads API base URLs

All of the APIs share a common [URL prefix](guides/get-started/first-call#url-prefixes)
depending on the region. These URLs are:

| Region        | Base URL                                                                    |
| ------------- | --------------------------------------------------------------------------- |
| North America | [https://advertising-api.amazon.com](https://advertising-api.amazon.com)       |
| Europe        | [https://advertising-api-eu.amazon.com](https://advertising-api-eu.amazon.com) |
| APAC          | [https://advertising-api-fe.amazon.com](https://advertising-api-fe.amazon.com) |

> [NOTE] Any of the above URLs can be used and all work "globally". You are encouraged to use the endpoint geographically closest to where calls are being made from.

### Marketplace

The marketplace identifiers called in requests through the Amazon-Advertising-API-MarketplaceId header parameter are tied to a country, as listed in the tables below:

#### North America

| Country                  | Marketplace ID | Country code |
| :----------------------- | :------------: | :----------: |
| Brazil                   | A2Q3Y263D00KWC |      BR      |
| Canada                   | A2EUQ1WTGCTBG2 |      CA      |
| Mexico                   | A1AM78C64UM0Y8 |      MX      |
| United States of America | ATVPDKIKX0DER |      US      |

#### Europe

| Country              | Marketplace ID | Country code |
| :------------------- | :------------: | :----------: |
| Germany              | A1PA6795UKMFR9 |      DE      |
| Spain                | A1RKKUPIHCS9HS |      ES      |
| France               | A13V1IB3VIYZZH |      FR      |
| Italy                | APJ6JRA9NG5V4 |      IT      |
| Netherlands          | A1805IZSGTT6HS |      NL      |
| Sweden               | A2NODRKZP88ZB9 |      SE      |
| Turkey               | A33AVAJ2PDY3EV |      TR      |
| United Kingdom       | A1F83G8C2ARO7P |      UK      |
| Saudi Arabia         | A17E79C6D8DWNP |      SA      |
| United Arab Emirates | A2VIGQ35RCS4UG |      AE      |

#### APAC

| Country   | Marketplace ID | Country code |
| :-------- | -------------- | ------------ |
| Australia | A39IBJ37TRP1C6 | AU           |
| Japan     | A1VC38T7YXB528 | JP           |
| India     | A21TJRUUN4KGV  | IN           |

### Header parameters

In addition to the individual API parameters, the following header parameters are required for most of your calls.

| Header                               | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| :----------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Amazon-Advertising-API-ClientId      | The client ID related to a Login with Amazon application.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Amazon-Advertising-API-MarketplaceId | The[marketplace identifier](#marketplace) for the marketplace in the request. Marketplaces are tied to the country.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Amazon-Advertising-API-AdvertiserId  | This is the sponsored ads entity ID if you are using instant self-service access API.  If not, this is an AMC account identifier provided to you by your Amazon Ads account executive when you onboarded AMC.   |
| Authorization                        | Login with Amazon token in the form of Bearer {token}                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Content-type                         | application/json                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |



* The **Amazon-Advertising-API-MarketplaceId** parameter is exclusive to AMC APIs and is not required for other Amazon Ads APIs. Make sure you specify the marketplace of your [account](guides/amazon-marketing-cloud/admin/account-management#view-amc-account-identifier) when calling AMC APIs. 
* Similarly, the **Amazon-Advertising-API-AdvertiserId** is a mandatory header parameter, required for all AMC APIs. This is the sponsored ads entity ID if you are using the instant self-service access API. If not, this is an AMC account identifier provided to you by your Amazon Ads account executive when you onboarded AMC. Alternatively, to determine the AMC account identifier, you can access the AMC console with your user identifier and password, and grab the entity identifier value that is displayed in the URL. The alphanumeric code that is prefixed with ENTITY is your account identifier.  You can also use the [GET operation of the `/amc/accounts](guides/amazon-marketing-cloud/admin/account-management#view-amc-account-identifier)` endpoint that returns a list of all the accounts your client Id has access to. An example of an account identifier is: ENTITY1AA1AA11AAA1.

> [NOTE] When completing the Login with Amazon flow, ensure that you have access to the AMC instance. If you do not have access to the AMC instance, you will receive an unauthorized response when attempting to call the AMC APIs.

## Your first call

Now, let's retrieve all the instances in your account using one of the following approaches:

- Using cURL
- Using Postman

### Using cURL

You can use cURL to create retrieve all instances in your account using the North America URL prefix.

#### Sample request

```
curl --location 'https://advertising-api.amazon.com/amc/instances' \
--header 'Authorization: Bearer {token}' \
--header 'Amazon-Advertising-API-ClientID: {Login with Amazon Client ID}' \
--header 'Amazon-Advertising-API-AdvertiserId: {Sponsored ads entity ID or AMC entity ID}' \
--header 'Amazon-Advertising-API-MarketplaceId: {Marketplace ID}' 

```

#### Sample response

```
{
   "instances": [
        {
            "instanceId": "amc-instance01",
            "instanceName": "my-amc-instance",
            "instanceType": "STANDARD",
            "customerCanonicalName": "My instance",
            "creationStatus": "COMPLETED",
            "creationDatetime": "2023-11-22T16:48:03.936Z",
            "s3BucketName": "amc-S3bucket-for-my-instance",
            "awsAccountId": "",
            "entities": [
                "NA"
            ],
            "dataUploadAwsAccountId": "123456589100",
            "apiEndpoint": "https://advertising-api.amazon.com",
            "acrCollaboration": null,
            "optionalDatasets": [
                {
                    "label": "OPTIONAL_DATASET_1",
                    "activationTime": "2023-11-22T16:09:08.957Z"
                },
                {
                    "label": "OPTIONAL_DATASET_2",
                    "activationTime": "2023-11-22T16:09:08.957Z"
                }
            ],
            "advertiserTypes": [
                "SPONSORED_ADS",
                "DISPLAY"
            ]
        },
}
```

### Using Postman

In this example, we'll be using Postman, an API platform that has an easy-to-use graphical user interface (GUI).

> [NOTE] Postman is not an Amazon-affiliated tool. You can use any API tool for this tutorial (such as the cURL command-line tool), but we chose Postman for its intuitive GUI.

1. Download and [install Postman](https://www.postman.com/downloads/).
2. Open Postman and click **Create new collection**. The collection will be used to group our requests and for inheritance of   authorization parameters, saving us a lot of time and effort.
3. Name your collection (for example, Amazon Ads APIs).
4. Set up your environment and collection by following [these](guides/postman) instructions.
5. [Generate an authorization grant code](guides/get-started/using-postman-collection#generate-an-authorization-grant-code-) and [retrieve access and refresh tokens](guides/get-started/using-postman-collection#retrieve-access-and-refresh-tokens) to complete your authorization.

Your authorization credentials will expire after 24-hours. When the authorization credentials expire, you will receive an HTTP 403 forbidden response when calling the API. In order to update, you'll need to re-generate your authorization credentials and update the parameters in the collection's Authorization tab with the regenerated details. Refer to [Generate API access credentials](guides/get-started/retrieve-access-token#call-the-authorization-url-to-request-access-and-refresh-tokens) for more information about generating the tokens.

6. Click **Add request** within the newly created collection to create a new request. This will inherit the authorization parameters of the collection.
7. Name your request (for example, Retrieve all instances).

By default, Postman creates a GET request.

8. In the request URL field, enter the base AMC API URL and append the endpoint path for the workflow executions resource (/instances). Using the North America URL prefix, your request will look something like the example below.

`https://advertising-api.amazon.com/amc/instances`

9. On the Headers tab, add the **mandatory [header parameters](#header-parameters)** with the appropriate values for each
10. Click **Send**. If successful, you will receive a response with the HTTP status code 200 OK and the following response parameters:

```
{
   "instances": [
        {
            "instanceId": "amc-instance01",
            "instanceName": "my-amc-instance",
            "instanceType": "STANDARD",
            "customerCanonicalName": "My instance",
            "creationStatus": "COMPLETED",
            "creationDatetime": "2023-11-22T16:48:03.936Z",
            "s3BucketName": "amc-S3bucket-for-my-instance",
            "awsAccountId": "",
            "entities": [
                "NA"
            ],
            "dataUploadAwsAccountId": "123456589100",
            "apiEndpoint": "https://advertising-api.amazon.com",
            "acrCollaboration": null,
            "optionalDatasets": [
                {
                    "label": "OPTIONAL_DATASET_1",
                    "activationTime": "2023-11-22T16:09:08.957Z"
                },
                {
                    "label": "OPTIONAL_DATASET_2",
                    "activationTime": "2023-11-22T16:09:08.957Z"
                }
            ],
            "advertiserTypes": [
                "SPONSORED_ADS",
                "DISPLAY"
            ]
        },
}
```

If you receive a response code other than 200, check if your credentials are authenticated and that your token has not expired.

If you receive the message "Terms and conditions must be accepted to continue," you must accept the terms and conditions before you can proceed. Log into the AMC UI using the email address that is associated with Login with Amazon. If you haven't logged into the AMC UI before, you will need to create an account using the email address associated with Login with Amazon. AMC terms and conditions must be accepted once in each region (NA, EU, and FE) where the AMC instance is going to be created and accessed.

## Next steps

For instances created using instant self-service access API, you can begin using the AMC APIs to run [reporting workflows](guides/amazon-marketing-cloud/reporting/workflow-management-service) and [create audiences](guides/amazon-marketing-cloud/audiences/rule-based-audiences). Note that adding and modifying accounts, users, and instance scope is not supported for instances created using instant self-service access API.

For other instances, your organization's AMC admin can create instances and add advertisers to the instances. The admin can than add users to the account to view and analyze data in the assigned instance. For details on AMC admin functions, refer to the [AMC administration functions.](guides/amazon-marketing-cloud/admin/1_amc_administration).