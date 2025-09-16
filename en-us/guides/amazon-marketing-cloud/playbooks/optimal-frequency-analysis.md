---
title: Amazon Marketing Cloud - Optimal Frequency Analysis
description: AMC Optimal Frequency Analysis Playbook
type: guide
interface: api
---
# Optimal frequency analysis

Within Amazon Marketing Cloud, advertisers have a unique opportunity to measure and attribute conversions to the frequency of impressions served to individual users over the course of a historical time window.
This playbook will dive deeper into the concept of frequency to gain clear optimization insights and direct actions within Amazon Ads to optimize frequency analysis.

To do so, we will be assessing the holistic business value with increasing or decreasing the desired frequency with an advertising activation. This will include identifying:

- identifying the budget spend on over-exposure and reinvesting to reach new audiences.
- determining where conversions are lost when audiences aren’t being properly retargeted.
- identifying the point where the diminishing returns of increased frequency takes effect to accordingly  plan  marketing efforts.

Without optimizing frequency, an advertiser is subject to overexposing their audience and creating “ad-fatigue” and in turn wasted ad spend that could have been reinvested to reaching new audience members. An underexposed audience is one where the audience has seen too few ads over the course of a time period, and is likely to forget about the brand / product when final making the purchase decision.

![Percent_change_kpi](/_images/amazon-marketing-cloud/playbooks/optimal_freq_analysis/PB_OFA_1_percent_change_kpi.png)

### Benefits

Through the guidance in this playbook, you will get answers to the following questions:

- How to measure frequency within a user-base based on your use case?
- How to find the optimal frequency point algorithmically?
- How to find the under-served frequency algorithmically?
- How find the over-exposure point with frequency algorithmically?

### Prerequisites

To derive results based on the guidance in this playbook, we assume that you fulfill the following criteria:

- Access to  AMC instance
- At least 7 days of backfilled data
- Minimum 4 active campaigns

To effectively use this playbook, we recommend you understand the following concepts:

- **AMC concepts**: Specifically concepts such as data aggregation thresholds, privacy limitations, and other restrictions listed in the AMC documentation.
- **Knowledge of AMC SQL**: The process outlined here relies on altering SQL code to match your purposes.
- **Basic Python**: This playbook is built using Python, and may require some scripting to adapt it to your use case.
- **Amazon QuickSight**: You will need to know how to ingest data in Amazon QuickSight to output the required visualizations.
- **Data-driven decision-making**:  A penchant for turning insights into action with media activations from AMC performance measures.

### Assumptions

- While the queries in this playbook are broken out by campaign ID, they can be broken out by any tactical dimensions available within AMC such as advertiser, audience segment, or entity.
- The queries listed here look into measuring the frequency cap on a daily basis. The attributed conversions towards these daily impressions follow the same attribution logic of the `amazon_attributed_events_*` table.
- The queries used in this playbook would be measured over the span of the most recent 90 days and will use aggregates to get the daily measure used for the insights.

### Playbook steps

The playbook is structured to perform the following steps:

1. Determine and set frequency caps
2. Identify under-served audiences
   * Activate under-served audiences
3. Determine optimal frequency point
4. Identify actions from analysis

> [NOTE] The concepts presented in this playbook will go into the technical depth of the mathematical concepts and code required to measure the frequency insights utilized in the playbook.

### Concepts and definitions

To help you understand this playbook better, the following section describes key concepts and defines relevant terminology:

**Advertising frequency** refers to the number of times a particular advertisement is exposed to a target audience within a given period, typically within a campaign duration. Through this metric, you must aim to strike a balance between creating brand awareness and avoiding audience fatigue.

An **Optimal frequency** ensures that the ad or message is seen or heard enough times to make an impact without becoming intrusive or irritating to the audience and being wasteful to the advertising budget. The optimal frequency finding can be utilized in media flights by audience creation, bid optimization, and frequency capping.

**Frequency capping** allows you (the advertiser) to specify a maximum number of impressions/views a user can see an ad or a cluster of ads over a specific time period. For example, if an advertiser specifies a frequency cap of 3 over the course of the day, the ad publisher will measure the number of impressions served to a user over the course of the day in real time, and once they’ve been served an impression thrice, they will then be excluded from any further advertising for the rest of the day. Finding a cap where the increased frequency is causing diminishing returns in brand value (or an ad fatigue) will help reduce media spend waste.

**Frequency buckets**, in the context of this playbook, is a accumulation of spend and return value.

## Frequency by ad format

The frequency approach demonstrated in this playbook will aim to factor in each ad format available in AMC to generate a holistic analysis across all advertising touch points that a user can experience within a campaign. This will include anything available within Amazon demand-side platform (Amazon DSP) and Sponsored ads tables within AMC.

The following is a baseline query that measures some aggregate metrics across all the ad types.
We will then build upon this frequency measure to collect some of the advanced insights that will address our major use cases.

##### SQL

<details class="details-bar">
<summary>Click to see: Query to measure an aggregate metric  </summary>

