---
title: Sponsored TV creative overview
description: Sponsored TV getting started with creatives overview. 
type: guide
interface: api
keywords:
    - Sponsored TV
    - Creatives
---

# Sponsored TV creatives overview


A Sponsored TV creative is the video component to your campaign. Through your creative, you can showcase your product and brand that invites customers to interact with your ad.


## Before you begin

* [Upload a video asset using the asset library to get assetId](guides/creative-asset/creating-assets).

## Creative management

To learn more about using the API to create and manage sponsored TV creatives, visit the pages below:

* [Creative moderation](guides/sponsored-tv/creatives/creative-moderation)
* [Creative preview](guides/sponsored-tv/creatives/creative-preview)



## Sponsored TV specification requirements

Below is a table showing the specifications of the video component. For more information on Sponsored TV specifications and guidelines, see [Amazon Ads streaming TV ad policies](https://advertising.amazon.com/resources/ad-policy/streaming-tv-ads?ref_=a20m_us_spcs_sptv_spcs_sttvad).

|Specification	|Details	|
|---	|---	|
|Maximum file size	|500MB	|
|Aspect ratio	|16:9	|
|Required duration	|6 to 45 sec <br /> **Note**: Most video tools show rounded durations for video creative, therefore double-checking the asset(s) in a professional video editing tool with frame accuracy and controls down to milliseconds is recommended.	|
|Minimum frame size	|1920x1080	|
|Minimum video bitrate	|4mbps	|
|Recommended video bitrate	|8mbps	|
|Video frame rate (fps)	|23.976 (recommended), 24, 25, or 29.97 fps	|
|Video frame rate mode	|Constant	|
|Minimum audio bitrate	|192kbps	|
|Audio sample rate	|44.1kHz or 48kHz	|
|Supported Formats	|Video: H.264 or H.265; Audio: PCM or AAC	|
|Audio Channel	|Minimum of 2-Channels	|

## Example

Once you have the `assetID` and `assetVersion` from using the asset library, you can associate the asset to an ad group by creating a creative. 

Request: 

Endpoint: POST [/st/creatives](sponsored-tv-open-beta#tag/Creatives/operation/CreateSponsoredTvCreatives)

```json
curl --location 'https://advertising-api.amazon.com/st/creatives' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data '{
    "creatives": [
        {
            "assetVersion": "version_v1",
            "adGroupId": "123456789123456",
            "assetId": "amzn1.assetlibrary.asset1.a123456789123456"
        }
    ]
}'
```

Response: 

A successful response returns a `creativeId`.


```

{
    "creatives": {
        "error": [],
        "success": [
            {
                "creative": {
                    "adGroupId": "123456789123456",
                    "assetId": "amzn1.assetlibrary.asset1.a123456789123456",
                    "assetVersion": "version_v1",
                    "creativeId": "12345678901234567890",
                    "moderationStatus": "PENDING_REVIEW"
                },
                "creativeId": "12345678901234567890",
                "index": 0
            }
        ]
    }
}
```