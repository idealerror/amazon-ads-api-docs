---
title: Campaign structure for Sponsored Brands
description: Campaign structure details for a Sponsored Brands campaign
type: guide
interface: api
tags: 
    - Sponsored Brands
---


# Campaign structure

Creating Sponsored Brands campaigns using the Amazon Ads API requires making multiple calls to create the entities that make up a campaign. Campaigns require you to create, at minimum, a [campaign](guides/sponsored-brands/campaigns/get-started-with-campaigns#step-1-create-a-campaign), [ad group](guides/sponsored-brands/campaigns/get-started-with-campaigns#step-2-create-ad-group), [ad](guides/sponsored-brands/campaigns/get-started-with-campaigns#step-4-create-an-ad), and either [keyword or product target](https://advertising.amazon.com/help?ref_=AAC_unav_support_center#G3XAU5G7C2JTNQTM).

Refer to diagram below to understand campaign hierarchy.

* **Campaigns**: Campaigns group your ads by their advertising budgets and dates.
* **Ad groups:** Ad groups let you organize and manage your ads by brands, keywords, products, categories, ad formats, landing pages, or other classifications that tie to your goals. Please note that you can only include a single type of ad in each ad group.
* **Ads:** Use ads to manage different variations of your creatives. An ad can have different creative formats including video, store spotlight, or product collection. 
* **Creative asset:** Each ad can contain a mix of image and video creatives.
* **Keyword or product targeting:** An ad group can use either keyword targeting or product targeting. Keyword targeting allows you to target specific customer search terms for which you want your ads to appear and optimizing bids for each. Product targeting can be applied at either the product, category, or brand level.

This example shows a campaign with two ad groups. In one ad group, there are two brand video ads each with a video creative as well as keyword targeting. In the other ad group, there are two product collection ads each with a logo and custom image creative as well as product targeting.

 ![Campaign structure](/_images/sponsored-brands/campaign_structure_SB.png)

To get started on campaigns, see [Getting started with campaigns](guides/sponsored-brands/campaigns/get-started-with-campaigns).
