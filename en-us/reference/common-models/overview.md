---
title: Amazon Ads API common model overview
description: Describes the common models and relationships for the Amazon Ads API.
type: guide
interface: api
tags:
    - Operations
---

# Amazon Ads API common model overview

The common model for the Amazon Ads API provides advertisers with standard way to refer to common concepts across our different ad products (Sponsored Products, Sponsored Brands, Sponsored Display, Sponsored Television, and Amazon DSP).

We currently support for common models for:

- [Campaigns](reference/common-models/campaigns)
- [Ad groups](reference/common-models/ad-groups)
- [Targets](reference/common-models/targets)
- [Ads](reference/common-models/ads)

We also support common [enums](reference/common-models/enums) that are referenced within each model.

## Hierarchy

All Amazon ad products share a similar structure. An advertising account can have multiple campaigns. Each campaign can have multiple ad groups, and ad groups can contain many ads and targets. 

![Ads common model high level relationships](/_images/common-model-high-level.png)
