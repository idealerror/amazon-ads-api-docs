---
title: Sponsored ads reporting metrics (version 2)
description: View descriptions for all sponsored ads version 2 reporting metrics supported by the Amazon Ads API. 
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
    - Sponsored Display
---

# Metrics

Every report in the Ads API is made of metrics. You specify what metrics the report should include in your report request body. Each report type supports different metrics, and some report types may support different metrics based on the ad type. 

## Sponsored ads

Metrics can be classified into three categories based on the type of data they represent:

* [Performance metrics](#performance-metrics)
* [Dimensional metrics](#dimensional-metrics)
* [Metadata](#metadata)

> [TIP] The column names in [advertising console](https://advertising.amazon.com/sign-in) reports may differ from API report metrics. In addition, the API does not support calculated metrics like cost per click or cost per acquisition that you see in console reports. To view equivalencies and calculation instructions, see [Console vs. API metrics](guides/reporting/v2/advertising-console#api-metrics-and-console-report-columns). 

### Performance metrics 

Most of your report should be made up of performance metrics. The performance metrics available for each report type may change based on the ad type. 

#### attributedBrandedSearches14d

**Type**: Integer

**Description**: The number of searches that included the name of your brand occurring within 14 days of an ad click.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedConversions14d

**Type**: Integer

**Description**: Number of attributed conversion events occurring within 14 days of an ad click. For Sponsored Brands only, this applies to attributed conversion events occurring within 14 days of an ad click or view.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedConversions14dSameSKU

**Type**: Integer

**Description**: Number of attributed conversion events occurring within 14 days of ad click where the purchased SKU was the same as the SKU advertised. For Sponsored Brands only, this applies to attributed conversion events occurring within 14 days of an ad click or view.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedConversions1d

**Type**: Integer

**Description**: Number of attributed conversion events occurring within 24 hours of ad click.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedConversions1dSameSKU

**Type**: Integer

**Description**: Number of attributed conversion events occurring within 24 hours of ad click where the purchased SKU was the same as the SKU advertised.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedConversions30d

**Type**: Integer

**Description**: Number of attributed conversion events occurring within 30 days of an ad click.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedConversions30dSameSKU

**Type**: Integer

**Description**: Number of attributed conversion events occurring within 30 days of ad click where the purchased SKU was the same as the SKU advertised.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedConversions7d

**Type**: Integer

**Description**: Number of attributed conversion events occurring within seven days of an ad click.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedConversions7dSameSKU

**Type**: Integer

**Description**: Number of attributed conversion events occurring within seven days of ad click where the purchased SKU was the same as the SKU advertised.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedDetailPageView14d

**Type**: Integer

**Description**: *This metric is in beta.* Number of attributed detail page views occurring within 14 days of ad click.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

#### attributedDetailPageViewsClicks14d

**Type**: Integer

**Description**: The number of attributed detail page view conversions occurring within 14 days of ad click. 

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports)

<hr />

#### attributedKindleEditionNormalizedPagesRead14d

**Type**: Integer

**Description**: Number of attributed Kindle edition normalized pages read within 14 days of ad click.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [ASIN](guides/reporting/v2/report-types#asin-reports), [Target](guides/reporting/v2/report-types#target-reports)

<hr />

#### attributedKindleEditionNormalizedPagesRoyalties14d

**Type**: Integer

**Description**: The estimated royalties of attributed estimated Kindle edition normalized pages within 14 days of ad click.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [ASIN](guides/reporting/v2/report-types#asin-reports), [Target](guides/reporting/v2/report-types#target-reports)

<hr />

#### attributedOrderRateNewToBrand14d

**Type**: Decimal

**Description**: The number of new-to-brand orders relative to the number of clicks. New-to-brand order rate = new-to-brand orders / clicks. Not available for book vendors.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports)

<hr />

#### attributedOrdersNewToBrand14d

**Type**: Integer 

**Description**: The number of first-time orders for brand products over a one-year lookback window. Not available for book vendors. For Sponsored Brands only, this applies to attributed conversion events occurring within 14 days of an ad click or view.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedOrdersNewToBrandPercentage14d

**Type**: Decimal 

**Description**: The percentage of total orders that are new-to-brand orders. Not available for book vendors. For Sponsored Brands only, this applies to attributed conversion events occurring within 14 days of an ad click or view.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports)

<hr />

#### attributedSales14d

**Type**: Decimal

**Description**: Total value of sales occurring within 14 days of an ad click. For Sponsored Brands only, this applies to attributed conversion events occurring within 14 days of an ad click or view.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedSales14dOtherSKU

**Type**: Decimal

**Description**: Total value of sales occurring within 14 days of an ad click where the purchased SKU was different from the SKU advertised.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### attributedSales14dSameSKU

**Type**: Decimal

**Description**: Total value of sales occurring within 14 days of ad click where the purchased SKU was the same as the SKU advertised. For Sponsored Brands only, this applies to attributed conversion events occurring within 14 days of an ad click or view.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedSales1d

**Type**: Decimal

**Description**: Total value of sales occurring within 24 hours of an ad click.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedSales1dOtherSKU

**Type**: Decimal

**Description**: Total value of sales occurring within 24 hours of an ad click where the purchased SKU was different from the SKU advertised.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### attributedSales1dSameSKU

**Type**: Decimal

**Description**: Total value of sales occurring within 24 hours of ad click where the purchased SKU was the same as the SKU advertised.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedSales30d

**Type**: Decimal

**Description**: Total value of sales occurring within 30 days of an ad click.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedSales30dOtherSKU

**Type**: Decimal

**Description**: Total value of sales occurring within 30 days of an ad click where the purchased SKU was different from the SKU advertised.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### attributedSales30dSameSKU

**Type**: Decimal

**Description**: Total value of sales occurring within 30 days of ad click where the purchased SKU was the same as the SKU advertised.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedSales7d

**Type**: Decimal

**Description**: Total value of sales occurring within seven days of an ad click.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedSales7dOtherSKU

**Type**: Decimal

**Description**: Total value of sales occurring within seven days of an ad click where the purchased SKU was different from the SKU advertised.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### attributedSales7dSameSKU

**Type**: Decimal

**Description**: Total value of sales occurring within seven days of ad click where the purchased SKU was the same as the SKU advertised.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedSalesNewToBrand14d

**Type**: Decimal

**Description**: Total value of new-to-brand sales occurring within 14 days of an ad click. Not available for book vendors. For Sponsored Brands only, this applies to attributed conversion events occurring within 14 days of an ad click or view.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedSalesNewToBrandPercentage14d

**Type**: Decimal

**Description**: Percentage of total sales made up of new-to-brand purchases. Not available for book vendors. For Sponsored Brands only, this applies to attributed conversion events occurring within 14 days of an ad click or view.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports)

<hr />

#### attributedUnitsOrdered14d

**Type**: Integer

**Description**: Total number of units ordered within 14 days of an ad click.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [ASIN](guides/reporting/v2/report-types#asin-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedUnitsOrdered14dOtherSKU

**Type**: Integer

**Description**: Total number of units ordered within 14 days of an ad click where the purchased SKU was different from the SKU advertised.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### attributedUnitsOrdered14dSameSKU

**Type**: Integer

**Description**: Total number of units ordered within 14 days of ad click where the purchased SKU was the same as the SKU advertised.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedUnitsOrdered1d

**Type**: Integer

**Description**: Total number of units ordered within 24 hours of an ad click.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [ASIN](guides/reporting/v2/report-types#asin-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedUnitsOrdered1dOtherSKU

**Type**: Integer

**Description**: Total number of units ordered within 24 hours of an ad click where the purchased SKU was different from the SKU advertised.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### attributedUnitsOrdered1dSameSKU

**Type**: Integer

**Description**: Total number of units ordered within 24 hours of ad click where the purchased SKU was the same as the SKU advertised.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports)

<hr />

#### attributedUnitsOrdered30d

**Type**: Integer

**Description**: Total number of units ordered within 30 days of an ad click.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [ASIN](guides/reporting/v2/report-types#asin-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedUnitsOrdered30dOtherSKU

**Type**: Integer

**Description**: Total number of units ordered within 30 days of an ad click where the purchased SKU was different from the SKU advertised.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### attributedUnitsOrdered30dSameSKU

**Type**: Integer

**Description**: Total number of units ordered within 30 days of ad click where the purchased SKU was the same as the SKU advertised.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports)

<hr />

#### attributedUnitsOrdered7d

**Type**: Integer

**Description**: Total number of units ordered within 7 days of an ad click.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [ASIN](guides/reporting/v2/report-types#asin-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedUnitsOrdered7dOtherSKU

**Type**: Integer

**Description**: Total number of units ordered within seven days of an ad click where the purchased SKU was different from the SKU advertised.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### attributedUnitsOrdered7dSameSKU

**Type**: Integer

**Description**: Total number of units ordered within seven days of ad click where the purchased SKU was the same as the SKU advertised.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports)

<hr />

#### attributedUnitsOrderedNewToBrand14d

**Type**: Integer

**Description**: Total number of attributed units ordered as part of new-to-brand sales occurring within 14 days of an ad click. Not available for book vendors. For Sponsored Brands only, this applies to attributed conversion events occurring within 14 days of an ad click or view.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### attributedUnitsOrderedNewToBrandPercentage14d

**Type**: Decimal

**Description**: Percentage of total attributed units ordered within 14 days of an ad click that are part of a new-to-brand purchase. Not available for book vendors. For Sponsored Brands only, this applies to attributed conversion events occurring within 14 days of an ad click or view.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports)

<hr />

#### clicks

**Type**: Integer

**Description**: Total number of ad clicks. 

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### cost

**Type**: Decimal

**Description**: Total cost of ad clicks.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### dpv14d

**Type**: Integer

**Description**: Number of attributed detail page views occurring within 14 days of click on an ad. Vendor-only field.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports)

<hr />

#### impressions

**Type**: Integer

**Description**: Total number of ad impressions.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### searchTermImpressionRank

**Type**: Integer

**Description**: Account-level ad-attributed impression rank for search terms.

**Report types**: [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports)

<hr />

#### searchTermImpressionShare

**Type**: Decimal

**Description**: Account-level ad-attributed impression share for search terms.

**Report types**: [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports)

<hr />

#### topOfSearchImpressionShare

**Type**: Decimal

**Description**: The percentage of top-of-search impressions earned out of all the top-of-search impressions that were eligible for a given date range. Various factors determine the eligibility for an impression including campaign status and targeting status.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Target](guides/reporting/v2/report-types#target-reports)

#### unitsSold14d

**Type**: Integer

**Description**: Number of attributed units sold within 14 days of click on an ad. For Sponsored Brands only, this applies to attributed conversion events occurring within 14 days of an ad click or view.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports)

