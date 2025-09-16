---
title: Sponsored Display Feature Availability by Marketplace 
description: Learn which Amazon Ads API for Sponsored Display features are supporting in each Amazon marketplace.
type: guide
interface: api 
tags:
    - Sponsored Display
keywords:
    - marketplace
    - features
---

# Sponsored Display Feature Availability by Marketplace

This document lists descriptions and marketplace availability for all Sponsored Display features.

## Bid optimizations

Bid optimizations determine how the campaign will bid and charge. For Sponsored Display, there is CPC and vCPM. 

### Cost per click (CPC)

[CPC](https://advertising.amazon.com/library/guides/cost-per-click/?ref_=a20m_us_search_title) is the cost per click that an ad receives. It’s a metric that applies to all types of ads, whether they have text, images, or videos.  

**Relevant resource:**

* [/sd/campaigns](sponsored-display/3-0/openapi#tag/Campaigns)
    * costType is the field where the `cpc` enum must be declared.   


**Marketplace availability:** All marketplaces


### Cost per thousand viewable impressions (vCPM)

[vCPM](https://advertising.amazon.com/en-us/resources/whats-new/sponsored-display-expands-custom-bid-optimizations/?ref_=a20m_us_wn_gw) is the cost per one thousand viewable impressions. An advertiser is charged when their ad has been viewed by shoppers, as opposed to a CPC strategy where advertisers pay for ad clicks.


**Relevant resource:**

* [/sd/campaigns](sponsored-display/3-0/openapi#tag/Campaigns) 
    * costType is the field where the `vcpm` enum must be declared.   


**Marketplace availability:** All marketplaces

## Creatives

With creatives, advertisers can add or edit their video, headlines, logos or custom product images for existing campaigns.   

### Creative customization (Videos, headlines, and logos)

  
**Relevant resource:**

* [/sd/creatives](sponsored-display/3-0/openapi#tag/Creatives)
    * `creativeType` is the field where the `IMAGE` or `VIDEO` enum must be declared.   


**Marketplace availability:** All marketplaces

### Headline real-time moderation

[Real-time policy](https://advertising.amazon.com/en-us/resources/whats-new/sponsored-display-launches-real-time-checks-for-custom-headlines/?ref_=a20m_us_wn_gw) checks on the headline directly in a campaign.   


**Relevant resource:**

* [/sd/creatives](sponsored-display/3-0/openapi#tag/Creatives)   


**Marketplace availability:** 

* North America: United States (US), Canada (CA)
* Europe: United Kingdom (UK)

### Custom image creatives

Leverage an automated image or a custom product image.  


**Relevant resource:**

* [/sd/creatives](sponsored-display/3-0/openapi#tag/Creatives)   


**Marketplace availability:** All marketplaces

## Recommendations

Recommendations provide insight into potential products, categories, and bids that can help increase the probability of ad clicks and drive product awareness and conversions.   

### Bid recommendations

[Bid Recommendations](https://advertising.amazon.com/en-us/resources/whats-new/sponsored-display-bid-recommendations/?ref_=a20m_us_wn_gw) gives suggested bids whenever any CPC-based or vCPM-based Sponsored Display contextual targeting or views remarketing campaign is created.   


**Relevant resource:**

* [/sd/targets/bid/recommendations](sponsored-display/3-0/openapi#tag/Bid-Recommendations)
    * `costType` will determine the bid recommendations depending on the enum that is selected   


**Marketplace availability:**

* North America: United States (US), Canada (CA), Mexico (MX)
* Europe: Germany (DE), Spain (ES), France (FR), Italy (IT), United Kingdom (UK), Netherlands (NL) 
* Middle East: United Arab Emirates (AE)
* Asia Pacific: Japan (JP), India (IN), Australia (AU)

### Category suggestions

When Sponsored Display advertisers submit a list of products to advertise, they will now receive improved category recommendations.  


**Relevant resource:**

* [/sd/targets/recommendations](sponsored-display/3-0/openapi#tag/Targeting-Recommendations)  


**Marketplace availability:** 

* North America: United States (US), Canada (CA), Mexico (MX)
* South America: Brazil (BR)
* Europe: Germany (DE), Spain (ES), France (FR), Italy (IT), United Kingdom (UK), Netherlands (NL), 
* Middle East: United Arab Emirates (AE)
* Asia Pacific: Japan (JP), India (IN), Australia (AU)

### Product suggestions

When Sponsored Display advertisers submit a list of products to advertise, they will now receive improved product recommendations.  


**Relevant resource:**

* [/sd/targets/recommendations](sponsored-display/3-0/openapi#tag/Targeting-Recommendations)  


**Marketplace availability:** 

* North America: United States (US), Canada (CA), Mexico (MX)
* South America: Brazil (BR)
* Europe: Germany (DE), Spain (ES), France (FR), Italy (IT), United Kingdom (UK), Netherlands (NL), 
* Middle East: United Arab Emirates (AE)
* Asia Pacific: Japan (JP), India (IN), Australia (AU)

## Metrics

Metrics define what performance data is included in Sponsored Display reports.   

### New-to-brand (NTB)

[NTB](https://advertising.amazon.com/en-us/resources/whats-new/sponsored-display-new-to-brand-metrics/?ref_=a20m_us_wn_gw) metrics enable you to measure product orders and sales generated from first-time customers.  


**Relevant resource:**

* [/sd/{recordType}/report](sponsored-display/3-0/openapi#tag/Reports)


**Marketplace availability:** All marketplaces

### Detail Page View (DPV)

DPV metrics help you understand how many shoppers are visiting your product pages as a result of your Sponsored Display campaigns. 


**Relevant resource:**

* [/sd/{recordType}/report](sponsored-display/3-0/openapi#tag/Reports)  


**Marketplace availability:** All marketplaces

## Brand Safety - API

Send a list of domains that you do not want to show your ads off-site on. DENY LIST of apps and sites to protect your brand.   

**Relevant resource:**

* [/sd/brandSafety/deny](sponsored-display/3-0/openapi#tag/Brand-Safety-List)  


**Marketplace availability:** All marketplaces

## Product ads
Organize and manage ads within a campaign and use portfolios to organize campaigns based on your advertising needs.  

### Ad groups

Use ad groups to group your ads by brand, product, category, price range, or other classifications like theme or targeting strategy.  

**Relevant resource:**

* [/sd/adGroups](sponsored-display/3-0/openapi#tag/Ad-groups)  


**Marketplace availability:** All marketplaces

### Portfolios

Portfolios are a group of campaigns that you can organize to meet your advertising needs. Create portfolios by brand, product category, or by season to provide structure and manage your advertising activity.  

**Relevant resource:**

* [/sd/campaigns](sponsored-display/3-0/openapi#tag/Campaigns) 


**Marketplace availability:** 

* North America: United States (US), Canada (CA), Mexico (MX)
* Europe: Belgium (BE), Germany (DE), Spain (ES), France (FR), Italy (IT), United Kingdom (UK), Netherlands (NL), Sweden (SE), Poland (PL), Turkey (TR)
* Middle East: United Arab Emirates (AE), Saudi Arabia (SA), Egypt (EG)
* Asia Pacific: Japan (JP), India (IN), Australia (AU), Singapore (SG)

## Targeting

Use audience to reach customer both on and off Amaozn and contextual targeting to reach customers on Amazon.  

### Contextual targeting

Creates targeting clauses for products or categories.   

**Relevant resource:**

* [/sd/targets](sponsored-display/3-0/openapi#tag/Targeting)  


**Marketplace availability:** All marketplaces

### Audiences - Views remarketing

Targets a customer based on past activity. For example, if a customer views a specific product or category, ads can be used to re-engage them.  


**Relevant resource:**

* [/sd/targets](sponsored-display/3-0/openapi#tag/Targeting)  


**Marketplace availability:** All marketplaces

### Audiences - Amazon audiences

[Amazon audiences](guides/sponsored-display/audience-targeting) are prebuilt audiences that consist of four segments:

* In-market: In-market audiences allow advertisers to engage audiences who are in the same category as their advertised products.
* Lifestyle: Positioned for awareness campaigns, these audiences reflect a variety of shopping and viewing signals, including shopping on Amazon, browsing on IMDb, streaming on Prime Video, or streaming on Twitch.
* Interests: Interest-based audiences allow advertisers to help raise awareness with prospective buyers based on what they frequently browse and buy.
* Life Events: Life Events audiences give brands the opportunity to drive awareness and consideration for relevant products based around life moments  


**Relevant resource:**

* [/sd/targets](sponsored-display/3-0/openapi#tag/Targeting)  


**Marketplace availability:** All marketplaces

### Audiences - Purchases remarketing

Remarket to previous purchasers of their own products, related products, as well as products from specific retail categories.   


**Relevant resource:**

* [/sd/targets](sponsored-display/3-0/openapi#tag/Targeting)  


**Marketplace availability:** All marketplaces

### Negative targeting

Negative contextual targeting excludes shoppers’ brands or products from displaying ads in shopping results or detail pages.  

**Relevant resource:**

* [/sd/negativeTargets](sponsored-display/3-0/openapi#tag/Negative-targeting)  


**Marketplace availability:** All marketplaces
