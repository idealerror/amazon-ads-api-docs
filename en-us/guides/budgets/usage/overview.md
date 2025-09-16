---
title: Budget usage overview
description: An overview of the Amazon Ads budget usage API.
type: guide
interface: api 
tags:
    - Sponsored Brands
    - Sponsored Display
    - Sponsored Products
    - Budget Usage
keywords:
    - budget
    - budgets
    - budget usage
    - budget usage percent
---

# Overview

The budget usage API retrieves the budget usage percent for your campaigns and portfolios to provide near real-time information about your budget usage. You can request the budget usage percent which is retrieved in near real-time for any active, paused, or expired campaigns or portfolios.

### Budget usage calculation

Budget usage percent is the current campaign ad spend for the day divided by the specified daily budget. This includes changes in budget due to [budget rules](https://advertising.amazon.com/help?ref_=AAC_unav_support_center#GNSMLANWNF344YBE) and increases from [average daily budget calculations](https://advertising.amazon.com/help?ref_=AAC_unav_support_center#GTGPQGUXNCTHE2DS). 

For example, consider a campaign with daily budget of \$100.00, no budget rules, and no average daily budget increases. In this case, if the current daily campaign spend is $80.00, the budget usage percent equals 80% since there was no increase in budget.

As another example, consider a campaign with daily budget of \$100.00 and a budget rule that increases budget by 50% on a specific date. On that date, the budget becomes \$150.00 due to the budget rule. If the campaign spends $80, the budget usage is \$80.00/\$150.00 = 53.3%. 