```
WITH
events AS (
  SELECT
    user_id,
    SUM(impressions) as impressions,
    0 AS clicks,
    0 AS purchases, 
    0 AS units_sold, 
    0 AS product_sales, 
    0 AS add_to_cart, 
    0 AS detail_page_view,
    0 AS new_to_brand_product_sales,
    0 AS new_to_brand_purchases,
    0 AS new_to_brand_units_sold,
    0 AS pixel_conversions, --new, total pixel conversions
    0 AS off_amazon_conversions,--new, total AAT/CAPI
    0 AS off_amazon_conversion_value, --overlap and combined. sales amount for Off-Amazon purchase conversions
    0 AS off_amazon_product_sales,--overlap and combined. value of off Amazon non-purchase conversions. Value is unit-less and advertiser defined.
    0 AS total_purchases,
    0 AS total_units_sold,
    0 AS total_product_sales,
    0 AS total_add_to_cart,
    0 AS new_to_brand_total_product_sales,
    0 AS new_to_brand_total_purchases,
    0 AS new_to_brand_total_units_sold,
    0 AS total_detail_page_view,
    SUM(total_cost/100000) as total_cost   
  FROM
    dsp_impressions
  GROUP BY
    1
  UNION ALL
  SELECT
    user_id,
    0 AS impressions,
    SUM(clicks) AS clicks,
    0 AS purchases, 
    0 AS units_sold, 
    0 AS product_sales, 
    0 AS add_to_cart, 
    0 AS detail_page_view,
    0 AS new_to_brand_product_sales,
    0 AS new_to_brand_purchases,
    0 AS new_to_brand_units_sold,
    0 AS pixel_conversions, --new, total pixel conversions
    0 AS off_amazon_conversions,--new, total AAT/CAPI
    0 AS off_amazon_conversion_value, --new, sales amount for Off-Amazon purchase conversions
    0 AS off_amazon_product_sales,--new, value of off Amazon non-purchase conversions. Value is unit-less and advertiser defined.
  
    0 AS total_purchases,
    0 AS total_units_sold,
    0 AS total_product_sales,
    0 AS total_add_to_cart,
    0 AS new_to_brand_total_product_sales,
    0 AS new_to_brand_total_purchases,
    0 AS new_to_brand_total_units_sold,
    0 AS total_detail_page_view,
  
    0 as total_cost  
  FROM
    dsp_clicks
  GROUP BY
    1
  
UNION ALL
SELECT 
    user_id, 
    SUM(impressions) as impressions,
    SUM(clicks) AS clicks,
    0 AS purchases, 
    0 AS units_sold, 
    0 AS product_sales, 
    0 AS add_to_cart, 
    0 AS detail_page_view,
    0 AS new_to_brand_product_sales,
    0 AS new_to_brand_purchases,
    0 AS new_to_brand_units_sold,
    0 AS pixel_conversions, --new, total pixel conversions
    0 AS off_amazon_conversions,--new, total AAT/CAPI
    0 AS off_amazon_conversion_value, --new, sales amount for Off-Amazon purchase conversions
    0 AS off_amazon_product_sales,--new, value of off Amazon non-purchase conversions. Value is unit-less and advertiser defined.
  
  
    0 AS total_purchases,
    0 AS total_units_sold,
    0 AS total_product_sales,
    0 AS total_add_to_cart,
    0 AS new_to_brand_total_product_sales,
    0 AS new_to_brand_total_purchases,
    0 AS new_to_brand_total_units_sold,
    0 AS total_detail_page_view,
  
  
    SUM(spend/100000000) as total_cost   
FROM sponsored_ads_traffic
group by 1
UNION ALL
SELECT 
    user_id, 
    0 AS impressions, 
    0 as clicks,  
    SUM(purchases) as purchases,
    SUM(units_sold) as units_sold, 
    SUM(product_sales) as product_sales,
    SUM(add_to_cart) as add_to_cart, 
    SUM(detail_page_view) AS detail_page_view,
    SUM(new_to_brand_product_sales) AS new_to_brand_product_sales,
    SUM(new_to_brand_purchases) AS new_to_brand_purchases,
    SUM(new_to_brand_units_sold) AS new_to_brand_units_sold,
    0 AS pixel_conversions, --new, total pixel conversions
    0 AS off_amazon_conversions,--new, total AAT/CAPI
    0 AS off_amazon_conversion_value, --new, sales amount for Off-Amazon purchase conversions
    0 AS off_amazon_product_sales,--new, value of off Amazon non-purchase conversions. Value is unit-less and advertiser defined.
  
    SUM(total_purchases) AS total_purchases,
    SUM(total_units_sold) AS total_units_sold,
    SUM(total_product_sales) AS total_product_sales,
    SUM(total_add_to_cart) AS total_add_to_cart,
    SUM(new_to_brand_total_product_sales) AS new_to_brand_total_product_sales,
    SUM(new_to_brand_total_purchases) AS new_to_brand_total_purchases,
    SUM(new_to_brand_total_units_sold) AS new_to_brand_total_units_sold,
    SUM(total_detail_page_view) AS total_detail_page_view,
  
    0 as total_cost  
FROM amazon_attributed_events_by_traffic_time
WHERE  conversion_event_category NOT in ('pixel', 'off-Amazon') -- update conversion_event_type for endemic only 
group by 1

UNION ALL --- extract pixel conversions
SELECT 
    user_id, 
    0 AS impressions, 
    0 as clicks,  
    0 as purchases,
    0 as units_sold, 
    0 as product_sales,
    SUM(add_to_cart) as add_to_cart, 
    SUM(detail_page_view) AS detail_page_view,
    0 AS new_to_brand_product_sales,
    0 AS new_to_brand_purchases,
    0 AS new_to_brand_units_sold,
    SUM(conversions) AS pixel_conversions, --new, total pixel conversions
    0 AS off_amazon_conversions,--new, total AAT/CAPI
    0 AS off_amazon_conversion_value, --new, sales amount for Off-Amazon purchase conversions
    0 AS off_amazon_product_sales,--new, value of off Amazon non-purchase conversions. Value is unit-less and advertiser defined.
  
    SUM(total_purchases) AS total_purchases,
    SUM(total_units_sold) AS total_units_sold,
    SUM(total_product_sales) AS total_product_sales,
    SUM(total_add_to_cart) AS total_add_to_cart,
    SUM(new_to_brand_total_product_sales) AS new_to_brand_total_product_sales,
    SUM(new_to_brand_total_purchases) AS new_to_brand_total_purchases,
    SUM(new_to_brand_total_units_sold) AS new_to_brand_total_units_sold,
    SUM(total_detail_page_view) AS total_detail_page_view,
    0 as total_cost  
FROM amazon_attributed_events_by_traffic_time
WHERE  conversion_event_category NOT IN ('purchase', 'off-Amazon')  -- update conversion_event_type for endemic only 
GROUP BY 1
UNION ALL --- extract off-Amazon conversions
SELECT 
    user_id, 
    0 AS impressions, 
    0 as clicks,  
    0 as purchases,
    0 as units_sold, 
    0 as product_sales,
    SUM(add_to_cart) as add_to_cart, 
    SUM(detail_page_view) AS detail_page_view,
    0 AS new_to_brand_product_sales,
    0 AS new_to_brand_purchases,
    0 AS new_to_brand_units_sold,
    0 AS pixel_conversions, --new, total pixel conversions
    SUM(conversions)  AS off_amazon_conversions,--new, total AAT/CAPI
    SUM(off_amazon_conversion_value) AS off_amazon_conversion_value, --new, sales amount for Off-Amazon purchase conversions
    SUM(off_amazon_product_sales) AS off_amazon_product_sales,--new, value of off Amazon non-purchase conversions. Value is unit-less and advertiser defined.
  
    SUM(total_purchases) AS total_purchases,
    SUM(total_units_sold) AS total_units_sold,
    SUM(total_product_sales) AS total_product_sales,
    SUM(total_add_to_cart) AS total_add_to_cart,
    SUM(new_to_brand_total_product_sales) AS new_to_brand_total_product_sales,
    SUM(new_to_brand_total_purchases) AS new_to_brand_total_purchases,
    SUM(new_to_brand_total_units_sold) AS new_to_brand_total_units_sold,
    SUM(total_detail_page_view) AS total_detail_page_view,

    0 as total_cost  
FROM amazon_attributed_events_by_traffic_time
WHERE  conversion_event_category NOT IN ('purchase', 'pixel')  -- update conversion_event_type for endemic only  
GROUP BY 1
  
),
events_by_user AS (
  SELECT
    user_id,
    IF(
      SUM(impressions) > 0,
      SUM(impressions),
      0
    ) AS impressions,
    IF(
      SUM(impressions) > 0, 
      SUM(impressions), 
      0
    ) AS frequency,
    SUM(clicks) as clicks,
    SUM(purchases) AS purchases,
    SUM(units_sold) AS units_sold,
    SUM(product_sales) AS product_sales,
    SUM(add_to_cart) AS add_to_cart,
    SUM(detail_page_view) AS detail_page_view,
    SUM(new_to_brand_product_sales) AS new_to_brand_product_sales,
    SUM(new_to_brand_purchases) AS new_to_brand_purchases,
    SUM(new_to_brand_units_sold) AS new_to_brand_units_sold, 
    SUM(pixel_conversions) AS pixel_conversions,
    SUM(off_amazon_conversions) AS off_amazon_conversions,
    SUM(off_amazon_conversion_value) AS off_amazon_conversion_value,
    SUM(off_amazon_product_sales) AS off_amazon_product_sales,
    SUM(total_purchases) AS total_purchases,
    SUM(total_units_sold) AS total_units_sold,
    SUM(total_product_sales) AS total_product_sales,
    SUM(total_add_to_cart) AS total_add_to_cart,
    SUM(new_to_brand_total_product_sales) AS new_to_brand_total_product_sales,
    SUM(new_to_brand_total_purchases) AS new_to_brand_total_purchases,
    SUM(new_to_brand_total_units_sold) AS new_to_brand_total_units_sold,
    SUM(total_detail_page_view) AS total_detail_page_view,
    SUM(total_cost) as cost
  FROM
    events
  GROUP BY
    1
),
frequency_by_user AS (
  SELECT
    impressions,
    frequency AS frequency_bucket,
    clicks,
    purchases,
    units_sold,
    product_sales,
    add_to_cart,
    detail_page_view,
    new_to_brand_product_sales,
    new_to_brand_purchases,
    new_to_brand_units_sold,
    pixel_conversions,
    off_amazon_conversions,
    off_amazon_conversion_value,
    off_amazon_product_sales,
    total_purchases,
    total_units_sold,
    total_product_sales,
    total_add_to_cart,
    new_to_brand_total_product_sales,
    new_to_brand_total_purchases,
    new_to_brand_total_units_sold,
    total_detail_page_view,
    cost,
    user_id
  FROM
    events_by_user
  WHERE
    user_id IS NOT NULL
)
SELECT
  frequency_bucket,
  SUM(clicks) AS clicks,
  SUM(purchases) AS purchases,
  SUM(units_sold) AS units_sold,
  SUM(product_sales) AS product_sales,
  SUM(add_to_cart) AS add_to_cart,
  SUM(detail_page_view) AS detail_page_view,
  COUNT(user_id) AS users_in_bucket,
  SUM(impressions) AS impressions_in_bucket,
  SUM(new_to_brand_product_sales) AS new_to_brand_product_sales,
  SUM(new_to_brand_purchases) AS new_to_brand_purchases,
  SUM(new_to_brand_units_sold) AS new_to_brand_units_sold, 
  SUM(pixel_conversions) AS pixel_conversions,
  SUM(off_amazon_conversions) AS off_amazon_conversions,
  SUM(off_amazon_conversion_value) AS off_amazon_conversion_value,
  SUM(off_amazon_product_sales) AS off_amazon_product_sales,
  SUM(total_purchases) AS total_purchases,
  SUM(total_units_sold) AS total_units_sold,
  SUM(total_product_sales) AS total_product_sales,
  SUM(total_add_to_cart) AS total_add_to_cart,
  SUM(new_to_brand_total_product_sales) AS new_to_brand_total_product_sales,
  SUM(new_to_brand_total_purchases) AS new_to_brand_total_purchases,
  SUM(new_to_brand_total_units_sold) AS new_to_brand_total_units_sold,  
  SUM(total_detail_page_view) AS total_detail_page_view,
  SUM(cost) AS cost
FROM
  frequency_by_user
GROUP BY
  1
  

```

