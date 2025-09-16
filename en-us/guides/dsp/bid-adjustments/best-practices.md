---
title: Tips and best practices for the ADSP bid adjustments API
description: A description of best practices for using the ADSP bid adjustments API
type: guide
interface: api
---

# Tips and best practices

## When to use bid adjustments

Default automated optimization already uses many campaign and Amazon proprietary signals. ([Learn more about bid optimization.](https://advertising.amazon.com/help/GF8GFPZ4C4EHNG6V))

We recommend using [bid adjustments](https://advertising.amazon.com/help/G298ZVT2MWD7ETWS) whenever our system isn’t able to acquire the signals needed to get to best optimization for you. Examples may include but are not limited to:

* Customized business goals: optimize goals that are currently not supported by Amazon DSP.
* Customized attribution window and events: how you count conversions can differ from Amazon DSP.
* Offline sales, delayed conversions, online conversion data such as conversion values that are not incorporated into real-time bidding flow.
* Contextual and inventory quality signals, media quality signals.
* Signals from brand lift and conversion lift studies.
* Signals from cross-channel marketing measurement, media mix modeling.

You don’t need to use bid adjustments if you’re only using the specific campaign data that Amazon DSP is already using to optimize bids with. Adjusting the bids in this case may negatively impact your campaign’s performance as the default optimization may already reach the goal at its maximum.

## Tips for using bid adjustments

* Bid adjustments are expected to redirect traffic based on your preference. However, this does not guarantee performance improvement. We recommend a trial and error approach to iteratively determine your optimal bid adjustments.
* If your line item has set a maximum average CPM, consider setting it higher to maximize the impact of bid adjustments, as this allow bid adjustments to influence the bids.
* Focus on relative bid adjustment values. The absolute values of bid adjustments often matter less than their relative difference. Typically, changing the absolute values of bid adjustments while maintaining the same relative scale will not affect the traffic distribution.
* Scaling down bids for less desirable inventory can lead to unintended consequences. When supply is limited or max average CPM is set low, lowering bids (example: apply bid adjustment below 0.2) may increase delivery pressure on remaining inventories that could result in overall under-delivery.
    * Example: Aggressively reducing bids for traffic A can shift delivery pressure to B, potentially impacting overall delivery and campaign performance.
* High final bid adjustments (example: bid adjustments over 5x), may harm campaign performance by quickly exhausting your budget. When applying bid adjustments, be cautious, as the potential combination of multiple terms can result in excessively high final bid adjustments.
    * We recommend optimizing bids strategically using data-driven insights, rather than relying on extreme bid inflation, to achieve desired visibility and performance without compromising overall advertising effectiveness.
