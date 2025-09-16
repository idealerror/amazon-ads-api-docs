---
title: Reporting in the advertising console vs. API (version 2)
description: Understand how reporting functionality differs in the advertising console and Amazon Ads API.  
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
    - Sponsored Display
---

# Advertising console vs. API reports

The [advertising console](https://advertising.amazon.com/en-us/sign-in) provides advertisers with another way to access reporting data. Reports generated through the Ads API are similar to the reports available in the advertising console, with a few key differences. 
 
## Reporting period

You can configure console reports for customized date ranges (e.g., one week, 90 days) while API reports contain only a single day's worth of data. To replicate a console report using the API, you must request a report for each day, then aggregate the data.

Reporting data is only available for 60 days via the API. Some console reports support longer reporting periods, so you may be able to access older data via the console. 

>[TIP:Version 3 reporting] In [version 3](guides/reporting/v3/overview) reporting, you can request reports for longer time periods, as well as request daily and summary reports. 

## Discrepancies

Small data discrepancies between advertising console and API reports may occur and are expected due to differences in the traffic validation and data refresh processes. If you experience a data discrepancy of greater than 5%, please contact the API support team at [ads-api-support@amazon.com](ads-api-support@amazon.com).

## Report type comparison

For descriptions of report types in the advertising console, see [Reports](https://advertising.amazon.com/help#GBYSPTSLR337JMLH).

The table below explains the relationship between API reports and console reports, and how to replicate console reports using the API.

**Note**: Matching API and console reports may support different metrics. See [API metrics and console report columns](#API-metrics-and-console-report-columns) for a comparison.

| Ad type | Console report | API report | Details |
|--------------|----------------|------------|------------------------------------------------------------------------------------------------------|
| Sponsored Products, Sponsored Brands, Sponsored Brands Video, Sponsored Display | Campaign | [Campaign](guides/reporting/v2/report-types#campaign-reports) or [ad group](guides/reporting/v2/report-types#ad-group-reports)  | The console and API campaign reports contain performance data at the campaign level. The API also provides an `adGroups` report with the same data broken out by ad group. The API reports provide additional attribution metrics for different day intervals (1, 7, 14, 30), but do not include calculated metrics found in the console report. |
| Sponsored Products, Sponsored Brands, Sponsored Brands Video | Placement | [Placement](guides/reporting/v2/report-types#placement) | The console and API placement reports return performance data at the campaign and placement level. |
| Sponsored Products, Sponsored Display | Advertised product | [Product ad](guides/reporting/v2/report-types#product-ad-reports) | The console advertised product report and the API product ad report return conversion data for advertised ASINs. |
| Sponsored Products, Sponsored Display | Purchased product | [ASIN](guides/reporting/v2/report-types#asin-reports) and [Product ad](guides/reporting/v2/report-types#product-ad-reports) | The API `productAds` report only includes conversion data for advertised ASINs. The API `asins` report contains conversion data for ASINs that were **not** advertised. Aggregate the API ASIN and product ad reports to get all purchased product data for both advertised and non-advertised products. This report is only available for sellers. |
| Sponsored Products, Sponsored Brands, Sponsored Brands Video | Targeting | [Keyword](guides/reporting/v2/report-types#keyword-reports) and [Target](guides/reporting/v2/report-types#target-reports) | The console targeting report is an aggregate of campaign keyword and targeting data. To create the same report using the the API, aggregate the data from the keyword and target reports. |
| Sponsored Products | Search terms | [Search term](guides/reporting/v2/report-types#search-term-reports) | The console search term report contains both keyword and targeting data broken out by search term. To recreate the console search term report, request both a target and keyword report segmented by `query`, then aggregate the data. *Note: Search terms only appear in the API report if they have at least one click while in the console report, search terms without clicks are included.* |
| Sponsored Brands, Sponsored Brands Video | Search term | [Search term](guides/reporting/v2/report-types#search-term-reports) | The console search terms report is an aggregate of all campaign targeting search terms data (keywords and product targets). The API report provides keyword search terms data using the query segment.  *Note: The Advertising API does not support segmenting Sponsored Brands target reports by query at this time. Additionally, search terms only appear in the API report if they have at least one click while in the console report, search terms without clicks are included. Because of these differences, the API report will not be equivalent to the console report.*  |
| Sponsored Display | Targeting | [Target](guides/reporting/v2/report-types#target-reports) |  The API report provides the same data through the target report. The targeting tactic must be specified in the report request payload. Advertisers running Sponsored Display using different tactics should request a report for each tactic (`T00020` and `T00030`) and aggregate the data.  |

>[NOTE] To recreate the Sponsored Brands **Category Benchmark** report, use the [Category Benchmark API](sponsored-brands/3-0/category-benchmark).


### Unsupported report types

The following console report types are not available using the Amazon Ads API:

- Sponsored Products Performance Over Time
- Sponsored Products and Sponsored Brands Search Term Impression Share
- Sponsored Products Budget

## Metric differences


Metrics names in the advertising console may differ from metric names used by the API. For descriptions of each API metric, see [Metrics](guides/reporting/v2/metrics). 

### Advertising console reports 

For more information about console report columns, see [Report column definitions](https://advertising.amazon.com/help#GH4CM33BLUN3XUYL).

>[NOTE] Sponsored Display campaigns with vCPM costType should use the new view- and click-based metrics (viewAttributedConversions14d, viewAttributedDetailPageView14d, viewAttributedSales14d, viewAttributedUnitsOrdered14d, viewImpressions)

#### Attribution metrics

**Sponsored Products sellers**

|Console report	|Report column	|API metric	|
|--------|--------|--------|
|Targeting, Search term, Advertised product	|7 Day Advertised SKU Units	|[attributedUnitsOrdered7dSameSKU](guides/reporting/v2/metrics#attributedunitsordered7dsamesku)	|
|Purchased Product, Advertised product, Targeting, Search term	|7 Day Other SKU Units	|[attributedUnitsOrdered7dOtherSKU](guides/reporting/v2/metrics#attributedunitsordered7dothersku)	|
|Campaign, Targeting, Advertised product, Placement, Search term	|7 Day Total Orders	|[attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d)	|
|Placement, Targeting, Advertised product, Search term	|7 Day Total Units	|[attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedconversions7d)	|
|Targeting, Advertised product, Search term	|7 Day Conversion Rate	|[attributedConversions7d](guides/reporting/v2/metrics#attributedconversions7d) divided by [clicks](guides/reporting/v2/metrics#clicks)	|
|Targeting, Advertised product, Search term	|7 Day Advertised SKU Sales	|[attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedsales7dsamesku)	|
|Campaign, Targeting, Advertised product, Placement, Search term	|7 Day Total Sales	|[attributedSales7d](guides/reporting/v2/metrics#attributedsales7d)	|
|Purchased Product, Advertised product, Targeting, Search term	|7 Day Other SKU Sales	|[attributedSales7dOtherSKU](guides/reporting/v2/metrics#attributedsales7dothersku)	|

**Sponsored Products vendors**

|Report	|Report column	|API metric	|
|---	|---	|---	|
|Placement, Campaign, Targeting, Search term	|14 Day Total Orders	|[attributedConversions14d](guides/reporting/v2/metrics#attributedconversions14d)	|
|Targeting, Placement, Search term	|14 Day Total Units	|[unitsSold14d](guides/reporting/v2/metrics#unitssold14d)	|
|Targeting	|14 Day Conversion Rate	|[attributedConversions14d](guides/reporting/v2/metrics#attributedconversions14d) divided by [clicks](guides/reporting/v2/metrics#clicks)	|
|Placement, Campaign, Targeting	|14 Day Total Sales	|[attributedSales14d](guides/reporting/v2/metrics#attributedSales14d)	|


**Sponsored Brands and Sponsored Display (all account types)**

|Ad Type	|Report	|Report column	|API metric	|
|-----------|-------|----------|--------|
|Sponsored Brands	|Keyword, Keyword placement, Campaign, Campaign placement. Search term	|14 Day Total Sales	|[attributedSales14d](guides/reporting/v2/metrics#attributedSales14d)	|
|Sponsored Display	|Campaign, Targeting	|14 Day Total Sales	|[attributedSales14d](guides/reporting/v2/metrics#attributedSales14d)	|
|Sponsored Brands	|Keyword, Keyword placement, Campaign, Campaign placement, Search term	|14 Day Total Orders	|[attributedConversions14d](guides/reporting/v2/metrics#attributedconversions14d)	|
|Sponsored Display	|Campaign, Targeting	|14 Day Total Orders	|[attributedConversions14d](guides/reporting/v2/metrics#attributedconversions14d)	|
|Sponsored Brands	|Keyword, Keyword placement, Campaign, Campaign placement, Search term	|14 Day Total Units	|[unitsSold14d](guides/reporting/v2/metrics#unitssold14d)	|
|Sponsored Display	|Campaign, Targeting	|14 Day Total Units	|[unitsSold14d](guides/reporting/v2/metrics#unitssold14d)	|
|Sponsored Brands	|Keyword, Keyword placement, Campaign, Campaign placement, Search term	|14 Day Conversion Rate	|[attributedConversions14d](guides/reporting/v2/metrics#attributedconversions14d)divided by [clicks](guides/reporting/v2/metrics#clicks)	|
|Sponsored Display	|Campaign, Targeting	|14 Day Conversion Rate	|[attributedConversions14d](guides/reporting/v2/metrics#attributedconversions14d)divided by [clicks](guides/reporting/v2/metrics#clicks)	|
|Sponsored Brands	|Purchased report	|14 Day New-to-brand Orders	|[newToBrandPurchases14d](guides/reporting/v3/columns#newtobrandpurchases14d)	|
|Sponsored Display	|Campaign, Targeting	|14 Day New-to-brand Orders	|[attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedordersnewtobrand14d)	|
|Sponsored Brands	|Purchased report	|14 Day New-to-brand sales	|[newToBrandSales14d](guides/reporting/v3/columns#newtobrandsales14d)	|
|Sponsored Display	|Campaign, Targeting	|14 Day New-to-brand sales	|[attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedsalesnewtobrand14d)	|
|Sponsored Brands	|Purchased report	|14 Day New-to-brand Units	|[newToBrandUnitsSold14d](guides/reporting/v3/columns#newtobrandunitssold14d)	|
|Sponsored Display	|Campaign, Targeting	|14 Day New-to-brand Units	|[attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedunitsorderednewtobrand14d)	|

#### Other metrics

| Ad Type | Console report | Console column name | API metric |
|---------|--------|-----------|------------|
| Sponsored Products | Campaign, Placement, Advertised product, Targeting, Search term | Spend | [cost](guides/reporting/v2/metrics#cost) |
| Sponsored Products | Campaign, Placement, Advertised product, Targeting, Search term | Cost-Per-Click (CPC) | [cost](guides/reporting/v2/metrics#cost) divided by [clicks](guides/reporting/v2/metrics#clicks) |
| Sponsored Products | Campaign, Placement, Advertised product, Targeting, Search term | Click-Thru Rate (CTR) | [impressions](guides/reporting/v2/metrics#impressions) divided by [clicks](guides/reporting/v2/metrics#clicks) |
| Sponsored Products | Advertised product | Advertised asin | [asin](guides/reporting/v2/metrics#asin) |
| Sponsored Products | Campaign, Placement, Advertised product, Targeting, Search term | Total Advertising Cost of Sales (ACoS)  | [cost](guides/reporting/v2/metrics#cost) divided by [attributedSales14d](oncepts/reporting/metrics#attributedSales14d) |
| Sponsored Products | Campaign, Placement, Advertised product, Targeting, Search term | Total Return on Advertising Spend (RoAS)  | [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d) divided by [cost](guides/reporting/v2/metrics#cost)  |
| Sponsored Products, Sponsored Brands | Search term | Customer search term | [query](guides/reporting/v2/metrics#query) |
| Sponsored Display | Campaign, Targeting | Impressions | [impressions](guides/reporting/v2/metrics#impressions) |
| Sponsored Display | Campaign, Targeting | Viewable Impressions (vCPM) | [viewableImpressions](guides/reporting/v2/metrics#viewableimpressions) |
| Sponsored Display | Campaign, Targeting | Clicks | [clicks](guides/reporting/v2/metrics#clicks) |
| Sponsored Display | Campaign, Targeting | Click-Thru Rate (CTR) | [impressions](guides/reporting/v2/metrics#impressions) divided by [clicks](guides/reporting/v2/metrics#clicks) |
| Sponsored Display | Campaign, Targeting | 14 Day Detail Page Views (DPV) | [attributedDetailPageView14d](guides/reporting/v2/metrics#attributeddetailpageview14d) |
| Sponsored Display | Campaign, Targeting | Spend | [cost](guides/reporting/v2/metrics#cost) |
| Sponsored Display | Campaign, Targeting | Cost-Per-Click (CPC) | [cost](guides/reporting/v2/metrics#cost) divided by [clicks](guides/reporting/v2/metrics#clicks) |
| Sponsored Display | Campaign, Targeting | Cost per 1,000 viewable impressions (vCPM) | ([cost](guides/reporting/v2/metrics#cost) divided by [viewableImpressions](guides/reporting/v2/metrics#viewableimpressions)) times 1,000 |
| Sponsored Display | Campaign, Targeting | Total Advertising Cost of Sales (ACoS) | [cost](guides/reporting/v2/metrics#cost) divided by [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d) |
| Sponsored Display | Campaign, Targeting | Total Return on Advertising Spend (RoAS) | [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d) divided by [cost](guides/reporting/v2/metrics#cost) |

### Advertising console campaigns UI

The advertising console campaigns UI contains a small subset of metrics for campaign performance. The values associated to these metrics differ based on the account type. Use the tables below to understand the values for different account and ad types.

![Advertising console campaigns UI metrics](/_images/console-campaigns-ui.png)

#### Sponsored Products (vendors), Sponsored Brands, & Sponsored Display

|Console campaigns UI	| Console report | API	|
|---	|---	|---	|
| Orders	| 14 Day Total Orders	| [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d)	|
| Sales	| 14 Day Total Sales	| [attributedSales14d](guides/reporting/v2/metrics#attributedsales14d)	|
| Orders (vCPM)	| 14 Day Total Orders	| [viewAttributedConversions14d](guides/reporting/v2/metrics#viewattributedconversions14d)	|
| Sales (vCPM)	| 14 Day Total Sales	| [viewAttributedSales14d](guides/reporting/v2/metrics#viewattributedsales14d)	|


#### Sponsored Products sellers

|Console campaigns UI	| Console report | API	|
|---	|---	|---	|
| Orders	| 7 Day Total Orders	| [attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d)	|
| Sales	| 7 Day Total Sales	| [attributedSales7d](guides/reporting/v2/metrics#attributedsales7d)	|