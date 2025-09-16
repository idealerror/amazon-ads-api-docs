---
title: Exports overview
description: Learn about exports in the Amazon Ads API.  
type: guide
interface: api
---

# Exports

>[NOTE] Exports are a replacement for the [snapshots](guides/snapshots/overview) functionality. To learn more, view the [migration guide](reference/migration-guides/snapshots-exports).

Use exports to efficiently retrieve the structure of your Amazon Ads campaigns. Exports are generated asynchronously and can be requested at multiple levels. 

## Exports vs. reports

While reports contain performance data for a specified day, exports provide metadata about your campaigns at the time of the request. In other words, you can use exports to retrieve metadata on all configured campaigns, ad groups, targeting expressions, and more, depending on the campaign type. 

## Exports vs. list operations

Exports return similar information to GET and GET /extended operations for campaigns, ad groups, keywords, and other campaign entities. Exports run asynchronously while GET and GET /extended are synchronous. If you have many campaigns or a complex campaign structure and are looking to pull information on all campaigns in your account, we suggest using exports.

If you need details for a single campaign, use a GET request with query parameters instead of an export. 

## Sample export output

View sample JSON for:

- [Campaigns](reference/common-models/campaigns)
- [Ad groups](reference/common-models/ad-groups)
- [Targets](reference/common-models/targets)
- [Ads](reference/common-models/ads)

## Export availability by ad type

Each ad product supports different types of exports. 

| Export type | Sponsored Products | Sponsored Brands | Sponsored Display |
|---------------|----|----|----|
| campaigns | x | x | x |
| adGroups | x | x | x |
| targets | x | x | x |
| ads | x | x | x |

## Fields by ad product and export type

Exports follow the common model for the entity type requested. Each ad product can contain a subset of the available fields for that entity. 

To view all fields available, see:

- [Campaign common model](reference/common-models/campaigns)
- [Ad group common model](reference/common-models/ad-groups)
- [Targets common model](reference/common-models/targets)
- [Ads common model](reference/common-models/ads)

You can use the following tables to understand which fields are available for each ad product, and how each fields maps to the campaign management APIs:

- [Campaign mapping table](reference/common-models/campaigns#ad-product-mapping)
- [Ad group mapping table](reference/common-models/ad-groups#ad-product-mapping)
- [Target mapping table](reference/common-models/targets#ad-product-mapping)
- [Ads mapping table](reference/common-models/ads#ad-product-mapping)

## Frequently asked questions

### What is the exports API's intended use case?

The intended use case of the exports API is to allow customers who manage multiple advertisers to synchronize their advertising data with Amazon’s advertising data one to two times a day. Customers should not use the exports API to continually retrieve real-time advertising data.

### How does throttling work for the exports API?

The exports API uses a queue-based throttling system, where all developers who onboard to the API are assigned a cap on how many in-progress requests they can have at once per endpoint. The default cap for all customers is 5 jobs per endpoint. 

For example, a customer can only request 5 exports with entity type ‘campaign’ at once, and then the customer must wait for at least one request to complete before they can submit another request for a campaign export. To request an increase to your job limit, please contact our [support team](support/overview).

## Learn more

- [API reference](exports)
- [Export APIs guide (PDF)](https://m.media-amazon.com/images/S/aapn-assets-prod/3b27bf03-bfe7-4484-9628-aca0a22567c6)
