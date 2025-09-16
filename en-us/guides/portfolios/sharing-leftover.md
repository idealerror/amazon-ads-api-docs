---
title: Sharing leftover budget in a portfolio 
description: Sharing leftover budget in a Portfolio
type: guide
interface: api
tags:
    - Portfolios
---

# Sharing leftover budget in a portfolio

## Overview

When the unspent campaign budget feature is turned on inside a portfolio, campaigns that have already spent their full daily budget can automatically use other campaigns’ unspent campaign budget from previous days in the month, allowing them to spend up to 100% more than their average daily budget on any given day. This system allows you to benefit from high traffic days. At the end of the month, the campaigns in your portfolio won’t spend more in total than the sum of the daily budget for each campaign, multiplied by the number of days in that month. Your invoice will be adjusted for any over-delivery, so that you won't be charged for any amount in excess of your monthly charging limit.

Your campaign will still be able to use its own unspent leftovers from previous days using the average daily budget system. The unspent campaign budget sharing system is offered to help campaigns that still run out of budget, even after spending their own leftovers from previous days. For example, suppose you have 3 campaigns in a portfolio which has the unspent campaign budget sharing feature turned on, and each campaign has a daily budget of $100. Campaign 1 spends $70 on the first day of the month, Campaign 2 spends $50, and Campaign 3 spends $100. There is now a total of $80 in unspent leftovers available which will be available to campaigns in the portfolio if they spend all their budget the next day. The next day, if Campaign 3 exhausts its $100 daily budget, it can still spend the additional $80 of unspent campaign budget to get additional clicks and impressions. This continues through the calendar month, and at the beginning of the next month the leftover amount is reset to 0 and restarts accumulating for the new month.

## Getting started

### Setting unspent campaign budget sharing 

You can set the unspent campaign budget sharing option to on or off. You select your choice at the portfolio level, so all campaigns within the portfolio are eligible to share leftover budget amounts to spend up to 100% over daily budget.

By default, all portfolios created after 6/20/2025 have unspent campaign budget sharing turned on. To turn the feature on or off for a given portfolio, use PUT /portfolios to update your settings and set `featureState` for `campaignUnspentBudgetSharing` as `DISABLED` inside __budgetControls__.

#### Sample request

```
{
    "portfolios": [{
        "portfolioId": "190881001",
        "name": "XYZ",
        "state": "ENABLED",
        "budgetControls": {
            "campaignUnspentBudgetSharing": {
                "featureState": "DISABLED"
            }
        },
        "budget": {
            "amount": 50,
            "endDate": "2024-02-15",
            "currencyCode": "USD",
            "startDate": "2024-01-15",
            "policy": "DATE_RANGE"
        }
    }]
}

```

#### Sample response

```
{
    "portfolios": {
        "success": [{
            "portfolioId": "190881001",
            "portfolio": {
                "inBudget": true,
                "portfolioId": "190881001",
                "name": "XYZ",
                "state": "ENABLED",
                "budgetControls": {
                    "campaignUnspentBudgetSharing": {
                        "featureState": "DISABLED"
                    }
                },
                "budget": {
                    "amount": 50,
                    "endDate": "2024-02-15",
                    "currencyCode": "USD",
                    "startDate": "2024-01-15",
                    "policy": "DATE_RANGE"
                },
                "extendedData": {
                    "statusReasons": [
                        "PORTFOLIO_STATUS_ENABLED_DETAIL"
                    ],
                    "servingStatus": "PORTFOLIO_STATUS_ENABLED",
                    "lastUpdateDate": 0,
                    "creationDate": 0
                }
            },
            "index": 0
        }],
        "error": [{}]
    }
}

```

### Re-enabling unspent campaign budget sharing

If you’ve previously turned off unspent campaign budget sharing for your portfolio but now you would like to turn on sharing, use `PUT /portfolios` to update your settings and set featureState for `campaignUnspentBudgetSharing` as `ENABLED` inside __budgetControls__.

#### Sample request

```
{
    "portfolios": [{
        "portfolioId": "190881001",
        "name": "XYZ",
        "state": "ENABLED",
        "budgetControls": {
            "campaignUnspentBudgetSharing": {
                "featureState": "ENABLED"
            }
        },
        "budget": {
            "amount": 50,
            "endDate": "2024-02-15",
            "currencyCode": "USD",
            "startDate": "2024-01-15",
            "policy": "DATE_RANGE"
        }
    }]
}
```

#### Sample response

```
{
    "portfolios": {
        "success": [{
            "portfolioId": "190881001",
            "portfolio": {
                "inBudget": true,
                "portfolioId": "190881001",
                "name": "XYZ",
                "state": "ENABLED",
                "budgetControls": {
                    "campaignUnspentBudgetSharing": {
                        "featureState": "ENABLED"
                    }
                },
                "budget": {
                    "amount": 50,
                    "endDate": "2024-02-15",
                    "currencyCode": "USD",
                    "startDate": "2024-01-15",
                    "policy": "DATE_RANGE"
                },
                "extendedData": {
                    "statusReasons": [
                        "PORTFOLIO_STATUS_ENABLED_DETAIL"
                    ],
                    "servingStatus": "PORTFOLIO_STATUS_ENABLED",
                    "lastUpdateDate": 0,
                    "creationDate": 0
                }
            },
            "index": 0
        }],
        "error": [{}]
    }
}
```

### Checking your unspent campaign budget sharing settings

To check your current settings for unspent campaign budget sharing use `POST /portfolios/list`.

#### Sample Request

```
curl --location --request POST 'https://advertising-api.amazon.com/portfolios/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/vnd.spPortfolio.v3+json' \
--data-raw '{
  "portfolioIdFilter": {
    "include": [
      "190881001"
    ]
  }
}'
```

#### Sample response if portfolio is sharing unspent campaign budget

```
{
    "totalResults": 0,
    "nextToken": "string",
    "portfolios": [{
        "inBudget": true,
        "portfolioId": "190881001",
        "name": "XYZ",
        "state": "ENABLED",
        "budgetControls": {
            "campaignUnspentBudgetSharing": {
                "featureState": "ENABLED"
            }
        },
        "budget": {
            "amount": 50,
            "endDate": "2024-02-15",
            "currencyCode": "USD",
            "startDate": "2024-01-15",
            "policy": "DATE_RANGE"
        },
        "extendedData": {
            "statusReasons": [
                "PORTFOLIO_STATUS_ENABLED_DETAIL"
            ],
            "servingStatus": "PORTFOLIO_STATUS_ENABLED",
            "lastUpdateDate": 0,
            "creationDate": 0
        }
    }]
}
```

#### Sample Response if portfolio is not sharing unspent campaign budget 

```
{
    "totalResults": 0,
    "nextToken": "string",
    "portfolios": [{
        "inBudget": true,
        "portfolioId": "190881001",
        "name": "XYZ",
        "state": "ENABLED",
        "budgetControls": {
            "campaignUnspentBudgetSharing": {
                "featureState": "DISABLED"
            }
        },
        "budget": {
            "amount": 50,
            "endDate": "2024-02-15",
            "currencyCode": "USD",
            "startDate": "2024-01-15",
            "policy": "DATE_RANGE"
        },
        "extendedData": {
            "statusReasons": [
                "PORTFOLIO_STATUS_ENABLED_DETAIL"
            ],
            "servingStatus": "PORTFOLIO_STATUS_ENABLED",
            "lastUpdateDate": 0,
            "creationDate": 0
        }
    }]
}
```

## Learn more

* [API reference](reference/portfolios)

