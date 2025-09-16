---
title: Including budget rules values in a report
description: Learn how to use the Amazon Ads API to include budget rules metrics and values in a report.
type: guide
interface: api 
tags:
    - Sponsored Brands
    - Sponsored Products
    - Sponsored Display
    - Budget Rules
keywords:
    - reports
    - report
    - reporting
    - budgets
---

# Including budget rules values in a report

To create a report using the API, you specify the fields for the report. Rule-based budget metrics are available for the `campaigns` report type only. To add rule-based budget metrics to a report, specify the following metrics in your report request:

| Metric | Description |
|--------|-------------|
| `campaignRuleBasedBudget` | The value of the rule-based budget. |
| `applicableBudgetRuleId` | The identifier of the active rule. |
| `applicableBudgetRuleName` | The name of the active rule. |

Depending on the ad type, you can request reports using the following endpoints:

| Ad type | Report request endpoint |
|----|------|
| Sponsored Products | [POST /reporting/reports](offline-report-prod-3p#tag/Asynchronous-Reports/operation/createAsyncReport) |
| Sponsored Brands | [POST /v2/hsa/campaigns/report](sponsored-brands/3-0/openapi#tag/Reports)| 
| Sponsored Display | [POST /sd/{recordType}/report](sponsored-display/3-0/openapi#tag/Reports)|

## Sample report request: Sponsored Brands

To create a report that includes all available rule-based budget metrics:

Use the `POST /v2/hsa/campaigns/report` operation, passing the following request body:

```JSON
{
    "reportDate":"20200930",
    "metrics":"campaignBudget,campaignRuleBasedBudget,applicableBudgetRuleId,applicableBudgetRuleName"`
}
```

The response will resemble:

```JSON
[
    {
        //For this campaign, there is no active budget rule or rule based budget is not enforced
        "campaignBudget": 100,
        "campaignId": 144375700196005800
    },
    {
        "campaignBudget": 100,
        "applicableBudgetRuleId": "d9c1d0c5-d686-4d0f-9161-10a0c1a288ff",
        "campaignId": 123456789012345,
        "applicableBudgetRuleName": "SAMPLE_BUDGET_RULE",
        "campaignRuleBasedBudget": 112
    }
]
```
