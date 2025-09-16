---
title:  Amazon DSP creative associations
description: A guide for using the latest Amazon DSP creative association APIs
type: guide
interface: api
tags: 
    - Amazon DSP
    - Creatives
keywords:
    - guide
---

# Amazon DSP creative associations

The Amazon DSP Creative Association Management API is a set of API endpoints that allow developers to programmatically manage their Amazon DSP creative association data.

Use cases include:

* Viewing association eligibility of ad creatives
* Associating ad creatives with ad groups
* Updating configurations of ad creative associations
* Deleting ad creative associations
* Viewing the moderation results of ad creative association

## Amazon DSP API Ad Creative Association Object Hierarchy

![Image: Developer Guide - Ad Creative Association API](/_images/dsp/dsp-creative-association-objects.png)

## Ad Creative Association Creation Workflow

The following steps guide you through reviewing the association eligibility of an ad creative and creating an ad creative association.

* To create an ad group, review [our getting started with Amazon DSP campaign managment guide](guides/dsp/developer-guide).
* To create an ad creative, please review the [developer guidance](guides/dsp/creative-management) of ad creative creation API.

### Step 1: Reviewing the association eligibility of ad creatives

In this step, you will use a list of ad group ids and get a list of ad creatives which are eligible to associate with some given ad groups.

#### Sample request: Get eligible ad creatives

