---
title: Sponsored ads reporting overview (version 2)
description: Understand the basics of using the Amazon Ads API for sponsored ads reporting (version 2).
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
    - Sponsored Display
---

# Version 2 reporting overview

The version 2 reporting APIs help advertisers access performance data for sponsored ads (Sponsored Products, Sponsored Display, and Sponsored Brands).

>[TIP:Migration guide] To learn more about migration from version 2 to version 3, see our [Migration guide](reference/migration-guides/reporting-v2-v3).

>[WARNING] As of March 30, 2023, Sponsored Products reports are no longer supported in version 2. Use the [version 3 reporting endpoints](release-notes/archive/ads-api#announcing-reporting-enhancements-for-sponsored-products-and-sponsored-brands) going forward. For more details on how to upgrade to version 3, see our [Migration guide](reference/migration-guides/reporting-v2-v3) and [Frequently asked questions](guides/reporting/v3/faq).

## Report types

You can request reports with performance data broken down at different levels including campaign, ad group, ad, keyword, and target. For example, a campaign report contains performance data broken down by campaign for the specified day. An ad group report contains performance data broken down by ad group.  

Learn more about available [Report types](guides/reporting/v2/report-types). 

## Format

Reports are downloaded in a compressed (gzip) format. Once unzipped, report data is in a JSON format.

For a sample report, see [Reading your report](guides/reporting/v2/sponsored-ads-reports#reading-your-report).

## Reporting period 

For sponsored ads reports, each call to the API requests a single day’s worth of data. To get performance data for a longer reporting period, you need to make multiple calls to the API then aggregate the data. 

Reporting data is only available up to 60 days back from the current date via the API. 

>[TIP] For longer reporting periods, see the [Version 3 APIs](guides/reporting/v3/overview).

## Advertising console vs. API reports

Both the [advertising console](https://advertising.amazon.com/en-us/sign-in) and API provide mechanisms for retrieving sponsored ads campaign performance data. 

Learn more about [reporting differences between the advertising console and API](guides/reporting/v2/advertising-console).

To understand best practices around data freshness, see [Frequently asked questions](guides/reporting/v2/faq).

> [TIP:Data discrepancies] Small data discrepancies between advertising console and API reports may occur and are expected due to differences in the traffic validation and data refresh processes. If you experience a data discrepancy of greater than 5%, contact the API support team at [ads-api-support@amazon.com](ads-api-support@amazon.com).

## Learn more

* [Get started with sponsored ads reports](guides/reporting/v2/sponsored-ads-reports) 
* [Metrics](guides/reporting/v2/metrics)
* [Advertising console vs. API reports](guides/reporting/v2/advertising-console)
* [Frequently asked questions](guides/reporting/v2/faq)