</details>

### Sample output

And the output is below is up to a frequency bucket of 15:

| frequency\_bucket | clicks | purchases | units_sold | product\_sales | add\_to\_cart | detail\_page\_view | users\_in\_bucket | impressions\_in\_bucket | new\_to\_brand\_product\_sales | new\_to\_brand\_purchases | new\_to\_brand\_units\_sold | pixel\_conversions | off\_amazon\_conversions | off\_amazon\_conversion\_value | off\_amazon\_product\_sales | total\_purchases | total\_units\_sold | total\_product\_sales | total\_add\_to\_cart | new\_to\_brand\_total\_product\_sales | new\_to\_brand\_total\_purchases | new\_to\_brand\_total\_units\_sold | total\_detail\_page\_view | cost       |
| ----------------- | ------ | --------- | ---------- | -------------- | ------------- | ------------------ | ----------------- | ----------------------- | ------------------------------ | ------------------------- | --------------------------- | ------------------ | ------------------------ | ------------------------------ | --------------------------- | ---------------- | ------------------ | --------------------- | -------------------- | ------------------------------------- | -------------------------------- | ---------------------------------- | ------------------------- | ---------- |
| 0                 | 1884   | 1500      | 1802       | 24312.07       | 1428          | 292839             | 215105            | 0                       | 16584.95                       | 1041                      | 1220                        | 158938             | 158938                   | 0                              | 0                           | 2741             | 3791               | 46291.19              | 6309                 | 26652.98                              | 1848                             | 2265                               | 463770                    | 3819.37141 |
| 1                 | 30699  | 957       | 1116       | 13245.27       | 2031          | 69402              | 58916153          | 58916153                | 8928.14                        | 679                       | 779                         | 47632              | 47632                    | 0                              | 0                           | 2242             | 2811               | 33192.3               | 10017                | 18786.01                              | 1394                             | 1675                               | 125607                    | 92775.6655 |
| 2                 | 28280  | 1499      | 1756       | 19177.65       | 2418          | 57546              | 24092003          | 48184006                | 13450.05                       | 1067                      | 1261                        | 39856              | 39856                    | 0                              | 0                           | 3177             | 3812               | 42254.6               | 11334                | 23992.77                              | 1929                             | 2281                               | 100233                    | 77132.9198 |
| 3                 | 24714  | 1781      | 1974       | 22980.08       | 2046          | 50352              | 14284874          | 42854622                | 16020.34                       | 1270                      | 1393                        | 33927              | 33927                    | 0                              | 0                           | 3545             | 4071               | 46208.59              | 11031                | 27708.16                              | 2198                             | 2445                               | 83145                     | 68634.9905 |
| 4                 | 27232  | 2080      | 2318       | 27568.68       | 2235          | 49800              | 12228028          | 48912112                | 18957.93                       | 1464                      | 1612                        | 33664              | 33664                    | 0                              | 0                           | 4085             | 4773               | 54272.39              | 11478                | 30997.63                              | 2504                             | 2826                               | 82263                     | 65181.5909 |
| 5                 | 24084  | 2349      | 2589       | 30960.55       | 2334          | 45288              | 7816056           | 39080280                | 21770.99                       | 1700                      | 1829                        | 30862              | 30862                    | 0                              | 0                           | 4446             | 5244               | 59551.41              | 11742                | 35634.56                              | 2804                             | 3126                               | 73875                     | 60435.2568 |
| 6                 | 23967  | 2674      | 2927       | 35280.42       | 2514          | 43920              | 6182858           | 37097148                | 24835.31                       | 1931                      | 2076                        | 30631              | 30631                    | 0                              | 0                           | 4831             | 5526               | 64061.69              | 12261                | 38310.46                              | 3068                             | 3399                               | 72549                     | 57939.5659 |
| 7                 | 22393  | 3131      | 3399       | 40744.29       | 2634          | 42780              | 4636208           | 32453456                | 28898.51                       | 2246                      | 2379                        | 29545              | 29545                    | 0                              | 0                           | 5315             | 5972               | 68412.17              | 12669                | 42181.89                              | 3453                             | 3746                               | 69162                     | 55052.6733 |
| 8                 | 22432  | 3201      | 3481       | 43197.35       | 2625          | 42972              | 3977962           | 31823696                | 31201.47                       | 2328                      | 2504                        | 29297              | 29297                    | 0                              | 0                           | 5497             | 6208               | 72026.56              | 12483                | 44293.31                              | 3566                             | 3914                               | 68511                     | 53717.3385 |
| 9                 | 21794  | 3363      | 3663       | 45258.55       | 2775          | 41712              | 3279764           | 29517876                | 31515.67                       | 2368                      | 2538                        | 28933              | 28933                    | 0                              | 0                           | 5649             | 6307               | 73523.3               | 12975                | 45417.81                              | 3669                             | 3998                               | 66786                     | 52389.5323 |
| 10                | 21745  | 3677      | 3980       | 48629.88       | 2814          | 43872              | 2795657           | 27956570                | 33816.4                        | 2588                      | 2738                        | 29867              | 29867                    | 0                              | 0                           | 6147             | 6909               | 79453.13              | 13338                | 48780                                 | 3931                             | 4272                               | 69276                     | 51352.588  |
| 11                | 21218  | 3943      | 4241       | 52537.46       | 3129          | 43287              | 2392244           | 26314684                | 37435.51                       | 2794                      | 2965                        | 29009              | 29009                    | 0                              | 0                           | 6453             | 7231               | 83781.86              | 13821                | 52583.48                              | 4185                             | 4560                               | 66471                     | 49892.6796 |
| 12                | 21437  | 4080      | 4359       | 53821.38       | 3168          | 44106              | 2175011           | 26100132                | 36785.31                       | 2810                      | 2962                        | 30119              | 30119                    | 0                              | 0                           | 6724             | 7356               | 86045.66              | 14472                | 52201.11                              | 4268                             | 4562                               | 68772                     | 49278.2075 |
| 13                | 21171  | 4180      | 4503       | 56641.58       | 3132          | 43494              | 1918969           | 24946597                | 39371.88                       | 2902                      | 3074                        | 29257              | 29257                    | 0                              | 0                           | 6906             | 7747               | 89610.81              | 13995                | 55780.96                              | 4424                             | 4864                               | 66975                     | 48026.0431 |
| 14                | 21293  | 4330      | 4666       | 58319.51       | 3237          | 44859              | 1711221           | 23957094                | 39659.71                       | 2982                      | 3158                        | 30181              | 30181                    | 0                              | 0                           | 7007             | 7748               | 90303.39              | 14064                | 55397.23                              | 4464                             | 4831                               | 68871                     | 47923.8989 |
| 15                | 20705  | 4355      | 4666       | 58367.99       | 3207          | 44499              | 1533020           | 22995300                | 39948.23                       | 2975                      | 3143                        | 29822              | 29822                    | 0                              | 0                           | 7180             | 7938               | 92461.17              | 14355                | 56917.08                              | 4552                             | 4901                               | 67470                     | 47167.0883 |

The query will group users by the number of impressions they were served during the time window (in this case 90 days), and measure the total number of clicks, purchases, units_sold, attributed to those impressions.

### Frequency analysis for first-party datasets

Depending on the first-party (1P) data uploaded, it's possible to incorporate frequency analysis queries into 1P data structures. The data uploaded must include revenue data with time stamps of conversions attached to a `user_id` that can match to the AMC data, it's possible to tie advertising frequency to non-endemic attribution. To do so, you would just need to UNION the off-Amazon conversion tables with the on-Amazon conversions in the Common Table Expression (CTE) to implement in the model.

Additionally, this playbook use case can be applied for advertisers who wish to upload log files from another ad server to create additional off-Amazon impressions. This will ensure that your entire advertising ecosystem is accounted for to measure the optimal frequency for action. To do so, you would want to ensure your uploaded impressions can be matched to the ad placement level to that which can be found in AMC.

## Solution methodology

The section below outlines a general approach to defining the inputs to calculate:

- Minimal frequency
- Optimal frequency
- Maximum frequency bucket caps across campaigns

