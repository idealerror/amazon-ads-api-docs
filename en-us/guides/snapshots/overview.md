---
title: Snapshots overview
description: Overview of API snapshots functionality, including information about how to use them to retrieve campaign data and avoid rate limiting. 
type: guide
interface: api
tags:
    - Account management
    - Campaign management
    - Reporting
keywords:
    - metadata
    - rate limit
    - extended
---

# Snapshots overview

>[WARNING] Snapshots APIs are deprecated and will be shut off on October 15, 2024. For replacement functionality, see the [exports](guides/exports/overview) API. To learn more, view the [migration guide](reference/migration-guides/snapshots-exports). 

Use snapshots to efficiently retrieve the structure of your Sponsored Products, Sponsored Brands, and Sponsored Display campaigns. Snapshots are generated asynchronously and can be requested at multiple levels, including campaign, ad group, product ad, target, keyword, and more.

## Snapshots vs. reports

While reports contain performance data for a specified day, snapshots provide metadata about your campaigns at the time of the request. In other words, you can use snapshots to retrieve metadata on all configured campaigns, ad groups, targeting expressions, and more, depending on the campaign type. 

## Snapshots vs. list operations

Snapshots return similar information to GET and GET /extended operations for campaigns, ad groups, keywords, and other campaign entities. Snapshots run asynchronously while GET and GET /extended are synchronous. If you have many campaigns or a complex campaign structure and are looking to pull information on all campaigns in your account, we suggest using snapshots.

If you need details for a single campaign, use a GET request with query parameters instead of a snapshot. 

## Sample snapshot

Snapshots are in JSON format. 

This example shows a Sponsored Products snapshot containing a single targeting record.

```json
[
    {
        "targetId":1234567890,
        "adGroupId":2345678901,
        "campaignId":345678901,
        "expressionType":"manual",
        "state":"enabled",
        "bid":1.18,
        "expression":[
            {
                "type":"asinCategorySameAs",
                "value":"2475895011"}
            ],
        "resolvedExpression":[
            {
                "type":"asinCategorySameAs",
                "value":"Men's Wallets"
            }
        ]
    }
]

```

## Snapshot types by ad type

Each ad type supports different types of snapshots. 

| Snapshot type | Sponsored Products | Sponsored Brands | Sponsored Display |
|---------------|----|----|----|
| campaigns | x | x | x |
| adGroups | x |  | x |
| keywords | x | x |  |
| negativeKeywords | x |  |  |
| campaignNegativeKeywords | x |  |  |
| productAds | x |  | x |
| targets | x |  | x |
| negativeTargets | x |  |  |


## Fields by ad and snapshot type

Snapshots for different ad types contain different fields. The tables in the following sections list the fields included in each type of snapshot.