<hr />

#### vctr

**Type**: Decimal

**Description**: Click-through rate for views (clicks divided by views).

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports)

<hr />

#### video5SecondViewRate

**Type**: Decimal

**Description**: The percentage of impressions where the customer watched the complete video or 5 seconds of the video (whichever is shorter). Sponsored Brands video-only metric.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports)

<hr />

#### video5SecondViews

**Type**: Integer

**Description**: The number of impressions where the customer watched the complete video or 5 seconds (whichever is shorter). Sponsored Brands video-only metric.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports)

<hr />

#### videoCompleteViews

**Type**: Integer

**Description**: The number of impressions where the video was viewed to 100%. 

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports)

<hr />

#### videoFirstQuartileViews

**Type**: Integer

**Description**: The number of impressions where the video was viewed to 25%.  

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports)

<hr />

#### videoMidpointViews

**Type**: Integer

**Description**: The number of impressions where the video was viewed to 50%.  

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports)

<hr />

#### videoThirdQuartileViews

**Type**: Integer

**Description**: The number of impressions where the video was viewed to 75%.  

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports)

<hr />

#### videoUnmutes

**Type**: Integer

**Description**: The number of impressions where a customer unmuted the video. 

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports)

<hr />

#### viewableImpressions

**Type**: Integer

