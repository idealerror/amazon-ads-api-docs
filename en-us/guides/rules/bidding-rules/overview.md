---
title: Sponsored Products Rule-based bidding overview 
description: Quickstart guide that walks a user through the process of creating budget rules for Sponsored Products
type: guide
interface: api 
tags:
    - Sponsored Products
keywords:
    - Overview
    - Guardrails
    - Minimum budget
---

# Sponsored Products rule-based bidding overview 

Rule-based bidding takes the guesswork out of adjusting bids to achieve your marketing strategy. For any existing campaign running for at least 10 days, you can apply a rule with a guardrail of ROAS and Amazon Ads may then adjust your base bids up and down to increase conversions while maintaining your guardrails. Amazon Ads doesn’t guarantee the ROAS guardrail. However, if your campaign is not meeting the guardrail and the campaign’s return on ad spend (ROAS) for the last 20 days drops to less than 30% of your ROAS over a 20-to-40-day period, we will disable the rule on your behalf and will enable your previous bidding strategy and targeting level bids. 

When we disable your rule, you will receive a notification in the bell drawer with a link to the campaign.

## Eligible campaigns

Rule-based bidding is available for Sponsored Products. Campaigns can use any type of targeting (Automatic, Keyword or Product Targeting). Your campaign must be running for at least 10 days prior to setting a rule and have a minimum of 10 conversions in the last 30 days. Additionally, your campaign must meet the minimum budget requirement in the marketplace listed below.

## Guardrails

The guardrail is applied at the campaign level and the ROAS guardrail is based on the previous 21 days. 

For example if you have a manual targeting campaign with a ROAS guardrail of $2 and two targeting clauses:

1. shoes, BROAD match, and
2. "Example brand shoes", EXACT match.

For the previous 21 days, the **shoes BROAD match** targeting clause may have a ROAS of $1.50 with $1 CPC and the **“Example brand shoes” EXACT match** targeting clause may have a ROAS of $4 with $0.25 CPC. In this example, we may continue to bid $1 for the broad match targeting clause and $0.50 for the exact match targeting clause to increase your conversions and achieve a ROAS of $2 on $125 of ad spend for your campaign.

Note that Amazon Ads does not guarantee that we will meet the guardrail that you set. However, if your campaign is not meeting the guardrail and the campaign’s return on ad spend (ROAS) for the last 20 days drops to less than 30% of your ROAS over a 20-to-40-day period, we will disable the rule on your behalf and will enable your previous bidding strategy and targeting level bids. When we disable your rule, you will receive an additional recommendation from AMR with an updated suggestion for your ROAS guardrail. If you choose to not accept the new recommendation
your campaign will use the bidding strategy set prior to applying the rule.

We suggest that you use the provided recommendation to set your ROAS guardrail. This recommendation is based on your historical campaign information and a group of similar advertisers. Additionally, if you set your ROAS guardrails too aggressively, you may see a decrease in the number of conversions compared to your historical performance. We recommend that you provide a ROAS that is in line with your expectations while following closely to our recommendations, then adjust to hit your business goals over time.

## Reporting

There are no additional steps to generate a bid with rule-based bidding enabled. To report on your ROAS, spend, CPC, and other metrics, use the [reports](sponsored-products/2-0/openapi#/prod#tag/Campaign-Optimization-Rules/operation/CreateOptimizationRule) to create either a search term report, campaign performance report, or impression rank report.

## Minimum budget required for rule-based bidding

Performance rules rely on having clicks and conversions on a campaign. The minimum required budget aims at increasing the likelihood the campaign will have enough traffic for metrics to be evaluated and increase the quality of the recommendations.

The table below lists the minimum daily budget for each supported [marketplace](reference/api-overview#api-endpoints):

| Marketplace | Minimum daily budget in local currency |
|-------------|----------------------------------------|
| AE | 10 |
| AU | 10 |
| CA | 10 |
| DE | 10 |
| ES | 10 |
| FR | 10 |
| IN | 300 |
| IT | 10 |
| JP | 600 |
| MX | 50 |
| UK | 10 |
| US | 10 |
| BR| 20|
| EG | 50 |
| SA | 20 |
| SE | 50|
| PL | 30 |
| TR | 50 |
| NL | 10 |
| BE | 10 |
| SG | 10 | 

## Next steps

If you don't have access to the Sponsored Products API, follow the steps in [Advertising API account setup](guides/onboarding/overview). 

If you do have access to the rule-based bidding API, learn more about [getting started with rule-based bidding](guides/rules/bidding-rules/getting-started).