The goal of this section is to provide a potential use case and a walk through  on how to use a frequency measurement approach to solve for it.

### Set frequency caps

This approach will measure if the accumulated investment after a certain point (cut-off) is outweighing the accumulating returns at an additional frequency bucket. To do so, measure cumulative cost and cumulative return KPI relationship across the frequency buckets and measure each bucket compared to the bucket prior.

![Frequency_caps](/_images/amazon-marketing-cloud/playbooks/optimal_freq_analysis/PB_OFA_2_frequency_caps.png)

To derive frequency buckets, we introduce a measure called the **cumulative sum** for both cost and return (such as total sales and purchases). These will allow us to measure the relative change in spend and return value with the increased frequency in the market. Before you get to calculating frequency caps, familiarize yourself with the following definitions and equations:

- **Cumulative cost**: The total cost of each frequency bucket along with all the frequency buckets before it.
  - `Cumulative cost = Cost (at N frequency Bucket) + Cost (at N-1 Frequency Bucket)`
- **Cumulative return KPI**: The total return KPI of each frequency bucket along with all the frequency buckets before it.
  - `Cumulative Return KPI (Purchases) = Total Purchases (at N frequency bucket) + Total Purchases (at N-1 frequency bucket)`
- **Cumulative cost percentage**: The percentage value of the cumulative cost at each frequency bucket / the total cumulative cost of all the frequency buckets.
  - `Cumulative Cost Percentage = Cost (at N frequency bucket) / Total Cost of all Frequency Buckets`
- **Cumulative return percentage**: The percentage value of the cumulative return kpi at each frequency bucket / the total cumulative return KPI of all the frequency buckets.
  - `Cumulative Return Percentage (Purchases) = Purchases (at N frequency bucket) / Total Purchases of all Frequency Buckets`
- **Percent difference**: The difference (subtract) of the cumulative cost percentage from the cumulative return percentage to measure the differences in cumulative investment and return from a cost and return standpoint.
  - `Percent Difference = Cumulative Conversion Percentage - Cumulative Cost Percentage`
- **Percent change**: Using the percent difference column, you will subtract the percent difference of each row from the previous frequency bucket to measure the relative change in the ratio of cumulative cost and return at each frequency bucket. This metric will be used to identify the recommended frequency cap.
  - `Percent Change = Percent Difference (at N Frequency Bucket) - Percent Difference (at N - 1 Frequency Bucket)`

First, calculate cumulative percentages and then use the percent change to determine the frequency cap.

To do so, we take the cumulative cost and cumulative KPI volume at each frequency bucket and then measure the cumulative percentage of each based on their overall volume of conversions. This is because frequency buckets don't exist in a vacuum and that a total investment has to increase to increase the frequency bucket. Hence the need to consider the cost of all the users prior who had a frequency at a given bucket and below.

To calculate the cumulative return percentage, see the example (below), if the conversions at frequency bucket 1 are 9,203 and the conversions at frequency bucket 2 are 6,532, then the cumulative frequency at bucket 2 is 15,735 (9,203+5,532). If the total cumulative conversions (for all frequency buckets) is 48,795, then the cumulative conversion percentage at frequency bucket 2 is 15,735 / 48,795 = 32%.

Repeat this step with the cost column at each of the frequency buckets to get both the cumulative conversions percentage and the cumulative cost percentage. With these two columns, you will then subtract the cumulative cost percentage from the cumulative conversions percentage to get the values for the Percent difference column.

And finally, subtract the percent difference at each row and the percent difference from the previous row to get the percentage change column. For example, if your percent difference in row two is 14.18% and your percent difference in row one is 9.90%, then the percentage change in row 2 is 4.28%.

The percentage change column approaches zero as the frequency bucket increases. At the point where the percent change passes the zero threshold, the growth rate of the additional cost exceeds the growth rate of the additional return value. It is at this point when the percentage change passes zero (in our example, it passes zero at frequency bucket = 4) that you should set for your frequency cap.

An example of how this could look is depicted below:

| Frequency bucket | Conversions | Cumulative conversions | Cost  | Cumulative cost | Cumulative conversions % | Cumulative cost percentage | Percent difference | Percentage change |
| ---------------- | ----------- | ---------------------- | ----- | --------------- | ------------------------ | -------------------------- | ------------------ | ----------------- |
| 1                | 9,203       | 9,203                  | 7,801 | $7,801.20       | 19%                      | 9%                         | 9.90%              |                   |
| 2                | 6,532       | 15,735                 | 7,933 | $15,733.90      | 32%                      | 18%                        | 14.18%             | 4.28%             |
| 3                | 5,024       | 20,759                 | 7,492 | $23,225.80      | 43%                      | 27%                        | 15.87%             | 1.69%             |
| 4                | 4,157       | 24,916                 | 7,060 | $30,286.20      | 51%                      | 35%                        | 16.28%             | 0.41%             |
| 5                | 3,569       | 28,485                 | 6,621 | $36,907.20      | 58%                      | 42%                        | 15.99%             | -0.29%            |
| 6                | 3,213       | 31,698                 | 6,217 | $43,123.70      | 65%                      | 50%                        | 15.43%             | -0.55%            |
| 7                | 2,564       | 34,262                 | 5,884 | $49,007.40      | 70%                      | 56%                        | 13.93%             | -1.50%            |
| 8                | 2,554       | 36,816                 | 5,598 | $54,605.70      | 75%                      | 63%                        | 12.74%             | -1.20%            |
| 9                | 2,276       | 39,092                 | 5,324 | $59,930         | 80%                      | 69%                        | 11.29%             | -1.45%            |
| 10               | 1,985       | 41,077                 | 5,036 | $64,965.80      | 84%                      | 75%                        | 9.57%              | -1.72%            |
| 11               | 1,773       | 42,850                 | 4,811 | $69,776.50      | 88%                      | 80%                        | 7.68%              | -1.89%            |
| 12               | 1,794       | 44,644                 | 4,609 | $74,385         | 91%                      | 85%                        | 6.06%              | -1.62%            |
| 13               | 1,485       | 46,129                 | 4,376 | $78,760.60      | 95%                      | 90%                        | 4.08%              | -1.98%            |
| 14               | 1,376       | 47,505                 | 4,217 | $82,977.40      | 97%                      | 95%                        | 2.06%              | -2.02%            |
| 15               | 1,290       | 48,795                 | 4,093 | $87,070         | 100%                     | 100%                       | 0%                 | -2.06%            |

This query accounts for multiple days of performance and averages the frequency buckets and performance by date to account for any variance that may take place over a long period of time. The method for analyzing is by applying an average aggregate on the date flag that will be used to normalize the total cost and total conversions. Using this approach, we are looking for the point in which the percentage change column goes below zero, and the recommended frequency cap would be the last percentage change value above zero. In this case, we would set our daily frequency cap to 4.

This query can be used to measure the frequency cap of any of the KPIs tracked within AMC using the cumulative cost and return methodology above. In the query below, we will demonstrate this use case across the following KPIs:

* **`clicks`** - Use when focused on building your frequency around maximizing upper funnel engagement and site traffic. Could also use Views.
* **`add_to_cart / detail_page_view`** - Use when focused on building your frequency around mid funnel engagement, and want to reinvest towards a rules based audience to re-target cart abandoners.
* **`purchases / units_sold / product_sales`** - Use when optimizing your frequency around maximizing lower funnel attribution, as well as when to reinvest towards reaching new audiences
* **`new_to_brand_product_sales`** - Use when optimizing your frequency around acquiring net new customers.

All aggregated on a daily basis. This query can be adjusted to account for any time frame in which you are interested in measuring frequency, as well as any conversion metric you want to build frequency caps around.

##### SQL

<detailsclass="details-bar">

<summary>Click to see: Query to check optimal frequency</summary>

