---
title: Sponsored Brands schedule-based budget rules
description: Learn how to use the Amazon Ads API to set campaign budgets in advance using rules based on a schedule.
type: guide
interface: api 
tags:
    - Sponsored Brands
    - Sponsored Products
    - Sponsored Display
    - Budget Rules
keywords:
    - budget performance
    - budget rule evaluation
    - budget rule recurrence
    - conditional budget increase
    - conditional budget decrease
---

# Schedule-based rules

Scheduled-based rules enable you to increase a campaign’s budget by a certain percentage during a date range. This date range can be based on an upcoming event or can be a custom date range.

Schedule-based budget rules are created using the following parameters:

1.  Rule type: `schedule`
2.  Date range: choose one of the recommended event identifiers - such as Black Friday - or select a custom date range by specifying the start and end date for the active time period of the rule. Note that the recommended event identifier can be retrieved from the recommendation APIs.
3.  Recurrence: choose the rule evaluation frequency.
  * `daily`: the rule is evaluated on a daily basis based on the start and end date.
  * `weekly`: the rule is evaluated only certain days of the week based on the start and end dates. For example, Mondays and Fridays starting on a specified month and day and ending on a specified month and day. Note that this parameter is only available for custom date ranges.
  * `intraDaySchedule`: schedule The rule is evaluated only during certain times of the day based on the start time and end times (when selected). For example, from 5:00PM to 10:00 PM. Note: `intraDaySchedule` is currently only available for campaigns in the following marketplaces: US, CA, UK, IN, JP. 
4.  Increase budget by: a percentage amount by which to increase your daily budget by. For recommended events, the recommendation is a percentage you can apply to your budget rules.  

>[NOTE] Using hours of day for budget rules is currently available only for schedule-based budget rules. To provide reporting on hourly campaign metrics with sufficient data, especially metrics related to conversions, there is a minimum budget required for hours-of-day, schedule-based budget rules to become active. The minimum required budget aims at increasing the likelihood the campaign will have enough traffic and the metrics can be provided.

## Request

You can create performance-based budget rules using the POST /budgetRules endpoints. Note there is a separate endpoint for each ad type.

