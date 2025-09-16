---
title: DSP reporting overview
description: Understand the basics of using the Amazon Ads API for DSP reporting.
type: guide
interface: api
tags:
    - DSP
    - Reporting
---

# DSP reporting overview

The DSP reporting API helps advertisers access their [DSP campaign data](guides/dsp/overview). 

>[NOTE] The DSP Report APIs are scheduled for deprecation on June 30, 2025. Please migrate to [version 3 reporting](guides/reporting/v3/overview). For more information on upgrading, please see the [migration guide](reference/migration-guides/dsp-reporting).

## Report types

You can request reports with performance data grouped by different report types. 

For example, a campaign report includes metrics related to your campaign’s performance for your selected period. A geography report contains data based on your customers’ geographical data. 

Learn more about available [Report types](guides/reporting/dsp/report-types).

## Format

The API generates reports in either CSV or JSON format. Add your preferred format in your request. By default, reports are in JSON format.

For a sample report, see [Read your report](guides/reporting/dsp/get-started#read-your-report).

## Reporting period

You can specify the time-based aggregation level of your report, `timeUnit`, as either `DAILY` or `SUMMARY`. 

Learn more about [the timeUnit parameter](guides/reporting/dsp/get-started#timeUnit).

## Learn more

* [Get started with DSP reports](guides/reporting/dsp/get-started)
* [Metrics](guides/reporting/dsp/metrics)

