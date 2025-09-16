---
title: Ad types overview
description: An overview for ad types for Sponsored Brands
type: guide
interface: api
tags: 
    - Sponsored Brands
keywords:
    - Ad types
    - Overview
---


# Ad overview

Sponsored Brands ads allow you to use different [ad formats](https://advertising.amazon.com/help?ref_=AAC_unav_support_center#GGWFYHL27MFXLHS6) to showcase your brand. Depending on your campaign objectives and ad format you can choose between [product collection](guides/sponsored-brands/ads/product-collection), [store spotlight](guides/sponsored-brands/ads/store-spotlight), or [video](guides/sponsored-brands/ads/video).


* **Product collection**: Use this format when you would like to feature a brand logo along with a selection of your products. When Amazon shoppers click a product's image, it takes them to the product detail page.
* **Store spotlight**: Use this format when you want to promote a store, including its subpages.
* **Video:** Use this format when you want to feature a horizontal or a vertical video telling your brand story, and being able to link to a product detail page or store. Campaigns leading to a store can select a variety of advertised products, or display a video-only creative.


Ads generally contain these components:

* Brand details, such as a logo and name
* Creative asset
* Landing page
* Advertised products




## Creative asset

A creative asset can either be an image or video. An example of an image creative would be your brand logo or a custom image that represents your products or brand in a lifestyle setting. You can also showcase your brand or products using a video creative.

>[NOTE]  You need to upload all video and image assets using the [creative asset library API](guides/creative-asset/asset-library-overview). Use the `assetId` returned by the asset library API in all ad creation requests.

## Landing page

The landing page is where shoppers are directed after they interact with your ad. Landing page options are custom (your own URL), your Store on Amazon, or a product detail page. You can assign different landing pages to each ad within your adgroup and edit them.

Below are restrictions where a landing page update is not allowed:

| Details | Product Collection | Store Spotlight | Brand Video | Video - product detail page |
| --- | --- | --- |
| Change new landing page to a custom URL landing page | Permitted | N/A | N/A | N/A | 
| Change store landing page to a custom URL landing page | Permitted | Permitted | Permitted | N/A | 
| Change store landing page to a new landing page | Permitted | Permitted | Permitted | N/A | 
| Change custom URL landing page to a store landing page | Permitted | N/A | N/A | N/A | 
| Change custom URL landing page  to a new landing page | Permitted | N/A | N/A | N/A | 
| Change store landing page to a new store landing page | Permitted | Permitted | Permitted | N/A | 
| Change store landing page and to a product detail page landing page | N/A | N/A | N/A | Not permitted | 
| Change product detail landing page to store landing page | N/A | N/A | N/A | Not permitted | 
| Change one store page to another store main or sub-page within the same store | Permitted | permitted | permitted | N/A | 
| Advertiser with New LP wants to add/remove ASINs on their existing landing page that are not featured in the ad | Permitted | N/A | N/A | N/A | 
| Add or remove AINs not featured in the ad on an existing landing page  | Permitted | N/A | N/A | N/A | 
| Add or remove ASINs featured in the ad on an existing
landing page | Permitted | N/A | N/A | N/A | 
| Add or remove ASINs not featured in your ad for your stores landing page | Permitted | Permitted | Permitted | N/A | 
| Add or remove ASINs featured in your ad for your stores landing page  | Permitted | Permitted | Permitted | N/A | 