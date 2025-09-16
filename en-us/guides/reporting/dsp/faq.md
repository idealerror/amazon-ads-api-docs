---
title: DSP reporting FAQ
description: Get answers to frequently asked questions about retrieving reports for DSP campaigns using the Amazon Ads API. 
type: guide
interface: api
tags:
    - DSP
    - Reporting
---

# DSP reporting FAQ

## Will my integration cover all regions or are there any regional considerations I must understand?

Amazon DSP Reporting API is available in the following regions: NA (US and CA), EU (UK, DE, FR, IT, ES, SE, DK, FI, NO, and AE) and FE (JP and AU). The endpoints for each region are as follows:

* NA: [https://advertising-api.amazon.com](https://advertising-api.amazon.com/)
* EU: [https://advertising-api-eu.amazon.com](https://advertising-api-eu.amazon.com/)
* FE: [https://advertising-api-fe.amazon.com](https://advertising-api-fe.amazon.com/)

## How often is data updated via the Amazon DSP Reporting API?

Data is updated several times a day. Data retention and update schedules can be found [here](https://advertising.amazon.com/help/G2QE6RKPFG6C4KBH).

## How often should I pull a report?

We're currently recommending 1-2 report requests, per advertiser, per day, for each report type. This is based on data refresh rates.

## Why is my report empty?

If there's no performance data available for any campaigns on the requested dates, your report may contain no data or an empty JSON array (`[]`).

## My account has over 100 advertisers. How do I get reports for all of them?

You must pull multiple reports, each for 100 advertisers at a time.

## How do I avoid a “Report size too large” error?

DSP reports support a maximum of 250,000 rows. Try reducing the date range, the number of dimensions, and the number of metrics included in your request. Filter by specific orders IDs whenever possible. 

## I’m receiving a “Not Acceptable” error when using the API.

This is due to using the wrong version in the Accept header. Find the correct version in [our API documentation](guides/reporting/dsp/get-started#headers).

## How does reporting differ between the DSP console and the API?

Some reporting functionality is only available in the DSP console. Additionally, some API fields have different names in the DSP console. 

Learn more about [DSP console and the API](guides/reporting/dsp/reporting-by-account-type).

## I am using a manager account to call the DSP reporting API. Can I get access at the advertiser level to edit orders and line items?

No. For DSP, manager accounts can only be assigned read-only permission. You cannot use a manager account with read-only access to call DSP campaign management endpoints.

Learn more about [manager accounts](guides/account-management/authorization/manager-accounts).

## I’m receiving a 401 Unauthorized error when calling the API. 

This can be caused by several things:

* Ensure you’re using the correct [entity or advertiser ID](guides/reporting/dsp/reporting-by-account-type) in your URL. Remember entity IDs must be in all caps.
* Ensure the API authorizing user is a user on the Manager Account.
* Ensure the advertiser is correctly linked to the Manager Account.

## I’m receiving a 429 “Too Many Requests” error when trying to request my report.

Request reports less often or decrease the size of your requested reports. For best practices, learn more about [rate limiting in the Ads API](reference/concepts/rate-limiting).

If you use the DSP reporting API regularly, you may notice higher than normal throttles during periods of high demand. This is a known issue.

## How often should I pull a report? 

Based on DSP data refresh rates, we recommend 1-2 report requests per advertiser per day for each report type.

# What timezone are reports in?

By default, JSON reports give dates in UNIX epoch time, whereas CSV reports give dates in UTC.