---
title: Managing assets
description: How to manage creative assets through the Amazon Ads API, with step-by-step instructions for searching and updating assets.
type: guide
interface: api
tags: 
    - Sponsored Display
    - Sponsored Brands
    - Stores
    - DSP
keywords:
    - Managing assets
    - Searching assets
    - Updating assets
---


# Managing assets

As you manage campaigns, you may want to create new versions of existing assets. 

## Searching assets

You can use `GET /assets` and `POST /assets/search` to retrieve previously created assets.

### GET /assets

[GET /assets](creative-asset-library#tag/Creative-Assets/operation/getAsset) returns a single asset based on an `assetId` query parameter. 

```
curl --location 'https://advertising-api.amazon.com/assets?assetId=amzn1.assetlibrary.xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx'
```

### POST /assets/search

[POST /assets/search](creative-asset-library#tag/Creative-Assets/operation/searchAssets) lets you search all assets by tag, ASIN, campaign, create date, size, and more. 

To view all assets associated to your profile, leave the request body empty.

```
curl --location 'https://advertising-api.amazon.com/assets/search' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
--header 'Content-Type: text/plain' \
--data '{}'
```

You can also filter by various criteria. See the [API reference](creative-asset-library#tag/Creative-Assets/operation/searchAssets) for all options.

```
curl --location 'https://advertising-api.amazon.com/assets/search' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--data '{
  "filterCriteria": {
    "valueFilters": [
      {
        "values": [
          "IMAGE"
        ],
        "valueField": "ASSET_TYPE"
      }
    ],
    "rangeFilters": [
      {
        "ranges": [
          {
            "start": "1",
            "end": "5000"
          }
        ],
        "rangeField": "SIZE"
      }
    ]
  },
  "sortCriteria": {
    "field": "CREATED_TIME",
    "order": "ASC"
  }
}'
```

## Updating assets

Each update to an asset results in a new version of that asset. The updated asset will have the same assetId with a version appended to the end. 

The process to create a new version is mostly the same as described in Creating assets. Start by [Creating an upload URL](guides/creative-asset/creating-assets#step-1-create-an-upload-url), and [Uploading the asset](guides/creative-asset/creating-assets#step-2-upload-your-asset). For step three, you will specify the assetId of the asset you want to update under `versionInfo`.

>[NOTE] The uploaded file must have a different name than the file in the current version. If the file is the same, then the existing assetId and version are returned. A new version is only generated when a new file is uploaded. 

**Example request**

```
curl --location 'https://advertising-api.amazon.com/assets/register' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Content-Type: text/plain' \
--data '{
  "url": "https://al-na-9d5791cf-3faf.s3.amazonaws.com/xxxxxxxxx",
  "name": "My asset",
  "assetType": "IMAGE",
  "assetSubTypeList": [
    "LOGO"
  ],
  "versionInfo": {
    "linkedAssetId": "amzn1.assetlibrary.asset1.xxxxxxx"
  }
}
```

**Example response**


```
{
 "assetId": "amzn1.assetlibrary.asset1.xxxxxxxx",
 "failedSpecChecks": [],
 "programPolicyValidationsList": **null**,
 "versionId": "version_v2"
}
```

You can then use the combination of `assetId` and `versionId` as part of your creatives, for example `amzn1.assetlibrary.asset1.xxxxxxxx:version_v2`