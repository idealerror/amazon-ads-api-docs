---
title: Viewing changes to daily budgets for campaigns with budget rules
description: Learn how to determine if an Amazon Ads campaign has an active budget rule, and how to retrieve the budget rule if it exists.
type: guide
interface: api 
tags:
   	- Sponsored Brands
    - Sponsored Products
    - Sponsored Display
    - Budget Rules
keywords:
    - active rule
    - rule history
    - budget
---

# Viewing changes to daily budgets for campaigns with budget rules 

You can view the budget for campaigns with budget rules by using the list campaign resource for each supported ad type. This process is the same for Sponsored Brands, Sponsored Display, and Sponsored Products Version 2, but differs for Sponsored Products Version 3. 

## Sponsored Products Version 3

In version 3, if a campaign's budget has been impacted by a budget rule, you will see an `effectiveBudget` field included in the campaign `budget` object returned by [POST /sp/campaigns/list](sponsored-products/3-0/openapi/prod#tag/Campaigns/operation/ListSponsoredProductsCampaigns).  

You can use `effectiveBudget` as an indicator that budget rules have been applied, then retrieve the full budget rules history for that campaign using [GET /sp/campaigns/{campaignId}/budgetRules](sponsored-products/3-0/openapi/prod#tag/BudgetRules/operation/ListAssociatedBudgetRulesForSPCampaigns). Budget rules with an `ACTIVE` status are acting on the campaign.

## Sponsored Brands, Sponsored Display, and Sponsored Products Version 2

You can view the budget for campaigns with budget rules by using the list campaign resource associated to that ad type.

| Ad type | List campaigns endpoint |
|------|-------|
| Sponsored Products (V2) | [GET v2/sp/campaigns](sponsored-products/2-0/openapi#tag/Campaigns/operation/listCampaigns)|
| Sponsored Brands | [POST /sb/v4/campaigns/list](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/ListSponsoredBrandsCampaigns)  |
| Sponsored Display | [GET /sd/campaigns](sponsored-display/3-0/openapi#tag/Campaigns/operation/listCampaigns)|

The presence of `ruleBasedBudget` in the response, indicates that budget rules have been applied. You can then retrieve the full budget rules history for that campaign using the relevant GET /campaigns/{campaignId}/budgetRules endpoint for that ad type. Budget rules with an `ACTIVE` status are acting on the campaign. 

| Ad type | GET budget rules endpoint |
|------|-------|
| Sponsored Products | [GET /sp/campaigns/{campaignId}/budgetRules](sponsored-products/3-0/openapi/prod#tag/BudgetRules/operation/ListAssociatedBudgetRulesForSPCampaigns)|
| Sponsored Brands | [GET /sb/campaigns/{campaignId}/budgetRules](sponsored-brands/3-0/openapi/prod#tag/BudgetRules/operation/getRuleBasedBudgetHistoryForSBCampaigns)  |
| Sponsored Display | [GET /sd/campaigns/{campaignId}/budgetRules](sponsored-display/3-0/openapi/prod#tag/BudgetRules/operation/ListAssociatedBudgetRulesForSDCampaigns)|

>[WARNING] As of February 2023, the `applicableRuleId` can be incomplete since multiple rules could be active at the same time (see [rule evaluation details](guides/rules/budget-rules/rule-evaluation)), so you should use relevant GET /campaigns/{campaignId}/budgetRules endpoint to see active rules on a campaign.

### Rule evaluation progress

The `ruleBasedBudget` object also includes the `isProcessing` field. If this field is set to `true`, rule evaluation is in progress. Once evaluation is complete, the rule budget value is updated, and the `isProcessing` is set to `false`.

## Example 

This example is for a Sponsored Brands campaign with budget rules applied. 

```bash
GET /sb/campaigns/{campaignId}
```

While rule evaluation is in progress, the response may resemble: 

```JSON
Scenario 2: When budget rule evaluation is in progress
{
    		"campaignId": 148864597212176,
    		"name": "Sample Budget Rule Campaign",
    		"campaignType": "sponsoredProducts",
    		"targetingType": "manual",
    		"premiumBidAdjustment": false,
    		"dailyBudget": 10.0,
    		"ruleBasedBudget": {
        		"isProcessing": true
    		},
    		"startDate": "20181010",
    		"state": "enabled",
    		"bidding": {
        		"adjustments": []
    		}
}
```

When rule evaluation is complete, the response may resemble:

```JSON
{
    		"campaignId": 148864597212176,
"name": "Sample Budget Rule Campaign",
    		"campaignType": "sponsoredProducts",
    		"targetingType": "manual",
    		"premiumBidAdjustment": false,
    		"dailyBudget": 10.0,
    		"ruleBasedBudget": {
        		"value": 12.0,
        		"applicableRuleId": "5bc531de-f000-44cf-8a86-54189bcdd380",
        		"applicableRuleName": "SAMPLE_BUDGET_RULE",
        		"isProcessing": false
    	},
    	"startDate": "20181010",
    	"state": "enabled",
    	"bidding": {
       	"adjustments": []
    		}
}
```