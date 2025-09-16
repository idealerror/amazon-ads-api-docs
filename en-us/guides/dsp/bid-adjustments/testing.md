---
title: Bid adjustments testing
description: A description of ADSP bid adjustments testing.
type: guide
interface: api
---

# How to test DSP bid adjustments

To measure the impact of your bid adjustments, first validate if your bid adjustments terms have been matched and applied to any traffic and by how much, and then determine if this is within your use case expectation. You can verify the match rate through [bid adjustment reporting](guides/dsp/bid-adjustments/reports).  If you have set up multiple bid adjustments terms, you can also verify the final bid adjustments value applied through the `bidAdjustment` column. 

After you have validated that you terms have been matched and applied to traffic within your expectations, run an A/B test with a controlled group to measure the impact of your bid adjustments. 

> [NOTE] We do not recommend comparing performance before and after implementing bid adjustments as market conditions and other factors vary over time.

## Set up A/B test environment to test bid adjustments on ADSP 

Amazon DSP provides 10 Customer holdout groups for A/B testing. Each group contains 10% of randomly assigned Amazon customers, allowing you to create controlled experiments. 

Before you get started:

* Ensure you have enough budget and time for statistically significant results.
* Randomize group assignments for each new test to avoid potential cross contamination between experiments or any long-term bias.

Complete the following steps to set up basic A/B testing:

1. Create two identical orders. Each order should have only one line item.

  * One control order that replicates your current campaign (i.e., no changes).
  * One "test" order (with your test variable - bid adjustments, in this case).

  Each order should have only one line item (optional).

  >[NOTE] You can create two identical line items within the same order to run the test as well; however you will need to ensure that there are no other settings such as order-level budget that could have two line items affecting each other’s delivery. 

1. Make both orders identical except for the bid adjustments you're testing. Turn off **Automated budget optimization** to ensure your testing line items are not affecting each other, or affected by other line items.

Complete the following steps to set up control and test groups: 

1. Split users 50/50 between control and test.
1. Randomly assign 5 holdout groups to control, 5 to test.
1. To add the groups, from the audience picker in line item setting:
  1. Search for **Customer holdout group**. As **Customer holdout group** splits Amazon users, this is best option for campaigns targeting Amazon customers
  1. Add selected groups to each line item.
  1. Use `OR` logic within groups.
  1. Use `AND` logic with other targeting.

![Audience picker](/_images/bid-adjustments/audience-picker.png)

1. Add bid adjustments to the test line item(s). Optionally, you can add bid adjustments of 1.0 with the same bid adjustments terms to the control line item(s) to get a comparison view for the bid report. See [How to get report from A/B test](#how-to-get-report-from-ab-test). To understand the change in traffic matching (match rate), bids volume and win rate, A/B test is highly recommended to measure true impact. 

## How to get report from A/B test

To get overall bid adjustments impact on campaign outcome,  pull reports for orders/line items metrics to compare.  

If you want to analyze further on how the bid adjustments terms affect the traffic (% of bid requests, represented by match rate), bids or win rate, you can download bid report from [bid adjustments report](guides/reporting/v3/report-types/dspBidAdjustment). To compare the control vs test group results for bid metrics, you can choose to associate the control line item(s) with the same bid adjustment terms, applying bid adjustments value of 1.0. This will not change any bidding for the control line item(s), but will enable you to get the same report as your test line item(s) to do comparisons. For a usage example, see [Bid Adjustments reports](guides/dsp/bid-adjustments/reports).

To expand other metrics breakdown by bid adjustments terms, use existing custom reports if your dimension is used in bid adjustments that are already supported. For example, you can pull the breakdown of other metrics if you used any geo dimensions in bid adjustments. See [geo report type](guides/reporting/v3/report-types/geo).