- [Sponsored Products](#sponsored-products)
- [Sponsored Brands](#sponsored-brands)
- [Sponsored Display](#sponsored-display)

### Sponsored Products

#### Campaigns

| Field | Description | 
|-------|-------------|
| campaignId | The ID of the campaign. Can be used to join with reporting data. |
| name | The name of the campaign as specified by the advertiser. |
| campaignType | The ad type related to the campaign. Always `sponsoredProducts`. |
| targetingType | The type of targeting used for the campaign, either `manual` or `auto`. |
| premiumBidAdjustment | A boolean representing whether premium bid adjustment is turned on for the campaign. |
| dailyBudget | The daily budget for the campaign as set by the advertiser. |
| startDate | The start date of the campaign. |
| state | The status of the campaign (`enabled`, `paused`, or `archived`). |
| bidding.strategy | The bidding strategy, either `legacyForSales`, `autoForSales`, or `manual`. |
| bidding.adjustments.predicate | The placement location where bid controls are used, either `placementTop` or `placementProductPage`. |
| bidding.adjustments.percentage | The bid adjustment percentage value. |

#### Ad groups

| Field | Description | 
|-------|-------------|
| adGroupId | The ID of the ad group. Can be used to join with reporting data.
| name | The name of the ad group as specified by the advertiser. |
| campaignId | The ID of the campaign. |
| defaultBid | The default bid price for the ad group. |
| state | The status of the ad group (`enabled`, `paused`, or `archived`). |

#### Keywords

>[NOTE] If you have Sponsored Products campaigns that use **auto targeting**, make sure to request both `keywords` and `targets` snapshots to ensure you receive all campaign targeting structures. 

| Field | Description | 
|-------|-------------|
| keywordId | The ID of the keyword. Can be used to join with reporting data. |
| adGroupId | The name of the ad group as specified by the advertiser. |
| campaignId | The ID of the campaign. |
| keywordText | The exact text for the keyword. |
| matchType | One of `broad`, `exact`, or `phrase`. |
| state | The status of the keyword (`enabled`, `paused`, or `archived`). |
| bid | The bid price for the keyword. |

#### Negative keywords

| Field | Description | 
|-------|-------------|
| keywordId | The ID of the ad group. Can be used to join with reporting data.
| adGroupId | The name of the ad group as specified by the advertiser. |
| campaignId | The ID of the campaign. |
| keywordText | The text of the negative keyword. |
| matchType | One of `broad`, `exact`, or `phrase`. |
| state | The status of the keyword (`enabled`, `paused`, or `archived`). |

#### Campaign negative keywords

| Field | Description | 
|-------|-------------|
| keywordId | The ID of the ad group. Can be used to join with reporting data. |
| campaignId | The ID of the campaign. |
| keywordText | The text of the negative keyword.  |
| matchType | One of `broad`, `exact`, or `phrase`. |
| state | The status of the keyword (`enabled`, `paused`, or `archived`). |

#### Product ads

| Field | Description | 
|-------|-------------|
| adId | The ID of the ad. Can be used to join with reporting data. |
| adGroupId | The ID of the ad group. |
| campaignId | The ID of the campaign. |
| asin | The ASIN associated with the ad. |
| sku | The product SKU associated with the ad. Only included for seller accounts. |
| state | The status of the ad (`enabled`, `paused`, or `archived`). |

#### Targets

>[NOTE] If you have Sponsored Products campaigns that use **auto targeting**, make sure to request both `keywords` and `targets` snapshots to ensure you receive all campaign targeting structures. 

| Field | Description | 
|-------|-------------|
| targetId | The ID of the targeting expression. Can be used to join with reporting data |
| adGroupId | The ID of the ad group. |
| campaignId | The ID of the campaign. |
| expressionType | The targeting strategy, either `manual` or `auto`. |
| state | The status of the targeting expression (`enabled`, `paused`, or `archived`). |
| bid | The bid price for the targeting expression. |
| expression.type | The operator used in the targeting expression. |
| expression.value | The value of the targeting expression. |
| resolvedExpression.type | The operator used in the targeting expression. |
| resolvedExpression.value | The resolved value of the targeting expression. |

#### Negative targets

| Field | Description | 
|-------|-------------|
| targetId | The ID of the targeting expression. Can be used to join with reporting data. |
| adGroupId | The ID of the ad group. |
| campaignId | The ID of the campaign. |
| expressionType | The targeting strategy, either `manual` or `auto`. |
| state | The status of the targeting expression (`enabled`, `paused`, or `archived`). |
| expression.type | The operator used in the targeting expression. |
| expression.value | The value of the targeting expression. |
| resolvedExpression.type | The operator used in the targeting expression. |
| resolvedExpression.value | The resolved value of the targeting expression. |

### Sponsored Brands

>[WARNING] Currently, the Ads API does not support snapshots for Sponsored Brands video campaigns or campaigns created using the [version 4 endpoints](sponsored-brands/3-0/openapi/prod#tag/Campaigns). Snapshots include records for [version 3](sponsored-brands/3-0/openapi#tag/Campaigns), non-video campaigns only. 

#### Campaigns

| Field | Description | 
|-------|-------------|
| campaignId | The ID of the campaign. Can be used to join with reporting data. |
| name | The name of the campaign as specified by the advertiser. |
| budget | The daily budget for the campaign as set by the advertiser. |
| budgetType | The time period for the budget, either `daily` or `lifetime`. |
| startDate | The start date of the campaign. |
| state | The status of the campaign (`enabled`, `paused`, or `archived`). |
| servingStatus | The [computed status](reference/concepts/computed-status) of the campaign.  |
| spendingPolicy | The spending policy for the campaign. |
| bidOptimization | Whether automatic bid optimization is enabled for the campaign, either `true` or `false`. |
| ruleBasedBudget.value | The value of the rule-based budget. |
| ruleBasedBudget.applicableRuleId | The identifier of the active rule. |
| ruleBasedBudget.applicableRuleName | The name of the active rule. |
| ruleBasedBudget.isProcessing | Set to `true` if rule evaluation is in progress. Set to `false` when evaluation is complete and the rule budget value is updated. |

#### Keywords

| Field | Description | 
|-------|-------------|
| keywordId | The ID of the keyword. Can be used to join with reporting data. |
| adGroupId | The name of the ad group as specified by the advertiser. |
| campaignId | The ID of the campaign. |
| keywordText | The exact text for the keyword. |
| matchType | One of `BROAD`, `EXACT`, or `PHRASE` |
| state | The status of the keyword (`enabled`, `paused`, or `archived`). |
| bid | The bid price for the keyword. |

### Sponsored Display

#### Campaigns

| Field | Description | 
|-------|-------------|
| campaignId | The ID of the campaign. Can be used to join with reporting data. |
| name | The name of the campaign as specified by the advertiser. |
| tactic | The targeting tactic for the campaign, either `T00020` or `T00030`. |
| budget | The budget amount for the campaign as set by the advertiser. |
| budgetType | The time period for which budget is allocated. |
| startDate | The start date of the campaign. |
| state | The status of the campaign (`enabled`, `paused`, or `archived`). |
| costType | How the campaign bills, either `vCPM` (cost per thousand viewable impressions) or `CPC` (cost per click). |
| deliveryProfile | Always `as_soon_as_possible`.  |
| servingStatus | The [computed status](reference/concepts/computed-status) of the campaign. |
| creationDate | The date the campaign was created. |
| lastUpdateDate | The date of the last update to the campaign. |

#### Ad groups

| Field | Description | 
|-------|-------------|
| adGroupId | The ID of the ad group. Can be used to join with reporting data.
| name | The name of the ad group as specified by the advertiser. |
| campaignId | The ID of the campaign. |
| tactic | The targeting tactic for the campaign, either `T00020` or `T00030`. |
| defaultBid | The default bid price for the ad group. |
| bidOptimization | For vCPM campaigns, the value is always `reach`. For CPC campaigns, value is either `clicks` or `conversions`. |
| state | The status of the ad group (`enabled`, `paused`, or `archived`). |
| servingStatus | The [computed status](reference/concepts/computed-status) of the ad group. |
| creationDate | The date the ad group was created. |
| lastUpdateDate | The date of the last update to the ad group. |
| creativeType | The type of creative associated with the ad group. |

#### Product ads

| Field | Description | 
|-------|-------------|
| adId | The ID of the ad. Can be used to join with reporting data. |
| adGroupId | The ID of the ad group. |
| campaignId | The ID of the campaign. |
| asin | The ASIN associated with the ad. |
| sku | The product SKU associated with the ad. Only included for seller accounts. |
| state | The status of the ad (`enabled`, `paused`, or `archived`). |
| servingStatus | The [computed status](reference/concepts/computed-status) of the ad. |
| creationDate | The date the ad was created. |
| lastUpdateDate | The date of the last update to the ad. |

#### Targets

| Field | Description | 
|-------|-------------|
| targetId | The ID of the targeting expression. Can be used to join with reporting data. |
| adGroupId | The ID of the ad group. |
| expressionType | The targeting strategy, either `manual` or `auto`. |
| state | The status of the targeting expression (`enabled`, `paused`, or `archived`). |
| expression.type | The operator used in the targeting expression. |
| expression.value | The value of the targeting expression. |
| resolvedExpression.type | The operator used in the targeting expression. |
| resolvedExpression.value | The resolved value of the targeting expression. |
| servingStatus | The [computed status](reference/concepts/computed-status) of the targeting expression. |
| creationDate | The date the targeting expression was created. |
| lastUpdateDate | The date of the last update to the targeting expression. |