```

--Optimal Frequency

WITH
events AS (
  SELECT
    impression_date,
    user_id,
    SUM(impressions) as impressions,
    0 AS clicks,
    0 AS purchases, 
    0 AS units_sold, 
    0 AS product_sales, 
    0 AS add_to_cart, 
    0 AS detail_page_view,
    0 AS new_to_brand_product_sales,
    SUM(total_cost/100000) as total_cost     
  FROM
    dsp_impressions
  GROUP BY
    1,
    2
  UNION ALL

  SELECT
    impression_date,
    user_id,
    0 AS impressions,
    SUM(clicks) AS clicks,
    0 AS purchases, 
    0 AS units_sold, 
    0 AS product_sales, 
    0 AS add_to_cart, 
    0 AS detail_page_view,
    0 AS new_to_brand_product_sales,

    0 as total_cost  
  FROM
    dsp_clicks
  GROUP BY
    1,
    2
    
UNION ALL

SELECT 
    event_date as impression_date,
    user_id, 
    SUM(impressions) as impressions,
    SUM(clicks) AS clicks,
    0 AS purchases, 
    0 AS units_sold, 
    0 AS product_sales, 
    0 AS add_to_cart, 
    0 AS detail_page_view,
    0 AS new_to_brand_product_sales,
    
    SUM(spend/100000000) as total_cost   
FROM sponsored_ads_traffic
group by 1, 2

UNION ALL

SELECT 
    impression_date,
    user_id, 
    0 AS impressions, 
    0 as clicks,    
    SUM(purchases) as purchases,
    SUM(units_sold) as units_sold, 
    SUM(product_sales) as product_sales,
    SUM(add_to_cart) as add_to_cart, 
    SUM(detail_page_view) AS detail_page_view,
    SUM(new_to_brand_product_sales) AS new_to_brand_product_sales,
    
    0 as total_cost  

FROM amazon_attributed_events_by_traffic_time

WHERE  conversion_event_category = 'purchase'  -- update conversion_event_type for endemic only 
group by 1, 2


UNION ALL --- extract pixel conversions

SELECT 
    impression_date,
    user_id, 
    0 AS impressions, 
    0 as clicks,    
    0 as purchases,
    0 as units_sold, 
    0 as product_sales,
    SUM(add_to_cart) as add_to_cart, 
    SUM(detail_page_view) AS detail_page_view,
    0 AS new_to_brand_product_sales,

    0 as total_cost  

FROM amazon_attributed_events_by_traffic_time

WHERE  conversion_event_category = 'pixel'  -- update conversion_event_type for endemic only 

GROUP BY 1, 2

UNION ALL --- extract off-Amazon conversions

SELECT 
    impression_date,
    user_id, 
    0 AS impressions, 
    0 as clicks,    
    0 as purchases,
    0 as units_sold, 
    0 as product_sales,
    SUM(add_to_cart) as add_to_cart, 
    SUM(detail_page_view) AS detail_page_view,
    0 AS new_to_brand_product_sales,

    0 as total_cost  

FROM amazon_attributed_events_by_traffic_time

WHERE  conversion_event_category = 'off-Amazon'  -- update conversion_event_type for endemic only  

GROUP BY 1, 2

    
),
events_by_user AS (
  SELECT
    impression_date,
    user_id,
    IF(
      SUM(impressions) > 0,
      SUM(impressions),
      0
    ) AS impressions,
    IF(
      SUM(impressions) > 0, 
      SUM(impressions), 
      0
    ) AS frequency,
    SUM(clicks) as clicks,
    SUM(purchases) AS purchases,
    SUM(units_sold) AS units_sold,
    SUM(product_sales) AS product_sales,
    SUM(add_to_cart) AS add_to_cart,
    SUM(detail_page_view) AS detail_page_view,
    SUM(new_to_brand_product_sales) AS new_to_brand_product_sales,
    SUM(total_cost) as cost
  FROM
    events
  GROUP BY
    1,
    2
),
frequency_by_user AS (
  SELECT
    impression_date,
    impressions,
    frequency AS frequency_bucket,
    clicks,
    purchases,
    units_sold,
    product_sales,
    add_to_cart,
    detail_page_view,
    new_to_brand_product_sales,
    cost,
    user_id
  FROM
    events_by_user
  WHERE
    user_id IS NOT NULL
)
, freq_bucket as (
SELECT
  impression_date,
  frequency_bucket,
  SUM(clicks) AS clicks,
  SUM(purchases) AS purchases,
  SUM(units_sold) AS units_sold,
  SUM(product_sales) AS product_sales,
  SUM(add_to_cart) AS add_to_cart,
  SUM(detail_page_view) AS detail_page_view,
  COUNT(user_id) AS users_in_bucket,
  SUM(impressions) AS impressions_in_bucket,
  SUM(new_to_brand_product_sales) AS new_to_brand_product_sales,
  SUM(cost) AS cost
FROM
  frequency_by_user
GROUP BY
  1,
  2
  ),
freq_bucket_avg as (
SELECT
  frequency_bucket,
  median(clicks) AS clicks,
  median(purchases) AS purchases,
  median(units_sold) AS units_sold,
  median(product_sales) AS product_sales,
  median(add_to_cart) AS add_to_cart,
  median(detail_page_view) AS detail_page_view,
  median(impressions_in_bucket) AS impressions_in_bucket,
  median(new_to_brand_product_sales) AS new_to_brand_product_sales,
  median(cost) AS cost
FROM
  freq_bucket
WHERE frequency_bucket <= 25 and frequency_bucket > 0
--Where frequency_bucket <= 25
GROUP BY
  1
  ),
cumulative_total as (
select
    c1.frequency_bucket,
    c1.clicks,
    sum(c2.clicks) over (partition by c1.frequency_bucket) as CumulativeClicks,
    c1.purchases,
    sum(c2.purchases) over (partition by c1.frequency_bucket) as CumulativePurchases,   
    c1.units_sold,
    sum(c2.units_sold) over (partition by c1.frequency_bucket) as Cumulativeunits_sold, 
    c1.product_sales,
    sum(c2.product_sales) over (partition by c1.frequency_bucket) as Cumulativeproduct_sales,
    c1.add_to_cart,
    sum(c2.add_to_cart) over (partition by c1.frequency_bucket) as CumulativeATC,
    c1.detail_page_view,
    sum(c2.detail_page_view) over (partition by c1.frequency_bucket) as CumulativeDPV,
    c1.impressions_in_bucket,
    sum(c2.impressions_in_bucket) over (partition by c1.frequency_bucket) as CumulativeImpressions,   
    c1.new_to_brand_product_sales,
    sum(c2.new_to_brand_product_sales) over (partition by c1.frequency_bucket) as Cumulative_new_to_brand_product_sales,   
    c1.cost,
    sum(c2.cost) over (partition by c1.frequency_bucket) as CumulativeCost
From freq_bucket_avg c1
left join freq_bucket_avg c2
on c1.frequency_bucket >= c2.frequency_bucket 
    ), 
percent_change as (
select
    frequency_bucket,
    cost,
    CumulativeCost,
    CumulativeCost / max(CumulativeCost) over () as cumulative_cost_perc,
    clicks,
    CumulativeClicks,
    CumulativeClicks / max(CumulativeClicks) over () as CumulativeClicks_perc,
    (CumulativeClicks / max(CumulativeClicks) over ()) - (CumulativeCost / max(CumulativeCost) over ()) as percent_clicks_diff, 
    purchases,
    CumulativePurchases,
    CumulativePurchases / max(CumulativePurchases) over () as CumulativePurchases_perc,
    (CumulativePurchases / max(CumulativePurchases) over ()) - (CumulativeCost / max(CumulativeCost) over ()) as percent_purchases_diff, 
    units_sold,
    Cumulativeunits_sold,
    Cumulativeunits_sold / max(Cumulativeunits_sold) over () as Cumulativeunits_sold_perc,
    (Cumulativeunits_sold / max(Cumulativeunits_sold) over ()) - (CumulativeCost / max(CumulativeCost) over ()) as percent_units_sold_diff, 
    product_sales,
    Cumulativeproduct_sales,
    Cumulativeproduct_sales / max(Cumulativeproduct_sales) over () as cumulative_product_sales_perc,
    (Cumulativeproduct_sales / max(Cumulativeproduct_sales) over ()) - (CumulativeCost / max(CumulativeCost) over ()) as percent_sales_diff,
    add_to_cart,
    CumulativeATC,
    CumulativeATC / max(CumulativeATC) over () as CumulativeATC_perc,
    (CumulativeATC / max(CumulativeATC) over ()) - (CumulativeCost / max(CumulativeCost) over ()) as percent_ATC_diff, 
    detail_page_view,
    CumulativeDPV,
    CumulativeDPV / max(CumulativeDPV) over () as CumulativeDPV_perc,
    (CumulativeDPV / max(CumulativeDPV) over ()) - (CumulativeCost / max(CumulativeCost) over ()) as percent_DPV_diff, 
    impressions_in_bucket,
    CumulativeImpressions,
    CumulativeImpressions / max(CumulativeImpressions) over () as CumulativeImpressions_perc,
    (CumulativeImpressions / max(CumulativeImpressions) over ()) - (CumulativeCost / max(CumulativeCost) over ()) as percent_impressions_diff,
    new_to_brand_product_sales,
    Cumulative_new_to_brand_product_sales,
    Cumulative_new_to_brand_product_sales / max(Cumulative_new_to_brand_product_sales) over () as Cumulative_new_to_brand_product_sales_perc,
    (Cumulative_new_to_brand_product_sales / max(Cumulative_new_to_brand_product_sales) over ()) - (CumulativeCost / max(CumulativeCost) over ()) as percent_new_to_brand_product_sales_diff
from cumulative_total
),
frequency_grouping as (
select 
    a.frequency_bucket,
    avg(a.cost) as cost,
    avg(a.CumulativeCost) as CumulativeCost,
    avg(a.cumulative_cost_perc) as cumulative_cost_perc,
    avg(a.clicks) as clicks,
    avg(a.CumulativeClicks) as CumulativeClicks,
    avg(a.CumulativeClicks_perc) as CumulativeClicks_perc,
    b.percent_clicks_diff - a.percent_clicks_diff as clicks_percent_change,
    avg(a.purchases) as purchases,
    avg(a.CumulativePurchases) as CumulativePurchases,
    avg(a.CumulativePurchases_perc) as CumulativePurchases_perc,
    b.percent_purchases_diff - a.percent_purchases_diff as purchases_percent_change,
    avg(a.units_sold) as units_sold,
    avg(a.Cumulativeunits_sold) as Cumulativeunits_sold,
    avg(a.Cumulativeunits_sold_perc) as Cumulativeunits_sold_perc,
    b.percent_units_sold_diff - a.percent_units_sold_diff as unts_sold_percent_change,
    avg(a.product_sales) as product_sales,
    avg(a.Cumulativeproduct_sales) as Cumulativeproduct_sales,
    avg(a.cumulative_product_sales_perc) as cumulative_product_sales_perc,
    b.percent_sales_diff - a.percent_sales_diff as sales_percent_change,
    avg(a.add_to_cart) as add_to_cart,
    avg(a.CumulativeATC) as CumulativeATC,
    avg(a.CumulativeATC_perc) as CumulativeATC_perc,
    b.percent_ATC_diff - a.percent_ATC_diff as ATC_percent_change,
    avg(a.detail_page_view) as detail_page_view,
    avg(a.CumulativeDPV) as CumulativeDPV,
    avg(a.CumulativeDPV_perc) as CumulativeDPV_perc,
    b.percent_DPV_diff - a.percent_DPV_diff as DPV_percent_change,
    avg(a.impressions_in_bucket) as impressions_in_bucket,
    avg(a.CumulativeImpressions) as CumulativeImpressions,
    avg(a.CumulativeImpressions_perc) as CumulativeImpressions_perc,
    b.percent_impressions_diff - a.percent_impressions_diff as impressions_percent_change,
    avg(a.new_to_brand_product_sales) as new_to_brand_product_sales,
    avg(a.Cumulative_new_to_brand_product_sales) as Cumulative_new_to_brand_product_sales,
    avg(a.Cumulative_new_to_brand_product_sales_perc) as Cumulative_new_to_brand_product_sales_perc,
    b.percent_new_to_brand_product_sales_diff - a.percent_new_to_brand_product_sales_diff as new_to_brand_product_sales_percent_change  
from percent_change a
left join percent_change b
on a.frequency_bucket = b.frequency_bucket - 1 
group by 1, 8, 12, 16, 20, 24, 28, 32, 36

)
SELECT
    frequency_bucket,
    clicks_percent_change,
    purchases_percent_change,
    unts_sold_percent_change,
    sales_percent_change,
    ATC_percent_change,
    DPV_percent_change,
    impressions_percent_change, 
    new_to_brand_product_sales_percent_change
from frequency_grouping 
    

```

