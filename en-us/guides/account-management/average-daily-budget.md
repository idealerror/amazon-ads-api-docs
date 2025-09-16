---
title: Average daily budget user guide
description: User guide for setting up average daily budget for an advertising account
type: guide
interface: api
tags:
    - Campaign management
keywords:
    - budget increase
    - isOptedOutForAverageDailyBudgetIncrease
---

# Average daily budget user guide

## Overview

Daily budget is the amount that you're willing to spend on a campaign each day. The daily budget amount is averaged over the course of a calendar month. On any given day you could spend less than your daily budget. Average daily budget allows you spend more efficiently and take advantage of high traffic days, so that your campaign can spend unused budget on a subsequent day in a calendar month. 

At the end of the month, you won't spend more than the daily budget you’ve set, multiplied by the number of days in that month. Your invoice will be adjusted for any over-delivery, so that you won't be charged for any amount in excess of your monthly charging limit. For example, if your budget is $100 and you receive $90 worth of clicks on the first day, you could receive up to $110 worth of clicks on the second day. This would bring your total spend over two days to $200, which averages to $100 per day.

>[NOTE] The default average daily budget setting is 100% of the daily budget. If you want to opt out of the 100% increase setting, you can revert to 25% using the [POST /accountBudgets/featureFlags endpoint](account-budgets#tag/Advertiser/operation/updateAccountBudgetFeatureFlags). This will set the default average daily budget increase amount to 25% for all sponsored ads campaigns associated to your profile. If you have previously set 25% as your average daily budget setting, you can also choose to opt in to 100% daily budget increase for all campaigns using the [POST /accountBudgets/featureFlags](account-budgets#tag/Advertiser/operation/updateAccountBudgetFeatureFlags) endpoint.

## Getting started

### Setting average daily budget

You can set the average daily budget increase amount at either 25% or 100% of campaign budget. You select your choice at the profile level, so all sponsored ads campaigns are set at the same amount. 

By default, all campaigns have a 100% average daily budget increase amount. To opt-out of 100% increases, and set your average daily budget increase settings to 25% more than daily budget, use the [POST /accountBudgets/featureFlags](account-budgets#tag/Advertiser/operation/updateAccountBudgetFeatureFlags) endpoint and set `isOptedOutForAverageDailyBudgetIncrease` to `true`.

Sample request body:

```json
{
  "featureFlags": {
    "isOptedOutForAverageDailyBudgetIncrease": true
  }
}
```

Sample response:

```json
{
    "code": "OK",
    "details": "Successfully updated account budget feature flags"
}
```

If you previously set 25% average daily budget increase amount for all campaigns, but now would like to switch to 100%,  use [POST /accountBudgets/featureFlags](account-budgets#tag/Advertiser/operation/updateAccountBudgetFeatureFlags) and set `isOptedOutForAverageDailyBudgetIncrease` to `false`.

```json
{
  "featureFlags": {
    "isOptedOutForAverageDailyBudgetIncrease": false
  }
}
```

Sample response:

```json
{
    "code": "OK",
    "details": "Successfully updated account budget feature flags"
}
```

### Checking average daily budget settings

To show your current average daily budget increase settings, use [GET /accountBudgets/featureFlags](account-budgets#tag/Advertiser/operation/getAccountBudgetFeatureFlags).

Sample response if opted in:

```json
{
  "featureFlags": {
    "isOptedOutForAverageDailyBudgetIncrease": false
  }
}
```

Sample response if opted out:

```json
{
  "featureFlags": {
    "isOptedOutForAverageDailyBudgetIncrease": true
  }
}
```