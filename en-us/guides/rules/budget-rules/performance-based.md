---
title: Performance-based rules 
description: Learn how to use the Amazon Ads API to set campaign budgets in advance using rules based on campaign performance.
type: guide
interface: api 
tags:
    - Sponsored Brands
    - Sponsored Products
    - Sponsored Display
    - Budget Rules
keywords:
    - budget
    - budget performance
    - budget rule evaluation
    - budget rule recurrence
    - conditional budget increase
    - conditional budget decrease
    - budget performance metrics
---

# Performance-based rules 

Performance-based rules enable you to create budget rules that increase your budget when a specified condition is met. For example, you could create a rule to increase your budget when top-of-search impression share (IS) is greater than 50%. 

## Available metrics by ad type

Each ad type supports different metrics that you can use to configure performance-based rules.

| Metric | Description | Supported ad types |
|---------|---------|
| `ACOS` | the percent of attributed sales spent on advertising within the specified time period for a type of campaign due to clicks on your ads. Calculated by dividing total spend by attributed sales. | Sponsored Products, Sponsored Display |
| `CTR` | The click-through rate for the campaign. Calculated by dividing the number of clicks by the number of impressions in the time period. | Sponsored Products, Sponsored Display |
| `CVR` | the conversion rate. Calculated by dividing the number of conversions attributed to clicks in the time period by the clicks that occurred in the time period. | Sponsored Products, Sponsored Display |
| `IS` | Top-of-search impression share for a campaign. Calculated as the percentage of top-of-search impressions a campaign received out of the total top-of-search impressions it was eligible to serve on. Note: `IS` is not available as a metric for any campaign that contains a video creative. | Sponsored Brands |
| ` NTB` | Percent of sales in local currency derived from new-to-brand customers | Sponsored Brands |
| `ROAS` | Return on ad spend. Amazon RoAS is calculated by dividing ad revenue by ad spend, and it tells you how much money you earn for every dollar you invest in advertising. | Sponsored Brands, Sponsored Products, Sponsored Display |

>[NOTE] For sellers, the time period used for metric calculation is within 7 days of an ad click. For vendors, metrics are calculated within 14 days of an ad click.

## Request

You can create performance-based budget rules using the POST /budgetRules endpoints. Note there is a separate endpoint for each ad type.

