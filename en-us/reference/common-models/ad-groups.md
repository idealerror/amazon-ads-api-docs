---
title: Amazon Ads API ad groups model overview
description: Describes the ad group model and associated fields for the Amazon Ads API.
type: guide
interface: api
tags:
    - Operations
---

# Ad groups

**Ad groups** are used to group ads that have common targeting, strategy, and creatives.

## Schema

Ad groups contain the following fields. **Read-only** indicates that the field is part of the model, but cannot be modified by advertisers. **Required** indicates that a field will always appear as part of model. 

>[NOTE] Some ad group fields are only available for certain ad products. For details, see the [Ad product mapping table](#ad-product-mapping).

|Field	|Description	|Type	|Required	|Read only	|
|---	|---	|---	|---	|---	|
|adGroupId	|The unique identifier of the ad group.	|String	|TRUE	|TRUE	|
|campaignId	|The unique identifier of the campaign the ad group belongs to.	|String	|TRUE	| 	|
|adProduct	|The ad product that the ad group belongs to.	|[Enum](reference/common-models/enums#adproduct)	|TRUE	| 	|
|name	|The name of the ad group.	|String	|TRUE	| 	|
|state	|The user-set state of the ad group.	|[Enum](reference/common-models/enums#state)	|TRUE	| 	|
|deliveryStatus	|This is an overall status if the ad group is delivering or not.	|[Enum](reference/common-models/enums#deliverystatus)	|TRUE	|TRUE	|
|deliveryReasons	|This is a list of reasons why the ad group is not delivering and the reasons behind the delivery status.	|[Enum](reference/common-models/enums#deliveryreasons)	|	|TRUE	|
|creativeType	|The creative type that this ad group contains. 	|String	|	|	|
|creationDateTime	|The date time that the ad group was created.	|datetime	|TRUE	|TRUE	|
|lastUpdatedDateTime	|The date time that the ad group was last updated.	|datetime	|TRUE	|TRUE	|
|bid.<br />defaultBid	|The default maximum bid for ads and targets in the ad group. This is used in sponsored ads as the maximum bid during the auction.	|double	|TRUE	| 	|
|bid.<br />currencyCode	|The currency code the bid is expressed in.	|[Enum](reference/common-models/enums#currencyCode)	|TRUE	| 	|
|optimization.<br />goalSetting.<br />goal	|The type of goal associated with the ad group.	|[Enum](reference/common-models/enums#goal)	| 	| 	|
|optimization.<br />goalSetting.<br />kpi	|The way the goal associated with the ad group is measured.	|[Enum](reference/common-models/enums#kpi)	| 	| 	|	

## Ad product mapping

The mapping table shows how current versions of different ad products map to the common ad group model. Over time, we will move to standardize the fields in each individual API to the common ad group model.

| Ad product | Latest version |
|-----|-------|
| Sponsored Products | [Version 3](sponsored-products/3-0/openapi/prod#tag/AdGroups) |
| Sponsored Brands | [Version 4](sponsored-brands/3-0/openapi/prod#tag/AdGroups) |
| Sponsored Display | [Version 1](sponsored-display/3-0/openapi#tag/Ad-Groups) |
| Sponsored Television | Version 1 |

For Amazon Marketing Stream, the table below references the [ad groups](guides/amazon-marketing-stream/datasets/sponsored-ads-campaign-management#ad-groups-dataset-beta) dataset.

**Legend**

**x**: The ad product uses the common field name in its current version.

**N/A**:  The ad product contains the field in the common model schema, but not in its current version schema. 

**Empty cell**: The field is not represented for the ad product.

|Common field	| Sponsored Products|Sponsored Brands	| Sponsored Display	| Amazon Marketing Stream | Sponsored TV |
|---	|---	|---	|---	|----|----|
|adGroupId	|x	|x	|x	| x | x |
|campaignId	|x	|x	|x	| x | x |
|adProduct	|N/A	|N/A	|N/A	| x | N/A |
|name	|x	|x	|x	| x | x |
|state	|x	|x	|x	| x | x|
|deliveryStatus	|N/A	|N/A	|servingStatus	| | N/A |
|deliveryReasons	|extendedData.<br />servingStatus	|extendedData.<br />servingStatus	|N/A	| | |
|creativeType	|	|	|x	| | |
|creationDateTime	|extendedData.creationDateTime	|extendedData.<br />creationDate	|creationDate	| audit.<br />creationDateTime | extendedData.<br />creationDate |
|lastUpdatedDateTime	|extendedData.<br />lastUpdateDateTime	|extendedData.<br />lastUpdateDate	|lastUpdateDate	| audit.<br />lastUpdatedDateTime | extendedData.<br />lastUpdateDate |
|bid.<br />defaultBid	|defaultBid	|	|defaultBid	| bidValue.<br />defaultBid.value | defaultBid.bid |
|bid.<br />currencyCode	|N/A	|	|N/A	| bidValue.<br />defaultBid.<br />currencyCode | |
|optimization.<br />goalSettings.<br />goal	|	|	|bidOptimization	| | |
|optimization.<br />goalSettings.<br />Kpi	|	|	| derived from bidOptimization	| |  |

## Representations

The following table shows the different areas where ad groups are surfaced in the Amazon Ads API.

| Feature | Operations | User guides |
|------|------|---------|
| [sp/adGroups](sponsored-products/3-0/openapi/prod#tag/AdGroups) | POST /sp/adGroups <br /> POST /sp/adGroups/list <br /> PUT /sp/adGroups <br /> POST /sp/adGroups/delete | [SP campaign model diagram](guides/sponsored-products/get-started/campaign-structure) <br /> [SP ad groups overview](guides/sponsored-products/ad-groups) |
| [sb/v4/adGroups](sponsored-brands/3-0/openapi/prod#tag/AdGroups) | POST /sb/v4/adGroups <br /> POST /sb/v4/adGroups/list <br /> PUT /sb/v4/adGroups <br /> POST /sb/v4/adGroups/delete | [SB campaign model diagram](guides/sponsored-brands/campaigns/structure) |
| [sd/adGroups](sponsored-display/3-0/openapi#tag/Ad-Groups) | POST /sd/adGroups <br /> GET sd/adGroups <br /> PUT /sd/adGroups <br /> DELETE /sd/adGroups/{adGroupId} | [SD campaigns overview](guides/sponsored-display/overview) |
| [Amazon Marketing Stream](guides/amazon-marketing-stream/overview) | N/A | [Ad groups dataset](guides/amazon-marketing-stream/datasets/ad-groups) |
| [adGroups/exports](exports) | POST /adGroups/exports | [Exports overview](guides/exports/overview) |
| st/adGroups | POST /st/adGroups <br /> POST /st/adGroups/list <br /> PUT /st/adGroups <br /> POST /st/adGroups/delete | | 



## JSON examples

Below you can find examples of how each ad product represents an ad group. 

### Generic

The generic sample includes a JSON representation of all possible fields in the common schema.

```json
[
    {
        "adGroupId": "string",
        "campaignId": "string",
        "adProduct": "string",
        "name": "string",
        "state": "string",
        "deliveryStatus": "string",
        "deliveryReasons": list<string>,
        "creativeType": "string",
        "creationDateTime": datetime,
        "lastUpdatedDateTime": datetime,
        "bid": {
            "defaultBid": 0.0,
            "currencyCode": "string"
        },
        "optimization": {
            "goalSettings": {
                "goalType": "string",
                "goalKpi": "string"
            }
        }
    }
]
```

### Sponsored Products

```json
[
    {
        "adGroupId": "string",
        "campaignId": "string",
        "adProduct": "SPONSORED_PRODUCTS",
        "name": "string",
        "state": "ARCHIVED",
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "CAMPAIGN_ARCHIVED"
        ],
        "creationDateTime": "2023-07-25T02:19:06.615Z",
        "lastUpdatedDateTime": "2023-07-25T02:20:11.046Z",
        "bid": {
            "defaultBid": 0.0,
            "currencyCode": "USD"
        }
    }
]
```

### Sponsored Brands

```json
[
    {
        "adGroupId": "string",
        "campaignId": "string",
        "adProduct": "SPONSORED_BRANDS",
        "name": "string",
        "state": "ARCHIVED",
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "CAMPAIGN_ARCHIVED"
        ],
        "creationDateTime": "2023-06-21T02:10:22.929Z",
        "lastUpdatedDateTime": "2023-06-21T02:10:42.835Z"
    }
]
```

### Sponsored Display

```json
[
    {
        "adGroupId": "string",
        "campaignId": "string",
        "adProduct": "SPONSORED_DISPLAY",
        "name": "string",
        "state": "ENABLED",
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "CAMPAIGN_PENDING_START_DATE"
        ],
        "creativeType": "IMAGE",
        "creationDateTime": "2023-07-26T00:09:00.971Z",
        "lastUpdatedDateTime": "2023-07-26T00:09:00.971Z",
        "bid": {
            "defaultBid": 0.0,
            "currencyCode": "USD"
        },
        "optimization": {
            "goalSettings": {
                "goalType": "ENGAGEMENT_WITH_MY_AD",
                "goalKpi": "CLICKS"
            }
        }
    }
]
```

