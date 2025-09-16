---
title: Introduction to the MMM data feed
description: Overview of the MMM data feed's inputs and output
type: guide
interface: api
tags:
  - Reporting
  - Marketing Mix Modeling
---
# MMM data feed

The purpose of this document aims to clarify the various inputs and outputs of the MMM data feed. It summarizes input parameters and outlines the resultant output files generated from these inputs.

## Brand groups
You can request MMM data for brands that sell or advertise on Amazon. MMM program managers set up brand groups as part of the MMM onboarding process. For more information, see [Brand group access](guides/reporting/marketing-mix-modeling/get-started#brand-group-access).

## Time unit
The report time unit determines how data is aggregated, either `DAILY` or `WEEKLY`. `DAILY` aggregation allows any report date range selection, while `WEEKLY` aggregation requires full weeks ending on Saturday or Sunday. 

We require a one-week period to verify data accuracy and complete aggregation so report data will be available up to the previous week’s Sunday. Requested data covers a maximum of the past three years.

## Metrics type
`MEDIA_ONLY` reports include data for DSP and Sponsored Ads campaigns only. `MEDIA_AND_SALES` reports provide sales insights, covering ASIN sales, promotions, coupons, and Subscribe & Save transactions.

## Geographic granularity
Data is also aggregated by geographic granularity of `COUNTRY`, `POSTAL_CODE`, or [`DMA`](https://www.nielsen.com/insights/2025/what-is-a-designated-market-area-and-why-does-it-matter/). `POSTAL_CODE` data is available in Canada, Germany, Spain, France, Italy, the United States, and the United Kingdom, while `DMA` data is limited to the United States.

## Report files
The output files generated depend on the selected inputs and report type. Each file is structured to align with the chosen reporting parameters. Refer to the table below for descriptions of the output files and how input selections impact file generation.

<table><thead>
  <tr>
    <th rowspan="2">Metrics type</th>
    <th rowspan="2">File</th>
    <th rowspan="2">Description</th>
    <th colspan="3">Geographic granularity</th>
  </tr>
  <tr>
    <th>Country</th>
    <th>Postal Code</th>
    <th>DMA®</th>
  </tr></thead>
<tbody>
  <tr>
    <td rowspan="6">Media</td>
    <td>AggregatedSummaryReport.xlsx</td>
    <td>Aggregate DSP, paid search, and sales metadata.</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
  </tr>
  <tr>
    <td>AdGrainGeoIPCampaignPerformanceMetrics.tsv.zip</td>
    <td>Display ads: Ad-grain campaign metadata for DSP campaigns. Includes: Buy type, creative tactic, site, device type, impressions, clicks, spend, and currency.</td>
    <td></td>
    <td>✓</td>
    <td>✓</td>
  </tr>
  <tr>
    <td>CreativeGrainNationalCampaignPerformanceMetrics.tsv.zip</td>
    <td>Display ads: Creative-grain campaign metadata for DSP campaigns. Includes: Buy type, creative tactic, site, device type, impressions, clicks, spend, and currency.</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
  </tr>
  <tr>
    <td>SSPAGeoCampaignPerformanceMetrics.tsv.zip</td>
    <td>Paid search ads: Geographic level campaign metadata for Sponsored Products, Product Display Ads, Sponsored Brands. Includes: Buy type, ad type, site, impressions, clicks, spend, and currency.</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
  </tr>
  <tr>
    <td>SSPANationalCampaignPerformanceMetrics.tsv.zip</td>
    <td>Paid search ads: National level campaign metadata for Sponsored Products, Product Display Ads, Sponsored Brands. Includes: Buy type, ad type, site, impressions, clicks, spend, and currency.</td>
    <td></td>
    <td>✓</td>
    <td>✓</td>
  </tr>
  <tr>
    <td>STVRecommendations.pdf</td>
    <td>STV Best Practices</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
  </tr>
  <tr>
    <td rowspan="7">Sales</td>
    <td>TotalSalesAsinList.tsv.zip</td>
    <td>ASIN and item descriptions. Includes: ASIN, GL code, product group label, category label, subcategory label, brand name, and item name.</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
  </tr>
  <tr>
    <td>TotalSalesGeoOrders.tsv.zip</td>
    <td>Sales by ASIN by geography by week. Includes: ASIN, geography, sold units, retail price, retail sales, and currency.</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
  </tr>
  <tr>
    <td>TotalSalesPromoAsinList.tsv.zip</td>
    <td>ASINs with promotional activity. Includes: ASIN and promo ID.</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
  </tr>
  <tr>
    <td>TotalSalesPromoDims.tsv.zip</td>
    <td>Promotion details and cost of promotion. Includes: Promo ID, start date, end date, promotion type, promotion name, merchandising fee, spend to promote, total spend, and currency.</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
  </tr>
  <tr>
    <td>TotalSalesPromoFacts.tsv.zip</td>
    <td>Promo sales per ASIN by geography by week. Includes: Promo ID, ASIN, geography, units sold, total discount, and currency.</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
  </tr>
  <tr>
    <td>TotalSalesVpcClips.tsv.zip</td>
    <td># of vendor powered coupons clipped nationally per promo. Includes: Promo ID and clips.</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
  </tr>
  <tr>
    <td>TotalSalesGeoOrdersSns.tsv.zip</td>
    <td>Sales via subscribe and save. Includes: ASIN, geography, new SnS units, new SnS sales, replenished SnS Units, replenished SnS sales, and currency.</td>
    <td>✓</td>
    <td>✓</td>
    <td>✓</td>
  </tr>
</tbody></table>

