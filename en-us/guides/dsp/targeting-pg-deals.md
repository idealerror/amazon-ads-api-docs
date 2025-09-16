---
title: Targeting Programmatic Guaranteed Deals
description: Steps to target PG deals on Amazon DSP campaigns.
type: guide
interface: api
tags:
    - Deals
    - Amazon DSP
---

# Targeting Programmatic Guaranteed Deals

Programmatic Guaranteed (PG) deals are pre-negotiated, invitation-only deals where the seller guarantees a fixed volume of impressions at a fixed price. The seller manages targeting and decisioning. When targeting a PG deal, there are specific validation rules that must be followed. Additionally, PG Share of Voice (PG SOV) deals have their own validation rules.

### Steps for targeting PG deals via API

1. Create campaign 
2. Find available PG deal Ids
3. Create ad group targeting PG deal Id


### 1. Create campaign

Create a campaign following the below validation rules.

#### PG validation rules

* flights.startDateTime must be \<=deal startDate. flights.endDateTime must be \>=deal endDate
    - Once the PG deal is targeted, flights.startDateTime and flights.endDateTime are immutable.
* frequencies.frequencyType=UNCAPPED
    - If the deal’s exchangeId=100 (Amazon Publisher Direct) or 1400 (Google AdX) then frequencyType may be set to CUSTOM rather than UNCAPPED.
* optimization.goalSetting.kpi != REACH
* optimization.bidStrategy=PRIORITIZE\_KPI\_TARGET
    - Amazon optimization is disabled for PG deal campaigns, so the goal and bidStrategy chosen will not impact delivery.

**PG SOV validation rules:**

* optimization.bidStrategy=SPEND\_BUDGET\_IN\_FULL


