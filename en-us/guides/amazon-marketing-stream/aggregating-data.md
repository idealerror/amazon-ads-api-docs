---
title: Aggregating and joining traffic and conversion records delivered by Amazon Marketing Stream
description: Instructions for deduplicating traffic and conversion records delivered using Amazon Marketing Stream 
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Campaign management
    - Sponsored Products
    - Sponsored Display
keywords:
    - stream traffic
    - stream conversions
    - deduplicate data
    - aggregate data
    - duplicate data
    - identify duplicate records
---

# Aggregating and joining traffic and conversion data delivered by Amazon Marketing Stream

Since Amazon Marketing Stream guarantees *at least once* delivery of messages, some messages can be sent more than once. For this reason, the traffic and conversion datasets use a field called `idempotency_id`. To ensure accurate summation of the delta records in these datasets, the duplicate records must removed prior to aggregation. 

## Order of operations

For the most accurate results, you must complete the steps in this order:

1. Identify duplicates using the `idempotency_id` key.
2. Aggregate and remove duplicates from the traffic table.
3. Aggregate and remove duplicates from the conversion table.
3. Join the traffic and conversion tables.

## Example implementation

The following code sample shows how to perform this order of operations on the sp-traffic and sp-conversion datasets. Note that traffic and conversion datasets from other ad programs may have slightly different schemas. Refer to the [Data guide](guides/amazon-marketing-stream/data-guide) to see the schema for each available dataset.  

