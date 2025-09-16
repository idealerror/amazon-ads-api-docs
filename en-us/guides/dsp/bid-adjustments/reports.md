---
title: Bid adjustments reports
description: A description of ADSP bid adjustments reports with usage examples.
type: guide
interface: api
---

# Bid adjustments reports

The bid adjustments report allows advertisers to access aggregated bid data through offline reporting. Advertisers can query metrics at the bid adjustment matched term level for their orders and lines within specified time periods.

There are two types of report available: 

* **Daily-level report**: Metrics include match rate, bids, and win rate. Metadata includes bid adjustments. This report can cover one day or multiple days. For example, if advertisers select a week, the report will show 7 rows for each combination of selected columns - one for each day.
* **Summary report**: This report covers any time unit other than daily, and the metrics are aggregated over the specified time period. Only bids and win rate metrics are available. Metadata includes bid adjustments.

## Bid adjustment report type

For more information about bid adjustment report types, see [dspBidAdjustment](guides/reporting/v3/report-types/bid-adjustment).

## Example usage

>[NOTE]All data shown in the examples are fictional and do not reflect actual performance.

1. Verify if any bid adjustment terms you wrote got any traffic (and by how much) with match rate metric. Note that match rate metric is currently only available in Daily report.  

    For instance, if you get following report, you would know that the slot size 728x90 and 320x50 that you chose to apply the bid adjustments covers 60.58% of the traffic. This could help you understand if you are making the adjustments to a small or a big part of the traffic, or if there isn’t any match at all! 

    |bidAdjustmentRuleId	|bidAdjustmentRule	|bidAdjustmentMatchedTermIds	|bidAdjustmentMatchedTerms	|bidAdjustment	|matchRate	|bids	|winRate	|
    |---	|---	|---	|---	|---	|---	|---	|---	|
    |`7ef92d14-****-****-****-************`	|Highest clicks	|[]	|Default traffic (no adjustments)	|	|39.42%	|5652184	|1.04%	|
    |`7ef92d14-****-****-****-************`	|Highest clicks	|[1]	|[{id:1, slotSize: ["728x90"]}]	|1.4	|20.83%	|5493878	|1.13%	|
    |`7ef92d14-****-****-****-************`	|Highest clicks	|[2]	|[{id:2, slotSize: ["320x50"]}]	|1.2	|39.75%	|15379006	|3.25%	|

1. Check your final bid adjustments values when there are multiple terms matched

    Note that when there are multiple bid adjustment terms matched, we multiply the bid adjustments you set for each term together. In the example below, an advertiser set 2 bid adjustment terms, one for slotPosition=‘ABOVE“ (adjust by 2), one for deviceType=”PC“(adjust by 1.2). The first row shows slotPosition is matched (above the fold) but deviceType is not matched. The second row shows deviceType is matched (on ”PC“) but slotPosition is not matched. The 3rd row shows both terms are matched at the same time (the bid request is an ‘above the fold’ slot and a browsing activity on the PC). Therefore, the final bid adjustment value 2.4 is a multiplication from 2 x 1.2. This will give you more visibility into what the ultimate bid adjustments are applied. 

    |bidAdjustmentMatchedTermIds	|bidAdjustmentMatchedTerms	|bidAdjustment	|
    |---	|---	|---	|
    |[1]	|[{id:1, slotPosition: ["ABOVE"]}	|2	|
    |[2]	|[{id:2, deviceType: ["PC"]}]	|1.2	|
    |[1, 2]	|[{id:1, slotPosition: ["ABOVE"]},  id:2, {deviceType: "PC"}}]	|2.4	|