</details>

##### Sample output

| frequency\_bucket | clicks\_percent\_change | purchases\_percent\_change | unts\_sold\_percent\_change | sales\_percent\_change | atc\_percent\_change | dpv\_percent\_change | impressions\_percent\_change | new\_to\_brand\_product\_sales\_percent\_change |
| ----------------- | ----------------------- | -------------------------- | --------------------------- | ---------------------- | -------------------- | -------------------- | ---------------------------- | ----------------------------------------------- |
| 0                 | \-0\.02239              | 0\.09392                   | 0\.10236                    | 0\.09469               | 0\.10263             | 0\.05155             | 0\.04808                     | 0\.06211                                        |
| 1                 | \-0\.00982              | 0\.04336                   | 0\.04356                    | 0\.04565               | 0\.04371             | 0\.02806             | 0\.04448                     | 0\.02813                                        |
| 2                 | \-0\.00654              | 0\.03660                   | 0\.03629                    | 0\.03849               | 0\.03606             | 0\.01647             | 0\.03535                     | 0\.01889                                        |
| 3                 | 0\.00587                | 0\.02391                   | 0\.02320                    | 0\.02539               | 0\.02407             | 0\.01310             | 0\.07588                     | 0\.00905                                        |
| 4                 | \-0\.00412              | 0\.02170                   | 0\.02053                    | 0\.02175               | 0\.01688             | 0\.00741             | 0\.02669                     | 0\.00841                                        |
| 5                 | \-0\.00516              | 0\.01206                   | 0\.01235                    | 0\.01205               | 0\.00947             | 0\.00344             | 0\.01285                     | 0\.00484                                        |
| 6                 | \-0\.00412              | 0\.00476                   | 0\.00532                    | 0\.00606               | 0\.00408             | \-0\.00016           | 0\.00608                     | 0\.00162                                        |
| 7                 | \-0\.00360              | 0\.00052                   | \-0\.00011                  | 0\.00168               | 0\.00166             | \-0\.00358           | 0\.00585                     | \-0\.00101                                      |
| 8                 | \-0\.00294              | 0\.00071                   | \-0\.00041                  | \-0\.00115             | \-0\.00030           | \-0\.00283           | 0\.00300                     | \-0\.00156                                      |
| 9                 | \-0\.00201              | \-0\.00153                 | \-0\.00087                  | \-0\.00161             | \-0\.00191           | \-0\.00421           | 0\.00207                     | \-0\.00172                                      |
| 10                | \-0\.00084              | \-0\.00162                 | \-0\.00190                  | \-0\.00226             | \-0\.00247           | \-0\.00362           | 0\.00098                     | \-0\.00260                                      |
| 11                | 0\.00037                | \-0\.00206                 | \-0\.00159                  | \-0\.00148             | \-0\.00212           | \-0\.00308           | 0\.00144                     | \-0\.00162                                      |
| 12                | 0\.00055                | \-0\.00293                 | \-0\.00286                  | \-0\.00337             | \-0\.00301           | \-0\.00376           | \-0\.00001                   | \-0\.00017                                      |
| 13                | 0\.00113                | \-0\.00192                 | \-0\.00201                  | \-0\.00297             | \-0\.00326           | \-0\.00337           | 0\.00038                     | \-0\.00110                                      |
| 14                | 0\.00058                | \-0\.00194                 | \-0\.00231                  | \-0\.00208             | \-0\.00278           | \-0\.00317           | \-0\.00043                   | 0\.00010                                        |
| 15                | 0\.00051                | \-0\.00209                 | \-0\.00216                  | \-0\.00196             | \-0\.00315           | \-0\.00285           | \-0\.00064                   | 0\.00124                                        |

Using the methodology above, we can plot the change in percent difference over time to get an aggregated report of the frequency capping point.

##### Example query output visualization

The visualization is for multiple conversion metrics

![Perc_change](/_images/amazon-marketing-cloud/playbooks/optimal_freq_analysis/PB_OFA_3_percent_change_freq_bucket.png)

Here, we can see that the percent change hovers at around zero until around the frequency bucket of 8 to which it then drops down to 9 and stays significantly below. Using this guidance, we can use 8 as our frequency cap and invest any additional budgets to reach new audiences.

In looking at the daily frequency rate we can see that the percent change dips below 0 at the frequency bucket of around 8. So If we were to use this as a guide for setting a frequency cap at the daily level we could go with 8 for the down funnel conversions (purchases, sales, add to cart, detailed page views).

For the upper funnel engagement metrics (clicks), we can see that the profitability in driving engagement increase towards the frequency bucket of 3, and then starts to approach zero at the frequency bucket of 5 and crosses at 6. So, if we were to be focused on driving reach and clicks, we would want to try to set our frequency minimum at 2 and our maximum at 5.

Let's take another example with metrics illustrated below. If we were to optimize frequency towards total sales over the span of a month across a single line_item, our model would recommend we set the frequency cap at 5.

