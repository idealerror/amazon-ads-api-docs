---
title: Bid adjustments overview
description: An introduction to bid adjustments in Amazon DSP
type: guide
interface: api
---

# Bid adjustments overview

**Bid adjustments**, previously known as bid modifiers, let you adjust bids up or down for a specific criteria at bidding time for a single line item.

Besides selecting a bid strategy, you can maintain nuanced and customized bidding with bid adjustments within a single line item. Bid adjustments are applied at line item level and work in tandem with Amazon DSP automated optimization.

Amazon DSP generates bids based on the predicted value of the impressions, how the campaign is performing against its Target KPI, and how it's pacing against its flight budget. Bid adjustments adjust one of these components of Amazon DSP bid generation - the predicted value of the impressions, not the final bids.

>[NOTE] Amazon DSP will continue raising your bids to ensure the budget is spent in full or optimize bids to maximize performance and delivery at or near your Target KPI. Your adjusted bids will be capped by the optional max average CPM field if it is set.

See more about [bid adjustments](https://advertising.amazon.com/help/G298ZVT2MWD7ETWS) and [best practices for using bid adjustments](https://advertising.amazon.com/help/GP6Y2AE2BUM5XWFX) in the console help center, or learn about [getting started with DSP bid adjustments in the Amazon Ads API](guides/dsp/bid-adjustments/overview). 
