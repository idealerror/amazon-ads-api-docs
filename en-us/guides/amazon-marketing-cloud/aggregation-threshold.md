---
title: Aggregation threshold
description: What is the AMC aggregation threshold
type: guide
interface: api
tags:
  - amc
---
# Data aggregation thresholds in AMC

Advertisers use AMC to perform analytics easily across multiple, **pseudonymized** data sets to generate aggregated reports. To protect the privacy of our shoppers exposed to ads, we have privacy safeguards in place that are apparent when you query the data. The privacy safeguards prevent AMC users from exporting data at a grain that would violate our privacy safeguards. The aggregation threshold of a column determines the restrictions that are enforced to ensure customer privacy safeguards are not violated if a column is used in a workflow.

Before we review the various aggregation threshold levels, it will be helpful to highlight a practical example of the column `user_id`, which has an aggregation threshold of "very high" and is used in many AMC queries. In practice, an aggregation threshold level of "very high" means that you will never be able to select the column in the final SELECT statement of a SQL query because this statement would expose the values of the column in the final output. In this example the selecting the actual `user_id` in the final SELECT statement would expose a single user's data, thus violating our privacy safeguards. Even with these restrictions in place, there are many permitted users of `user_id` that do not expose the actual `user_id`.

Below are just a few examples of insights that advertisers derive using the column `user_id`, even though it is highly sensitive:

- First, many advertisers use the `user_id` to count the number of unique users reached by campaign.
- Second, the optimal ad frequency insight, which we reviewed in the previous section, uses the `user_id`.
- Third, other advertisers leverage the `user_id` in queries to measure the customer journey across various ad products, such as Display + OTT or Display + Sponsored Products.

## Column Classifications

Each column is classified into one of the categories below based on the combination of the aggregation threshold level and the filtering restrictions. All categories can be found in the [data sources](guides/amazon-marketing-cloud/datasources/overview) under the column 'aggregation threshold'. The classifications are also in the schema explorer.

### None

Values from the column can be included in the output of workflows without any special restrictions being enforced. This classification is generally used for metric columns, such as impressions and clicks. The aggregation threshold is 1 and there are no filtering restrictions.

### Low

Values from this column can be only be included in the output of workflows if the row contains at least two distinct users. This classification is generally used for columns that contain dimensions that describe campaigns such as `campaign_id` and `ad_id`. The aggregation threshold is 2 and there are no filtering restrictions.

### Medium

Values from the column can only be included in the output of workflows if the row contains at least one hundred distinct users. Examples: `bid_price` and `winning_bid_cost`. The aggregation threshold is 100 and there are no filtering restrictions.

### High

Values from the column can only be included in the output of workflows if the row contains at least one hundred distinct users. The aggregation threshold is 100 and there are filtering restrictions. Filters using literal (static) values cannot be applied to the column. For example, `postal_code` falls into this category and this filter is not allowed: WHERE `postal_code` in (10001, 10011).

### Very high

Values from the column can never be included in workflow output, though they may be used for intermediate purposes in workflows as long as they are dropped or aggregated with COUNT or COUNT DISTINCT. There is no aggregation threshold that will reveal columns with the classification 'very high' because doing so would expose events from a single user. There are also filter restrictions. Filters using literal (static) values cannot be applied to the column. For example, `user_id` falls into this category and this filter is not allowed: WHERE `user_id` in (111, 222). If a query compares literal values to high or very high sensitivity columns (or those derived from them), the query will be rejected. Both direct literal values and values that were influenced by literal values are considered. Literal values can originate from literal or parameter expressions in order to prevent indirect ways of producing a value. This protection is in place for all query constructs that could potentially compare multiple values, such as:

- Expressions that compare values such as Equals, RegexMatches, Min, etc.
- Join keys in join operations
- Group-by keys in aggregate operations

### Internal

Values from the column can never be included in workflow output, though they may be used for intermediate purposes in workflows as long as they are dropped or aggregated with COUNT or DISTINCT COUNT. Examples for this type of column include `advertiser` and `campaign`.

## Postal code restriction example


### Scenario

A query to evaluate purchases by `postal`, a field with the aggregation threshold of **high,** returns many NULL (i.e. blank) rows.

#### Example query used

```
select postal_code, sum(purchases) as purchases
from amazon_attributed_events_by_conversion_time
group by 1
```

#### Results returned

Due to the privacy safeguards, NULL rows were returned. The data returned includes 95 rows, of which 91 include the postal code and the purchases. 4 rows include the purchases, but exclude the postal code. A sum of the purchases from the file = 12,000. For brevity, the entire dataset is not shown here, but the head and tail of the csv is shown.

Examples of the first four of the 91 complete rows that include both the postal code and purchases are below. The data returned is NOT NULL because there were at least 100 users for each postal code.

| postal code | purchases |
| ----------- | --------- |
| 10001       | 752       |
| 10023       | 610       |
| 10007       | 425       |
| 10012       | 323       |

The 4 rows without a postal code, but with purchases are below. The postal code was omitted from the output because there were fewer than 100 users in the postal codes.


| postal code  | purchases |
|:------------:|:---------:|
|      -       | 75        |
|      -       | 52        |
|      -       | 41        |
|      -       | 15        |



## How should the NULL rows be managed?

First, consider discarding the NULL rows. In the example above, we can evaluate the percentage of purchases that are missing a postal code. There were over 12,000 purchases and only 183 did not have a zip code match. Only 4 postal codes representing 1.5% of all purchases were missing a postal code. It's likely in your best interest to focus on the
insights from 95 rows that include the zip code.

Alternatively, try the tips below to include more users.

- Extend the time window of the query.
- Make any filters less restrictive to include more campaigns, line items, ASINs, etc.

## Steps to follow when your query output has NULL rows

- **Step 1:** For every key metric, what percentage of this metric is represented by a NULL vs non-NULL dimensions? In the example above, we found that 1.5% of postal codes had a NULL value. A pivot table in Excel will help you to answer this question.
- **Step 2**: If a low percentage of your key metrics are from NULL dimensions, it's likely in your best interest to delete them from the file. You decide what your threshold for 'low' is.
- **Step 3**: If you have too many metrics with NULL dimensions (based on step 1), you will need to make your data less granular to get meaningful insights. Try the following:

  - Are your dimensions high/medium or low? See the section that describes [column classifications.](#column-classifications)  Knowing the aggregation threshold of your dimensions will help you to understand if you must have 2 users (low) or 100 users (high/medium) in the aggregated output.
  - Consider extending the time window of the query.
  - If you are selecting the day in your final output, consider selecting a two-day period or a week.
  - If you are selecting an hour in your final output, consider selecting a 4- or 6-hour window.
  - If you are selecting a line item, consider selecting a campaign instead.
  - Make any filters less restrictive to include more campaigns, line items, ASINs, etc.