| frequency\_bucket | cost      | cumulativecost | product\_sales | cumulativeproduct\_sales | sales\_percent\_change | cumulative roas |
| ----------------- | --------- | -------------- | -------------- | ------------------------ | ---------------------- | --------------- |
| 0                 | 0\.000    | 0\.000         | 6100\.140      | 6100\.140                | 0\.254                 | \ -        |
| 1                 | 8875\.213 | 8875\.213      | 15399\.200     | 21499\.340               | 0\.085                 | 2\.422          |
| 2                 | 7230\.394 | 16105\.608     | 6643\.590      | 28142\.930               | 0\.031                 | 1\.747          |
| 3                 | 4811\.318 | 20916\.926     | 3185\.340      | 31328\.270               | 0\.024                 | 1\.498          |
| 4                 | 6313\.664 | 27230\.589     | 3392\.810      | 34721\.080               | 0\.006                 | 1\.275          |
| 5                 | 3595\.224 | 30825\.813     | 1538\.020      | 36259\.100               | 0\.004                 | 1\.176          |
| 6                 | 3108\.070 | 33933\.883     | 1285\.560      | 37544\.660               | 0\.000                 | 1\.106          |
| 7                 | 2559\.675 | 36493\.557     | 875\.980       | 38420\.640               | \-0\.002               | 1\.053          |
| 8                 | 2884\.820 | 39378\.377     | 933\.110       | 39353\.750               | \-0\.005               | 0\.999          |
| 9                 | 2420\.816 | 41799\.193     | 620\.630       | 39974\.380               | \-0\.006               | 0\.956          |
| 10                | 2208\.339 | 44007\.532     | 504\.590       | 40478\.970               | \-0\.004               | 0\.920          |

In doing so, you would leave with a cumulative ROAS (cumulatve sales / cumulative costs) of $1.17. If you were to choose a more conservative frequency cap of, say 9 over a span of a month, your cumulative ROAS would drop 20% to $0.95, which would mean your overall spend was greater than your overall sales. So, optimizing your frequency has a significant impact on campaign performance.
![Cumulative_roas](/_images/amazon-marketing-cloud/playbooks/optimal_freq_analysis/PB_OFA_4_cumulative_roas_freq_bucket.png)

### Identify under-served audiences

This approach finds the point in which the advertising return KPI compared to costs will start to increase to a positive with an increased advertising investment towards a higher frequency. This approach here can help users define the right value threshold for identifying audiences that are considered under-served from a frequency standpoint.

In cases like the above example with clicks, you get a pretty clear direction on what should serve as the frequency minimum and frequency cap, but with some of the lower funnel metrics, there are ways to determine the point in which the cumulative cost and KPI relationship is more lucrative by increasing the frequency. To do this, we can utilize python to measure the derivatives of the cumulative percentage to find a point in which the values increase above zero.

To demonstrate, we will look at measuring the cumulative ROAS (cumulative sales / cumulative costs) at each frequency bucket and see if we can plot a curve that helps to identify the frequency minimums.

An example starting point will use some dummy data below:

| frequency\_bucket | purchases | product\_sales | cost     | roas    | cumulative\_sales | cumulative\_cost | cum\_sales\_perc | cum\_cost\_perc | percent\_diff | percentage\_change |
| ----------------- | --------- | -------------- | -------- | ------- | ----------------- | ---------------- | ---------------- | --------------- | ------------- | ------------------ |
| 1                 | 895       | 11754\.29      | 1468\.15 | 8\.0062 | 11754\.29         | 1468\.15         | 32\.36968        | 21\.15466       | 11\.20        | NaN                |
| 2                 | 408       | 5818\.44       | 818\.37  | 7\.1098 | 17572\.73         | 2286\.52         | 48\.39286        | 32\.94659       | 15\.40        | 37\.72828          |
| 3                 | 237       | 3188\.28       | 563\.93  | 5\.6537 | 20761\.01         | 2850\.45         | 57\.17294        | 41\.07229       | 16\.10        | 4\.23651           |
| 4                 | 151       | 2023\.61       | 415\.52  | 4\.8701 | 22784\.62         | 3265\.97         | 62\.74568        | 47\.05954       | 15\.70        | \-2\.57449         |
| 5                 | 121       | 1692\.25       | 331\.79  | 5\.1003 | 24476\.87         | 3597\.76         | 67\.4059         | 51\.84033       | 15\.60        | \-0\.76857         |
| 6                 | 85        | 1479\.59       | 280\.26  | 5\.2794 | 25956\.46         | 3878\.02         | 71\.48049        | 55\.87861       | 15\.60        | 0\.23323           |
| 7                 | 68        | 1082\.16       | 234\.24  | 4\.62   | 27038\.62         | 4112\.26         | 74\.46061        | 59\.25378       | 15\.20        | \-2\.53212         |
| 8                 | 70        | 1129\.79       | 203\.06  | 5\.5639 | 28168\.41         | 4315\.32         | 77\.57189        | 62\.17969       | 15\.40        | 1\.21907           |
| 9                 | 47        | 626\.7         | 183\.31  | 3\.4188 | 28795\.11         | 4498\.63         | 79\.29774        | 64\.82101       | 14\.50        | \-5\.94768         |
| 10                | 65        | 1030\.47       | 472\.08  | 2\.1828 | 29825\.58         | 4970\.71         | 82\.13551        | 71\.62324       | 10\.50        | \-27\.38503        |
| 11                | 53        | 1020\.28       | 276\.46  | 3\.6906 | 30845\.86         | 5247\.17         | 84\.94522        | 75\.60677       | 9\.34         | \-11\.16617        |
| 12                | 49        | 696\.6         | 183\.34  | 3\.7995 | 31542\.46         | 5430\.51         | 86\.86356        | 78\.24852       | 8\.62         | \-7\.74664         |
| 13                | 27        | 453\.66        | 146\.5   | 3\.0966 | 31996\.12         | 5577\.01         | 88\.11288        | 80\.35945       | 7\.75         | \-10\.00123        |

Plotting the frequency cap curve using percent change in Python, we get the following:

`plt.plot(performance_t["frequency_bucket"], performance_t['percentage_change'], 'r-', label='percent_change') plt.legend() plt.rcParams["figure.figsize"] = (18, 5) plt.show()`

![perc_change](/_images/amazon-marketing-cloud/playbooks/optimal_freq_analysis/PB_OFA_5_freq_cap_curve_percent_change.png)

Here, we can see that the percent change hovers at around zero until around the frequency bucket of 8 to which it then drops down to 9 and stays significantly below. Using this guidance, we can use 8 as our frequency cap and invest any additional budgets to reach new audiences.

Plotting ROAS, we can use the same Python function below using ROAS in place of percent change.

```
plt.plot(performance_t["frequency_bucket"], performance_t['roas'], 'r-', label='roas')
plt.legend()
plt.rcParams["figure.figsize"] = (18, 5)
plt.show()
```

![roas_change](/_images/amazon-marketing-cloud/playbooks/optimal_freq_analysis/PB_OFA_6_freq_cap_curve_roas_change.png)

In order to make a recommendation of minimum frequency using diminishing returns, we would need to look into using derivatives to smoothen the line and be able to build a bell curve of optimal frequency.

