---
title: Overview
description: An overview of the AMC data sources
type: guide
interface: api
---
# Amazon Marketing Cloud data sources

Data from your Amazon Ads campaign events are ingested into AMC. This data could be from ad activity on Amazon DSP or any of the Sponsored products, and can be referenced through the following tables:

- [Amazon DSP traffic](#amazon-dsp-traffic-tables)
- [Amazon Ads conversions](#amazon-ads-conversion-tables)
- [Sponsored ads](#sponsored-ads-table)
- [AMC Paid features](#amc-paid-features)
- [AMC audiences tables](#amc-audiences-tables)


All tables have a flat schema consisting of a list of columns that each have a name, a data type, a column type of metric or dimension, and an aggregation threshold. For details, see [AMC data aggregation thresholds](guides/amazon-marketing-cloud/aggregation-threshold).

Creating or modifying materialized tables is not supported in AMC SQL. The only supported operations are queries that operate on defined tables within AMC. Tables in AMC can be queried like SQL tables.

In addition, AMC also supports for your subscriptions through Paid Features. You can also upload your own datasets.

## Amazon DSP traffic tables

The following tables contain inputs from all Amazon DSP campaigns from the DSP advertising accounts that have been added to this instance. Includes records from ad product types such as Display, Online Video, Streaming TV, Audio, etc. 

- [Amazon DSP impressions](guides/amazon-marketing-cloud/datasources/dsp_impressions)
- [Amazon DSP clicks](guides/amazon-marketing-cloud/datasources/dsp_clicks)
- [Amazon DSP views](guides/amazon-marketing-cloud/datasources/dsp_views)
- [Amazon DSP impressions by segments](guides/amazon-marketing-cloud/datasources/dsp_impressions_by_segments)

## Amazon Ads conversion tables

These tables both contain pairs of traffic and conversion events. "Traffic events" are impressions and clicks. "Conversion events" include purchases, various interactions with Amazon website, such as detailed page views and pixel fires. Each conversion event in the table is attributed to its corresponding traffic event.

- [Conversions](guides/amazon-marketing-cloud/datasources/conversions)
- [Conversions with relevance](guides/amazon-marketing-cloud/datasources/conversions_with_relevance)
- [Amazon attributed events by conversion time](guides/amazon-marketing-cloud/datasources/amazon_attributed_events_by_conversion_time)

## Sponsored ads table

This table includes both impressions and clicks for sponsored ads programs.

- [Sponsored ads traffic table](guides/amazon-marketing-cloud/datasources/sponsored_ads_traffic)

## AMC paid features

The following tables contain signals associated with AMC paid features. For more information about availability and eligibility, refer to [Paid features overview](guides/amazon-marketing-cloud/datasources/paid_features_overview).

- [Conversions all](guides/amazon-marketing-cloud/datasources/conversions_all_paid)
- [Amazon audience segment membership](guides/amazon-marketing-cloud/datasources/audience_segment_membership_paid)
- [Amazon Your Garage](guides/amazon-marketing-cloud/datasources/amazon_your_garage_paid)
- [Amazon Brand Store insights](guides/amazon-marketing-cloud/datasources/brand_store_insights_paid)
- [Amazon Retail Purchase](guides/amazon-marketing-cloud/datasources/amazon_retail_purchase_paid)
- [Amazon Prime Video Channel Insights](guides/amazon-marketing-cloud/datasources/prime_video_channel_insights_paid)
- [Experian vehicle purchases](guides/amazon-marketing-cloud/datasources/experian_paid.md)
- [NC Solutions (NCS) CPG Insights Stream](guides/amazon-marketing-cloud/datasources/cpg_insights_stream_paid.md)

> [NOTE] Dataset availability can vary by the marketplace to which the AMC account belongs.

## AMC Audiences tables

AMC Audiences uses a unique set of tables that mirror the data available in AMC for both Amazon Ads tables and Paid features tables.

### Amazon Ads audience tables

* amazon\_attributed\_events\_by\_conversion\_time\_for\_audiences
* amazon\_attributed\_events\_by\_traffic\_time\_for\_audiences
* conversions\_for\_audiences
* conversions\_with\_relevance\_for\_audiences
* dsp\_clicks\_for\_audiences
* dsp\_impressions\_by\_matched\_segments\_for\_audiences
* dsp\_impressions\_by\_user\_segments\_for\_audiences
* dsp\_impressions\_for\_audiences
* dsp\_views\_for\_audiences
* dsp\_video\_events\_feed\_for\_audiences
* sponsored\_ads\_traffic\_for\_audiences

### Paid features audience tables

If the Paid feature is available in your region or marketplace and your instance is eligible, the audience table will be automatically available in your instance. For more information, refer to [Paid features overview](guides/amazon-marketing-cloud/datasources/paid_features_overview).

* conversions\_all\_for\_audiences
* audience\_segments\_[amer|apac|eu]\_inmarket\_for\_audiences
* audience\_segments\_[amer|apac|eu]\_inmarket\_snapshot\_for\_audiences
* audience\_segments\_[amer|apac|eu]\_lifestyle\_for\_audiences
* audience\_segments\_[amer|apac|eu]\_lifestyle\_snapshot\_for\_audiences
* segment\_metadata\_for\_audiences
* amazon\_your\_garage\_for\_audiences
* amazon\_brand\_store\_page\_views(\_non\_endemic)\_for\_audiences
* amazon\_brand\_store\_engagement\_events(\_non\_endemic)\_for\_audiences
* amazon\_retail\_purchases\_for\_audiences
* amazon\_pvc\_enrollments\_for\_audiences
* amazon\_pvc\_streaming\_events\_feed\_for\_audiences


## Amazon Ad Server tables

> [WARNING] Amazon Ad Server was sunset in Q4 2024, please visit the [AAS offboarding information page](https://support.sizmek.com/hc/en-us/articles/21193881095565-AAS-offboarding-package) for offboarding support resources and sunset FAQs.

## Important notes for querying data sets

### Time Zones

AMC queries are based on UTC time by default, but the advertising console provides reports based on advertiser time zone (e.g. America/Los_Angeles).

When working with AMC APIs, to match AMC to the advertiser time zone, the `timeWindowTimeZone` parameter of the `CreateWorkflowExecution` request can be used to specify the time zone.

### Arbitrary Attribution Intervals

In the API, there are 4 intervals metrics can be viewed at: `_1d, _7d, _14d, _30d`, but in AMC, a user can aggregate results for any interval up to the attribution window, which is 14d. For example, to get conversions that occurred within `_9d `of the traffic time, use the following SQL

```
SELECT
campaign,
SUM(total_purchases) AS total_orders_9d
FROM amazon_attributed_events_by_traffic_time
WHERE
SECONDS_BETWEEN(traffic_event_dt_utc, conversion_event_dt_utc) <= 606024 * 9
GROUP BY
campaign
```

Alternatively, fitting the same concept into a single expression:

```
SUM(IF(SECONDS_BETWEEN(traffic_event_dt_utc, conversion_event_dt_utc) <= 9 * 24 * 60 * 60, total_purchases_clicks, 0)) AS purchases_9d
```

### Conversion vs Traffic Time

As products and in the advertising console, Sponsored Products and Sponsored Display uses `amazon_attributed_events_by_traffic_time `and Sponsored Brands uses `amazon_attributed_events_by_conversion_time`.

### Sponsored Products Conversions and Traffic

The following example shows how to union conversions with Sponsored Products. Notice that we filter specifically for the ad product we care about here as seen by:

```
     WHERE
        ad_product_type = 'sponsored_products'
```

This ensures that we do not include display conversions, which currently has a null value for `ad_product_type.`

```
WITH
traffic_and_conversion_events AS
(
SELECT
advertiser
, campaign
, 0 AS impressions
, 0 AS clicks        , IF(SECONDS_BETWEEN(traffic_event_dt_utc, conversion_event_dt_utc) <= 60*60*24 * 7, total_purchases, 0) AS total_purchases_7d
        , IF(SECONDS_BETWEEN(traffic_event_dt_utc, conversion_event_dt_utc) <= 60*60*24 * 14, total_purchases, 0) AS total_purchases_14d
        , IF(SECONDS_BETWEEN(traffic_event_dt_utc, conversion_event_dt_utc) <= 60*60*24 * 14, purchases, 0) AS purchases_14d
        , IF(SECONDS_BETWEEN(traffic_event_dt_utc, conversion_event_dt_utc) <= 60*60*24 * 14, total_product_sales, 0) AS total_product_sales_14d
        , IF(SECONDS_BETWEEN(traffic_event_dt_utc, conversion_event_dt_utc) <= 60*60*24 * 14, total_units_sold, 0) AS total_units_sold_14d
        , IF(SECONDS_BETWEEN(traffic_event_dt_utc, conversion_event_dt_utc) <= 60*60*24 * 14, brand_halo_units_sold, 0) AS brand_halo_units_sold_14d

    FROM
        amazon_attributed_events_by_traffic_time
    WHERE
        ad_product_type = 'sponsored_products'

    UNION ALL

    SELECT
        advertiser
        , campaign
        , impressions
        , clicks

        , 0 AS total_purchases_7d
        , 0 AS total_purchases_14d
        , 0 AS purchases_14d

        , 0 AS total_product_sales_14d
        , 0 AS total_units_sold_14d
        , 0 AS brand_halo_units_sold_14d

    FROM
        sponsored_products_traffic
)
        , IF(SECONDS_BETWEEN(traffic_event_dt_utc, conversion_event_dt_utc) <= 60*60*24 * 7, total_purchases, 0) AS total_purchases_7d
        , IF(SECONDS_BETWEEN(traffic_event_dt_utc, conversion_event_dt_utc) <= 60*60*24 * 14, total_purchases, 0) AS total_purchases_14d
        , IF(SECONDS_BETWEEN(traffic_event_dt_utc, conversion_event_dt_utc) <= 60*60*24 * 14, purchases, 0) AS purchases_14d
        , IF(SECONDS_BETWEEN(traffic_event_dt_utc, conversion_event_dt_utc) <= 60*60*24 * 14, total_product_sales, 0) AS total_product_sales_14d
        , IF(SECONDS_BETWEEN(traffic_event_dt_utc, conversion_event_dt_utc) <= 60*60*24 * 14, total_units_sold, 0) AS total_units_sold_14d
        , IF(SECONDS_BETWEEN(traffic_event_dt_utc, conversion_event_dt_utc) <= 60*60*24 * 14, brand_halo_units_sold, 0) AS brand_halo_units_sold_14d

    FROM
        amazon_attributed_events_by_traffic_time
    WHERE
        ad_product_type = 'sponsored_products'

    UNION ALL

    SELECT
        advertiser
        , campaign
        , impressions
        , clicks

        , 0 AS total_purchases_7d
        , 0 AS total_purchases_14d
        , 0 AS purchases_14d

        , 0 AS total_product_sales_14d
        , 0 AS total_units_sold_14d
        , 0 AS brand_halo_units_sold_14d

    FROM
        sponsored_products_traffic
)
SELECT
advertiser
, campaign
, SUM(impressions) AS impressions
, SUM(clicks) AS clicks, SUM(total_purchases_7d) AS total_purchases_7d
, SUM(total_purchases_14d) AS total_purchases_14d
, SUM(purchases_14d) AS purchases_14d
, SUM(total_units_sold_14d) AS total_units_sold_14d
, SUM(brand_halo_units_sold_14d) AS brand_halo_units_sold_14d
, SUM(total_product_sales_14d) AS total_product_sales_14d
, SUM(total_purchases_7d) AS total_purchases_7d
, SUM(total_purchases_14d) AS total_purchases_14d
, SUM(purchases_14d) AS purchases_14d
, SUM(total_units_sold_14d) AS total_units_sold_14d
, SUM(brand_halo_units_sold_14d) AS brand_halo_units_sold_14d
, SUM(total_product_sales_14d) AS total_product_sales_14d
FROM
traffic_and_conversion_events
GROUP BY
advertiser, campaign
```
