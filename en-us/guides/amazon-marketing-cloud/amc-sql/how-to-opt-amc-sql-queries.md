---
title: Optimize AMC SQL queries
description: Best practices, query examples, and steps for SQL troubleshooting and optimization
type: guide
interface: api 
---


# Optimize AMC SQL queries

AMC supports use cases of varying complexity, and each AMC instance contains up to 13 months of your advertising records. When you are running complex queries or queries that run across long date ranges, you may have experienced longer-than-expected run times or post-execution errors. In many cases, these issues can be mitigated by optimizing the SQL code of your query.

SQL optimization is the process of improving the performance of a query by adjusting query elements to reduce execution time and resource usage while still producing accurate results. Queries can often be optimized for simplicity and efficiency by:

* More efficiently filtering records earlier in the query
* Eliminating redundant sub-queries
* Optimizing JOIN conditions
* Optimizing the query’s order of operations

This guide provides AMC SQL best practices, query examples, and steps for SQL troubleshooting and optimization.

>[TIP:Tip]If you have access to the AMC UI, you can also view this guide as an Instructional Query in the AMC UI. For more information, visit [Instructional Query: How to optimize AMC SQL queries (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries/3c308a66e41d7572f9df0a196bf498036980e265bf80328e694819660b40884b).

## Understanding query execution order

When optimizing SQL, it is beneficial to first have an understanding of the different parts of a SQL query and the order in which they are executed. Every AMC query has the following parts:

* **Sources (FROM):** Tables and views to which your query is applied
* **Filters (WHERE):** Clauses that are used to define the records relevant to your analysis
* **Joins (e.g. INNER JOIN):** Means of combining one or more sources together
* **Aggregations (GROUP BY):** Used with aggregate functions to create summary records with common dimension values
* **Final conditions (HAVING):** Additional logic, similar to WHERE, but applied atop aggregated results

When an AMC query is executed, it will follow the order as listed below:

1. The sources are chosen and read
2. The specified WHERE filters are applied atop the sources
3. Joins and/or aggregations are performed (the exact order depends on the query structure)
4. HAVING conditions are applied to the final result set

Below is an example query that demonstrates this order:

![Order of operations](/_images/amazon-marketing-cloud/iq_sql_opt_order_ops_sm.png)

## Best practice summary
Below are some query best practices to keep in mind as you work in AMC.

### Filter data early and properly
Sources and any filters applied atop them ultimately dictate the amount of records that your query is executing across. The primary optimization mechanism is to ensure that your query is leveraging the correct sources, and that you are applying filters as specifically as possible. Optimizing sources and filters has compounding benefits on the joins and aggregations later in the query by reducing cached record volume.

For example, let’s say you want to gather conversion events for one specific campaign and one specific event type. If your instance contains 50 advertisers in it, all of whom are associated with a variety of campaigns and event types, it would be wasteful for the query to search records across all advertisers, campaigns, and event types, and/or output more records than are necessary for your needs. 

Illustrated on the below-left is a query that allows you to get the information you want (but you’ll have to filter the results after you get them to your specific campaign and event type). On the right is a query that allows you to achieve the same result (conversion event by campaign, limited to one event type) - faster, as it provides specific guidance on the exact records of interest. Filtering to just the relevant records, as early on in the query as possible, will improve the query’s performance.

![Side-by-side comparison](/_images/amazon-marketing-cloud/iq_sql_opt_comparison.png)

It is also preferable to use **S**earch **ARG**ument **ABLE** (**Sargable**) filters where possible. Sargable filters allow your query to more efficiently narrow down to the relevant records because they avoid using functions within WHERE filters. Whereas non-sargable filters force the query engine to read all of the records and apply the function to determine whether a record is relevant or not. Below is an example of a non-sargable query, using ARRAY\_CONTAINS to check if the value of a column is in the list of in-scope values. While the query is functionally correct, this results in the query engine having to evaluate the result of ARRAY\_CONTAINS (a function) which would then prevent the query from reading unnecessary segments of data from the table.

```
SELECT
  campaign_id_string,
  COUNT(DISTINCT user_id) AS users
FROM
  amazon_attributed_events_by_traffic_time
WHERE
  ARRAY_CONTAINS(ARRAY [ '1234567', '2345678' ], campaign_id_string)
  AND user_id IS NOT NULL
GROUP BY
  1
```

The above query can be optimized and made sargable by using either CTE + INNER JOIN, or CTE + IN clause. An example of optimizing the query using CTE + INNER JOIN is given below:

```
WITH campaigns (campaign_id_string) AS (
  VALUES
    ('1234567'),
    ('2345678')
)
SELECT
  a.campaign_id_string,
  COUNT(DISTINCT user_id) AS users
FROM
  amazon_attributed_events_by_traffic_time a
  INNER JOIN campaigns c ON (a.campaign_id_string = c.campaign_id_string)
WHERE
  user_id IS NOT NULL
GROUP BY
  1
```

### Leverage UNION ALL instead of JOIN when possible
When row-by-row matching between tables is not required, it is more appropriate to use a UNION ALL operation instead of a JOIN. UNION ALL is used to stack data vertically, without any matching logic, whereas JOIN is used to combine datasets horizontally, based on a matching condition. The example below illustrates a scenario where UNION ALL can be used in place of a JOIN.

<details class="details-bar">
  <summary><b>Click to see: Non-optimized example</b></summary> 
  
```
WITH sa_reach AS (
  SELECT
    campaign_id_string,
    COUNT(user_id) AS reach
  FROM
    sponsored_ads_traffic
  GROUP BY
    1
),
att_conv AS(
  SELECT
    campaign_id_string,
    SUM(conversions) AS conversions
  FROM
    amazon_attributed_events_by_conversion_time
  GROUP BY
    1
) -- OPTIMIZATION OPPORTUNITY START: The query uses a JOIN to combine records, which is inefficient in this context
SELECT
  COALESCE(
    sa_reach.campaign_id_string,
    att_conv.campaign_id_string
  ) AS campaign_id_string,
  reach,
  conversions
FROM
  sa_reach
  FULL OUTER JOIN att_conv ON sa_reach.campaign_id_string = att_conv.campaign_id_string -- OPTIMIZATION OPPORTUNITY END
```
</details>

In this example, you could use UNION ALL to combine the tables by stacking them on top of each other, without the need for any matching criteria between the rows. This would achieve the same result in a more efficient way.

<details class="details-bar">
  <summary><b>Click to see: Optimized example</b></summary>
  
```
WITH sa_reach AS (
  SELECT
    campaign_id_string,
    COUNT(user_id) AS reach
  FROM
    sponsored_ads_traffic
  GROUP BY
    1
),
att_conv AS(
  SELECT
    campaign_id_string,
    SUM(conversions) AS conversions
  FROM
    amazon_attributed_events_by_conversion_time
  GROUP BY
    1
) -- OPTIMIZED VERSION START: UNION ALL with GROUPing outside subquery achieves the same result
SELECT
  campaign_id_string,
  SUM(reach) AS reach,
  SUM(conversions) AS converions
FROM
  (
    SELECT
      campaign_id_string,
      reach,
      0 AS conversions
    FROM
      sa_reach
    UNION ALL
    SELECT
      campaign_id_string,
      0 AS reach,
      conversions
    FROM
      att_conv
  )
GROUP BY
  1 -- OPTIMIZED VERSION END
```
</details>

### Optimize join order
Joining two or more sources can increase or decrease the volume of data that the query is operating across. An improper join condition can result in a cartesian join (matching multiple rows from the first table with multiple rows from the second table). This can significantly increase the number of records your query is operating across. Some things to keep in mind:

* Is a JOIN necessary, or would UNION/UNION ALL suffice?
* Can you use EQUI-JOINs (indicated with an equal sign such as `t1.col_a = t2.col_b`)?
* Does your JOIN include an ON clause at the proper level of granularity? (e.g. is it necessary to join on `user_id`, or would it be possible to join on `campaign_id_string` and `user_id`?)

The order in which tables/CTEs are joined can also impact query performance. To maximize performance, you should join tables/CTEs in the order where the next operation will work on the smallest amount of records as possible. As an example, let’s assume that you have the following three tables and each of them can be joined with each other:

* t1: 10,000,000 records
* t2: 1,000,000 records
* t3: 100 records

The most efficient approach would be to join both t1 and t2 with t3, as t3 is small enough that a query engine could load the whole table into memory. For example:

```
SELECT
  [...]
FROM
  t1
  INNER JOIN t2 ON (t1.col_a = t2.col_a)
  INNER JOIN t3 ON (t1.col_b = t3.col_b)
GROUP BY
  [...]
```

### Avoid logical errors and hidden cartesian (cross) joins

It is crucial to avoid logical errors in queries, as these can significantly impact both accuracy and runtime. Logical errors, in addition to generating incorrect results, can often cause unintentional or hidden cartesian (cross) joins that lead to exponential growth in the amount of data processed. This means that in addition to the end result being incorrect, it can take a long time for the query to run. Carefully reviewing your query logic can help identify and address such errors.

The example query below analyzes the cost, purchase, and new-to-brand (NTB) purchases for a group of campaigns. However, it accidentally inflates the data through some logical errors, resulting in cartesian joins.

<details class="details-bar">
  <summary><b>Click to see: Non-optimized example</b></summary>
  
```
WITH campaigns_scope (campaign_group, campaign_id_string, ecpm) AS (
  VALUES
    ('A', '1111', 1.5),
    ('B', '2222', 0),
    ('C', 'C3333', 0)
),
-- OPTIMIZATION OPPORTUNITY 1 START: Incorrect aggregation
conversions_purchase AS (
  SELECT
    c.user_id,
    c.campaign_id_string AS campaign_id_string,
    c.new_to_brand AS ntb_flag,
    MAX(IF(new_to_brand = TRUE, 1, 0)) AS ntb_purchaser,
    SUM(c.total_purchases) AS total_purchases,
    SUM(c.total_product_sales) AS total_product_sales,
    SUM(c.new_to_brand_total_purchases) AS ntb_purchases,
    SUM(c.new_to_brand_total_product_sales) AS ntb_sales
  FROM
    amazon_attributed_events_by_traffic_time AS c
  WHERE
    user_id IS NOT NULL
    AND total_product_sales > 0
    AND c.campaign_id_string IN (
      SELECT
        campaign_id_string
      FROM
        CAMPAIGNS_SCOPE
    )
  GROUP BY
    1,
    2,
    3
  UNION ALL
  SELECT
    c.user_id,
    c.campaign_id_string AS campaign_id_string,
    c.new_to_brand AS ntb_flag,
    MAX(IF(new_to_brand = TRUE, 1, 0)) AS ntb_purchaser,
    SUM(c.total_purchases) AS total_purchases,
    SUM(c.total_product_sales) AS total_product_sales,
    SUM(c.new_to_brand_total_purchases) AS ntb_purchases,
    SUM(c.new_to_brand_total_product_sales) AS ntb_sales
  FROM
    amazon_attributed_events_by_traffic_time AS c
  WHERE
    c.user_id IS NOT NULL
    AND total_product_sales > 0
    AND c.campaign_id_string IN (
      SELECT
        campaign_id_string
      FROM
        campaigns_scope
    )
  GROUP BY
    1,
    2,
    3
),
-- OPTIMIZATION OPPORTUNITY 1 END
-- OPTIMIZATION OPPORTUNITY 2 START: Incorrect aggregation
impressions_spend AS (
  SELECT
    i.user_id,
    i.campaign_id_string,
    d.campaign_group,
    SUM(I.impressions) AS impressions,
    SUM(
      IF(
        CAST(d.ecpm AS DECIMAL(6, 3)) > 0,
        CAST(d.ecpm AS DECIMAL(6, 3)) / 1000 * i.impressions,
        i.total_cost / 100000
      )
    ) AS cost
  FROM
    dsp_impressions AS i
    JOIN campaigns_scope AS d ON i.campaign_id_string = d.campaign_id_string
  WHERE
    i.user_id IS NOT NULL
  GROUP BY
    1,
    2,
    3
  UNION ALL
  SELECT
    i.user_id,
    i.campaign_id_string AS campaign_id_string,
    s.campaign_group,
    SUM(i.impressions) AS impressions,
    SUM(
      IF(
        CAST(s.ecpm AS DECIMAL(6, 3)) > 0,
        CAST(s.ecpm AS DECIMAL(6, 3)) / 1000 * i.impressions,
        i.spend / 100000000
      )
    ) AS cost
  FROM
    sponsored_ads_traffic AS i
    JOIN campaigns_scope AS s ON i.campaign_id_string = s.campaign_id_string
  WHERE
    i.user_id IS NOT NULL
  GROUP BY
    1,
    2,
    3
) -- OPTIMIZATION OPPORTUNITY 2 END
SELECT
  a.campaign_id_string,
  a.campaign_group,
  SUM(a.impressions) AS impressions,
  SUM(a.cost) AS cost,
  COUNT(DISTINCT b.user_id) AS total_purchasers,
  SUM(b.total_purchases) AS total_Purchases,
  SUM(b.total_product_sales) AS total_sales,
  SUM(b.ntb_purchaser) AS ntb_purchasers,
  SUM(b.ntb_purchases) AS ntb_purchases,
  SUM(b.ntb_sales) AS ntb_sales
FROM
  impressions_spend AS a -- OPTIMIZATION OPPORTUNITY 3 START: Hidden cartesian JOIN and incorrect business logic
  LEFT JOIN conversions_purchase AS b ON a.campaign_id_string = b.campaign_id_string
  AND a.user_id = b.user_id -- OPTIMIZATION OPPORTUNITY 3 END
GROUP BY
  1,
  2
```
</details>

In example above, the `conversions_purchase` and `impressions_spend` CTEs are aggregated at the `user_id` level (a missed aggregation opportunity) and joined together in the final SELECT on the `campaign_id_string` and `user_id`. This approach is incorrect because a single campaign can serve multiple impressions to the same `user_id`, and will result in duplicative metrics. For example, if a `user_id` was served 10 impressions from the same campaign and made two purchases, the query logic (hidden cross join) would incorrectly show 10 x 2 = 20 conversions and 20 impressions.

To address this logical error, the query `conversions_purchase` and `impressions_spend` CTEs should be revised to aggregate data at the campaign level, rather than the `user_id` level. In the final SELECT statement, the CTEs can then be joined at the `campaign_id_string` level. This ensures that the metrics are correctly calculated. Review these modifications in the query below.

<details class="details-bar">
  <summary><b>Click to see: Optimized example</b></summary> 
  
```
WITH campaigns_scope (campaign_group, campaign_id_string, ecpm) AS (
  VALUES
    ('A', '1111', 1.5),
    ('B', '2222', 0),
    ('C', 'C3333', 0)
),
-- OPTIMIZED 1 START: Properly aggregated, which also results in fewer records processed
conversions_purchase AS (
  SELECT
    c.campaign_id_string AS campaign_id_string,
    c.new_to_brand AS ntb_flag,
    COUNT(c.user_id) user_id,
    MAX(IF(new_to_brand = TRUE, 1, 0)) AS ntb_purchaser,
    SUM(c.total_purchases) AS total_purchases,
    SUM(c.total_product_sales) AS total_product_sales,
    SUM(c.new_to_brand_total_purchases) AS ntb_purchases,
    SUM(c.new_to_brand_total_product_sales) AS ntb_sales
  FROM
    amazon_attributed_events_by_traffic_time AS c
  WHERE
    user_id IS NOT NULL
    AND total_product_sales > 0
    AND c.campaign_id_string IN (
      SELECT
        campaign_id_string
      FROM
        campaigns_scope
    )
  GROUP BY
    1,
    2
  UNION ALL
  SELECT
    c.campaign_id_string AS campaign_id_string,
    c.new_to_brand AS ntb_flag,
    COUNT(c.user_id) user_id,
    MAX(IF(new_to_brand = TRUE, 1, 0)) AS ntb_purchaser,
    SUM(c.total_purchases) AS total_purchases,
    SUM(c.total_product_sales) AS total_product_sales,
    SUM(c.new_to_brand_total_purchases) AS ntb_purchases,
    SUM(c.new_to_brand_total_product_sales) AS ntb_sales
  FROM
    amazon_attributed_events_by_traffic_time AS c
  WHERE
    c.user_id IS NOT NULL
    AND total_product_sales > 0
    AND c.campaign_id_string IN (
      SELECT
        campaign_id_string
      FROM
        campaigns_scope
    )
  GROUP BY
    1,
    2
),
-- OPTIMIZED 1 END
-- OPTIMIZED 2 START: Properly aggregated, which also results in fewer records processed
impressions_spend AS (
  SELECT
    i.campaign_id_string,
    d.campaign_group,
    COUNT(DISTINCT i.user_id) user_id,
    SUM(i.impressions) AS impressions,
    SUM(
      IF(
        CAST(d.ecpm AS DECIMAL(6, 3)) > 0,
        CAST(d.ecpm AS DECIMAL(6, 3)) / 1000 * i.impressions,
        i.total_cost / 100000
      )
    ) AS cost
  FROM
    dsp_impressions AS i
    JOIN campaigns_scope AS d ON i.campaign_id_string = d.campaign_id_string
  WHERE
    i.user_id IS NOT NULL
  GROUP BY
    1,
    2
  UNION ALL
  SELECT
    i.campaign_id_string AS campaign_id_string,
    s.campaign_group,
    COUNT(DISTINCT i.user_id) user_id,
    SUM(i.impressions) AS impressions,
    SUM(
      IF(
        CAST(s.ecpm AS DECIMAL(6, 3)) > 0,
        CAST(s.ecpm AS DECIMAL(6, 3)) / 1000 * i.impressions,
        i.spend / 100000000
      )
    ) AS cost
  FROM
    sponsored_ads_traffic AS i
    JOIN campaigns_scope AS s ON i.campaign_id_string = s.campaign_id_string
  WHERE
    i.user_id IS NOT NULL
  GROUP BY
    1,
    2
) -- OPTIMIZED 2 END
SELECT
  a.campaign_id_string,
  a.campaign_group,
  SUM(a.impressions) AS impressions,
  SUM(a.cost) AS cost,
  SUM(a.user_id) AS total_purchasers,
  SUM(b.total_purchases) AS total_purchases,
  SUM(b.total_product_sales) AS total_Sales,
  SUM(b.ntb_purchaser) AS ntb_purchasers,
  SUM(b.ntb_purchases) AS ntb_purchases,
  SUM(b.ntb_sales) AS ntb_sales
FROM
  IMPRESSIONS_Spend a -- OPTIMIZED 3 START: Correct JOIN condition and no hidden cross-joins
  LEFT JOIN CONVERSIONS_Purchase b ON a.campaign_id_string = b.campaign_id_string -- OPTIMIZED 3 END
GROUP BY
  1,
  2
```
</details>

### Eliminate or reduce calculations
Another technique for reducing the amount of computation is delaying the use of aggregation functions/calculations in the query. When implemented in a query, aggregation functions require the processing of records and take up system memory, increasing the amount of work that needs to be done. As a general rule, you want to apply such functions to the smallest amount of data as possible - this usually means applying them at the very end of the query.

Queries may also sometimes contain calculated fields in order to help you easily interpret the results. In AMC, for instance, you can create calculated fields for cost-per-mille (or CPM, the cost in your currency for one thousand impressions) or purchase rate (percentage of users reached by your campaign that go on to purchase). These types of calculations can be implemented within the query, or performed manually after your query has run. In cases where query performance is an issue, it may be beneficial to adjust the query such that the output contains those fields needed for the calculation, which is then performed manually outside of AMC. For instance, instead of including a `purchase_rate` calculated field in your query, you can:

1. Include fields for `unique_reach` (COUNT of distinct `user_id` values associated with impression events) and `unique_purchasers` (COUNT of distinct `user_id` values associated with purchase events) in your query’s final SELECT statement.
2. Within the resulting output, calculate `purchase_rate` by dividing `unique_purchasers` by `unique_reach`.

## Optimization handbook
This section dives deep into a single query, reviewing how to apply best practices to improve the query's performance.

### User-defined JOINs
Some JOIN choices within user-defined query syntax can force the query planner to expend significant amounts of compute resources in order to arrive at the same result set that would have been returned with a more efficient JOIN type.

Things to look for:

* Excessive usage of LEFT JOIN keywords
* Usage of FULL OUTER JOIN keywords
* OUTER JOINs, without NULL value handling
* Hidden cartesian/CROSS joins (e.g. `JOIN table_a ON 1 < 2` or `JOIN table_a ON 1 = 1`)

#### Optimization opportunities

1. **Use UNION/UNION ALL instead of a join, where possible.** In some cases, uniting fields from two tables with UNION is faster than doing so with JOINs since UNION does not need to match values. However, note that these cannot be used in your query logic interchangeably - see Introduction to AMC SQL for more guidance on using each function.
2. **Use a more restrictive join type.** For example, it may be possible to use INNER JOIN instead of LEFT JOIN in some cases.
3. Remove cartesian/CROSS joins where possible.
4. **Utilize EXISTS operators where columns from a joined table are used only for filtering purposes.** In cases where a column from a joined table is used only for filtering purposes, utilizing an EXISTS clause can be more efficient, since EXISTS returns a Boolean value (TRUE or FALSE) based on the evaluation of condition, as opposed to returning an entire string or numeric column.
5. **Eliminating unnecessary JOIN conditions.** Certain JOIN conditions can be removed if an existing column within current table or selection can be used to achieve the same result. The example in below query shows how to replace filter condition using a column in the existing table, thus eliminating an additional join that is only required for a filter condition.

#### Query example

Let’s say an AMC user defined two groups of campaigns: “Display” and “Streaming TV”. The query is trying to assess the overlap of those exposed to both the display campaigns and streaming TV campaigns, and calculate overall impressions and clicks for this group.

The original query is shown below. Keep an eye out for two elements that decrease the efficiency of this query.

1. LEFT JOIN is used in `imp` & `click` CTEs to filter `campaign_id_string`. When you only care about the values that the tables have in common (i.e. the campaigns that are found in both impression and click tables, in this instance) a more efficient alternative would be using a more restrictive join type (INNER JOIN).
2. INNER and LEFT joins are also used to combine the `imp`, `click`, and `combined_user` CTEs in the final SELECT statement. A more efficient alternative would be using UNION ALL instead.

<details class="details-bar">
  <summary><b>Click to see: Non-optimized example</b></summary>
  
```
WITH display (campaign_id_string) AS (
  VALUES
    ('11111')
),
stv (campaign_id_string) AS (
  VALUES
    ('22222')
),
combined AS (
  SELECT
    campaign_id_string,
    'Display' AS campaign_category
  FROM
    display
  UNION ALL
  SELECT
    campaign_id_string,
    'Streaming TV' AS campaign_category
  FROM
    stv
),
imp AS (
  SELECT
    i.user_id,
    i.campaign_id_string,
    cmb.campaign_category,
    SUM(impressions) AS impressions
  FROM
    dsp_impressions i -- OPTIMIZATION OPPORTUNITY 1A START: Inefficient join
    LEFT JOIN combined cmb ON i.campaign_id_string = cmb.campaign_id_string -- OPTIMIZATION OPPORTUNITY 1A END
  WHERE
    user_id IS NOT NULL
  GROUP BY
    1,
    2,
    3
),
click AS (
  SELECT
    c.user_id,
    c.campaign_id_string,
    cmb.campaign_category,
    SUM(clicks) AS clicks
  FROM
    dsp_clicks c -- OPTIMIZATION OPPORTUNITY 1B START: Inefficient join
    LEFT JOIN combined cmb ON c.campaign_id_string = cmb.campaign_id_string -- OPTIMIZATION OPPORTUNITY 1B END
  WHERE
    user_id IS NOT NULL
  GROUP BY
    1,
    2,
    3
),
display_user AS (
  SELECT
    DISTINCT user_id
  FROM
    imp i
  WHERE
    campaign_category = 'Display'
),
stv_user AS (
  SELECT
    DISTINCT user_id
  FROM
    imp i
  WHERE
    campaign_category = 'Streaming TV'
),
combined_user AS (
  SELECT
    du.user_id
  FROM
    display_user du
    INNER JOIN stv_user su ON du.user_id = su.user_id
) -- OPTIMIZATION OPPORTUNITY 2 START: Inefficient join within final SELECT statement
SELECT
  COUNT(DISTINCT i.user_id) AS reach,
  SUM(impressions) AS impressions,
  SUM(clicks) AS clicks
FROM
  imp i
  INNER JOIN combined_user cu ON cu.user_id = i.user_id
  LEFT JOIN click c ON i.user_id = c.user_id
  AND i.campaign_id_string = c.campaign_id_string -- OPTIMIZATION OPPORTUNITY 2 END
```
</details>

Below is the optimized version of the above query, now leveraging INNER JOIN and UNION ALL:

<details class="details-bar">
  <summary><b>Click to see: Optimized example</b></summary> 
  
```
WITH display (campaign_id_string) AS (
  VALUES
    ('11111')
),
stv (campaign_id_string) AS (
  VALUES
    ('22222')
),
combined AS (
  SELECT
    campaign_id_string,
    'Display' AS campaign_category
  FROM
    display
  UNION ALL
  SELECT
    campaign_id_string,
    'Streaming TV' AS campaign_category
  FROM
    stv
),
imp AS (
  SELECT
    i.user_id,
    i.campaign_id_string,
    cmb.campaign_category,
    SUM(impressions) AS impressions
  FROM
    dsp_impressions i -- OPTIMIZED 1A: Now using INNER JOIN
    INNER JOIN combined cmb ON i.campaign_id_string = cmb.campaign_id_string -- OPTIMIZED 1A END
  WHERE
    user_id IS NOT NULL
  GROUP BY
    1,
    2,
    3
),
click AS (
  SELECT
    c.user_id,
    c.campaign_id_string,
    cmb.campaign_category,
    SUM(clicks) AS clicks
  FROM
    dsp_clicks c -- OPTIMIZED 1B START: Now using INNER JOIN
    INNER JOIN combined cmb ON c.campaign_id_string = cmb.campaign_id_string -- OPTIMIZED 1B END
  WHERE
    user_id IS NOT NULL
  GROUP BY
    1,
    2,
    3
),
display_user AS (
  SELECT
    DISTINCT user_id
  FROM
    imp i
  WHERE
    campaign_category = 'Display'
),
stv_user AS (
  SELECT
    DISTINCT user_id
  FROM
    imp i
  WHERE
    campaign_category = 'Streaming TV'
),
combined_user AS (
  SELECT
    du.user_id
  FROM
    display_user du
    INNER JOIN stv_user su ON du.user_id = su.user_id
),
-- OPTIMIZED 2 START: Now using UNION ALL plus additional final SELECT statement
pre AS (
  SELECT
    i.user_id,
    SUM(impressions) AS impressions,
    0 AS clicks
  FROM
    imp i
    INNER JOIN combined_user cu ON cu.user_id = i.user_id
  GROUP BY
    1
  UNION ALL
  SELECT
    c.user_id,
    0 AS impressions,
    SUM(clicks) AS clicks
  FROM
    click c
    INNER JOIN combined_user cu ON cu.user_id = c.user_id
  GROUP BY
    1
)
SELECT
  COUNT(DISTINCT user_id) AS reach,
  SUM(impressions) AS impressions,
  SUM(clicks) AS clicks
FROM
  pre -- OPTIMIZED 2 END
```
</details>

### Missed aggregation opportunities

The result sets returned by queries (or subqueries/CTEs) should be aggregated/reduced to the smallest possible size to reduce computational complexity and increase query efficiency. For CTEs, an unaggregated returned result set can have significant impacts on JOINs or CASE statements performed by subsequent subqueries or other operations. A CTE that returns a result set with duplicate rows should be de-duplicated prior to JOINs with other tables/result sets.

#### Things to look for

* CTEs that could include an aggregation function (such as SUM) due to subsequent aggregation by later operations
* Non-aggregated CTEs that select a subset of a table’s fields but lack a GROUP BY clause
* CTEs that select fields which are not utilized later in the query

#### Optimization opportunities

1. **Add aggregation functions to CTEs where applicable.** Aggregation is a means of reducing the number of records, as much as possible, that are going into the next operation. The lower the number of records, the faster the execution.
2. **Add GROUP BY clauses to non-aggregated CTEs that could return duplicate rows.**
3. **Use GROUP BY instead of DISTINCT to remove duplicates.** DISTINCT returns all rows then de-duplicates them, whereas GROUP BY de-deduplicates rows as they're read by the algorithm one by one. In some cases, you can use GROUP BY to group metrics along common dimension values, thereby removing duplicate rows (in place of using DISTINCT).
4. **Don’t SELECT columns within CTEs if you will not be using them later in your query.**

#### Query example

Let’s continue optimizing the same query used in the previous section, where you adjusted the JOINs for more efficiency. Now let’s look for some additional means of optimization.

1. The `imp` and `click` CTEs produce intermediary result sets with one record per `user_id`, per `campaign_id_string`, per `campaign_category` to work towards determining which users were exposed to more than both campaign groups (display and streaming TV). A more efficient alternative would be to apply an aggregation function on the `campaign_category` dimension in the relevant CTEs, and use `campaign_id_string` only in the ON clause. The new intermediary result sets produce only one record per `user_id` value, reducing the amount of records the query is operating over.
2. In the section from `display_user` to `combined_user`, the only purpose is to filter `user_id`. This can be done more efficiently via a single CTE.

<details class="details-bar">
  <summary><b>Click to see: Non-optimized example</b></summary> 
  
```
WITH display (campaign_id_string) AS (
  VALUES
    ('11111')
),
stv (campaign_id_string) AS (
  VALUES
    ('22222')
),
combined AS (
  SELECT
    campaign_id_string,
    'Display' AS campaign_category
  FROM
    display
  UNION ALL
  SELECT
    campaign_id_string,
    'Streaming TV' AS campaign_category
  FROM
    stv
),
imp AS (
  SELECT
    i.user_id,
    -- OPTIMIZATION OPPORTUNITY 1A START: Reduce record count
    i.campaign_id_string,
    cmb.campaign_category,
    -- OPTIMIZATION OPPORTUNITY 1A END
    SUM(impressions) AS impressions
  FROM
    dsp_impressions i
    INNER JOIN combined cmb ON i.campaign_id_string = cmb.campaign_id_string
  WHERE
    user_id IS NOT NULL
  GROUP BY
    1,
    2,
    3
),
click AS (
  SELECT
    c.user_id,
    -- OPTIMIZATION OPPORTUNITY 1B START: Reduce record count
    c.campaign_id_string,
    cmb.campaign_category,
    -- OPTIMIZATION OPPORTUNITY 1B END
    SUM(clicks) AS clicks
  FROM
    dsp_clicks c
    INNER JOIN combined cmb ON c.campaign_id_string = cmb.campaign_id_string
  WHERE
    user_id IS NOT NULL
  GROUP BY
    1,
    2,
    3
),
-- OPTIMIZATION OPPORTUNITY 2 START: Simplify user_id filtering
display_user AS (
  SELECT
    DISTINCT user_id
  FROM
    imp i
  WHERE
    campaign_category = 'Display'
),
stv_user AS (
  SELECT
    DISTINCT user_id
  FROM
    imp i
  WHERE
    campaign_category = 'Streaming TV'
),
combined_user AS (
  SELECT
    du.user_id
  FROM
    display_user du
    INNER JOIN stv_user su ON du.user_id = su.user_id
),
-- OPTIMIZATION OPPORTUNITY 2 END
pre AS (
  SELECT
    i.user_id,
    SUM(impressions) AS impressions,
    0 AS clicks
  FROM
    imp i
    INNER JOIN combined_user cu ON cu.user_id = i.user_id
  GROUP BY
    1
  UNION ALL
  SELECT
    c.user_id,
    0 AS impressions,
    SUM(clicks) AS clicks
  FROM
    click c
    INNER JOIN combined_user cu ON cu.user_id = c.user_id
  GROUP BY
    1
)
SELECT
  COUNT(DISTINCT user_id) AS reach,
  SUM(impressions) AS impressions,
  SUM(clicks) AS clicks
FROM
  pre
```
</details>

Below is the optimized version of the above query, now leveraging better aggregation practices and consolidating CTEs used to filter to the relevant `user_id` values.

<details class="details-bar">
  <summary><b>Click to see: Optimized example</b></summary> 
  
```
WITH display (campaign_id_string) AS (
  VALUES
    ('11111')
),
stv (campaign_id_string) AS (
  VALUES
    ('22222')
),
combined AS (
  SELECT
    campaign_id_string,
    'Display' AS campaign_category
  FROM
    display
  UNION ALL
  SELECT
    campaign_id_string,
    'Streaming TV' AS campaign_category
  FROM
    stv
),
imp AS (
  SELECT
    i.user_id,
    -- OPTIMIZED 1A START: Improved aggregation with one record per user_id
    COUNT(DISTINCT cmb.campaign_category) AS campaign_category,
    -- OPTIMIZED 1A END
    SUM(impressions) AS impressions
  FROM
    dsp_impressions i
    INNER JOIN combined cmb ON i.campaign_id_string = cmb.campaign_id_string
  WHERE
    user_id IS NOT NULL
  GROUP BY
    1
),
click AS (
  SELECT
    c.user_id,
    -- OPTIMIZED 1B START: Improved aggregation with one record per user_id
    COUNT(DISTINCT cmb.campaign_category) AS campaign_category,
    -- OPTIMIZED 1B END
    SUM(clicks) AS clicks
  FROM
    dsp_clicks c
    INNER JOIN combined cmb ON c.campaign_id_string = cmb.campaign_id_string
  WHERE
    user_id IS NOT NULL
  GROUP BY
    1
),
-- OPTIMIZED 2 START: Consolidated to a single CTE
combined_user AS (
  SELECT
    user_id
  FROM
    imp
  WHERE
    campaign_category = 2
  GROUP BY
    1
  UNION ALL
  SELECT
    user_id
  FROM
    click
  WHERE
    campaign_category = 2
  GROUP BY
    1
),
-- OPTIMIZED 2 END
pre AS (
  SELECT
    i.user_id,
    SUM(impressions) AS impressions,
    0 AS clicks
  FROM
    imp i
    INNER JOIN combined_user cu ON cu.user_id = i.user_id
  GROUP BY
    1
  UNION ALL
  SELECT
    c.user_id,
    0 AS impressions,
    SUM(clicks) AS clicks
  FROM
    click c
    INNER JOIN combined_user cu ON cu.user_id = c.user_id
  GROUP BY
    1
)
SELECT
  COUNT(DISTINCT user_id) AS reach,
  SUM(impressions) AS impressions,
  SUM(clicks) AS clicks
FROM
  pre
```
</details>

### Non-restrictive predicates

Filtering within a query’s predicates (WHERE, HAVING, and JOIN conditions) reduces result set row counts and can significantly improve query performance when utilized. CTEs should be filtered using predicate clauses to only the relevant records before they are related to other result sets.

#### Things to look for

* Queries/CTEs that lack a WHERE clause
* WHERE clauses that do not effectively filter rows (e.g. `WHERE campaign is NOT NULL`).
* JOIN conditions which are always TRUE (e.g. `ON 1=1`)
* HAVING conditions that do not reference an aggregate

#### Solutions

* **Determine whether a WHERE clause can be added to the query/CTE.** A WHERE clause will reduce the size of its result set by filtering to only the relevant records.
* **Adjust ineffective/inefficient predicate clauses.** Use the most restrictive filters possible to meaningfully limit the size of the result set to only the relevant records.
* **Predicates should be added at the earliest possible stage of the query.** Optimizing filters has compounding benefits on the joins and aggregations later in the query by reducing cached record volume.

#### Query example

Continuing on with the same query as used in the previous examples, let’s now consider how you can reduce the record volume as early as possible in the query. Additional areas for optimization in the code are depicted below.

The `combined_user` CTE filters to the `relevant user_id` values, and the pre CTE gathers metrics; metrics are then limited to relevant `user_id` values with an INNER JOIN. While this is more efficient than previous versions of the query, you can still do more to reduce the cached record volume at earliest possible opportunity in the query. A more efficient option would be to combine the `combined_user` and pre CTEs, gathering relevant `user_id` values and associated metrics in a single step.

<details class="details-bar">
  <summary><b>Click to see: Non-optimized example</b></summary> 
  
```
WITH display (campaign_id_string) AS (
  VALUES
    ('11111')
),
stv(campaign_id_string) AS (
  VALUES
    ('22222')
),
combined AS (
  SELECT
    campaign_id_string,
    'Display' AS campaign_category
  FROM
    display
  UNION ALL
  SELECT
    campaign_id_string,
    'Streaming TV' AS campaign_category
  FROM
    stv
),
imp AS (
  SELECT
    i.user_id,
    COUNT(DISTINCT cmb.campaign_category) AS campaign_category,
    SUM(impressions) AS impressions
  FROM
    dsp_impressions i
    INNER JOIN combined cmb ON i.campaign_id_string = cmb.campaign_id_string
  WHERE
    user_id IS NOT NULL
  GROUP BY
    1
),
click AS (
  SELECT
    c.user_id,
    COUNT(DISTINCT cmb.campaign_category) AS campaign_category,
    SUM(clicks) AS clicks
  FROM
    dsp_clicks c
    INNER JOIN combined cmb ON c.campaign_id_string = cmb.campaign_id_string
  WHERE
    user_id IS NOT NULL
  GROUP BY
    1
),
-- OPTIMIZATION OPPORTUNITY 1 START: Combine user_id filtering and gathering of metrics
combined_user AS (
  SELECT
    user_id
  FROM
    imp
  WHERE
    campaign_category = 2
  GROUP BY
    1
  UNION ALL
  SELECT
    user_id
  FROM
    click
  WHERE
    campaign_category = 2
  GROUP BY
    1
),
pre AS (
  SELECT
    i.user_id,
    SUM(impressions) AS impressions,
    0 AS clicks
  FROM
    imp i
    INNER JOIN combined_user cu ON cu.user_id = i.user_id
  GROUP BY
    1
  UNION ALL
  SELECT
    c.user_id,
    0 AS impressions,
    SUM(clicks) AS clicks
  FROM
    click c
    INNER JOIN combined_user cu ON cu.user_id = c.user_id
  GROUP BY
    1
) -- OPTIMIZATION OPPORTUNITY 1 END
SELECT
  COUNT(DISTINCT user_id) AS reach,
  SUM(impressions) AS impressions,
  SUM(clicks) AS clicks
FROM
  pre
```
</details>

Below is the final optimized version of the query, which gathers relevant user_id values and associated metrics in a single CTE, rather than using multiple CTEs.

<details class="details-bar">
  <summary><b>Click to see: Optimized example</b></summary> 
  
```
WITH display (campaign_id_string) AS (
  VALUES
    ('11111')
),
stv(campaign_id_string) AS (
  VALUES
    ('22222')
),
combined AS (
  SELECT
    campaign_id_string,
    'Display' AS campaign_category
  FROM
    display
  UNION ALL
  SELECT
    campaign_id_string,
    'Streaming TV' AS campaign_category
  FROM
    stv
),
imp AS (
  SELECT
    i.user_id,
    COUNT(DISTINCT cmb.campaign_category) AS campaign_category,
    SUM(impressions) AS impressions
  FROM
    dsp_impressions i
    INNER JOIN combined cmb ON i.campaign_id_string = cmb.campaign_id_string
  WHERE
    user_id IS NOT NULL
  GROUP BY
    1
),
click AS (
  SELECT
    c.user_id,
    COUNT(DISTINCT cmb.campaign_category) AS campaign_category,
    SUM(clicks) AS clicks
  FROM
    dsp_clicks c
    INNER JOIN combined cmb ON c.campaign_id_string = cmb.campaign_id_string
  WHERE
    user_id IS NOT NULL
  GROUP BY
    1
),
-- OPTIMIZED 1 START: Consolidated to a single CTE
combined_user AS (
  SELECT
    user_id,
    SUM(impressions) AS impressions,
    0 AS clicks
  FROM
    imp
  WHERE
    campaign_category = 2
  GROUP BY
    1
  UNION ALL
  SELECT
    user_id,
    0 AS impressions,
    SUM(clicks) AS clicks
  FROM
    click
  WHERE
    campaign_category = 2
  GROUP BY
    1
) -- OPTIMIZED 1 END
SELECT
  COUNT(DISTINCT user_id) AS reach,
  SUM(impressions) AS impressions,
  SUM(clicks) AS clicks
FROM
  combined_user
```
</details>

## Getting more help

Are your queries still timing out?

- Have you tried optimizing the query using techniques listed above?

- If so, have you shortened the time window the query is running for? Try reducing it from monthly to 2 weeks, and then 1 week if needed.

- If you've optimized and reduced the date range but it's still timing out, submit a [support ticket](https://advertising.amazon.com/API/docs/en-us/support/overview#amazon-marketing-cloud-amc) with your instance ID, the SQL ID, screenshots of the error, and the query itself.