1. Validate if your bid adjustments are working in a non-A/B testing environment. 

    Note that we always recommend setting up an A/B test to understand the effect of applying bid adjustments. We do not recommend comparing pre- versus post-bid adjustments, as the market fluctuates over time and there are many confounding factors. However, if you apply very small bid adjustments, you should see your bids drop significantly for items that previously had large traffic volumes, compared with any historical data/information you obtained. Note that even if you apply a bid adjustment of 0, you may not drive the bids down absolutely to 0, as there are other factors involved in our bid valuation process. See explanations [in the bid adjustments FAQ](guides/dsp/bid-adjustments/frequently-asked-questions#what-if-the-bid-adjustment-is-set-to-0). 

    |bidAdjustmentRuleId	|bidAdjustmentRule	|bidAdjustmentMatchedTermIds	|bidAdjustmentMatchedTerms	|bidAdjustment	|bids	|winRate	|
    |---	|---	|---	|---	|---	|---	|---	|
    |`8f51a923-****-****-****-************`	|Rule Example	|[1]	|[{id:1, browser: ["Mobile ABC"]}]	|0.02	|10	|10%	|
    |`8f51a923-****-****-****-************`	|Rule Example	|[2]	|[{id2: browser: ["ABC WebView"]}]	|0.03	|173	|8.18%	|
    |`8f51a923-****-****-****-************`	|Rule Example	|[3]	|[{id3: browser: ["ABC"]}]	|0.02	|36	|12.50%	|
    |`8f51a923-****-****-****-************`	|Rule Example	|[]	|Default traffic (no adjustments)	|	|374996	|10.95%	|

1. Validate if your bid adjustments are working in an A/B test environment. 

    You can set up an A/B test following the guidelines [here](guides/dsp/bid-adjustments/testing). To understand the overall impact of bid adjustments on campaign outcomes, you can simply pull reports for order/line item metrics to compare.

    If you want to analyze further on how the bid adjustments terms affect the traffic (% of bid requests, represented by match rate), bids or win rate, you can use this bid adjustments report.  

    Note that currently we only report on line item(s) with bid adjustments applied. If you run an A/B test and are comparing multiple different bid adjustment strategies, you can pull the report by bidAdjustmentRule. If your control group does not use bid adjustments, you could apply the exact same bid adjustment terms in your control line item/order as in your test line item/order, but set the bid adjustment value to 1 to get the report for the control group without changing the bid values. For example, if you have a control line item without bid adjustments and a test line item applying bid adjustments, you can get the following aggregated report: 

    |orderId	|orderName	|lineItemId	|lineItemName	|bidAdjustmentMatchedTermIds	|bidAdjustment	|winRate	|
    |---	|---	|---	|---	|---	|---	|---	|
    |5874XXXXXXXXXXXXX	|My remarketing campaign 	|5506XXXXXXXXXXXXXX	|bid adjustments - control	|[1]	|1	|2.89%	|
    |5874XXXXXXXXXXXXX	|My remarketing campaign	|5506XXXXXXXXXXXXXX	|bid adjustments - control	|[2]	|1	|0.95%	|
    |5874XXXXXXXXXXXXX	|My remarketing campaign	|3283XXXXXXXXXXXXXX	|bid adjustments - test	|[1]	|3	|4.35%	|
    |5874XXXXXXXXXXXXX	|My remarketing campaign	|3283XXXXXXXXXXXXXX	|bid adjustments - test	|[2]	|1.5	|1.88%	|

    The above report can be re-organized into the calculation of win rate delta ((test-control)/control): 

    |bidAdjustmentMatchedTermIds	|bidAdjustment	|Control line item winRate	|Test line item winRate	|winRate Delta: (test - control) / control	|
    |---	|---	|---	|---	|---	|
    |[1]	|3	|2.89%	|4.35%	|50.52%	|
    |[2]	|1.5	|0.95%	|1.88%	|97.89%	|

1. Analyze the relationship between bid adjustments and win rate delta ((test-control)/control)

    If you have sufficient traffic data, you can analyze patterns between your applied bid adjustments and win rate delta. Note that higher bid adjustments don't necessarily lead to higher win rate, but this analysis will help you understand the effectiveness of your current bid adjustments and inform future iterations.

    For instance, if you run an A/B test with bid adjustments applied to a selected list of domains for the test line item, you can create a chart plotting bid adjustment values (X-axis) against win rate delta (Y-axis). (Refer to the win rate delta calculation in item 4 above). You might observe that while increases in bid adjustments may increase win rate delta, bid adjustments beyond 2x may not further yield additional improvements in win rate. 

    ![Bid adjustments win rate](/_images/bid-adjustments/bid_adjustments_win_rate.png)

1. Analyze pattern: Understand the patterns of the bid adjustments terms on bids or win rate metrics to help you make decisions on the bid adjustments values. 

    For instance, if you run an A/B test and set up the reporting for control group, you may graph the *win rate* for control group to understand the status quo of win rate by bid adjustments match terms.  You may also graph the same chart for test group and compare the two. 

    Assume you created the following bid adjustments rule: 

    |bidAdjustmentMatchedTerms	|bidAdjustment	|
    |---	|---	|
    |[{id:1, deviceType:["TV", "ConnectedDevice"]}]	|1.5	|
    |[{id:2, deviceType:["TABLET", "PHONE"]}]	|1.5	|

    The below chart shows the win rate delta (((test-control)/control) by matched terms over time. It shows you that the bid adjustments applied to “TV” and “ConnectedDevice” is always more effective than “Phone” and “Tablet”. This can change your strategy for your existing bid adjustments values (where you are using the same bid adjustments value for both now).

    ![Bid adjustments win rate by matched terms](/_images/bid-adjustments/bid_adjustments_win_rate_matched_terms.png)

## FAQs

- **Why is my win rate still low despite applying a large bid adjustment?**

    Win rate represents how often you successfully get your ad shown. This depends on several factors: bid price (which you adjust through bid adjustment), auction dynamics, inventory availability, campaign settings (like frequency capping), and other constraints. While increasing bid adjustment will likely raise your bid price, its impact may be limited due to other factors. For example, if you are targeting users who have viewed one specific product on Amazon from the previous day, there might only be 100 such users available daily. Even with very high bids, you can’t win more impressions than available inventory. Additionally, raising your bid may help you win more initially, but after a certain point, you get fewer additional wins and may reach diminishing returns as the losses are due to factors beyond price. 

- **Why do I see a lower win rate for segments where I've set a higher bid adjustment?  For example, if I set a bid adjustment of 2.0 for mobile traffic and see a 15% win rate, while desktop traffic with a bid adjustment of 1.5 shows a 25% win rate, why isn't the higher bid adjustment winning more auctions?**

    Each row in the report represents a unique combination of traffic segment (defined by matched terms) for a specific time window (e.g., daily). For example, one row might show performance for deviceType = ‘PC’, while another shows deviceType = ”Phone”. Their competition level, available inventory, and user behaviors are different. Thus, each segment starts with its own baseline win rate, and when you apply bid adjustment, the impact varies by segment. For example, deviceType = “Phone” may have a 8% baseline win rate whereas deviceType = “PC” have a 20% baseline win rate to begin with for the specific group of users you are targeting. Therefore, the win rates across different matched terms should not be directly compared, even with identical bid adjustments.

- **Why do I see lower win rates in rows with higher bids (bid volumes)?  For example, if I bid on 10,000 auctions for popular sites and get a 15% win rate, while I bid on 1,000 auctions for niche sites and get a 30% win rate, why aren't the popular sites winning at similar rates?**

    The bids are counts of bid responses we submitted. Higher bid volumes do not suggest any relationship with higher win rates. Higher bid adjustments may increase our bid valuation, thus increasing the bid volumes. However, placing more bids with an increased bid prices through bid adjustments may not necessarily result in an increase in win rate due to market competitiveness, ad inventory, ad quality, or ad relevance to users. Additionally, win rates across rows should not be directly compared, as mentioned in above question.

- **Why does match rate not sum up to 100%?**

    We report match rates broken down by bid adjustment matched terms. If your bid adjustment terms do not match with any traffic, that traffic will be reported under 'Default traffic (no adjustments)'. In theory, adding up all rows for one bid adjustment rule for a line item should sum to 100%. However, records with low occurrence (less than 2 bid counts per hour) are filtered out before aggregation. As a result, the match rates may not sum to 100%. The difference between the sum of reported match rates and 100% represents the filtered-out traffic.

- **Why do I see low bid volumes for a specific domain (e.g. `foo.com`) alone while I see see large amounts of bids for the same domain with other dimensions?**

    |date	|bidAdjustmentRuleId	|bidAdjustmentRule	|bidAdjustmentMatchedTermIds	|bidAdjustmentMatchedTerms	|bidAdjustment	|bids	|winRate	|
    |---	|---	|---	|---	|---	|---	|---	|---	|
    |4/7/2025	|`9d4e2f1c-****-****-****-************`	|Display Bid Adjustments Test	|[1, 4]	|[{id:1, deviceType: ["Tablet"]}, {id:4, **domain: ["foo.com"]**}]	|1.5	|488	|2.08%	|
    |4/7/2025	|`9d4e2f1c-****-****-****-************`	|Display Bid Adjustments Test	|[2, 4]	|[{id:2, deviceType: ["PC"]}, {id:4, **domain: ["foo.com"]**}]	|2.4	|436538	|6.72%	|
    |4/7/2025	|`9d4e2f1c-****-****-****-************`	|Display Bid Adjustments Test	|[3, 4]	|[{id:3, deviceType: ["Phone"]}, {id:4, **domain: ["foo.com"]**}]	|2.2	|89157	|6.51%	|
    |4/7/2025	|`9d4e2f1c-****-****-****-************`	|Display Bid Adjustments Test	|[4]	|[{id:4, **domain: ["foo.com"]**}]	|1.1	|5	|0%	|
    |4/7/2025	|`9d4e2f1c-****-****-****-************`	|Display Bid Adjustments Test	|[]	|Default traffic (no adjustments)	|	|6768	|3.25%	|

    The matched terms listed in each row of your report are mutually exclusive. Unlike your audience report where audiences may overlap, the matched terms do not overlap with each other. In your daily report for a line item, you should find that your match rates add up to 100% for each bid adjustment rule (if no filters applied). If your bids for `foo.com` alone are low (e.g., 5) but show significant bid volumes when combined with other dimensions, this indicates that most `foo.com` traffic also matches additional targeting dimensions (such as deviceType) specified in your bid adjustment terms.

- **Why do I see extremely large bid adjustment value?**

    Values under the bidAdjustment column represent the final bid adjustment applied when multiple terms match. (See [How are bid adjustments calculated?](guides/dsp/bid-adjustments/guide#how-are-bid-adjustments-calculated).)  However, this will not result in extremely large final bid prices, as the final bid will be shaded and capped at the Max Avg CPM you set. Currently, we:

    * Enforce a maximum of 10 for each individual bid adjustment value
    * Plan to enforce a maximum of 10 for the final combined bid adjustment value in the future

    These limits ensure your bid prices remain reasonable when multiple targeting terms are combined.