**Description**: Number of impressions that met the Media Ratings Council (MRC) viewability standard. See viewability details at https://advertising.amazon.com/library/guides/viewability.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports)

<hr />

#### viewAttributedBrandedSearches14d

**Type**: Integer

**Description**: For Sponsored Display vCPM campaigns, this is the number of searches that included the name of your brand occurring within 14 days of an ad click or view. For CPC campaigns, this value is equal to attributedBrandedSearches14d.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### viewAttributedConversions14d

**Type**: Integer

**Description**: For Sponsored Display vCPM campaigns, the total number of attributed conversion events occurring within 14 days of an ad click or view. For CPC campaigns, this value is equal to attributedConversions14d.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### viewAttributedDetailPageView14d

**Type**: Integer

**Description**: *This metric is in beta.* For Sponsored Display vCPM campaigns, the total number of attributed detail page views occurring within 14 days of an ad click or view. For CPC campaigns, this value is equal to attributedDetailPageView14d.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### viewAttributedOrdersNewToBrand14d

**Type**: Integer 

**Description**: For Sponsored Display vCPM campaigns, the number of first-time orders for brand products over a one-year lookback window. Not available for book vendors. For CPC campaigns, this value is equal to attributedOrdersNewToBrand14d.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### viewAttributedOrderRateNewToBrand14d

