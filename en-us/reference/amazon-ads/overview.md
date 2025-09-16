---
title: Amazon Ads API v1
description: Overview of the Amazon Ads API v1
type: release-note
interface: api
keywords:
    - campaign management
    - common model
---

# Amazon Ads API v1

The Amazon Ads API v1 represents a reimagined approach to our Ads API, built from the ground up with consistency and efficiency in mind. The new API provides a seamless experience across all Amazon advertising products through a common model. This common model introduces standard naming and behavior for common concepts across Sponsored Products, Sponsored Brands, Sponsored Display, Sponsored Television, and Amazon DSP.

>[TIP:Getting started with Amazon Ads API v1]To learn about prerequisites, header values, and access, see [Getting started with Amazon Ads API v1](reference/amazon-ads/getting-started). To get started with campaign management, see the [campaign management overview](guides/campaign-management/overview) and [entity guides](guides/campaign-management/entities/overview).

## Key benefits

* **Consistent Experience**: Common field names, error handling, and resource patterns across all advertising products
* **Reduced Development Effort**: Significantly less code duplication when implementing features across multiple ad products
* **Single Integration**: Integrate once and access multiple ad products (Sponsored Products, Sponsored Brands, Sponsored Display, etc.) through a consistent API interface
* **Future-Ready**: New features and ad products will be built into this consistent framework
* **Predictable Updates**: Clear versioning strategy, support windows, and deprecation timelines for major versions

## Vision

The Amazon Ads API v1 establishes the foundation for all future Amazon Ads API development. Our vision includes:

* Building all new features and capabilities into the common model
* Expanding the common model across all of our advertising products
* Maintaining predictable release cycles and deprecation schedules
* Providing a seamless upgrade path for developers

## Developer experience

This new API structure is designed to make your development process more efficient:

* Consistent error codes and messaging across products
* Simplified documentation and implementation guides
* Reusable code patterns across different ad products
* Reduced time to market for new feature adoption
* Consistent OAS file and namespacing that makes for easy generation or integration into a client

## Currently supported operations

* [Campaign management](guides/campaign-management/overview) for Amazon DSP, Sponsored Brands, and Sponsored Products

For news on feature expansions and other updates, see the [release notes for the Amazon Ads API v1](release-notes/ads-api).

For detailed implementation information, please see the [Amazon Ads API v1 specification](amazon-ads/1-0/openapi).

### Beta support

Additional v1 APIs are currently available in beta states. See [Betas](developer/betas) for more information.

| API | Status |
| --- | ------ |
| [Account registration](guides/account-management/registration) | Closed beta |
| [Linked accounts](guides/account-management/linked-accounts) | Closed beta |
| [Forecasts](guides/dsp/forecasts) | Open beta |
| [Brand Stores](guides/brand-store/user-guide) | Open beta |

## Current limitations

### Campaign management scale

The north star is to allow multiple-ad product support within payload for writes and reads without scale differences across ad products.

- At launch, calls will be bound to one ad product at a time.
- At launch, API rate limits (TPS) will be per each ad product.

Following launch, we will:

- support additional ad products like Sponsored Display and Sponsored Television.
- enable support for multi-ad product operations in the same payload and move rate limiting to payload level, allowing mixing of ad products. This helps use cases such as bulk updates across ad products without orchestration of multiple write calls. 
- continue to increase the scaling across ad products to create a consistent TPS experience.

## Future Vision

Our long-term vision for the Ads API v1 is to continue expanding the coverage of Amazon's advertising products and features. We plan to generate all APIs in the Ads API v1 from a common domain model, ensuring consistent data representations and functionality across ad products. This will enable us to quickly add new ad products and features to the Ads API v1 while maintaining a consistent developer experience.

Some of the key areas we expect to expand in the future include support for:

- reporting
- recommendations
- rules 
- media planning

Please let us know which features you would like to see developed and generated from a common domain model first according to your priorities and where you see the most divergences today that cause you inefficiency. [You can submit feedback to our team via Jira](https://amzn-clicks.atlassian.net/servicedesk/customer/portal/2/group/2/create/6).