|Ad type| Create budget rule endpoint |
|----|-----|
| Sponsored Products | [POST /sp/budgetRules](sponsored-products/3-0/openapi/prod#tag/BudgetRules/operation/CreateBudgetRulesForSPCampaigns) |
| Sponsored Brands | [POST /sb/budgetRules](sponsored-brands/3-0/openapi/prod#tag/BudgetRules/operation/CreateBudgetRulesForSBCampaigns) |
| Sponsored Display | [POST /sd/budgetRules](sponsored-display/3-0/openapi/prod#tag/BudgetRules/operation/CreateBudgetRulesForSDCampaigns) |

### Parameters

1. Rule type: `performance`
2. Start date: The start date of the rule.
3. End date: This parameter is optional, but it's the end date of the rule.
4. Recurrence:
    * `daily`: the rule is evaluated on a daily basis based on the start and end date.
    * `weekly`: the rule is evaluated on only certain days of the week such as Fridays and Saturdays between the start and end dates.
5. Performance measure: for performance-based rules, you must specify the following properties:

    * Metric name: See [Available metrics by ad type](guides/rules/budget-rules/performance-based#available-metrics-by-ad-type) to understand which metrics are available for each ad type.
    * Threshold: The numeric value of chosen metric. For example, if the metric name is `NTB` and threshold is set to 50, this represents a percent of sales new-to-brand of 50%. 
    * Comparison operator: one of `LESS_THAN`, `GREATER_THAN`, `EQUAL_TO`, `LESS_THAN_OR_EQUAL_TO`, of `GREATER_THAN_OR_EQUAL_TO`.

6. Increase budget by: percentage amount by which to increase daily budget.

## Scenarios

### Sponsored Brands: Increase the budget by 10% if ROAS is less than 20%.

To achieve this outcome, create a rule by specifying ROAS for the metric name and set 10% as threshold for performance measure. Next, associate the rule to a campaign.

1. Create a rule by specifying ROAS for the metric name and set 10% as the threshold using POST /sb/budgetRules.

Example request body:

```json
{
  "budgetRulesDetails": [
    {
      "name": "SAMPLE_BUDGET_RULE_ROAS",
      "ruleType": "PERFORMANCE",
      "duration": {
        "dateRangeTypeRuleDuration": {
          "startDate": "20201121"
        }
      },
      "recurrence": {
        "type": "DAILY"
      },
      "budgetIncreaseBy": {
        "type": "PERCENT",
        "value": 10
      },
      "performanceMeasureCondition": {
        "metricName": "ROAS",
        "threshold": 20,
        "comparisonOperator": "LESS_THAN_OR_EQUAL_TO"
      }
    }
  ]
}
```

Expected HTTP Status code: 207

Example response body:

```json
{
    "responses": [
        {
            "code": "Ok",
            "details": "Budget rule created",
            "ruleId": "5bc531de-f000-44cf-8a86-54189bcdd380"
        }
    ]
}
```

2. Associate the budget rule to a campaign using POST /sb/campaigns/{campaignId}/budgetRules.

### Sponsored Products: Increase the budget by 10% if ACOS is less than 20%.

To achieve this outcome, create a rule by specifying ACOS for the metric name and set 10% as threshold for performance measure. Next, associate the rule to a campaign.

1. Create a rule by specifying ACOS for the metric name and set 10% as the threshold using POST /sp/budgetRules.

Example request body:

```json
{
  "budgetRulesDetails": [
    {
      "name": "SAMPLE_BUDGET_RULE_ACOS",
      "ruleType": "PERFORMANCE",
      "duration": {
        "dateRangeTypeRuleDuration": {
          "startDate": "20201121"
        }
      },
      "recurrence": {
        "type": "DAILY"
      },
      "budgetIncreaseBy": {
        "type": "PERCENT",
        "value": 10
      },
      "performanceMeasureCondition": {
        "metricName": "ACOS",
        "threshold": 20,
        "comparisonOperator": "LESS_THAN_OR_EQUAL_TO"
      }
    }
  ]
}
```

Expected HTTP Status code: 207

Example response body:

```json
{
    "responses": [
        {
            "code": "Ok",
            "details": "Budget rule created",
            "ruleId": "5bc531de-f000-44cf-8a86-54189bcdd380"
        }
    ]
}
```

2. Associate the budget rule to a campaign using POST /sp/campaigns/{campaignId}/budgetRules.

### Sponsored Display: Increase the budget by 10% if ACOS is less than 20%.

To achieve this outcome, create a rule by specifying ACOS for the metric name and set 10% as threshold for performance measure. Next, associate the rule to a campaign.

1. Create a rule by specifying ACOS for the metric name and set 10% as the threshold using POST /sd/budgetRules.

Example request body:

```json
{
  "budgetRulesDetails": [
    {
      "name": "SAMPLE_BUDGET_RULE_ACOS",
      "ruleType": "PERFORMANCE",
      "duration": {
        "dateRangeTypeRuleDuration": {
          "startDate": "20201121"
        }
      },
      "recurrence": {
        "type": "DAILY"
      },
      "budgetIncreaseBy": {
        "type": "PERCENT",
        "value": 10
      },
      "performanceMeasureCondition": {
        "metricName": "ACOS",
        "threshold": 20,
        "comparisonOperator": "LESS_THAN_OR_EQUAL_TO"
      }
    }
  ]
}
```

Expected HTTP Status code: 207

Example response body:

```json
{
    "responses": [
        {
            "code": "Ok",
            "details": "Budget rule created",
            "ruleId": "5bc531de-f000-44cf-8a86-54189bcdd380"
        }
    ]
}
```

2. Associate the budget rule to a campaign using POST /sd/campaigns/{campaignId}/budgetRules.

## Minimum budgets

Performance rules rely on having clicks and conversions on a campaign so the metrics can be generated. The minimum required budget aims at increasing the likelihood the campaign will have enough traffic and the metrics can be evaluated.