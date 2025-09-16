---
title: Getting started with the Amazon DSP Campaign Management API
description: A getting started guide for using the latest Amazon DSP campaign resources 
type: guide
interface: api
tags: 
    - Amazon DSP
    - Campaign management
keywords:
    - guide
---

# Getting started with the Amazon DSP Campaign Management API

## Overview

>[TIP:New campaign management APIs]This document describes campaign management using the product-specific DSP APIs. However, you can now manage campaigns across different ad products using a common model and a single set of API endpoints. To get started using the common campaign model in the Amazon Ads API v1, see the [campaign management overview](guides/campaign-management/overview).

The DSP campaign management APIs enable self-serve advertisers to programmatically manage their Amazon DSP campaign data (campaigns, ad groups, targeting). Use cases include:

* **Track campaigns in your own tools**: Sync campaign metadata with your own data storage solution and surface in a custom UI to track all of your campaigns.
* **Simplify campaign creation**: Build templates or custom workflows that reduce the time it takes to create a new campaign. 
* **Automate campaign optimization**: Automatically adjust bids and budgets in real-time to maximize campaign performance.
* **Targeting**: Target specific inventory and audiences, add domain exclusions, and easily add or remove targets to maximize campaign performance. 

## Amazon DSP API Object Hierarchy

![Amazon DSP API Object Hierarchy](/_images/dsp/adsp-objects.png "Amazon DSP campaign management API object hierarchy")

## Campaign Creation Workflow

The following steps will guide you through creating a live campaign from start to finish.

### Step 1: Create a campaign

In this step, you’ll define your campaign name, optimization settings, flight dates & budgets, frequency caps, and (optionally) budget caps, agency fee, PO number, and comments.

**Example: Create a campaign**