**Type**: Decimal

**Description**: For Sponsored Display vCPM campaigns, the number of new-to-brand orders relative to the number of views. New-to-brand order rate = new-to-brand orders / views. Not available for book vendors. For CPC campaigns, this value is equal to attributedOrderRateNewToBrand14d. For Sponsored Brands only, this applies to attributed conversion events occurring within 14 days of an ad click or view.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### viewAttributedSales14d

**Type**: Decimal

**Description**: For Sponsored Display vCPM campaigns, the total value of sales occurring within 14 days of an ad click or view. For CPC campaigns, this value is equal to attributedSales14d.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### viewAttributedSalesNewToBrand14d

**Type**: Decimal

**Description**: For Sponsored Display vCPM campaigns, the total value of new-to-brand sales occurring within 14 days of an ad click or view. Not available for book vendors. For CPC campaigns, this value is equal to attributedSalesNewToBrand14d.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### viewAttributedUnitsOrdered14d

**Type**: Integer

**Description**: For Sponsored Display vCPM campaigns, the total number of units ordered within 14 days of an ad click or view. For CPC campaigns, this value is equal to attributedUnitsOrdered14d.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### viewAttributedUnitsOrderedNewToBrand14d

**Type**: Integer

**Description**: For Sponsored Display vCPM campaigns, the total number of attributed units ordered as part of new-to-brand sales occurring within 14 days of an ad click or view. Not available for book vendors. For CPC campaigns, this value is equal to attributedUnitsOrderedNewToBrand14d.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### viewAttributedUnitsOrdered1dOtherSKU

**Type**: Integer

**Description**: For Sponsored Display vCPM campaigns, the total number of units ordered within 24 hours of an ad click or view where the purchased SKU was different from the SKU advertised. For CPC campaigns, this value is equal to attributedUnitsOrdered1dOtherSKU.
 
**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### viewAttributedUnitsOrdered7dOtherSKU

**Type**: Integer

**Description**: For Sponsored Display vCPM campaigns, the total number of units ordered within seven days of an ad click or view where the purchased SKU was different from the SKU advertised. For CPC campaigns, this value is equal to attributedUnitsOrdered7dOtherSKU.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### viewAttributedUnitsOrdered14dOtherSKU

**Type**: Integer

**Description**: For Sponsored Display vCPM campaigns, the total number of units ordered within 14 days of an ad click or view where the purchased SKU was different from the SKU advertised. For CPC campaigns, this value is equal to attributedUnitsOrdered14dOtherSKU.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### viewAttributedUnitsOrdered30dOtherSKU

**Type**: Integer

**Description**: For Sponsored Display vCPM campaigns, the total number of units ordered within 30 days of an ad click or view where the purchased SKU was different from the SKU advertised. For CPC campaigns, this value is equal to attributedUnitsOrdered30dOtherSKU.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### viewAttributedSales1dOtherSKU

**Type**: Decimal

**Description**: For Sponsored Display vCPM campaigns, the total value of sales occurring within 24 hours of an ad click or view where the purchased SKU was different from the SKU advertised. For CPC campaigns, this value is equal to attributedSales1dOtherSKU.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### viewAttributedSales7dOtherSKU

**Type**: Decimal

**Description**: For Sponsored Display vCPM campaigns, the total value of sales occurring within seven days of an ad click or view where the purchased SKU was different from the SKU advertised. For CPC campaigns, this value is equal to attributedSales7dOtherSKU.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### viewAttributedSales14dOtherSKU

**Type**: Decimal

**Description**: For Sponsored Display vCPM campaigns, the total value of sales occurring within 14 days of an ad click or view where the purchased SKU was different from the SKU advertised. For CPC campaigns, this value is equal to attributedSales14dOtherSKU.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### viewAttributedSales30dOtherSKU

**Type**: Decimal

**Description**: For Sponsored Display vCPM campaigns, the total value of sales occurring within 30 days of an ad click or view where the purchased SKU was different from the SKU advertised. For CPC campaigns, this value is equal to attributedSales30dOtherSKU.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### viewAttributedConversions1dOtherSKU

**Type**: Integer

**Description**: For Sponsored Display vCPM campaigns, the total number of attributed conversion events occurring within 24 hours of an ad click or view where the purchased SKU was different from the SKU advertised. For CPC campaigns, this value is equal to attributedConversions1dOtherSKU.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### viewAttributedConversions7dOtherSKU