**API:**
[POST/dsp/v1/campaigns](dsp-ad-group-and-campaign#tag/Campaign/operation/DspBatchPostCampaignV1)

**Example API call:**

```
curl -L 'https://advertising-api.amazon.com/dsp/v1/campaigns' \
-H 'Authorization: Bearer {{accessToken}}' \
-H 'Amazon-Ads-AccountId: {{advertiserId}}' \
-H 'Amazon-Advertising-API-ClientId: {{clientId}}' \
-H 'Amazon-Advertising-API-Scope: {{profileId}}' \
-d '{
    "campaigns": [
        {
            "optimization": {
                "bidStrategy": "PRIORITIZE_KPI_TARGET",
                "goalSetting": {
                    "kpi": "VIDEO_COMPLETION_RATE",
                    "targetKpi": 50
                }
            },
            "name": "!New PG Deal campaign",
            "flights": {
                "flights": [
                    {
                        "startDateTime": "2024-08-11T00:00:00Z",
                        "budgetAmount": 10000,
                        "endDateTime": "2025-07-31T23:59:59Z"
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
}'

RESPONSE:

{
    "requestId": "19e44eba-9fa1-40d5-a024-9b4f29996f55",
    "success": [
        {
            "campaignId": "581943349423511954",
            "index": 0
        }
    ]
}
```

### 2. Find available PG deals for targeting

Find deals where dealType=PROGRAMMATIC\_GUARANTEED, and the deal start/end dates are aligned with your campaign start/end dates.

**API:**
[POST/dsp/inventorySourceDeals/list](dsp-deals-3p#tag/Discovery/operation/listCMInventoryDiscoveryDeals)

**Example API call:**

```
curl -L 'https://advertising-api.amazon.com/dsp/inventorySourceDeals/list' \
-H 'Authorization: Bearer {{accessToken}}' \
-H 'Amazon-Ads-AccountId: {{advertiserId}}' \
-H 'Amazon-Advertising-API-ClientId: {{clientId}}' \
-H 'Content-Type: application/vnd.dspinventorysourcedeals.v1+json' \
-H 'Amazon-Advertising-API-Scope: {{profileId}}' \
-d '{
    "filters": [
        {
            "include": true,
            "field": "DEAL_TYPE",
            "values": [
                "PROGRAMMATIC_GUARANTEED"
            ]
        },
        {
            "include": true,
            "field": "START_DATE_TIME",
            "values": "2024-08-11T00:00:00Z"
        },
        {
            "include": true,
            "field": "END_DATE_TIME",
            "values": "2025-07-31T23:59:59Z"
        }
    ]
}'

RESPONSE:
{
    "deals": [
        {
            "id": "584868489873928834",
            "name": "Test PG Deal",
            "inventoryType": "UNKNOWN",
            "dealPrice": {
                "amount": 4600000,
                "currency": "USD",
                "priceType": "FIXED_CPM"
            },
            "dealBudget": {
                "amount": 2806.00000,
                "currency": "USD"
            },
            "dealStatus": "UPCOMING",
            "marketplaceDeal": false,
            "recommendedDeal": false,
            "externalDealId": "aps4a4446e5",
            "dealType": "PROGRAMMATIC_GUARANTEED",
            "publisherName": "Test Publisher",
            "startDateTime": "2024-08-12T04:00:00.000+00:00",
            "endDateTime": "2024-08-17T03:59:59.000+00:00",
            "exchangeId": "100",
            "targetedPgDeal": true
        }
    ]
}
```

Note: For PG SOV deals, priceType will be FLAT\_FEE instead of FIXED\_CPM.

### 3. Create ad group targeting PG deal

Create an ad group following the below validation rules.

#### PG validation rules

* creativeRotationType=RANDOM
* advertisedProductCategoryIds=343050382647447279
* pacing.deliveryProfile=ASAP
* pacing.catchUpBoostPercentage=0
* targetingSettings.videoCompletionTier=ALL_TIERS
* targetingSettings.amazonViewability.viewabilityTier=ALL_TIERS
* targetingSettings.includeUnmeasurableImpressions=false
* targetingSettings.timeZoneType=ADVERTISER_REGION
* targetingSettings.userLocation=EVERYWHERE
* targetingSettings.userLocationSignal=CURRENT
* targetingSettings.targetedPGDealId must include one valid PG deal ID.
* optimization.bidStrategy=PRIORITIZE\_KPI\_TARGET
* bid.baseBid and bid.maxAverageCPM must be set to the PG deal price
* frequencies.frequencyType=UNCAPPED
    - Note: If the deal’s exchangeId=100 (Amazon Publisher Direct) or 1400 (AdX) then frequencyType may be set to CUSTOM rather than UNCAPPED.
* A PG campaign can have multiple ad groups.
* A PG ad group can only target one PG deal.
* A PG deal can be targeted by multiple campaigns and ad groups.

**PG SOV validation rules:**

* A PG SOV campaign can only have one ad group.
* A PG SOV ad group can only target one PG SOV deal.
* A PG SOV deal can only be targeted by one ad group.
* optimization.bidStrategy=SPEND\_BUDGET\_IN\_FULL


**API:**
[POST/dsp/v1/adGroups](dsp-ad-group-and-campaign#tag/Ad-Group/operation/DspBatchPostAdGroupV1)

**Example API call:**

```
curl -L 'https://advertising-api.amazon.com/dsp/v1/adGroups' \
-H 'Authorization: Bearer {{accessToken}}' \
-H 'Amazon-Ads-AccountId: {{advertiserId}} \
-H 'Amazon-Advertising-API-ClientId: {{clientId}}' \
-H 'Amazon-Advertising-API-Scope: {{profileId}} \
-d '{
    "adGroups": [
        {
            "inventoryType": "VIDEO",
            "campaignId": "581943349423511954",
            "creativeRotationType": "RANDOM",
            "advertisedProductCategoryIds": [
                "343050382647447279"
            ],
            "frequencies": [
                {
                    "frequencyType": "UNCAPPED"
                }
            ],
            "pacing": {
                "deliveryProfile": "ASAP",
                "catchUpBoostPercentage": 0
            },
            "targetingSettings": {
                "videoCompletionTier": "ALL_TIERS",
                "amazonViewability": {
                    "viewabilityTier": "ALL_TIERS",
                    "includeUnmeasurableImpressions": false
                },
                "timeZoneType": "ADVERTISER_REGION",
                "userLocation": "EVERYWHERE",
                "userLocationSignal": "CURRENT",
                "targetedPGDealId": "584868489873928834"
            },
            "optimization": {
                "bidStrategy": "PRIORITIZE_KPI_TARGET"
            },
            "name": "Video Line Created via API",
            "bid": {
                "baseBid": 46,
                "maxAverageCPM":46
            },
            "startDateTime": "2024-08-11T00:00:00Z",
            "budgetAmount": 10000,
            "endDateTime": "2025-07-31T23:59:59Z"
        }
    ]
}'

RESPONSE:

{
    "requestId": "19b0a85f-2001-4206-9b30-b45cb4d15abe",
    "success": [
        {
            "adGroupId": "579804111566850507",
            "index": 0
        }
    ]
}
```