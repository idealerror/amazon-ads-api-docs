---
title: Brand Stores management overview
description: Learn how to use 
type: guide
interface: api 
tags:
    - Brand Stores
keywords:
    - brand
    - brand store
    - brand store metrics
---

# Brand Stores management (beta)

The Brand Stores management APIs offer brands and advertisers programmatic access to manage their Amazon Brand Stores more efficiently. These APIs, now available through the Amazon Ads API, provide two key functions: 

- retrieve Brand Store page content and update the product content and selection across Brand Stores pages
- submit store updates for moderation

These functionalities can be combined with Brand Home APIs to automate and maintain product information freshness, creating a more efficient and streamlined management process for Brand Stores on Amazon.

[View the technical specifications for the Brand Stores management APIs.](amazon-ads/1-0/brand-stores)

## Brand Stores management endpoints

|API	|Description	|API Specs Link	|
|---	|---	|---	|
|POST: /adsApi/v1/update/brandStoreEditionPublishVersions	|Update state to submit the store publish version for moderation	|[Link](amazon-ads/1-0/brand-stores#tag/BrandStoreEditionPublishVersions/operation/UpdateBrandStoreEditionPublishVersion)	|
|POST: /adsApi/v1/query/brandStoreEditionPublishVersions	|Retrieve the available store publish version	|[Link](amazon-ads/1-0/brand-stores#tag/BrandStoreEditionPublishVersions/operation/QueryBrandStoreEditionPublishVersion)	|
|GET: /adsApi/v1/brandStoreEditions	|Retrieve the brand store edition information	|[Link](amazon-ads/1-0/brand-stores#tag/BrandStoreEditions/operation/ListBrandStoreEdition)	|
|POST: /adsApi/v1/update/brandStorePages	|Update product selection on brand store page	|[Link](amazon-ads/1-0/brand-stores#tag/BrandStorePages/operation/UpdateBrandStorePage)	|
|POST: /adsApi/v1/query/brandStorePages	|Retrieve brand store page content	|[Link](amazon-ads/1-0/brand-stores#tag/BrandStorePages/operation/QueryBrandStorePage)	|

