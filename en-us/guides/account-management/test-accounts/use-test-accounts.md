---
title: Using test accounts in the Amazon Ads API
description: How to use test accounts in the Amazon Ads API, including instructions for retrieving a profile, testing Sponsored Products and Sponsored Brands, and adding creative assets for use in test campaigns.
type: guide
interface: api
tags:
    - Testing
    - Sponsored Products
    - Sponsored Brands
    - Stores
keywords:
    - creative assets
    - moderation
    - video asset
    - image asset
---

# Using test accounts

Once you have successfully [created a test account](guides/account-management/test-accounts/create-test-accounts), you can use the same client ID and an access token with permission for the `advertising::campaign_management` scope to access other endpoints in the API.

>[TOPIC:Product data in test accounts]If you linked a *vendor code* to your vendor test account during account creation, you can use the [product metadata API](product-metadata) to get information about ASINs in your selling account which can then be used for test campaign creation.<br /><br />For author test accounts, test e-book ASINs are created along with the test account. These ASINs can be retrieved by [a `GET` request at `/testAccounts`](guides/account-management/test-accounts/create-test-accounts#checking-the-status-of-an-account-creation-request).

## Profiles

A `GET` request to the [Profiles resource](reference/2/profiles) returns the profiles available for the authorized user account. Each profile in the Amazon Ads API represents a user account's advertising account in a specific marketplace.

For test accounts, the `id` key in `accountInfo` in a returned profile will match the `id` returned from a `GET` request at `/testAccounts`.

```json
[
    {
        "profileId": "xxxxx", 
        "countryCode": "US",
        "currencyCode": "USD",
        "timezone": "America/Los_Angeles",
        "accountInfo": {
            "marketplaceStringId": "ATVPDKIKX0DER",
            "id": "ENTITYxxxxxxxxxxx",
            "type": "vendor",
            "name": "3PTestBrand-xxxxxxx",
            "validPaymentMethod": true
        }
    }
]
```

To start testing the Amazon Ads API, use the `profileId` of a returned profile as the value for the `Amazon-Advertising-API-Scope` header in subsequent requests.

- [Learn more about profiles.](guides/account-management/authorization/profiles)

## Testing Sponsored Products

Using the `profileId` of a test account, you can test any of the APIs under [Sponsored Products](sponsored-products/2-0/openapi). 

>[NOTE]The [budget rules](sponsored-products/budget-rules/getting-started) and [rule-based bidding](guides/sponsored-products/rule-based-bidding/overview) APIs can be tested with test accounts, but the test will not provide the full experience. The test accounts can be used to configure budget rules and bidding rules, but the rules will not take effect as the ads are not served.

## Testing Sponsored Brands

Using the `profileId` of a test account, you can test the APIs under [Sponsored Brands](sponsored-brands/3-0/openapi). 

Please view the [technical specifications for Sponsored Brands campaign creation](sponsored-brands/3-0/openapi/prod#tag/Campaigns) to create any test campaigns just as you would in a real account.

>[NOTE]The [budget rules API](guides/rules/budget-rules/overview) can be tested with test accounts, but the test will not provide the full experience. The test accounts can be used to configure rules, but the rules will not take effect as the ads are not served.

### Moderation of Sponsored Brands campaigns

Amazon Ads test accounts are enabled with auto-moderation for ease of testing Sponsored Brands campaigns and APIs. Any Sponsored Brands campaign created under the test account will be approved and available to use for testing. 

>[NOTE]Campaign approval with a test account does not necessarily mean that the same campaign created in a real advertiser account will be approved. Campaigns in standard advertiser accounts will go through the regular moderation process.

### Testing creative assets for Sponsored Brands campaigns

A creative asset is required for creating Sponsored Brands campaign creatives. You can create new assets using test accounts in either of two ways:

#### To create an asset using the stores API

1. Create a new asset using the [stores/createAsset](sponsored-brands/3-0/openapi#tag/Stores/operation/createAsset) operation. Note the `assetID` returned in the response.
2. Use the `assetID` from the previous step to create a [Sponsored Brands campaign](sponsored-brands/3-0/openapi/prod#tag/Campaigns).

#### To create an asset using the creative assets API *(IMAGE only)*

1. Follow the process described in the [creative assets API documentation](creative-asset-library) to upload and register any new image asset.
2. Once an asset is registered, note the `assetId` returned from the registration request and use it to create a [Sponsored Brands campaign](sponsored-brands/3-0/openapi/prod#tag/Campaigns).

For either method, the campaign and creative will both be approved by auto-moderation. There is no need for separate moderation of the creative assets. The asset will be available under the `brandEntityId` used during the campaign creation.

A list of existing assets can be retrieved with the [stores/listAssets](sponsored-brands/3-0/openapi#tag/Stores/operation/listAssets) operation for use in future Sponsored Brands campaign creation.

### Unsupported Sponsored Brands APIs

The following APIs under Sponsored Brands are not supported for test accounts, and it is not recommended that they be used:

- [Category benchmark](sponsored-brands/3-0/category-benchmark#tag/Category-Benchmark)
- [Premoderation](sponsored-brands/premoderation/openapi#tag/Pre-Moderation) (Only SPONSORED_BRANDS will be supported)

To learn about other unsupported features, see [Features and functionality not supported for test accounts](guides/account-management/test-accounts/overview#features-and-functionality-not-supported).

### Testing Sponsored Brands Video campaigns 

A video asset is required for creating a Sponsored Brands video campaign. You can create a new video asset using test accounts using the following method:

1. Use the [media/upload resource](sponsored-brands/3-0/openapi#tag/Media) to retrieve the URL you'll use to upload the video.

    Currently, Sponsored Brands is the only supported program type and video is the only supported creative type. The request body is:

    ```JSON
    {
    "programType": "SponsoredBrands",
    "creativeType": "Video"
    }
    ```

    The response includes the URL that you'll use to upload the video:

    ```JSON
    {
    "uploadLocation" : "URL for uploading your file"
    }
    ```

2. There are many ways to use the [presigned URL](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-presigned-url.html) to upload a video. In this example, we'll explain how to use to use [Postman](guides/postman).

    1. In Postman, select "File", then "New".
    2. Select "HTTP Request" to create an HTTP request.
    3. In the HTTP operation drop-down, select `PUT`.
    4. In the "Enter request URL" text box, add the presigned URL from the response above.
    5. Select "Body" from the horizontal list of attributes under the "Enter request URL" text box from step 4.
    6. Click the "binary" radio button.
    7. Click the "Select File" button in the pane directly below the horizontal list of attributes from step 5. Navigate to the video file you'd like to upload. Select it, and select "Okay".
    8. Click "Send".

3. Once the upload is complete, you'll use the [media/complete](sponsored-brands/3-0/openapi#tag/Media/operation/completeUpload) resource to retrieve the `mediaId` property.

4. Once you've retrieved it in step 3, pass the `mediaId` in the `videoMediaIds` array of the `creative` property in the request object of the create operation for your [Sponsored Brands video campaign](sponsored-brands/3-0/openapi/prod#tag/Campaigns).

## Testing Sponsored Display

Using the authorization credentials and `profileId` of a test account, you can test requests to the Sponsored Display API.

[Technical specifications for the Sponsored Display API](sponsored-display/3-0/openapi)

### Creating a Sponsored Display campaign

To create a test campaign, use the authorization credentials for your test account and send a [POST request to `/sd/campaigns`](sponsored-display/3-0/openapi#tag/Campaigns/operation/createCampaigns).

**Example request payload**

```json
[
  {
    "name": "Test approve 3P SD Campaign",
    "budgetType": "daily",
    "budget": "100.00",
    "startDate": "20240401",
    "endDate": null,
    "costType": "cpc",
    "state": "enabled",
    "portfolioId": null,
    "tactic": "T00020"
  }
]
```

**Example response**

```json
[
    {
        "campaignId": 535322133423617,
        "code": "SUCCESS"
    }
]
```

### Creating a Sponsored Display ad group

Using the campaign ID retrieved above, you can create an ad group and associate it with your test campaign via a [POST request to `/sd/adGroups`](sponsored-display/3-0/openapi#tag/Ad-Groups/operation/createAdGroups).

**Example request payload**

```json
[
  {
    "name": "Test approve 3P Ad group",
    "campaignId": 535322133423617,
    "defaultBid": 0.1,
    "bidOptimization": "clicks",
    "state": "enabled",
    "creativeType": "IMAGE"
  }
]
```

**Example response**

```json
[
    {
        "code": "SUCCESS",
        "adGroupId": 342576371033945
    }
]
```

### Creating a Sponsored Display product ad

Using the campaign ID and ad group ID, you can create a product ad via a [POST request to `/sd/productAds`](sponsored-display/3-0/openapi#tag/Product-Ads/operation/createProductAds).

**Example request payload**

```json
[
  {
    "state": "enabled",
    "adGroupId": 342576371033945,
    "campaignId": 535322133423617,
    "asin": "B0BLJ3LHCH"
  }
]
```

**Example response**

```json
[
    {
        "code": "SUCCESS",
        "adId": 545635580911524
    }
]
```

### Adding a creative asset

Using the authorization credentials for your test account, use the creative assets API to:

- Generate an asset upload URL.
- Upload a creative asset.
- Register your creative asset and retrieve the asset ID.

For step-by-step instructions, see [Creating assets](guides/creative-asset/creating-assets).

### Creating a Sponsored Display creative

Using the `adGroupId` and `assetId` retrieved in earlier steps, you can create a Sponsored Display creative via a [POST request to `/sd/creatives`](sponsored-display/3-0/openapi#tag/Creatives/operation/createCreatives).

**Example request payload**

```json
[
  {
    "adGroupId": 366235716610812,
    "creativeType": "IMAGE",
    "properties": {
      "rectCustomImage": {
        "assetId": "amzn1.assetlibrary.asset1.527aea846233b0f75f260d7173bf91eb",
        "assetVersion": "version_v1",
        "croppingCoordinates": {
          "top": 0,
          "left": 0,
          "width": 1200,
          "height": 628
        }
      },
      "squareCustomImage": {
        "assetId": "amzn1.assetlibrary.asset1.527aea846233b0f75f260d7173bf91eb",
        "assetVersion": "version_v1",
        "croppingCoordinates": {
          "top": 0,
          "left": 0,
          "width": 628,
          "height": 628
        }
      }
    }
  }
]
```
