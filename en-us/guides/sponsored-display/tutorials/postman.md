---
title: Setting up the Sponsored Display Postman collecction
description: Learn how to use set up the Postman collection for the Amazon Ads API for Sponsored Display.
type: guide
interface: api 
tags:
    - Sponsored Display
keywords:
    - setting up
    - postman
---

# Setting up your development environment 

These tutorial walks you through the steps to build and update Sponsored Display campaigns using [Postman](https://www.postman.com/downloads/). 

* If you haven't already, you must [register for the Sponsored Display API](guides/onboarding/overview) and wait for an email confirmation with instructions to complete the onboarding process. If you have registered, make note of your client identifier and client secret.
* [Download and install Postman](https://www.postman.com/downloads/).
* Download the [product targeting collection](https://d3a0d0y2hgofx6.cloudfront.net/openapi/en-us/sponsored-display/postman/Product_Targeting_postman_collection.json).
* Download the [audiences collection](https://d3a0d0y2hgofx6.cloudfront.net/openapi/en-us/sponsored-display/postman/AUDT_postman.json).
* Download the [recommendations collection](https://d3a0d0y2hgofx6.cloudfront.net/openapi/en-us/sponsored-display/postman/Amazon_SD_API_recommendations.json).
* Download the [environment file](https://d3a0d0y2hgofx6.cloudfront.net/openapi/en-us/sponsored-display/postman/Amazon_SD_API_postman_environment.json).
* Download the [Amazon audiences collection](https://d3a0d0y2hgofx6.cloudfront.net/openapi/en-us/sponsored-display/postman/Amazon%20Audiences.postman_collection.json).
* Download the [Brand safety collection](https://d3a0d0y2hgofx6.cloudfront.net/openapi/en-us/sponsored-display/postman/Brand_Safety.postman_collection.json).
* Download the [Creatives collection](https://d3a0d0y2hgofx6.cloudfront.net/openapi/en-us/sponsored-display/postman/Creatives%20API.postman_collection.json)

## Import the Sponsored Display collections and environment setting files. 

To import the product and audiences collections, follow these steps:

  1. Select **import** in the toolbar.
  2. On the **import** dialog box, select **file**, then click the **upload files** button.
  3. Navigate to the folder where you downloaded the audiences collection in the prerequisites section. Select the **Amazon SD API (AUDT)** postman collection file and click the **open** button.
  4. Verify that the  **format** is **Postman Collection v2.1**, and **import as** is set to **collection**. If these values are correct, click the **import** button. If the values are not correct, check that you selected the correct file in the previous step.
  5. Repeat the steps to upload the product targeting collection file and select **Amazon SD API (Contextual)**.

## Import the environment file. 

To import the environment file, follow these steps:

  1. Select **import** in the toolbar.
  2. On the **import** dialog box, select **file**, then click the **upload files** button.
  3. Navigate to the folder where you downloaded the files in the prerequisites section. Select the **Amazon SD API.postman_environment.json** file and click the **open** button.
  4. Verify that the **format** is **Postman Environment**, and **import as** is set to **Environment**. If these values are correct, click the **import** button. If the values are not correct, check that you selected the correct file in the previous step.

## Configure environment variables

To configure the environment variables, follow these steps:

1. In the right-hand section of Postman, ensure that **collections** is selected. Find the collection named **Amazon SD API (Contextual)**. 
2. Click the arrow icon to expand the folder.
3. Click on the **environment dropdown** beside the collection tabs at the top right of the Postman user interface.
4. Select **Amazon SD API**.
5. Click on the **manage environments** icon.
6. When the **manage environments** dialog opens, select **Amazon SD API**.
7. Enter the **initial value** for the following variables:

  | Variable | Description |
  |----------|-------------|
  | email | The email account associated with your Amazon Advertiser account. |
  | password | The password associated your Amazon Advertiser account. |
  | token_url | `https://api.amazon.com/auth/o2/token` |
  | auth_url | `https://www.amazon.com/ap/oa` |
  | redirect_uri | The **content privacy notice URL** you specified during [account setup](guides/onboarding/overview). |
  | scope | `cpc_advertising:campaign_management` |
  | client_id | The **client ID** assigned to you during [account setup](guides/onboarding/overview). |
  | client_secret | The **client secret** assigned to your during [account setup](guides/onboarding/overview). |
  | baseUrl | `https://advertising-api.amazon.com` |
  | profile_id | Your [profile identifier](reference/2/profiles#tag/Profiles). |

> [WARNING] It is not a good security practice to store your Amazon credentials in the Postman environment. Once you have obtained a refresh token in following steps, remove these values from your environment.

You are now ready to execute Sponsored Display tutorials!