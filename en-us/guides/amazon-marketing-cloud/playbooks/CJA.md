---
title: Amazon Marketing Cloud - Customer journey analytics
description: AMC Customer journey analytics - Sankey Diagram
type: guide
interface: api
---

# Customer journey analytics

Through this playbook, you will learn how to extract and group your campaigns, campaign categories, or brands within the funnel (through phases such as awareness, consideration, retargeting and so on). You will then process these insights to output visually as a **Sankey diagram**. 

## What is customer journey analytics?

Customer journey analytics is the process of identifying a customer’s interactions with an ad prior to an outcome. Outcomes include, but not limited to, adding to cart, making a purchase, or even cart abandonment. 

For a campaign or a brand, this process helps you identify the series of events in each stage of your funnel that finally led to a particular outcome. 
While the Path-to-Purchase or Path-to-Conversion analysis deliver powerful insights with the chronological sequence of interactions, those insights are challenging to visualize. 

>[NOTE] A **Sankey diagram** is a form of visualization to depict data flow between campaign paths.

### Benefits 
Through the guidance in this playbook, you will:

-	represent the customer journey and subsequently the complicated paths across purchase using an effective visual diagram.
-	manage to identify the campaigns that influenced the buyer in his journey of purchase.
-	manage to identify different conversion event subtypes like ‘add to cart’, ‘customer review’, ‘add to wish list’, and so on.
-	manage to identify different ad product types like Amazon DSP or Sponsored Ads campaigns that influenced the path to purchase of a customer

### Prerequisites 
To effectively use this playbook, we recommend you understand the following concepts:

-	**AMC concepts**: Specifically concepts such as data aggregation thresholds, privacy limitations, and other restrictions listed in the AMC documentation.
-	**Knowledge of AMC SQL**: The process outlined here relies on altering SQL code to match your purposes.
-	**Basic Python**: This playbook is built using Python, and may require some scripting to adapt it to your use case. 
-	**Amazon QuickSight**: You will need to know how to ingest data in Amazon QuickSight to output the required visualizations.
-	**Data-driven decision-making**:  A penchant for turning insights into action with media activations from AMC performance measures. 

### Assumptions
-	Access to  AMC instance
-	At least 7 days of backfilled data 
-	Minimum 4 active campaigns

