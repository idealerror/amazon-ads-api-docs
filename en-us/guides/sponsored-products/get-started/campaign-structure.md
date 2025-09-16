---
title: Sponsored Products campaign structure
description: Conceptual overview of Sponsored Products campaign structure
type: guide
interface: api 
tags:
    - Sponsored Products
    - Campaign management
keywords:
    - auto campaigns
    - manual campaigns
---

# Sponsored Products campaign structure

Creating Sponsored Products campaigns using the Amazon Ads API requires making multiple calls to create the entities that make up a campaign. The number of entities you need to create is determined by the type of campaign you are creating, either manual or auto. 

Both manual and auto campaigns require you to create, at minimum, [campaigns](guides/sponsored-products/campaigns), [ad groups](guides/sponsored-products/ad-groups), and [product ads](guides/sponsored-products/product-ads). However, for manual campaigns, the advertiser also has complete control over [keywords](guides/sponsored-products/keywords/overview) and [product targeting expressions](guides/sponsored-products/product-targeting/overview). For auto campaigns, Amazon suggests relevant targeting expressions and keywords based on the products you are advertising. Both auto and manual campaigns also support [negative keyword](guides/sponsored-products/negative-targeting/keywords) and [product targeting](guides/sponsored-products/negative-targeting/product-brand). 

>[TIP] We suggest new advertisers start with auto campaigns. You can then analyze the performance of suggested keyword and targeting expressions, and create a manual campaign using the highest performers. 

## Auto campaigns

For **auto** campaigns, advertisers only need to create a **campaign**, **ad group**, and **product ad**. Amazon assigns targeting expressions and keywords, although advertisers can choose to define [auto targeting expressions](guides/sponsored-products/auto-targeting) if desired. 

The following diagram shows an example of an auto campaign with two ad groups. Each ad group is advertising two products (product ads). The first ad group has a **negative targeting expression**, while the second uses an auto targeting expression and negative keyword. 

![Auto campaign entity diagram](/_images/sponsored-products/auto-campaign.png)

## Manual campaigns

For **manual** campaigns, advertisers also need to create a **campaign**, **ad group**, and **product ad**, but they must also include at least one **product targeting expression** or **keyword** in each ad group. 

The following diagram shows an example of a manual campaign with two ad groups, and each ad group has two product ads. The first ad group targets two keywords and also has a negative keyword. The second ad group targets both an individual product and a category of products, and includes a negative targeting expression.

![Manual campaign entity diagram](/_images/sponsored-products/manual-campaign.png)

>[NOTE] Both manual and auto campaigns can also contain negative product targeting expressions or negative keywords at the campaign level (in addition to the ad group level). 

## Learn more

* [Creating auto campaigns](guides/sponsored-products/get-started/auto-campaigns)
* [Creating manual campaigns](guides/sponsored-products/get-started/manual-campaigns)
* [Entity limits](https://advertising.amazon.com/help?#G86H2227323T8T4Q)



