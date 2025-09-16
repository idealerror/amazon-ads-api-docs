---
title: Optimize Sponsored Display campaigns with recommendations
description: Learn how to use the Amazon Ads API for Sponsored Display to optimize Sponsored Display campaigns with recommendations.
type: guide
interface: api 
tags:
    - Sponsored Display
keywords:
    - campaign optimization
    - recommendations
    - postman
---

# Optimize Sponsored Display campaigns with recommendations

This tutorial walks you through the steps to build and update Sponsored Display campaigns from recommendations.

## Prerequisites

Follow the setup instructions [here](guides/sponsored-display/tutorials/postman). If you have previously setup the Sponsored Display Postman collections, you can skip these steps.

## Instructions

The recommendations API is protected using OAuth and requires a valid access token. You will need to perform the following steps:

1. Obtain an access token
2. Call the Recommendations service
3. Create or Update Sponsored Display campaigns

### Step 1. Obtain an access token

Execute the following steps to obtain a new access token. If you have already acquired an access token, but it has expired, you can use the `OAuth Refresh Token` request to replace the expired access token. Otherwise, follow these instructions:

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

### Step 2. Call the Recommendations service

Use the Postman collection to call the Recommendations API.

1. In the **collections** tab, select **Amazon SD API Recommendations** and expand the `Recommendations` subfolder.
2. Select the **SD Product Recommendations** operation.
3. Select the `Body` tab and modify the payload with your ASIN within the `products` array:

    ```JSON
    {
        "tactic": "T00020",
        "products": [
            {
                "asin": "B015FK1EH2"
            }
        ],
        "typeFilter": [
            "PRODUCT"
        ]
    }
    ```
**Note**: You can include multiple ASINs within the `products` array using the same object structure.

4. Click **Send**. The response returns a rank-ordered set of recommended product ASINs. 
5. Click on the `Tests` tab and review the code. in line 8, the rank number specifies the ASIN to save into the variable `targetASIN`. You can change this value to use a different rank number.
6. Open the `Get Amazon ASIN Details` request to call the Amazon detail page for the recommended ASIN. In the response, toggle the `Pretty` tab to `Preview` to examine the Web page.

### Step 3. Create or Update Sponsored Display campaigns

To create a Sponsored Display campaign using Postman, follow the steps from this [tutorial](guides/sponsored-display/tutorials/how-to-build-audt-campaigns) with the exception of creating a target clause. Use the instructions below to create a target clause from the recommended ASIN:

1. Open the `Amazon SD API (Contextual)` collection and select the **body** tab and modify the field values to reflect your campaign. Specifically, chanage the `expression` object `value` attribute to `{{targetASIN}}`. The value of `{{targetASIN}}` is automatically copied from the previous request. You would use the same request to update existing campaigns with target clauses using the recommended ASINs. 

    ```JSON
    [
        {
            "adGroupId": "{{adGroupId}}",
            "state": "enabled",
            "expressionType": "manual",
            "expression": [
                {
                    "type": "asinSameAs",
                    "value": "{{targetASIN}}"
                }
            ]
        }
    ]
    ```

2.	Click the **send** button and verify that your request is successful. For example:

    ```JSON
    [
    {
        "targetId": 111222333444555,
        "code": "SUCCESS"
    }
    ]
    ```

3. You can remove existing target clauses that are not performing well using the following:

    ```bash
    DELETE /sd/targets/222333444555666 HTTP/1.1
    ```

This approach allows you to programmatically optimize your target clauses based on product recommendations combined with campaign performance by removing target clauses with poor performance.

## Summary

In this tutorial, you learned how to use the product recommendations API to create and update campaigns, enabling you to optimize the performance of your campaigns based on your campaign goals.

