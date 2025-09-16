---
title: Sponsored ads reporting FAQ (version 3) 
description: Get answers to frequently asked questions about version 3 reporting for sponsored ads campaigns using the Amazon Ads API. 
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
---

# Reporting frequently asked questions

Get answers to commonly asked questions about Amazon Ads version 3 reporting.

## About version 3

### What are the key needs for this latest API update?

Campaign reporting is currently fragmented across Amazon Ads products. This results in developers spending more time and resources accessing and reconciling data, and leaving campaign managers frustrated waiting for the metrics that they need to optimize. The API enhancements for reporting aim to alleviate these challenges by consolidating all campaign reporting data in one place, streamlining the process of turning raw data into actionable insights. Now, developers and campaign managers can spend less time organizing data and more time solving business challenges.

### What are the benefits of the new reporting updates to the API compared to version 2?

The key benefits that these reporting updates provide are:

1. All reports can be accessed from a single interface, have standardized metric names, and contain the same dimension names.
2. Date ranges up to 31 days, instead of one date at a time. 
3. Longer historical reporting window of up to 95 days (versus 60 days for version 2 reports). 
4. Common report schema for all ad programs. Currently we are starting with Sponsored Products and one Sponsored Brands report.
5. Supports JSON (GZIP files). Support for CSV files coming soon.

### What is the benefit of common request and response schemas? 

Currently, when reports for different ad products are obtained, it’s a challenge to use these reports directly as they have different request schemas and metric names. The version 3 reporting endpoint provides a standard request schema and metric names for all reports, so consolidation of data can be streamlined and used to gain insights faster.

### In what regions will the updated API be available?

The reporting updates are available worldwide (NA, EU, FE) wherever you can run sponsored ads.

### How do I start using the new API updates?

See [Get started with the version 3 reporting](guides/reporting/v3/get-started) for detailed instructions on getting access to the API, the API reference, and our getting started tutorial.

### What reports are currently available?

A complete list of reports and their mapping to version 2 reports can be found in our [Migration guide](reference/migration-guides/reporting-v2-v3#report-type-comparison).

## About version 2 deprecation

### Our integration uses the version 2 API specifications. Do we have to switch to the new specifications? 

Moving forward, there will be no changes to any existing version so you can continue using the version 2 for now. 

**We will be deprecating the version 2 Sponsored Products endpoints (version 2) on March 30th, 2023**, so we recommend upgrading as soon as possible. The new version adds upgraded historical data and date range functionality that could meet your data needs better. 

### Does this mean the version 2 reporting endpoints will be deprecated?

Yes, the current version 2 reporting endpoints for Sponsored Products will be deprecated on March 30th, 2023. This should give you sufficient time to migrate to the new schema. If you have any concerns with this timeline, contact the API support team at [ads-api-support@amazon.com](ads-api-support@amazon.com) or reach out to your Amazon partner account manager.

>[NOTE] Sponsored Brands and Sponsored Display reports on version 2 will continue to work after March 30th, 2023.

## General questions

### What should I do if I have issues connecting to the new API or consuming the reports from it?

You can report any difficulties connecting to the API by contacting the API support team at [ads-api-support@amazon.com](ads-api-support@amazon.com) or reach out to your partner account manager for more information.

### What's the difference between an export and a report?

Exports contain metadata about an account's campaign structure at the time the export request was made. Reports contain campaign performance data, and may also contain metadata. For best performance we recommend requesting all metadata via exports, then joining metadata and performance data using an identifier or name. 

- Learn more about [Reports](guides/reporting/overview).
- Learn more about [Exports](guides/exports/overview). 

### How long does it take for sponsored ads reporting data to become available?

Initial impression and click data is available using the API within 12 hours. Note that small changes to impression and click data may happen up to three days after the initial click date due to the [traffic validation process](https://advertising.amazon.com/help#GBYPYH79NGJJ5JPS). We recommend retrieving click and impression data again after three days to ensure you have the latest data. 

Initial conversion data is available within 24 hours, but may change as part of the restatement process. Restatements of conversion data occur 1, 7, and 28 days after the conversion event. Because conversions are reported on the date of the ad interaction, restatement includes all dates in your attribution window. For example, if your attribution window is 14 days, restatement can update attribution up to 42 days (14 plus 28) after the report date.

### How long does a report take to generate?

Report generation can take as long as three hours. 

Reports that request many fields may take longer to generate. To avoid large reports, you can:

- specify only necessary metrics in your report request. 
- make report requests that don't specify metadata (for example, `campaignName`, `campaignId`, `adGroupName`, `adGroupId`) in the report request body itself. Instead, retrieve metadata fields using an [export](guides/exports/overview) and then merge the retrieved metadata into your report. 

Repeated calls to check report status may generate a 429 response, indicating that your requests have been throttled. To retrieve reports programmatically, your application logic should institute a delay between requests. For more information, see [Retry logic with exponential backoff](reference/concepts/rate-limiting#use-retry-logic-with-exponential-backoff).

### How often should I pull a report?

We're currently recommending 1-2 report requests, per advertiser, per day, for each report type. This is based on data refresh rates.

### Why are my reports being rate limited?

You may experience report limiting during busy times of day. To avoid excessive rate limiting, spread report requests throughout the day rather than requesting all at once.

For more best practices, see [Rate limiting](reference/concepts/limits).

### Why does the search term report only have a subset of the items included in the advertising console report?

The search term report in the advertising console contains all search terms that had an impression, regardless of if that term resulted in a click. The API search term report only contains impressions that also had at least one click. Therefore you will see a difference in the number of entries between the console and API reports.

### Why is my report missing some Sponsored Brands records?

Sponsored Brands reports are currently available in preview. During the preview period, data related to Sponsored Brands campaigns with flag `isMultiAdGroupsEnabled=False` won’t be available. Once version 3 reporting supports all Sponsored Brands campaigns, we will announce general availability in the [release notes](release-notes/index).
