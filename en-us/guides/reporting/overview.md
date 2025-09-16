---
title: Amazon Ads reporting overview
description: Conceptual overview of reporting in the Amazon Ads API
type: guide
interface: api
tags:
    - Reporting
keywords:
    - data freshness
    - data consistency
    - different data
---

# Reporting overview

The Amazon Ads API provides a number of reporting features to help advertisers understand and optimize their campaign performance.

## Reporting tools

| Feature                                                                    | Description                                                                                    |
|----------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| [Version 3 reporting](guides/reporting/v3/overview)                        | Daily and summary performance reports for sponsored ads and Amazon DSP campaigns               |
| [Version 2 reporting (sponsored ads)](guides/reporting/v2/overview)        | 1-day performance reports for sponsored ads campaigns                                          |
| [Amazon Marketing Stream](guides/amazon-marketing-stream/overview)         | Hourly-grain traffic, conversion, and budget usage data                                        |
| [Amazon DSP reporting](guides/get-started/first-call)                      | Performance reports for Amazon DSP campaigns                                                   |
| [Amazon Attribution (beta)](guides/amazon-attribution/overview)            | Measures the impact of non-Amazon marketing strategies on Amazon shopping activity             |
| [Amazon Brand Metrics (beta)](guides/reporting/brand-metrics/overview)     | Quantifies opportunities for your brand at each stage of the customer journey on Amazon        |
| [Marketing Mix Modeling](guides/reporting/marketing-mix-modeling/overview) | Aggregated Amazon sales and media performance reports needed for marketing mix modeling (MMM). |

## Data freshness 

### Sponsored ads

Initial impression and click data from sponsored ads campaigns is available using the API within 12 hours.

Amazon validates every click to make sure advertisers are not charged unnecessarily. Most of the invalid traffic at post-impression stage is invalidated in real-time. However, some invalid traffic takes more time to detect. Our invalid traffic detection system monitors the traffic data for up to 30 days after the fact to recognize invalid traffic patterns. We recommend waiting at least 5 days for the most accurate click data to be available before evaluating campaign performance.

Learn more about [Traffic validation](https://advertising.amazon.com/help#GBYPYH79NGJJ5JPS) for clicks on sponsored ads campaigns.

Conversion data is refreshed and validated at daily, weekly, and monthly intervals, and is subject to change for up to 60 days back from the current date. We recommend re-retrieving reports monthly to ensure your conversion metrics are up to date.

### DSP

For DSP campaigns, Amazon has a process in place to validate impression data and ensure advertisers are not billed for invalidated impressions. Most of the invalid traffic at post-impression stage is invalidated in real-time. However, some invalid traffic takes more time to detect. Our invalid traffic detection system monitors the traffic data for up to 72 hours after fact to recognize invalid traffic patterns.

For details, see [Gross and invalid traffic metrics](https://advertising.amazon.com/dsp/help/ss/en#GTPS2R55RD5B5WBJ).

## Reports vs. exports

Reports include performance data like impressions or clicks for a chosen report type. Exports contain campaign metadata like budget or status. For example, a target export for Sponsored Products returns all the targeting clauses associated with Sponsored Products campaigns, while a target report contains performance data broken down at the target level. 

While reports can include certain metadata, we recommend requesting any metadata using the export resource for improved performance.

Learn more about [Exports](guides/exports/overview). 

## Rate limits

Due to the large volume of report requests, you may encounter a limitation on the number of calls you can make. Rate limited calls receive a `429` response code. Rate liming is dynamic and based on the time of day and number of requests. We recommend scheduling your reports throughout the day to reduce occurrences of rate limiting.

Learn more about [Rate limiting](reference/concepts/rate-limiting). 

