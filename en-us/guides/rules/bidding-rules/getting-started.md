---
title: Getting started with the Sponsored Products rule-based bidding API
description: Quickstart guide that walks a user through the process of using rule-based bidding for Sponsored Products
type: guide
interface: api 
tags:
    - Sponsored Products
keywords:
    - rule-based bidding
    - rules
    - rule
---

# Getting started with the Sponsored Products rule-based bidding API

Rule-based bids are represented by the [campaignOptimization](sponsored-products/3-0/openapi/prod#tag/Campaign-Optimization-Rules/operation/CreateOptimizationRule) resource in the [Sponsored Products API](sponsored-products/3-0/openapi/prod).

The API includes operations to create, read, update, and delete `campaignOptimization` resources. You can also query the **state** of a `campaignOptimization` resource, determine campaign eligibility, and get a ROAS guardrail.

## Create a rule

You can create a rule by setting the theme to “increase conversations while maintaining a ROAS guardrail”. You can set the ROAS guardrail in your local currency that can be applied across multiple campaigns. You should then associate the rule to a set of campaigns.

```bash
POST /sp/rules/campaignOptimization
```

Request body: 

```JSON
{
    "recurrence": "DAILY",
    "ruleAction": "ADOPT",
    "ruleCondition": [
        {
            "metricName": "ROAS",
            "comparisonOperator": "GREATER_THAN",
            "threshold": "4"
        }
    ],
    "ruleType": "BID",
    "ruleName": "RuleROAS4",
    "campaignIds": [
        "123784",
        "1223785"
    ]
}
```

Response Body:

```JSON
{
    "campaignOptimizationId": "string",
    "code": "string",
    "details": "string"
}
```

> [NOTE] The recurrence property currently supports the `DAILY` value only. The `ruleAction` property currently supports the `ADOPT` value only. While `comparisonOperator` is an enumeration with multiple values, if `metricName` is set to `ROAS` the supported values are `GREATER_THAN` or `EQUAL_TO`.

## Retrieve a Rule

To retrieve a rule by identifier, use the `GET` operation specifying the identifier in the path:

```bash
GET /sp/rules/campaignOptimization/{campaignOptimizationId}/
```

Response Body:

```JSON
{
    "CampaignOptimizationRule": {
        "recurrence": "DAILY",
        "ruleAction": "ADOPT",
        "campaignOptimizationId": "string",
        "createdDate": "2021-04-20T01:09:04.505Z",
        "ruleCondition": [
            {
                "metricName": "ROAS",
                "comparisonOperator": "EQUAL",
                "threshold": "3",
            }
        ],
        "ruleType": "BID",
        "ruleName": "string",
        "campaignIds": [
            "string"
        ],
        "ruleStatus": "ACTIVE"
    }
}
```

> [NOTE] The 'ruleStatus' property is an enumeration that currently supports two values only: `[ACTIVE, ARCHIVED]`.

## Rule Update

To update a rule, use the `PUT` operation:

```bash
PUT /sp/rules/campaignOptimization
```

Request body:

```JSON
{
    "recurrence": "DAILY",
    "ruleAction": "ADOPT",
    "campaignOptimizationId": "10001",
    "ruleCondition": [
        {
        "metricName": "ROAS",
        "comparisonOperator": " EQUALS ",
        "threshold": "3"
        }
    ],
    "ruleType": "BID",
    "ruleName": "RuleROAS4",
    "campaignIds": [
        "123784",
        "1223785"
    ]
}
```

Response body:

```JSON
{
    "campaignOptimizationId": "string",
    "code": "string",
    "details": "string"
}
```

## Rule Deletion

To delete a rule specified by identifier, use the `DELETE` operation specifying the identifier in the path:

```bash
DELETE /sp/rules/campaignOptimization/{campaignOptimizationId}/
```

Response body:

```JSON
{
    "campaignOptimizationId": "string",
    "code": "string",
    "details": "string"
}
```

## State of the Rule

You can query the state of a rule associated with a campaign by specifying a list of `campaignIds` in the request body of the `campaignOptimation/state` resource:

```bash
POST /sp/rules/campaignOptimization/state
```

Request body:

```JSON
{
    "campaignIds": [
        "string"
    ]
}
```

Response body:

```JSON
{
    "CampaignOptimizationRecommendationsError": [
        {
            "campaignId": "string",
            "Error": {
                "code": "string",
                "details": "string"
            }
        }
    ],
    "CampaignOptimizationNotifications": [
        {
        "ruleState": "ENABLED",
        "campaignOptimizationId": "string",
        "campaignId": "string",
        "notificationString": "string"
        }
    ]
}
```

> [NOTE] We recommend you execute this query a minimum of once per day.

## Eligibility

To determine whether a campaign is eligible for rule-based bidding or not, use the `POST` operation to get the `campaignOptimization/eligibility` resource. If a campaign is eligible, the response includes a ROAS and bid guardrail guidelines for given a campaign identifier. The response data is based on the historical campaign performance of advertisers similar to you. 

```bash
POST /sp/rules/campaignOptimization/eligibility
```

Request body:

```JSON
{
    "campaignIds": [
        "string"
    ]
}
```

Response body:

```JSON
{
    "CampaignOptimizationRecommendations": [
        {
            "campaignId": "string",
            "performanceMetrics": {
                "roas": 0
            }
        }
    ],
    "CampaignOptimizationRecommendationsError": [
        {
            "campaignId": "string",
            "Error": {
                "code": "string",
                "details": "string"
            }
        }
    ]
}
```
