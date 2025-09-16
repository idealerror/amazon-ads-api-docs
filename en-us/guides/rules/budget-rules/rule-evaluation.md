---
title: Budget rule evaluation
description: Learn how budget rules in the the Amazon Ads API are evaluated to help you create useful budget rules.
type: guide
interface: api 
tags:
    - Sponsored Brands
    - Sponsored Products
    - Sponsored Display
    - Budget Rules
keywords:
    - rule evaluation
    - daily budget computation
---

# Budget rule evaluation

Budget rules are evaluated on your campaigns at the start of the day to calculate campaign budget for the day. If the rule meets the conditions set, the budget is increased for the day in the percentage configured in the rule. If the rule doesn’t meet the conditions set, the daily budget amount is applied for the campaign.

Each time a new budget rule is created or an existing rule is updated, the budget rule is evaluated immediately to update the budget for that day. Similarly, budget rules for a campaign are evaluated to calculate the budget for that day each time the campaign daily budget is modified.

Daily budget is computed for campaigns that have a set budget rule. If the budget rule criteria is satisfied, the  campaign budget is increased by the specified percent. Campaigns will not go out of budget as long as the campaign spend is less than the increased budget. If any rule criteria are not satisfied, then the specified daily budget is used. 

Multiple rules can specified for a campaign to a maximum of 250. As of February 2023, we have updated the logic for budget rules. If more than one budget rule satisfies your set conditions, all budget rules are applied and their increases are added to daily budget independently of other budget rules. 

For example, consider a campaign “Running Shoes for Q4,” which has three rules set: 

1. Increase budget by 20% with no end date if ROAS is greater than 3 (performance rule), 
2. Increase budget by 40% with no end date if ROAS is greater than 3 (performance rule), and 
3. Increase budget by 100% on November 27 (Black Friday, schedule rule). 


Suppose that ROAS for the campaign is 4. When we evaluate the rules for November 27, we’ll implement a 140% daily budget increase for that day. This includes 40% increase from the performance rule whose conditions were met, and 100% increase from the scheduled rule. Note that the 20% performance rule was not applied because its condition was not met.

As another example, if the campaign has two rules set: 

1. Increase budget by 50% on November 27, and 
2. Increase budget by 25% between 5:00PM and 9:00PM with no end date. 

When we evaluate the rules for November 27, we'll implement a 50% increase from 12:00AM to 5:00PM. From 5:00PM to 9:00PM, the campaign will get an additional 25% from the scheduled rule with hours of day increase for a total of 75% in that period, and at 9:00PM the increase will be back to 50%.

## Budget rule status

Budget rules include status based on conditions specified in the rule, including the date range and performance goal. 

**Active**

The rule is active and budget for the day is increased based on the rule configuration. 

**On Hold**

The budget rule can be “on hold” for any of the following reasons:

1.	Performance criteria haven’t yet been met. 
2.	Another active rule supersedes the current rule.
3.	A recurrence condition has not yet been met.

**Paused**

The rule is paused and not eligible for evaluation.

**Budget threshold not met**

The rule is not applicable because the minimum required daily budget threshold required is not met. This is applicable for performance rules only.

**Pending Start**

The start date of the rule is in the future. On the start date, the rule will be evaluated and will become active if the condition is satisfied.

**Expired**

The rule has reached the end date specified in the rule configuration.