>[NOTE] This example uses the `idempotency_id` key using a windowing function called `row_number()` to identify duplicates. [Learn more about windowing functions](https://prestodb.io/docs/current/functions/window.html).

```
-- Identify duplicates in both tables

with sp_conv as 
(
    SELECT *, row_number() OVER (PARTITION BY idempotency_id ORDER BY idempotency_id ASC) AS rn
    FROM [INSERT CONVERSION TABLE HERE]
    
   --Input initial WHERE filter criteria here.
),
sp_traff as
(
    SELECT *, row_number() OVER (PARTITION BY idempotency_id ORDER BY idempotency_id ASC) AS rn
    FROM [INSERT TRAFFIC TABLE HERE]
    
    --Input initial WHERE filter criteria here.
),

-- Aggregate and remove duplicates from sp-conversions

sp_conv_agg as
(
    SELECT    "advertiser_id"
            , "marketplace_id"
            , "time_window_start"
            , "campaign_id"
            , "ad_group_id"
            , "ad_id"
            , "keyword_id"
            , "placement"
            , "currency"
            , sum("attributed_sales_1d") as "attributed_sales_1d"
            , sum("attributed_sales_1d_same_sku") as "attributed_sales_1d_same_sku"
            , sum("attributed_sales_7d") as "attributed_sales_7d"
            , sum("attributed_sales_7d_same_sku") as "attributed_sales_7d_same_sku"
            , sum("attributed_sales_14d") as "attributed_sales_14d"
            , sum("attributed_sales_14d_same_sku") as "attributed_sales_14d_same_sku"
            , sum("attributed_sales_30d") as "attributed_sales_30d"
            , sum("attributed_sales_30d_same_sku") as "attributed_sales_30d_same_sku"
            , sum("attributed_conversions_1d") as "attributed_conversions_1d"
            , sum("attributed_conversions_1d_same_sku") as "attributed_conversions_1d_same_sku"
            , sum("attributed_conversions_7d") as "attributed_conversions_7d"
            , sum("attributed_conversions_14d_same_sku") as "attributed_conversions_14d_same_sku"
            , sum("attributed_conversions_30d") as "attributed_conversions_30d"
            , sum("attributed_conversions_30d_same_sku") as "attributed_conversions_30d_same_sku"
            , sum("attributed_units_ordered_1d") as "attributed_units_ordered_1d"
            , sum("attributed_units_ordered_1d_same_sku") as "attributed_units_ordered_1d_same_sku"
            , sum("attributed_units_ordered_7d") as "attributed_units_ordered_7d"
            , sum("attributed_units_ordered_7d_same_sku") as "attributed_units_ordered_7d_same_sku"
            , sum("attributed_units_ordered_14d") as "attributed_units_ordered_14d"
            , sum("attributed_units_ordered_14d_same_sku") as "attributed_units_ordered_14d_same_sku"
            , sum("attributed_units_ordered_30d") as "attributed_units_ordered_30d"
            , sum("attributed_units_ordered_30d_same_sku") as "attributed_units_ordered_30d_same_sku"
    FROM sp_conv
    
    --Ensures only 1 unique idempotency_id record is used (in the event of duplicates)
    WHERE rn = 1
    
    --GROUP BY all the keys identified above
    GROUP BY 1, 2, 3, 4, 5, 6, 7, 8, 9

),

-- Aggregate and remove duplicates from sp-traffic

sp_traff_agg as
(
    SELECT    "advertiser_id"
            , "marketplace_id"
            , "time_window_start"
            , "campaign_id"
            , "ad_group_id"
            , "ad_id"
            , "keyword_id"
            , "keyword_text" 
            , "placement"
            , "currency"
            , sum("cost") as "cost"
            , sum("impressions") as "impressions"
            , sum("clicks") as "clicks"
    FROM sp_traff
    
    --Ensures only 1 unique idempotency_id record is used (in the event of duplicates)
    WHERE rn = 1
    
    --GROUP BY all the keys identified above
    GROUP BY 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
),

-- Join sp-traffic and sp-conversion

SELECT    spt."advertiser_id"
        , spt."marketplace_id"
        , spt."time_window_start"
        , spt."campaign_id"
        , spt."ad_group_id"
        , spt."ad_id"
        , spt."keyword_id"
        , spt."keyword_text" 
        , spt."placement"
        , spt."currency"
        , spt."cost"
        , spt."impressions"
        , spt."clicks"
        , spc."attributed_sales_1d"
        , spc."attributed_sales_1d_same_sku"
        , spc."attributed_sales_7d"
        , spc."attributed_sales_7d_same_sku"
        , spc."attributed_sales_14d"
        , spc."attributed_sales_14d_same_sku"
        , spc."attributed_sales_30d"
        , spc."attributed_sales_30d_same_sku"
        , spc."attributed_conversions_1d"
        , spc."attributed_conversions_1d_same_sku"
        , spc."attributed_conversions_7d"
        , spc."attributed_conversions_14d_same_sku"
        , spc."attributed_conversions_30d"
        , spc."attributed_conversions_30d_same_sku"
        , spc."attributed_units_ordered_1d"
        , spc."attributed_units_ordered_1d_same_sku"
        , spc."attributed_units_ordered_7d"
        , spc."attributed_units_ordered_7d_same_sku"
        , spc."attributed_units_ordered_14d"
        , spc."attributed_units_ordered_14d_same_sku"
        , spc."attributed_units_ordered_30d"
        , spc."attributed_units_ordered_30d_same_sku"
FROM sp_traff_agg spt
LEFT JOIN sp_conv_agg spc 

--START OF JOIN KEY LOGIC --
ON  spt.advertiser_id   = spc.advertiser_id 
AND spt.marketplace_id  = spc.marketplace_id
AND spt.campaign_id     = spc.campaign_id
AND spt.ad_group_id     = spc.ad_group_id
AND spt.ad_id           = spc.ad_id
AND spt.keyword_id      = spc.keyword_id
AND spt.placement       = spc.placement
AND spt.time_window_start = spc.time_window_start
AND spt.currency        = spc.currency
--END OF JOIN KEY LOGIC --

--Add any additional WHERE filtering criteria here

--Change ORDER clause to desired order
ORDER BY time_window_start
```

>[WARNING] For accurate results, the filter criteria **must match between each level of CTE**. In the above example, `sp_conv` filter criteria should match `sp_traff` filter criteria. Similarly, the two aggregated CTEs (`sp_traffic_agg` and `sp_conv_agg`) must also have the same filtering criteria. In general, you will see better performance if pre-filtering is done higher in the chain of commands. We recommend parameterizing any filters shared between CTEs to reduce the chance of error. 