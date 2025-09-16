---
title:  Amazon DSP creative management
description: A guide for using the latest Amazon DSP creative management APIs
type: guide
interface: api
tags: 
    - Amazon DSP
    - Creatives
keywords:
    - guide
---

# Amazon DSP creative generation and management

## Overview

The new Amazon DSP Ad Creative Management API’s enable advertisers to programmatically manage Amazon DSP ad creatives (supporting Display, Video, Audio, Third Party, and Component Formats). Use cases include:

* **Creating and updating ad creatives:** Create new Ad Creatives by specifying properties such as assets and products which can be used to serve on Amazon DSP ad groups.

* **Viewing ad creative moderation results:** View the moderation results for your ad creatives to receive actionable feedback on Amazon DSP ad creative policy guidelines. 

## Ad creative object hierarchy

These APIs streamline the process of creating, validating, and managing creatives in Amazon DSP. They involve two fundamental objects:

1. **Ad Creative**: This is the parent-level object that holds all information about a creative, such as metadata, assets, and information about the product being promoted. It is the base from which all creatives are generated and managed.

2. **Ad Experience**: Ad Experiences represent the rendered view of a creative given the elements provided by the `adCreative` object. It allows developers to visualize the ad in its final form, further enabling them to adjust and optimize their creative. Each Ad Experience represents a specific rendering of the Ad Creative. Ad creatives require at least one associated Ad Experience to serve on an Amazon DSP ad group. 

![Image: Developer Guide - Ad Creative management API - object hierarchy](/_images/dsp/dsp-creative-hierarchy.png)

## Create an ad creative

The following steps guide you through creating an Ad Creative, associating it to an Ad Experience, and reviewing any Amazon DSP moderation policy responses. 

>[NOTE] For help associating ad creatives to ad groups, please review the [Amazon DSP Ad Creative Association Developer Guide](guides/dsp/creative-associations).

>[NOTE] For help with uploading media assets, please review the [asset library developer guides](guides/creative-asset/asset-library-overview).

### Step 1: Generate an Ad Creative

In this step, you will use an asset (previously uploaded to the Amazon Ads Asset Library), a click through, and an impression tracker to create a new Ad Creative object. In the response, you will receive an Ad Creative ID that references the object you've created.


