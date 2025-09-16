---
title: Product collection ad
description: Overview for a product collection ad type
type: guide
interface: api
tags: 
    - Sponsored Brands
keywords: 
    - Product collection
---


# Product collection

With the product collection ad type, you can promote your products from a landing page of your choice. This ad features your brand logo, a custom headline, and three or more products. When Amazon shoppers click a product's image, it takes them to the product detail page. Clicking the brand logo or the headline, directs shoppers to your store or a custom landing page that you designate.

See the [API reference](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsProductCollectionAds) for more details.

## Before you begin

You must have the following before creating a product collection ad:

* An `assetId` for any creatives being used in the ad:
    * custom image (optional)
    * brand logo (required) requires minimum dimensions of 400 x 400 px
    * These IDs are created using the [asset library API](guides/creative-asset/asset-library-overview).
* [An ad group ID](guides/sponsored-brands/campaigns/get-started-with-campaigns#step-2-create-ad-group)
* [A campaign ID](guides/sponsored-brands/campaigns/get-started-with-campaigns#step-1-create-a-campaign)
* Three eligible ASINs to show in the ad
* A headline for the ad
* The brand name you want to appear on the ad



## Creative specifications

A creative must meet certain requirements before it can be accepted. Below are specifications for both a brand logo and a custom image.


### Brand Logo specifications
	 
|Specification	|Details|
|---	|---	|
|Image size	|400 x 400 px or larger	|
|File size	|1MB or smaller	|
|File format	|PNG or JPG	|
|Content	|Logo must fill the image or have a white or transparent background. For more information, visit [Brand Logo Creative Acceptance Policy](https://advertising.amazon.com/help?ref_=AAC_unav_support_center#GYUVDSUJJQV4NGQB).|	 

### Custom image specifications

|Specification	|Details	|
|---	|---	|
|Image size	|1200 x 628 px or larger	|
|File size	|5MB or smaller	|
|File format	|PNG or JPG	|
|Content	|No text, graphics, or logos added to the image. For more information, visit [Custom Image Creative Acceptance Policy](https://advertising.amazon.com/help?ref_=AAC_unav_support_center#G76M96SHBLDBRSQT).	|

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
|Content	|No text, graphics, or logos added to the image. For more information, visit [Custom Image Creative Acceptance Policy](https://advertising.amazon.com/help?ref_=AAC_unav_support_center#G76M96SHBLDBRSQT).	|



## Examples

### Simple landing page

A simple landing page supports up to one hundred ASINS, including the three ASINS advertised in the creative. If you want a simple landing page, set `pageType`  as `PRODUCT_LIST` and include a list of ASINS on the landing page object as show in the example below: 


**Example request body: PRODUCT_LIST**

```json
curl --location 'https://advertising-api.amazon.com/sb/v4/ads/productCollectionExtended' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Accept: application/vnd.sbadresource.v4+json' \
--data '{
"ads": [{
    "landingPage": {
        "asins": ["B0CGY22J6Y", "B0CGY4QJTC", "B0CGY1MQFD", "B0BVYTJFLY", "B0B6963LJN"],
        "pageType": "PRODUCT_LIST"
    },
    "name": "Name of product collection ad",
    "state": "ENABLED",
    "adGroupId": "{{adGroupId}}",
    "creative": {
        "brandLogoCrop": {
            "height": 1181,
            "left": 326,
            "top": 125,
            "width": 734
        },
        "asins": ["B0CGY1MQFD", "B0BVYTJFLY", "B0B6963LJN"],
        "brandName": "CuentazillaVendorBrandChange",
        "customImages": [{
            "assetId": "amzn1.assetlibrary.asset1.xxxxxx:version_v1",
            "crop": {
                "height": 2592,
                "left": 0,
                "top": 354,
                "width": 4950
            },
            "url": "https://m.media-amazon.com/images/S/example.png"
        }],
        "consentToTranslate": true,
        "brandLogoAssetID": "amzn1.assetlibrary.asset1.xxxxxx:version_v1",
        "headline": "Find the Perfect LEGO® Gift for Boys of All Ages"
    }
}]
}
'
```

### Store

If you set `pageType` to `STORE`, this will reference to an existing Amazon store page. The store url must be included in the landing page object as shown in the example below. Learn more about [Stores](https://advertising.amazon.com/help?ref_=AAC_unav_support_center#GEQXGAH9CZWU2K76). 

**Example request body: STORE**


```json
curl --location 'https://advertising-api.amazon.com/sb/v4/ads/productCollectionExtended' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Accept: application/vnd.sbadresource.v4+json' \
--data '{
    "ads": [{
        "adId": "{{adGroupId}}",
        "creative": {
            "asins": [
                "B0C81DZTX4",
                "B0C81CBH6K",
                "B0BTHXJ718"
            ],
            "brandLogoAssetId": "amzn1.assetlibrary.asset1.xxxxxx:version_v1",
            "brandLogoCrop": {
                "height": 768,
                "left": 0,
                "top": 0,
                "width": 768
            },
            "brandLogoUrl": "https://m.media-amazon.com/images/S/example.png",
            "brandName": "the brand name",
            "customImages": [{
                "assetId": "amzn1.assetlibrary.asset1.xxxxxx:version_v1",
                "crop": {
                    "height": 628,
                    "left": 0,
                    "top": 0,
                    "width": 1200
                },
                "url": "https://m.media-amazon.com/images/S/example.png"
            }],
            "headline": "Test",
            "landingPage": {
                "type": "PRODUCT_LIST",
                "asins": [
                    "B0C81DZTX4",
                    "B0C81CBH6K",
                    "B0BTHXJ718"
                ]
            }
        }
    }]
}'
```



### Custom URL

If you are a vendor and you have a custom landing page, you can set `pageType` to `CUSTOM_URL`. The landing page must include the ASINs of at least three products that are advertised as part of the campaign.

>[NOTE] Only vendors can use the `CUSTOM_URL` enum. This `pageType` is not available for sellers.  

**Example request body: CUSTOM_URL**


```json
curl --location 'https://advertising-api.amazon.com/sb/v4/ads/productCollectionExtended' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Accept: application/vnd.sbadresource.v4+json' \
--header 'Content-Type: application/json' \
--data '{
"ads": [{
    "landingPage": {
        "pageType": "CUSTOM_URL",
        "url": "https://www.amazon.com/stores/page/xxxxxx"
    },
    "name": "Name of product collection ad",
    "state": "ENABLED",
    "adGroupId": "{{adGroupId}}",
    "creative": {
        "brandLogoCrop": {
            "top": 0,
            "left": 0,
            "width": 400,
            "height": 400
        },
        "asins": [
            "B001000000",
            "B020000000",
            "B000000000"
        ],
        "brandName": "Name of the brand",
        "customImageAssetId": "amzn1.assetlibrary.asset1.xxxxxx",
        "brandLogoAssetID": "amzn1.assetlibrary.asset1.xxxxxx",
        "headline": "The headline text of the products in the ad"
    }
}]
}'
```

### Custom image

To add a custom image, you must first get an `assetId` using the [asset library API](guides/creative-asset/asset-library-overview). Within the creative object, you can specify the dimensions of the custom image. If you do not specify dimensions, the custom image will default to an image size of 1200 x 628 px.

>[NOTE] A custom image is optional and not required for the creation of a product collection ad. 

**Request example with custom image crop dimensions:**


```json
curl --location 'https://advertising-api.amazon.com/sb/v4/ads/productCollectionExtended' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Accept: application/vnd.sbadresource.v4+json' \
--header 'Content-Type: application/json' \
--data '{
    "ads": [{
        "landingPage": {
            "pageType": "STORE",
            "url": "https://www.amazon.com/stores/page/C4DC3142-D46C-4AAA-BAFD-43263D3D8654/"
        },
        "name": "Name of custom image",
        "state": "ENABLED",
        "adGroupId": "{{adGroupId}}",
        "creative": {
            "brandLogoCrop": {
                "height": 1181,
                "left": 326,
                "top": 125,
                "width": 734
            },
            "asins": [
                "B0892GH19Q",
                "B08ZT3H56V",
                "B0BT42N18P"
            ],
            "brandName": "name of brand",
            "customImages": [{
                    "assetId": "amzn1.assetlibrary.asset1.xxxxxx:version_v1",
                    "crop": {
                        "height": 1500,
                        "left": 0,
                        "top": 32,
                        "width": 2864
                    },
                    "url": "https://m.media-amazon.com/images/S/example.png"
                },
                {
                    "assetId": "amzn1.assetlibrary.asset1.xxxxxx:version_v1",
                    "url": "https://m.media-amazon.com/images/S/example.png"
                }
            ],
            "brandLogoAssetID": "amzn1.assetlibrary.asset1.xxxxxx:version_v1",
            "headline": "Find the Perfect LEGO® Gift for Boys of All Ages"
        }
    }]
}'
```

### Change to a new landing page from existing simple landing page

The `POST sb/ads/creatives/productCollectionExtended` endpoint changes an existing product collection ad landing page to a new landing page. The `adId` parameter represents the id of the existing product collection ad. The creative object contains the details of the updated creative for the new landing page.

Endpoint : [sb/ads/creatives/productCollectionExtended](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateExtendedProductCollectionCreative)

```json
curl --location 'https://advertising-api.amazon.com/sb/ads/creatives/productCollectionExtended' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Accept: application/vnd.sbadresource.v4+json' \
--header 'Content-Type: application/json' \
--data '{
    "adId": "{{adId}}",
    "creative": {
        "asins": [
            "B0C81DZTX4",
            "B0C81CBH6K",
            "B0BTHXJ718"
        ],
        "brandLogoAssetId": "amzn1.assetlibrary.asset1.xxxxxx:version_v1",
        "brandLogoCrop": {
            "height": 768,
            "left": 0,
            "top": 0,
            "width": 768
        },
        "brandLogoUrl": "https://m.media-amazon.com/images/S/example.png",
        "brandName": "name of brand",
        "customImages": [{
            "assetId": "amzn1.assetlibrary.asset1.xxxxxx:version_v1",
            "crop": {
                "height": 628,
                "left": 0,
                "top": 0,
                "width": 1200
            },
            "url": "https://m.media-amazon.com/images/S/example.png"
        }],
        "headline": "name of headline",
        "landingPage": {
            "type": "PRODUCT_LIST",
            "asins": [
                "B0C81DZTX4",
                "B0C81CBH6K",
                "B0BTHXJ718"
            ]
        }
    }
}'

```

### Change to a Store landing page from existing simple landing page

```json
curl --location 'https://advertising-api.amazon.com/sb/ads/creatives/productCollectionExtended' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Accept: application/vnd.sbadresource.v4+json' \
--header 'Content-Type: application/json' \
--data '{
    "adId": "{{adId}}",
    "creative": {
        "brandName": "name of brand",
        "headline": "headline of creative",
        "asins": [
            "B0747XRQDJ",
            "B09T98XHKB",
            "B0747XHBYS"
        ],
        "brandLogoAssetId": "amzn1.assetlibrary.asset1.xxxxxx:version_v1",
        "brandLogoCrop": {
            "top": 0,
            "left": 0,
            "width": 1469,
            "height": 400
        },
        "landingPage": {
            "type": "STORE",
            "url": "https://www.amazon.com/stores/page/xxxxxxx"
        },
        "customImages": [{
            "assetId": "amzn1.assetlibrary.asset1.xxxxxx:version_v1"
        }],
        "consentToTranslate": null
    }
}
```

### Change to a custom URL landing page from existing simple landing page

```json
curl --location 'https://advertising-api.amazon.com/sb/ads/creatives/productCollectionExtended' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Accept: application/vnd.sbadresource.v4+json' \
--header 'Content-Type: application/json' \
--data '{
    "adId": "{{adId}}",
    "creative": {
        "brandName": "name of brand",
        "headline": "headline text",
        "asins": [
            "B0747XRQDJ",
            "B09T98XHKB",
            "B0747XHBYS"
        ],
        "brandLogoAssetId": "amzn1.assetlibrary.asset1.xxxxxx:version_v1",
        "brandLogoCrop": {
            "top": 0,
            "left": 0,
            "width": 1469,
            "height": 400
        },
        "landingPage": {
            "type": "CUSTOM_URL",
            "url": "https://www.amazon.com/stores/page/B13A3463-D507-4CCA-8447-7CF5E000BFBE"
        },
        "customImages": [{
            "assetId": "amzn1.assetlibrary.asset1.xxxxxx:version_v1"
        }],
        "consentToTranslate": null
    }
}
```

## Frequently Asked Questions

### Why should we add a custom image? What are the benefits of using a custom image?

Historical data shows that, on average, Sponsored Brands ads with a branded creative generates a 50% increase in CTR and a 60% increase in branded search compared to ads with only product images.


### What are branded creatives? Is product collection with a custom image or Lifestyle considered a branded creative?

Yes, product collection with a custom image (also known as a Lifestyle image) is considered a branded creative. product collection without a custom image is not considered a branded creative.


### What is the visual difference between a product collection ASIN-only creative and a product collection custom image creative?

The main difference between a product collection ASIN-only creative and a product collection custom image is the presence of the custom image. A product collection custom image ad may contain zero or one products, but an ASIN-only ad with zero or one product does not display on [Amazon](https://www.amazon.com/).


### How can I identify campaigns with no custom images?

You can use the existing POST API [/sb/v4/ads/list](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/ListSponsoredBrandsAds) to identify if your creative is missing a custom image or not. This API returns the latest creative status, including if the moderation is pending or rejected.


### What should I do when my creative is under moderation review?

If your creative is under moderation review, please wait until the moderation is completed. In case your creative is rejected due to ad policy violation, you can edit your creative using the POST API [/sb/ads/creatives/productCollection](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateProductCollectionCreative).


### We are not integrated with creative edit features. How can we edit product collection ads using ad console?

Please follow the instructions in this [YouTube tutorial](https://www.youtube.com/watch?v=QvHIhQ419Vw).


### What if I don’t have a suitable custom image?

For existing campaigns, we recommend that you edit your campaign to include a custom image from your computer, asset library, or, if available to you, create a GenAI image using the beta feature found at [image generation](https://advertising.amazon.com/en-us/resources/whats-new/image-generation-for-sponsored-brands?ref_=pe_37211531_889230061). If your existing campaign links to your Store landing page, then Amazon Ads can generate an image from your Store in your ASIN-only product collection ads if applicable. Additionally, you can build a Sponsored Brands video ad if you have video assets or a Store spotlight ad if you have a Store.

Please reach out to us in the [Amazon Ads API Service Desk](https://advertising.amazon.com/API/docs/en-us/support/overview) for more information on how to use Gen AI through APIs.


### Do I have to upload one custom image per ASIN in the product collection?

No, you need one custom image per product collection ad. product collection ads allow you to promote your products from a landing page of your choice. These ads feature your brand logo, a custom headline, and three or more products. When Amazon customers click a product’s image, it takes them to the product detail page. Clicking the brand logo or the headline will direct customers to your Store or a custom landing page. For product collection, you will need a minimum of three products.