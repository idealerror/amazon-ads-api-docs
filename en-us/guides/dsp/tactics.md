---
title: Understanding Amazon DSP Tactics
description: A getting started guide for using the latest Amazon DSP Tactics
type: guide
interface: api
tags: 
    - Amazon DSP
    - Campaign management
    - Amazon DSP Tactics
    - Tactics
keywords:
    - guide
---

# Understanding Amazon DSP tactics: A guide to Performance+ and Brand+ (beta)

## Overview

Amazon DSP's Performance+ and Brand+ tactics are AI-powered advertising solutions that leverage Amazon first-party 
data to optimize campaign performance. These solutions serve both endemic advertisers (selling on-Amazon) and non-endemic advertisers (selling off-Amazon) by automating ad group creation and optimization levers.

Performance+ tactics drive immediate conversions through customer acquisition, remarketing, and retention strategies, while Brand+ tactics identify and engage future customers via a prospecting tactic.

>[NOTE]Brand+ tactics are currently in beta.

## Understanding tactics

Tactics are machine learning-optimized ad groups that deliver enhanced performance through specialized targeting strategies. Amazon DSP offers two primary tactical approaches:

### Designed specifically to drive conversion outcomes

* **Customer acquisition tactic:** Reach shoppers who are similar to past purchasers
* **Remarketing tactic:** Reach shoppers who have viewed a product detail page, searched for your product or visited your homepage 
* **Retention tactic:** Reach shoppers who have purchased your product

### Designed to drive awareness and influence potential customers

* **Prospecting tactic:** Reach consumers who are highly likely to show interest and engage with your brand or product

## Key benefits of using tactics

* Automated campaign setup and optimization
* AI-powered audience targeting
* Pre-configured ML predictive technology aligned with goal objectives
* Transparent performance monitoring
* Control over brand safety, geography, and inventory selection
* Integration with Amazon's first-party signals for better targeting

![Amazon DSP Tactics Step By Step Guide](/_images/tactics/Step-by-Step-guide-Tactics-Workflow.png "Amazon DSP Tactics Step By Step Guide")

## Tactics workflow

The following steps will guide you through creating a live campaign and tactics from start to finish.

![Amazon DSP Tactics Workflow Sequence Diagram](/_images/tactics/Sequence-Diagram-Tactics-Workflow.png "Amazon DSP Tactics Workflow Sequence Diagram")

### Step 1: Campaign creation: Configure tactic-eligible campaign settings

