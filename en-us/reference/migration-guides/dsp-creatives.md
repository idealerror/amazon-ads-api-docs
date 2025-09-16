---
title: Amazon DSP creative management
description: Migrate to the latest DSP creative management APIs
type: guide
interface: api
tags:
    - Creatives
    - Amazon DSP
---

# Amazon DSP Creative Management API Migration Guide

## Overview

The [DSP creative management APIs](dsp-ad-creative) enable self-serve advertisers and their partners to programmatically manage their Amazon DSP AdCreatives, AdCreative-AdGroup associations and moderations. These new APIs replace the [previous creative management APIs](dsp-campaigns).

## New API Highlights

* The API provides a single, unified structure (the adCreative) that can host multiple variations (the adExperiences). This simplifies management and enhances the creative process by allowing users to experiment with and optimize multiple renderings or versions of a creative without having to create a new adCreative object each time.
* These APIs use the [Amazon Ads API Common Model](reference/common-models/overview) which provides a standardized model and field names for each entity across ad products.
* These APIs are available in all marketplaces where ADSP operates:
    * NA: BR, CA, MX, US
    * EU: AE, DE, ES, FR, IN, IT, NL, SA, SE, TR, UK
    * FE: AU, JP
* The API supports PATCH operations (partial updates) so you can update individual field without sending all the fields. 
* The list endpoint can support up to 100 objects read at a time. 
* `createCreativeAssociation`, `patchCreativeAssociation` and `deleteCreativeAssociation` support a batch size of 20. 
* The new eligibleCreatives API can list the basic ad creative objects eligible to associate with given ad group.
* The new AdCreative association API can update the association object state (`ACTIVE`, `INACTIVE`). 

## Endpoint Equivalencies

### AdCreative entity

|Old Endpoint	|New Endpoint	|Notes	|
|---	        |---	        |---	|
|GET /dsp/creatives/	|POST /dsp/adCreatives/list	|Read operation will be handled by one LIST endpoint  which returns a list of creative objects based on the supplied filters.	| 
|GET /dsp/creatives/image	| POST /dsp/adCreatives/list | |
|GET /dsp/creatives/video	| POST /dsp/adCreatives/list | | 
|GET /dsp/creatives/rec	| POST /dsp/adCreatives/list | | 
|GET /dsp/creatives/thirdparty	| POST /dsp/adCreatives/list | |
|POST /dsp/creatives/image	| POST /dsp/v1/adCreatives	|One single endpoint will be used to create the `adCreative` object.	|
|POST /dsp/creatives/video	| POST /dsp/v1/adCreatives | |
|POST /dsp/creatives/rec	| POST /dsp/v1/adCreatives | |
|POST /dsp/creatives/thirdparty	| POST /dsp/v1/adCreatives | |
|PUT /dsp/creatives/image	|PATCH /dsp/v1/adCreatives/{adCreativeId}	|One single endpoint will be used to update the adCreative object.	|
|PUT /dsp/creatives/video	|PATCH /dsp/v1/adCreatives/{adCreativeId} | | 
|PUT /dsp/creatives/rec	|PATCH /dsp/v1/adCreatives/{adCreativeId} | |
|PUT /dsp/creatives/thirdparty	|PATCH /dsp/v1/adCreatives/{adCreativeId} | |
|	|GET /dsp/v1/adExperiences	|New endpoint to get Ad Experiences associated to a creative	|  
|	|PUT /dsp/v1/adExperiences	|New endpoint to update Ad Experiences associated to a creative	| 
|	|GET /dsp/adCreatives/{adCreativeId}/validation	|New endpoint to get validation for all Ad Experiences within the adCreative. 	|
|POST /dsp/creatives/image/preview	|POST /dsp/v1/adCreatives/preview	|One single endpoint to preview the adCreative.	|
|POST /dsp/creatives/video/preview	|POST /dsp/v1/adCreatives/preview| |
|POST /dsp/creatives/rec/preview	|POST /dsp/v1/adCreatives/preview| |
|POST /dsp/creatives/thirdparty/preview	|POST /dsp/v1/adCreatives/preview | | 

### AdCreative-AdGroup assocation

|Old Endpoint	|New Endpoint	|
|---	        |---	        | 
|POST /dsp/lineItemCreativeAssociations	|POST /dsp/v1/adCreatives/associations/adGroups	|
|POST /dsp/lineItemCreativeAssociations |POST /dsp/v1/adCreatives/associations/adGroups/delete	|
|PUT /dsp/lineItemCreativeAssociations	|PATCH /dsp/v1/adCreatives/associations/adGroups	|
|GET /dsp/lineItemCreativeAssociations	|POST /dsp/v1/adCreatives/associations/adGroups/list	|
|	|POST /dsp/v1/adCreatives/associations/adGroups/eligibleCreatives/list	|

### Moderation

|Old Endpoint	|New Endpoint	|
|---	        |---	        | 
| GET /dsp/moderation/creatives	|GET /dsp/v1/adCreatives/{adCreativeId}/moderation	|
|	| POST /dsp/v1/adCreatives/associations/adGroups/moderations/list	|

## Entity naming

In the new endpoints, creatives have been renamed to `adCreative`. AdExperience, which represents the rendered view of a creative, is introduced as part of the the `adCreative` object.

## Authorization

These endpoints require an Advertiser header (`Amazon-Ads-AccountId`). This is your Amazon DSP advertiser ID, which can be retrieved from the [advertiser endpoint](dsp-advertiser#tag/Advertiser/paths/~1dsp~1advertisers/get). 

## Update operations

The previous creative and association endpoints used the PUT operation for updates which required a full replacement of all fields. The new endpoints utilize PATCH (partial updates) which means you can send only the fields you wish to update and any fields not included in the request will remain unchanged. 

## Read operations

POST /list operations are added to allow batch operations with filters in the request body. 

## Error model

The error model has been improved. Key changes include:

* Removed `fieldName`, `errorDetails`, and top level `message`.
* Renamed `errorType` to `errorCode`.
* Renamed `message` to `errorMessage`.
* New fields:
    * `success`: This array contains the list of successful operations and their corresponding index.
    * `index`: This is the position of the operation in your original request.
* Added `requestId` to response for successful calls. This should be included when submitting support tickets so that our support team can find the request in our logs.
* If `success` or `error` array is empty, the array will be omitted from the response.

## Creative creation 

This example is a sample request to create legacy Video template:

```
{
        "name": "Video Creative",
        "advertiserId": {{advertiser_id}},
        "marketplace": "US",
        "externalId": "This is a freeflowing external Id",
        "asset": {
            "assetId": "{{asset_id}}",
            "version": "{{asset_version}}"
        },
        "clickThroughAction": {
            "customUrl": {
                "url": "https://www.amazon.com"
            }
        },
        "thirdPartyTrackers": [
            {
                "type": "IMPRESSION",
                "trackerUrl": "https://amazon.com/impressions1"
            }
        ]
    }
```

This example is a sample request to create a `VIDEO` adCreative and set `trackingUrls` to a selected `adExperience`.

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
    }
}
```

This example is a sample request to associate the `STREAMING_TV_VIDEO` adExperience the above video Ad Creative.

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
