---
title: Campaign structure for Sponsored TV
description: Campaign structure details for a Sponsored TV campaign
type: guide
interface: api
tags: 
    - Sponsored TV
---

# Sponsored TV campaign structure

Creating Sponsored TV campaigns using the Amazon Ads API requires making multiple calls to create the entities that make up a campaign. Campaigns require you to create, at minimum, a [campaign](guides/sponsored-tv/campaigns/get-started#step-1-create-a-sponsored-tv-campaign), [ad group](guides/sponsored-tv/campaigns/get-started#step-2-create-an-ad-group), [ad](guides/sponsored-tv/campaigns/get-started#step-6-create-an-ad), [creative](guides/sponsored-tv/campaigns/get-started#step-5-create-a-creative), and [targeting](guides/sponsored-tv/campaigns/get-started#step-3-create-a-target-clause).

Refer to diagram below to understand campaign hierarchy.

* **Campaigns**: Campaigns group your ads by their advertising budgets and dates.
* **Ad groups:** Ad groups let you organize and manage your ads by products, categories, or other classifications that tie to your goals.
* **Ads:** Use ads to manage a product. Select a product from your brand to include in your ad group. This product will be the landing page for your ad.
* **Creative asset:** Each ad contains a video creative. The creative for your campaign is a critical component to an advertising campaign’s success.
* **Targeting:** Each ad group will have its own content-based audience and in-market category segment. Choosing the right segments for each product can help with your consideration goals. For more infomation, see [targeting](guides/sponsored-tv/targeting).

This example shows a campaign with one ad group. In one ad group, there is one ad with a video creative as well as content category targeting.

![Campaign structure](/_images/sponsored-tv/SPOT_diagram.png)

To get started on campaigns, see [Getting started with Sponsored TV campaigns](guides/sponsored-tv/campaigns/get-started).