[POST /dsp/v1/adCreatives/associations/adGroups/eligibleCreatives/list](dsp-ad-creative-association#tag/DSP-Ad-Creative-Association/operation/listEligibleCreativesV1)

```json
{
  "maxResults": "${desired count of the result in one page}",
  "nextToken": "${nextToken}",
  "adGroupIdFilter": {
    "include": [
      "${adGroupId1}", "${adGroupId2}"
    ]
  },
  "includeExtendedDataFields": false
}
```

Only the `adGroupIdFilter` field is required. You can add additional filter of:
* ad creative id
* a creative name
* ad creative format type (it also supports legacy creative template name in this filter). The concept of format type can be found in the [Ad Creative API Developer Guidance](dsp/creative-management).
* ad creative moderation status

Each filter except `adCreativeModerationStatusFilter` supports up to 20 items. Fuzzy search is supported in `adCreativeNameFilter` and `adCreativeFormatTypeFilter`.
`nextToken` is used to fetch the next page. It will be returned in case your response has multiple pages. 

#### Sample request: Get eligible ad creatives with additional filters

[POST /dsp/v1/adCreatives/associations/adGroups/eligibleCreatives/list](dsp-ad-creative-association#tag/DSP-Ad-Creative-Association/operation/listEligibleCreativesV1)

```json
{
  "maxResults": "${desired count of the result in one page}",
  "nextToken": "${nextToken}",
  "adGroupIdFilter": {
    "include": [
      "${adGroupId1}", "${adGroupId2}", "${adGroupId3}"
    ]
  },
  "adCreativeIdFilter": {
    "include": [
      "${adCreativeId1}", "${adCreativeId2}"
    ]
  },
  "adCreativeNameFilter": {
    "include": [
      "${adCreativeName1}", "*${adCreativeName2}*"
    ]
  },
  "adCreativeFormatTypeFilter": {
    "include": [
      "${adCreativeFormatType1}", "*${adCreativeFormatType2}*", "${lagecyCreativeTemplateName}"
    ]
  },
  "adCreativeModerationStatusFilter": {
    "include": [
      "${status1}", "${status2}"
    ]
  }
}
```

#### Sample response: Get eligible ad creatives

```json
{
  "totalResults": "${total count of the result in all pages}",
  "nextToken": "${nextToken}",
  "adCreatives": [{
    "adCreativeName": "string",
    "adCreativeId": "string",
    "eligibleAdGroupIds": ["${adGroupId1}", "${adGroupId2}"]
  }]
}
```

Some ad creatives may not be eligible with all the ad groups provided in the request. The `eligibleAdGroupIds` field shows you which ad group IDs are eligible for each adCreative to associate with. 

To request more data fields of ad creative, you can enable `includeExtendedDataFields`.

#### Sample request: Get eligible ad creatives with extended data fields

[POST /dsp/v1/adCreatives/associations/adGroups/eligibleCreatives/list](dsp-ad-creative-association#tag/DSP-Ad-Creative-Association/operation/listEligibleCreativesV1)

```json
{
    "maxResults": "${desired count of the result in one page}",
    "nextToken": "${nextToken}",
    "adGroupIdFilter": {
        "include": [
            "${adGroupId1}", "${adGroupId2}"
        ]
    },
    "includeExtendedDataFields": true
}
```

#### Sample response: Get eligible ad creatives with extended data fields

```json
{
    "totalResults": "${total count of the result in all pages}",
    "nextToken": "${nextToken}",
    "adCreatives": [{
        "adCreativeName": "string",
        "adCreativeId": "string",
        "eligibleAdGroupIds": ["${adGroupId1}", "${adGroupId2}"],
        "extendedData": {
            "createdDate": "2021-01-09T01:22:00.000Z",
            "lastUpdatedDate": "2021-01-09T01:22:00.000Z",
            "language": "en",
            "country": "US",
            "adCreativeModerationStatus": "APPROVED",
            "adCreativeFormatProperties": {
                "adCreativeFormatType": "DISPLAY",
                "creativeSizes": [{
                    "width": 0,
                    "height": 0,
                    "responsive": true
                }]
            }
        }
    }]
}
```

### Step 2: Create an association between an ad creative and an ad group

In this step, you will use the `adGroupId` and `adCreativeId` received in the [step one](dsp/creative-associations#step-1-reviewing-the-association-eligibility-of-ad-creatives) to create an association between them.

#### Sample request: Create adCreative association with adGroup

[POST /dsp/v1/adCreatives/associations/adGroups](dsp-ad-creative-association#tag/DSP-Ad-Creative-Association/operation/createCreativeAssociationsV1)

```json
{
  "associations": [{
    "adCreativeId": "${adCreativeId}",
    "weight": "${weight}",
    "state": "ACTIVE",
    "adGroupId": "${adGroupId}",
    "startDate": "2021-01-09T01:22:00.000Z",
    "endDate": "2021-01-09T01:22:00.000Z"
  }]
}
```

This is a batch endpoint which can support up to 20 associations at a time.

#### Sample response: Create adCreative association with adGroup

```json
{
  "success": [
    {
      "index": 0,
      "adCreativeId": "${adCreativeId}",
      "adGroupId": "${adGroupId}"
    }
  ],
  "error": [
    {
      "index": 0,
      "code": "${errorCode}",
      "adCreativeId": "${adCreativeId}",
      "description": "${actionable description of the error}",
      "adGroupId": "${adGroupId}"
    }
  ]
}
```

You can find all successful associations in the `success` list and all unsuccessful associations in the `error` list.

`index` indicates the original position of this association in your requested `associations` list.

## Ad Creative Association Management Workflow

The following steps guide you through:

* Reviewing the moderation results of ad creative associations
* Updating an ad creative associations configuration
* Deleting an ad creative association

### Step 1: Review the moderation result of an ad creative association

In the step, you will use `adGroupId` and `adCreativeId` to fetch the moderation results of associations. Moderation results contain feedback from our Amazon DSP policy guidelines. You can then respond by updating or deleting the association.

#### Example request: Review adCreative association moderation result

[POST /dsp/v1/adCreatives/associations/adGroups/moderations/list](dsp-ad-creative-association#tag/DSP-Ad-Creative-Association-Moderation/operation/listCreativeAssociationModerationsV1)

```json
{
  "maxResults": "${desired count of the result in one page}",
  "nextToken": "${nextToken}",
  "adGroupIdFilter": {
    "include": [
      "${adGroupId}"
    ]
  },
  "adCreativeIdFilter": {
    "include": [
      "${adCreativeId}"
    ]
  }
}
```

You can provide an `adGroupIdFilter`, and `adCreativeIdFilter`, or both. If you provide both, this endpoint will return only association in which BOTH the `adGroupId` **AND** the `adCreativeId` are present. Each filter supports up to 20 items.

#### Example response: Review adCreative association moderation results

```json
{
  "totalResults": "${total count of the result in all pages}",
  "nextToken": "${nextToken}",
  "associationStatuses": [{
    "adCreativeId": "${adCreativeId}",
    "adGroupId": "${adGroupId}",
    "adExperiences": [{
      "adExperience": "{adExperience name}",
      "associationModerationStatus": "${moderation status}",
      "associationModerationReasons": [
        "{feedbacks from Amazon DSP policy guidelines}"
      ]
    }]
  }]
}
```

>[NOTE] An association moderation scan is performed on each ad experience after the ad creative is associated with an ad group. Not all ad experiences under the same ad creative are compatible with the ad group. Normally, some ad creatives are approved, and some ad creatives are not approved.

>[NOTE] For creatives created using the [POST /dsp/v1/adCreatives](dsp-ad-creative#tag/DSP-Ad-Creative/operation/createAdCreativeV1) API, the ad experience will be the name of ad experience associated by the [PUT /dsp/v1/adExperiences](dsp-ad-creative#tag/DSP-Ad-Experience/operation/updateAdExperiencesV1) API. For legacy creatives, the ad experience is the creative template name in uppercase. For example: THIRD_PARTY_VIDEO.

>[NOTE] When `associationModerationStatus` is NOT_APPROVED, `associationModerationReasons` will be present.

>[NOTE] If ad creative isn’t approved, `associationModerationStatus` will be NOT_APPROVED as well.

### Step 2: Update an ad creative association

Update an existing association's configuration. 

#### Example request: Update an ad creative association

[PATCH /dsp/v1/adCreatives/associations/adGroups](dsp-ad-creative-association#tag/DSP-Ad-Creative-Association/operation/patchCreativeAssociationsV1)

```json
{
  "associations": [{
    "adCreativeId": "${adCreativeId}",
    "weight": "${weight}",
    "state": "INACTIVE",
    "adGroupId": "${adGroupId}",
    "startDate": "2021-01-09T01:22:00.000Z",
    "endDate": "2021-01-09T01:22:00.000Z"
  }]
}
```

This is a batch endpoint that can support up to 20 associations at a time.

#### Example response: Update an ad Creative association

```json
{
  "success": [{
    "index": 0,
    "adCreativeId": "${adCreativeId}",
    "adGroupId": "${adGroupId}"
  }],
  "error": [{
    "index": 0,
    "code": "${errorCode}",
    "adCreativeId": "${adCreativeId}",
    "description": "${actionable description of the error}",
    "adGroupId": "${adGroupId}"
  }]
}
```

>[NOTE] If an ad creative association’s state is set to INACTIVE and then set to ACTIVE later, it won’t trigger a new moderation scan.

### Step 3: Delete an ad creative association

Delete an existing ad creative association.

#### Example request: Delete an ad creative association

[POST /dsp/v1/adCreatives/associations/adGroups/delete](dsp-ad-creative-association#tag/DSP-Ad-Creative-Association/operation/deleteCreativeAssociationsV1)

```json
{
  "associations": [{
    "adCreativeId": "${adCreativeId}",
    "adGroupId": "${adGroupId}"
  }]
}
```

This is a batch endpoint that can support up to 20 associations at a time.

#### Example response: Delete an ad creative association

```json
{
  "success": [{
    "index": 0,
    "adCreativeId": "${adCreativeId}",
    "adGroupId": "${adGroupId}"
  }],
  "error": [{
    "index": 0,
    "code": "${errorCode}",
    "adCreativeId": "${adCreativeId}",
    "description": "${actionable description of the error}",
    "adGroupId": "${adGroupId}"
  }]
}
```

## Retrieve all of your ad creative associations

Retrieve all creative associations for a given advertiser. You can filter by `adCreativeId` and `adGroupId`. Each filter supports up to 20 items.

#### Example request: Review ad creative associations

[POST dsp/v1/adCreatives/associations/adGroups/list](dsp-ad-creative-association#tag/DSP-Ad-Creative-Association/operation/listCreativeAssociationsV1)

```json
{
  "maxResults": "${desired count of the result in one page}",
  "nextToken": "${nextToken}",
  "adGroupIdFilter": {
    "include": [
      "${adGroupId}"
    ]
  }
}
```

#### Example response: Review ad creative associations

```json
{
  "totalResults": "${total count of the result in all pages}",
  "nextToken": "${nextToken}",
  "associations": [{
    "adCreativeId": "${adCreativeId}",
    "adGroupId": "${adGroupId}",
    "creativeAssociationStatus": "${overall moderation status}",
    "weight": 0,
    "state": "ACTIVE",
    "startDate": "2021-01-09T01:22:00.000Z",
    "endDate": "2021-01-09T01:22:00.000Z",
    "adExperiences": [{
      "adExperience": "{adExperience name}",
      "associationModerationStatus": "${moderation status}"
    }]
  }]
}
```

To request more data fields of ad creative, you can enable `includeExtendedDataFields`.

#### Example request: Review ad creative associations with extended data fields

[POST dsp/v1/adCreatives/associations/adGroups/list](dsp-ad-creative-association#tag/DSP-Ad-Creative-Association/operation/listCreativeAssociationsV1)

```json
{
    "maxResults": "${desired count of the result in one page}",
    "nextToken": "${nextToken}",
    "adGroupIdFilter": {
        "include": [
            "${adGroupId1}", "${adGroupId2}"
        ]
    },
    "includeExtendedDataFields": true
}
```

#### Example response: Review ad creative associations with extended data fields

```json
{
    "totalResults": "${total count of the result in all pages}",
    "nextToken": "${nextToken}",
    "associations": [{
        "adCreativeId": "${adCreativeId}",
        "adGroupId": "${adGroupId}",
        "creativeAssociationStatus": "${overall moderation status}",
        "weight": 0,
        "state": "ACTIVE",
        "startDate": "2021-01-09T01:22:00.000Z",
        "endDate": "2021-01-09T01:22:00.000Z",
        "adExperiences": [{
            "adExperience": "{adExperience name}",
            "associationModerationStatus": "${moderation status}"
        }],
        "extendedData": {
            "adCreative": {
                "createdDate": "2021-01-09T01:22:00.000Z",
                "lastUpdatedDate": "2021-01-09T01:22:00.000Z",
                "language": "en",
                "country": "US",
                "adCreativeModerationStatus": "APPROVED",
                "adCreativeFormatProperties": {
                    "adCreativeFormatType": "DISPLAY",
                    "creativeSizes": [{
                        "width": 0,
                        "height": 0,
                        "responsive": true
                    }]
                }
            }
        }
    }]
}
```

>[NOTE] If startDate and endDate were not overwritten by [POST /dsp/v1/adCreatives/associations/adGroups](dsp-ad-creative-association#tag/DSP-Ad-Creative-Association/operation/createCreativeAssociationsV1) or [PATCH /dsp/v1/adCreatives/associations/adGroups](dsp-ad-creative-association#tag/DSP-Ad-Creative-Association/operation/patchCreativeAssociationsV1) API, they will be null in the response.

>[NOTE] `creativeAssociationStatus` is an overall status which takes association moderation result of each ad experience into consideration.

## Default behavior

The Amazon DSP API sets values for certain fields automatically if no value is provided in the request.

|Workflow	|API where you can list and update the default values	|Field	|API Default value	|
|---	|---	|---	|---	|
|Create an ad creative association	|[/dsp/v1/adCreatives/associations/adGroups/eligibleCreatives/list](dsp-ad-creative-association#tag/DSP-Ad-Creative-Association/operation/listEligibleCreativesV1)	|maxResult	|100	|
|Create an ad creative association	|[/dsp/v1/adCreatives/associations/adGroups](dsp-ad-creative-association#tag/DSP-Ad-Creative-Association/operation/createCreativeAssociationsV1)	|state	|ACTIVE	|
|Manage the ad creative association	|[/dsp/v1/adCreatives/associations/adGroups/moderations/list](dsp-ad-creative-association#tag/DSP-Ad-Creative-Association-Moderation/operation/listCreativeAssociationModerationsV1)	|maxResult	|100	|
|Review ad creative association	|[dsp/v1/adCreatives/associations/adGroups/list](dsp-ad-creative-association#tag/DSP-Ad-Creative-Association/operation/listCreativeAssociationsV1)	|maxResult	|100	|

## API Rule

This section list out key validation rules in each API.

### Rules of createCreativeAssociationsV1 API

| Field	        | Optional	 | Rule	                                                                                   |
|---------------|-----------|-----------------------------------------------------------------------------------------|
| adGroupId	    | No	       | Ad group must exist.	                                                                   |
| adCreativeId	 | No	       | Ad creative must exist.	                                                                |
| weight	       | Yes	      | If can't be set if rotationType in adGroup is random. Accepted value is [1, 999]	       |
| state	        | Yes	      | Accepted values are ACTIVE or INACTIVE	                                                 |
| startDate	    | Yes	      | It needs to be later then adGroup's startDate and earlier than endDate of association.	 |
| endDate	      | Yes	      | It needs to be earlier then adGroup's endDate and later than startDate of association.	 |

Ad creative types and ad group types need to be compatible. Compatibility rules are as follows:

|AdGroup Type	| AdCreative Type	                                                                                                                                                                   |
|---	|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|Standard Display	| Responsive eCommerce, Image, Third party, Standard Display	                                                                                                                        |
|Amazon Mobile Display	| Responsive eCommerce, Image - mobile O&O, Standard Display	                                                                                                                        |
|AAP Mobile App	| Responsive eCommerce, Image - mobile AAP, Third party - mobile AAP, Fire Tablet - Amazon Video Ads	                                                                                |
|Display	| Responsive eCommerce, Image, Image - mobile AAP, Image - mobile O&O, Third party, Third party - mobile AAP, Fire Tablet - Amazon Video Ads, Image - Fire Tablet, Standard Display	 |
|Video	| Legacy Video, Streaming TV, Online Video, Third party - video, Video Creative Builder	                                                                                             |
|Streaming TV	| Streaming TV	                                                                                                                                                                      |
|Online Video	| Online Video	                                                                                                                                                                      |
|Audio	| Auido,  Audio - Interactive audio ads	                                                                                                                                             |
|Podcast	| Audio - Podcast	                                                                                                                                                                   |

There are specific rules per ad experience.
#### Standard Display
Ad group with amazon mobile web targeting requires associated creative to have 2x resolution asset. `Standard Display` ad experience without 2x resolution image can’t be associated with `Amazon Mobile Display` ad group. It will result in 0 impression when associating with Display ad group which only has amazon mobile web targeting. 

#### Audio
`Audio` creative can only be associated with the `Audio` ad group.
If the ad group targets Twitch supply (Twitch video mobile app, Twitch video mobile web, APS video), the `Audio` creative must have a video asset, and the duration must be within 12s - 15s, or 27s - 30s.

#### Audio - Interactive audio ads
`Audio - Interactive` audio ads creative can only be associated with the `Audio` ad group.
It requires ad group to target connected devices. If you are using a deal, deal needs to include connected devices. For example `Audio - audience guaranteed - interactive creative` deal type.
If ad group targets only Amazon Music Free inventory or Third Party inventory, it requires at least one of following publisher not in the block list:
* IHeartRadio App
* Federated Alexa
* Salem
* Prisa
* Estrella
* Chavez
* Entravision
* Beasley

If `Audio - Interactive` audio ads creative has `Remind Me` choose as the call to action, it needs to meet following rules
* For `absolute time` reminder type, the reminder date must occur after the ad group end date.
* For `weekly recurring time` reminder type, there must be at least 1 reminder date occurring after the ad group end date.

### Rules of patchCreativeAssociationsV1 API

| Field	        | Optional	 | Rule	                                                                                   |
|---------------|-----------|-----------------------------------------------------------------------------------------|
| adGroupId	    | No	       | One association between given ad group and creative must exist.                          |
| adCreativeId	 | No	       | One association between given ad group and creative must exist.                             |
| weight	       | Yes	      | If can't be set if rotationType in adGroup is random. Accepted value is [1, 999]	       |
| state	        | Yes	      | Accepted values are ACTIVE or INACTIVE	                                                 |
| startDate	    | Yes	      | It needs to be later then adGroup's startDate and earlier than endDate of association.	 |
| endDate	      | Yes	      | It needs to be earlier then adGroup's endDate and later than startDate of association.	 |

### Rules of deleteCreativeAssociationsV1 API

| Field	        | Optional	 | Rule	                                                           |
|---------------|-----------|-----------------------------------------------------------------|
| adGroupId	    | No	       | One association between given ad group and creative must exist. |
| adCreativeId	 | No	       | One association between given ad group and creative must exist.  |

### Rules of listEligibleCreativesV1 API
| Field	        | Optional	 | Rule	                                                                                   |
|---------------|-----------|-----------------------------------------------------------------------------------------|
| adGroupId	    | No	       | Ad group must exist.     | 

Creatives returned by this API comply with rules in createCreativeAssociationsV1 API.