To do so, we will be fitting a non-linear model on a set of data using python. We will use the [curve_fit function within scipy.optimize](https://gsalvatovallverdu.gitlab.io/python/curve_fit/) in which will take into account the uncertainties on the response that is Y.

The function code will utilize the model below:

![function_code](/_images/amazon-marketing-cloud/playbooks/optimal_freq_analysis/PB_OFA_7_func1.png)

Where X is the frequency bucket, the Y represents the cumulative sales / cumulative cost at each frequency bucket, and A and C will represent the uncertainty principle at each frequency bucket.

```
def f_model(x, a, c):
    return pd.np.log((a + x)**2 / (x - c)**2)
```

Now, we will perform the fit with the curve_fit algorithm using the f_model() function we created and give an initial guess for the parameters (3,2). The curve fit model will follow a least-square approach to minimize the function below:

![function_code](/_images/amazon-marketing-cloud/playbooks/optimal_freq_analysis/PB_OFA_8_func2.png)

For the ydata, you will be measuring cumulative ROAS by way of taking the cumulative sales / cumulative costs.

```
popt, pcov = curve_fit(
    f=f_model,       # model function
    xdata=performance_t["frequency_bucket"],   # x data
    ydata=performance_t["percent_diff"],   # y data
    p0=(3, -2)      # initial value of the parameters
    #sigma=df["Dy"]   # uncertainties on y
)
```

Once you collect the a_opt uncertainties, you will then take the first derivative of Y and X and using the np.gradient() function and then the second derivative of that using the same gradient function to get a bell curve shaped curve for the frequency distribution.

```

a_opt, c_opt = popt
x = np.linspace(0,25,25)
dx = x[1]-x[0]
y = pd.np.log((a_opt + x)**2 / (x - c_opt)**2)
# Take the Derivative of Y and X
dydx = np.gradient(y, x)

# Take the second derivative of Y and X
dydx = np.gradient(dydx, x)
  
Taking the derivative, you can then plot the values across the frequency buckets below:

x = np.linspace(0,25,25)
lines = [-dydx]
colors  = ['g']
labels  = ['Derivative of Percent Diff']

# fig1 = plt.figure()
for i,c,l in zip(lines,colors,labels):  
    plt.plot(x,i,c,label='l')
    plt.legend(labels)  
plt.show()

```

And the output is below:

![derivative_perc_diff](/_images/amazon-marketing-cloud/playbooks/optimal_freq_analysis/PB_OFA_9_derivate_perc_diff.png)

#### Activate underserved audiences

Using this function, we would look at the first point that the second derivative of percent diff crosses the 0 line as the minimum frequency. In this case, we would want to look at using a minimum frequency of 6, along with using the other analysis that would show a maximum frequency of 8.

As an action coming out of this minimum frequency value, we can create audiences who have less than a frequency of 6 and target them more aggressively with advertising. The AMC query below shows you how to create these audiences to activate against.

> [NOTE] Run the following SQL script in the audience query editor

##### SQL

<details class="details-bar">
<summary>Click to see: Audience creation on frequency bucket  </summary>

```
-- Audience Creation on Frequency Bucket --
-- Frequency bucket between 2 and 5
--aggregate by campaign id
/*UPDATE to your campaign id */
WITH campaign_ids (campaign_id_string) AS (
  VALUES
    (111111111111)
),
events AS (
  SELECT
    user_id,
    SUM(impressions) as impressions
  FROM
    dsp_impressions
  WHERE
    campaign_id_string IN (
      SELECT
        campaign_id_string
      FROM
        campaign_ids
    )
  GROUP BY
    1
  UNION ALL

  SELECT 
    user_id, 
    SUM(impressions) as impressions
  FROM sponsored_ads_traffic
  WHERE
    campaign_id_string IN (
      SELECT
        campaign_id_string
      FROM
        campaign_ids
    )
  group by 1
),
conversion_events as (
  SELECT 
      user_id
  FROM amazon_attributed_events_by_traffic_time
),
non_converters AS (
  SELECT 
    a.impression_date,
    a.user_id, 
    sum(a.impressions) as impressions
  FROM events a
  LEFT JOIN conversion_events c 
    on a.user_id = c.user_id 
    and a.impression_date = c.impression_date
  WHERE c.user_id is null
  GROUP BY 1
),
events_by_user AS (
  SELECT
    user_id,
    IF(
      SUM(impressions) > 0,
      SUM(impressions),
      0
    ) AS impressions,
    IF(
      SUM(impressions) > 0, 
      SUM(impressions), 
      0
    ) AS frequency
  FROM
    non_converters
  GROUP BY
    1
),
users_in_bucket as (
  SELECT
    user_id,
    median(frequency) as frequency
  FROM events_by_user
  GROUP BY 1
 )
    SELECT 
    distinct user_id
    FROM users_in_bucket
  WHERE
    user_id IS NOT NULL
    and frequency < 6

```

</details>

From here, you can adjust your bids strategy in Amazon DSP for these audience members to reach them to get them above the minimum.

### Determine optimal frequency point

Now that we have the framework for minimum frequency and maximum frequency that we can action upon, the next option would be to see if we can fully plot the frequency bell curve across each bucket to identify the optimal frequency point.

Using a similar approach as before, we will be using derivatives on the “percent diff” column to create a curve that will help us identify the optimal frequency with respect to the cumulative sales and costs at each bucket.

For this case, we will follow the guide below:

```

def f_model(x, a, c):
    return pd.np.log((a + x)**2 / (x - c)**2)
  
  
popt, pcov = curve_fit(
    f=f_model,       # model function
    xdata=performance_t["frequency_bucket"],   # x data
    ydata=performance_t["percent_diff"],   # y data
    p0=(3, -2)      # initial value of the parameters
    #sigma=df["Dy"]   # uncertainties on y
)

a_opt, c_opt = popt

x = np.linspace(0,25,25)
dx = x[1]-x[0]
y = pd.np.log((a_opt + x)**2 / (x - c_opt)**2)
#take the derivative of Y and X
dydx = np.gradient(y, x)
#take the second derivative of Y and X
dydx = np.gradient(dydx, x)

```

And then plot the results using the following code:

```
x = np.linspace(0,25,25)
lines = [-dydx]
colors  = ['g']
labels  = ['GREEN']

# fig1 = plt.figure()
for i,c,l in zip(lines,colors,labels):  
    plt.plot(x,i,c,label='l')
    plt.legend(labels)  
plt.show()
```

![Freq_bell_curve](/_images/amazon-marketing-cloud/playbooks/optimal_freq_analysis/PB_OFA_10_freq_bell_curve.png)

Here, we can see that the optimal frequency level crosses the zero threshold at around the 6 bucket to 8 bucket with the apex being at around 7. This would be the point in which the cumulative sales are peaking against the cumulative costs.

The action from this result would be to aim to adjust bidding so that you are trying to get as many people as possible to a frequency of 7, reinvesting budget to reach new audiences instead of serving these users additional messaging.

##### SQL

<details class="details-bar">
<summary>Click to see: Audience creation on frequency bucket</summary>

```
-- Audience creation on frequency bucket --
-- Frequency bucket between 2 and 5
--aggregate by campaign id
/*UPDATE to your campaign id */
WITH campaign_ids (campaign_id_string) AS (
  VALUES
    (111111111111)
),
events AS (
  SELECT
    user_id,
    SUM(impressions) as impressions
  FROM
    dsp_impressions
  WHERE
    campaign_id_string IN (
      SELECT
        campaign_id_string
      FROM
        campaign_ids
    )
  GROUP BY
    1
  UNION ALL

  SELECT 
    user_id, 
    SUM(impressions) as impressions
  FROM sponsored_ads_traffic
  WHERE
    campaign_id_string IN (
      SELECT
        campaign_id_string
      FROM
        campaign_ids
    )
  group by 1
),
conversion_events as (
  SELECT 
      user_id
  FROM amazon_attributed_events_by_traffic_time
),
non_converters AS (
  SELECT 
    a.impression_date,
    a.user_id, 
    sum(a.impressions) as impressions
  FROM events a
  LEFT JOIN conversion_events c 
    on a.user_id = c.user_id 
    and a.impression_date = c.impression_date
  WHERE c.user_id is null
  GROUP BY 1
),
events_by_user AS (
  SELECT
    user_id,
    IF(
      SUM(impressions) > 0,
      SUM(impressions),
      0
    ) AS impressions,
    IF(
      SUM(impressions) > 0, 
      SUM(impressions), 
      0
    ) AS frequency
  FROM
    non_converters
  GROUP BY
    1
),
users_in_bucket as (
  SELECT
    user_id,
    median(frequency) as frequency
  FROM events_by_user
  GROUP BY 1
 )
    SELECT 
    distinct user_id
    FROM users_in_bucket
  WHERE
    user_id IS NOT NULL
    and frequency between 5 and 9

```

</details>

The query above will create audiences within Amazon DSP within that time window that can be activated upon to maintain an optimal frequency of 7 for the campaign.

## Automation architecture

The automation component of the playbook will live as an Amazon Quicksight dashboard showing the cumulative totals, percentages, percent differences, and percent changes that can further filtered by campaign and measured over a specified time period.

> [NOTE] We recommend remeasuring the optimal frequency monthly to utilize the latest findings and accordingly minimize your ad budget.

To get started, you will need to define a workflow that can be utilized to visualize the frequency caps to apply to your specific campaign.

To do so, you would need to:

- Establish a workflow call from AMC on a regular cadence that measures the ongoing percent difference to pull into a BI tool of your choice to see how the optimal frequency cap changes update with time.
- Establish a scheduled cadence for re-measuring the frequency cap based on what is feasible for refreshing and reactivating audiences to support.
- Use the mid-campaign audience optimization approach in the [Programmatic Audience Framework playbook](guides/amazon-marketing-cloud/playbooks/programmatic_audience_framework) to maintain this strategy.
