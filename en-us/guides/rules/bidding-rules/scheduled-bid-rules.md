---
title: Scheduled bid rules
description: Amazon Ads API version 2 Sponsored Products scheduled bid rules
type: guide
interface: api
tags:
    - Scheduled bid rules
    - Optimization rules
    - rules
---

# Schedule Bid Rules

Schedule bid rules allow advertisers to automatically increase bids at specific times of the day, days of the week, date ranges, or during high-traffic events like Black Friday. To set up a schedule bid rule, advertisers must first create an optimization rule using the POST [/sp/rules/optimization endpoint](sponsored-products/3-0/openapi/prod#tag/Optimization-Rules/operation/CreateOptimizationRules), which defines the conditions and bid adjustment for the rule. After creating the optimization rule, the `optimizationRuleId` can be used to associate the rule with a specific campaign through the POST [/sp/campaigns/{campaignId}/optimizationRules](sponsored-products/3-0/openapi/prod#tag/Optimization-Rules/operation/AssociateOptimizationRulesToCampaign) endpoint. The bid is increased according to the rule's configuration when the defined conditions are met. However, if the conditions are not met, no bid increase is applied.

## Create an optimization rule

Consider a campaign “Running Shoes for Q4,” which has the following rules set: 

1. Daily rule - Increase bids by 10% with no end date.
2. Black Friday rule - Increase bids by 20% on Black Friday.
3. 4 hour rule - Increase bids by 20% between 5:00PM and 9:00PM with no end date.

On Black Friday, we will implement a 30% daily bid increase for that day. This includes the 10% increase from the daily rule whose conditions were met, and 20% increase from the Black Friday rule. The campaign will get an additional 20% increase from the 4 hour rule increasing the bid total to 50%. After 9:00PM, the daily bid increase will be back to 30%.

>[NOTE] Whenever a new schedule bid rule is created or an existing rule is updated, the changes will take effect within 24 hours. A maximum of 20 schedule bid rules can be specified for a campaign. Schedule bid rules are applied on top of ‘Placement adjustment’ and ‘Bidding strategies’.

For the example below, the [POST /sp/rules/optimization](sponsored-products/3-0/openapi/prod#tag/Optimization-Rules/operation/CreateOptimizationRules) endpoint is called with `recurrence` set to `DAILY` and `timesOfDay` set with start time of 5:00 and end time of 13:00.


```json
curl --location --request POST 'https://advertising-api.amazon.com/sp/rules/optimization' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{  
    "optimizationRules": 
    [
        { "ruleName": "increase_bids_by_15%_on_mornings",      
          "ruleCategory": "BID",      
          "ruleSubCategory": "SCHEDULE",      
          "recurrence": 
            {"type": "DAILY",        
             "timesOfDay": 
             [          
                {"startTime": "05:00",            
                 "endTime": "13:00"}
             ],        
             "duration": 
                {"startTime": "2023-08-01T00:00:00Z"}
             },      
          "action": 
                {"actionType": "ADOPT",        
                 "actionDetails": 
                    {"actionOperator": "INCREMENT",          
                     "value": 15,          
                     "actionUnit": "PERCENT"}
                 },      
          "status": "ENABLED"}
    ]
}'
```

A successful response includes the `optimizationRuleId`, which is used to associate an optimization rule to a campaign. See [associate optimization rule to a campaign](guides/rules/bidding-rules/scheduled-bid-rules#associate-optimization-rule-to-a-campaign) for more details. 


```json
{
    "code": "207",
    "responses": [
        {
            "code": "200",
            "details": "Successfully created rule",
            "optimizationRule": {
                "ruleName": "increase_bids_by_15%_on_mornings",
                "ruleCategory": "BID",
                "ruleSubCategory": "SCHEDULE",
                "recurrence": {
                    "type": "DAILY",
                    "daysOfWeek": null,
                    "timesOfDay": [
                        {
                            "startTime": "05:00",
                            "endTime": "13:00"
                        }
                    ],
                    "duration": {
                        "startTime": "2024-08-01T00:00:00Z",
                        "endTime": null
                    }
                },
                "conditions": null,
                "action": {
                    "actionType": "ADOPT",
                    "actionDetails": {
                        "actionOperator": "INCREMENT",
                        "value": 15.0,
                        "actionUnit": "PERCENT"
                    }
                },
                "status": "SCHEDULED",
                "optimizationRuleId": "amzn1.ads-rule.m.xxxxxx"
            }
        }
    ]
}
```

### Scenario examples

**Scenario 1:** You want to increase the budget for the whole day, only on weekends, with no end date. 

**Solution:** Call the [POST /sp/rules/optimization](sponsored-products/3-0/openapi/prod#tag/Optimization-Rules/operation/CreateOptimizationRules) endpoint with `recurrence` set to `WEEKLY` and `daysOfWeek` set to `SATURDAY` and `SUNDAY`, with no filter on times of day.


```json
curl --location --request POST 'https://advertising-api.amazon.com/sp/rules/optimization' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{  
    "optimizationRules": 
    [
        { "ruleName": "increase_bids_by_20%_on_weekends",      
          "ruleCategory": "BID",      
          "ruleSubCategory": "SCHEDULE",      
          "recurrence": 
            {"type": "WEEKLY", 
             "daysOfWeek":
             [
                "SATURDAY",
                "SUNDAY"
             ],
             "duration": 
                {"startTime": "2023-08-01T00:00:00Z"}
             },      
          "action": 
                {"actionType": "ADOPT",        
                 "actionDetails": 
                    {"actionOperator": "INCREMENT",          
                     "value": 20,          
                     "actionUnit": "PERCENT"}
                 },      
          "status": "ENABLED"}
    ]
}'
```


**Scenario 2:** You want to increase the budget from 10:00PM to 11:00PM, only on Wednesday, but with the rule ending in a year. 

**Solution:** Call the [POST /sp/rules/optimization](sponsored-products/3-0/openapi/prod#tag/Optimization-Rules/operation/CreateOptimizationRules) endpoint with `recurrence` set to `weekly`, `daysOfWeek` set to `WEDNESDAY`, `timesOfDay` set with start time of 22:00 and end time of 23:00, and an duration end time of 08/01/2024.


```json
curl --location --request POST 'https://advertising-api.amazon.com/sp/rules/optimization' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{  
    "optimizationRules": 
    [
        { "ruleName": "increase_bids_by_30%_on_wednesday_night",      
          "ruleCategory": "BID",      
          "ruleSubCategory": "SCHEDULE",      
          "recurrence": 
            {"type": "WEEKLY", 
             "daysOfWeek":
             [
                "WEDNESDAY"
             ],
             "timesOfDay": 
             [          
                {"startTime": "22:00",            
                 "endTime": "23:00"}
             ], 
             "duration": 
                {"startTime": "2023-08-01T00:00:00Z",
                 "endTime": "2024-08-01T00:00:00Z"}
             },      
          "action": 
                {"actionType": "ADOPT",        
                "actionDetails": 
                    {"actionOperator": "INCREMENT",          
                     "value": 30,          
                     "actionUnit": "PERCENT"}
                 },      
          "status": "ENABLED"}
    ]
}'
```


**Scenario 3:** You want to change the already-created above rule in scenario 3 to only modulate bids by 15% instead of 30%.

**Solution:** Call the [PUT /sp/rules/optimization](sponsored-products/3-0/openapi/prod#tag/Optimization-Rules/operation/UpdateOptimizationRules) endpoint with the same fields as the scenario 3 rule, but change the `action.actionDetails.value`to 15.


```
curl --location --request PUT 'https://advertising-api.amazon.com/sp/rules/optimization' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{  
    "optimizationRules": 
    [
        { "optimizationRuleId" : "amzn1.ads-rule.m.xxxxxx",
          "ruleName": "increase_bids_by_30%_on_wednesday_night",      
          "ruleCategory": "BID",      
          "ruleSubCategory": "SCHEDULE",      
          "recurrence": 
            {"type": "WEEKLY", 
             "daysOfWeek":
             [
                "WEDNESDAY"
             ],
             "timesOfDay": 
             [          
                {"startTime": "22:00",            
                 "endTime": "23:00"}
             ], 
             "duration": 
                {"startTime": "2023-08-01T00:00:00Z",
                 "endTime": "2024-08-01T00:00:00Z"}
             },      
          "action": 
                {"actionType": "ADOPT",        
                "actionDetails": 
                    {"actionOperator": "INCREMENT",          
                     "value": 15,          
                     "actionUnit": "PERCENT"}
                 },      
          "status": "ENABLED"}
    ]
}'
```

**Scenario 4:** You want to create a rule that increases the bid by 20% during the Black Friday event.

**Solution:** Call the [POST /sp/rules/optimization](sponsored-products/3-0/openapi/prod#tag/Optimization-Rules/operation/UpdateOptimizationRules) endpoint with the duration field containing the event name and event Id. Event details can be obtained from the [POST /sp/v1/events](sponsored-products/3-0/openapi/prod#tag/BudgetRulesRecommendation/operation/SPGetAllRuleEvents) endpoint.


```
curl --location --request POST 'https://advertising-api.amazon.com/sp/rules/optimization' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{
    "optimizationRules": [{
        "recurrence": {
            "duration": {
                "startTime": "2024-11-21T00:00:00Z",
                "endTime": "2024-11-29T00:00:00Z",
                "eventId": "183491cd-3d76-5171-b347-0e36284a342d",
                "eventName": "Black Friday"
            },
            "timesOfDay": [{
                "startTime": "08:00",
                "endTime": "21:00"
            }],
            "type": "DAILY"
        },
        "ruleCategory": "BID",
        "ruleSubCategory": "SCHEDULE",
        "ruleName": "increase_bids_by_20%_during_black_friday_event",
        "action": {
            "actionType": "ADOPT",
            "actionDetails": {
                "actionUnit": "PERCENT",
                "value": "20",
                "actionOperator": "INCREMENT"
            }
        },
        "status": "ENABLED"
    }]
}'
```


## Associate optimization rule to a campaign

Using the POST [/sp/campaigns/{campaignId}/optimizationRules](sponsored-products/3-0/openapi/prod#tag/Optimization-Rules/operation/AssociateOptimizationRulesToCampaign) endpoint, you can associate a optimization rule to a campaign. You must use the `optimizationRuleId` obtained from creating an optimization rule.

**Example request:**

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/campaigns/{campaignId}/optimizationRules' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: 3440516497411847' \
--header 'Accept: application/vnd.spoptimizationrules.v1+json' \
--header 'Content-Type: application/json' \
--data-raw '{
  "optimizationRuleIds": [
    "amzn1.ads-rule.m.xxxxxx"
  ]
}'
```

A successful response will include a message confirming that the optimization rule was associated with a campaign. 

```json
{
    "code": "207",
    "responses": [
        {
            "code": "200",
            "details": "Successfully associated rule with id amzn1.ads-rule.m.1xv0wgtlnj39odlbdp8098met to campaign with id 543852220259845",
            "optimizationRuleId": "amzn1.ads-rule.m.1xv0wgtlnj39odlbdp8098met"
        }
    ]
}
```

## Search and retrieve existing optimization rules

Using the POST [/sp/rules/optimization/search](sponsored-products/3-0/openapi/prod#tag/Optimization-Rules/operation/SearchOptimizationRules) endpoint, you can search for and retrieve existing optimization rules. This endpoint is useful especially if you are looking for optimization rules to associate to a specific campaign. 

Example request:

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/rules/optimization/search' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Accept: application/vnd.spoptimizationrules.v1+json' \
--header 'Content-Type: application/json' \
--data-raw '{
  "optimizationRuleFilter": {
    "optimizationRuleId": {
      "values": [
        "amzn1.ads-rule.m.1xv0wgtlnj39odlbdp8098met"
      ],
      "filterType": "EXACT_MATCH"
    }
  }
}'
```

Response:

```json
{
    "code": "200",
    "optimizationRules": [
        {
            "ruleName": "increase_bids_by_15%_on_mornings",
            "ruleCategory": "BID",
            "ruleSubCategory": "SCHEDULE",
            "recurrence": {
                "type": "DAILY",
                "daysOfWeek": null,
                "timesOfDay": [
                    {
                        "startTime": "05:00",
                        "endTime": "13:00"
                    }
                ],
                "duration": {
                    "startTime": "2024-08-01T00:00:00Z",
                    "endTime": null
                }
            },
            "conditions": null,
            "action": {
                "actionType": "ADOPT",
                "actionDetails": {
                    "actionOperator": "INCREMENT",
                    "value": 15.0,
                    "actionUnit": "PERCENT"
                }
            },
            "status": "SCHEDULED",
            "optimizationRuleId": "amzn1.ads-rule.m.1xv0wgtlnj39odlbdp8098met"
        }
    ],
    "nextToken": null
}
```