### Playbook steps
1.	[Run queries](#run-queries). Run the **Path to conversion by campaign** and **Path to conversion by campaign groups** queries on an AMC instance with a desired timeframe. 
2.	[Transform data to input for a Sankey diagram visualization.](#transform-data-to-input-for-a-sankey-diagram-visualization)
3.	[Visualize and interpret results.](#visualize-and-interpret-results) 

## Run queries

### Query to generate path to conversion by campaign group 

The query will return the brand, path and relevant KPIs associated with each path. Understanding your customers’ buying journey can help you improve your campaign performance. To better understand the customer journey, you’ll need to look at the conversion paths your customers took. 
This instructional query will enable you to identify the most popular paths your customers took before purchasing your products on Amazon. The query considers all touch points and their order in the 'path' column of the final output. However please note that paths in the final output are based on the timing of the first impression of the campaign group for simplicity. (see illustration below)

#### Tables used
* dsp\_impressions
* sponsored\_ads\_traffic
* amazon\_attributed\_events\_by\_traffic\_time

The queries below are leveraged from the AMC IQ Library (Path to conversion by campaign group). 


<details class="details-bar">
<summary>Click to see: Instructional Query: Path to conversion by campaign group</summary>

```
-- Instructional Query: Path to conversion by campaign groups
-- Please follow through the instructions in comments to customize this insight
/* Campaign Group Start */
/*
 Instructions:
 1. If no grouping is required, ignore the section below.
 Note: Default grouping will be set if no groupings are provided. DSP campaigns will be tagged as ""DSP-Others"", 
 SP campaigns will be tagged as ""SP-Others"" and SD campaigns will be tagged as ""SD-Others"" and SB campaigns will be tagged as ""SB-Others
 2. To apply grouping:
 Add the grouping for campaigns below. 
 For DSP, add campaign_id and group. Remove section if not required.
 For SP, add campaign name and group. Remove section if not required.
 For SD, add campaign name and group. Remove section if not required.
 DSP campaigns with no groupings will be tagged as ""DSP-Others"", SP campaigns will be tagged as ""SP-Others"" 
 and  SD campaigns will be tagged as ""SD-Others"" and SB campaigns will be tagged as ""SB-Others""
 */

/* Filters end */
/* Query start */
with impressions AS (
  SELECT
  campaign,
    'DSP' AS product_type,
    user_id,
    MIN(impression_dt) AS impression_dt_first,
    SUM(impressions) AS impressions,
    SUM(total_cost) AS total_cost
  FROM
    dsp_impressions i --INNER JOIN dsp_campaigns dsp on dsp.campaign_id = i.campaign_id /*[1 OF 5]: Uncomment this line to apply campaign filters. Keep this line commented to skip filters*/
  --  LEFT JOIN campaign_group G ON g.campaign_id_or_name = i.campaign_id
  WHERE
    user_id IS NOT NULL and advertiser = 'ABC'
  GROUP BY
    1,
    2,
    3 
    
  -- Sponsored Products Section START
  UNION ALL
  SELECT
    --COALESCE(campaign_group, 'SP-Others') AS campaign_group,
    campaign,
    'SP' AS product_type,
    user_id,
    MIN(event_dt) AS impression_dt_first,
    SUM(impressions) AS impressions,
    SUM(spend) AS total_cost
  FROM
    sponsored_ads_traffic a --INNER JOIN sp_campaigns sp on sp.campaign = a.campaign 
    /*[2 OF 5]: Uncomment this line to apply campaign filters. Keep this line commented to skip filters*/
    --LEFT JOIN campaign_group G ON g.campaign_id_or_name = a.campaign
  WHERE
    user_id IS NOT NULL
    AND ad_product_type = 'sponsored_products' and advertiser = 'ABC'
  GROUP BY
    1,
    2,
    3 
   
   -- Sponsored Products Section END

  -- Sponsored Display Section START
  UNION ALL
  SELECT
    --COALESCE(campaign_group, 'SD-Others') AS campaign_group,
    campaign,
    'SD' AS product_type,
    user_id,
    MIN(event_dt) AS impression_dt_first,
    SUM(impressions) AS impressions,
    SUM(spend) AS total_cost
  FROM
    sponsored_ads_traffic a --INNER JOIN sd_campaigns sd on sd.campaign = a.campaign 
    /*[3 OF 5]: Uncomment this line to apply campaign filters. Keep this line commented to skip filters*/
  --  LEFT JOIN campaign_group G ON g.campaign_id_or_name = a.campaign
  WHERE
    user_id IS NOT NULL and advertiser = 'ABC'
    AND ad_product_type = 'sponsored_display'
  GROUP BY
    1,
    2,
    3 
    
  -- Sponsored Display Section END
  -- Sponsored Brands Section START
  UNION ALL
  SELECT
    --COALESCE(campaign_group, 'SB-Others') AS campaign_group,
    campaign,
    'SB' AS product_type,
    user_id,
    MIN(event_dt) AS impression_dt_first,
    SUM(impressions) AS impressions,
    SUM(spend) AS total_cost
  FROM
    sponsored_ads_traffic a --INNER JOIN sb_campaigns sb on sb.campaign = a.campaign 
    /*[3 OF 5]: Uncomment this line to apply campaign filters. Keep this line commented to skip filters*/
   -- LEFT JOIN campaign_group G ON g.campaign_id_or_name = a.campaign
  WHERE
    user_id IS NOT NULL
    AND ad_product_type = 'sponsored_brands' and advertiser = 'ABC'
  GROUP BY
    1,
    2,
    3 -- Sponsored Brands Section END
),
-- gather conversions --
converted AS (
  SELECT
    
    user_id,
    MAX(conversion_event_dt) AS conversion_event_dt_last,
    -- Sales metrics
    SUM(
      IF(
        ad_product_type IS NULL,
        product_sales,
        total_product_sales
      )
    ) AS product_sales,
    -- replace product_sales with total_product_sales to include brand halo sales from DSP
    SUM(
      IF(ad_product_type IS NULL, purchases, total_purchases)
    ) AS purchases,
    -- replace purchases with total_purchases to include brand halo purchases from DSP
    -- NTB sales metrics
    MAX(IF(new_to_brand = TRUE, 1, 0)) AS ntb_cust,
    SUM(
      IF(
        ad_product_type IS NULL,
        new_to_brand_product_sales,
        new_to_brand_total_product_sales
      )
    ) AS ntb_product_sales,
    -- replace new_to_brand_product_sales with new_to_brand_total_product_sales to include brand halo sales from DSP
    SUM(
      IF(
        ad_product_type IS NULL,
        new_to_brand_purchases,
        new_to_brand_total_purchases
      )
    ) AS ntb_purchases -- replace new_to_brand_purchases with new_to_brand_total_purchases to include brand halo purchases from DSP
  FROM
    amazon_attributed_events_by_traffic_time
  WHERE
    IF(ad_product_type IS NULL, purchases, total_purchases) > 0 -- replace purchases with total_purchases to include brand halo conversions from DSP
    AND user_id IS NOT NULL and advertiser = 'ABC' 
    --AND (campaign_id IN ( Select campaign_id from dsp_campaigns) OR campaign IN (Select campaign from sp_campaigns) OR campaign IN (Select campaign from sd_campaigns) OR campaign IN (Select campaign from sb_campaigns))  /*[4 OF 5]: Uncomment this line to apply campaign filters. Keep this line commented to skip filters*/
    --AND tracked_asin IN (Select ASIN from tracked_asins) /*[5 OF 5]: Uncomment this line to apply ASIN filters. Keep this line commented to skip filters*/
  GROUP BY
    1
),
-- only include impressions that happened before conversion event time --
filter_impressions AS (
  SELECT
    i.user_id AS imp_user_id,
    c.user_id AS pur_user_id,
   i.campaign,
    i.impressions,
    i.impression_dt_first,
    -- DSP cost is reported in millicents. To calculate the cost in dollars/your currency, divide the cost value by 100,000 (1e5). Sponsored Ads spend is reported as microcents. Divide by 100,000,000 (1e8) to get the cost in dollars/your currency.
    IF(
      i.product_type = 'DSP',
      (i.total_cost / 1e5),
      (i.total_cost / 1e8)
    ) AS total_cost,
    c.conversion_event_dt_last,
    COALESCE(c.product_sales, 0) AS product_sales,
    COALESCE(c.purchases, 0) AS purchases,
    COALESCE(c.ntb_cust, 0) AS ntb_cust,
    COALESCE(c.ntb_purchases, 0) AS ntb_purchases,
    COALESCE(c.ntb_product_sales, 0) AS ntb_product_sales
  FROM
    impressions i
    LEFT JOIN converted c ON c.user_id = i.user_id
  WHERE
    (
      c.user_id IS NOT NULL
      AND i.impression_dt_first < c.conversion_event_dt_last
    )
    OR c.user_id IS NULL
),
-- order campaigns based on impression event time--
ranked AS (
  SELECT
    NAMED_ROW(
      'order',
      ROW_NUMBER() OVER (
        PARTITION BY f.imp_user_id
        ORDER BY
          f.impression_dt_first
      ),
      'campaign_group',
    f.campaign
   ) AS campaign_order,
    imp_user_id
  FROM
    filter_impressions f
  WHERE
    f.imp_user_id IS NOT NULL
),
-- create campaign path group by user --
assembled AS (
  SELECT
    ARRAY_SORT(COLLECT(DISTINCT a.campaign_order)) AS path,
    a.imp_user_id
  FROM
    ranked a
  GROUP BY
    a.imp_user_id
),
-- dedupe table conversions to represent each row as each user --
filter_impressions_dedupe AS (
  SELECT
    imp_user_id AS imp_user_id,
    SUM(impressions) AS impressions,
    SUM(total_cost) AS total_cost,
    MAX(IF(pur_user_id IS NOT NULL, 1, 0)) AS converted,
    MAX(product_sales) AS product_sales,
    MAX(purchases) AS purchases,
    MAX(ntb_cust) AS ntb_cust,
    MAX(ntb_purchases) AS ntb_purchases,
    MAX(ntb_product_sales) AS ntb_product_sales
  FROM
    filter_impressions
  GROUP BY
    imp_user_id
),
-- assemble impressions and conversions --
assembled_with_imp_conv AS (
  SELECT
    path AS path,
    COUNT(DISTINCT a.imp_user_id) AS reach,
    SUM(b.impressions) AS impressions,
    SUM(b.total_cost) AS imp_total_cost,
    SUM(b.converted) AS users_that_purchased,
    SUM(b.product_sales) AS sales_amount,
    SUM(b.purchases) AS purchases,
    SUM(b.ntb_cust) AS ntb_users_that_purchased,
    SUM(b.ntb_product_sales) AS ntb_sales_amount,
    SUM(b.ntb_purchases) AS ntb_purchases
  FROM
    assembled a
    LEFT JOIN filter_impressions_dedupe b ON a.imp_user_id = b.imp_user_id
  GROUP BY
    path
) -- FINAL SELECT
SELECT
  path,
  reach AS path_occurrences,
  impressions AS impressions,
  imp_total_cost AS imp_total_cost,
  users_that_purchased AS users_that_purchased,
  sales_amount AS sales_amount,
  purchases AS purchases,
  (users_that_purchased / reach) AS user_purchase_rate,
  ntb_users_that_purchased AS ntb_users_that_purchased,
  ntb_sales_amount AS ntb_sales_amount,
  ntb_purchases AS ntb_purchases,
  (ntb_users_that_purchased / users_that_purchased) AS ntb_percentage
FROM
  assembled_with_imp_conv
  /* Query End */

```

</details>

### Sample output 

path          | path\_occurrences | impressions | imp\_total\_cost | users\_that\_purchased | sales\_amount | purchases | ntb\_users\_that\_purchased | ntb\_sales\_amount | ntb\_purchases | ntb\_percentage 
---|---|---|---|---|---|---|---|---|---|---
 \[\[1, Campaign A\]\] | 7305511 | 10817374 | 12737\.4718 | 39 | 696\.48 | 40 | 24 | 434\.5 | 25 | 62% 
 \[\[1, Campaign A\], \[2, Campaign B\]\] | 1280584 | 7017620 | 6295\.70186 | 5 | 78\.31 | 5 | 2 | 20\.78 | 2 | 40% 
 \[\[1, Campaign A\], \[2, Campaign C\], \[3, Campaign D\]\] | 885739 | 2933304 | 12971\.1453 | 126 | 1615\.3 | 128 | 87 | 1050\.51 | 87 | 69% 
 \[\[1, Campaign B\], \[2, Campaign C\], \[3, Campaign D\]\] | 859763 | 3260511 | 3483\.31 | 587 | 6914\.14 | 603 | 493 | 5531\.21 | 496 | 84% 
 \[\[1, Campaign B\], \[2, Campaign C\], \[3, Campaign D\],\[4, Campaign F\]\] | 386559 | 633547 | 99\.06 | 60 | 867\.56 | 65 | 43 | 609\.01 | 44 | 72% 
 \[\[1, Campaign B\], \[2, Campaign C\], \[3, Campaign E\], \[4, Campaign F\]\] | 373620 | 795947 | 3574\.52074 | 27 | 433\.97 | 28 | 23 | 346\.77 | 24 | 85% 


### Query to generate path to conversion by campaign category 

This query is same as the above query but there is an additional campaign category added based on the goal of the campaign. This is dependent per advertiser and the campaign categories have to be modified depending on campaign goals and settings.

#### Tables used
* dsp\_impressions
* sponsored\_ads\_traffic
* amazon\_attributed\_events\_by\_traffic\_time

<details class="details-bar">
<summary>Click to see: Instructional Query: Path to conversion by campaign category</summary>
```
-- Instructional Query: Path to conversion by campaign category
-- Please follow through the instructions in comments to customize this insight
/* Campaign Group Start */
/*
 Instructions:
 1. If no grouping is required, ignore the section below.
 Note: Default grouping will be set if no groupings are provided. DSP campaigns will be tagged as "DSP-Others", SP campaigns will be tagged as "SP-Others" and SD campaigns will be tagged as "SD-Others" and SB campaigns will be tagged as "SB-Others
 2. To Apply grouping:
 Add the grouping for campaigns below.
 For DSP, add campaign_id and group. Remove section if not required.
 For SP, add campaign name and group. Remove section if not required.
 For SD, add campaign name and group. Remove section if not required.
 DSP campaigns with no groupings will be tagged as "DSP-Others", SP campaigns will be tagged as "SP-Others" and  SD campaigns will be tagged as "SD-Others" and SB campaigns will be tagged as "SB-Others"
 3. Based on the criteria of the campaign create a case statement to identify the campaign category for ex: Awareness, Consideration , Retargeting etc.
 */
/* Filters End */
/* Query Start */
WITH impressions AS (
  SELECT
    --Case statement should be modified based on campaign grouping per advertiser
    CASE
      WHEN campaign SIMILAR TO '(?i)Consideration' THEN 'Consideration'
      WHEN campaign SIMILAR TO '(?i)Conversion' THEN 'Conversion'
      WHEN campaign SIMILAR TO '(?i)SBV' THEN 'Brand Building'
      WHEN campaign SIMILAR TO '(?i)SBA' THEN 'Preference'
      WHEN campaign SIMILAR TO '(?i)UAP' THEN 'Awareness'
      ELSE 'Loyalty'
    END AS campaign_group,
    'DSP' AS product_type,
    user_id,
    MIN(impression_dt) AS impression_dt_first,
    SUM(impressions) AS impressions,
    SUM(total_cost) AS total_cost
  FROM
    dsp_impressions i --INNER JOIN dsp_campaigns dsp on dsp.campaign_id = i.campaign_id 
    /*[1 OF 5]: Uncomment this line to apply campaign filters. Keep this line commented to skip filters*/
    --  LEFT JOIN campaign_group G ON g.campaign_id_or_name = i.campaign_id
  WHERE
    user_id IS NOT NULL
    AND advertiser = 'ABC'
  GROUP BY
    1,
    2,
    3 -- Sponsored Products Section START
  UNION ALL
  SELECT
    --Case statement should be modified based on campaign grouping per advertiser
    CASE
      WHEN campaign SIMILAR TO '(?i)Consideration' THEN 'Consideration'
      WHEN campaign SIMILAR TO '(?i)Conversion' THEN 'Conversion'
      WHEN campaign SIMILAR TO '(?i)SBV' THEN 'Brand Building'
      WHEN campaign SIMILAR TO '(?i)SBA' THEN 'Preference'
      WHEN campaign SIMILAR TO '(?i)UAP' THEN 'Awareness'
      ELSE 'Loyalty'
    END AS campaign_group,
    'SP' AS product_type,
    user_id,
    MIN(event_dt) AS impression_dt_first,
    SUM(impressions) AS impressions,
    SUM(spend) AS total_cost
  FROM
    sponsored_ads_traffic a --INNER JOIN sp_campaigns sp on sp.campaign = a.campaign /*[2 OF 5]: Uncomment this line to apply campaign filters. Keep this line commented to skip filters*/
    --LEFT JOIN campaign_group G ON g.campaign_id_or_name = a.campaign
  WHERE
    user_id IS NOT NULL
    AND ad_product_type = 'sponsored_products'
    AND advertiser = 'ABC'
  GROUP BY
    1,
    2,
    3 -- Sponsored Products Section END
    -- Sponsored Display Section START
  UNION ALL
  SELECT
    --COALESCE(campaign_group, 'SD-Others') AS campaign_group,
    CASE
      WHEN campaign SIMILAR TO '(?i)Consideration' THEN 'Consideration'
      WHEN campaign SIMILAR TO '(?i)Conversion' THEN 'Conversion'
      WHEN campaign SIMILAR TO '(?i)SBV' THEN 'Brand Building'
      WHEN campaign SIMILAR TO '(?i)SBA' THEN 'Preference'
      WHEN campaign SIMILAR TO '(?i)UAP' THEN 'Awareness'
      ELSE 'Loyalty'
    END AS campaign_group,
    'SD' AS product_type,
    user_id,
    MIN(event_dt) AS impression_dt_first,
    SUM(impressions) AS impressions,
    SUM(spend) AS total_cost
  FROM
    sponsored_ads_traffic a --INNER JOIN sd_campaigns sd on sd.campaign = a.campaign /*[3 OF 5]: Uncomment this line to apply campaign filters. Keep this line commented to skip filters*/
    --  LEFT JOIN campaign_group G ON g.campaign_id_or_name = a.campaign
  WHERE
    user_id IS NOT NULL
    AND advertiser = 'ABC'
    AND ad_product_type = 'sponsored_display'
  GROUP BY
    1,
    2,
    3 -- Sponsored Display Section END
    -- Sponsored Brands Section START
  UNION ALL
  SELECT
    --COALESCE(campaign_group, 'SB-Others') AS campaign_group,
    CASE
      WHEN campaign SIMILAR TO '(?i)Consideration' THEN 'Consideration'
      WHEN campaign SIMILAR TO '(?i)Conversion' THEN 'Conversion'
      WHEN campaign SIMILAR TO '(?i)SBV' THEN 'Brand Building'
      WHEN campaign SIMILAR TO '(?i)SBA' THEN 'Preference'
      WHEN campaign SIMILAR TO '(?i)UAP' THEN 'Awareness'
      ELSE 'Loyalty'
    END AS campaign_group,
    'SB' AS product_type,
    user_id,
    MIN(event_dt) AS impression_dt_first,
    SUM(impressions) AS impressions,
    SUM(spend) AS total_cost
  FROM
    sponsored_ads_traffic a --INNER JOIN sb_campaigns sb on sb.campaign = a.campaign /*[3 OF 5]: Uncomment this line to apply campaign filters. Keep this line commented to skip filters*/
    -- LEFT JOIN campaign_group G ON g.campaign_id_or_name = a.campaign
  WHERE
    user_id IS NOT NULL
    AND ad_product_type = 'sponsored_brands'
    AND advertiser = 'ABC'
  GROUP BY
    1,
    2,
    3 -- Sponsored Brands Section END
),
-- gather conversions --
converted AS (
  SELECT
    user_id,
    MAX(conversion_event_dt) AS conversion_event_dt_last,
    -- Sales metrics
    SUM(
      IF(
        ad_product_type IS NULL,
        product_sales,
        total_product_sales
      )
    ) AS product_sales,
    -- replace product_sales with total_product_sales to include brand halo sales from DSP
    SUM(
      IF(ad_product_type IS NULL, purchases, total_purchases)
    ) AS purchases,
    -- replace purchases with total_purchases to include brand halo purchases from DSP
    -- NTB sales metrics
    MAX(IF(new_to_brand = TRUE, 1, 0)) AS ntb_cust,
    SUM(
      IF(
        ad_product_type IS NULL,
        new_to_brand_product_sales,
        new_to_brand_total_product_sales
      )
    ) AS ntb_product_sales,
    -- replace new_to_brand_product_sales with new_to_brand_total_product_sales to include brand halo sales from DSP
    SUM(
      IF(
        ad_product_type IS NULL,
        new_to_brand_purchases,
        new_to_brand_total_purchases
      )
    ) AS ntb_purchases -- replace new_to_brand_purchases with new_to_brand_total_purchases to include brand halo purchases from DSP
  FROM
    amazon_attributed_events_by_traffic_time
  WHERE
    IF(ad_product_type IS NULL, purchases, total_purchases) > 0 -- replace purchases with total_purchases to include brand halo conversions from DSP
    AND user_id IS NOT NULL
    AND advertiser = 'ABC' --AND (campaign_id IN ( Select campaign_id from dsp_campaigns) OR campaign IN (Select campaign from sp_campaigns) OR campaign IN (Select campaign from sd_campaigns) OR campaign IN (Select campaign from sb_campaigns))  /*[4 OF 5]: Uncomment this line to apply campaign filters. Keep this line commented to skip filters*/
    --AND tracked_asin IN (Select ASIN from tracked_asins) /*[5 OF 5]: Uncomment this line to apply ASIN filters. Keep this line commented to skip filters*/
  GROUP BY
    1
),
-- only include impressions that happened before conversion event time --
filter_impressions AS (
  SELECT
    i.user_id AS imp_user_id,
    c.user_id AS pur_user_id,
    i.campaign_group,
    i.impressions,
    i.impression_dt_first,
    -- DSP cost is reported in millicents. To calculate the cost in dollars/your currency, divide the cost value by 100,000 (1e5). Sponsored Ads spend is reported as microcents. Divide by 100,000,000 (1e8) to get the cost in dollars/your currency.
    IF(
      i.product_type = 'DSP',
      (i.total_cost / 1e5),
      (i.total_cost / 1e8)
    ) AS total_cost,
    c.conversion_event_dt_last,
    COALESCE(c.product_sales, 0) AS product_sales,
    COALESCE(c.purchases, 0) AS purchases,
    COALESCE(c.ntb_cust, 0) AS ntb_cust,
    COALESCE(c.ntb_purchases, 0) AS ntb_purchases,
    COALESCE(c.ntb_product_sales, 0) AS ntb_product_sales
  FROM
    impressions i
    LEFT JOIN converted c ON c.user_id = i.user_id
  WHERE
    (
      c.user_id IS NOT NULL
      AND i.impression_dt_first < c.conversion_event_dt_last
    )
    OR c.user_id IS NULL
),
-- order campaigns based on impression event time--
ranked AS (
  SELECT
    named_row(
      'order',
      ROW_NUMBER() OVER (
        PARTITION BY f.imp_user_id
        ORDER BY
          f.impression_dt_first
      ),
      'campaign_group',
      f.campaign_group
    ) AS campaign_order,
    imp_user_id
  FROM
    filter_impressions f
  WHERE
    f.imp_user_id IS NOT NULL
),
-- create campaign path group by user --
assembled AS (
  SELECT
    array_sort(COLLECT(DISTINCT a.campaign_order)) AS path,
    a.imp_user_id
  FROM
    ranked a
  GROUP BY
    a.imp_user_id
),
-- dedupe table conversions to represent each row as each user --
filter_impressions_dedupe AS (
  SELECT
    imp_user_id AS imp_user_id,
    SUM(impressions) AS impressions,
    SUM(total_cost) AS total_cost,
    MAX(IF(pur_user_id IS NOT NULL, 1, 0)) AS converted,
    MAX(product_sales) AS product_sales,
    MAX(purchases) AS purchases,
    MAX(ntb_cust) AS ntb_cust,
    MAX(ntb_purchases) AS ntb_purchases,
    MAX(ntb_product_sales) AS ntb_product_sales
  FROM
    filter_impressions
  GROUP BY
    imp_user_id
),
-- assemble impressions and conversions --
assembled_with_imp_conv AS (
  SELECT
    path AS path,
    COUNT(DISTINCT a.imp_user_id) AS reach,
    SUM(b.impressions) AS impressions,
    SUM(b.total_cost) AS imp_total_cost,
    SUM(b.converted) AS users_that_purchased,
    SUM(b.product_sales) AS sales_amount,
    SUM(b.purchases) AS purchases,
    SUM(b.ntb_cust) AS ntb_users_that_purchased,
    SUM(b.ntb_product_sales) AS ntb_sales_amount,
    SUM(b.ntb_purchases) AS ntb_purchases
  FROM
    assembled a
    LEFT JOIN filter_impressions_dedupe b ON a.imp_user_id = b.imp_user_id
  GROUP BY
    path
) -- FINAL SELECT
SELECT
  path,
  reach AS path_occurrences,
  impressions AS impressions,
  imp_total_cost AS imp_total_cost,
  users_that_purchased AS users_that_purchased,
  sales_amount AS sales_amount,
  purchases AS purchases,
  (users_that_purchased / reach) AS user_purchase_rate,
  ntb_users_that_purchased AS ntb_users_that_purchased,
  ntb_sales_amount AS ntb_sales_amount,
  ntb_purchases AS ntb_purchases,
  (ntb_users_that_purchased / users_that_purchased) AS ntb_percentage
FROM
  assembled_with_imp_conv
  /* Query End */
```
</details>

### Sample output

 path | path\_occurrences | impressions | imp\_total\_cost | users\_that\_purchased | sales\_amount | purchases | user\_purchase\_rate | ntb\_users\_that\_purchased | ntb\_sales\_amount | ntb\_purchases | ntb\_percentage 
---|---|---|---|---|---|---|---|---|---|---|---
 \[\[1, Loyalty\], \[2, Awareness\], \[3, Loyalty\]\] | 445100 | 289549 | $1,176\.16 | 251 | $8,068\.75 | 346 | 1\.13% | 190 | 2980\.54 | 197 | 76% 
 \[\[1, Loyalty\], \[2, Awareness\], \[3, Conversion\]\] | 381560 | 1129427 | $12,631\.80 | 1145 | $21,144\.82 | 1399 | 6% | 605 | 9042\.52 | 630 | 53% 
 \[\[1, Loyalty\], \[2, Awareness\], \[3, Consideration\], \[4, Conversion\]\] | 342340 | 1532544 | $14,796\.18 | 776 | $15,163\.60 | 1015 | 4\.53% | 428 | 6650\.7 | 463 | 55% 
 \[\[1, Loyalty\], \[2, Brand Building\]\] | 318680 | 341519 | $4,399\.20 | 512 | $7,604\.83 | 555 | 3\.21% | 381 | 5266\.38 | 393 | 74% 
 \[\[1, Loyalty\], \[2, Awareness\], \[3, Conversion\], \[4, Consideration\]\] | 300240 | 1294418 | $13,837\.36 | 530 | $9,967\.80 | 653 | 3\.53% | 309 | 4675\.43 | 322 | 58% 
 \[\[1, Awareness\], \[2, Loyalty\], \[3, Conversion\]\] | 211280 | 500741 | $7,852\.80 | 499 | $8,387\.41 | 565 | 4\.72% | 266 | 3987\.03 | 274 | 53% 


## Transform data to input for a Sankey diagram visualization

After you run the above queries, download the query results to process it to a format that can be consumed by Amazon QuickSight to visualize it on a Sankey diagram. Sankey diagram is a visual type within Amazon QuickSight and requires a one dimension in source and one dimension in destination. This visual type is available within other BI tools and the data transformation can be modified accordingly.

See [Preparing data for Sankey diagrams](https://docs.aws.amazon.com/quicksight/latest/user/sankey-diagram.html#sankey-diagram-prepare) to learn how to process your query results before you upload it to Amazon QuickSight. 

Below is an example of how the data is prepared by exploding the path into a series of Source-Destination steps with each step in a new row. 

For example, a single-row 4-element path that looks like [campaign-X, campaign-Z, Campaign-Q, campaign-Y]

 Path | impressions | reach | conversions 
---|---|---|---
 \[campaign\-X, campaign\-Z, campaign\-Q, campaign\-Y\] | 100 | 50 | 5 


will “explode into” 3 separate steps/rows:

 path\-step\-source | path\-step\-destination | impressions | reach | conversions 
---|---|---|---|---
 campaign\-X | campaign\-Z | 100 | 50 | 5 
 campaign\-Z | campaign\-Q | 100 | 50 | 5 
 campaign\-Q | campaign\-Y | 100 | 50 | 5 


### Parse the path to source and destination. 

The script below will update the path and explode it into a file consumable by the Sankey diagram visual type  within Amazon Quicksight. This Python script takes the output file from the Query path to conversion by campaign categories as an input, uses regex to clean and transform the data and custom function explode the data into source and destination columns.
Usually, for a Sankey a source and destination is required and this python script can be modified accordingly depending on what other BI platforms require.

#### Importing libraries
 
```
# Importing Libraries
import pandas as pd
import re
import boto3
import json
from datetime import datetime
from io import StringIO 
from urllib.parse import unquote_plus

import warnings
warnings.filterwarnings('ignore')

```

#### Data Functions 

```
#Functions
def clean_alt_list(list_):
    list_ = list_[1:-1]
    list_ = re.sub(r'(\d)\,',r'\1**',list_)
    list_ = list_.split(', ')
    list_ = [x.replace('**', ', ') for x in list_]
    return list_

def explode_into_source_dest(origin_df, list_col='path_new', 
                             other_dim_col=['brand','path','tracked_asin','attribution_period_days','time_window_start']):
    '''
    Inputs:
    ------
     * origin_df: inputpandas dataframe
     * list_col: the path column containing your list of touchpoints
     * other_dim_col: the other dimension columns used to join back to original dataset (Can by str or lst)
    
    Outputs:
    -------
     * New dataframe
    '''
    
    columns = ['src', 'dest', list_col]
    if isinstance(other_dim_col,list):
        other_dim_col_str = ', '.join(other_dim_col)
        columns.extend(other_dim_col)
    else: 
        columns._append(other_dim_col)
        
    new_df = pd.DataFrame(columns = columns)
    
    for x, row in origin_df.iterrows():
        temp_df = pd.DataFrame(columns = columns) 
        for i, j in enumerate(row[list_col][:-1]):
            if isinstance(other_dim_col,list):
                temp_row = [j, row[list_col][i+1], row[list_col]]
                temp_row.extend([row[a] for a in other_dim_col])
                temp_df = temp_df._append(pd.DataFrame([temp_row],columns=columns))
            else:
                temp_df = temp_df._append(pd.DataFrame([[j, row[list_col][i+1], row[list_col], row[other_dim_col]]],
                                                      columns=columns))
        new_df = new_df._append(temp_df,ignore_index = True)
    
    return new_df       

```

#### Data Exploration

```
# Exploratory Data Analysis
df= pd.read_csv('QueryOutputfile.csv', sep=',')
display(df.head())
```


#### Data Wrangling

```
# Data Pre-Processing
# This is an example of a file using tracked_asin, you can modify the data preprocessing accordingly.
df_clean =df.dropna(subset=["tracked_asin"])
df_clean.head()
```


#### Data Processing

```
df_clean['rownum'] = df_clean.sort_values(['path_occurrences'], ascending=False)\
                                         .groupby(['Brand', 'attribution_period_days'])\
                                         .cumcount() + 1

df_top = df_clean[df_clean['rownum']<=50].sort_values(['Brand', 'rownum']).reset_index(drop=True)
df_top["path_new"] = df_top["path"].apply(clean_alt_list)

df_exploded = explode_into_source_dest(df_top, list_col='path_new', other_dim_col = ['Brand','tracked_asin','path','attribution_period_days'])
df_final = pd.merge(df_top, df_exploded, how='inner', on=['Brand','tracked_asin','path', 'attribution_period_days'])
```

```
# Rearranging the Columns
final_cols = [
'src',
'dest',
'path',
'path_occurrences',
'Brand',
'tracked_asin',
'impressions',
'imp_total_cost',
'users_that_purchased',
'sales_amount',
'purchases',
'user_purchase_rate',
'ntb_users_that_purchased',
'ntb_sales_amount',
'ntb_purchases',
'ntb_percentage']

df_final = df_final[final_cols]
```

```
# Previewing the Final Result before Exporting
csv_buffer = StringIO()
df_final.to_csv(csv_buffer, index=False)
display(df_final.head())

```



#### Exporting to CSV

```
# Exporting to CSV
df_final.to_csv('Sankey-Diagram_Input.csv', index=False)

```

## Visualize and interpret results


### Sample Sankey diagram

![customerjourney_sankey](/_images/amazon-marketing-cloud/playbooks/CJA_01.png)

### Interpret results 
The above Sankey diagram showcases the top 20 paths by purchase. Based on the visual you can identify that high number of purchasers were usually exposed to loyalty and consideration campaign categories. Also, overlap of users who were exposed to Awareness campaigns with other campaign categories is comparatively lower.

With this, advertisers can identify different attributes per campaign category like impression frequency, cost , other line item details for loyalty and consideration campaigns and use similar attributes to design campaigns for brand building ,conversion or other categories. Additionally, based on the product category advertisers can target audiences similar to those that have had a high overlap in this case loyalty and consideration.

The above Sankey is a representation for path to purchase by campaign categories, this can be replaced with campaigns, campaign groups based on ad product types like Amazon demand-side platform, Sponsored Brands, Sponsored Display and Sponsored Products and many more variations.
