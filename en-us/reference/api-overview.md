---
title: Overview
description: Learn more about design, use cases, and set up for developers using the Amazon Ads API. 
type: guide
interface: api
tags:
    - Operations
---

# Amazon Ads API overview

The Amazon Ads API enables Amazon advertisers and partners to programmatically manage advertising operations and retrieve performance data. 
  		  
You can find reference documentation for the API by selecting from the **API reference** menu tree.

For user guides, recommendations, and tutorials, explore the **Developer guides** menu or see the [developer guides overview](guides/overview).

>[TOPIC:New to the Amazon Ads API?]Find useful first steps in our table of contents at left or in the list below:<ul><li>**[Onboarding](guides/onboarding/overview)**: step-by-step instructions for gaining access.</li><li>**[Getting started](guides/get-started/overview)**: step-by-step instructions for managing auth and making your first requests.</li></ul>

>[TOPIC:Amazon Ads API v1 is available]The new Amazon Ads API v1 is a reimagined approach to our Ads API, designed to provide a seamless experience across all Amazon advertising products through a common model. Campaign management operations are currently available in general release.<br> <br>For more details, see the [Amazon Ads API v1 overview](reference/amazon-ads/overview).

## Typical use cases

We expect a typical client will perform the following operations regularly:

1. Make batch requests for all campaigns, ad groups, ads and keywords in paginated requests and store/update a local copy of the data.
2. Request recent performance data for all campaigns, ad groups, ads and keywords in reports and use the IDs to associate and store/update performance data with the entities retrieved from a batch request.
3. Analyze performance and make changes to bids and budgets. Optimize performance using the batch update API for campaigns, ad groups, ads and keywords.

These, as well as operations to modify single entities, are supported use cases for the API.

## API endpoints

The API is accessible through the following regional hosts:

| URL | Region | Marketplaces |
| --- | --- | --- |
| https://<span></span>advertising-api.amazon.com | North America (NA) | United States (US), Canada (CA), Mexico (MX), Brazil (BR) |
| https://<span></span>advertising-api-eu.amazon.com | Europe (EU) | United Kingdom (UK), France (FR), Italy (IT), Spain (ES), Germany (DE), Netherlands (NL), United Arab Emirates (AE), Poland (PL), Turkey (TR), Egypt (EG), Saudi Arabia (SA), Sweden (SE), Belgium (BE), India (IN), South Africa (ZA) |
| https://<span></span>advertising-api-fe.amazon.com | Far East (FE) | Japan (JP), Australia (AU), Singapore (SG) |
