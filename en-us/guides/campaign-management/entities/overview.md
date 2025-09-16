---
title: Campaign entities in the Amazon Ads API
description: Conceptual overview of campaign entities in the Amazon Ads API
type: guide
interface: api
tags:
    - Campaign management
keywords:
    - unified
    - entity guides
---

# Campaign management entities

Campaigns group ads that share common objectives and optimization strategies. Campaigns may also have budgets, fees and frequency caps settings that are defaults to all ad groups, targets, and ads that are associated with the campaign.

Specific attributes for each ad product are preserved in the common model based on the `adProduct` attribute of a given campaign or child entity.

For more information, see the [technical specification for the campaign management API](amazon-ads/1-0/openapi).

## Entity relationships

<div style="display: flex; flex-wrap: wrap; gap: 1.4em;">
    <div>

        ### Sponsored ads

        ![Campaign entity relationships chart - Sponsored ads](/_images/campaign-management/entities-sponsored-ads.png) 

    </div>
    <div>

        ### Amazon DSP

        ![Campaign entity relationships chart - ADSP](/_images/campaign-management/entities-adsp.png) 

    </div>
</div>

_The general structure of campaigns for sponsored ads and Amazon DSP._

## Entity guides

The following documents contain information about available attributes by ad product for each type of entity.

- [Campaign](guides/campaign-management/entities/campaign)
- [Ad group](guides/campaign-management/entities/ad-group)
- [Ad](guides/campaign-management/entities/ad)
- [Target](guides/campaign-management/entities/target)
- [Ad association](guides/campaign-management/entities/ad-association)