>[NOTE] Your ad creative will not be eligible for ad group association until you select an ad experience to render the creative with.
[POST /dsp/v1/adCreatives](dsp-ad-creative#tag/DSP-Ad-Creative/operation/createAdCreativeV1)

```
{
    "name": "ADSP API Guide Ad Creative",
    "language": "en",
    "country": "US",
    "adChoicesPosition": "TOP_RIGHT",
    "adCreativeFormatProperties": {
      "adCreativeFormatType": "DISPLAY",
      "creativeSizes": [
        {
          "width": 300,
          "height": 250,
        },
      ],
      "assets": [
        {
          "assetId": "${assetId}",
          "assetVersion": "version_v1",
          "applyBorder": true,
        },
      ],
      "clickActions": [
        {
          "clickActionType": "URL",
          "urlDestination": "https://advertising.amazon.com/API/docs/en-us/dsp-ad-creative#tag/DSP-Ad-Creative/operation/createAdCreativeV1",
        },
      ],
      "trackingUrls": [
        {
          "type": "IMPRESSION",
          "url": "https://advertising.amazon.com",
          "associatedAdExperience": "STANDARD_DISPLAY"
        }
      ]
    }
  }
```

The response from the API will be:

```
{
  "adCreativeId": "${adCreativeId}"
}
```

>[NOTE] When the `associatedAdExperience` sub-property is set, it means that the wrapping object will only apply to the specified Ad Experience. If it is not set, it will apply to all ad experiences associated to the ad creative. In the case of the previous example, we have specified that the impression tracker should only be used when rendering this Ad Creative for the `STANDARD_DISPLAY` ad experience.

#### Supported languages in each ad marketplace

| Country code | Languages |
| -- | -- | 
| US | English, Spanish | 
|CA| English, French |
|NL| Dutch, English, French |
|DE| German, English, Czech, Polish, Turkish, French, Italian, Spanish |
|ES| Spanish, Portuguese, French, English, German, Italian |
|AE| English, Arabic |
|SA| Arabic, English |
|JP| Japanese, English, Chinese |
|FR| French, English, German, Italian, Spanish |
|IT| Italian, English, French, German, Spanish |
|UK| English, French, German, Italian, Spanish |
|IN| English, Hindi, Tamil, Telugu, Kannada, Marathi, Malayalam, Bengali, Gujarati, Punjabi |
|AU| English |
|MX| Spanish, English |
|BR| Portuguese, English |
|SE| Swedish, English, Norwegian, Finnish, Danish.  |
|TR | Turkish |

### Step 2:  Associate an Ad Experience

Use the `adCreativeId` acquired in Step 1 to call the `updateAdExperience` API. You must associate an Ad Experience with your Ad Creative before adding it to an ad group. 

[PUT /dsp/v1/adExperiences](dsp-ad-creative#tag/DSP-Ad-Experience/operation/updateAdExperiencesV1)

```
{
    "adCreativeId": "${adCreativeId}",
    "adExperiences": ["STANDARD_DISPLAY"]
}
```

The API will return a `204 no content` response if the request was successful. Each Ad Experience you associate to the Ad Creative will have its own set of requirements on the Ad Creative model. The Ad Experiences API is an all or nothing operation: this means that if any of the requested Ad Experiences are invalid for the specified Ad Creative, none of the requested Ad Experiences will be added to the Ad Creative. Error responses include validation messages for each of the requested Ad Experiences. 

>[NOTE] As of now, only a single Ad Experience can be associated to a given Ad Creative. Additionally, Ad Experiences may not be removed from Ad Creatives once associated.

The list of Ad Experiences available to be associated to your Ad Creative will depend on the `adCreativeFormatType` selected in Step 1. For a DISPLAY Ad Creative, only display Ad Experiences can be added. The table below details the current mapping:

|Ad Creative Format Type	|Ad Experiences	|
|---	|---	|
|DISPLAY	|STANDARD_DISPLAY	|
|VIDEO	|ONLINE\_VIDEO, STREAMING\_TV\_VIDEO	|
|COMPONENT	|RESPONSIVE\_ECOMMERCE	|
|AUDIO	|Not Yet Released	|
|THIRD PARTY	|THIRD\_PARTY\_VIDEO, THIRD\_PARTY\_DISPLAY	|

### Step 3: View Ad Creative moderation 

Once a Ad Creative has been associated to a line item, Amazon DSP will begin screening content moderation policies against the Ad Creative. Moderation decisions enforce Amazon DSP and associated publisher policies to ensure the Ad Creative’s content meets the criteria of the supply it is targeted to serve. 

The Ad Creative API Moderation endpoint vends granular moderation statuses for each of the selected Ad Experiences. This means it is possible for one Ad Experience to be approved and start serving while another is rejected. Additionally, moderations statuses for a given Ad Experience may differ for each line item it is associated to (in this case the response from this API will be `APPROVED_WITH_EXCEPTIONS`). To see the status and reason for each (Ad Group, Ad Creative) pair, please use the association moderation API.

[GET /dsp/v1/adCreatives/{adCreativeId}/moderations](dsp-ad-creative#tag/DSP-Ad-Creative-Moderation/operation/getAdCreativeModerationV1)

```
{
  "maxResults": 20,
  "nextToken": "${NEXT_TOKEN}",
  "adCreativeIdFilter": {
    "include": ["${adCreativeId}"]
  }
}
```

The GET response will look something like:

```
{
  "totalResults": 1,
  "nextToken": "${NEXT_TOKEN}",
  "adCreativeStatuses": [
    {
      "adCreativeId": "${adCreativeId}",
      "adCreativeModerationStatus": "PENDING",
      "adExperiences": [
        {
          "adExperience": "STANDARD_DISPLAY",
          "reasons": [],
          "moderationStatus": "PENDING"
        }
      ]
    }
  ]
}
```

>[NOTE] The overall `adCreativeModerationStatus` is a high level indication of the granular Ad Experience statuses. However, the actual statuses used in ad serve will be based on the specific ad experiences moderation status.

### Step 4: View Ad Creative moderation results at the component level

Amazon DSP moderates certain ad creatives at the component level. That means, along with moderating the overall ad creatives, individual components, such as an images, are also being moderated, so that you can figure out which components are causing your creative rejection and fix the issue. This component level moderation is only enabled for standard display and asset based creatives. 

[POST /dsp/v1/adCreatives/moderations/list](dsp-ad-creative#tag/DSP-Ad-Creative-Moderation) returns both the overall moderation status of an ad creative along with the creative components being moderated and its moderation status. There are three possible statuses associated with a component: `APPROVED`, `NOT_APPROVED`, and `COMPONENT_NOT_YET_MODERATED`. The `COMPONENT_NOT_YET_MODERATED` status will be returned when the corresponding creative is not yet moderated or if the creative was created prior to Amazon DSP enabling component level moderation.

[POST /dsp/v1/adCreatives/componentModerations/list](dsp-ad-creative#tag/DSP-Ad-Creative-Moderation)

```json
{
  "adCreativeId": "${adCreativeId}",
  "maxResults": 2,
  "nextToken": "${NEXT_TOKEN}"
}
```

The response will look something like:

```json
{
    "adCreativeId": "${adCreativeId}",
    "components": [
        {
            "componentId": "${assetId}",
            "componentVersion": "version_v1",
            "componentType": "IMAGE",
            "moderationStatus": "NOT_APPROVED",
            "reasons": [
                "To ensure accuracy, price/savings claims must be communicated via responsive ecommerce creatives (REC). Remove the price/savings claim from this creative. Refer to <a target=\"_blank\" href=\"https://advertising.amazon.com/resources/ad-policy/creative-acceptance/advertising-copy?ref_=a20m_us_spcs_cap_spcs_cap2#pricesandsavingclaims\">Policy</a> for details."
            ]
        },
        {
            "componentId": "${assetId}",
            "componentVersion": "version_v1",
            "componentType": "IMAGE",
            "moderationStatus": "APPROVED",
            "reasons": []
        }
    ],
    "totalResults": 2,
    "nextToken": "${NEXT_TOKEN}"
}
```

## A note on legacy creatives

Creatives created without using the new Ad Creative APIs (which may include creatives created with legacy types in the Amazon DSP ads console) are not supported by default in this new API. You will not be able to read, update or create legacy creative IDs or legacy creative types using this API. 

We have released support for updating and reading a subset of legacy creative types. These types will not support the association or removal of Ad Experiences, and it is not possible to associate a legacy creative type to a new Ad Creative created via this API. The following table is a map of `adCreativeFormatType` to legacy creative types supported by this API. 

|Ad Creative Format Type	|Legacy Ad Experiences	|
|---	|---	|
|DISPLAY	|IMAGE, IMAGE\_MOBILE\_OO, IMAGE\_MOBILE\_AAP	|
|VIDEO	|LEGACY\_VIDEO	|
|COMPONENT	|LEGACY\_RESPONSIVE\_ECOMMERCE	|
|AUDIO	|Not Yet Supported	|
|THIRD PARTY	|LEGACY\_THIRD\_PARTY\_VIDEO	|

## Additional APIs

### Updating existing ad creatives

Any updates to an Ad Creative will apply to all Ad Experiences associated with that Ad Creative. As a result, when updating an Ad Creative, the updates must be valid for all associated Ad Experiences. Otherwise, the request will fail, and you'll be given errors detailing why it failed.

>[NOTE] This API is a `PATCH` API, which means it supports partial updates. You need only provide the fields you plan to update within your request.

**Notes:** 

* For list fields, the full list must be provided. Partial updates within a list are not currently supported.
* If updating `adCreativeFormatProperties`, the `adCreativeFormatType` field must be provided as well. The `adCreativeFormatType` field is immutable, and so the input must match what is already listed on the creative.

[PATCH /dsp/v1/adCreatives/{adCreativeId}](dsp-ad-creative#tag/DSP-Ad-Creative/operation/updateAdCreativeV1)

```
{
  "name": "updatedName",
  "adCreativeFormatProperties": {
        "adCreativeFormatType": "DISPLAY",
        "creativeSizes": [
          {
            "width": 720,
            "height": 90,
          },
        ]
  }
}
```

Example [PATCH /dsp/v1/adCreatives/{adCreativeId}](https://advertising.amazon.com/API/docs/en-us/dsp-ad-creative#tag/DSP-Ad-Creative/operation/updateAdCreativeV1) for responsive eCommerce:

```
{
    "name": "RECTEST",
    "language": "en",
    "country": "US",
    "adChoicesPosition": "TOP_RIGHT",
    "adCreativeFormatProperties": {
        "adCreativeFormatType": "COMPONENT",
        "assets": [
            {
                "assetId": "amzn1.assetlibrary.asset1.2dd8a4feaf46b6fff7e43685ecee5fce",
                "assetVersion": "version_v1",
                "assetType": "CUSTOM_IMAGE",
                "assetCroppingCoordinates": [
                    {
                        "x": 85.5,
                        "y": 0,
                        "height": 954,
                        "width": 954,
                        "aspectRatio": 1
                    },
                    {
                        "x": 294.1875,
                        "y": 0,
                        "height": 954,
                        "width": 536.625,
                        "aspectRatio": 0.5625
                    },
                    {
                        "x": 0,
                        "y": 182.4973821989529,
                        "height": 589.0052356020942,
                        "width": 1125,
                        "aspectRatio": 1.91
                    }
                ]
            },
            {
                "assetId": "amzn1.assetlibrary.asset1.2dd8a4feaf46b6fff7e43685ecee5fce",
                "assetVersion": "version_v1",
                "assetType": "LOGO_IMAGE"
            }
        ],
        "adCopy": [
            {
                "type": "HEADLINE",
                "value": "TEST_HEADLINE"
            }
        ],
        "products": [
            {
                "productId": "B0BLS3Y632",
                "productType": "ASIN",
                "productTitle": "test product"
            }
        ],
        "trackingUrls": [
            {
                "type": "CLICK",
                "url": "https://www.amazon.com?tracking=track",
                "associatedAdExperience": "RESPONSIVE_ECOMMERCE"
            },
            {
                "type": "IMPRESSION",
                "url": "https://www.amazon.com/?test",
                "associatedAdExperience": "RESPONSIVE_ECOMMERCE"
            }
        ],
        "additionalAttributes": {
            "optimizationGoal": "CLICK_THROUGH_RATE",
            "targetPlacements": [
                "DISPLAY",
                "NATIVE"
            ],
            "allowedRetailFormats": [
                "SHOP_NOW"
            ],
            "allowThirdPartySellers": true
        }
    }
}
```

The response will be a `204 no content` if successful. 

### Listing ad creatives

You can query for your Ad Creatives based on the provided Amazon DSP Advertiser/Account ID within the header. We also support additional filtering of these responses based on a provided list of Ad Creative IDs. This API will return all properties provided by the `POST` and `PATCH` Ad Creative APIs.

[POST /dsp/v1/adCreatives/list](dsp-ad-creative#tag/DSP-Ad-Creative/operation/listAdCreativesV1)

```
{
  "adCreativeIdFilter": {
    "include": ["${adCreativeId}"]
  },
  "maxResults": 100,
  "nextToken": null
}
```

All `include` fields within the API request are optional. If none are provided, the API returns a paginated list (with a page size of 50) of the Ad Creatives associated with the Account ID in the request header. If there are more Ad Creatives to return than can fit on a single page, a `nextToken` string will be returned in the response that can be provided on a subsequent API request to fetch the next page. 

An example response:

```
{
  "totalResults": 1,
  "nextToken": null,
  "adCreatives": [
    {
      "name": "updatedName",
      "language": "en",
      "country": "US",
      "adChoicesPosition": "TOP_RIGHT",
      "adCreativeFormatProperties": {
        "adCreativeFormatType": "DISPLAY",
        "creativeSizes": [
          {
            "width": 720,
            "height": 90,
          },
        ],
        "assets": [
          {
            "assetId": "${assetId}",
            "assetVersion": "version_v1",
            "applyBorder": true,
          },
        ],
        "clickActions": [
          {
            "clickActionType": "URL",
            "urlDestination": "https://advertising.amazon.com/API/docs/en-us/dsp-ad-creative#tag/DSP-Ad-Creative/operation/createAdCreativeV1",
          },
        ],
        "trackingUrls": [
          {
            "type": "IMPRESSION",
            "url": "https://advertising.amazon.com",
            "associatedAdExperience": "STANDARD_DISPLAY"
          }
        ]
      }
    }
  ]
}
```

### Previewing ad creatives

The Preview API enables you to view a sample impression response for a selected experience of your Ad Creative. You can further specify `previewOverrides` which allows you to preview different variations of a given Ad Experience. For example, you can select which ASIN to preview for a `RESPONSIVE_ECOMMERCE` creative. The preview API will return an HTML string that can be rendered in your browser. 

The API supports both previewing using an existing `adCreativeId` and previewing by providing an Ad Creative model, just like the input in [step 1: generate an ad creative](#step-1-generate-an-ad-creative). For an existing Ad Creative, it is possible to preview an Ad Experience not associated to that Ad Creative, as long as that experience has the same `adCreativeFormatType` as the selected Ad Creative. 

[POST /dsp/v1/adCreatives/preview](dsp-ad-creative#tag/DSP-Ad-Creative-Preview/operation/previewAdCreativeV1)

```
{
  "adCreativeId": "${adCreativeId}",
  "adExperience": "STANDARD_DISPLAY",
  "previewOverrides": {
    "sizeFilter": {
      "height": 720,
      "width": 90
    }
  }
}
```

Responsive eCommerce [POST /dsp/v1/adCreatives/preview](https://advertising.amazon.com/API/docs/en-us/dsp-ad-creative#tag/DSP-Ad-Creative-Preview/operation/previewAdCreativeV1) sample payload:

```
{
  "adExperience": "RESPONSIVE_ECOMMERCE",
  "adCreativeId": "${adCreativeId}",
  "previewOverrides": {
    "adChoicesPositionFilter": "TOP_RIGHT",
    "previewSite": "ON_AMAZON",
    "adCreativeFormatPreviewOverrides": {
      "adCreativeFormatType": "COMPONENT",
      "sizeFilter": {
        "width": 320,
        "height": 50
      },
      "productFilter": "B005QB4X4C",
      "retailFormatFilter": "SHOP_NOW"
    }
  }
}
```

Sample response:

```
{
  "previewContent": "<html></html>"
}
```

### Pre-validating ad experiences

Sometimes, while building a new Ad Creative, you might want to know which Ad Experiences your current Ad Creative can associate to without actually completing the task of associating it to any Ad Experience. For example, imagine you're building a UI that displays a list of Ad Experiences, but which greys out Ad Experiences the Ad Creative can't be associated with. In that case, you'll need to use the creative validation API. This API will take an existing `adCreativeId` and return the list of Ad Experiences that can be associated to its `adCreativeFormatType`. For each experience, there will be a boolean flag indicating if the experience is valid, and just in case the experience is not valid, you'll see a list of reasons why.

[GET /dsp/v1/adCreatives/{adCreativeId}/validation](dsp-ad-creative#tag/DSP-Ad-Creative-Validation)

Sample response:

```
{
  "totalResults": 1,
  "nextToken": null,
  "validations": [
    {
      "adExperience": "STANDARD_DISPLAY",
      "validationStatus": "VALID",
      "errors": []
    }
  ]
}
```

### Viewing associated ad experiences

Since Ad Experiences are modeled as a separate resource from Ad Creatives, we do not return the list of associated Ad Experiences in the `listAdCreatives` API. However, we do offer the `GET` Ad Experiences API. This API takes an `adCreativeId` as input and returns the list of associated ad experiences within the response.

[GET /dsp/v1/adExperiences?adCreativeId={adCreativeId}&nextToken={nextToken}](dsp-ad-creative#tag/DSP-Ad-Experience/operation/getAdExperienceV1)

```
{
  "maxResults": 20,
  "nextToken": "${NEXT_TOKEN}",
  "adCreativeIdFilter": {
    "include": ["${adCreativeId}"]
  }
}
```

Sample response:

```
{
  "totalResults": 1,
  "nextToken": "${NEXT_TOKEN}",
  "adCreatives": [
    {
      "adCreativeId": "${adCreativeId}",
      "adExperiences": [
        "STANDARD_DISPLAY"
      ]
    }
  ]
}
```

## Specific ad experience examples

### Videos

#### Sample request: online video

This request creates a `VIDEO` adCreative and sets `trackingUrls` items to both preselected ad experiences and to all adExperiences.

[PATCH /dsp/v1/adCreatives/{adCreativeId}](dsp-ad-creative#tag/DSP-Ad-Creative/operation/updateAdCreativeV1)

```
{
    "name": "ONLINE_VIDEO",
    "language": "en",
    "country": "US",
    "adCreativeFormatProperties": {
        "adCreativeFormatType": "VIDEO",
        "assets": [
            {
                "assetId": "{{asset_id}}",
                "assetVersion": "{{asset_version}}"
            }
        ],
        "trackingUrls": [
            {
                "url": "https://amazon.com/impressions1",
                "associatedAdExperience": "ONLINE_VIDEO",
                "type": "IMPRESSION"
            },
            {
                "url": "https://amazon.com/impressions2",
                "associatedAdExperience": "ONLINE_VIDEO",
                "type": "IMPRESSION"
            },
            {
                "url": "https://amazon.com/impressions3",
                "type": "IMPRESSION" // applies to all associated adExperiences
            }
        ]
    }
}
```

This request associates an `ONLINE_VIDEO` ad experience to the video ad creative above:

[PUT /dsp/v1/adExperiences](dsp-ad-creative#tag/DSP-Ad-Experience/operation/updateAdExperiencesV1)

```
{
    "adCreativeId": "{{adCreative_id}}",
    "adExperiences": [
        {
            "adExperience": "ONLINE_VIDEO"
        }
    ]
}
```

#### Sample request: streaming TV videos

This request creates a `VIDEO` adCreative and sets `trackingUrls` items to both preselected ad experiences and to all adExperiences.

[PATCH /dsp/v1/adCreatives/{adCreativeId}](dsp-ad-creative#tag/DSP-Ad-Creative/operation/updateAdCreativeV1)

```
{
    "name": "STREAMING_TV_VIDEO",
    "language": "en",
    "country": "US",
    "adCreativeFormatProperties": {
        "adCreativeFormatType": "VIDEO",
        "assets": [
            {
                "assetId": "{{asset_id}}",
                "assetVersion": "{{asset_version}}"
            }
        ],
        "trackingUrls": [
            {
                "url": "https://amazon.com/impressions1",
                "associatedAdExperience": "STREAMING_TV_VIDEO",
                "type": "IMPRESSION"
            },
            {
                "url": "https://amazon.com/impressions2",
                "associatedAdExperience": "STREAMING_TV_VIDEO",
                "type": "IMPRESSION"
            },
            {
                "url": "https://amazon.com/impressions3",
                "type": "IMPRESSION" // applies to all associated adExperiences
            }
        ]
    }
}
```

This request associates a `STREAMING_TV_VIDEO` adExperience to the newly created video ad creative:

[PUT /dsp/v1/adExperiences](dsp-ad-creative#tag/DSP-Ad-Experience/operation/updateAdExperiencesV1)

```
{
    "adCreativeId": "{{adCreative_id}}",
    "adExperiences": [
        {
            "adExperience": "STREAMING_TV_VIDEO"
        }
    ]
}
```

#### Business validation rules

Prior to creating an online video ad creative, you must first upload and register a video asset in asset library. To be a valid asset for online video, the video must meet the following criteria:

* The video must have an `assetStatus` of `ACTIVE`. A video becomes active once it finishes transcoding. Any creative you try to save while the status is `PROCESSING` will throw a validation error. 
* The video must pass the `DEMAND_SIDE_PLATFORM_OLV` specification profile in the asset library.

#### Other rules

* Online video creatives do not currently support `CLICK` trackers.
* Online video creatives only support `URL` click actions.
* Online video creatives should have a `hostedType` of `"AMAZON_HOSTED"`

### Standard Display

#### Sample request: standard display

[PATCH /dsp/v1/adCreatives/{adCreativeId}](dsp-ad-creative#tag/DSP-Ad-Creative/operation/updateAdCreativeV1)

```
{
  "name": "Standard Display Test AdCreative",
  "language": "en",
  "country": "US",
  "hostedType": "AMAZON_HOSTED",
  "adChoicesPosition": "TOP_RIGHT",
  "adCreativeFormatProperties": {
    "adCreativeFormatType": "DISPLAY",
    "creativeSizes": [{"width":320,"height":50}],
    "clickActions": [
      {
        "clickActionType": "URL",
        "urlDestination": "https://www.amazon.com"
      }
    ],
    "assets": [
      {
        "assetId": "{{asset ID}}",
        "assetVersion": "{{version ID}}"
      }
    ],
    "trackingUrls": [
        {
          "type": "IMPRESSION",
          "url": "https://www.impression.com"
        },
        {
          "type": "CLICK",
          "url": "https://www.click.com"
        }
      ]
  },
    "additionalHtml": "<script></script>"
}
```

This request associates a `STANDARD_DISPLAY` ad experience to the newly created `Display` ad creative.

[PUT /dsp/v1/adExperiences](dsp-ad-creative#tag/DSP-Ad-Experience/operation/updateAdExperiencesV1)

```
{
    "adCreativeId": "{{adCreative_id}}",
    "adExperiences": [
        {
            "adExperience": "STANDARD_DISPLAY"
        }
    ]
}
```

#### Business validation rules

Prior to creating a Standard Display adCreative, you must first upload and register an asset in asset library. To be a valid standard display ad creative asset, the asset must meet the following criteria:

* AdCreative image assets must be of type `PNG`, `JPG`, `JPEG`, or `GIF`.

#### Other rules

* Required fields for standard display eligible adCreatives are `name`, `assets`, and `clickActions` of `clickActionType` `url`.
* An adCreative can’t have more than 10 assets.
* For the assets being added, the `creativeSizes` field must include the applicable width and height combinations.

### Responsive eCommerce

#### Sample request: responsive eCommerce

[PATCH /dsp/v1/adCreatives/{adCreativeId}](dsp-ad-creative#tag/DSP-Ad-Creative/operation/updateAdCreativeV1)

```
{
    "name": "RECTest",
    "language": "en",
    "country": "US",
    "hostedType": "AMAZON_HOSTED",
    "adChoicesPosition": "TOP_RIGHT",
    "adCreativeFormatProperties": {
        "adCreativeFormatType": "COMPONENT",
        "creativeSizes": [
            {
                "width": 320,
                "height": 50
            },
            {
                "width": 160,
                "height": 600
            },
            {
                "width": 414,
                "height": 125
            }
        ],
        "assets": [
            {
                "assetId": "amzn1.assetlibrary.asset1.2dd8a4feaf46b6fff7e43685ecee5fce",
                "assetVersion": "version_v1",
                "assetType": "CUSTOM_IMAGE",
                "assetCroppingCoordinates": [
                    {
                        "x": 85.5,
                        "y": 0,
                        "height": 954,
                        "width": 954,
                        "aspectRatio": 1.179245283018868
                    },
                    {
                        "x": 294.1875,
                        "y": 0,
                        "height": 954,
                        "width": 536.625,
                        "aspectRatio": 1.179245283018868
                    },
                    {
                        "x": 0,
                        "y": 182.4973821989529,
                        "height": 589.0052356020942,
                        "width": 1125,
                        "aspectRatio": 1.179245283018868
                    }
                ]
            },
            {
                "assetId": "amzn1.assetlibrary.asset1.2dd8a4feaf46b6fff7e43685ecee5fce",
                "assetVersion": "version_v1",
                "assetType": "LOGO_IMAGE"
            }
        ],
        "adCopy": [
            {
                "type": "HEADLINE",
                "value": "TEST_HEADLINE"
            }
        ],
        "products": [
            {
                "productId": "B0BLS3Y632",
                "productType": "ASIN",
                "productTitle": "TEST PRODUCT TITLE"
            }
        ],
        "trackingUrls": [
            {
                "type": "CLICK",
                "url": "https://www.amazon.com/click-test",
                "associatedAdExperience": "RESPONSIVE_ECOMMERCE"
            },
            {
                "type": "IMPRESSION",
                "url": "https://www.amazon.com/impression-test",
                "associatedAdExperience": "RESPONSIVE_ECOMMERCE"
            }
        ],
        "additionalAttributes": {
            "optimizationGoal": "CLICK_THROUGH_RATE",
            "targetPlacements": [
                "DISPLAY",
                "NATIVE"
            ],
            "allowedRetailFormats": [
                "SHOP_NOW"
            ],
            "allowThirdPartySellers": true
        }
    }
}
```

This request associates a `RESPONSIVE_ECOMMERCE` ad experience to the newly created `Component` ad creative.

[PUT /dsp/v1/adExperiences](dsp-ad-creative#tag/DSP-Ad-Experience/operation/updateAdExperiencesV1)

```
{
    "adCreativeId": "{{adCreativeId}}",
    "adExperiences": [
        {
            "adExperience": "RESPONSIVE_ECOMMERCE"
        }
    ]
}
```

Preview by model example request:

[POST /dsp/v1/adCreatives/preview](dsp-ad-creative#tag/DSP-Ad-Creative-Preview)

```
{
    "adExperience": "RESPONSIVE_ECOMMERCE",
    "creative": {
        "name": "RECTest",
        "language": "en",
        "country": "US",
        "hostedType": "AMAZON_HOSTED",
        "adChoicesPosition": "TOP_RIGHT",
        "adCreativeFormatProperties": {
            "adCreativeFormatType": "COMPONENT",
            "creativeSizes": [
                {
                    "width": 320,
                    "height": 50
                },
                {
                    "width": 160,
                    "height": 600
                },
                {
                    "width": 414,
                    "height": 125
                }
            ],
            "assets": [
                {
                    "assetId": "amzn1.assetlibrary.asset1.2dd8a4feaf46b6fff7e43685ecee5fce",
                    "assetVersion": "version_v1",
                    "associatedAdExperience": "RESPONSIVE_ECOMMERCE",
                    "assetType": "CUSTOM_IMAGE",
                    "assetCroppingCoordinates": [
                        {
                            "x": 175.25,
                            "y": 0.0,
                            "height": 728,
                            "width": 728,
                            "aspectRatio": 1.043956
                        },
                        {
                            "x": 16.0,
                            "y": 0.0,
                            "height": 269.443,
                            "width": 269.318,
                            "aspectRatio": 1.043956
                        },
                        {
                            "x": 0.0,
                            "y": 165.04712,
                            "height": 397.90576,
                            "width": 760.0,
                            "aspectRatio": 1.043956
                        }
                    ]
                },
                {
                    "assetId": "amzn1.assetlibrary.asset1.2dd8a4feaf46b6fff7e43685ecee5fce",
                    "assetVersion": "version_v1",
                    "associatedAdExperience": "RESPONSIVE_ECOMMERCE",
                    "assetType": "CUSTOM_IMAGE",
                    "assetCroppingCoordinates": [
                        {
                            "x": 90,
                            "y": 0.0,
                            "height": 1020,
                            "width": 573.75,
                            "aspectRatio": 0.5625
                        },
                        {
                            "x": 16.0,
                            "y": 0.0,
                            "height": 269.443,
                            "width": 269.318,
                            "aspectRatio": 1.043956
                        },
                        {
                            "x": 0.0,
                            "y": 165.04712,
                            "height": 397.90576,
                            "width": 760.0,
                            "aspectRatio": 1.043956
                        }
                    ]
                },
                {
                    "assetId": "amzn1.assetlibrary.asset1.2dd8a4feaf46b6fff7e43685ecee5fce",
                    "assetVersion": "version_v1",
                    "associatedAdExperience": "RESPONSIVE_ECOMMERCE",
                    "assetType": "CUSTOM_IMAGE",
                    "assetCroppingCoordinates": [
                        {
                            "x": 175.25,
                            "y": 0.0,
                            "height": 728.0,
                            "width": 409.5,
                            "aspectRatio": 1.043956
                        },
                        {
                            "x": 16.0,
                            "y": 0.0,
                            "height": 269.443,
                            "width": 269.318,
                            "aspectRatio": 1.043956
                        },
                        {
                            "x": 140,
                            "y": 165.04712,
                            "height": 418.848167539267,
                            "width": 800.0,
                            "aspectRatio": 1.91
                        }
                    ]
                },
                {
                    "assetId": "amzn1.assetlibrary.asset1.2dd8a4feaf46b6fff7e43685ecee5fce",
                    "assetVersion": "version_v1",
                    "assetType": "LOGO_IMAGE"
                }
            ],
            "adCopy": [
                {
                    "type": "HEADLINE",
                    "value": "TEST_HEADLINE"
                }
            ],
            "products": [
                {
                    "productId": "B0BL94TDYQ",
                    "productType": "ASIN",
                    "productTitle": "TEST PRODUCT TITLE",
                    "reviewUrl": "https://www.amazon.com/gp/customer-reviews/RWNI47M02L6I2/ref=cm_cr_dp_d_rvw_ttl?ie=UTF8&ASIN=B0BL94TDYQ",
                    "reviewText": "TEST_REVIEW"
                },
                {
                    "productId": "B001TJZO16",
                    "productType": "ASIN",
                    "productTitle": "TEST PRODUCT TITLE"
                }
            ],
            "trackingUrls": [
                {
                    "type": "CLICK",
                    "url": "https://www.amazon.com?tracking=track",
                    "associatedAdExperience": "RESPONSIVE_ECOMMERCE"
                },
                {
                    "type": "IMPRESSION",
                    "url": "https://www.amazon.com/?test",
                    "associatedAdExperience": "RESPONSIVE_ECOMMERCE"
                }
            ],
            "additionalAttributes": {
                "optimizationGoal": "CLICK_THROUGH_RATE",
                "targetPlacements": [
                    "DISPLAY",
                    "NATIVE"
                ]
            }
        }
    },
    "previewOverrides": {
        "sizeFilter": {
            "width": 320,
            "height": 50
        },
        "productFilter": "B0BL94TDYQ",
        "retailFormatFilter": "SHOP_NOW"
    }
}
```

#### Business validation rules

Prior to creating a responsive eCommerce ad creative, you must first upload and register an asset in the asset library. To be a valid responsive eCommerce ad creative asset, the asset must meet the following criteria:

* AdCreative image assets must be of type `PNG`, `JPG`, or `JPEG.`

#### Other rules

* Required fields for Responsive eCommerce are `name`, `products`, `language`, `country`, `hostedType`
* View [our API reference](dsp-ad-creative#tag/DSP-Ad-Creative/operation/createAdCreativeV1) to see the limits on each individual field
* When calling preview, ensure you include a `sizeFilter`, `productFilter` and `retailFormatFilter`



### Third party video

#### Sample request: third party video

This request creates a `THIRD_PARTY` ad creative format and sets `trackingUrls` items to both preselected ad experiences and to all adExperiences.

[PATCH /dsp/v1/adCreatives/{adCreativeId}](dsp-ad-creative#tag/DSP-Ad-Creative/operation/updateAdCreativeV1)

```
{
    "name": "Third party - video Test",
    "language": "en",
    "country": "US",
    "adCreativeFormatProperties": {
    "adCreativeFormatType": "THIRD_PARTY",
    "thirdPartyTag": {
      "tagSource": "https://sample_vast_tag.xml",
      "tagType": "VAST_VPAID",
      "destinationOnAmazon": "OFF_AMAZON"
    },
    "trackingUrls": [
      {
        "type": "IMPRESSION",
        "url": "https://www.amazon.com/?test",
        "associatedAdExperience": "THIRD_PARTY_VIDEO"
      }
    ]
  }
}
```

This request associates a `THIRD_PARTY_VIDEO` ad experience to the newly created third party ad creative:

[PUT /dsp/v1/adExperiences](dsp-ad-creative#tag/DSP-Ad-Experience/operation/updateAdExperiencesV1)

```
{
    "adCreativeId": "{{adCreative_id}}",
    "adExperiences": [
        {
            "adExperience": "THIRD_PARTY_VIDEO"
        }
    ]
}
```

#### Business validation rules

To create a third party video ad creative, you must provide a valid `VAST` or `VPAID` tag.  View our [video requirements for more details](https://advertising.amazon.com/resources/ad-specs/dsp/video). 

####  Other rules

* Third party video creatives do not currently support `CLICK` trackers.
* Third party video creatives only support a single `VAST` or `VPAID` tag.
* Third party video creatives support Nielsen DAR tags as an impression tracking url. 


### Third party display

#### Sample request: third party display

[PATCH /dsp/v1/adCreatives/{adCreativeId}](dsp-ad-creative#tag/DSP-Ad-Creative/operation/updateAdCreativeV1)

```
{
    "name": "Third Party Display Test Creative",
    "language": "en",
    "country": "US",
    "adCreativeFormatProperties": {
        "adCreativeFormatType": "THIRD_PARTY",
        "creativeSizes": [
            {
                "width": 120,
                "height": 60,
                "responsive": false
            }
        ],
        "thirdPartyTag": {
            "tagSource": "${third_party_tag}",
            "tagType": "DISPLAY",
            "destinationOnAmazon": "OFF_AMAZON"
        }
    }
}
```

This requests associates a `STANDARD_DISPLAY` ad experience the newly created `Display` ad creative:

[PUT /dsp/v1/adExperiences](dsp-ad-creative#tag/DSP-Ad-Experience/operation/updateAdExperiencesV1)

```
{
    "adCreativeId": "{{adCreative_id}}",
    "adExperiences": [
        {
            "adExperience": "THIRD_PARTY_DISPLAY"
        }
    ]
}
```

#### Business validation rules

The tag provided to create the Third Party Display ad-experience will be processed, and some macros will be replaced for the approved third-party ad servers. If the ad server is not supported in the list, you can still create the ad experience, but no replacements will be done for that particular tag source.

View our [approved ad-server list](https://advertising.amazon.com/resources/ad-policy/approved-3p-ad-servers).