**Type**: Integer

**Description**: For Sponsored Display vCPM campaigns, the total number of attributed conversion events occurring within seven days of an ad click or view where the purchased SKU was different from the SKU advertised. For CPC campaigns, this value is equal to attributedConversions7dOtherSKU.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### viewAttributedConversions14dOtherSKU

**Type**: Integer

**Description**: For Sponsored Display vCPM campaigns, the total number of attributed conversion events occurring within 14 days of an ad click or view where the purchased SKU was different from the SKU advertised. For CPC campaigns, this value is equal to attributedConversions14dOtherSKU.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### viewAttributedConversions30dOtherSKU

**Type**: Integer

**Description**: For Sponsored Display vCPM campaigns, the total number of attributed conversion events occurring within 30 days of an ad click or view where the purchased SKU was different from the SKU advertised. For CPC campaigns, this value is equal to attributedConversions30dOtherSKU.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### viewAttributedConversions1dOtherSKU

**Type**: Integer

**Description**: For Sponsored Display vCPM campaigns, the total number of attributed conversion events occurring within 24 hours of an ad click where the purchased SKU was different from the SKU advertised. For CPC campaigns, this value is equal to attributedConversions1dOtherSKU.

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### attributedConversions7dOtherSKU

**Type**: Integer

**Description**: The total number of attributed conversion events occurring within seven days of an ad click where the purchased SKU was different from the SKU advertised. 

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### attributedConversions14dOtherSKU

**Type**: Integer

**Description**: The total number of attributed conversion events occurring within 14 days of an ad click where the purchased SKU was different from the SKU advertised. 

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### attributedConversions30dOtherSKU

**Type**: Integer

**Description**: The total number of attributed conversion events occurring within 30 days of an ad click where the purchased SKU was different from the SKU advertised. 

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### viewImpressions

**Type**: Integer

**Description**: The total number of viewable impressions. Only relevant for Sponsored Display vCPM campaigns.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### vtr

**Type**: Decimal

**Description**: View-through rate (vtr). Views divided by impressions.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports)

<hr />

#### avgImpressionsFrequency

**Type**: Decimal

**Description**: Average number of times unique users were exposed to an ad over the lifetime of the campaign.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports)

<hr />

#### cumulativeReach

**Type**: Integer

**Description**: Total number of unique users exposed to an ad from either a campaign, ad group, or product ad over the lifetime of the campaign or the past six months, whichever is shorter. This metric is updated daily.

