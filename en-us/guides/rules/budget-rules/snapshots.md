---
title: Including budget rules values in a snapshot
description: Learn how to use the Amazon Ads API to include budget rules metrics and values in a snapshot.
type: guide
interface: api 
tags:
    - Sponsored Brands
    - Sponsored Products
    - Sponsored Display
    - Budget Rules
keywords:
    - snapshot
    - budget
---

# Including budget rules values in a snapshot

>[WARNING] Snapshots APIs are deprecated and will be shut off on October 15, 2024. For replacement functionality, see the [exports](guides/exports/overview) API. To learn more, view the [migration guide](reference/migration-guides/snapshots-exports). 

Snapshots enable you to asynchronously retrieve a record of your campaigns and keywords in bulk. 

Use the following table to understand which endpoint to use to request a snapshot based on your ad type:

| Ad types | Snapshot request endpoint |
|------|-------|
| Sponsored Brands | [POST /v2/hsa/{recordType}/snapshot](reference/sponsored-brands/2/snapshots) |
| Sponsored Products |[POST /v2/sp/{recordType}/snapshot](sponsored-products/2-0/openapi#tag/Snapshots) |
| Sponsored Display | [POST /v2/sd/{recordType}/snapshot](sponsored-display/3-0/openapi#tag/Snapshots)|

If you are already using the Snapshots API to retrieve bulk data, you don’t need to make any changes to your request. If you have created a budget rule on a campaign and the rule-based budget is currently applied, the snapshot response will contain the `ruleBasedBudget` object.

The `ruleBasedBudget` object contains the following attributes:

1.  **value** : the value of rule based budget
2. **applicableRuleId**: the identifier of the active rule 
3. **applicableRuleName** : the name of the active rule
4.  **isProcessing**: set to **true** if rule evaluation is in progress. Set to **false ** when evaluation is complete and the rule budget value is updated.

As of February 2023, you can use the presence of the `ruleBasedBudget` object as an indicator that budget rules have been applied, then retrieve the full budget rules history for that campaign using GET /sb/campaigns/{campaignId}/budgetRules. Budget rules with an `ACTIVE` status are acting on the campaign. 

>[WARNING] As of February 2023, the `applicableRuleId` can be incomplete since multiple rules could be active at the same time (see rule evaluation details), so you should use GET /sb/campaigns/{campaignId}/budgetRules to see active rules on a campaign. 

For example:

A rule-based budget that’s under current enforcement:

```JSON
{
    "campaignId": 12345678901234,
    "name": "Sample Budget Rule Campaign",
    "budget": 100,
    "budgetType": "daily",
    "ruleBasedBudget": {
        "value": 110,
        "applicableRuleId": "e887669c-0f22-42fa-85ab-2ba96ea8190c",
        "applicableRuleName": "SAMPLE_BUDGET_RULE",
        "isProcessing": false
    },
    "startDate": "20200915",
    "state": "enabled",
    "bidOptimization": "false"
}
```

A campaign with no rule-based budget under current enforcement, or a campaign with no active budget rule: 

```JSON
{
     "campaignId": 12345678901234,
    "name": "Sample Budget Rule Campaign",
    "budget": 100,
    "budgetType": "daily",
    "startDate": "20200915",
    "state": "enabled",
    "bidOptimization": "false"
}
```
