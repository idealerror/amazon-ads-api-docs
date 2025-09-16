---
title: Amazon Ads API ads model overview
description: Describes the ads model and associated fields for the Amazon Ads API.
type: guide
interface: api
tags:
    - Operations
---

# Ads

An **ad** is the smallest unit of an advertising campaign. An ad represents the creative and advertised products that a customer sees and interacts with. In the Amazon Ads API, the common ad model represents ads, product ads, and ad creatives found across sponsored ads campaigns. 

## Ad types

Amazon Ads supports a variety of ad types depending on the ad product. 

| Ad type | Description | Sponsored Products | Sponsored Brands | Sponsored Display |
|------|-----|------|------|--|----|----|
| PRODUCT_AD | Product ads are build based on the advertised product and an optional headline. | x |  | x |
| IMAGE |  Features one or more custom images. |  |  | x |
| VIDEO |  Features one or more videos. For Sponsored Brands, includes both video and brand video ad types. |  | x | x |
| PRODUCT_COLLECTION  | Features a collection of products along with a customer image. |  | x |  |
| STORE_SPOTLIGHT | Features an Amazon Store an chosen sub-pages. | | x |  |

## Schema

Ads contain the following fields. **Read-only** indicates that the field is part of the model, but cannot be modified by advertisers. **Required** indicates that a field will always appear as part of model. 