[POST /dsp/v1/campaigns](dsp-ad-group-and-campaign#tag/Campaign/operation/DspBatchPostCampaignV1)

```
{
  "campaigns": [
    {
      "optimization": {
        "bidStrategy": "SPEND_BUDGET_IN_FULL",
        "automateBudgetAllocation": true,
        "goalSetting": {
          "targetKpi": 0.1,
          "kpi": "CLICK_THROUGH_RATE"
        }
      },
      "name": "My Q1 Test Campaign",
      "flights": {
        "flights": [
          {
            "startDateTime": "2024-01-01T00:00:00Z",
            "budgetAmount": 1,
            "endDateTime": "2024-03-31T11:59:59Z"
          }
        ]
      },
      "advertiserId": "520724305968518499",
      "frequencies": [
        {
          "frequencyType": "UNCAPPED"
        }
      ]
    }
  ]
}
```

For the `goalSetting` object, only the `kpi` field is required on create. The value for `goal` is automatically inferred from the `kpi` value and can only be found in the list operation response. Here is the mapping of `goal` to `kpi`:

|Goal (read-only)	|Kpi	|
|---	|---	|
|AWARENESS	|REACH	|
|CONSIDERATION	|CLICK\_THROUGH\_RATE, COST\_PER\_CLICK, COST\_PER\_VIDEO\_COMPLETION, VIDEO\_COMPLETION\_RATE, COST\_PER\_DETAIL\_PAGE\_VIEW, DETAIL\_PAGE\_VIEW\_RATE	|
|CONVERSIONS	|RETURN\_ON\_AD\_SPEND, TOTAL\_RETURN\_ON\_AD\_SPEND, COST\_PER\_ACTION, COMBINED\_RETURN\_ON\_AD\_SPEND, COST\_PER\_SIGN\_UP, COST\_PER\_FIRST\_APP\_OPEN	|

### Step 2: Create an ad group

When creating an ad group, you will define the inventory type, advertised product categories, start and end dates, budget, pacing, optimization settings, bid, frequency caps, creative rotation type, and some targeting settings. Optionally, you may also provide fees, budget caps, a purchase order number, and comments. Advertised product categories refer to the category of products being advertised and can be retrieved from the [advertised product categories discovery API](dsp-discovery-advertised-product-categories). 

>[NOTE] After you create an ad group, certain [default values](#default-behavior) are set. If you do not want these default values applied, remove or update the defaults after ad group creation. 

These inventory types are scheduled for deprecation on June 30, 2025: STANDARD\_DISPLAY, AMAZON\_MOBILE\_DISPLAY, AAP\_MOBILE\_APP, VIDEO. We recommend you do not create new ad groups with these inventoryTypes. Instead, create ad groups with these inventoryTypes: DISPLAY, STREAMING\_TV, ONLINE\_VIDEO.

**Example: Create an ad group**

[POST /dsp/v1/adGroups](dsp-ad-group-and-campaign#tag/Ad-Group/operation/DspBatchPostAdGroup)

```
{
  "adGroups": [
    {
      "inventoryType": "DISPLAY",
      "campaignId": "587673741928966411",
      "creativeRotationType": "RANDOM",
      "advertisedProductCategoryIds": [
        "304669741353807342"
      ],
      "frequencies": [
        {
          "frequencyType": "UNCAPPED"
        }
      ],
      "pacing": {
        "deliveryProfile": "EVEN",
        "catchUpBoostPercentage": 0
      },
      "targetingSettings": {
        "amazonViewability": {
          "viewabilityTier": "ALL_TIERS",
          "includeUnmeasurableImpressions": false
        },
        "timeZoneType": "VIEWER",
        "userLocationSignal": "CURRENT"
      },
      "optimization": {
        "bidStrategy": "SPEND_BUDGET_IN_FULL",
        "automateBudgetAllocation": false
      },
      "name": "Test Display Ad Group",
      "bid": {
        "baseBid": 3
      },
      "startDateTime": "2024-01-01T00:00:00Z",
      "budgetAmount": 1,
      "endDateTime": "2024-01-31T11:59:59Z"
    }
  ]
}
```

### Step 3: Add/remove targeting on the ad group 

A few targeting fields can be found in the `targetingSettings` object in /dsp/v1/adGroups; however, any targeting setting where multiple targets can be applied to an ad group is found in the Universal Targets endpoint, /dsp/v1/targets. The Universal Targets endpoint allows you to add or remove different types of targeting objects across multiple ad groups in a single request. There are separate endpoints for adding and removing targets, so if you need to add and remove targets, you will do this in two API requests.

During the ad group creation workflow, we automatically apply default targets for devices, inventory, day parts, advertiser domain list, and video ad format.

**3a) Example: Add domain and location targeting to an ad group**

[POST /dsp/v1/targets](dsp-universal-targeting#tag/Targets/operation/UtaCreateDspTargets)

```
{
    "targets": [
        {
            "adGroupId": "579602514554384269",
            "adProduct": "AMAZON_DSP",
            "state": "ENABLED",
            "negative": true,
            "targetDetails": {
                "domainTarget": {
                    "domainTargetDetails": {
                        "domainName": {
                            "domainName": "www.nytimes.com",
                            "domainTargetType": "DOMAIN_NAME"
                        }
                    },
                    "targetType": "DOMAIN"
                }
            }
        },
        {
            "adGroupId": "579602514554384269",
            "adProduct": "AMAZON_DSP",
            "state": "ENABLED",
            "negative": false,
            "targetDetails": {
                "locationTarget": {
                    "geoLocation": "XHvCjcKHXmJ7woVowo7CjmvCjcKWbMKHwp3CsGvCk8Krcms=",
                    "targetType": "LOCATION"
                }
            }
        }
    ]
}

```

To retrieve the geoLocation ID, you will use the geoLocations discovery endpoint, /dsp/geoLocations.

**3b) Example: Remove domain and location targeting from an ad group**

[POST /dsp/v1/targets/deleteByObject](dsp-universal-targeting#tag/Targets/operation/UtaDeleteDspTargets)

```
{
    "targets": [
        {
            "adGroupId": "579602514554384269",
            "adProduct": "AMAZON_DSP",
            "state": "ENABLED",
            "negative": true,
            "targetDetails": {
                "domainTarget": {
                    "domainTargetDetails": {
                        "domainName": {
                            "domainName": "www.nytimes.com",
                            "domainTargetType": "DOMAIN_NAME"
                        }
                    },
                    "targetType": "DOMAIN"
                }
            }
        },
        {
            "adGroupId": "579602514554384269",
            "adProduct": "AMAZON_DSP",
            "state": "ENABLED",
            "negative": false,
            "targetDetails": {
                "locationTarget": {
                    "geoLocation": "XHvCjcKHXmJ7woVowo7CjmvCjcKWbMKHwp3CsGvCk8Krcms=",
                    "targetType": "LOCATION"
                }
            }
        }
    ]
}

```

### Step 4: Associate creatives to the ad group

You can use the adCreatives association endpoints to associate both legacy as well as new creatives to ad groups. 

**Example: Associate an ad creative to an ad group**

```
POST /dsp/v1/adCreatives/associations/adGroups

{
  "associations": [
    {
      "adGroupId": "579602514554384269",
      "adCreativeId": "123456789",
      "startDate": "2023-12-12T22:17:26.072Z",
      "endDate": "2023-12-12T22:17:26.072Z",
      "weight": 0,
      "state": "ACTIVE"
    }
  ]
}
```

### Step 5: Enable conversion tracking on the campaign (optional)

On the campaign, if you have chosen a `Kpi` (e.g. `RETURN_ON_AD_SPEND`) that requires tracking products sold on Amazon (ASINs), then you must associate products to the campaign.

**Example: Associate products (ASINs) to a campaign**

[POST /dsp/v1/campaigns/587673741928966411/conversionTracking/products](dsp-ad-group-cvrtracking-products)

```
{
  "productTrackingList": [
    {
      "productId": "B09D36CVP1",
      "productAssociation": "FEATURED_WITH_VARIATION",
      "domain": "AMAZON_US"
    },
    {
      "productId": "B003E1OUKS",
      "productAssociation": "FEATURED",
      "domain": "AMAZON_US"
    },
    {
      "productId": "B08KXDFWBT",
      "productAssociation": "FEATURED",
      "domain": "AMAZON_US"
    },
    {
      "productId": "B099RZF6P9",
      "productAssociation": "FEATURED",
      "domain": "AMAZON_US"
    }
  ]
}

```

On the campaign, if you have chosen a `Kpi` (e.g. `COST_PER_ACTION`) that requires tracking off-Amazon conversions, then you must associate conversion definitions to the campaign.

[POST accounts/{accountId}/dsp/orders/{orderId}/conversionDefinitionAssociations](dsp-conversion-builder#tag/Conversion-Order-Association/operation/dspAmazonUpdateAssociatedConversionDefinitionsForOrder)

Visit the DSP support center for more information on [using the conversions API for off-amazon event tracking](https://advertising.amazon.com/help/GEDE65PCE2CL5P63) and on [conversion API security FAQs](https://advertising.amazon.com/help/G5DUWUKZ9KDG474J).

### Step 6: Add, remove, and/or update campaign flights (optional)

During campaign creation, you may create up to 12 flights in the campaign creation request. If you would like to add, remove, and/or update flights after campaign creation, you can use the flights array in PATCH/dsp/v1/campaigns. The flights array acts as a full replacement of all flights on a campaign. To add, delete, and/or update flights, include the full list of existing flights and only update those you wish to change. 

* To update existing flights, include all fields including flightId. 
* To add new flights, include all fields except flightId. 
* To delete flights, do not include them. Flights with a start date in the past cannot be deleted.
* If you do not want to make any changes to flights, leave the array null.
* Campaigns can have a max of 12 current + future flights. Campaigns can have a max of 150 total flights.

Alternatively, you can also do this using the flights endpoint.

https://advertising.amazon.com/API/docs/en-us/dsp-ad-group-campaign-flights

**Example: Add flights to a campaign - flights endpoint**

[POST /dsp/v1/campaigns/587673741928966411/flights](dsp-ad-group-campaign-flights)

```
{
  "flights": [
    {
      "startDateTime": "2024-04-01T00:00:00.483Z",
      "endDateTime": "2024-06-30T23:59:59.483Z",
      "budgetAmount": 1000
    }
  ]
}
```

### Step 7: Activate the campaign and ad group(s) 

The final step in launching a campaign is to activate both the campaign and ad group(s). Note that activation cannot be done during campaign or ad group creation. These can only be activated with the PATCH operation.

**Example: Activate a campaign**

[PATCH /dsp/v1/campaigns](dsp-ad-group-and-campaign#tag/Campaign/operation/DspBatchPatchCampaign)

```
{
    "campaigns": [
        {
            "campaignId": "587673741928966411",
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
            "adGroupId": "579602514554384269",
            "state": "ACTIVE"
        }
    ]
}
```

## Using audience targeting

The structure of audience targeting has changed from the previous API. The new structure is simplified and less likely to lead to errors during setup. 

New audience structure:

* Up to 500 audiences can be associated per ad group.
* All audiences within the same group must either be included or excluded.
* You can specify up to 10 inclusion groups and 1 exclusion group.
* To add audiences to a group, you must specify a groupId. Only numbers 1 through 11 (formatted as strings) are accepted (e.g., "1").  
* We removed the `intraOperator` and `interOperator` fields from the write operations. Instead, the boolean logic will be set automatically: 
    * Audiences within an inclusion group are joined by OR/ANY logic. Audiences within the exclusion group are joined by AND/ALL logic. This means that users can be in any of the audiences inside a group to be included or excluded.
    * Groups are joined with AND logic. 
* We have future plans to update the ADSP UI to match this simplified workflow. Until then, ad groups can be set up with the old structure in the UI. To enable you to read the old structure, the /dsp/v1/targets/list endpoint has the legacy fields `inGroupOperator` and `acrossGroupOperator`. These fields cannot be included in write operations as the operators are set automatically.

What happens if I try to edit an ad group that does not follow the new structure?

* For line items with more than 10 inclusion and/or 1 exclusion groups, you will receive an error when trying to add new audiences or groups; however, you will be allowed to delete existing audiences and groups.
* For groups that have both inclusion & exclusion audiences, you won't be able to add new audiences to these groups. However, you are allowed to delete these audiences/groups, add new audiences to other groups, and add new groups.

**Example: Add audience targets to an ad group**

[POST /dsp/v1/targets](dsp-universal-targeting#tag/Targets/operation/UtaCreateDspTargets)

```
{
  "targets": [
    {
      "adGroupId": "579602514554384269",
      "adProduct": "AMAZON_DSP",
      "negative": false,
      "state": "ENABLED",
      "targetDetails": {
        "audienceTarget": {
          "audienceId": "365849257058325086",
          "groupId": "1",
          "targetType": "AUDIENCE"
        }
      }
    },
    {
      "adGroupId": "579602514554384269",
      "adProduct": "AMAZON_DSP",
      "negative": false,
      "state": "ENABLED",
      "targetDetails": {
        "audienceTarget": {
          "audienceId": "376964516785083972",
          "groupId": "1",
          "targetType": "AUDIENCE"
        }
      }
    },
    {
      "adGroupId": "579602514554384269",
      "adProduct": "AMAZON_DSP",
      "negative": false,
      "state": "ENABLED",
      "targetDetails": {
        "audienceTarget": {
          "audienceId": "407259404244917362",
          "groupId": "2",
          "targetType": "AUDIENCE"
        }
      }
    },
    {
      "adGroupId": "579602514554384269",
      "adProduct": "AMAZON_DSP",
      "negative": false,
      "state": "ENABLED",
      "targetDetails": {
        "audienceTarget": {
          "audienceId": "403996061702860925",
          "groupId": "2",
          "targetType": "AUDIENCE"
        }
      }
    },
    {
      "adGroupId": "579602514554384269",
      "adProduct": "AMAZON_DSP",
      "negative": true,
      "state": "ENABLED",
      "targetDetails": {
        "audienceTarget": {
          "audienceId": "378970165887759667",
          "groupId": "3",
          "targetType": "AUDIENCE"
        }
      }
    },
    {
      "adGroupId": "579602514554384269",
      "adProduct": "AMAZON_DSP",
      "negative": true,
      "state": "ENABLED",
      "targetDetails": {
        "audienceTarget": {
          "audienceId": "424562951816105407",
          "groupId": "3",
          "targetType": "AUDIENCE"
        }
      }
    }
  ]
}
```

**Example: List audience targets on an ad group**

[POST /dsp/v1/targets/list](dsp-universal-targeting#tag/Targets/operation/UtaListDspTargets)

```
{
  "filters": {
    "targetTypeFilter": [
      "AUDIENCE"
    ],
    "adGroupIdFilter": [
      "579602514554384269"
    ]
  }
}

RESPONSE:{
  "targets": [
    {
      "adGroupId": "579602514554384269",
      "adProduct": "AMAZON_DSP",
      "negative": false,
      "state": "ENABLED",
      "targetDetails": {
        "audienceTarget": {
          "acrossGroupOperator": "ALL",
          "audienceId": "365849257058325086",
          "groupId": "1",
          "inGroupOperator": "ANY",
          "targetType": "AUDIENCE"
        }
      }
    },
    {
      "adGroupId": "579602514554384269",
      "adProduct": "AMAZON_DSP",
      "negative": false,
      "state": "ENABLED",
      "targetDetails": {
        "audienceTarget": {
          "acrossGroupOperator": "ALL",
          "audienceId": "376964516785083972",
          "groupId": "1",
          "inGroupOperator": "ANY",
          "targetType": "AUDIENCE"
        }
      }
    },
    {
      "adGroupId": "579602514554384269",
      "adProduct": "AMAZON_DSP",
      "negative": false,
      "state": "ENABLED",
      "targetDetails": {
        "audienceTarget": {
          "acrossGroupOperator": "ALL",
          "audienceId": "407259404244917362",
          "groupId": "2",
          "inGroupOperator": "ANY",
          "targetType": "AUDIENCE"
        }
      }
    },
    {
      "adGroupId": "579602514554384269",
      "adProduct": "AMAZON_DSP",
      "negative": false,
      "state": "ENABLED",
      "targetDetails": {
        "audienceTarget": {
          "acrossGroupOperator": "ALL",
          "audienceId": "403996061702860925",
          "groupId": "2",
          "inGroupOperator": "ANY",
          "targetType": "AUDIENCE"
        }
      }
    },
    {
      "adGroupId": "579602514554384269",
      "adProduct": "AMAZON_DSP",
      "negative": true,
      "state": "ENABLED",
      "targetDetails": {
        "audienceTarget": {
          "acrossGroupOperator": "ALL",
          "audienceId": "378970165887759667",
          "groupId": "3",
          "inGroupOperator": "ALL",
          "targetType": "AUDIENCE"
        }
      }
    },
    {
      "adGroupId": "579602514554384269",
      "adProduct": "AMAZON_DSP",
      "negative": true,
      "state": "ENABLED",
      "targetDetails": {
        "audienceTarget": {
          "acrossGroupOperator": "ALL",
          "audienceId": "424562951816105407",
          "groupId": "3",
          "inGroupOperator": "ALL",
          "targetType": "AUDIENCE"
        }
      }
    }
  ]
}
```

## Using the discovery APIs

The Discovery APIs are a group of endpoints that list all the object Ids eligible to be applied to a specific ad group. For example, the Inventory - Exchanges endpoint provides a list of inventory exchange Ids that are eligible to be targeted given a set of ad group filters. These endpoints have two purposes: 1) To provide the identifiers, and 2) To tell you which objects are eligible to be applied to an ad group.

**Discovery APIs:**

1. [Advertised Product Categories](dsp-discovery-advertised-product-categories): The category of the products or services being advertised. These categories are sent to publishers and exchanges to determine whether a campaign is eligible to run on a given set of inventory. All ad groups must have at least one advertised product category.
2. [Audiences](audiences#tag/Discovery/operation/listAudiences): Audiences can be Amazon-created, custom built by the user, or created by a third-party data provider.
3. [Domain Lists](dsp-discovery-domain-lists): User-created lists of site domains for targeting. Note that these lists can only be created and edited in the ADSP UI.
4. [IAB Content Categories](dsp-discovery-iab-content): The category of the inventory content as defined by the Interactive Advertising Bureau.
5. [Inventory - Deals](dsp-deals-3p#tag/Discovery/operation/listCMInventoryDiscoveryDeals): Deals are curated groupings of inventory with specific prices and dates. All ad groups must target at least one deal or exchange.
6. [Inventory - Exchanges](dsp-discovery-inventory-source-exchanges): Includes third-party exchanges and SSPs, Amazon owned & operated inventory, and Amazon Publisher Direct. All ad groups must target at least one deal or exchange.
7. [Locations](dsp-discovery-geo): Includes countries, states, postal codes, DMAs, and cities for targeting.
8. [Products](dsp-category-discovery#tag/ContextualTargeting-Product-Discovery/operation/dspContextualTargetingProductOptions): A type of contextual targeting using ASINs.
9. [Product Categories](dsp-category-discovery#tag/ContextualTargeting-ProductCategory-Discovery/operation/dspContextualTargetingProductCategoryOptions): A type of contextual targeting using ASIN categories.
10. [Streaming TV Apps](dsp-discovery-apps): Streaming TV apps used for targeting on video ad groups.
11. [Third-Party Target Segments](dsp-thirdparty-target-segments): Segments from third-party vendors including DoubleVerify and Oracle Data Cloud.

Note: We do not have a discovery endpoint for mobile app identifiers. To retrieve these, you can pull them from the Google Play Store and the Apple App Store.

**Example: List inventory exchange IDs eligible to be targeted by a standard display ad group**

```
POST /dsp/v1/inventorySourceExchanges/list
{
  "filters": [
    {
      "field": "INVENTORY_TYPE",
      "value": "STANDARD_DISPLAY",
      "include": true
    },
    {
      "field": "INVENTORY_SOURCE_TYPE",
      "value": "AMAZON",
      "include": true
    },
    {
      "field": "INVENTORY_SOURCE_TYPE",
      "value": "AMAZON_PUBLISHER_DIRECT",
      "include": true
    },
    {
      "field": "INVENTORY_SOURCE_TYPE",
      "value": "THIRD_PARTY_EXCHANGE",
      "include": true
    }
  ]
}
```

## Default behavior

For some settings, we will set a value for certain fields automatically, usually during the campaign or ad group creation workflow. If you do not want these default values applied, you can remove or update the defaults after ad group creation.

**List of Defaults:**

|Workflow	|API(s) where you can list and update the default values	|Field	|API Default value	|
|---	|---	|---	|---	|
|Campaign & ad group creation	|/dsp/v1/campaigns <br /> /dsp/v1/adGroups	|`state`	|`INACTIVE`	|
|Ad group creation and update	|/dsp/v1/adGroups	|`budgetAmount`	|Varies based on optimization. Users are not allowed to edit budgetAmount when automateBudgetAllocation=true at both the campaign and ad group levels. Instead, it's set automatically by our optimization systems. To edit budgetAmount, change automateBudgetAllocation to false.	|
|Ad group creation	|/dsp/v1/targets	|`dayPartTarget`	|All dayparts are included.	|
|Ad group creation	|/dsp/v1/targets	|`deviceTarget`	|All eligible devices are included.	|
|Ad group creation	|/dsp/v1/targets	|`inventorySourceTarget`	|All eligible inventory sources (excluding deals and inventory groups) are included.	|
|Ad group creation	|/dsp/v1/targets	|`domainTargetDetails.advertiserDomainList`	|`boolean: true`. This does not apply to ad groups where `inventoryType=AAP_MOBILE_APP` because these don't support domain targeting.	|
|Ad group creation	|/dsp/v1/targets	|`videoAdFormatTarget`	|All ad formats. This only applies to ad groups where `inventoryType=VIDEO`.	|




