---
title: Bid adjustments FAQs
description: Frequently asked questions about the ADSP bid adjustments API
type: guide
interface: api
---

# Bid adjustments FAQs

## How do I access the product feature? What are the supply that are available?

The feature is available to all advertisers WW for non-guaranteed ads (open and private auctions) on [Amazon DSP](https://advertising.amazon.com/solutions/products/amazon-dsp) in both Amazon DSP console and API. **Fire TV and Fire Tablet ads are currently not supported.**

## Are there any restrictions for using bid adjustments?

Please refer to our [Audience Creation Policy](https://advertising.amazon.com/resources/ad-policy/creative-acceptance/amazon-audience-policies?ref_=a20m_us_spcs_cap_spcs_cap9) and [Prohibited Audience Categories](https://advertising.amazon.com/help/GBZDH7USH5MVGJRF).

If you updated your [Advertiser settings to exclude demographic signals](http://your%20advertiser%20settings%20to%20exclude%20demographic%20signals/), bid adjustments will require you to remove audiences with demographic signals in bid adjustments rule and remove the dimensions city and postal code from the bid adjustments rule.

Dimensions such as postal code and city are restricted to the list of cities and postal codes supported.

## What does bid adjustment actually change?

Amazon DSP generates bids based on the predicted value of the impressions, how the campaign is performing against its Target KPI, and how it's pacing against its flight budget. Bid adjustments adjust one of these components of Amazon DSP bid generation - the predicted value of the impressions, not the final bids. Amazon DSP will continue raising your bids to ensure the budget is spent in full  or optimize bids to maximize performance and delivery at or near your Target KPI. Your adjusted bids will be capped by the optional max average CPM field if it is set. This is to ensure that we have mechanisms to still meet advertisers' other objectives such as pacing, Target KPI and maximum average CPM (if chosen). This means that if you set the bid adjustment as 2 for an impression opportunity, you may not see exact 2X in final bid price, but a directional positive shift in the bid price towards 2X.

## What if the bid adjustment is set to 0?

If you make the bid adjustment to 0, this means we will adjust the predicted value of impressions to 0. However, this may not completely override the bid price to 0 as the bid price could be affected by other factors such as pacing, Target KPI and maximum average CPM (if chosen). Therefore, the effect of setting bid adjustments to 0 will be different from targeting exclusions. We recommend using targeting exclusions if your goal is to completely exclude the inventory from bidding.

## What is the maximum amount that the predicted value can be adjusted?

Each single bid adjustment can go up to 10 individually, with the combined final bid adjustments also capped at 10. The final bid is ultimately limited by the maximum average CPM set for the ad line.

## What happens if the same dimension matches twice?

If the same dimension is matched across different bid adjustment terms, then we will match it both times and multiply accordingly. 
