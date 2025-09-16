---
title: Sponsored ads report types (version 2)
description: Learn what report types and metrics are available in version 2 reporting for sponsored ads using the Amazon Ads API.
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
    - Sponsored Display
---

# Report types

The Ads API supports a number of report types based on campaign entity structure. Depending on the ad type, campaign performance can be broken down by campaign, ad group, ad, keyword, target, and ASIN.

While most report types are determined by path parameters, some report types are also defined by the use of the `segment` parameter in the request body. 

>[NOTE]Reports in the advertising console are slightly different than the API. For more information, see [API versus console reporting](guides/reporting/v2/advertising-console).

### Availability by ad type

>[WARNING] As of March 30, 2023, Sponsored Products reports are no longer supported in version 2. Use the [version 3 reporting endpoints](release-notes/archive/ads-api#announcing-reporting-enhancements-for-sponsored-products-and-sponsored-brands) going forward. For more details on how to upgrade to version 3, see our [Migration guide](reference/migration-guides/reporting-v2-v3) and [Frequently asked questions](guides/reporting/v3/faq).

Each type of sponsored ad supports different report types and has a separate path defined.

| Report type | Sponsored Products [DEPRECATED] | Sponsored Brands | Sponsored Display |
|-------------|--------------------|------------------|-------------------|
| [Campaign](#campaign-reports) | x | x | x |
| [Placement](#placement-reports) | x | x |  |
| [Ad group](#ad-group-reports) | x | x | x |
| [Product ad](#product-ad-reports) | x |  | x |
| [Target](#target-reports) | x | x | x |
| [Keyword](#keyword-reports) | x | x |  |
| [Search term](#search-term-reports) | x | x |  |
| [ASIN](#asin-reports) | x |   | x |
| [Ad](#ad-reports) |   | x |   |
| [Matched target](#matched-target-reports) |   |   | x |

## Campaign reports

Campaign reports contain performance data broken down at the campaign level. Campaign reports include all campaigns of the requested sponsored ad type that have performance activity for the requested day. For example, a Sponsored Products campaign report returns performance data for all Sponsored Products campaigns that received impressions on the chosen date. 

### Paths

| Ad type |	Path |
|--------|--------------------|
| Sponsored Products | /v2/sp/campaigns/report [DEPRECATED]|
| Sponsored Brands | /v2/hsa/campaigns/report |
| Sponsored Display | /sd/campaigns/report |

### Available metrics

 <details class="details-bar">
    <summary>Sponsored Products [DEPRECATED]</summary>

[applicableBudgetRuleId](guides/reporting/v2/metrics#applicableBudgetRuleId), [applicableBudgetRuleName](guides/reporting/v2/metrics#applicableBudgetRuleName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedConversions1d](guides/reporting/v2/metrics#attributedConversions1d), [attributedConversions1dSameSKU](guides/reporting/v2/metrics#attributedConversions1dSameSKU), [attributedConversions30d](guides/reporting/v2/metrics#attributedConversions30d), [attributedConversions30dSameSKU](guides/reporting/v2/metrics#attributedConversions30dSameSKU), [attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d), [attributedConversions7dSameSKU](guides/reporting/v2/metrics#attributedConversions7dSameSKU), [attributedKindleEditionNormalizedPagesRead14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRead14d), [attributedKindleEditionNormalizedPagesRoyalties14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRoyalties14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSales1d](guides/reporting/v2/metrics#attributedSales1d), [attributedSales1dSameSKU](guides/reporting/v2/metrics#attributedSales1dSameSKU), [attributedSales30d](guides/reporting/v2/metrics#attributedSales30d), [attributedSales30dSameSKU](guides/reporting/v2/metrics#attributedSales30dSameSKU), [attributedSales7d](guides/reporting/v2/metrics#attributedSales7d), [attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedSales7dSameSKU), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d), [attributedUnitsOrdered14dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered14dSameSKU), [attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d), [attributedUnitsOrdered1dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered1dSameSKU), [attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d), [attributedUnitsOrdered30dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered30dSameSKU), [attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d), [attributedUnitsOrdered7dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered7dSameSKU), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId)\*, [campaignName](guides/reporting/v2/metrics#campaignName), [campaignRuleBasedBudget](guides/reporting/v2/metrics#campaignRuleBasedBudget), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [currency](guides/reporting/v2/metrics#currency), [impressions](guides/reporting/v2/metrics#impressions)

</details>
 <details class="details-bar">
    <summary>Sponsored Brands (version 3)</summary>

[applicableBudgetRuleId](guides/reporting/v2/metrics#applicableBudgetRuleId), [applicableBudgetRuleName](guides/reporting/v2/metrics#applicableBudgetRuleName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedDetailPageViewsClicks14d](guides/reporting/v2/metrics#attributedDetailPageViewsClicks14d), [attributedOrderRateNewToBrand14d](guides/reporting/v2/metrics#attributedOrderRateNewToBrand14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedOrdersNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedOrdersNewToBrandPercentage14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedSalesNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedSalesNewToBrandPercentage14d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [attributedUnitsOrderedNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrandPercentage14d), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId)\*, [campaignName](guides/reporting/v2/metrics#campaignName), [campaignRuleBasedBudget](guides/reporting/v2/metrics#campaignRuleBasedBudget), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [dpv14d](guides/reporting/v2/metrics#dpv14d), [impressions](guides/reporting/v2/metrics#impressions), [unitsSold14d](guides/reporting/v2/metrics#unitsSold14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d), [topOfSearchImpressionShare](guides/reporting/v2/metrics#topOfSearchImpressionShare)

</details>
 <details class="details-bar">
    <summary>Sponsored Brands Video (version 3) & multi-ad group campaigns (version 4)</summary>

[attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId)\*, [campaignName](guides/reporting/v2/metrics#campaignName), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [dpv14d](guides/reporting/v2/metrics#dpv14d), [impressions](guides/reporting/v2/metrics#impressions), [vctr](guides/reporting/v2/metrics#vctr), [video5SecondViewRate](guides/reporting/v2/metrics#video5SecondViewRate), [video5SecondViews](guides/reporting/v2/metrics#video5SecondViews), [videoCompleteViews](guides/reporting/v2/metrics#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v2/metrics#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v2/metrics#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v2/metrics#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v2/metrics#videoUnmutes), [viewableImpressions](guides/reporting/v2/metrics#viewableImpressions), [vtr](guides/reporting/v2/metrics#vtr), [dpv14d](guides/reporting/v2/metrics#dpv14d), [attributedDetailPageViewsClicks14d](guides/reporting/v2/metrics#attributedDetailPageViewsClicks14d), [attributedOrderRateNewToBrand14d](guides/reporting/v2/metrics#attributedOrderRateNewToBrand14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedOrdersNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedOrdersNewToBrandPercentage14d), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedSalesNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedSalesNewToBrandPercentage14d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [attributedUnitsOrderedNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrandPercentage14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d), [currency](guides/reporting/v2/metrics#currency), [topOfSearchImpressionShare](guides/reporting/v2/metrics#topOfSearchImpressionShare)

</details>
 <details class="details-bar">
    <summary>Sponsored Display</summary>

[attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedConversions1d](guides/reporting/v2/metrics#attributedConversions1d), [attributedConversions1dSameSKU](guides/reporting/v2/metrics#attributedConversions1dSameSKU), [attributedConversions30d](guides/reporting/v2/metrics#attributedConversions30d), [attributedConversions30dSameSKU](guides/reporting/v2/metrics#attributedConversions30dSameSKU), [attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d), [attributedConversions7dSameSKU](guides/reporting/v2/metrics#attributedConversions7dSameSKU), [attributedDetailPageView14d](guides/reporting/v2/metrics#attributedDetailPageView14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSales1d](guides/reporting/v2/metrics#attributedSales1d), [attributedSales1dSameSKU](guides/reporting/v2/metrics#attributedSales1dSameSKU), [attributedSales30d](guides/reporting/v2/metrics#attributedSales30d), [attributedSales30dSameSKU](guides/reporting/v2/metrics#attributedSales30dSameSKU), [attributedSales7d](guides/reporting/v2/metrics#attributedSales7d), [attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedSales7dSameSKU), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d), [attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d), [attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d), [attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [costType](guides/reporting/v2/metrics#costType), [currency](guides/reporting/v2/metrics#currency), [impressions](guides/reporting/v2/metrics#impressions), [viewAttributedConversions14d](guides/reporting/v2/metrics#viewAttributedConversions14d), [viewAttributedDetailPageView14d](guides/reporting/v2/metrics#viewAttributedDetailPageView14d), [viewAttributedSales14d](guides/reporting/v2/metrics#viewAttributedSales14d), [viewAttributedUnitsOrdered14d](guides/reporting/v2/metrics#viewAttributedUnitsOrdered14d), [viewImpressions](guides/reporting/v2/metrics#viewImpressions), [viewAttributedOrdersNewToBrand14d](guides/reporting/v2/metrics#viewAttributedOrdersNewToBrand14d), [viewAttributedSalesNewToBrand14d](guides/reporting/v2/metrics#viewattributedsalesnewtobrand14d), [viewAttributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#viewattributedunitsorderednewtobrand14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d), [viewAttributedBrandedSearches14d](guides/reporting/v2/metrics#viewAttributedBrandedSearches14d), [videoCompleteViews](guides/reporting/v2/metrics#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v2/metrics#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v2/metrics#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v2/metrics#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v2/metrics#videoUnmutes), [vtr](guides/reporting/v2/metrics#vtr), [vctr](guides/reporting/v2/metrics#vctr), [avgImpressionsFrequency](guides/reporting/v2/metrics#avgImpressionsFrequency), [cumulativeReach](guides/reporting/v2/metrics#cumulativeReach)

</details>

\*Metric is added to the report by default. 

### Sample requests

If you are copying the example request, make sure to use the correct URL for your geography and valid authentication credentials for your account. 

 <details class="details-bar">
    <summary>Sponsored Products [DEPRECATED]</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/sp/campaigns/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "impressions,clicks,cost,attributedConversions14d,attributedSales14d"
    }'
    ```
</details>

 <details class="details-bar">
    <summary>Sponsored Brands multi-ad group campaigns</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/hsa/campaigns/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "impressions,clicks,cost,attributedConversions14d,attributedSales14d",
    "creativeType": "all"
    }'
    ```
</details>
 <details class="details-bar">
    <summary>Sponsored Brands Video</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/hsa/campaigns/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "viewableImpressions,vtr,vctr,clicks,cost,attributedConversions14d,attributedSales14d",
    "creativeType": "video"
    }'
    ```
</details>

 <details class="details-bar">
    <summary>Sponsored Display</summary>

    <p>This sample requests performance data for campaigns using contextual targeting (tactic T00020). The metrics requested in this example return data for campaigns using both cost types: cost per click (CPC) and cost per thousand viewable impressions (vCPM) campaigns.</p>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/sd/campaigns/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "impressions,viewImpressions,clicks,cost,attributedConversions14d,viewAttributedConversions14d,attributedSales14d,viewAttributedSales14d",
    "tactic": "T00020"
    }'
    ```
</details>

## Placement reports

Placement reports show performance broken down by campaign and the position of the ad on the page. 

For Sponsored Products and Sponsored Brands, you can create a placement report by requesting a `campaigns` report and setting `segment` to `placement` in the request body. 

### Paths

| Ad type |	Path |
|--------|--------------------|
| Sponsored Products | /v2/sp/campaigns/report [DEPRECATED] |
| Sponsored Brands | /v2/hsa/campaigns/report |

**Note**: Placement reports are not available for Sponsored Display campaigns.

### Available metrics

 <details class="details-bar">
    <summary>Sponsored Products [DEPRECATED]</summary>

[applicableBudgetRuleId](guides/reporting/v2/metrics#applicableBudgetRuleId), [applicableBudgetRuleName](guides/reporting/v2/metrics#applicableBudgetRuleName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedConversions1d](guides/reporting/v2/metrics#attributedConversions1d), [attributedConversions1dSameSKU](guides/reporting/v2/metrics#attributedConversions1dSameSKU), [attributedConversions30d](guides/reporting/v2/metrics#attributedConversions30d), [attributedConversions30dSameSKU](guides/reporting/v2/metrics#attributedConversions30dSameSKU), [attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d), [attributedConversions7dSameSKU](guides/reporting/v2/metrics#attributedConversions7dSameSKU), [attributedKindleEditionNormalizedPagesRead14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRead14d), [attributedKindleEditionNormalizedPagesRoyalties14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRoyalties14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSales1d](guides/reporting/v2/metrics#attributedSales1d), [attributedSales1dSameSKU](guides/reporting/v2/metrics#attributedSales1dSameSKU), [attributedSales30d](guides/reporting/v2/metrics#attributedSales30d), [attributedSales30dSameSKU](guides/reporting/v2/metrics#attributedSales30dSameSKU), [attributedSales7d](guides/reporting/v2/metrics#attributedSales7d), [attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedSales7dSameSKU), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d), [attributedUnitsOrdered14dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered14dSameSKU), [attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d), [attributedUnitsOrdered1dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered1dSameSKU), [attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d), [attributedUnitsOrdered30dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered30dSameSKU), [attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d), [attributedUnitsOrdered7dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered7dSameSKU), [bidPlus](guides/reporting/v2/metrics#bidPlus), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId)\*, [campaignName](guides/reporting/v2/metrics#campaignName), [campaignRuleBasedBudget](guides/reporting/v2/metrics#campaignRuleBasedBudget), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [currency](guides/reporting/v2/metrics#currency), [impressions](guides/reporting/v2/metrics#impressions), [placement](guides/reporting/v2/metrics#placement)\*

</details>

 <details class="details-bar">
    <summary>Sponsored Brands (version 3)</summary>

[applicableBudgetRuleId](guides/reporting/v2/metrics#applicableBudgetRuleId), [applicableBudgetRuleName](guides/reporting/v2/metrics#applicableBudgetRuleName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedDetailPageViewsClicks14d](guides/reporting/v2/metrics#attributedDetailPageViewsClicks14d), [attributedOrderRateNewToBrand14d](guides/reporting/v2/metrics#attributedOrderRateNewToBrand14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedOrdersNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedOrdersNewToBrandPercentage14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedSalesNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedSalesNewToBrandPercentage14d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [attributedUnitsOrderedNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrandPercentage14d), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId)\*, [campaignName](guides/reporting/v2/metrics#campaignName), [campaignRuleBasedBudget](guides/reporting/v2/metrics#campaignRuleBasedBudget), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [dpv14d](guides/reporting/v2/metrics#dpv14d), [impressions](guides/reporting/v2/metrics#impressions), [placement](guides/reporting/v2/metrics#placement)\*,
[unitsSold14d](guides/reporting/v2/metrics#unitsSold14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d)

</details>

 <details class="details-bar">
    <summary>Sponsored Brands Video (version 3) & multi-ad group campaigns (version 4)</summary>

[attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId)\*, [campaignName](guides/reporting/v2/metrics#campaignName), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [impressions](guides/reporting/v2/metrics#impressions), [placement](guides/reporting/v2/metrics#placement)\*, [vctr](guides/reporting/v2/metrics#vctr), [video5SecondViewRate](guides/reporting/v2/metrics#video5SecondViewRate), [video5SecondViews](guides/reporting/v2/metrics#video5SecondViews), [videoCompleteViews](guides/reporting/v2/metrics#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v2/metrics#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v2/metrics#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v2/metrics#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v2/metrics#videoUnmutes), [viewableImpressions](guides/reporting/v2/metrics#viewableImpressions), [vtr](guides/reporting/v2/metrics#vtr), [dpv14d](guides/reporting/v2/metrics#dpv14d), [attributedDetailPageViewsClicks14d](guides/reporting/v2/metrics#attributedDetailPageViewsClicks14d), [attributedOrderRateNewToBrand14d](guides/reporting/v2/metrics#attributedOrderRateNewToBrand14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedOrdersNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedOrdersNewToBrandPercentage14d), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedSalesNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedSalesNewToBrandPercentage14d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [attributedUnitsOrderedNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrandPercentage14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d), [currency](guides/reporting/v2/metrics#currency)

</details>

### Sample requests

If you are copying the example request, make sure to use the correct URL for your geography and valid authentication credentials for your account.

 <details class="details-bar">
    <summary>Sponsored Products [DEPRECATED]</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/sp/campaigns/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "impressions,clicks,cost,attributedConversions14d,attributedSales14d",
    "segment": "placement"
    }'
    ```
</details>
 <details class="details-bar">
    <summary>Sponsored Brands</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/hsa/campaigns/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "impressions,clicks,cost,attributedConversions14d,attributedSales14d",
    "segment": "placement",
    "creativeType": "all"
    }'
    ```
</details>

## Ad group reports

Every campaign contains one or more ad groups that share targeting and bid settings. You can get performance data at the ad group level by creating an `adGroups` report. 

### Paths

| Ad type |	Path |
|--------|--------------------|
| Sponsored Products | /v2/sp/adGroups/report [DEPRECATED]| 
| Sponsored Brands | /v2/hsa/adGroups/report |
| Sponsored Display | /sd/adGroups/report | 

### Available metrics

 <details class="details-bar">
    <summary>Sponsored Products [DEPRECATED]</summary>
    
[adGroupId](guides/reporting/v2/metrics#adGroupId)\*, [adGroupName](guides/reporting/v2/metrics#adGroupName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedConversions1d](guides/reporting/v2/metrics#attributedConversions1d), [attributedConversions1dSameSKU](guides/reporting/v2/metrics#attributedConversions1dSameSKU), [attributedConversions30d](guides/reporting/v2/metrics#attributedConversions30d), [attributedConversions30dSameSKU](guides/reporting/v2/metrics#attributedConversions30dSameSKU), [attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d), [attributedConversions7dSameSKU](guides/reporting/v2/metrics#attributedConversions7dSameSKU), [attributedKindleEditionNormalizedPagesRead14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRead14d), [attributedKindleEditionNormalizedPagesRoyalties14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRoyalties14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSales1d](guides/reporting/v2/metrics#attributedSales1d), [attributedSales1dSameSKU](guides/reporting/v2/metrics#attributedSales1dSameSKU), [attributedSales30d](guides/reporting/v2/metrics#attributedSales30d), [attributedSales30dSameSKU](guides/reporting/v2/metrics#attributedSales30dSameSKU), [attributedSales7d](guides/reporting/v2/metrics#attributedSales7d), [attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedSales7dSameSKU), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d), [attributedUnitsOrdered14dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered14dSameSKU), [attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d), [attributedUnitsOrdered1dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered1dSameSKU), [attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d), [attributedUnitsOrdered30dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered30dSameSKU), [attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d), [attributedUnitsOrdered7dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered7dSameSKU), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [currency](guides/reporting/v2/metrics#currency), [impressions](guides/reporting/v2/metrics#impressions)

</details>
 <details class="details-bar">
    <summary>Sponsored Brands (version 3)</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId)\*, [adGroupName](guides/reporting/v2/metrics#adGroupName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedDetailPageViewsClicks14d](guides/reporting/v2/metrics#attributedDetailPageViewsClicks14d), [attributedOrderRateNewToBrand14d](guides/reporting/v2/metrics#attributedOrderRateNewToBrand14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedOrdersNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedOrdersNewToBrandPercentage14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedSalesNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedSalesNewToBrandPercentage14d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [attributedUnitsOrderedNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrandPercentage14d), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [dpv14d](guides/reporting/v2/metrics#dpv14d), [impressions](guides/reporting/v2/metrics#impressions), [unitsSold14d](guides/reporting/v2/metrics#unitsSold14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d)

</details>
 <details class="details-bar">
    <summary>Sponsored Brands Video (version 3) & multi-ad group campaigns (version 4)</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId)\*, [adGroupName](guides/reporting/v2/metrics#adGroupName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId) [campaignName](guides/reporting/v2/metrics#campaignName), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [impressions](guides/reporting/v2/metrics#impressions), [vctr](guides/reporting/v2/metrics#vctr), [video5SecondViewRate](guides/reporting/v2/metrics#video5SecondViewRate), [video5SecondViews](guides/reporting/v2/metrics#video5SecondViews), [videoCompleteViews](guides/reporting/v2/metrics#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v2/metrics#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v2/metrics#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v2/metrics#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v2/metrics#videoUnmutes), [viewableImpressions](guides/reporting/v2/metrics#viewableImpressions), [vtr](guides/reporting/v2/metrics#vtr), [dpv14d](guides/reporting/v2/metrics#dpv14d), [attributedDetailPageViewsClicks14d](guides/reporting/v2/metrics#attributedDetailPageViewsClicks14d), [attributedOrderRateNewToBrand14d](guides/reporting/v2/metrics#attributedOrderRateNewToBrand14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedOrdersNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedOrdersNewToBrandPercentage14d), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedSalesNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedSalesNewToBrandPercentage14d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [attributedUnitsOrderedNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrandPercentage14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d), [currency](guides/reporting/v2/metrics#currency)


</details>
 <details class="details-bar">
    <summary>Sponsored Display</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedConversions1d](guides/reporting/v2/metrics#attributedConversions1d), [attributedConversions1dSameSKU](guides/reporting/v2/metrics#attributedConversions1dSameSKU), [attributedConversions30d](guides/reporting/v2/metrics#attributedConversions30d), [attributedConversions30dSameSKU](guides/reporting/v2/metrics#attributedConversions30dSameSKU), [attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d), [attributedConversions7dSameSKU](guides/reporting/v2/metrics#attributedConversions7dSameSKU), [attributedDetailPageView14d](guides/reporting/v2/metrics#attributedDetailPageView14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSales1d](guides/reporting/v2/metrics#attributedSales1d), [attributedSales1dSameSKU](guides/reporting/v2/metrics#attributedSales1dSameSKU), [attributedSales30d](guides/reporting/v2/metrics#attributedSales30d), [attributedSales30dSameSKU](guides/reporting/v2/metrics#attributedSales30dSameSKU), [attributedSales7d](guides/reporting/v2/metrics#attributedSales7d), [attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedSales7dSameSKU), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d), [attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d), [attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d), [attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [bidOptimization](guides/reporting/v2/metrics#bidOptimization), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [currency](guides/reporting/v2/metrics#currency), [impressions](guides/reporting/v2/metrics#impressions), [viewAttributedConversions14d](guides/reporting/v2/metrics#viewAttributedConversions14d), [viewAttributedDetailPageView14d](guides/reporting/v2/metrics#viewAttributedDetailPageView14d), [viewAttributedSales14d](guides/reporting/v2/metrics#viewAttributedSales14d), [viewAttributedUnitsOrdered14d](guides/reporting/v2/metrics#viewAttributedUnitsOrdered14d), [viewImpressions](guides/reporting/v2/metrics#viewImpressions), [viewAttributedOrdersNewToBrand14d](guides/reporting/v2/metrics#viewAttributedOrdersNewToBrand14d), [viewAttributedSalesNewToBrand14d](guides/reporting/v2/metrics#viewattributedsalesnewtobrand14d), [viewAttributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#viewattributedunitsorderednewtobrand14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d), [viewAttributedBrandedSearches14d](guides/reporting/v2/metrics#viewAttributedBrandedSearches14d), [videoCompleteViews](guides/reporting/v2/metrics#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v2/metrics#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v2/metrics#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v2/metrics#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v2/metrics#videoUnmutes), [vtr](guides/reporting/v2/metrics#vtr), [vctr](guides/reporting/v2/metrics#vctr), [avgImpressionsFrequency](guides/reporting/v2/metrics#avgImpressionsFrequency), [cumulativeReach](guides/reporting/v2/metrics#cumulativeReach)

</details>

\*Metric is added to the report by default. 

### Sample request

If you are copying the example request, make sure to use the correct URL for your geography and valid authentication credentials for your account. 

 <details class="details-bar">
    <summary>Sponsored Products [DEPRECATED]</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/sp/adGroups/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "impressions,clicks,cost,attributedConversions14d,attributedSales14d"
    }'
    ```
</details>

 <details class="details-bar">
    <summary>Sponsored Brands</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/hsa/adGroups/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "impressions,clicks,cost,attributedConversions14d,attributedSales14d",
    "creativeType": "all"
    }'
    ```
</details>

 <details class="details-bar">
    <summary>Sponsored Display</summary>

    <p>This sample requests performance data for ad groups using audience targeting (tactic T00030). The metrics requested in this example return data for campaigns using the cost per click (cpc) cost type.</p>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/sd/adGroups/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "adGroupId,impressions,clicks,cost,attributedConversions14d,attributedSales14d",
    "tactic": "T00030"
    }'
    ```
</details>

## Product ad reports

Product ad reports show the performance of your advertised products. You can generate performance data at the advertised product level by requesting a `productAds` report.

### Paths

| Ad type |	Path |
|--------|--------------------|
| Sponsored Products | /v2/sp/productAds/report [DEPRECATED] |
| Sponsored Display |	/sd/productAds/report |

**Note**: Product ad reports are not available for Sponsored Brands campaigns.

### Available metrics

 <details class="details-bar">
    <summary>Sponsored Products</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [adId](guides/reporting/v2/metrics#adId)\*, [asin](guides/reporting/v2/metrics#asin), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedConversions1d](guides/reporting/v2/metrics#attributedConversions1d), [attributedConversions1dSameSKU](guides/reporting/v2/metrics#attributedConversions1dSameSKU), [attributedConversions30d](guides/reporting/v2/metrics#attributedConversions30d), [attributedConversions30dSameSKU](guides/reporting/v2/metrics#attributedConversions30dSameSKU), [attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d), [attributedConversions7dSameSKU](guides/reporting/v2/metrics#attributedConversions7dSameSKU), [attributedKindleEditionNormalizedPagesRead14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRead14d), [attributedKindleEditionNormalizedPagesRoyalties14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRoyalties14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSales1d](guides/reporting/v2/metrics#attributedSales1d), [attributedSales1dSameSKU](guides/reporting/v2/metrics#attributedSales1dSameSKU), [attributedSales30d](guides/reporting/v2/metrics#attributedSales30d), [attributedSales30dSameSKU](guides/reporting/v2/metrics#attributedSales30dSameSKU), [attributedSales7d](guides/reporting/v2/metrics#attributedSales7d), [attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedSales7dSameSKU), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d), [attributedUnitsOrdered14dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered14dSameSKU), [attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d), [attributedUnitsOrdered1dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered1dSameSKU), [attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d), [attributedUnitsOrdered30dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered30dSameSKU), [attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d), [attributedUnitsOrdered7dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered7dSameSKU), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [currency](guides/reporting/v2/metrics#currency), [impressions](guides/reporting/v2/metrics#impressions), [sku](guides/reporting/v2/metrics#sku) (seller only)

</details>

 <details class="details-bar">
    <summary>Sponsored Display</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [adId](guides/reporting/v2/metrics#adId), [asin](guides/reporting/v2/metrics#asin), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedConversions1d](guides/reporting/v2/metrics#attributedConversions1d), [attributedConversions1dSameSKU](guides/reporting/v2/metrics#attributedConversions1dSameSKU), [attributedConversions30d](guides/reporting/v2/metrics#attributedConversions30d), [attributedConversions30dSameSKU](guides/reporting/v2/metrics#attributedConversions30dSameSKU), [attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d), [attributedConversions7dSameSKU](guides/reporting/v2/metrics#attributedConversions7dSameSKU), [attributedDetailPageView14d](guides/reporting/v2/metrics#attributedDetailPageView14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSales1d](guides/reporting/v2/metrics#attributedSales1d), [attributedSales1dSameSKU](guides/reporting/v2/metrics#attributedSales1dSameSKU), [attributedSales30d](guides/reporting/v2/metrics#attributedSales30d), [attributedSales30dSameSKU](guides/reporting/v2/metrics#attributedSales30dSameSKU), [attributedSales7d](guides/reporting/v2/metrics#attributedSales7d), [attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedSales7dSameSKU), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d), [attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d), [attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d), [attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [currency](guides/reporting/v2/metrics#currency), [impressions](guides/reporting/v2/metrics#impressions), [sku](guides/reporting/v2/metrics#sku) (seller only), [viewAttributedConversions14d](guides/reporting/v2/metrics#viewAttributedConversions14d), [viewImpressions](guides/reporting/v2/metrics#viewImpressions), [viewAttributedDetailPageView14d](guides/reporting/v2/metrics#viewAttributedDetailPageView14d), [viewAttributedSales14d](guides/reporting/v2/metrics#viewAttributedSales14d), [viewAttributedUnitsOrdered14d](guides/reporting/v2/metrics#viewAttributedUnitsOrdered14d), [viewAttributedOrdersNewToBrand14d](guides/reporting/v2/metrics#viewAttributedOrdersNewToBrand14d), [viewAttributedSalesNewToBrand14d](guides/reporting/v2/metrics#viewattributedsalesnewtobrand14d), [viewAttributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#viewattributedunitsorderednewtobrand14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d), [viewAttributedBrandedSearches14d](guides/reporting/v2/metrics#viewAttributedBrandedSearches14d), [videoCompleteViews](guides/reporting/v2/metrics#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v2/metrics#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v2/metrics#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v2/metrics#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v2/metrics#videoUnmutes), [vtr](guides/reporting/v2/metrics#vtr), [vctr](guides/reporting/v2/metrics#vctr), [avgImpressionsFrequency](guides/reporting/v2/metrics#avgImpressionsFrequency), [cumulativeReach](guides/reporting/v2/metrics#cumulativeReach) 

</details>

\*Metric is added to the report by default. 

### Sample requests

If you are copying the example request, make sure to use the correct URL for your geography and valid authentication credentials for your account.

 <details class="details-bar">
    <summary>Sponsored Products [DEPRECATED]</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/sp/productAds/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "impressions,clicks,cost,attributedConversions14d,attributedSales14d"
    }'
    ```
</details>
 <details class="details-bar">
    <summary>Sponsored Display</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/sd/productAds/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "adId,impressions,clicks,cost,attributedConversions14d,attributedSales14d",
    "tactic": "T00020"
    }'
    ```
</details>

## Target reports

Targeting reports contain performance data related to targeting expressions. You can generate performance data at the target level by requesting a `targets` report.

### Paths

| Ad type |	Path |
|--------------------|--------------------|
| Sponsored Products | /v2/sp/targets/report [DEPRECATED]|
| Sponsored Brands | /v2/hsa/targets/report |
| Sponsored Display | /sd/targets/report |

### Available metrics

 <details class="details-bar">
    <summary>Sponsored Products [DEPRECATED]</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedConversions1d](guides/reporting/v2/metrics#attributedConversions1d), [attributedConversions1dSameSKU](guides/reporting/v2/metrics#attributedConversions1dSameSKU), [attributedConversions30d](guides/reporting/v2/metrics#attributedConversions30d), [attributedConversions30dSameSKU](guides/reporting/v2/metrics#attributedConversions30dSameSKU), [attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d), [attributedConversions7dSameSKU](guides/reporting/v2/metrics#attributedConversions7dSameSKU), [attributedKindleEditionNormalizedPagesRead14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRead14d), [attributedKindleEditionNormalizedPagesRoyalties14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRoyalties14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSales1d](guides/reporting/v2/metrics#attributedSales1d), [attributedSales1dSameSKU](guides/reporting/v2/metrics#attributedSales1dSameSKU), [attributedSales30d](guides/reporting/v2/metrics#attributedSales30d), [attributedSales30dSameSKU](guides/reporting/v2/metrics#attributedSales30dSameSKU), [attributedSales7d](guides/reporting/v2/metrics#attributedSales7d), [attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedSales7dSameSKU), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d), [attributedUnitsOrdered14dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered14dSameSKU), [attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d), [attributedUnitsOrdered1dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered1dSameSKU), [attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d), [attributedUnitsOrdered30dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered30dSameSKU), [attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d), [attributedUnitsOrdered7dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered7dSameSKU), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [impressions](guides/reporting/v2/metrics#impressions), [targetId](guides/reporting/v2/metrics#targetId)\*, [targetingExpression](guides/reporting/v2/metrics#targetingExpression), [targetingText](guides/reporting/v2/metrics#targetingText), [targetingType](guides/reporting/v2/metrics#targetingType)

</details>
 <details class="details-bar">
    <summary>Sponsored Brands (version 3)</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedDetailPageViewsClicks14d](guides/reporting/v2/metrics#attributedDetailPageViewsClicks14d), [attributedOrderRateNewToBrand14d](guides/reporting/v2/metrics#attributedOrderRateNewToBrand14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedOrdersNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedOrdersNewToBrandPercentage14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedSalesNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedSalesNewToBrandPercentage14d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [attributedUnitsOrderedNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrandPercentage14d), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [dpv14d](guides/reporting/v2/metrics#dpv14d), [impressions](guides/reporting/v2/metrics#impressions), [targetId](guides/reporting/v2/metrics#targetId)\*, [targetingExpression](guides/reporting/v2/metrics#targetingExpression), [targetingText](guides/reporting/v2/metrics#targetingText), [targetingType](guides/reporting/v2/metrics#targetingType), [unitsSold14d](guides/reporting/v2/metrics#unitsSold14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d), [topOfSearchImpressionShare](guides/reporting/v2/metrics#topOfSearchImpressionShare)

</details>
 <details class="details-bar">
    <summary>Sponsored Brands Video (version 3) & multi-ad group campaigns (version 4)</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [impressions](guides/reporting/v2/metrics#impressions), [targetId](guides/reporting/v2/metrics#targetId)\*, [targetingExpression](guides/reporting/v2/metrics#targetingExpression), [targetingText](guides/reporting/v2/metrics#targetingText), [targetingType](guides/reporting/v2/metrics#targetingType), [vctr](guides/reporting/v2/metrics#vctr), [video5SecondViewRate](guides/reporting/v2/metrics#video5SecondViewRate), [video5SecondViews](guides/reporting/v2/metrics#video5SecondViews), [videoCompleteViews](guides/reporting/v2/metrics#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v2/metrics#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v2/metrics#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v2/metrics#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v2/metrics#videoUnmutes), [viewableImpressions](guides/reporting/v2/metrics#viewableImpressions), [vtr](guides/reporting/v2/metrics#vtr), [dpv14d](guides/reporting/v2/metrics#dpv14d), [attributedDetailPageViewsClicks14d](guides/reporting/v2/metrics#attributedDetailPageViewsClicks14d), [attributedOrderRateNewToBrand14d](guides/reporting/v2/metrics#attributedOrderRateNewToBrand14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedOrdersNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedOrdersNewToBrandPercentage14d), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedSalesNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedSalesNewToBrandPercentage14d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [attributedUnitsOrderedNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrandPercentage14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d), [currency](guides/reporting/v2/metrics#currency), [topOfSearchImpressionShare](guides/reporting/v2/metrics#topOfSearchImpressionShare)

</details>
 <details class="details-bar">
    <summary>Sponsored Display</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedConversions1d](guides/reporting/v2/metrics#attributedConversions1d), [attributedConversions1dSameSKU](guides/reporting/v2/metrics#attributedConversions1dSameSKU), [attributedConversions30d](guides/reporting/v2/metrics#attributedConversions30d), [attributedConversions30dSameSKU](guides/reporting/v2/metrics#attributedConversions30dSameSKU), [attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d), [attributedConversions7dSameSKU](guides/reporting/v2/metrics#attributedConversions7dSameSKU), [attributedDetailPageView14d](guides/reporting/v2/metrics#attributedDetailPageView14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSales1d](guides/reporting/v2/metrics#attributedSales1d), [attributedSales1dSameSKU](guides/reporting/v2/metrics#attributedSales1dSameSKU), [attributedSales30d](guides/reporting/v2/metrics#attributedSales30d), [attributedSales30dSameSKU](guides/reporting/v2/metrics#attributedSales30dSameSKU), [attributedSales7d](guides/reporting/v2/metrics#attributedSales7d), [attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedSales7dSameSKU), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d), [attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d), [attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d), [attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [currency](guides/reporting/v2/metrics#currency), [impressions](guides/reporting/v2/metrics#impressions), [targetId](guides/reporting/v2/metrics#targetId), [targetingExpression](guides/reporting/v2/metrics#targetingExpression), [targetingText](guides/reporting/v2/metrics#targetingText), [targetingType](guides/reporting/v2/metrics#targetingType), [viewImpressions](guides/reporting/v2/metrics#viewImpressions), [viewAttributedConversions14d](guides/reporting/v2/metrics#viewAttributedConversions14d), [viewAttributedDetailPageView14d](guides/reporting/v2/metrics#viewAttributedDetailPageView14d), [viewAttributedSales14d](guides/reporting/v2/metrics#viewAttributedSales14d), [viewAttributedUnitsOrdered14d](guides/reporting/v2/metrics#viewAttributedUnitsOrdered14d), [viewAttributedOrdersNewToBrand14d](guides/reporting/v2/metrics#viewAttributedOrdersNewToBrand14d), [viewAttributedSalesNewToBrand14d](guides/reporting/v2/metrics#viewattributedsalesnewtobrand14d), [viewAttributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#viewattributedunitsorderednewtobrand14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d), [viewAttributedBrandedSearches14d](guides/reporting/v2/metrics#viewAttributedBrandedSearches14d), [videoCompleteViews](guides/reporting/v2/metrics#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v2/metrics#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v2/metrics#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v2/metrics#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v2/metrics#videoUnmutes), [vtr](guides/reporting/v2/metrics#vtr), [vctr](guides/reporting/v2/metrics#vctr) 

</details>

\*Metric is added to the report by default. 

### Sample requests

If you are copying the example request, make sure to use the correct URL for your geography and valid authentication credentials for your account. 

 <details class="details-bar">
    <summary>Sponsored Products [DEPRECATED]</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/sp/targets/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "impressions,clicks,cost"
    }'
    ```
</details>

 <details class="details-bar">
    <summary>Sponsored Brands</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/hsa/targets/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "impressions,clicks,cost",
    "creativeType": "all"
    }'
    ```
</details>

 <details class="details-bar">
    <summary>Sponsored Display</summary>

     <p>This sample requests performance data for ad groups using contextual targeting (tactic T00020). The metrics requested in this example return data for campaigns using the cost per click (cpc) cost type.</p>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/sd/targets/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "targetId,impressions,clicks,cost",
    "tactic": "T00020"
    }'
    ```
</details>

## Keyword reports

Keywords reports return performance data broken down by the keyword used for targeting. You can generate performance data at the keyword level by requesting a `keywords` report.

>[NOTE] Theme-based targeting has no new metrics compared to the existing keyword report. It is shown as a new entry in the report with `MatchType` as `THEME`.

### Paths

| Ad type |	Path |
|--------------------|--------------------|
| Sponsored Products | /v2/sp/keywords/report [DEPRECATED] |
| Sponsored Brands | /v2/hsa/keywords/report |

>[NOTE] Keyword reports are not available for Sponsored Display campaigns.

### Available metrics

 <details class="details-bar">
    <summary>Sponsored Products [DEPRECATED]</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedConversions1d](guides/reporting/v2/metrics#attributedConversions1d), [attributedConversions1dSameSKU](guides/reporting/v2/metrics#attributedConversions1dSameSKU), [attributedConversions30d](guides/reporting/v2/metrics#attributedConversions30d), [attributedConversions30dSameSKU](guides/reporting/v2/metrics#attributedConversions30dSameSKU), [attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d), [attributedConversions7dSameSKU](guides/reporting/v2/metrics#attributedConversions7dSameSKU), [attributedKindleEditionNormalizedPagesRead14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRead14d), [attributedKindleEditionNormalizedPagesRoyalties14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRoyalties14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSales1d](guides/reporting/v2/metrics#attributedSales1d), [attributedSales1dSameSKU](guides/reporting/v2/metrics#attributedSales1dSameSKU), [attributedSales30d](guides/reporting/v2/metrics#attributedSales30d), [attributedSales30dSameSKU](guides/reporting/v2/metrics#attributedSales30dSameSKU), [attributedSales7d](guides/reporting/v2/metrics#attributedSales7d), [attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedSales7dSameSKU), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d), [attributedUnitsOrdered14dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered14dSameSKU), [attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d), [attributedUnitsOrdered1dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered1dSameSKU), [attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d), [attributedUnitsOrdered30dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered30dSameSKU), [attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d), [attributedUnitsOrdered7dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered7dSameSKU), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [currency](guides/reporting/v2/metrics#currency), [impressions](guides/reporting/v2/metrics#impressions), [keywordId](guides/reporting/v2/metrics#keywordId)\*, [keywordStatus](guides/reporting/v2/metrics#keywordStatus), [keywordText](guides/reporting/v2/metrics#keywordText), [matchType](guides/reporting/v2/metrics#matchType)

</details>
 <details class="details-bar">
    <summary>Sponsored Brands (version 3)</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [applicableBudgetRuleId](guides/reporting/v2/metrics#applicableBudgetRuleId), [applicableBudgetRuleName](guides/reporting/v2/metrics#applicableBudgetRuleName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedDetailPageViewsClicks14d](guides/reporting/v2/metrics#attributedDetailPageViewsClicks14d), [attributedOrderRateNewToBrand14d](guides/reporting/v2/metrics#attributedOrderRateNewToBrand14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedOrdersNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedOrdersNewToBrandPercentage14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedSalesNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedSalesNewToBrandPercentage14d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [attributedUnitsOrderedNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrandPercentage14d), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [campaignRuleBasedBudget](guides/reporting/v2/metrics#campaignRuleBasedBudget), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [dpv14d](guides/reporting/v2/metrics#dpv14d), [impressions](guides/reporting/v2/metrics#impressions), [keywordBid](guides/reporting/v2/metrics#keywordBid), [keywordId](guides/reporting/v2/metrics#keywordId)\*, [keywordStatus](guides/reporting/v2/metrics#keywordStatus), [keywordText](guides/reporting/v2/metrics#keywordText), [matchType](guides/reporting/v2/metrics#matchType), [searchTermImpressionRank](guides/reporting/v2/metrics#searchTermImpressionRank), [searchTermImpressionShare](guides/reporting/v2/metrics#searchTermImpressionShare), [unitsSold14d](guides/reporting/v2/metrics#unitsSold14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d), [topOfSearchImpressionShare](guides/reporting/v2/metrics#topOfSearchImpressionShare)

</details>
 <details class="details-bar">
    <summary>Sponsored Brands Video (version 3) & multi-ad group campaigns (version 4)</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [impressions](guides/reporting/v2/metrics#impressions), [keywordBid](guides/reporting/v2/metrics#keywordBid), [keywordId](guides/reporting/v2/metrics#keywordId)\*, [keywordStatus](guides/reporting/v2/metrics#keywordStatus), [keywordText](guides/reporting/v2/metrics#keywordText), [matchType](guides/reporting/v2/metrics#matchType), [vctr](guides/reporting/v2/metrics#vctr), [video5SecondViewRate](guides/reporting/v2/metrics#video5SecondViewRate), [video5SecondViews](guides/reporting/v2/metrics#video5SecondViews), [videoCompleteViews](guides/reporting/v2/metrics#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v2/metrics#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v2/metrics#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v2/metrics#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v2/metrics#videoUnmutes), [viewableImpressions](guides/reporting/v2/metrics#viewableImpressions), [vtr](guides/reporting/v2/metrics#vtr), [dpv14d](guides/reporting/v2/metrics#dpv14d), [attributedDetailPageViewsClicks14d](guides/reporting/v2/metrics#attributedDetailPageViewsClicks14d), [attributedOrderRateNewToBrand14d](guides/reporting/v2/metrics#attributedOrderRateNewToBrand14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedOrdersNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedOrdersNewToBrandPercentage14d), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedSalesNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedSalesNewToBrandPercentage14d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [attributedUnitsOrderedNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrandPercentage14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d), [currency](guides/reporting/v2/metrics#currency), [topOfSearchImpressionShare](guides/reporting/v2/metrics#topOfSearchImpressionShare)

</details>

\*Metric is added to the report by default.

### Sample requests

If you are copying the example request, make sure to use the correct URL for your geography and valid authentication credentials for your account. 

 <details class="details-bar">
    <summary>Sponsored Products [DEPRECATED]</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/sp/keywords/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "impressions,clicks,cost"
    }'
    ```
</details>

 <details class="details-bar">
    <summary>Sponsored Brands</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/hsa/keywords/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "impressions,clicks,cost",
    "creativeType": "all"
    }'
    ```
</details>

 <details class="details-bar">
    <summary>Sponsored Brands theme-based targeting</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/hsa/keywords/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20230803",
    "metrics": "campaignName,campaignId,campaignStatus,campaignBudget,campaignBudgetType,adGroupId,adGroupName,keywordId,keywordStatus,keywordBid,keywordText,matchType,impressions,clicks,cost,attributedDetailPageViewsClicks14d,attributedSales14d,attributedSales14dSameSKU,attributedConversions14d,attributedConversions14dSameSKU,topOfSearchImpressionShare,searchTermImpressionRank,searchTermImpressionShare"
    "creativeType": "all"
    }'
    ```
</details>

## Search term reports

Search term reports contain performance data broken out by the search term entered by a customer. Note that search term reports only include impressions that resulted in at least one ad click. 

For Sponsored Products and Sponsored Brands, you can create a search term report by requesting a `keywords` report and setting `segment` to `query` in the request body.  

For Sponsored Products campaigns only, you can also request a search terms report for product-targeted campaigns by setting `segment` to `query` in the request body of a `targets` report. 

**Note**: The API does not currently support search terms reports for product-targeted Sponsored Brands campaigns.

### Paths

| Ad type |	Path |
|--------------------|--------------------|
| Sponsored Products | /v2/sp/keywords/report [DEPRECATED]|
| Sponsored Products | /v2/sp/targets/report |
| Sponsored Brands | /v2/hsa/keywords/report |

**Note**: Search term reports are not available for Sponsored Display campaigns.

### Available metrics

 <details class="details-bar">
    <summary>Sponsored Products: keywords [DEPRECATED]</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedConversions1d](guides/reporting/v2/metrics#attributedConversions1d), [attributedConversions1dSameSKU](guides/reporting/v2/metrics#attributedConversions1dSameSKU), [attributedConversions30d](guides/reporting/v2/metrics#attributedConversions30d), [attributedConversions30dSameSKU](guides/reporting/v2/metrics#attributedConversions30dSameSKU), [attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d), [attributedConversions7dSameSKU](guides/reporting/v2/metrics#attributedConversions7dSameSKU), [attributedKindleEditionNormalizedPagesRead14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRead14d), [attributedKindleEditionNormalizedPagesRoyalties14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRoyalties14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSales1d](guides/reporting/v2/metrics#attributedSales1d), [attributedSales1dSameSKU](guides/reporting/v2/metrics#attributedSales1dSameSKU), [attributedSales30d](guides/reporting/v2/metrics#attributedSales30d), [attributedSales30dSameSKU](guides/reporting/v2/metrics#attributedSales30dSameSKU), [attributedSales7d](guides/reporting/v2/metrics#attributedSales7d), [attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedSales7dSameSKU), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d), [attributedUnitsOrdered14dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered14dSameSKU), [attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d), [attributedUnitsOrdered1dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered1dSameSKU), [attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d), [attributedUnitsOrdered30dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered30dSameSKU), [attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d), [attributedUnitsOrdered7dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered7dSameSKU), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [currency](guides/reporting/v2/metrics#currency), [impressions](guides/reporting/v2/metrics#impressions), [keywordId](guides/reporting/v2/metrics#keywordId)\*, [keywordStatus](guides/reporting/v2/metrics#keywordStatus), [keywordText](guides/reporting/v2/metrics#keywordText), [matchType](guides/reporting/v2/metrics#matchType), [query](guides/reporting/v2/metrics#query)\*

</details>

 <details class="details-bar">
    <summary>Sponsored Products: targets [DEPRECATED]</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedConversions1d](guides/reporting/v2/metrics#attributedConversions1d), [attributedConversions1dSameSKU](guides/reporting/v2/metrics#attributedConversions1dSameSKU), [attributedConversions30d](guides/reporting/v2/metrics#attributedConversions30d), [attributedConversions30dSameSKU](guides/reporting/v2/metrics#attributedConversions30dSameSKU), [attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d), [attributedConversions7dSameSKU](guides/reporting/v2/metrics#attributedConversions7dSameSKU), [attributedKindleEditionNormalizedPagesRead14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRead14d), [attributedKindleEditionNormalizedPagesRoyalties14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRoyalties14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSales1d](guides/reporting/v2/metrics#attributedSales1d), [attributedSales1dSameSKU](guides/reporting/v2/metrics#attributedSales1dSameSKU), [attributedSales30d](guides/reporting/v2/metrics#attributedSales30d), [attributedSales30dSameSKU](guides/reporting/v2/metrics#attributedSales30dSameSKU), [attributedSales7d](guides/reporting/v2/metrics#attributedSales7d), [attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedSales7dSameSKU), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d), [attributedUnitsOrdered14dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered14dSameSKU), [attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d), [attributedUnitsOrdered1dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered1dSameSKU), [attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d), [attributedUnitsOrdered30dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered30dSameSKU), [attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d), [attributedUnitsOrdered7dSameSKU](guides/reporting/v2/metrics#attributedUnitsOrdered7dSameSKU), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [impressions](guides/reporting/v2/metrics#impressions), [query](guides/reporting/v2/metrics#query)\*, [targetId](guides/reporting/v2/metrics#targetId)\*, [targetingExpression](guides/reporting/v2/metrics#targetingExpression), [targetingText](guides/reporting/v2/metrics#targetingText), [targetingType](guides/reporting/v2/metrics#targetingType)

</details>

 <details class="details-bar">
    <summary>Sponsored Brands (version 3)</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [impressions](guides/reporting/v2/metrics#impressions), [keywordBid](guides/reporting/v2/metrics#keywordBid), [keywordId](guides/reporting/v2/metrics#keywordId)\*, [keywordStatus](guides/reporting/v2/metrics#keywordStatus), [keywordText](guides/reporting/v2/metrics#keywordText), [matchType](guides/reporting/v2/metrics#matchType), [query](guides/reporting/v2/metrics#query)\*, [searchTermImpressionRank](guides/reporting/v2/metrics#searchTermImpressionRank), [searchTermImpressionShare](guides/reporting/v2/metrics#searchTermImpressionShare)

</details>

 <details class="details-bar">
    <summary>Sponsored Brands Video (version 3) & multi-ad group campaigns (version 4)</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [impressions](guides/reporting/v2/metrics#impressions), [keywordBid](guides/reporting/v2/metrics#keywordBid), [keywordId](guides/reporting/v2/metrics#keywordId)\*, [keywordStatus](guides/reporting/v2/metrics#keywordStatus), [keywordText](guides/reporting/v2/metrics#keywordText), [matchType](guides/reporting/v2/metrics#matchType), [query](guides/reporting/v2/metrics#query)\*, [vctr](guides/reporting/v2/metrics#vctr), [video5SecondViewRate](guides/reporting/v2/metrics#video5SecondViewRate), [video5SecondViews](guides/reporting/v2/metrics#video5SecondViews), [videoCompleteViews](guides/reporting/v2/metrics#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v2/metrics#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v2/metrics#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v2/metrics#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v2/metrics#videoUnmutes), [viewableImpressions](guides/reporting/v2/metrics#viewableImpressions), [vtr](guides/reporting/v2/metrics#vtr), [currency](guides/reporting/v2/metrics#currency) 
</details>

\*Metric is added to the report by default. 

### Sample requests

If you are copying the example request, make sure to use the correct URL for your geography and valid authentication credentials for your account.

 <details class="details-bar">
    <summary>Sponsored Products [DEPRECATED]</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/sp/keywords/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "impressions,clicks,cost",
    "segment": "query"
    }'
    ```

</details>

 <details class="details-bar">
    <summary>Sponsored Brands</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/hsa/keywords/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "impressions,clicks,cost",
    "segment": "query",
    "creativeType": "all"
    }'
    ```
    
</details>

 <details class="details-bar">
    <summary>Sponsored Brands Video</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/hsa/keywords/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "impressions,clicks,cost",
    "segment": "query",
    "creativeType": "video"
    }'
    ```
    
</details>

 <details class="details-bar">
    <summary>Sponsored Brands theme-based targeting</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/hsa/keywords/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20230803",
    "metrics": "campaignName,campaignId,campaignStatus,campaignBudget,campaignBudgetType,adGroupId,adGroupName,keywordId,keywordStatus,keywordBid,keywordText,matchType,impressions,clicks,cost,attributedDetailPageViewsClicks14d,attributedSales14d,attributedSales14dSameSKU,attributedConversions14d,attributedConversions14dSameSKU,searchTermImpressionRank,searchTermImpressionShare",
    "segment": "query",
    "creativeType": "all"
    }'
    ```
    
</details>

## ASIN reports

ASIN reports contain *halo conversions*, or conversions that occur when a customer clicks on your ad, but places an order where the purchased SKU/ASIN was different from the SKU/ASIN advertised.

>[TIP:Sponsored Products] Currently ASIN reports cannot include metrics associated with both keywords and targets. If `targetingId` is added as a metric in the request, the report filters on targets and does not return sales associated with keywords. If `targetingId` is not included in the request, the report filters on keywords and does not return sales associated with targets. Therefore, the default behavior filters the report on keywords. Also note that if you include both `keywordId` and `targetingId` in the metrics list, the report filters on targets only and does not return keywords.

### Paths

| Ad type |	Path |
|--------------------|--------------------|
| Sponsored Products | /v2/sp/asins/report [DEPRECATED]|
| Sponsored Display | /sd/asins/report |

**Note**: ASIN reports are not available for Sponsored Brands campaigns.

### Available metrics

 <details class="details-bar">
    <summary>Sponsored Products [DEPRECATED]</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [asin](guides/reporting/v2/metrics#asin), [attributedKindleEditionNormalizedPagesRead14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRead14d), [attributedKindleEditionNormalizedPagesRoyalties14d](guides/reporting/v2/metrics#attributedKindleEditionNormalizedPagesRoyalties14d), [attributedSales14dOtherSKU](guides/reporting/v2/metrics#attributedSales14dOtherSKU), [attributedSales1dOtherSKU](guides/reporting/v2/metrics#attributedSales1dOtherSKU), [attributedSales30dOtherSKU](guides/reporting/v2/metrics#attributedSales30dOtherSKU), [attributedSales7dOtherSKU](guides/reporting/v2/metrics#attributedSales7dOtherSKU), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d), [attributedUnitsOrdered14dOtherSKU](guides/reporting/v2/metrics#attributedUnitsOrdered14dOtherSKU), [attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d), [attributedUnitsOrdered1dOtherSKU](guides/reporting/v2/metrics#attributedUnitsOrdered1dOtherSKU), [attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d), [attributedUnitsOrdered30dOtherSKU](guides/reporting/v2/metrics#attributedUnitsOrdered30dOtherSKU), [attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d), [attributedUnitsOrdered7dOtherSKU](guides/reporting/v2/metrics#attributedUnitsOrdered7dOtherSKU), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [currency](guides/reporting/v2/metrics#currency), [keywordId](guides/reporting/v2/metrics#keywordId), [keywordText](guides/reporting/v2/metrics#keywordText), [matchType](guides/reporting/v2/metrics#matchType), [otherAsin](guides/reporting/v2/metrics#otherAsin), [sku](guides/reporting/v2/metrics#sku) (seller only), [targetId](guides/reporting/v2/metrics#targetId), [targetingExpression](guides/reporting/v2/metrics#targetingExpression), [targetingText](guides/reporting/v2/metrics#targetingText), [targetingType](guides/reporting/v2/metrics#targetingType)

</details>

 <details class="details-bar">
    <summary>Sponsored Display</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [asin](guides/reporting/v2/metrics#asin), [attributedSales14dOtherSKU](guides/reporting/v2/metrics#attributedSales14dOtherSKU), [attributedSales1dOtherSKU](guides/reporting/v2/metrics#attributedSales1dOtherSKU), [attributedSales30dOtherSKU](guides/reporting/v2/metrics#attributedSales30dOtherSKU), [attributedSales7dOtherSKU](guides/reporting/v2/metrics#attributedSales7dOtherSKU), [attributedUnitsOrdered14dOtherSKU](guides/reporting/v2/metrics#attributedUnitsOrdered14dOtherSKU), [attributedUnitsOrdered1dOtherSKU](guides/reporting/v2/metrics#attributedUnitsOrdered1dOtherSKU), [attributedUnitsOrdered30dOtherSKU](guides/reporting/v2/metrics#attributedUnitsOrdered30dOtherSKU), [attributedUnitsOrdered7dOtherSKU](guides/reporting/v2/metrics#attributedUnitsOrdered7dOtherSKU), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [currency](guides/reporting/v2/metrics#currency), [otherAsin](guides/reporting/v2/metrics#otherAsin), [sku](guides/reporting/v2/metrics#sku) (seller only), [viewAttributedUnitsOrdered1dOtherSKU](guides/reporting/v2/metrics#viewAttributedUnitsOrdered1dOtherSKU), [viewAttributedUnitsOrdered7dOtherSKU](guides/reporting/v2/metrics#viewAttributedUnitsOrdered7dOtherSKU), [viewAttributedUnitsOrdered14dOtherSKU](guides/reporting/v2/metrics#viewAttributedUnitsOrdered14dOtherSKU), [viewAttributedUnitsOrdered30dOtherSKU](guides/reporting/v2/metrics#viewAttributedUnitsOrdered30dOtherSKU), [viewAttributedSales1dOtherSKU](guides/reporting/v2/metrics#viewAttributedSales1dOtherSKU), [viewAttributedSales7dOtherSKU](guides/reporting/v2/metrics#viewAttributedSales7dOtherSKU), [viewAttributedSales14dOtherSKU](guides/reporting/v2/metrics#viewAttributedSales14dOtherSKU), [viewAttributedSales30dOtherSKU](guides/reporting/v2/metrics#viewAttributedSales30dOtherSKU), [viewAttributedConversions1dOtherSKU](guides/reporting/v2/metrics#viewAttributedConversions1dOtherSKU), [viewAttributedConversions7dOtherSKU](guides/reporting/v2/metrics#viewAttributedConversions7dOtherSKU), [viewAttributedConversions14dOtherSKU](guides/reporting/v2/metrics#viewAttributedConversions14dOtherSKU), [viewAttributedConversions30dOtherSKU](guides/reporting/v2/metrics#viewAttributedConversions30dOtherSKU), [attributedConversions1dOtherSKU](guides/reporting/v2/metrics#attributedConversions1dOtherSKU), [attributedConversions7dOtherSKU](guides/reporting/v2/metrics#attributedConversions7dOtherSKU), [attributedConversions14dOtherSKU](guides/reporting/v2/metrics#attributedConversions14dOtherSKU), [attributedConversions30dOtherSKU](guides/reporting/v2/metrics#attributedConversions30dOtherSKU).

</details>

### Sample requests

If you are copying the example request, make sure to use the correct URL for your geography and valid authentication credentials for your account.

 <details class="details-bar">
    <summary>Sponsored Products [DEPRECATED]</summary>

    <p>**Note**: Sponsored Products ASINs reports require the `campaignType` parameter as part of the request body. It should always be set to `sponsoredProducts`. This example returns sales associated with keywords. To get the full set of purchased products, request a second report that includes `targetId` and omits `keywordId` in the metric list.</p>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/sp/asins/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "otherAsin,asin,keywordId,campaignId,adGroupId,attributedSales14dOtherSKU,attributedSales1dOtherSKU,attributedUnitsOrdered30dOtherSKU",
    "campaignType": "sponsoredProducts"
    }'
    ```
</details>

 <details class="details-bar">
    <summary>Sponsored Display</summary>

     <p>This sample requests performance data for ad groups using audience targeting (tactic T00030).</p>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/sd/asins/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "otherAsin,asin,campaignId,adGroupId,attributedSales1dOtherSKU,attributedUnitsOrdered14dOtherSKU",
    "tactic": "T00030"
    }'
    ```
</details>

## Ad reports

Ad reports help you understand how Sponsored Brands campaigns are performing at the ad level. 

### Paths

| Ad type |	Path |
|--------------------|--------------------|
| Sponsored Brands | /v2/hsa/ads/report |

**Note**: To get performance data at the advertised product level for Sponsored Products and Sponsored Display, see [Product ad reports](#product-ad-reports).

### Available metrics

<details class="details-bar">
    <summary>Sponsored Brands (version 3)</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [applicableBudgetRuleId](guides/reporting/v2/metrics#applicableBudgetRuleId), [applicableBudgetRuleName](guides/reporting/v2/metrics#applicableBudgetRuleName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedDetailPageViewsClicks14d](guides/reporting/v2/metrics#attributedDetailPageViewsClicks14d), [attributedOrderRateNewToBrand14d](guides/reporting/v2/metrics#attributedOrderRateNewToBrand14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedOrdersNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedOrdersNewToBrandPercentage14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedSalesNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedSalesNewToBrandPercentage14d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [attributedUnitsOrderedNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrandPercentage14d), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [campaignRuleBasedBudget](guides/reporting/v2/metrics#campaignRuleBasedBudget), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [dpv14d](guides/reporting/v2/metrics#dpv14d), [impressions](guides/reporting/v2/metrics#impressions), [unitsSold14d](guides/reporting/v2/metrics#unitsSold14d), [vctr](guides/reporting/v2/metrics#vctr), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d)

</details> 

<details class="details-bar">
    <summary>Sponsored Brands Video (version 3) & multi-ad group campaigns (version 4)</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [adId](guides/reporting/v2/metrics#adId), [applicableBudgetRuleId](guides/reporting/v2/metrics#applicableBudgetRuleId), [applicableBudgetRuleName](guides/reporting/v2/metrics#applicableBudgetRuleName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedDetailPageViewsClicks14d](guides/reporting/v2/metrics#attributedDetailPageViewsClicks14d), [attributedOrderRateNewToBrand14d](guides/reporting/v2/metrics#attributedOrderRateNewToBrand14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedOrdersNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedOrdersNewToBrandPercentage14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedSalesNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedSalesNewToBrandPercentage14d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [attributedUnitsOrderedNewToBrandPercentage14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrandPercentage14d), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignBudgetType](guides/reporting/v2/metrics#campaignBudgetType), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [campaignRuleBasedBudget](guides/reporting/v2/metrics#campaignRuleBasedBudget), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [dpv14d](guides/reporting/v2/metrics#dpv14d), [impressions](guides/reporting/v2/metrics#impressions), [vctr](guides/reporting/v2/metrics#vctr), [video5SecondViewRate](guides/reporting/v2/metrics#video5SecondViewRate), [video5SecondViews](guides/reporting/v2/metrics#video5SecondViews), [videoCompleteViews](guides/reporting/v2/metrics#videoCompleteViews), [videoFirstQuartileViews](guides/reporting/v2/metrics#videoFirstQuartileViews), [videoMidpointViews](guides/reporting/v2/metrics#videoMidpointViews), [videoThirdQuartileViews](guides/reporting/v2/metrics#videoThirdQuartileViews), [videoUnmutes](guides/reporting/v2/metrics#videoUnmutes), [viewableImpressions](guides/reporting/v2/metrics#viewableImpressions), [vtr](guides/reporting/v2/metrics#vtr), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d), [currency](guides/reporting/v2/metrics#currency)

</details> 

>[NOTE] `adId` is only available for [multi-ad group campaigns](reference/migration-guides/sb-v3-v4). For video campaigns created using the [version 3 campaigns endpoints](sponsored-brands/3-0/openapi#/Campaigns), `adId` will not be available. 

### Sample requests

If you are copying the example request, make sure to use the correct URL for your geography and valid authentication credentials for your account.

 <details class="details-bar">
    <summary>Sponsored Brands and Sponsored Brands Video</summary>

    <p>Ad reports must have `creativeType` set to `all` in the request body. The report will contain data for both Sponsored Brands and Sponsored Brands Video campaigns.</p>


    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/hsa/ads/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "adId,adGroupId,campaignId,impressions,clicks,cost,attributedConversions14d,attributedSales14d",
    "creativeType":"all"
    }'
    ```
</details>

## Matched target reports

Matched target reports help you understand how Sponsored Display campaigns are performing based on the product detail page where they appeared. Matched target reports only contain ASINs for product detail pages that recorded at least one ad click. Matched target reports for Sponsored Display can be broken down at the campaign, ad group, or target level.

For Sponsored Display, you can create a matched target report by requesting a `campaigns`, `adGroups`, or `targets`  report and setting `segment` to `matchedTarget` in the request body. 

### Paths

| Ad type |	Path |
|--------------------|--------------------|
| Sponsored Display campaigns | /sd/campaigns/report |
| Sponsored Display ad groups | /sd/adGroups/report |
| Sponsored Display targets | /sd/targets/report |

### Available metrics

 <details class="details-bar">
    <summary>Sponsored Display: campaigns</summary>

[attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedConversions1d](guides/reporting/v2/metrics#attributedConversions1d), [attributedConversions1dSameSKU](guides/reporting/v2/metrics#attributedConversions1dSameSKU), [attributedConversions30d](guides/reporting/v2/metrics#attributedConversions30d), [attributedConversions30dSameSKU](guides/reporting/v2/metrics#attributedConversions30dSameSKU), [attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d), [attributedConversions7dSameSKU](guides/reporting/v2/metrics#attributedConversions7dSameSKU), [attributedDetailPageView14d](guides/reporting/v2/metrics#attributedDetailPageView14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSales1d](guides/reporting/v2/metrics#attributedSales1d), [attributedSales1dSameSKU](guides/reporting/v2/metrics#attributedSales1dSameSKU), [attributedSales30d](guides/reporting/v2/metrics#attributedSales30d), [attributedSales30dSameSKU](guides/reporting/v2/metrics#attributedSales30dSameSKU), [attributedSales7d](guides/reporting/v2/metrics#attributedSales7d), [attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedSales7dSameSKU), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d), [attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d), [attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d), [attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [campaignBudget](guides/reporting/v2/metrics#campaignBudget), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [campaignStatus](guides/reporting/v2/metrics#campaignStatus), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [costType](guides/reporting/v2/metrics#costType), [currency](guides/reporting/v2/metrics#currency), [impressions](guides/reporting/v2/metrics#impressions), [viewAttributedConversions14d](guides/reporting/v2/metrics#viewAttributedConversions14d), [viewAttributedDetailPageView14d](guides/reporting/v2/metrics#viewAttributedDetailPageView14d), [viewAttributedSales14d](guides/reporting/v2/metrics#viewAttributedSales14d), [viewAttributedUnitsOrdered14d](guides/reporting/v2/metrics#viewAttributedUnitsOrdered14d), [viewImpressions](guides/reporting/v2/metrics#viewImpressions), [viewAttributedOrdersNewToBrand14d](guides/reporting/v2/metrics#viewAttributedOrdersNewToBrand14d), [viewAttributedSalesNewToBrand14d](guides/reporting/v2/metrics#viewattributedsalesnewtobrand14d), [viewAttributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#viewattributedunitsorderednewtobrand14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d), [viewAttributedBrandedSearches14d](guides/reporting/v2/metrics#viewAttributedBrandedSearches14d), [matchedTarget](guides/reporting/v2/metrics#matchedTarget)\*

</details>

 <details class="details-bar">
    <summary>Sponsored Display: ad groups</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedConversions1d](guides/reporting/v2/metrics#attributedConversions1d), [attributedConversions1dSameSKU](guides/reporting/v2/metrics#attributedConversions1dSameSKU), [attributedConversions30d](guides/reporting/v2/metrics#attributedConversions30d), [attributedConversions30dSameSKU](guides/reporting/v2/metrics#attributedConversions30dSameSKU), [attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d), [attributedConversions7dSameSKU](guides/reporting/v2/metrics#attributedConversions7dSameSKU), [attributedDetailPageView14d](guides/reporting/v2/metrics#attributedDetailPageView14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSales1d](guides/reporting/v2/metrics#attributedSales1d), [attributedSales1dSameSKU](guides/reporting/v2/metrics#attributedSales1dSameSKU), [attributedSales30d](guides/reporting/v2/metrics#attributedSales30d), [attributedSales30dSameSKU](guides/reporting/v2/metrics#attributedSales30dSameSKU), [attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedSales7dSameSKU), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d), [attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d), [attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d), [attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [bidOptimization](guides/reporting/v2/metrics#bidOptimization), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [currency](guides/reporting/v2/metrics#currency), [impressions](guides/reporting/v2/metrics#impressions), [viewAttributedConversions14d](guides/reporting/v2/metrics#viewAttributedConversions14d), [viewAttributedDetailPageView14d](guides/reporting/v2/metrics#viewAttributedDetailPageView14d), [viewAttributedSales14d](guides/reporting/v2/metrics#viewAttributedSales14d), [viewAttributedUnitsOrdered14d](guides/reporting/v2/metrics#viewAttributedUnitsOrdered14d), [viewImpressions](guides/reporting/v2/metrics#viewImpressions),[viewAttributedOrdersNewToBrand14d](guides/reporting/v2/metrics#viewAttributedOrdersNewToBrand14d), [viewAttributedSalesNewToBrand14d](guides/reporting/v2/metrics#viewattributedsalesnewtobrand14d), [viewAttributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#viewattributedunitsorderednewtobrand14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d), [viewAttributedBrandedSearches14d](guides/reporting/v2/metrics#viewAttributedBrandedSearches14d), [matchedTarget](guides/reporting/v2/metrics#matchedTarget)\*

</details>

 <details class="details-bar">
    <summary>Sponsored Display: targets</summary>

[adGroupId](guides/reporting/v2/metrics#adGroupId), [adGroupName](guides/reporting/v2/metrics#adGroupName), [attributedConversions14d](guides/reporting/v2/metrics#attributedConversions14d), [attributedConversions14dSameSKU](guides/reporting/v2/metrics#attributedConversions14dSameSKU), [attributedConversions1d](guides/reporting/v2/metrics#attributedConversions1d), [attributedConversions1dSameSKU](guides/reporting/v2/metrics#attributedConversions1dSameSKU), [attributedConversions30d](guides/reporting/v2/metrics#attributedConversions30d), [attributedConversions30dSameSKU](guides/reporting/v2/metrics#attributedConversions30dSameSKU), [attributedConversions7d](guides/reporting/v2/metrics#attributedConversions7d), [attributedConversions7dSameSKU](guides/reporting/v2/metrics#attributedConversions7dSameSKU), [attributedDetailPageView14d](guides/reporting/v2/metrics#attributedDetailPageView14d), [attributedOrdersNewToBrand14d](guides/reporting/v2/metrics#attributedOrdersNewToBrand14d), [attributedSales14d](guides/reporting/v2/metrics#attributedSales14d), [attributedSales14dSameSKU](guides/reporting/v2/metrics#attributedSales14dSameSKU), [attributedSales1d](guides/reporting/v2/metrics#attributedSales1d), [attributedSales1dSameSKU](guides/reporting/v2/metrics#attributedSales1dSameSKU), [attributedSales30d](guides/reporting/v2/metrics#attributedSales30d), [attributedSales30dSameSKU](guides/reporting/v2/metrics#attributedSales30dSameSKU), [attributedSales7d](guides/reporting/v2/metrics#attributedSales7d), [attributedSales7dSameSKU](guides/reporting/v2/metrics#attributedSales7dSameSKU), [attributedSalesNewToBrand14d](guides/reporting/v2/metrics#attributedSalesNewToBrand14d), [attributedUnitsOrdered14d](guides/reporting/v2/metrics#attributedUnitsOrdered14d), [attributedUnitsOrdered1d](guides/reporting/v2/metrics#attributedUnitsOrdered1d), [attributedUnitsOrdered30d](guides/reporting/v2/metrics#attributedUnitsOrdered30d), [attributedUnitsOrdered7d](guides/reporting/v2/metrics#attributedUnitsOrdered7d), [attributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#attributedUnitsOrderedNewToBrand14d), [campaignId](guides/reporting/v2/metrics#campaignId), [campaignName](guides/reporting/v2/metrics#campaignName), [clicks](guides/reporting/v2/metrics#clicks), [cost](guides/reporting/v2/metrics#cost), [currency](guides/reporting/v2/metrics#currency), [impressions](guides/reporting/v2/metrics#impressions), [targetId](guides/reporting/v2/metrics#targetId), [targetingExpression](guides/reporting/v2/metrics#targetingExpression), [targetingText](guides/reporting/v2/metrics#targetingText), [targetingType](guides/reporting/v2/metrics#targetingType), [viewAttributedConversions14d](guides/reporting/v2/metrics#viewAttributedConversions14d), [viewAttributedDetailPageView14d](guides/reporting/v2/metrics#viewAttributedDetailPageView14d), [viewAttributedSales14d](guides/reporting/v2/metrics#viewAttributedSales14d), [viewAttributedUnitsOrdered14d](guides/reporting/v2/metrics#viewAttributedUnitsOrdered14d), [viewAttributedOrdersNewToBrand14d](guides/reporting/v2/metrics#viewAttributedOrdersNewToBrand14d), [viewAttributedSalesNewToBrand14d](guides/reporting/v2/metrics#viewattributedsalesnewtobrand14d), [viewAttributedUnitsOrderedNewToBrand14d](guides/reporting/v2/metrics#viewattributedunitsorderednewtobrand14d), [attributedBrandedSearches14d](guides/reporting/v2/metrics#attributedBrandedSearches14d), [viewAttributedBrandedSearches14d](guides/reporting/v2/metrics#viewAttributedBrandedSearches14d), [matchedTarget](guides/reporting/v2/metrics#matchedTarget)\*

</details>

\*Metric is added to the report by default. 

### Sample requests

If you are copying the example request, make sure to use the correct URL for your geography and valid authentication credentials for your account.

 <details class="details-bar">
    <summary>Sponsored Display: targets</summary>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/sd/targets/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": 20220301,
    "tactic": "T00030",
    "metrics": "campaignId,adGroupId,targetId,targetingExpression,targetingText,targetingType,impressions,clicks,cost,currency,attributedConversions1d,attributedConversions7d,attributedConversions14d,attributedConversions30d,attributedUnitsOrdered1d,attributedUnitsOrdered7d,attributedUnitsOrdered14d,attributedUnitsOrdered30d,attributedSales1d,attributedSales7d,attributedSales14d,attributedSales30d,attributedSales1dSameSKU,attributedSales7dSameSKU,attributedSales14dSameSKU,attributedSales30dSameSKU",
    "segment": "matchedTarget"
    }'
    ```
</details>