>[NOTE] Some ad fields are only available for certain ad products. For details, see the [Ad product mapping table](#ad-product-mapping).

|Common field	|Type	|Required	|Read-only	|Description	|
|---	|---	|---	|---	|---	|
|adId	|string	|Required	|Read Only	|The unique identifier of the ad.	|
|adGroupId	|string	|Required	| 	|The unique identifier of the ad group	|
|adProduct	|[Enum](reference/common-models/enums#adproduct)	|Required	|	|The ad product associated to the ad. 	|
|state	|[Enum](reference/common-models/enums#state)	|Required	| 	|The user-set state of the ad.	|
|name	|string	|	| 	|The user-set name of the ad.	|
|creationDateTime	|datetime	|Required	|Read Only	|The date time that the ad was created.	|
|lastUpdatedDateTime	|datetime	|Required	|Read Only	|The date time that the target was last updated.	|
|deliveryStatus	|[Enum](reference/common-models/enums#deliverystatus)	|Required	|Read Only	|The overall status indicating whether the ad is delivering or not.	|
|deliveryReasons	|[Enum](reference/common-models/enums#deliveryreasons)	| 	|Read Only	|A list of reasons why the ad is not delivering.	|
|adVersionId	|string	| 	|Read Only	|The version associated with the ad.	|
|adType	|[Enum](reference/common-models/enums#adtype)	|Required	|	|The type of the ad. 	|
|creative	|obj	|Required	| 	|An object containing creative and landing page details.	|
|creative.<br />products	|array	| 	| 	|The products or subject to be featured as part of the ad.	|
|creative.<br />products.productIdType	|[Enum](reference/common-models/enums#productidtype)	| 	| 	|The type of product ID.	|
|creative.<br />products.productId	|string	| 	| 	|The ID associated with the product.	|
|creative.<br />brandLogo	|obj	| 	| 	|The brand logo asset shown on the ad.	|
|creative.<br />brandLogo.<br />assetId	|string	| 	| 	|The asset library ID associated with the brand logo image asset.	|
|creative.<br />brandLogo.<br />assetVersion	|string	| 	| 	|The version of the brand logo asset.	|
|creative.<br />brandLogo.<br />formatProperties	|obj	| 	| 	|Allows you to crop the brand logo asset.	|
|creative.<br />brandLogo.<br />formatProperties.<br />top	|integer	| 	| 	|Desired coordinates for the top of the cropped image.	|
|creative.<br />brandLogo.<br />formatProperties.<br />left	|integer	| 	| 	|Desired coordinates for the left of the cropped image.	|
|creative.<br />brandLogo.<br />formatProperties.<br />width	|integer	| 	| 	|The desired width of the cropped image.	|
|creative.<br />brandLogo.<br />formatProperties.<br />height	|integer	| 	| 	|The desired height of the cropped image.	|
|creative.<br />customImages	|array	| 	| 	|Allows you to specify custom images to be used in the creative.	|
|creative.<br />customImages.<br />assetId	|string	| 	| 	|The asset library ID associated with the custom image asset.	|
|creative.<br />customImages.<br />assetVersion	|string	| 	| 	|The version of the custom image asset.	|
|creative.<br />customImages.<br />formatProperties	|obj	| 	| 	|Allows you to crop the brand logo asset.	|
|creative.<br />customImages.<br />formatProperties.<br />top	|integer	| 	| 	|Desired coordinates for the top of the cropped image.	|
|creative.<br />customImages.<br />formatProperties.<br />left	|integer	| 	| 	|Desired coordinates for the left of the cropped image.	|
|creative.<br />customImages.<br />formatProperties.<br />width	|integer	| 	| 	|The desired width of the cropped image.	|
|creative.<br />customImages.<br />formatProperties.<br />height	|integer	| 	| 	|The desired height of the cropped image.	|
|creative.<br />videos	|array	| 	| 	|Videos associated with the creative.	|
|creative.<br />videos.<br />assetId	|string	| 	| 	|The asset library ID associated with the video asset.	|
|creative.<br />videos.<br />assetVersion	|string	| 	| 	|The version of the video asset.	|
|creative.<br />headline	|string	| 	| 	|The headline or custom text associated with the ad.	|
|creative.<br />brandName	|string	| 	| 	|The brand name displayed in the ad.	|
|creative.<br />landingPage	|obj	| 	| 	|The details of the landing page for the ad.	|
|creative.<br />landingPage.<br />landingPageType	|[Enum](reference/common-models/enums#landingpagetype)	| 	| 	|The type of landing page associated with the ad.	|
|creative.<br />landingPage.<br />landingPageUrl	|string	| 	| 	|The URL of the landing page.	|
|creative.<br />cards	|array	| 	| 	|Defines sections of the ad that have a different group of creative elements (assets, headlines, CTAs) and landing page are specific to that portion of the ad	|
|creative.<br />cards.<br />headline	|string	| 	| 	|The headline associated to the card.	|
|creative.<br />cards.<br />landingPage	|obj	| 	| 	|The landing page associated to the card.	|
|creative.<br />cards.<br />landingPage.<br />landingPageType	|[Enum]([Enum](reference/common-models/enums#landingpagetype)	| 	| 	|The type of landing page associated to the card.	|
|creative.<br />cards.<br />landingPage.<br />landingPageUrl	|string	| 	| 	|The URL of the landing page associated to the card.	|
|creative.<br />cards.<br />product	|obj	| 	| 	|The product associated with the card.	|
|creative.<br />cards.<br />products.<br />productIdType	|[Enum](reference/common-models/enums#productidtype)	| 	| 	|The type of identifier for the product associated with the card.	|
|creative.<br />cards.<br />products.<br />productId	|string	| 	| 	|The identifier of the product associated with the card.	|


## Ad product mapping

The mapping table shows how current versions of different ad products map to the common ad group model. Over time, we will move to standardize the fields in each individual API to the common ad group model.

| Ad product | Latest version |
|-----|-------|
| Sponsored Products | [Version 3](sponsored-products/3-0/openapi/prod#tag/Product-ads) |
| Sponsored Brands | [Version 4](sponsored-brands/3-0/openapi/prod#tag/Ads) |
| Sponsored Display | [Version 1](sponsored-display/3-0/openapi#tag/product-ads) |
| Sponsored TV | [Version 1](sponsored-tv-open-beta#tag/Ads)| 

For Amazon Marketing Stream, the table below references the [ads](guides/amazon-marketing-stream/datasets/sponsored-ads-campaign-management#ads-dataset-beta) dataset.

**Legend**

**x**: The ad product uses the common field name in its current version.

**N/A**:  The ad product contains the field in the common model schema, but not in its current version schema. 

**Empty cell**: The field is not represented for the ad product.

|Common field	|Sponsored Products	|Sponsored Brands	|Sponsored Display	|Amazon Marketing Stream	|
|---	|---	|---	|---	|---	|
|adId	|x	|x	|x	|x	|
|adGroupId	|x	|x	|x	|x	|
|adProduct	|SPONSORED_PRODUCTS	|SPONSORED_BRANDS	|SPONSORED_DISPLAY	|x	|
|state	|x	|x	|x	|x	|
|name	| 	|x	|adName	|x	|
|creationDateTime	|x	|creationDate	|creationDate	|audit.creationDateTime	|
|lastUpdatedDateTime	|x	|lastUpdatedDate	|lastUpdatedDate	|audit.lastUpdatedDateTime	|
|deliveryStatus	|servingStatus mapped to DELIVERING or NOT_DELIVERING	|servingStatus mapped to DELIVERING or NOT_DELIVERING	|servingStatus mapped to DELIVERING or NOT_DELIVERING	|	|
|deliveryReasons	|servingStatus	|servingStatus	|servingStatus	|	|
|adVersionId	| 	|creativeVersion (property of SB creative)	| 	|	|
|adType	|PRODUCT_AD	|creative.type	|creativeType or PRODUCT_AD	|	|
|creative.<br />products.<br />productIdType	|ASIN or SKU	|ASIN	|ASIN or SKU	|	|
|creative.<br />products.<br />productId	|asin / sku	|creative.<br />asins	|asin / sku	|asin / sku	|
|creative.<br />brandLogo	| 	| 	|properties.<br />brandLogo (property of SD creative)	|	|
|creative.<br />brandLogo.<br />assetId	| 	|creative.brandLogoAssetId	| properties.brandLogo.assetId (property of SD creative)	|	|
|creative.<br />brandLogo.<br />assetVersion	| 	|creative.<br />brandLogoAssetId (appended)	|properties.<br />brandLogo.<br />assetVersion (property of SD creative)	|	|
|creative.<br />brandLogo.<br />formatProperties	| 	|brandLogoCrop	|properties.<br />brandLogo.<br />croppingCoordinates (property of SD creative)	|	|
|creative.<br />brandLogo.<br />formatProperties.<br />top	| 	|brandLogoCrop.<br />top	|properties.<br />brandLogo.<br />croppingCoordinates.<br />top (property of SD creative)	|	|
|creative.<br />brandLogo.<br />formatProperties.left	| 	|brandLogoCrop.<br />left	|properties.<br />brandLogo.<br />croppingCoordinates.<br />left (property of SD creative)	|	|
|creative.<br />brandLogo.<br />formatProperties.<br />width	| 	|brandLogoCrop.<br />width	|properties.<br />brandLogo.<br />croppingCoordinates.<br />width (property of SD creative)	|	|
|creative.<br />brandLogo.<br />formatProperties.<br />height	| 	|brandLogoCrop.<br />height	|properties.<br />brandLogo.<br />croppingCoordinates.<br />height (property of SD creative)	|	|
|creative.<br />customImages.<br />assetId	| 	| creative.customImageAssetId | creativeElements.squareImages.assetId creativeElements.horizontalImages.assetId creativeElements.verticalImages.assetId (property of SD creative)	|	|
|creative.<br />customImages.<br />assetVersion	| | creative.customImageAssetId (version appended)	|creativeElements.squareImages.assetVersion creativeElements.horizontalImages.assetVersion creativeElements.verticalImages.assetVersion (property of SD creative)	|	|
|creative.<br />customImages.<br />formatProperties	| 	|customImageCrop	|creativeElements.squareImages.croppingCoordinates creativeElements.horizontalImages.croppingCoordinates  creativeElements.verticalImages.croppingCoordinates (property of SD creative)	|	|
|creative.<br />customImages.<br />formatProperties.<br />top	| 	|customImageCrop.<br />top	|creativeElements.squareImages.croppingCoordinates.top creativeElements.horizontalImages.croppingCoordinates.top  creativeElements.verticalImages.croppingCoordinates.top (property of SD creative)	|	|
|creative.<br />customImages.<br />formatProperties.<br />left	| 	|customImageCrop.<br />left	|creativeElements.squareImages.croppingCoordinates.left creativeElements.horizontalImages.croppingCoordinates.left creativeElements.verticalImages.croppingCoordinates.left (property of SD creative)	|	|
creative.<br />customImages.<br />formatProperties.<br />width	| 	| customImageCrop.<br />width |creativeElements.squareImages.croppingCoordinates.width creativeElements.horizontalImages.croppingCoordinates.width creativeElements.verticalImages.croppingCoordinates.width (property of SD creative)	|	|
|creative.<br />customImages.<br />formatProperties.<br />height	| 	|customImageCrop.<br />height	|creativeElements.squareImages.croppingCoordinates.height creativeElements.horizontalImages.croppingCoordinates.height creativeElements.verticalImages.croppingCoordinates.height (property of SD creative)	|	|
|creative.<br />videos.<br />assetId	| 	|creative.<br />videoAssetId	|properties.<br />video.<br />assetId	|	|
|creative.<br />videos.<br />assetVersion	| 	|creative.<br />videoAssetId (version appended)	|properties.<br />video.<br />assetVersion	|	|
|creative.<br />headline	| 	|creative.<br />headline	|properties.<br />headline	|customText (SP only)	|
|creative.<br />brandName	| 	|creative.<br />brandName	| 	|	|
|creative.<br />landingPage	| 	|landingPage	| 	|	|
|creative.<br />landingPage.<br />landingPageType	| 	|landingPage.<br />pageType	|landingPageType	|landingPage.<br />type	|
|creative.<br />landingPage.<br />landingPageUrl	| 	|landingPage.<br />url	|landingPageUrl	|landingPage.<br />url	|
|creative.<br />cards	| 	|creative.<br />subpages	| 	|	|
|creative.<br />cards.<br />headline	| 	|creative.<br />subpages.<br />pageTitle	| 	|	|
|creative.<br />cards.<br />landingPage.<br />landingPageType	| 	|STORE	| 	|	|
|creative.<br />cards.<br />landingPage.<br />landingPageUrl	| 	|creative.<br />subpages.<br />url	| 	|	|
|creative.<br />cards.<br />product	| 	| 	| 	|	|
|creative.<br />cards.<br />products.<br />productIdType	| 	|ASIN	| 	|	|
|creative.<br />cards.<br />products.<br />productId	| 	|creative.<br />subpages.<br />asin	| 	|	|

## Representations

The following table shows the different areas where ads are surfaced in the Amazon Ads API.

| Resource | User guides |
|------|------|---------|
| [sp/productAds](sponsored-products/3-0/openapi/prod#tag/Product-ads)  | [SP campaign model diagram](guides/sponsored-products/get-started/campaign-structure) <br /> [SP product ads overview](guides/sponsored-products/product-ads) |
| [sb/v4/ads](sponsored-brands/3-0/openapi/prod#tag/Ads) | [SB campaign model diagram](guides/sponsored-brands/campaigns/structure) |
| [sb/adCreatives](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives) | [SB campaign model diagram](guides/sponsored-brands/campaigns/structure)| 
| [sd/productAds](sponsored-display/3-0/openapi#tag/Product-Ads) | [SD campaigns overview](guides/sponsored-display/overview) |
| [sd/creatives](sponsored-display/3-0/openapi#tag/Creatives) | [Creatives guide](guides/sponsored-display/creatives) | 
| [st/ads](sponsored-tv-open-beta#tag/Ads) | [ST guide](guides/sponsored-tv/overview) | 
| [st/creatives](sponsored-tv-open-beta#tag/Creatives) | [ST guide](guides/sponsored-tv/overview) |
| [Amazon Marketing Stream](guides/amazon-marketing-stream/overview) | [Ads dataset](guides/amazon-marketing-stream/datasets/sponsored-ads-campaign-management#ads-dataset-beta) | 
| [ads/exports](exports) | [Exports overview](guides/exports/overview) |

# JSON examples

Below you can find examples of how each ad product represents an ad. 

### Generic

The generic sample includes a JSON representation of all possible fields in the common schema.

```json
{
    "adGroupId": "string",
    "adId": "string",
    "adProduct": "SPONSORED_BRANDS",
    "state": "ENABLED",
    "adType": "PRODUCT_AD",
    "creative": {
        "products": [
            {
                "productIdType": "ASIN",
                "productId": "string"
            }
        ],
        "headline": "string",
        "brandLogo": {
            "assetId": "string",
            "assetVersion": "string",
            "formatProperties": {
                "top": 0,
                "left": 0,
                "height": 0,
                "width": 0
            }
        },
        "brandName": "string",
        "landingPage": {
            "landingPageType": "PRODUCT_LIST",
            "landingPageUrl": "string"
        },
        "videos": [
            {
                "assetId": "string",
                "assetVersion": "string"
            }
        ],
         "customImages": [
            {
                "assetId": "string",
                "assetVersion": "string",
                "formatProperties": {
                    "top": 0,
                    "left": 0,
                    "width": 0,
                    "height": 0
                }
            }
        ],
        "cards": [
            {
                "headline": "string",
                "landingPage": {
                    "landingPageType": "STORE",
                    "landingPageUrl": "string"
                },
                "product": {
                    "productIdType": "ASIN",
                    "productId": "string"
                }
            }
        ]
    },
    "deliveryStatus": "NOT_DELIVERING",
    "deliveryReasons": [
        "CAMPAIGN_PAUSED"
    ],
    "creationDateTime": "2021-11-29T18:37:59.008Z",
    "lastUpdatedDateTime": "2024-01-03T01:31:40.346Z",
    "adVersionId": "string"
}
```

### Sponsored Products generic

```json
{
    "adId": "string",
    "adGroupId": "string",
    "adProduct": "SPONSORED_PRODUCTS",
    "state": "ENABLED",
    "adType": "PRODUCT_AD",
    "creative": {
        "headline": "string",
        "products": [
            {
                "productIdType": "ASIN",
                "productId": "string"
            }
        ]
    },
    "deliveryStatus": "NOT_DELIVERING",
    "deliveryReasons": [
        "CAMPAIGN_PAUSED"
    ],
    "creationDateTime": "2018-11-08T15:41:14Z",
    "lastUpdatedDateTime": "2018-11-08T15:41:14Z"
}
```

### Sponsored Brands generic

```json
{
    "adGroupId": "string",
    "adId": "string",
    "adProduct": "SPONSORED_BRANDS",
    "state": "ENABLED",
    "adType": "PRODUCT_COLLECTION",
    "creative": {
        "products": [
            {
                "productIdType": "ASIN",
                "productId": "string"
            }
        ],
        "headline": "string",
        "brandLogo": {
            "assetId": "string",
            "assetVersion": "string",
            "formatProperties": {
                "top": 0,
                "left": 0,
                "height": 0,
                "width": 0
            }
        },
        "brandName": "string",
        "landingPage": {
            "landingPageType": "PRODUCT_LIST",
            "landingPageUrl": "string"
        },
        "videos": [
            {
                "assetId": "string",
                "assetVersion": "string"
            }
        ],
         "customImages": [
            {
                "assetId": "string",
                "assetVersion": "string",
                "formatProperties": {
                    "top": 0,
                    "left": 0,
                    "width": 0,
                    "height": 0
                }
            }
        ],
        "cards": [
            {
                "headline": "string",
                "landingPage": {
                    "landingPageType": "STORE",
                    "landingPageUrl": "string"
                },
                "product": {
                    "productIdType": "ASIN",
                    "productId": "string"
                }
            }
        ]
    },
    "deliveryStatus": "NOT_DELIVERING",
    "deliveryReasons": [
        "CAMPAIGN_PAUSED"
    ],
    "creationDateTime": "2021-11-29T18:37:59.008Z",
    "lastUpdatedDateTime": "2024-01-03T01:31:40.346Z",
    "adVersionId": "string"
}
```

### Sponsored Display generic

```json
{
    "adGroupId": "string",
    "adId": "string",
    "adProduct": "SPONSORED_DISPLAY",
    "state": "ENABLED",
    "adType": "IMAGE",
    "creative": {
        "products": [
            {
                "productIdType": "ASIN",
                "productId": "string"
            }
        ],
        "headline": "string",
        "brandLogo": {
            "assetId": "string",
            "assetVersion": "string",
            "formatProperties": {
                "top": 0,
                "left": 0,
                "height": 0,
                "width": 0
            }
        },
        "landingPage": {
            "landingPageType": "PRODUCT_LIST",
            "landingPageUrl": "string"
        },
        "videos": [
            {
                "assetId": "string",
                "assetVersion": "string"
            }
        ],
         "customImages": [
            {
                "assetId": "string",
                "assetVersion": "string",
                "formatProperties": {
                    "top": 0,
                    "left": 0,
                    "width": 0,
                    "height": 0
                }
            }
        ]
    },
    "deliveryStatus": "NOT_DELIVERING",
    "deliveryReasons": [
        "CAMPAIGN_PAUSED"
    ],
    "creationDateTime": "2021-11-29T18:37:59.008Z",
    "lastUpdatedDateTime": "2024-01-03T01:31:40.346Z",
    "adVersionId": "string"
}
```

### Product ad

```json
{
    "adId": "string",
    "adGroupId": "string",
    "adProduct": "SPONSORED_PRODUCTS",
    "adType": "PRODUCT_AD",
    "state": "ENABLED",
    "creative": {
        "headline": "string",
         "products": [
            {
                "productIdType": "ASIN",
                "productId": "string"
            }
        ]
    },
    "deliveryStatus": "NOT_DELIVERING",
    "deliveryReasons": [
        "CAMPAIGN_PAUSED"
    ],
    "creationDateTime": "2018-11-08T15:41:14Z",
    "lastUpdatedDateTime": "2018-11-08T15:41:14Z"
}
```

### Image

```json
{
    "adId": "string",
    "adGroupId": "string",
    "adProduct": "SPONSORED_DISPLAY",
    "adType": "IMAGE",
    "state": "ENABLED",
    "creative": {
        "headline": "string",
        "products": [
            {
                "productIdType": "ASIN",
                "productId": "string"
            }
        ],
        "brandLogo": {
            "assetId": "string",
            "assetVersion": "string",
            "formatProperties": {
                "top": 0,
                "left": 0,
                "width": 0,
                "height": 0
            }
        },
        "customImages": [
            {
                "assetId": "string",
                "assetVersion": "string",
                "formatProperties": {
                    "top": 0,
                    "left": 0,
                    "width": 0,
                    "height": 0
                }
            }
        ]
    },
    "deliveryStatus": "NOT_DELIVERING",
    "deliveryReasons": [
        "OTHER"
    ],
    "creationDateTime": "2024-02-07T16:33:42.375Z",
    "lastUpdatedDateTime": "2024-02-07T16:33:42.375Z"
}
```

### Video

```json
{
    "adId": "string",
    "adGroupId": "string",
    "adProduct": "SPONSORED_BRANDS",]
    "adType": "VIDEO",
    "state": "ENABLED",
    "creative": {
        "products": [
            {
                "productIdType": "ASIN",
                "productId": "string"
            }
        ],
        "videos": [
            {
                "assetId": "string",
                "assetVersion": "string"
            }
        ],
        "landingPage": {
            "landingPageType": "DETAIL_PAGE",
            "landingPageUrl": "string"
        },
        "headline": "string",
        "brandLogo": {
            "assetId": "string",
            "assetVersion": "string",
            "formatProperties": {
                "top": 0,
                "left": 0,
                "width": 0,
                "height": 0
            }
        }
    },
    "deliveryStatus": "DELIVERING",
    "creationDateTime": "2023-10-26T18:25:38.311Z",
    "lastUpdatedDateTime": "2023-12-27T21:50:39.042Z",
    "name": "string",
    "adVersionId": "string"
}
```

### Product collection

```json
{
    "adGroupId": "string",
    "adId": "string",
    "adProduct": "SPONSORED_BRANDS",
    "state": "ENABLED",
    "adType": "PRODUCT_COLLECTION",
    "creative": {
        "products": [
            {
                "productIdType": "ASIN",
                "productId": "string"
            }
        ],
        "headline": "string",
        "brandLogo": {
            "assetId": "string",
            "assetVersion": "string",
            "formatProperties": {
                "top": 0,
                "left": 0,
                "height": 0,
                "width": 0
            }
        },
        "brandName": "string",
        "landingPage": {
            "landingPageType": "PRODUCT_LIST",
            "landingPageUrl": "string"
        },
        "customImages": [
            {
                "assetId": "string",
                "assetVersion": "string",
                "formatProperties": {
                    "top": 0,
                    "left": 0,
                    "width": 0,
                    "height": 0
                }
            }
        ]
    },
    "deliveryStatus": "NOT_DELIVERING",
    "deliveryReasons": [
        "CAMPAIGN_PAUSED"
    ],
    "creationDateTime": "2021-11-29T18:37:59.008Z",
    "lastUpdatedDateTime": "2024-01-03T01:31:40.346Z",
    "adVersionId": "string"
}
```

### Store spotlight

```json
{
        "adGroupId": "string",
        "adId": "string",
        "adProduct": "SPONSORED_BRANDS",
        "state": "ENABLED",
        "adType": "STORE_SPOTLIGHT",
        "creative": {
            "headline": "string",
            "brandLogo": {
                "assetId": "string",
                "assetVersion": "string",
                "formatProperties": {
                    "top": 0,
                    "left": 0,
                    "height": 0,
                    "width": 0
                }
            },
            "brandName": "string",
            "landingPage": {
                "landingPageType": "STORE",
                "landingPageUrl": "string"
            },
            "cards": [
                {
                    "headline": "string",
                    "landingPage": {
                        "landingPageType": "STORE",
                        "landingPageUrl": "string"
                    },
                    "product": {
                        "productIdType": "ASIN",
                        "productId": "string"
                    }
                }
            ]
        },
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "CAMPAIGN_PAUSED"
        ],
        "creationDateTime": "2022-09-27T16:49:18.994Z",
        "lastUpdatedDateTime": "2023-12-28T23:27:22.616Z",
        "adVersionId": "string"
    }
```