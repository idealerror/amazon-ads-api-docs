---
title: Campaign management in the Amazon Ads API
description: Conceptual overview of campaign management in the Amazon Ads API
type: guide
interface: api
tags:
    - Campaign management
keywords:
    - unified
    - campaign entity
---

# Campaign management in the Amazon Ads API

Amazon Ads' campaign management APIs enable advertisers to create, read, and update campaigns across multiple ad products using consistent data models.

>View the [technical specification for the campaign management API](amazon-ads/1-0/openapi).

The campaign management APIs support Amazon DSP, Sponsored Brands, and Sponsored Products at launch, and will expand to support Sponsored Display and Sponsored Television in 2025.

>[TIP:Amazon Ads API v1]The campaign management API is an aspect of Amazon Ads API v1, a reimagined approach to our Ads API, built from the ground up with consistency and efficiency in mind. To learn more, see the [Amazon Ads API v1 overview](reference/amazon-ads/overview).<br><br>For new users of the Amazon Ads API, we recommend creating your integration using these campaign management endpoints, as these will eventually fully replace the ad product-specific APIs. Existing integrators can migrate to the campaign management API or continue using product-specific campaign management endpoints.

## Campaign management entities

The campaign management API normalizes terminology across ad products, breaks out endpoints that combine multiple concepts, and modifies API schemas to model concepts consistently. The API currently covers five industry-standard entities: campaigns, ad groups, targets, ads, and ad associations.

See the [entity guides overview](guides/campaign-management/entities/overview) or these individual guides for information about each entity:

- [Campaign](guides/campaign-management/entities/campaign)
- [Ad group](guides/campaign-management/entities/ad-group)
- [Ad](guides/campaign-management/entities/ad)
- [Target](guides/campaign-management/entities/target)
- [Ad association](guides/campaign-management/entities/ad-association)