**Report types**:  [Campaign](guides/reporting/v2/report-types#campaign-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports)

<hr />

### Dimensional metrics

Dimensional metrics can be used to further segment certain report types. These metrics are automatically added to certain report types, and do not need to be included as part of the request metrics list.

#### matchedTarget

**Type**: String

**Description**: The ASIN associated with the product page where a Sponsored Display ad appeared.

**Report types**: [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### placement

**Type**: String

**Description**: The page location where an ad appeared. 

**Report types**: [Placement](guides/reporting/v2/report-types#placement-reports)

<hr />

#### query

**Type**: String

**Description**: The search term used by the customer. 

**Report types**: [Search term](guides/reporting/v2/report-types#search-term-reports)

<hr />

### Metadata 

You can include necessary metadata attributes in your report, but we suggest limiting your use of these attributes, and requesting available metadata via an [Export](guides/exports/overview). The more metadata attributes you include in your report request, the larger your output file, and the longer your report will take to generate. 

Some metadata attributes are automatically added to certain report types, and don't need to be included in the metrics list. See [Report types](guides/reporting/v2/report-types) for more details.

#### adId

**Type**: Integer

**Description**: Unique numerical ID of the ad.

**Report types**: [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Ad](guides/reporting/v2/report-types#ad-reports)

<hr />

#### adGroupId

**Type**: Integer

**Description**: Unique numerical ID of the ad group.

**Report types**: [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [ASIN](guides/reporting/v2/report-types#asin-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### adGroupName

**Type**: String

**Description**: The name of the ad group as entered by the advertiser.

**Report types**: [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [ASIN](guides/reporting/v2/report-types#asin-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### applicableBudgetRuleId

**Type**: String

**Description**: The ID associated to the active budget rule for Sponsored Brands campaigns. For more information, see [Including budget rules values in Reports](guides/rules/budget-rules/reports).

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports)

<hr />

#### applicableBudgetRuleName

**Type**: String

**Description**: The name associated to the active budget rule for Sponsored Brands campaigns. For more information, see [Including budget rules values in Reports](guides/rules/budget-rules/reports).

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports)

<hr />

#### asin

**Type**: String

**Description**: The ASIN associated to an advertised product. 

**Report types**: [Product ad](guides/reporting/v2/report-types#product-ad-reports), [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### bidOptimization

**Type**: String

**Description**: Bid optimization for Sponsored Display ad groups. For vCPM campaigns, the value is always `reach`. For CPC campaigns, value is either `clicks` or `conversions`. 

**Report types**: [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### bidPlus

**Type**: String

**Description**: `true` or `false` based on whether premium bid adjustments are enabled for a Sponsored Product campaign.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports)

<hr />

#### campaignBudget

**Type**: Decimal

**Description**: Total budget allocated to the campaign.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### campaignBudgetType

**Type**: String

**Description**: One of `daily` or `lifetime`.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports)

<hr />

#### campaignId

**Type**: Integer

**Description**: Unique numerical ID of a campaign.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [ASIN](guides/reporting/v2/report-types#asin-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### campaignName

**Type**: String

**Description**: The name of the campaign as entered by the advertiser.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [ASIN](guides/reporting/v2/report-types#asin-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### campaignRuleBasedBudget

**Type**: Decimal

**Description**: The value of the rule-based budget for Sponsored Brands campaigns. For more information, see [Including budget rules values in Reports](guides/rules/budget-rules/reports).

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports)

<hr />

#### campaignStatus

**Type**: String

**Description**: Current status of the campaign. One of `active`, `paused`, or `archived`.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [Target](guides/reporting/v2/report-types#target-reports), [Ad](guides/reporting/v2/report-types#ad-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### costType

**Type**: String

**Description**: Determines how the campaign will bid and charge. One of `vCPM` (cost per thousand viewable impressions) or `CPC` (cost per click). Only relevant for Sponsored Display campaigns.

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### currency

**Type**: String

**Description**: The currency code associated with the campaign. 

**Report types**: [Campaign](guides/reporting/v2/report-types#campaign-reports), [Placement](guides/reporting/v2/report-types#placement-reports), [Ad group](guides/reporting/v2/report-types#ad-group-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [Product ad](guides/reporting/v2/report-types#product-ad-reports), [ASIN](guides/reporting/v2/report-types#asin-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### keywordBid

**Type**: Decimal

**Description**: Advertiser-set bid value for keyword

**Report types**: [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports)

<hr />

#### keywordId

**Type**: Integer

**Description**: Unique numerical ID of the keyword.

**Report types**: [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### keywordStatus

**Type**: String

**Description**: The status of the keyword.

**Report types**: [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports)

<hr />

#### keywordText

**Type**: String

**Description**: Text of the keyword or phrase.

**Report types**: [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### matchType

**Type**: String

**Description**: Type of matching for the keyword or phrase used in bid. Must be one of: `broad`, `phrase`, or `exact`.

**Report types**: [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### otherAsin

**Type**: String

**Description**: Represents a product that was purchased after an ad click but has a different SKU/ASIN that what was advertised. 

**Report types**: [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### sku

**Type**: String

**Description**: The SKU being advertised. Not available for vendors.

**Report types**: [Product ad](guides/reporting/v2/report-types#product-ad-reports), [ASIN](guides/reporting/v2/report-types#asin-reports)

<hr />

#### targetingExpression

**Type**: String

**Description**: A string representation of the expression object used in the targeting clause.

**Report types**: [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [ASIN](guides/reporting/v2/report-types#asin-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### targetId

**Type**: Integer

**Description**: The identifier of the targeting expression.

**Report types**: [Keyword](guides/reporting/v2/report-types#keyword-reports), [Search term](guides/reporting/v2/report-types#search-term-reports), [ASIN](guides/reporting/v2/report-types#asin-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### targetingText

**Type**: String

**Description**: The text used in the targeting expression.

**Report types**: [Search term](guides/reporting/v2/report-types#search-term-reports), [ASIN](guides/reporting/v2/report-types#asin-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)

<hr />

#### targetingType

**Type**: String

**Description**: The type of targeting used in the expression.

**Report types**: [Search term](guides/reporting/v2/report-types#search-term-reports), [Keyword](guides/reporting/v2/report-types#keyword-reports), [ASIN](guides/reporting/v2/report-types#asin-reports), [Target](guides/reporting/v2/report-types#target-reports), [Matched target](guides/reporting/v2/report-types#matched-target-reports)