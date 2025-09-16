---
title: Getting started with the budget usage API
description: Learn how to get started using the Amazon Ads budget usage API.
type: guide
interface: api 
tags:
    - Sponsored Brands
    - Sponsored Products
    - Sponsored Display
    - Budget Usage
keywords:
    - budget
    - budgets
    - budget usage
    - portfolios
---

# Getting started with the budget usage API

You can check budget usage at both the individual campaign level and the portfolio level. 

Each ad type has its own budget usage endpoint. For example, to check the current budget usage for a Sponsored Products campaign, you would use the POST /sp/campaigns/budget/usage endpoint. 

>[TIP] For portfolios, use the [POST /portfolios/budget/usage](reference/portfolios) endpoint.

|Ad type| Endpoint |
|-------|----------|
| Sponsored Products |[POST /sp/campaigns/budget/usage](sponsored-products/3-0/openapi/prod#tag/Budget-Usage/operation/spCampaignsBudgetUsage)|
| Sponsored Brands | [POST /sb/campaigns/budget/usage](sponsored-brands/3-0/openapi/prod#tag/Budget-Usage)|
| Sponsored Display | [POST /sd/campaigns/budget/usage](sponsored-display/3-0/openapi#tag/Budget-Usage/operation/sdCampaignsBudgetUsage)|

## Retrieving budget usage percentage for one or more campaigns

This example shows how to retrieve budget usage for a Sponsored Brands campaign. The pattern is the same for all three ad types.

```
POST /sb/campaigns/budget/usage
```

### Request body

The request object for the usage resource supports a maximum of 100 campaigns per request.

```json
{
"campaignIds":  [
  "campaign1"
 ]
}
```

### Response body

A successful response will return a `207` as well as a list of budget usage response objects reflecting the same order as the input. `lastUpdatedDate` provides the timestamp at which the budgetUsagePercent was recorded.

```json
{
    "success": [
        {
            "lastUpdatedDate": "2022-01-05T02:45:32.486Z",
            "budgetUsagePercent": 0,
            "campaignId": "campaign1",
            "index": 0,
            "budget": 0
        }
    ],
    "error": [
        {
            "code": "string",
            "campaignId": "string",
            "index": 0,
            "details": "string"
        }
    ]
}
```

## Other examples

The following examples demonstrate several scenarios you can expect when you retrieve the `usage` resource.

This example demonstrates all requested `campaignIds` being retrieved successfully. 

### Request body

```json
{
 "campaignIds": [
  "campaign1",
  "campaign2",
  "campaign3"
 ]
}
```

### Response body

```json
{
"success":  [
  {
   "lastUpdatedDate":  "2022-01-05T02:45:32.486Z",
   "budgetUsagePercent": 50,
   "campaignId":  "campaign1",
   "index":  0,
   "budget": 100
  }
  {
   "lastUpdatedDate": "2022-01-05T02:45:32.486Z",
   "budgetUsagePercent": 60,
   "campaignId": "campaign2",
   "index": 1,
   "budget": 100
  }
  {
   "lastUpdatedDate": "2022-01-05T02:45:32.486Z",
   "budgetConsumptionPercent": 75,
   "campaignId": "campaign3",
   "index": 2,
   "budget": 100
  }
 ],
 "error":  [
 ]
}
```

In this example, `campaign1` was retrieved successfully, but `campaign2` and  `campaign3` received an error.

### Request body

```json
{
 "campaignIds": [
  "campaign1",
  "campaign2",
  "campaign3"
 ]
}
```

### Response body

```json
{
    "success": [
        {
            "lastUpdatedDate": "2022-01-05T02:45:32.486Z",
            "budgetUsagePercent": 50,
            "campaignId": "campaign1",
            "index": 0,
            "budget": 100
        }
    ],
    "error": [
        {
            "code": "string",
            "campaignId": "campaign2",
            "index": 1,
            "details": "string"
        }, 
      {
            "code": "string",
            "campaignId": "campaign3",
            "index": 2,
            "details": "string"
        }
    ]
}
```

## Retrieving budget usage percentage for a portfolio

A portfolio is a group of campaigns that you can organize by brand, product category, or by season. This example demonstrates all requested `portfolioIds` being retrieved successfully.

> [NOTE] To view the individual budget usage percentages associated with each campaign, you will have to use the list campaigns resource for that ad type, then use the `portfolioIdFilter` parameter in the header to get an array of campaign objects associated with the portfolio ID you specified in the `portfolioIdFilter` parameter. Then you can retrieve each of the campaign budget usage percentages using the associated campaign budget usage resource as mentioned above. 

```
POST /portfolios/budget/usage
```

### Request body

The request object for the usage resource supports a maximum of 100 portfolios per request.

```json
{
 "portfolioIds": [
  "`portfolio1`",
  "portfolio2",
  "portfolio3"
 ]
}
```

### Response body

```json
{
    "success": [
        {
            "lastUpdatedDate": "2022-01-05T02:45:32.486Z",
            "budgetUsagePercent": 50,
            "portfolioId": "portfolio1",
            "index": 0,
            "budget": 100
        }
    ],
    "error": [
        {
            "code": "string",
            "portfolioId": "portfolio2",
            "index": 1,
            "details": "string"
        },
        {
            "code": "string",
            "portfolioId": "portfolio3",
            "index": 2,
            "details": "string"
        }
    ]
}
```