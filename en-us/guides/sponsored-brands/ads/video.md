---
title: Video ad
description: Overview for a video ad type
type: guide
interface: api
tags: 
    - Sponsored Brands
keywords:
    - Video ad
    - Brand video ad
---


# Video

This ad format features a horizontal or vertical video that tells your brand story or showcases an individual product or a collection of products. Video ads are eligible for mobile and desktop placements in shopping results.

See the [API reference for video](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsVideoAds) or the [API reference for brand video](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsBrandVideoAds).

##  Video vs. Brand video formats

If your goal is to drive conversion and purchases of an individual product, you can use the [video resource](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsVideoAds) to promote a single product and link that video to the product detail page. 

If your goal is to drive awareness and consideration, you can use the [brand video resource](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsBrandVideoAds) to promote your brand and link that video to your Store.

Under `Drive Page Visits` goal, brand video campaigns allow for one, two, or three advertised products. Under `Grow Brand Impression Share` goal, brand video campaigns allow for none, one, two, or three advertised products. In both advertising goals, you can use a horizontal or vertical video. If you are using a store as a landing page, the ASIN featured in the creative must also appear on your store landing page.

## Before you begin

Both types of campaigns require the following information:

* [A campaign ID](guides/sponsored-brands/campaigns/get-started-with-campaigns#step-1-create-a-campaign)
* [An ad group ID](guides/sponsored-brands/campaigns/get-started-with-campaigns#step-2-create-ad-group)
* Gather creative components
* Upload any needed assets using the [asset library API](guides/creative-asset/asset-library-overview)

## Video creative components

### Video 

Video creatives that take shoppers to a Detail page contain a featured advertised product, a brand logo, a headline, a brand name, and a video.

|Asset	|Notes	|
|---	|---	|
|Featured video	|Upload a video using the creative asset library. See [video requirements and best practices](https://advertising.amazon.com/en-us/resources/ad-specs/sponsored-brands-video). Only horizontal video is allowed.	|
|Featured ASIN	|Make sure your ASIN is [eligible](https://advertising.amazon.com/help?ref_=AAC_unav_support_center#GRA9BRB5SC7DGNE6).	|


### Brand video

Brand video creatives that link to a brand store also contain a brand logo, a headline, a brand name, a video, and can advertise a collection of products.

|Content| Description|
|---	|---	|
|Brand logo image	|Upload your logo using the creative asset library. <br> Image size:  400 x 400 px or larger<br> File size:  1MB or smaller<br>File format : PNG or JPG<br>Content: Logo must fill the image or have a white or transparent background	|
|Headline	|Headline copy should meet the [Amazon advertising standards](https://advertising.amazon.com/help?#G7ZD5SH3UXGQQMJC). Maximum length is 50 characters for all geographies other than Japan (30 characters)	|
|Brand name	|The name of your brand will display in the creative. 	|
| Advertised products |Under Drive Page Visits goal, brand video campaigns allow for one, two, or three advertised products. Under Grow Brand Impression Share goal, brand video campaigns allow for none, one, two, or three advertised products. In both advertising goals, you can use horizontal or vertical video.  |

### Vertical video specifications

|Specifications	|Description	|
|---	|---	|
|File Format	|Any	|
|Aspect ration	|9:16	|
|Dimensions	|720x1280, 1080x1920, or 2160x3840	|
|Duration	|6-45 seconds	|
|Framerate	|23.976, 23.98, 24, 25, 29.97, 29.98, or 30 FPS	|
|Video Bitrate	|1 mbps or more	|
|Video Codec	|H.264 or H.265	|
|Number of Video streams	|1	|
|Audio Sample Rate	|44.1 kHz or higher	|
|Audio Bitrate	|96 kbps or more	|
|Audio Codec	|PCM, AAC, or MP3	|


## Examples

### Product detail landing page request

For product detail landing page video campaigns, you don’t need to specify a landing page. Amazon uses the ASIN specified in the creative and directs customers that click on the ad directly to that product detail page. 

See API reference for [video](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsVideoAds).


```json
curl --location 'https://advertising-api.amazon.com/sb/v4/ads/video' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Accept: application/vnd.sbadresource.v4+json' \
--header 'Content-Type: application/json' \
--data '{
"ads": [{
    "name": "Name of the video ad",
    "state": "ENABLED",
    "adGroupId": "{{adGroupId}}",
    "creative": {
        "asins": [
            "B001000000"
        ],
        "videoAssetIds": [
            "amzn1.assetlibrary.asset1.xxxxxx"
        ]
    }
}]
}
'
```

### Store landing page request

Brand video requires a `STORE` landing page type. Ensure the ASIN used in your creative is also featured on the Store page URL. You can check what ASINs are present on the page using the [GET /pageAsins endpoint](sponsored-brands/3-0/openapi#/Landing%20page%20asins/listAsins).

See API reference for [brand video](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsBrandVideoAds).

```json
curl --location 'https://advertising-api.amazon.com/sb/v4/ads/brandVideo' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Accept: application/vnd.sbadresource.v4+json' \
--header 'Content-Type: application/json' \
--data '{
"ads": [{
    "landingPage": {
        "pageType": "STORE",
        "url": "https://www.amazon.com/stores/page/xxxxxx"
    },
    "name": "Name of the video ad",
    "state": "ENABLED",
    "adGroupId": "{{adGroupId}}",
    "creative": {
        "asins": [
            "B001000000"
        ],
        "videoAssetIds": [
            "amzn1.assetlibrary.asset1.xxxxxx"
        ],
        "brandName": "My brand",
        "brandLogoAssetID": "amzn1.assetlibrary.asset1.xxxxxx",
        "headline": "The headline text of the product in the ad"
    }
}]
}
'
```

### Change to a new Store landing page from existing Brand video ad

The `POST sb/ads/creatives/brandVideo` endpoint changes an existing Brand video ad landing page to a new Store landing page. The `adId` parameter represents the id of the existing landing page. The creative object contains the details of the updated creative for the new landing page.

Endpoint : [/sb/ads/creatives/brandVideo](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateBrandVideoCreative)

```json
curl --location 'https://advertising-api.amazon.com/sb/ads/creatives/brandVideo' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Accept: application/vnd.sbadresource.v4+json' \
--header 'Content-Type: application/json' \
--data '{
    "adId": "A068630018056XN8IKPDR",
    "creative": {
        "brandName": "dacxamsprodusa",
        "headline": "Test",
        "asins": [],
        "brandLogoAssetId": "AXK-hUyrCz8PNIFMVQek:version_v1",
        "brandLogoCrop": {
            "top": 0,
            "left": 0,
            "width": 1469,
            "height": 400
        },
        "videoAssetIds": [
            "amzn1.assetlibrary.asset1.50f4f804724369db5e5c2f4762a0bf5b:version_v1"
        ],
        "landingPage": {
            "type": "STORE",
            "url": "https://www.amazon.com/stores/page/B13A3463-D507-4CCA-8447-7CF5E000BFBE"
        },
        "consentToTranslate": null
    }
}
```