In this step, you’ll define your campaign name, primary inventory types, goals, flight dates & budgets, frequency caps, and (optionally) budget caps, agency fee, PO number, and comments. Before proceeding, review the eligibility criteria outlined in the [Performance+ tactics](https://advertising.amazon.com/help/GJEB6HC73Y8YGXQ9) 
and [Brand+ tactics](https://advertising.amazon.com/help/GEG2QTAPBPH7PHSL) documentation to ensure your campaign meets the necessary requirements.

**Example: Create a tactic eligible campaign**

[POST /dsp/v1/campaigns](dsp-ad-group-and-campaign#tag/Campaign/operation/DspBatchPostCampaignV1)

```
{
    "campaigns": [{
        "optimization": {
            ...
            "primaryInventoryTypes": ["DISPLAY", "VIDEO_OLV", "VIDEO_STV"],
            "goalSetting": {
                "targetKpi": 0.1,
                "kpi": "TOTAL_RETURN_ON_AD_SPEND"
            }
        },
        "name": "My Tactic Campaign",
        "flights": [
            ...
        ],
        "advertiserId": "advertiserId_XXXX",
        ...
    }]
}
```

In order for the campaign to be tactic eligible, the campaign should have appropriate `primaryInventoryTypes` selected.  

`primaryInventoryTypes` is a field that indicates the main types of media the advertiser plans to use in their campaign (Display, Online Video, or Streaming TV). While this field helps Amazon DSP suggest, it is not a restrictive setting. This means that:

* It serves as an indication of the advertiser's primary intended media types
* It is used by Amazon DSP to recommend appropriate tactics
* It does not limit or restrict the actual types of ad groups that can be created within the campaign
* Advertisers can still manually create ad groups with different inventory

>[WARNING] Tactics are only available for Display, Online Video, and Streaming TV inventory types. If you include Audio in your campaign's primaryInventoryTypes setting, no Audio-related tactics will be available when requesting recommendations through the Guidance API in Step 3. This is because tactics functionality is not currently supported for Audio inventory.

Choose appropriate goal `kpi` in the campaign, to request relevant tactics. 

![Amazon DSP Tactics To Goal Table](/_images/tactics/ApiEnum_UITactic_Goal.png "Amazon DSP Tactics To Goal Table")

### Step 2.a: [Endemic Campaigns] Enable conversion tracking on the campaign

When selecting products for your campaign, you must designate at least one ASIN as "featured." These featured ASINs are essential as their performance data powers the ML algorithms that drive targeting. This enables Amazon DSP to deliver optimized targeting for your tactics.

>[NOTE] **Featured ASIN**: A specific product that is promoted in creatives within the order. Tracking non-featured ASINs or variations, in addition to featured and parent ASINs, ensures proper conversion metrics and prevents data loss that isn't recoverable once a campaign starts.

**Example: Associate products (ASINs) to a campaign**

[POST /dsp/v1/campaigns/{CAMPAIGN_ID}/conversionTracking/products](dsp-ad-group-cvrtracking-products#tag/Product-Conversion-Tracking/operation/DspPostProductConversionTrackingV1)

```
{
    "productTrackingList": [
        {
            "productId": "productId_XXX",
            "productAssociation": "FEATURED",
            "domain": "AMAZON_US"
        },
        {
            "productId": "productId_XXX",
            "productAssociation": "FEATURED_WITH_VARIATION",
            "domain": "AMAZON_US"
        }
    ]
}
```

### Step 2.b: [Non-Endemic Campaigns] Enable off-Amazon conversions on the campaign

For non-endemic advertisers, tactics have specific conversion event requirements. The campaign requires a designated optimized conversion event. Please refer the [Conversions API documentation](https://advertising.amazon.com/help/GEDE65PCE2CL5P63) to learn more about how to set up conversion events for your application. 

**Example: Associate off-Amazon conversions to a campaign**

[POST accounts/{accountId}/dsp/orders/{orderId}/conversionDefinitionAssociations](dsp-conversion-builder#tag/Conversion-Order-Association/operation/dspAmazonUpdateAssociatedConversionDefinitionsForOrder)

```
[{
    "isOptimized": true, 
    "conversionDefinitionId": "conversionDefinitionId_XXXX",
    "operation": "ADD"
}]
```

>[WARNING] Campaign eligibility for tactics is determined by both on- and off-Amazon conversion events. If these tactics aren't available, it may be due to conversion events failing privacy and safety checks.

### Step 3: Tactic Selection: Access available tactics for your campaign

Based on both on- and off-Amazon conversion events, and goal outcomes, the users must call the [DSP Guidance APIs](dsp-guidance) to retrieve Amazon-recommended ad groups. The [List Campaign Guidance API](dsp-guidance#tag/Guidance/operation/listCampaignGuidanceV1) responds with a list of recommendations and action IDs. 
Action IDs are used to create recommended tactics.

**Example: Retrieve all available tactics for a campaign**

[POST /dsp/v1/guidance/campaigns/list](dsp-guidance#tag/Guidance/operation/listCampaignGuidanceV1)

```
{
  "filters": {
    "guidanceNames": [
      "AMAZON_RECOMMENDED_TACTICS_GUIDANCE"
    ],
    "includeRecommendations": "true"
  },
  "campaignIds": [
    "CAMPAIGN_ID_XXX"
  ]
}
```

Record the `actionIds` from the response. These `actionIds` will be required for creating tactics in subsequent steps. The recommendations are based on your campaign settings and eligibility criteria.

### Step 4: Ad group creation: Set up tactics

Using the `actionIds` obtained from Step 3, you can create tactic ad groups to implement the recommended targeting strategies via the [DSP Quick Actions APIs](guides/dsp/guidance-and-quick-actions).

**Example: Start execution of multiple quick actions**

[POST /dsp/v1/quickactions/batchCreateExecutions](dsp-quick-actions#tag/DSP-Quick-Actions/operation/batchCreateExecutionsV1)

```
[
    "action_id_XXXXX",
    "action_id_YYYYY"
]
```

In order to get details on quick actions, please refer [DSP Quick Actions APIs](guides/dsp/guidance-and-quick-actions) on how you can poll execution status of multiple actions.

You can also view & modify all ad groups (tactic & non-tactic ad groups) via [DSP Campaign Management Adgroup APIs](dsp-ad-group-and-campaign#tag/Ad-Group)

**Example: Fetch all ad groups under a campaign**

[POST /dsp/v1/adGroups/list](dsp-ad-group-and-campaign#tag/Campaign/operation/DspListCampaignV1)

```
{
  "campaignIdFilter": [
    "campaignID-XXXX"
  ]
}
```

This will give you all the ad groups present under the campaign. The tactics have the following properties correctly set under an ad group: 

- `inventoryType` : Based on the primaryInventoryTypes selected at campaign level, the inventoryType  is set. This value determines what inventory to display creatives on.
- `automatedTargetingTactic` : Representing the targeting strategy for ad group which can be employed to drive better outcomes.
- `tacticsConvertersExclusionType`: Represents the exclusion criteria applied to purchasers. Customer acquisition and remarketing tactics will exclude past converters by default. The exclusion lookback periods vary by tactic and conversion type, to know more about the look back window refer to [audience exclusions by tactic type](https://advertising.amazon.com/help/GJEB6HC73Y8YGXQ9).

If you wish to edit the `tacticsConvertersExclusionType`, you can use the [PATCH update ad group API](dsp-ad-group-and-campaign#tag/Ad-Group/operation/DspBatchPatchAdGroupV1) to reset it as per your own custom requirements.

For example, running a `Remarketing - Display` recommendation creates a tactic with: 

* `name`: `Remarketing - Display`
* `automatedTargetingTactic`: `REMARKETING`
* `tacticsConvertersExclusionType`: `RECENT_PURCHASERS`

```
{
    "adGroupId": "adGroupId-XXX",
    "name": "Remarketing - Display"
    ...
    "inventoryType": "DISPLAY",
    "targettingSettings": {
        ...
        "automatedTargetingTactic": "REMARKETING",
        "tacticsConvertersExclusionType": "RECENT_PURCHASERS"
    
    }
}
```

>[WARNING] You cannot edit the tactic `automatedTargetingTactic` after the ad group is created. The targeting tactic is an immutable field which only be set while creating tactic ad groups via DSP Quick Actions API.

### Step 5: (optional) Edit tactic settings  

While advertisers can manage targeting settings (locations, audiences, etc.) including creating, modifying, and removing different types of targets via the [Universal Targeting APIs](dsp-universal-targeting#tag/Targets/operation/UtaCreateDspTargets), there are certain guardrails to keep in mind for tactics:

#### [Inventory Selection](dsp-discovery-inventory-source-exchanges)

* Include at least one third-party inventory source for Online Video or Display
* Options include Amazon Publisher Direct, 3P Exchanges, or non-guaranteed deals

#### [Contextual Targeting](dsp-category-discovery)

* Users cannot add any contextual targeting like “keywords”, “product categories” for tactics

#### [Audience Management](dsp-universal-targeting#tag/Targets/operation/UtaCreateDspTargets)

* Can add audience suppression lists for first-party advertiser audiences
* Cannot suppress In-Market or Lifestyle audience segments

**Example: Add excluded audience targets to an ad group**

[POST /dsp/v1/targets](dsp-universal-targeting#tag/Targets/operation/UtaCreateDspTargets)

```
{ 
    "targets": [
        {
            "adGroupId": "XXX",
            "adProduct": "AMAZON_DSP",
            "negative": true, 
            "state": "ENABLED",
            "targetDetails": {
                "audienceTarget": {
                    "audienceId": "XXX", 
                    "groupId": "1",
                    "targetType": "AUDIENCE"
                }
            }
        }
    ]
}
```

### Step 6: Creative association: Link appropriate creative assets

You can use the [Amazon DSP Creatives association](guides/dsp/creative-associations) endpoints to associate creatives to tactics.

**Example: Associate an ad creative to an ad group**

[POST /dsp/v1/adCreatives/associations/adGroups](guides/dsp/creative-associations#sample-request-create-adcreative-association-with-adgroup)

```
{
    "associations": [{
        "adGroupId": "tacticAdGroupId_XXX",
        "adCreativeId": "adCreativeID_XXX",
        "startDate": "2023-12-12T22:17:26.072Z",
        "endDate": "2023-12-12T22:17:26.072Z",
        "weight": 0,
        "state": "ACTIVE"
    }]
}
```

### Step 7: Activate campaign & tactics

The final step in launching a campaign is to activate both the campaign and ad group(s). Note that activation cannot be done during campaign or ad group creation. These can only be activated with the PATCH operation.

**Example: Activate a campaign**

[PATCH /dsp/v1/campaigns](dsp-ad-group-and-campaign#tag/Campaign/operation/DspBatchPatchCampaignV1)

```
{
    "campaigns": [
        {
            "campaignId": "campaignId_XXX",
            "state": "ACTIVE"
        }
    ]
}
```

**Example: Activate an ad group**

[PATCH /dsp/v1/adGroups](dsp-ad-group-and-campaign#tag/Ad-Group/operation/DspBatchPatchAdGroup)

```
{
    "adGroups": [
        {
            "adGroupId": "adGroupId_XXX",
            "state": "ACTIVE"
        }
    ]
}  
```