|Ad type| Create budget rule endpoint |
|----|-----|
| Sponsored Products | [POST /sp/budgetRules](sponsored-products/3-0/openapi/prod#tag/BudgetRules/operation/CreateBudgetRulesForSPCampaigns) |
| Sponsored Brands | [POST /sb/budgetRules](sponsored-brands/3-0/openapi/prod#tag/BudgetRules/operation/CreateBudgetRulesForSBCampaigns) |
| Sponsored Display | [POST /sd/budgetRules](sponsored-display/3-0/openapi/prod#tag/BudgetRules/operation/CreateBudgetRulesForSDCampaigns) |

## Scenarios

### Increasing budget for a custom date range

In this scenario, you want to increase budget for a campaign from November 21st to November 30th by 20% on a daily basis. In this example you're creating a budget rule for a custom date range, which will increase the budget every day by 20%.

To achieve this, call the rule creation API with `recurrence` set to `DAILY` and `dateRange` set to custom with start date set to November 21st and end date set to November 30th. The response includes a rule identifier that you'll use to call the rule association API.

#### Example: Sponsored Products

1. Create a budget rule using POST /sp/budgetRules.

Example request body:

```json
{
  "budgetRulesDetails": [
    {
      "name": "SAMPLE_BUDGET_RULE_DAILY",
      "ruleType": "SCHEDULE",
      "duration": {
        "dateRangeTypeRuleDuration": {
          "startDate": "20201121",
          "endDate": "20201130"
        }
      },
      "recurrence": {
        "type": "DAILY"
      },
      "budgetIncreaseBy": {
        "type": "PERCENT",
        "value": 20
      }
    }
  ]
}
```

Expected HTTP status code: 207

Response body:

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

Example request body:

```json
{
	"budgetRuleIds": ["5bc531de-f000-44cf-8a86-54189bcdd380"]
}
```

Expected HTTP status code: 207

Example response body:

```json
{
    "responses": [
        {
            "code": "Ok",
            "details": "Budget rule associated",
            "ruleId": "5bc531de-f000-44cf-8a86-54189bcdd380"
        }
    ]
}
```

### Increasing budget for a recommended event

>[NOTE] Recommended events are only available for Sponsored Products and Sponsored Brands.

Creating rules for recommended events involves three steps:

1. Call the Budget Rules Event Recommendation API to receive list of events ([Sponsored Brands endpoint](sponsored-brands/3-0/openapi/prod#tag/BudgetRulesRecommendation) and [Sponsored Products endpoint](sponsored-products/3-0/openapi/prod#tag/BudgetRulesRecommendation)). Include the date range for each event and the percentage by which it is recommended that you increase your budget. For events that are available in the list before the dates are announced, the start and end date are empty during rule creation. Later, these rules are automatically updated when the dates are announced.
2. Create a budget rule.
3. Associate the budget rule created in step 2 with a campaign.

#### Example: Sponsored Brands

1. Call the budget rules event recommendation API (POST /sb/campaigns/budgetRules/recommendations).

Example request body:

```json
{
    "campaignId": "148864597212176"
}
```

Example response:

```json
{
  "recommendedBudgetRuleEvents": [
    {
      "eventId": "55583b89-b236-4ccb-ad86-e72f23691eef",
      "eventName": "Halloween",
      "startDate": "20201027",
      "endDate": "20201031",
      "suggestedBudgetIncreasePercent": 20
    }
  ]
}
```

2. Create a budget rule using POST /sb/budgetRules.

Example request body:

```json
{
  "budgetRulesDetails": [
    {
      "name": "SAMPLE_BUDGET_RULE_RECOMMENDED_EVENT",
      "ruleType": "SCHEDULE",
      "duration": {
        "eventTypeRuleDuration": {
          "eventId": "69ce6478-5215-4bd3-8de0-a8eb28ae69a7"
        }
      },
      "recurrence": {
        "type": "DAILY"
      },
      "budgetIncreaseBy": {
        "type": "PERCENT",
        "value": 20
      }
    }
  ]
}
```

Expected HTTP status code: 207

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

3. Associate the budget rule created in step 2 with a campaign using POST /sb/campaigns/{campaignId}/budgetRules.

### Increasing budgets on Fridays, Saturdays, and Sundays with no end date

You can set up this rule by calling the rule creation API with recurrence set to weekly and day of week set to Friday, Saturday, and Sunday. 

#### Example: Sponsored Display

1. Call POST /sd/budgetRules to set up the rule. 

Example request body:

```json
{
  "budgetRulesDetails": [
    {
      "name": "SAMPLE_BUDGET_RULE_WEEKLY",
      "ruleType": "SCHEDULE",
      "duration": {
        "dateRangeTypeRuleDuration": {
          "startDate": "20200901"
        }
      },
      "recurrence": {
        "type": "weekly",
        "daysOfWeek": [
          "FRIDAY",
          "SATURDAY",
          "SUNDAY"
        ]
      },
      "budgetIncreaseBy": {
        "type": "PERCENT",
        "value": 20
      }
    }
  ]
}
```

Expected HTTP status code: 207

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

2. Associate the budget rule created in step 1 with a campaign using POST /sd/campaigns/{campaignId}/budgetRules.

### Increasing the budget from 5:00PM to 10:00PM on a daily basis with no end date

Call the rule creation API with recurrence set to daily and IntraDaySchedule set with start time of 17:00 and end time of 22:00.

#### Example: Sponsored Products

```bash
curl --location 'https://advertising-api.amazon.com/sp/budgetRules' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--data '{
  "budgetRulesDetails": [
    {
        "name": "IntraDayAdditive",
        "ruleType": "SCHEDULE",
        "duration": {
            "dateRangeTypeRuleDuration": {
                "startDate": "20231120",
                "endDate": "20231220"
            }
        },
        "recurrence": {
            "type": "WEEKLY",
            "daysOfWeek": [
                "MONDAY"
            ],
            "intraDaySchedule": [
                {
                    "startTime": "17:00:00",
                    "endTime": "22:00:00"
                }
            ]
        },
        "budgetIncreaseBy": {
            "type": "PERCENT",
            "value": 3.14159
        }
    }
  ]
}'
```