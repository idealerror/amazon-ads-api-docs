---
title: Sponsored Brands features by marketplace
description: An overview of the features by marketplace for Sponsored Brands
type: guide
interface: api
tags: 
    - Sponsored Brands
keywords: 
    - Features
    - Marketplace
---

# Sponsored Brands feature availability by marketplace

This document lists descriptions and marketplace availability for all Sponsored Brands features.


## Campaigns

Campaigns allow you to advertise your products in high-visibility placements on Amazon.com. For more information, see Sponsored Brands overview

**Relevant resource:**

* [/sb/v4/campaigns](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/CreateSponsoredBrandsCampaigns)


**Marketplace availability:** All marketplaces.


## Ad groups

This [feature](guides/sponsored-products/ad-groups) provides the ability to organize, manage, and track performance of the products within your campaign.

**Relevant resource:**

* [/sb/v4/adGroups](sponsored-brands/3-0/openapi/prod#tag/Ad-Groups/operation/CreateSponsoredBrandsAdGroups)

**Marketplace availability:** All marketplaces.


## Product collection ad

With the product collection ad type, you can promote your products from a landing page of your choice. For more information, see [product collection overview.](guides/sponsored-brands/ads/product-collection)

**Relevant resource:**

* [/sb/v4/ads/productCollection](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsProductCollectionAds)

**Marketplace availability:** All marketplaces.

## Store spotlight ad

Store spotlight ad type features a brand logo, a custom headline, and your Store on Amazon with at least three subpages. For more information, see [store spotlight overview](guides/sponsored-brands/ads/store-spotlight).

**Relevant resource:**

* [/sb/v4/ads/storeSpotlight](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandStoreSpotlightAds)

**Marketplace availability:** All marketplaces.

## Video and brand video ad

Video ad type allows you to promote a single product and link that video to a product detail page. Brand video ad type promotes your brand and can link to your Store. For more information between both video formats, see [video overview](guides/sponsored-brands/ads/video).

**Relevant resources:**

* [/sb/v4/ads/video](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsVideoAds)
* [/sb/v4/ads/brandVideo](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandsBrandVideoAds)

**Marketplace availability:** All marketplaces.


## Keyword targeting

With keyword targeting, Amazon matches your keywords to a customer's search terms and displays the branded headline and advertised products in your campaign.

**Relevant resource:**

* [/sb/keywords](sponsored-brands/3-0/openapi#tag/Keywords/operation/createKeywords)

**Marketplace availability:** All marketplaces. 

## Negative keyword targeting

Negative keywords prevent your keyword targeted ads from displaying when a shopper’s shopping queries match your negative keywords.

**Relevant resource:**

* [/sb/negativeKeywords](sponsored-brands/3-0/openapi#tag/Negative-keywords/operation/createNegativeKeywords)

**Marketplace availability:** All marketplaces.

## Product targeting

Product targeting allows you to choose specific products, categories, brands, or other product features that are relevant to the product in your ad.

**Relevant resource:**

* [/sb/targets](sponsored-brands/3-0/openapi#tag/Product-targeting/operation/createTargets)

**Marketplace availability:** 

* North America: United States (US), Canada (CA)
* Europe: Germany (DE), Spain (ES), France (FR), Italy (IT), United Kingdom (UK), India (IN)
* Asia Pacific: Japan (JP)

## Negative product targeting

Negative product targeting allows you to exclude certain products and brands from your targeting.

**Relevant resource:**

* [/sb/negativeTargets](sponsored-brands/3-0/openapi#tag/Negative-product-targeting/operation/createNegativeTargets)

**Marketplace availability:**

* North America: United States (US), Canada (CA)
* Europe: Germany (DE), Spain (ES), France (FR), Italy (IT), United Kingdom (UK), India (IN)
* Asia Pacific: Japan (JP)

## Theme-based targeting

Theme-based targeting is a curated set of keywords that enable brands to simplify the campaign creation process and improve campaign performance through well-performing targets.

**Relevant resource:**

* [/sb/themes](sponsored-brands/3-0/openapi#tag/Theme-targeting/operation/sbCreateThemes)

**Marketplace availability:**

* North America: United States (US), Canada (CA), Mexico (MX)
* South America: Brazil (BR)
* Europe: Germany (DE), Spain (ES), France (FR), Italy (IT), United Kingdom (UK), Belgium (BE), Poland (PL), Netherlands (NL), Sweden (SE), Turkey (TR)
* Middle East: United Arab Emirates (AE), Saudi Arabia (SA), Egypt (EG)
* Asia Pacific: Australia (AU), Japan (JP), India (IN), Singapore (SG)

## Exports

Exports provide metadata on all configured campaigns, ad groups, targeting expressions, and more, depending on the campaign type. For more information, please see [exports overview](exports/overview).

**Relevant resource:**

* [/campaigns/export](exports)

**Marketplace availability:** All marketplaces.

## Moderation

Moderation API allows you to get review process status of an ad or page  was approved or if changes need to be made.

**Relevant resource:**

* [/moderation/results](moderation#tag/Moderation-Results/operation/moderationResults)

**Marketplace availability:** All marketplaces.

## Pre moderation

Pre moderation API allows you to check different components of an ad or page to validate before submitting for moderation review. 

**Relevant resource:**

* [/premoderation](pre-moderation)

**Marketplace availability:** All marketplaces.
