---
title: Data aggregation thresholds in the traffic events API
description: Explanation of data aggregation thresholds.
type: guide
interface: api
tags:
    - Reporting
keywords:
    - SQL query
    - clean room
---

# Data aggregation thresholds in TCR

Advertisers use the traffic events API to perform analytics easily across multiple, **pseudonymized** data sets to generate aggregated reports. To protect the privacy of our shoppers exposed to ads, we have privacy safeguards in place that are apparent when you query the data. The privacy safeguards prevent traffic events API users from exporting data at a grain that would violate our privacy safeguards. The aggregation threshold of a column determines the restrictions that are enforced to ensure customer privacy safeguards are not violated if a column is used in a workflow.

Before we review the various aggregation threshold levels, it will be helpful to highlight a practical example of the column `user_id`, which has an aggregation threshold of "very high" and is used in many traffic events API queries. In practice, an aggregation threshold level of "very high" means that you will never be able to select the column in the final SELECT statement of a SQL query because this statement would expose the values of the column in the final output. In this example the selecting the actual `user_id` in the final SELECT statement would expose a single user's data, thus violating our privacy safeguards. Even with these restrictions in place, there are many permitted users of `user_id` that do not expose the actual `user_id`.

Below are just a few examples of insights that advertisers derive using the column `user_id`, even though it is highly sensitive:

* First, many advertisers use the `user_id` to count the number of unique users reached by campaign.
* Second, the optimal ad frequency insight, which we reviewed in the previous section, uses the `user_id`.
* Third, other advertisers leverage the `user_id` in queries to measure the customer journey across various ad products, such as Display + OTT or Display + Sponsored Products.

## Column Classifications

Each column is classified into one of the categories below based on the combination of the aggregation threshold level and the filtering restrictions. All categories can be found in the data sources under the column 'aggregation threshold'. The classifications are also in the schema explorer.

### **None**

Values from the column can be included in the output of workflows without any special restrictions being enforced. This classification is generally used for metric columns, such as impressions and clicks. The aggregation threshold is 1 and there are no filtering restrictions.

### Low

Values from this column can be only be included in the output of workflows if the row contains at least two distinct users. This classification is generally used for columns that contain dimensions that describe campaigns such as `campaign_id` and `ad_id`. The aggregation threshold is 2 and there are no filtering restrictions.

### Medium

Values from the column can only be included in the output of workflows if the row contains at least one hundred distinct users. Examples: event_dt. The aggregation threshold is 100 and there are no filtering restrictions.

### High

Values from the column can only be included in the output of workflows if the row contains at least one hundred distinct users. The aggregation threshold is 100 and there are filtering restrictions. Filters using literal (static) values cannot be applied to the column. 

### Very high

Values from the column can never be included in workflow output, though they may be used for intermediate purposes in workflows as long as they are dropped or aggregated with COUNT or COUNT DISTINCT. There is no aggregation threshold that will reveal columns with the classification 'very high' because doing so would expose events from a single user. There are also filter restrictions. Filters using literal (static) values cannot be applied to the column. For example, `user_id` falls into this category and this filter is not allowed: WHERE `user_id` in (111, 222). If a query compares literal values to high or very high sensitivity columns (or those derived from them), the query will be rejected. Both direct literal values and values that were influenced by literal values are considered. Literal values can originate from literal or parameter expressions in order to prevent indirect ways of producing a value. This protection is in place for all query constructs that could potentially compare multiple values, such as:

* Expressions that compare values such as Equals, RegexMatches, Min, etc.
* Join keys in join operations
* Group-by keys in aggregate operations


## Impressions by timestamp example

### Scenario

A query to evaluate impressions by timestamp, a field with the aggregation threshold of **high,** returns many NULL (i.e. blank) rows.

#### Example query used

```
select event_dt, sum(impressions) as impressions
from dsp_impressions
group by 1
```

#### Results returned

Due to the privacy safeguards, NULL rows were returned. The data returned includes 95 rows, of which 91 include the timestamp and the impressions. 4 rows include the impressions, but exclude the timestamp. A sum of the impressions from the file = 12,000. For brevity, the entire dataset is not shown here, but the head and tail of the csv is shown.
Examples of the first four of the 91 complete rows that include both the timestamp and impressions are below. The data returned is NOT NULL because there were at least 100 users for each timestamp.

|event_dt	|impressions	|
|---	|---	|
|2023-10-01T00:00:01	|752	|
|2023-10-01T00:01:01	|610	|
|2023-10-01T00:02:01	|425	|
|2023-10-01T00:03:01	|323	|

The 4 rows without a timestamp, but with impressions are below. The timestamp was omitted from the output because there were fewer than 100 users in the timestamps.

|event_dt	|impressions	|
|---	|---	|
|	|75	|
|	|52	|
|	|41	|
|	|15	|

## How should the NULL rows be managed?

Consider discarding the NULL rows.
Alternatively, try the tips below to include more users.

* Extend the time window of the query.
* Make any filters less restrictive to include more campaigns, line items, ASINs, etc.

## Steps to follow when your query output has NULL rows

* **Step 1:** For every key metric, what percentage of this metric is represented by a NULL vs non-NULL dimensions? In the example above, we found that 1.5% of timestamp had a NULL value. A pivot table in Excel will help you to answer this question.
* **Step 2**: If a low percentage of your key metrics are from NULL dimensions, it's likely in your best interest to delete them from the file. You decide what your threshold for 'low' is.
* **Step 3**: If you have too many metrics with NULL dimensions (based on step 1), you will need to make your data less granular to get meaningful insights. Try the following:
    * Are your dimensions high/medium or low? See the section that describes [column classifications.](guides/traffic-events/data-aggregation-thresholds#column-classifications) Knowing the aggregation threshold of your dimensions will help you to understand if you must have 2 users (low) or 100 users (high/medium) in the aggregated output.
    * Consider extending the time window of the query.
    * If you are selecting the day in your final output, consider selecting a two-day period or a week.
    * If you are selecting an hour in your final output, consider selecting a 4- or 6-hour window.
    * If you are selecting a line item, consider selecting a campaign instead.
    * Make any filters less restrictive to include more campaigns, line items, ASINs, etc.
