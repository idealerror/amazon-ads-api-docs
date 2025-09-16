---
title: Querying Amazon Marketing Stream Data
description: Instructions for querying Amazon Marketing Stream, including sample queries
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Display
    - Onboarding
keywords:
    - stream 
    - get started with stream
    - querying data
    - sample stream queries
    - stream FAQs 
---

# Querying Amazon Marketing Stream data

Amazon Marketing Stream sends data aggregated at the hourly level directly to your AWS account. Any changes to your data as a result of the click or conversion invalidation process are sent separately as deltas. 

Depending on how your AWS administrator has designed your solution, you can query Amazon Marketing Stream like you would any other database. 

Once you have [set up your SQS queues and subscribed to Amazon Marketing Stream datasets](guides/amazon-marketing-stream/onboarding), you can use the sample SQL queries in this document as a starting point for your data analysis.

## Sample queries

Below are examples of queries that you can use to query Amazon Marketing Stream data.

>[WARNING] The following queries are based on a hypothetical setup that assumes Amazon Marketing Stream data is stored in a table called `amazon-marketing-stream.sp-traffic`. Your implementation may use a different database structure or naming convention, so be sure to update the WHERE clause before attempting to run any of these queries. 

### Pull traffic data for targets

This query is for pulling traffic data for all targets on a certain date.

```
SELECT *
FROM "amazon-marketing-stream"."sp-traffic"
WHERE DATE(from_iso8601_timestamp(time_window_start)) = date '2021-03-24'
AND match_type IN('TARGETING_EXPRESSION_PREDEFINED','TARGETING_EXPRESSION')
```

### Pull traffic data for keywords

This query pulls traffic data for all keywords on a given date.

```
SELECT *
FROM "amazon-marketing-stream"."sp-traffic"
WHERE DATE(from_iso8601_timestamp(time_window_start)) = date '2021-03-24'
AND match_type IN('BROAD','PHRASE','EXACT')
```

### Pull traffic data for a specific keyword and hour

This query returns traffic data (impressions, clicks, and views) for a specific keyword in a given hour.

```
SELECT * 
FROM "amazon-marketing-stream"."sp-traffic"
WHERE keyword_id = '12345678'and time_window_start = '2022-02-01T10:00:00-08:00'
ORDER BY day, hour DESC;
```

## FAQ

### How do I get data for targets versus keywords?

Currently, both `target_id` and `keyword_id` values are passed within the `keyword_id` field. You can use the `match_type` column to differentiate between target and keyword.

For targets, `match_type` is set to either `TARGETING_EXPRESSION` (for manual targeted campaigns) or `TARGETING_EXPRESSION_PREDEFINED` (for auto targeted campaigns).

* Example values of `keyword_text`  where `match_type` is `TARGETING_EXPRESSION`
    * `category="3764301"` (category targeting)
    * `asin=“B086PQR1Y3”` (ASIN targeting)
* Example values of `keyword_text`  where `match_type` is `TARGETING_EXPRESSION_PREDEFINED`
    * loose-match (auto-keyword targeting)
    * close-match (auto-keyword targeting)
    * substitutes (auto-product targeting)
    * complements (auto-product targeting)

For keywords, `match_type` is set to either `BROAD`, `PHRASE`, or `EXACT`. The `keyword_text` field contains information on the exact keyword.

> [NOTE] `match_type` and `keyword_text` are not available for the [sp-conversions](guides/amazon-marketing-stream/data-guide#sponsored-products-conversions-sp-conversion) dataset. In order to identify keywords versus targets in sp-conversion, you should join the sp-conversion and sp-traffic datasets using `keyword_id`.

### Why am I seeing conversion data with no related clicks?

There is a known bug in Amazon Marketing Stream that may cause a small number of records from another ad program to show up in [sp-conversions](guides/amazon-marketing-stream/data-guide#sponsored-products-conversions-sp-conversion) data without the corresponding records in sp-traffic data. We have confirmed such unwanted data is a very small percentage of the overall conversion records and should not cause any material impact to your analysis. Until we fix this issue, you can ignore these records from your dataset.