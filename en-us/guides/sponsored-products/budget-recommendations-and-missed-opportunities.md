---
title: Budget recommendations & missed opportunities 
description: Amazon Ads API version 3 Sponsored Products Budget recommendations & missed opportunities reference.
type: guide
interface: api 
tags:
    - Sponsored Products
    - Campaign management
keywords:
    - Budget recommendations
    - Missed opportunities
---

# Budget recommendations and missed opportunities

Budget recommendations resource gets you the recommended daily budget and estimated missed opportunities metrics for campaigns.

This resource returns the following metrics: 

1. **Recommended daily budget -** Estimated budget needed to keep the campaign in budget for the full 24-hour period. Consider this budget to minimize your campaign's chances of running out of budget.
2. **Percent time in budget** - The share of time the campaign was in budget during the past seven days.
3. **Estimated missed impressions, clicks and sales for all campaigns** - These are the estimated additional impressions, clicks and sales the campaign might have generated had it adopted the recommended budget. These are estimates based on previous website traffic and historical campaign performance, not a guarantee of actual impressions, clicks, and sales.

## Before you begin

We recommend waiting until a campaign has been active nine days to use this endpoint.

## Endpoint

[POST sp/campaigns/budgetRecommendations](sponsored-products/3-0/openapi/prod#tag/Budget-recommendations-and-missed-opportunities/operation/getBudgetRecommendations)

## Request

A list of campaign IDs is needed for the request body. 

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/campaigns/budgetRecommendations' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer xxxxxxxx' \
--header 'Accept: application/vnd.budgetrecommendation.v3+json' \
--header 'Content-Type: application/vnd.budgetrecommendation.v3+json' \
--data-raw '{
  "campaignIds": [
    "153839444046652"
  ]
}'
```




## Response

**Scenario 1:** A successful response with a suggested budget as well as the estimated missed impressions, clicks, and sales. In the `budgetRuleRecommendation` object, you can see the suggested budget increase percentage for a specific budget rule. This object will only be populated if at least one budget rule has been created. 

```
{
  "budgetRecommendationsSuccessResults": [
    {
      "index": 0,
      "campaignId": "259207489965973",
      "suggestedBudget": 13,
      "sevenDaysMissedOpportunities": {
        "startDate": "20221001",
        "endDate": "20221007",
        "percentTimeInBudget": 0.5,
        "estimatedMissedImpressionsLower": 3,
        "estimatedMissedImpressionsUpper": 5,
        "estimatedMissedClicksLower": 8,
        "estimatedMissedClicksUpper": 12,
        "estimatedMissedSalesLower": 2,
        "estimatedMissedSalesUpper": 4
      },
      "budgetRuleRecommendation": {
        "suggestedBudgetIncreasePercent": 5,
        "ruleName": "sample rule",
        "ruleId": "123"
      }
    }
  ],
  "budgetRecommendationsErrorResults": []
}
```

**Scenario 2:**  A successful response with a suggested budget but with no estimated missed impressions, clicks, or sales. Since the `percentTimeInBudget` parameter value is not less than one, it means that the campaign is in budget which means there are no missed opportunities. 


```
{
    "budgetRecommendationsSuccessResults": [
        {
            "index": 0,
            "campaignId": "259207489965973",
            "suggestedBudget": 13,
            "sevenDaysMissedOpportunities": {
                "startDate": "20210609",
                "endDate": "20210615",
                "percentTimeInBudget": 1,
                "estimatedMissedImpressionsLower": 0,
                "estimatedMissedImpressionsUpper": 0,
                "estimatedMissedClicksLower": 0,
                "estimatedMissedClicksUpper": 0,
                "estimatedMissedSalesLower": 0,
                "estimatedMissedSalesUpper": 0
            },
            "budgetRuleRecommendation": null
        }
    ],
    "budgetRecommendationsErrorResults": []
} 
```

**Scenario 3:** A successful response but with null suggested budget, missed opportunities, and budget rule recommendation. This means that there is not enough historic data to make a recommendation. 

```
{
    "budgetRecommendationsSuccessResults": [
        {
            "index": 0,
            "campaignId": "254916660662680",
            "suggestedBudget": null,
            "sevenDaysMissedOpportunities": null,
            "budgetRuleRecommendation": null
        }
    ],
    "budgetRecommendationsErrorResults": []
}
```



## Next steps

* Use the [PUT sp/campaigns](sponsored-products/3-0/openapi/prod#tag/Campaigns/operation/UpdateSponsoredProductsCampaigns) resource and update the new suggested bid in the `dailyBudget` parameter.

