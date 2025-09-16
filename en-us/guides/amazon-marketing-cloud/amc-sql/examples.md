---
title: AMC SQL examples
description: The examples of AMC SQL
type: guide
interface: api
---

# AMC SQL examples

**Rounded cost as GROUP BY**

In this case we want to bucket all impressions and the sum of supply
costs into finer detail by adding a new dimension which is the supply
cost rounded down to the cent.

```
SELECT
  advertiser,
  ROUND(supply_cost / 1000000, 0) AS rounded_supply_cost,
  SUM(impressions) AS impressions,
  SUM(supply_cost / 1000000) AS supply_cost
FROM 
  dsp_impressions
GROUP BY
  advertiser,
  rounded_supply_cost
```

**Impressions aggregated on the hour**

Simple example of creating a column based on the date to add an
additional bucket to group on for impressions.

```
SELECT
  advertiser,
  EXTRACT(HOUR FROM impression_date) AS impression_date_hour,
  SUM(impressions) AS impressions
FROM
  dsp_impressions
GROUP BY
  advertiser,
  impression_date_hour
```

**Impression user frequency counting**

To find the number of impressions viewed per user, first aggregate all
impressions by user and then aggregate against the result with a
generated STRING frequency column.

```
SELECT
  advertiser_id,
  advertiser,
  IF(impressions < 10, CONCAT('frequency_', impressions), 'frequency_10+') AS frequency_buckets,
  COUNT(DISTINCT user_id) AS users_in_bucket
FROM (
    SELECT
      advertiser,
      user_id,
      SUM(impressions) AS impressions
    FROM
      dsp_impressions
    GROUP BY
        advertiser,
        user_id
)
GROUP BY
  advertiser,
  frequency_buckets
```

**Aggregation by Brand**

The following query partitions impressions into two buckets, one being a
specific brand and the other being any other brands in the data set.
This example could be extended to bucket by addition brands using chains
of IF statements or more simply with a CASE statement. Note that SIMILAR
TO takes a Java regular expressions as input.

```
SELECT
  advertiser,
  IF(campaign SIMILAR TO '^BRAND_A.*', 'BRAND_A', 'BRAND_OTHER') AS brand,
  SUM(impressions) AS impressions
FROM
  dsp_impressions
GROUP BY
  advertiser,
  brand
```

**Joining two data sets**

Simple join between the clicks and impressions data set to calculate the
sum total of clicks and impressions at the campaign level.

```
SELECT
  dsp_impressions.campaign_id,
  SUM(dsp_impressions.impressions) AS impressions,
  SUM(clicks) AS clicks
FROM
  dsp_impressions LEFT JOIN display_clicks USING (request_tag)
GROUP BY
  dsp_impressions.campaign_id
```
