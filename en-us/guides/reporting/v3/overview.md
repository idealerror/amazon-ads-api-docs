---
title: Sponsored ads reporting overview (version 3)
description: Understand the basics of using the Amazon Ads API for sponsored ads reporting (version 3).
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
    - Sponsored Display
---

# Version 3 reporting overview

>[NOTE] Sponsored Brands reports are currently available in preview. During the preview period, data related to Sponsored Brands campaigns with flag `isMultiAdGroupsEnabled=False` won’t be available. Once version 3 reporting supports all Sponsored Brands campaigns, we will announce general availability in the [release notes](release-notes/index).

Version 3 reporting in the Amazon Ads API improves the reporting experience for advertisers and integrators. 

>[TIP:Migration guide] To learn more about migration from version 2 to version 3, see our [Migration guide](reference/migration-guides/reporting-v2-v3).

## Report types

You can request reports with performance data broken down at different levels including campaign, ad group, ad, keyword, and target. For example, a campaign report contains performance data broken down by campaign for the specified day. An ad group report contains performance data broken down by ad group.  

Learn more about available [Report types](guides/reporting/v3/report-types/overview). 

## Format

You can specify your desired format as part of the request. Different formats are available based on the type of report you are requesting. To view available formats, see [Report types](guides/reporting/v3/report-types/overview). 

Most report types are available for download in a compressed (gzip) format. Once unzipped, report data is in a JSON format. 

For a sample report, see [Reading your report](guides/reporting/v3/get-started#reading-your-report).

## Reporting period 

As part of the request, you can specify the time period for your report. Different reporting periods are available based on the type of report you are requesting. To view reporting period by report type, see [Report types](guides/reporting/v3/report-types/overview). 

>[WARNING] The reporting APIs are designed to provide daily and summary performance report. If you need to receive data more frequently, you can use [Amazon Marketing Stream](guides/amazon-marketing-stream/overview), which offers hourly updates. Otherwise, you might experience throttling if you attempt too many consecutive calls using the reporting APIs. 

## Learn more

* [Get started with sponsored ads reports](guides/reporting/v3/get-started) 
* [Metrics](guides/reporting/v3/columns)