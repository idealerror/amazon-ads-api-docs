---
title: Get started with Portfolios
description: Get started with using Portfolios
type: guide
interface: api
tags:
    - Portfolios
---

# Get started with Portfolios

## Overview

Portfolios are groups of campaigns that you can organize to meet your advertising needs. For example, you can create portfolios to organize campaigns by brand, product category, or season to provide structure and to manage your advertising activities. Portfolios within your advertising account do not compete against each other to help ensure that best performing campaigns continue to show to the customer. 

Portfolios can simplify your day-to-day campaign management in multiple ways:

*  __Multibrand management__ – Group your campaigns by brand, business line or season to reflect your marketing needs.
*  __Budget caps__ – Define a date range or recurring monthly budget cap and ensure that your campaign spends don’t exceed your desired marketing needs.
* __Share unspent campaign budget__ – Automatically share unspent campaign budget from previous days in the month with other campaigns in the same portfolio that have already spent their full daily budget. 
*  __Track Performance__ – Review portfolio performance and create reports for campaigns by portfolio.
*  __Streamlined billing__ – Simplified billing details broken down by aggregated portfolio spend in each billing cycle.

Learn more about [Portfolios](https://advertising.amazon.com/help/GPEJN2E6R52G7C2T).


## Before you begin

Complete the Amazon Ads API [Onboarding](guides/onboarding/overview) and [Getting started](guides/get-started/overview) processes to obtain your access token and profile ID. You will need these to make all the calls referenced in the tutorial.


## Creating  Portfolios

To create a portfolio, use [POST /portfolios](reference/portfolios#tag/Portfolios/operation/CreatePortfolios). For budgets, the parameters are as follows:

* The `dateRange` policy specifies a budget cap for a specific period of time.
* The `monthlyRecurring` policy specifies a budget that is automatically renewed at the beginning of each month.
* The `NO_CAP` setting allows the campaigns in your portfolio to continue spending until those individual campaign budgets have been used.


**Sample request**

```
curl --location --request POST 'https://advertising-api.amazon.com/portfolios' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{
  "portfolios": [
    {
      "name": "My portfolio",
      "state": "ENABLED",
      "budget": {
        "amount": 0.1,
        "endDate": "2024-09-25",
        "currencyCode": "USD",
        "startDate": "2024-08-30",
        "policy": "DATE_RANGE"
      }
    }
  ]
}'

```

**Sample response**


```
{
    "portfolios": {
        "error": [],
        "success": [
            {
                "index": 0,
                "portfolioId": "69616386521924"
            }
        ]
    }
}
```

Once you obtain a `portfolioId`, you can associate campaigns to the portfolio created. 

>[NOTE] A campaign can only belong to one portfolio at a time.

## Retrieve a list of Portfolios

Use the [POST /portfolios/list](reference/portfolios#tag/Portfolios/operation/ListPortfolios) endpoint to retrieve portfolios. The response includes a set of properties for each portfolio you have created.

**Sample request**


```
curl --location --request POST 'https://advertising-api.amazon.com/portfolios/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/vnd.spPortfolio.v3+json' \
--data-raw '{
  "portfolioIdFilter": {
    "include": [
      "69616386521924"
    ]
  }
}'
```

**Sample response**


```
{
    "portfolios": [
        {
            "budget": {
                "amount": 10.0,
                "currencyCode": "USD",
                "endDate": "2024-09-30",
                "policy": "DATE_RANGE",
                "startDate": "2024-08-30"
            },
            "inBudget": true,
            "name": "My portfolio",
            "portfolioId": "69616386521924",
            "state": "ENABLED"
        }
    ],
    "totalResults": 1
}
```

## Budget usage

The [/portfolios/budget/usage](https://advertising.amazon.com/API/docs/en-us/reference/portfolios#tag/Budget-Usage) endpoint returns budget usage percentages for the portfolio specified in the request body. This allows you to manage and track your advertising spend across all portfolios you have under your advertiser account. 

**Sample request**


```
curl --location --request POST 'https://advertising-api.amazon.com/portfolios/budget/usage' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/vnd.portfoliobudgetusage.v1+json' \
--data-raw '{
  "portfolioIds": [
    "274923182315193",
    "112009396696771"
  ]
}'
```

**Sample response**


```
{
    "error": [],
    "success": [
        {
            "budget": 10.0,
            "budgetUsagePercent": 0.0,
            "index": 0,
            "portfolioId": "274923182315193",
            "usageUpdatedTimestamp": "2024-08-20T19:39:44Z"
        },
        {
            "budget": 0.1,
            "budgetUsagePercent": 0.0,
            "index": 1,
            "portfolioId": "112009396696771",
            "usageUpdatedTimestamp": "2024-08-20T18:19:14Z"
        }
    ]
}
```



## Reporting

The `portfolioId` metric in reporting returns all campaigns associated with a specific `portfolioId`. To view the report types where this metric is available, refer to the [Columns](guides/reporting/v3/columns#portfolioid) section.

## Learn more

* [Learn about sharing leftover budget across campaigns in a portfolio](guides/portfolios/sharing-leftover)
* [API reference](reference/portfolios)

