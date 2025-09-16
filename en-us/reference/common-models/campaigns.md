---
title: Amazon Ads API campaigns model overview
description: Describes the campaign model and associated fields for the Amazon Ads API.
type: guide
interface: api
tags:
    - Operations
---

# Campaigns

**Campaigns** are used to group ads that share a budget, objective, and optimization strategy. Campaigns can be one of a number of different ad products, including Sponsored Products, Sponsored Brands, Sponsored Display, and Amazon DSP.

## Schema

Campaigns contains the following fields. **Read-only** indicates that the field is part of the model, but cannot be modified by advertisers. **Required** indicates that a field will always appear in the model. 

>[NOTE] Some campaign fields are only available for certain ad products. For details, see the [Ad product mapping table](#ad-product-mapping).

|Common field	|Type	|Required	|Read-only	|Description	|	
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|campaignId	|String	|TRUE	|TRUE	|The unique numerical ID associated with a campaign.	|	
|adProduct	| [Enum](reference/common-models/enums#adproduct)	|	|	|The ad product associated with the campaign.	|
|portfolioId	|String	|	|	|Optional. Shows the portfolio ID associated with a campaign.	|
|name	|String	|TRUE	|	|The advertiser-specified name of the campaign.	|	
|startDate	|String	|	|	|The start date of the campaign.	|	
|endDate	|String	|	|	|The end date of the campaign.	|	
|lastUpdatedDateTime	|String	|TRUE	|TRUE	|The date time that the campaign was las updated.	|	
|creationDateTime	|String	|TRUE	|TRUE	|The date time that the campaign was created.	|
|state	|[Enum](reference/common-models/enums#state)	|TRUE	|	|The state of the campaign.	|	
|deliveryStatus	|[Enum](reference/common-models/enums#deliverystatus)	|TRUE	|TRUE	|This is an overall status if the campaign is delivering or not.	|	
|deliveryReasons	|[Enum](reference/common-models/enums#deliveryreasons)	|	|TRUE	|This is a list of reasons why the campaign is not delivering and the reasons behind the delivery status.	|
|brandEntityId	|String	|	|	|This is the brand entity ID that the campaign is associated with. Required for sellers creating Sponsored Brands campaigns.	|
|optimization.<br />bidStrategy	|[Enum](reference/common-models/enums#bidstrategy)	|	|	|The auto bidding strategy applied to to the campaign.	|	
|optimization.<br />placementBidAdjustments.<br />placement	|[Enum](reference/common-models/enums#placement)	|	|	|The selection of the placement associated with bid adjustment settings.	|
|optimization.<br />placementBidAdjustments.<br />percentage	|Integer	|	|	|The selection of the percentage change associated with a given placement and bid adjustment settings.	|
|optimization.<br />shopperSegmentBidAdjustment.<br />shopperSegment	|[Enum](reference/common-models/enums#shoppersegment)	|	|	|The selection of the shopper segment associated with bid adjustment settings.	|
|optimization.<br />shopperSegmentBidAdjustment.<br />percentage|Integer	|	|	|The selection of the percentage change associated with a given shopper segment and bid adjustment settings.	|	
|optimization.<br />shopperCohortBidAdjustment.<br />shopperCohortType|[Enum](reference/common-models/enums#shoppercohorttype)	|	|	|The selection of the type associated with a shopper cohort.	|	
|optimization.<br />shopperCohortBidAdjustment.<br />percentage|Integer	|	|	|The selection of the percentage change associated with a given shopper cohort bid.	|
|optimization.<br />shopperCohortBidAdjustment.<br />audienceSegments.<br />audienceId|String	|	|	|The unique numerical ID associated with a audience segment.	|
|optimization.<br />shopperCohortBidAdjustment.<br />audienceSegments.<br />audienceSegmentType|[Enum](reference/common-models/enums#audiencesegmenttype)	|	|	|The selection of the type associated with an audience segment.	|
|budgetCaps.<br />budgetType	|[Enum](reference/common-models/enums#budgettype)	|TRUE	|	|The type of budget.	|	
|budgetCaps.<br />recurrenceTimePeriod	|[Enum](reference/common-models/enums#recurrenceTimePeriod)	|TRUE	|	|The recurrence time period that the budget cap is associated with.	|	
|budgetCaps.<br />budgetValue.monetaryBudget.<br />currencyCode	|[Enum](reference/common-models/enums#currencyCode)	|TRUE	|	|The currency associated with this monetary budget.	|	
|budgetCaps.<br />budgetValue.monetaryBudget.<br />amount	|Decimal	|TRUE	|	|The monetary amount of the budget cap in the given currency	|	
|budgetCaps.<br />budgetValue.monetaryBudget.<br />ruleAmount	|Decimal	|	|TRUE	|The monetary amount of the budget when a budget rule is applied.	|	
|targetingSettings	|[Enum](reference/common-models/enums#targetingSettings)	|	|	|The type of targeting supported by the campaign.	|	
| costType | [Enum](reference/common-models/enums#costType) | | | The way the campaign bids and is charged. | 
|tags.<br />key	|String	|	|	|Open ended labels with a key value pair applied to the campaign.	|	
|tags.<br />value	|String	|	|	|Open ended labels with a key value pair applied to the campaign.	|	

## Ad product mapping

The mapping table shows how current versions of different ad products map to the common campaign model. Over time, we will move to standardize the fields in each individual API to the common campaign model.

| Ad product | Latest version |
|-----|-------|
| Sponsored Products | [Version 3](sponsored-products/3-0/openapi/prod#tag/Campaigns) |
| Sponsored Brands | [Version 4](sponsored-brands/3-0/openapi/prod#tag/Campaigns) |
| Sponsored Display | [Version 1](sponsored-display/3-0/openapi#tag/Campaigns) |
| Sponsored Television | Version 1 |

For Amazon Marketing Stream, the table below references the [campaigns](guides/amazon-marketing-stream/datasets/sponsored-ads-campaign-management#campaigns-dataset-beta) dataset.

**Legend**

**x**: The ad product uses the common field name in its current version.

**N/A**:  The ad product contains the field in the common model schema, but not in its current version schema. 

**Empty cell**: The field is not represented for the ad product.

|Common field	|Sponsored Products | Sponsored Brands | Sponsored Display	|	Amazon Marketing Stream | Sponsored TV |
|---	|---	|---	|---	|---	|-----|
|campaignId	|x	|x	|x	| x | x| 
|adProduct	|N/A	|N/A	|N/A	|	x | N/A|
|portfolioId	|x	|x	|x	|	x| | 
|name	|x	|x	|x	| x | x| 
|startDate	|x	|x	|x	|	startDateTime | x|
|endDate	|x	|x	|x	|	endDateTime | x |
|lastUpdatedDateTime	|extendedData.<br />lastUpdateDateTime	|lastUpdatedDate	|lastUpdatedDate	|	audit.<br />lastUpdatedDateTime |extendedData.<br />lastUpdateDate|
|creationDateTime	|extendedData.<br />creationDateTime	|extended.<br />creationDateTime	|extended.<br />creationDateTime	|	audit.<br />creationDateTime | extendedData.<br />creationDate |
|state	|x	|x	|x	|	x | x |
|deliveryStatus	|N/A	|N/A	|N/A	| | N/A |
|deliveryReasons	|servingStatus	|servingStatus	|servingStatus	|	| |
|brandEntityId	|	|x	|	| | |
|optimization.<br />bidStrategy	|dynamicBidding.<br />strategy	|bidding.<br />bidOptimizationStrategy	|	| bidSetting.<br />bidStrategy | |
|optimization.<br />placementBidAdjustments.<br />placement	|dynamicBidding.<br />placementBidding.<br />placement	|bidding.<br />bidAdjustmentsByPlacement.<br />placement	|	| bidSetting.<br />placementBidAdjustment.<br />placement | |
|optimization.<br />placementBidAdjustments.<br />percentage	|dynamicBidding.<br />placementBidding.<br />percentage	|bidding.<br />bidAdjustmentsByPlacement.<br />percentage	|	| bidSetting.<br />placementBidAdjustment.<br />percentage | |
|optimization.<br />shopperSegmentBidAdjustment.<br />shopperSegment	|	|bidding.<br />bidAdjustmentsByShopperSegment.<br />shopperSegment	|	| | |
|optimization.<br />shopperSegmentBidAdjustment.<br />percent	|	|bidding.<br />bidAdjustmentsByShopperSegment.<br />percentage	|	| | |
|budgetCaps.<br />budgetType	|N/A	|N/A	|N/A	|	| N/A| 
|budgetCaps.<br />recurrenceTimePeriod	|budgetType	|budget.<br />budgetType	|budgetType	|	budget.budgetCap.<br />recurrence.<br />recurrenceType | budgetSettings.<br />budget.<br />recurrenceType |
|budgetCaps.<br />budgetValue.<br />monetaryBudget.<br />currencyCode	|N/A	|N/A	|N/A	|	 budget.<br />budgetCap.<br />monetaryBudget.<br />currencyCode | budgetSettings.<br />budget.<br />budgetValue.<br />budgetCurrencyCode |
|budgetCaps.<br />budgetValue.<br />monetaryBudget.<br />amount	|budget.budget	|budget	|budget	|	budget.<br />budgetCap.<br />monetaryBudget.<br />amount | budgetSettings.<br />budget.<br />budgetValue.<br />amount |
|budgetCaps.<br />budgetValue.<br />monetaryBudget.<br />ruleAmount	|budget.<br />effectiveBudget	|ruleBasedBudget.<br />value	|ruleBasedBudget.<br />value	| | |
|targetingSettings	|targetingType	|	|tactic	| x | |
| costType | | x| x| | |
|tags.key	|x	|x	|	| x |x|
|tags.value	|x	|x	|	| x |x|


## Representations

The following table shows the different areas where campaigns are surfaced in the Amazon Ads API.

| Feature | Operations | User guides |
|------|------|---------|
| [sp/campaigns](sponsored-products/3-0/openapi/prod#tag/Campaigns) | POST /sp/campaigns <br /> POST /sp/campaigns/list <br /> PUT /sp/campaigns <br /> POST /sp/campaigns/delete | [SP campaign model diagram](guides/sponsored-products/get-started/campaign-structure) <br /> [SP campaigns overview](https://advertising.amazon.com/API/docs/en-us/guides/sponsored-products/campaigns) |
| [sb/v4/campaigns](sponsored-brands/3-0/openapi/prod#tag/Campaigns) | POST /sb/v4/campaigns <br /> POST /sb/v4/campaigns/list <br /> PUT /sb/v4/campaigns <br /> POST /sb/v4/campaigns/delete | [SB campaign model diagram](guides/sponsored-brands/campaigns/structure) <br /> [SB campaigns overview](guides/sponsored-brands/campaigns/get-started-with-campaigns) |
| [sd/campaigns](sponsored-display/3-0/openapi#tag/Campaigns) | POST /sd/campaigns <br /> GET sd/campaigns <br /> PUT /sd/campaigns <br /> DELETE /sd/campaigns/{campaignId} | [SD campaigns overview](guides/sponsored-display/overview) |
| [Amazon Marketing Stream](guides/amazon-marketing-stream/overview) | N/A | [Campaigns dataset](guides/amazon-marketing-stream/datasets/campaigns) |
| [campaigns/exports](exports) | POST /campaigns/exports | [Exports overview](guides/exports/overview) |
| st/campaigns | POST /st/campaigns <br /> POST /st/campaigns/list <br /> PUT /st/campaigns <br /> POST /st/campaigns/delete | |

## JSON examples

Below you can find examples of how each ad product represents a campaign. 

### Generic

The generic sample includes a JSON representation of all possible fields in the common schema.

```json
[
    {
        "portfolioId": "string",
        "name": "string",
        "startDate": "string",
        "endDate": "string",
        "state": "ENABLED",
        "brandEntityId": "string",
        "costType": "CPC",
        "tags": [
            {
                "key": "string",
                "value": "string"
            }
        ],
        "targetingSettings": "AUTO",
        "campaignId": "string",
        "optimization": [
            {
                "bidStrategy": "string",
                "placementBidAdjustments": [
                    {
                        "placement": "string",
                        "percentage": 0
                    }
                ],
                "shopperSegmentBidAdjustment": [
                    {
                        "placement": "string",
                        "percentage": 0
                    }
                ],
                "shopperCohortBidAdjustment": [
                  {
                    "shopperCohortType": "AUDIENCE_SEGMENT",
                    "percentage": 0,
                    "audienceSegments": [
                        {
                            "audienceId": "string",
                            "audienceSegmentType": "SPONSORED_ADS_AMC"
                        }
                    ]
                  }
                ]
            }
        ],
        "budgetCaps": {
            "recurrenceTimePeriod": "DAILY",
            "budgetType": "MONETARY",
            "budgetValue": {
                "monetaryBudget": {
                    "currencyCode": "USD",
                    "amount": 0,
                    "ruleAmount": 0
                }
            }
        },
        "creationDateTime": "2023-08-30T17:49:26.756Z",
        "lastUpdatedDateTime": "2023-08-30T17:49:26.756Z",
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "CAMPAIGN_PENDING_START_DATE",
            "CAMPAIGN_INCOMPLETE"
        ]
    }
]
```

### Sponsored Products

```json
[
    {
        "portfolioId": "string",
        "campaignId": "string",
        "adProduct": "SPONSORED_PRODUCTS",
        "name": "string",
        "startDate": "2023-11-30",
        "endDate": "2023-12-08",
        "state": "ENABLED",
        "tags": [
            {
                "key": "string",
                "value": "string"
            }
        ],
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "CAMPAIGN_PENDING_START_DATE"
        ],
        "optimization": {
            "bidStrategy": "SALES_BID_DOWN_ONLY",
            "placementBidAdjustments": [
                {
                    "percentage": 0,
                    "placement": "PLACEMENT_TOP"
                }
            ]
        },
        "budgetCaps": {
            "recurrenceTimePeriod": "DAILY",
            "budgetType": "MONETARY",
            "budgetValue": {
                "monetaryBudget": {
                    "currencyCode": "USD",
                    "amount": 0
                }
            }
        },
        "targetingSettings": "MANUAL",
        "lastUpdatedDateTime": "2023-08-08T14:23:40.091Z",
        "creationDateTime": "2023-08-08T14:23:40.091Z"
    }
]
```

### Sponsored Brands

```json
[
    {
        "portfolioId": "string",
        "campaignId": "string",
        "adProduct": "SPONSORED_BRANDS",
        "name": "string",
        "startDate": "2023-12-21",
        "state": "ENABLED",
        "costType": "CPC",
        "tags": [
            {
                "key": "string",
                "value": "string"
            }
        ],
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "CAMPAIGN_PENDING_START_DATE"
        ],
        "brandEntityId": "string",
        "optimization": {
            "bidStrategy": "NEW_TO_BRAND_UP_AND_DOWN",
            "placementBidAdjustments": [
                {
                    "percentage": 15,
                    "placement": "OTHER"
                }
            ],
            "shopperSegmentBidAdjustment": [
                {
                    "percentage": 200,
                    "shopperSegment": "NEW_TO_BRAND_PURCHASE"
                }
            ]
        },
        "budgetCaps": {
            "recurrenceTimePeriod": "DAILY",
            "budgetType": "MONETARY",
            "budgetValue": {
                "monetaryBudget": {
                    "currencyCode": "USD",
                    "amount": 0.0
                }
            }
        },
        "lastUpdatedDateTime": "2023-09-18T20:07:28.202Z",
        "creationDateTime": "2023-09-18T20:07:28.224Z"
    }
]
```

### Sponsored Display

```json
[
    {
        "portfolioId": "string",
        "campaignId": "string",
        "adProduct": "SPONSORED_DISPLAY",
        "name": "string",
        "startDate": "2023-10-10",
        "endDate": "2023-11-01",
        "state": "PAUSED",
        "costType": "CPC",
        "tags": [
            {
                "key": "string",
                "value": "string"
            }
        ],
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "CAMPAIGN_PAUSED"
        ],
        "budgetCaps": {
            "recurrenceTimePeriod": "DAILY",
            "budgetType": "MONETARY",
            "budgetValue": {
                "monetaryBudget": {
                    "currencyCode": "USD",
                    "amount": 0
                }
            }
        },
        "targetingSettings": "T00030",
        "lastUpdatedDateTime": "2023-08-08T14:25:07.024Z",
        "creationDateTime": "2023-08-08T14:25:07.024Z"
    }
]
```
