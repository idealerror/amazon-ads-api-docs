---
title: How to build Sponsored Display contextual targeting campaigns 
description: Learn how to build contextual targeting campaigns using the Amazon Ads API for Sponsored Display.
type: guide
interface: api 
tags:
    - Sponsored Display
keywords:
    - contextual targeting
    - contextual targeting campaigns
    - postman
---

# How to build Sponsored Display contextual targeting campaigns

This how-to walks you through the steps to build Sponsored Display campaigns.

## Prerequisites

Follow the setup instructions [here](guides/sponsored-display/tutorials/postman). If you have previously setup the Sponsored Display Postman collections, you can skip these steps.

## Instructions

**Obtain an access token**

Execute the following steps to obtain a new access token. If you have already acquired an access token, but it has expired, you can use the `OAuth Refresh Token` request to replace the expired access token. Otherwise, follow the instructions in this step.

1. In the **collections** tab, select **Amazon SD API (Contextual)** to expand the subfolders.
2. In the subfolders, select **OAuth**.
3. Select the **GET OAuth Access Code** request.
4. Select the **send** button to send the request.
5. The response is a web page populated with a login form. Select **preview**.
6. Select **OAuth Form Login** to simulate a form login. This will use the email and password you configured in your environment earlier. Note that if you're logging in for the first time, you're redirected to a consent page to give permission.
7. The **OAuth Form Login** returns an authorization code. This code in an intermediary code that is returned to your **redirect_uri**. The Postman script copies this value to the correct variable for the next step.
8. Open the **OAuth Code to Token** request. Select **send**. An access token and refresh token are included in the response. For example:

  ```JSON
  {
      "access_token": "<access_token>",
      "refresh_token": "<refresh_token>",
      "token_type": "bearer",
      "expires_in": 3600
  }
  ```

9. You will also need a value for the `Amazon-Advertising-API-Scope` header. Select the **GET profile information** request and select **send**. The response includes the value in the `profileId` field.

**Create a Sponsored Display campaign**

To create a Sponsored Display campaign using Postman, follow these steps:

1. In the Postman collection, open the **create one or more campaigns** request. Select the **body** tab and modify the field values to reflect your campaign. For example:

  ```JSON
  [{
    "name": "My Contextual Targeting Campaign",
    "tactic": "T00020",
    "startDate": "20200810",
    "endDate": "20301201",
    "state": "enabled",
    "budget": 100.00,
    "budgetType": "daily",
    "costType": "cpc"
  }]
  ```

2. Click the **send** button and verify that your request is successful. For example:

  ```JSON
  [
    {
      "campaignId": 222756009537583,
      "code": "SUCCESS"
    }
  ]
  ```

3. Open the subsequent request. Get a requested campaign and select **send**. The value of `campaignId` from the previous response is copied automatically to the subsequent request.

  ```JSON
  {
      "campaignId": 222756009537583,
      "name": "My Contextual Targeting Campaign 2",
      "tactic": "T00020",
      "startDate": "20200812",
      "endDate": "20301201",
      "state": "paused",
      "budget": 100.00,
      "budgetType": "daily",
      "costType": "cpc",
      "deliveryProfile": "as_soon_as_possible"
  }
  ```

4. Repeat the same steps to create and retrieve the remaining components.

  - POST Creates one or more ad groups.
  - GET Gets a requested ad group.
  - POST Creates one or more product ads.
  - GET Gets a requested product ad.
  - POST Creates one or more targeting clauses.
  - GET Gets a targeting clause specified by identifier.

5. The final response from `GET Gets a targeting clause specified by identifier.` resembles the following:

  ```JSON
  {
      "adGroupId": 12345678900000,
      "expression": [
          {
              "type": "asinSameAs",
              "value": "B07V5HK595"
          }
      ],
      "expressionType": "manual",
      "resolvedExpression": [
          {
              "type": "asinSameAs",
              "value": "B07V5HK595"
          }
      ],
      "state": "enabled",
      "targetId": 1234567890
  }
  ```

**Modify existing campaigns using the Sponsored Display API**

You can also modify Sponsored Display campaigns to update values or delete resources. Expand the **Modify Campaigns** folder within the **Amazon SD API** collection to experiment with these requests and modify the campaigns based on your requirements. Each component contains a PUT (updating) and DELETE (archiving) resource.

**Reporting**

You can also request and download reports to understand detailed metrics about the performance of your campaigns. Expand the **Report** folder and examine the requests:

* **Create a report request**: initiates a request to generate a report. This operation returns a report identifier.
* **Get the status of a report previously requested**: check the status of the report using the report identifier.
* **Download a previously requested report**: downloads the completed reported using the report identifer.

## Automate testing of Sponsored Display Campaigns using Postman

In the previous sections, you created campaigns by calling the Sponsored Display API manually. You can also automate campaign creation, including creating campaigns in bulk using the **Postman Collection Runner**. 

1.	In Postman, select the **Runner** button at the top left-hand corner. This opens a new window.
2.	In **Collection Runner**, select the **Amazon SD API (Contextual)** collection.
3.	For **Environment**, select **Amazon SD API**.
4.	For the **RUN ORDER**, Deselect **All**. Then, select all requests starting from **OAuth Refresh Token**. This assumes you have a valid access token. If you do not, you can manually make the OAuth requests to get an access token as described in the previous sections. 
5.	Open **create one or more campaigns request**. Enter a unique name. If you don't enter a unique name, the first call will fail because of a duplicate campaign name.
6. Select **Run Amazon SD API**.
7. Verify the results. If the results are expected, you have successfully automated Sponsored Display campaign creation.

## Summary

In this tutorial, you learned how to use the Audiences API to create and update campaigns, enabling you to target audiences and drive awareness and sales for your products.