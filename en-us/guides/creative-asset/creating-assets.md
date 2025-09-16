---
title: Creating assets
description: How to add creative assets through the Amazon Ads API, with step-by-step instructions for uploading and registering assets.
type: guide
interface: api
tags: 
    - Sponsored Display
    - Sponsored Brands
    - Stores
    - DSP
keywords:
    - Creating assets
    - Upload URL
    - Upload asset
    - Register asset
---


# Creating assets

Asset library currently supports video and image creatives. 

Regardless of the type of creative, you need to make three calls:


1. Generate an asset upload URL.
2. Upload your asset to the provided URL.
3. Register your asset. 


## Procedure

### Step 1: Create an upload URL

Use [POST /assets/upload](creative-asset-library#tag/Creative-Assets/operation/getUploadLocation) to get an upload URL. Make sure you are using the correct filename and a supported dimension for the asset you want to upload. Using an unsupported extension will not result in error in this step, but you will encounter errors later. 

**Example: Image**

```
curl --location --request POST 'https://advertising-api.amazon.com/assets/upload' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxx' \
--header 'Content-Type: text/plain' \
--data-raw '{
    "filename": "logo.png"
}
' 
```

The response contains a custom URL you can use to upload your file in step 2.

```
{
 "url": "https://al-na-184b9306-7f8a.s3.amazonaws.com/xxxxxxxxxxxxxx"
 }

```

**Example: Video**

```
curl --location --request POST 'https://advertising-api.amazon.com/assets/upload' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxx' \
--header 'Content-Type: text/plain' \
--data-raw '{
    "filename": "product_video.mov"
}
' 
```

```
{
 "url": "https://al-na-184b9306-7f8a.s3.amazonaws.com/xxxxxxxxxx"
}
```

### Step 2: Upload your asset

To upload your asset, use `PUT` on the URL returned in step 1.

>[WARNING] Make sure you pass the correct `Content-Type` based on the type of asset.

**Example: Image**

```
curl --location -g --request PUT 'https://al-na-184b9306-7f8a.s3.amazonaws.com/xxxxxxxxxxxxxx' \
--header 'Content-Type: image/png' \
--data-binary '@/Users/xxxxxx/desktop/logo.png'
```

**Example: Video**

```
curl --location --request PUT 'https://al-na-9d5791cf-3faf.s3.amazonaws.com/xxxxxxxx' \
--header 'Content-Type: video/quicktime' \
--data-binary '@/Users/xxxxxxxxxx/Downloads/brand_video.mov'
```

If the asset uploads correctly, you receive a 200 response. 

### Step 3:  Register the asset

Register the asset using [POST /assets/register](creative-asset-library#tag/Creative-Assets/operation/registerAsset).  

#### Parameters

|Parameter	|Required?	|Description	|
|---	|---	|---	|
|url	|Yes	|The upload URL for the asset generated in step 1.	|
|name	|Yes	|The name of the asset.	|
|asinList	|No	|An optional list of ASINs associated with the asset to be used for searching and categorization.	|
|assetType	|Yes	|The type of asset. Currently accepts IMAGE and VIDEO.	|
|assetSubTypeList	|Yes	|The sub type of the asset.	|
|versionInfo	|No	|If you are uploading a new version of an existing asset, include the assetId in versionInfo.	|
|tags	|No	|An optional parameter to add relevant tags to the asset for searching and categorization.	|
|registrationContext	|Yes, for DSP assets	|Only required for assets that you will in DSP campaigns. If you are using the asset for sponsored ads or stores, you don't need to include this parameter. 	|
|associatedSubEntityList	|Yes, for Amazon sellers	|Amazon sellers must include their brand entity ID. You can find this using the GET /brands endpoint. For other advertisers, we recommend including brand entity ID, but it is not required.	|
|skipAssetSubTypesDetection	|No	|By default, the system attempts to automatically classify your image into sub types. If you don't want the system to do this classifcation, set this to `TRUE`.	|


**Example: Image**

```
curl --location --request POST 'https://advertising-api.amazon.com/assets/register' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Content-Type: text/plain' \
--data-raw '{
  "url": "https://al-na-184b9306-7f8a.s3.amazonaws.com/xxxxxxxxx",
  "name": "logo",
  "asinList": [
    "B07XXXXXX1",
    "B07XXXXXX2",
    "B07XXXXXX3"
  ],
  "assetType": "IMAGE",
  "assetSubTypeList": [
    "LOGO"
  ],
  "associatedSubEntityList": [
    {
      "brandEntityId": "ENTITYXXXXXXXXXX"
    }
  ]
}

'
```

The response returns 200 with an `assetId` that you can use when creating or editing campaigns. 

```
{
 "assetId": "amzn1.assetlibrary.asset1.xxxxx",
 "failedSpecChecks": [],
 "programPolicyValidationsList": null,
 "versionId": "version_v1"
}
```

>[NOTE] Asset library does not perform checks on eligibility for images. To optimize the moderation process, make sure you follow the requirements set by the campaign and image type.  

**Example: Video**

```
curl --location --request POST 'https://advertising-api.amazon.com/assets/register' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--data-raw '{
  "url": "https://al-na-9d5791cf-3faf.s3.amazonaws.com/xxxxxxxxx",
  "name": "SB video asset",
  "asinList": [
    "B0XXXXXX1",
    "B0XXXXXX2",
    "B0XXXXXX3"
  ],
  "assetType": "VIDEO",
  "assetSubTypeList": [
    "BACKGROUND_VIDEO"
  ],
  "associatedSubEntityList": [
    {
      "brandEntityId": "ENTITYXXXXXXXXXX"
    }
  ]
}
'
```

A successful response returns 200 with an `assetId` that can be used when managing your campaign. The response returns a list of the program types for which the video asset has failed validation checks. 

In this example, the registered video asset is not eligible for DSP OLV, DSP OTT, or Stores intro splash. This video is for a Sponsored Brands campaign, and you will see Sponsored Brands is not mentioned in the failedSpecChecks. 

>[NOTE] Creative asset library processing is not part of the moderation process. A creative may not be eligible to be shown due to violations of creative policy for the ad program. If you need to upload a new version of a creative, see [Managing assets](guides/creative-asset/managing-assets).

```
{
    "assetId": "amzn1.assetlibrary.asset1.xxxxxxxxxxx",
    "failedSpecChecks": [
        {
            "Program": "AMAZON_DSP",
            "specProgramName": "DEMAND_SIDE_PLATFORM_OLV",
            "specifications": [
                {
                    "actualValue": "30.0",
                    "arguments": [
                        "23.98",
                        "24.0",
                        "25.0",
                        "29.97"
                    ],
                    "failureReason": "Frame rate is not one of 23.98, 24.0, 25.0, 29.97.",
                    "isPassed": false,
                    "stringId": "al-spec-video-frame-rate"
                },
                {
                    "actualValue": "0",
                    "arguments": [
                        "2"
                    ],
                    "failureReason": "Total number of audio channels across all audio streams is less than 2.",
                    "isPassed": false,
                    "stringId": "al-spec-min-audio-channel-count"
                }
            ]
        },
        {
            "Program": "AMAZON_DSP",
            "specProgramName": "DEMAND_SIDE_PLATFORM_OTT",
            "specifications": [
                {
                    "actualValue": "30.0",
                    "arguments": [
                        "23.98",
                        "24.0",
                        "25.0",
                        "29.97"
                    ],
                    "failureReason": "Frame rate is not one of 23.98, 24.0, 25.0, 29.97.",
                    "isPassed": false,
                    "stringId": "al-spec-video-frame-rate"
                },
                {
                    "actualValue": "9.966667",
                    "arguments": [
                        "15.0",
                        "30.0"
                    ],
                    "failureReason": "Duration is not one of 15.0, 30.0 seconds.",
                    "isPassed": false,
                    "stringId": "al-spec-video-duration"
                },
                {
                    "actualValue": "0",
                    "arguments": [
                        "2"
                    ],
                    "failureReason": "Total number of audio channels across all audio streams is less than 2.",
                    "isPassed": false,
                    "stringId": "al-spec-min-audio-channel-count"
                }
            ]
        },
        {
            "Program": "STORES",
            "specProgramName": "STORES_INTRO_SPLASH",
            "specifications": [
                {
                    "actualValue": "16:9",
                    "arguments": [
                        "9:16"
                    ],
                    "failureReason": "Aspect ratio is not 9:16.",
                    "isPassed": false,
                    "stringId": "al-spec-video-aspect-ratio"
                }
            ]
        }
    ],
    "programPolicyValidationsList": null,
    "versionId": "version_v1"
}
```

## Next steps

After registering your asset, it is set to a `PROCESSING` status. Once the asset is in `ACTIVE` status, it can be used in a campaign.

>[TIP] Depending on the size of the file you are uploading, the asset can be in `PROCESSING` status up to 30 minutes. We suggest polling the `GET /assets` endpoint to check that the status is set to `ACTIVE` before trying to use the asset in a campaign. Video assets are generally in `PROCESSING` status for longer as the processing period includes the video transcoding process. If the transcoding process fails, the asset status moves to `INACTIVE`.

If your asset failed validation, you can make corrections and upload a new version of the asset. See [Managing assets](guides/creative-asset/managing-assets) for more information.