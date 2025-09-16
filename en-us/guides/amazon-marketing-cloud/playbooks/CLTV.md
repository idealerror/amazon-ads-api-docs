---
title: Amazon Marketing Cloud - customer long-term value
description: Customer long-term value
type: guide
interface: api
---



# Customer long-term value

Customer long-term value or customer lifetime value (CLTV) assesses customer value over the course of the brand relationship.

The granular datasets that made calculating CLTV straightforward are disappearing as privacy takes the forefront of the discussion around consumer data. With AMC, advertisers can still measure advertising activity paired with conversions at a broader scale while maintaining a privacy safe environment.

The metric aims to couple all costs to acquire and retain with customer revenue from purchases. Offsetting cost and revenue reveals the estimated net profit a customer will bring to a company over time. With this metric, CLTV can be used as a north star for a number of actionable outcomes.

Use the formula below to calculate CLTV in the most basic form:

```
CLTV = (customer revenue) — (the cost of acquiring and retaining that customer)
```

### Benefits of CLTV over Return on Ad spend

Return on ad spend (ROAS) and CLTV are both important metrics to tell a comprehensive story of digital media performance, but ultimately serve different purposes. ROAS is more focused on measuring the performance of a specific tactic in driving revenue. CLTV focuses on the holistic revenue generated from a customer over the lifetime of a company. 

Below are some reasons to consider CLTV as a preferred metric over ROAS for measuring digital media performance. 

| CLTV                                                         | ROAS                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Focuses on the long-term value of a customer and hence  provides a comprehensive view to the company over time. | Measures the short-term impact of a specific advertising  campaign. |
| Considers both ad-attributed conversions and  non-ad-attributed conversions for a full picture of the user journey. | Only considers revenue and conversions that were  ad-attributed. |
| Considers the customers’ repeat purchase and  potential to purchase additional products around the brand halo. | Only considers revenue generated from initial sale.          |
| Provides a more accurate measure of profitability,  taking into account the cost to acquire a customer, cost to re retain a  customer, and fixed costs to consider like production fees and shipping,  which can spread over a long period of time. | Does not factor the cost of acquisition, which can lead to  inaccurate profitability calculations. |

CLTV allows for more targeted advertising strategies that focus on customer retention and loyalty, which can lead to a more profitable business model over time. 

This playbook will answer the following questions:

- How to define cost and revenue inputs that factor into CLTV?
- How to analyze customer base using CLTV?
- How to segment audiences based on CLTV measure?
- How to activate upon audience segmentation with Audience activation in Amazon demand-side platform (Amazon DSP)?

![CLTV_Answers](/_images/amazon-marketing-cloud/playbooks/CLTV/CLTV_1_Flowchart.jpg)

### Prerequisites

It is recommended you understand the following concepts to get the most out of the playbook:

#### Technical concepts

- **AMC concepts**: Specifically, concepts such as data aggregation thresholds, privacy limitations, and other restrictions listed in the [AMC documentation](guides/amazon-marketing-cloud/overview).
- **Knowledge of [AMC SQL](guides/amazon-marketing-cloud/amc-sql/overview):**  You can also [access the SQL Guide on AMC UI](https://advertising.amazon.com/marketing-cloud/instructional-queries/f53b1ad1359ef811fcdccdc8c6e69422b83bf06a163da53fddf79b6ca729edb6). 

  >[NOTE] The process outlined here relies on altering SQL code to match your purposes.
- **Basic Python**: This playbook is built using Python, and may require some scripting to adapt it to your use case.
- **Basic AWS knowledge:** Amazon SageMaker, and Amazon Athena in specific. You will need to know how to set up an Amazon SageMaker Notebook in accordance with your company's standards.
- **Basic Amazon DSP** knowledge: To activate upon CLTV Audience Segmentation in the advertising market, you’ll need to know how to query audiences to activate upon in Amazon DSP. 

####  Assumptions

For this playbook, we assume the following:

- **Long-term**: Since AMC only allows the maximum long-term window of one year, for the purposes of this playbook, the term "long-term" shall be defined as a one-year timeframe. Some customer relationships will be abbreviated by this window length. However, see [CLTV audience segmentation - year-over-year](#cltv-audience-segmentation-year-over-year) for a predictive approach to CLTV.
- **Revenue/costs inputs**: It is up to your discretion to define both the [revenue and cost models](#identify-inputs-to-calculate-cltv). 

>[NOTE] We recommend table mapping of Amazon Standard Identification Number (ASIN)s to broader product categories.

####  Recommended: Shopping insights datasets

Incorporating Paid features shopping insights for a complete revenue measurement adds conversions that were not driven by advertising, including Subscribe and Save (SnS) purchases for applicable brands. While CLTV can be calculated and activated upon as a rules-based audience without requiring a subscription to Paid Features, measuring CLTV on only advertising-attributed revenue streams does not tell a full story.  

### Playbook steps

The playbook is structured in the following order:

1.  [Identify inputs to calculate CLTV](#identifying-inputs-to-calculate-cltv)
2.  [Query data from AMC for CLTV](#query-data-from-amc-for-cltv)
3.  [Analyze audience value](#analyze-audience-value) and [activate audiences in ADSP](#activate-audiences)

## Identify inputs to calculate CLTV

The section below outlines a general approach to defining the inputs to calculate CLTV across advertisers and their sub-brands at the user_id level

- Cost/revenue inputs 
- Duration of relationship 
- Overall long-term value distribution 
- Audience segment structure 

### Revenue and cost inputs

Within CLTV inputs, leverage a series of value levers from cost and revenue value drivers. For each lever, identify a KPI and method for measurement and optimization within each to inspire use cases and analyses coming from CLTV. The levers and KPIs are outlined below:

![Cost_Revenue_inputs](/_images/amazon-marketing-cloud/playbooks/CLTV/CLTV_2_cost_revenue.png)

- **Revenue**
  - Increase duration of the relationship: Average customer long-term value
  - Increase upsell/cross-sell: Average purchase revenue
  - Increase Buy Frequency: Purchases Per customer per month
- **Cost** 
  - Optimize awareness/advertising costs: Spend per customer towards driving video views, page views, impressions.
  - Optimize consideration advertising costs: Spend per customer towards detail page views, site visits.
  - Optimize purchase advertising costs: Spend per customer towards driving conversions, sales, purchases.
  - Optimize retention advertising costs: Spend per customer towards driving subscriptions, repeat Purchases.

Shopping insights signals should be included at the revenue level to provide a comprehensive picture of revenue garnered from each customer. This data differentiates organic customers from "near-customers" who are not in the market yet.

The inputs above give a clear view of how long a customer has had a relationship with the brand, the average revenue garnered from all purchases, and the frequency with which the purchases take place. This is just one approach, however, as there are many different ways to calculate CLTV based on the definition of value for a brand. See below for a way to measure and define CLTV, and common use cases.

### Duration of relationship

The final input for calculating CLTV is the period of time used to account for revenue and costs. Currently, AMC supports CLTV queries of up to a year, which provides context to how customers return and interact with a brand over a year. Determining this input is based on the time frame in which you run the forthcoming SQL queries.

#### SQL output

Visually, the inputs of revenue and costs combine to create an ongoing story of the current and future ad spend and usage behavior.

![CLTV Duration](/_images/amazon-marketing-cloud/playbooks/CLTV/CLTV_3.png)


The figure above is a representative example of how customer value (represented by the black curve in the illustration above) changes over time for a customer. Initially, advertising is spent to generate awareness and consideration of the customer, until the customer makes their first purchase. Over time, the customer purchases additional products adding to the revenue streams, with advertising costs going down to focus on retargeting and acquisition, causing the customer to become high-value.

### Audience segmentation structure

As you determine your revenue and cost inputs to calculate CLTV, you will also need to define a criterion to segment customers over their relative value. This, using the long-term profitability CLTV approach, will bucket audiences into groups of related revenue/cost potential to query and activate upon in the Amazon DSP. 

![CLTV Duration](/_images/amazon-marketing-cloud/playbooks/CLTV/CLTV_4_seg_structure.png)

A simple way to do so is to first calculate the average cost and revenue generated from each audience. Then label each audience member based on their corresponding location in the grid above, with high-revenue/low-cost customers having the highest value. 

Below is an outline of each segment, their definition, and strategy to approach each (in this case, High = Above Average):

- **Low-value customers (high-cost/low-revenue):** Low in revenue and high-cost audiences being exposed to a higher value of media spend offset with the value of revenue they generate. This set of audience is considered “low value.” 
  - **Potential Action:** Redirect advertising to a more valuable audience or adjust maximum bids when serving these audiences.
- **Growth potential customers (low-cost/low-revenue):** Low in revenue and low in cost audiences are under-reached with a below average revenue. This set of audience is considered “low-value”, but they have some growth potential if promoted with the right product.
  - **Potential Action:** Increase spend to these audiences with discount introductory offers to see if anything causes them to stick with the brand. If doing so does not generate an uptick in revenue, then these customers will move to low-value over time.
- **Revenue potential customers (high-cost/high-revenue):** High in cost and high in revenue audiences being served an above average amount in advertising spend while also generating an above average amount of revenue compared to the entire customer base. 
  - **Potential Action:** Promote subscriptions to them as the likelihood of continued revenue generation without additional ad spend is high. Explore the attribution stream to eliminate waste.
- **High-value customers (low-cost/high-revenue):** Customers who are high-revenue and low-cost are considered the highest value customers to the brand.
  - **Potential Action**: Study these audiences to garner insights to understand the makeup of a high-value audience and attempt to push the other audience segments in this direction.

CLTV is something that can be customized on a per-advertiser basis using the inputs that are important to the advertisers when defining customer value. Even the audience labels can utilize medians, percentiles and standard deviations to define audience segments based on their overall distribution. 

## Query data from AMC for CLTV

To calculate CLTV, you will need to make some decisions on [revenue and cost inputs at the user-level](#revenue-and-cost-inputs). These decisions will depend on the analysis objectives as well as the individual brand’s definition of value.

**At a high level, the SQL script below will:**

- **Pull ad data from the following source tables in AMC**: Amazon DSP, Sponsored Display and Sponsored Product. For this case, the default level of aggregation will be set to the user.
- **Pull conversion data and metrics from tables in AMC**: Conversions and the paid features data set conversions\_non\_ad\_attributed for Shopping insights.
- **Combine data at a user-level** for a fully-aggregated descriptive statistics and audience segmentation.

| customer\_audience\_segment         | num\_users | total\_sales | total\_spend | cltv    |
|-----------------------------------|-----------|-------------|-------------|---------|
| Higher Revenue Potential Customer | 14,514    | \$18.57     | \$5.41      | \$13.16 |
| High Growth Potential Customer    | 14,180    | \$9.53      | \$5.64      | \$3.89  |
| Low Value Customer                | 607,585   | \$9.30      | \$4.11      | \$5.18  |
| High Value Customer               | 3,133,429 | \$14.21     | \$2.76      | \$11.45 |

The table above gives an example of the expected output from running an audience segmentation query across multiple months for a brand, with the number of audiences for each segment, total sales, total spend in advertising, and average CLTV.

### A simplified version of the data flow diagram is: 

![CLTV DFD](/_images/amazon-marketing-cloud/playbooks/CLTV/CLTV_5_simple_data_flow.jpg)

#### Instructional Query: Customer value - average spending by exposure group

Reference this instructional query for more details about assessing customer value based on groups of advertising exposure vs unexposed advertising: [AMC Instructional Query:Customer Value - Average Spending by Exposure Group](https://advertising.amazon.com/marketing-cloud/instructional-queries/39d9dc61cb350797e022dfd0c9a77470ff37792b6673163808ed72b083cd941a)(link only available for AMC UI users).

### User input and decisions

-  **Level of aggregation**: CLTV is aggregated at different levels to build a narrative around which products, sub brands and campaigns drive customer value over time. For Audience segmentation, the level of aggregation will be each of the four segments for each audience.
-  **Duration of CLTV**: It is recommended to ensure the query includes a year’s worth of data. The entire duration must be included in one query, or you not be able to match customers who purchased in January 2022 with customers who purchased in December 2022.
-  **Relevant revenue and cost metrics**  In the example below, we will use: total\_product\_sales, total\_units\_sold, spend, total\_cost, sns\_purchase, and new\_to\_brand.

### How to execute the SQL scripts?

The script can be executed in two ways:

- Run manually in the [AMC UI](https://advertising.amazon.com/marketing-cloud)(link only available for AMC UI users). 
- Run via [AMC API](guides/amazon-marketing-cloud/reporting/workflow-management-service) - More advanced solution, recommended when building an automated data pipeline.

### AMC query for aggregated CLTV and audience segmentation

The query requires that you combine all the spend and revenue inputs and then calculate CLTV within the query, then you define an audience segmentation method at the user level. The output of this SQL statement will be a .csv that illustrates the impact of new-to-brand and subscription purchase flags to revenue, cost, and CLTV. 

Listed below are the formulas used in the query.

- **`tracked_asin`** = The purchase ASIN number for the product associated with the CLTV figure
- **`avg_purchase_frequency_ratio`** = The customer's average purchase frequency on this particular ASIN/the total average purchase frequency of all ASINs
- **`avg_purchase_quantity_ratio`** = The customer's average purchase quantity of this particular ASIN/the average purchase quantity across all ASINs
- **`avg_user_lifetime_ratio`** = The customer's average purchase lifetime for this particular ASIN/the average purchase lifetime across all ASINs
- **`total_sales`** = Total revenue generated from sales
- **`total_units_sold`** = Total units sold from the sales
- **`dsp_spend`** = Advertising spend associated with Amazon DSP advertising
- **`sponsored_spend`** = Total spend associated with sponsored products advertising
- **`total_spend`** = Combination of Amazon DSP spend and sponsored spend
- **`avg_value_score`** = Net return value generated from total sales - total cost as a function of overall cost. 
  The goal of this measure is to present a ratio of CLTV to each dollar spent towards the customer. 
- **`Avg_lifetime_score`** = Product of the purchase quantity ratio, purchase frequency ratio, and user lifetime ratio that denotes the overall relationship flags that users can input into the query.
- **`CLTV`** = Net return value generated from total sales - total cost
  ​	(includes minimum, maximum, total counts, and average value of CLTV within the group by)

**Lifetime\*Value CLTV SQL export script:** 

Based on our framework of table inputs, we can calculate CLTV using the logic below. Note that within the CLTV calculation, we will be incorporating purchase quantity, purchase frequency, and user lifetime per user as the ratio of all users to provide a weight to the overall profitability measure. This factors in the overall customer loyalty to the brand into the measure of overall customer value.

![CLTV Lifetime value](/_images/amazon-marketing-cloud/playbooks/CLTV/CLTV_6_lifetimescore.png)

The query to generate this can be found below:

<details class="details-bar">
  <summary>Click to see: CLTV SQL export script</summary> 
  
```
With user_ntb_cte AS (  
    -- Gets the purchase details per order for individual customer  
    SELECT  
       user_id  
       , tracked_asin  
       , total_sales  
       , total_units_sold  
       , conversion_frequency  
       , min_date  
       , max_date  
       , round(SECONDS_BETWEEN(min_date,max_date) / 60 / 60 / 24 / 30) as cust_lifetime  
    from (  
            SELECT  
               user_id  
               , tracked_asin  
               , sum(total_product_sales) as total_sales  
               , sum(total_units_sold) as total_units_sold  
               , sum(conversions) as conversion_frequency  
               , min(case when event_subtype = 'order' then event_date_utc else null end) as min_date  
               , max(case when event_subtype = 'order' then event_date_utc else null end) as max_date  
            from conversions_all  
            where event_subtype = 'order' and user_id is not null  
            group by 1, 2  
    )  
    where user_id is not null  
),  
user_lifetime AS (  
    SELECT ntb.user_id  
    , ntb.tracked_asin  
    , ntb.min_date  
    , ntb.max_date  
    , ntb.total_sales  
    , ntb.total_units_sold  
    , ntb.conversion_frequency  
    , ntb.cust_lifetime  
    , ntb.total_units_sold / (avg(ntb.total_units_sold) OVER ()) as purchase_quantity_ratio  
    , ntb.conversion_frequency / (avg(ntb.conversion_frequency) OVER ()) as purchase_frequency_ratio  
    , ntb.cust_lifetime / (avg(ntb.cust_lifetime) OVER ()) as user_lifetime_ratio  
    FROM user_ntb_cte ntb  
),  
user_cost_cte AS (  
    SELECT user_id  
    , sum(dsp_spend) AS dsp_spend  
    , sum(sponsored_spend) as sponsored_spend  
    , sum(total_spend) as total_spend  
    FROM  (  
        SELECT user_id  
        , 0 as dsp_spend  
        , sum(spend)/ 100000000 as sponsored_spend  
        , sum(spend)/ 100000000 as total_spend  
        from sponsored_ads_traffic  
        where clicks > 0  
        group by 1  
    UNION ALL  
        SELECT user_id  
        , sum(total_cost) / 100000 as dsp_spend  
        , 0 as sponsored_spend  
        , sum(total_cost) / 100000  as total_spend  
        from dsp_impressions  
        where impressions > 0  
        group by 1  
    )  
    group by 1  
),  
value_cte AS (  
    SELECT r.user_id  
    , r.tracked_asin  
    , r.total_sales  
    , r.total_units_sold  
    , r.purchase_quantity_ratio  
    , r.conversion_frequency  
    , r.purchase_frequency_ratio  
    , r.cust_lifetime  
    , r.user_lifetime_ratio  
    , c.dsp_spend  
    , c.sponsored_spend  
    , c.total_spend  
    , ((r.purchase_frequency_ratio) * (r.purchase_quantity_ratio) * (r.user_lifetime_ratio)) as lifetime_score  
    , (r.total_sales - c.total_spend)/c.total_spend AS value_score  
    FROM user_lifetime r  
    LEFT JOIN user_cost_cte c  
        ON c.user_id = r.user_id  
)  
SELECT  
    tracked_asin  
    , avg(purchase_frequency_ratio) as avg_purchase_frequency_ratio  
    , avg(purchase_quantity_ratio) as avg_purchase_quantity_ratio  
    , avg(user_lifetime_ratio) as avg_user_lifetime_ratio  
    , avg(total_sales) as total_sales  
    , sum(total_units_sold) as total_units_sold  
    , sum(dsp_spend) as dsp_spend  
    , sum(sponsored_spend) as sponsored_spend  
    , avg(total_spend) as total_spend  
    , avg(lifetime_score) as avg_lifetime_score  
    , avg(value_score) as avg_value_score  
    , avg(lifetime_score) * avg(value_score) as CLTV  
FROM value_cte  
group by 1
```
</details>

The output for the above query is below:

tracked\_asin | avg\_purchase\_frequency\_ratio | avg\_purchase\_quantity\_ratio | avg\_user\_lifetime\_ratio | total\_sales | total\_units\_sold | dsp\_spend | sponsored\_spend | total\_spend | avg\_lifetime\_score | avg\_value\_score | cltv 
---|---|---|---|---|---|---|---|---|---|---|---
 ASIN106 | 0\.90005 | 6\.4006 | 0\.4028 | 29\.28511 | 719 | 19\.06598 | 1075\.72099 | 4\.63893 | 10\.58231 | 28\.54052 | 302\.02471 
 ASIN16 | 0\.99894 | 0\.23583 | 0\.98362 | 1\.07902 | 105 | 78\.17999 | 3619\.84575 | 4\.09527 | 0\.5514 | 0\.02214 | 0\.01221 
 ASIN17 | 1\.01766 | 0\.21269 | 1\.15489 | 1\.34161 | 100 | 80\.36274 | 3934\.53073 | 4\.28484 | 0\.31147 | 0\.01755 | 0\.00547 
 ASIN19 | 1\.02007 | 0\.24874 | 1\.13519 | 2\.19689 | 108 | 77\.79985 | 3933\.32293 | 4\.43218 | 0\.29759 | 0\.97889 | 0\.29131 
 ASIN109 | 0\.89614 | 6\.60548 | 0\.34782 | 72\.95082 | 890 | 21\.2797 | 1274\.6853 | 4\.67857 | 7\.35227 | 86\.18254 | 633\.63744 
 ASIN9 | 1\.02526 | 0\.15709 | 1\.18469 | 0\.79261 | 69 | 81\.56003 | 3882\.72734 | 4\.37559 | 0\.25495 | 0\.51887 | 0\.13229 
 ASIN101 | 0\.88281 | 7\.19623 | 0\.2688 | 31\.76735 | 822 | 19\.46951 | 1066\.68449 | 4\.52564 | 7\.54124 | 31\.05173 | 234\.16868 
 ASIN4 | 0\.99726 | 0\.23183 | 0\.95159 | 2\.63277 | 100 | 78\.23977 | 3859\.40919 | 4\.4443 | 0\.61908 | 2\.5571 | 1\.58304 
 ASIN108 | 0\.89268 | 6\.5773 | 0\.2575 | 58\.11173 | 743 | 17\.15976 | 1042\.21211 | 4\.60596 | 7\.39674 | 64\.31857 | 475\.74768 
 ASIN105 | 0\.90516 | 6\.92549 | 0\.307 | 41\.49858 | 802 | 18\.0322 | 1001\.73567 | 4\.45313 | 7\.28973 | 45\.46169 | 331\.40358 
 ASIN6 | 1\.00883 | 0\.21608 | 1\.16759 | 1\.77205 | 96 | 80\.97122 | 4090\.56061 | 4\.54415 | 1\.04699 | 1\.50151 | 1\.57207 

The above is an output that uses the **Lifetime Value** approach for measuring CLTV among users that purchase specific ASINs. This is an example of a use case to define which ASIN are associated with customer value over time, and provide detail over whether they tend to be purchased in large quantities fewer times vs more frequently. 

>[NOTE] In the above example, tracked ASIN was selected to depict the products to support. Instead, you can use audiences, campaigns, or tactics to understand which approach is associated with stronger customer loyalty over time. 

#### Audience segmentation (based on CLTV)

After defining the audience segmentation conditions and generating the CLTV using the query above, use the SQL snippet below gives user ids an audience label some descriptive statistics to measure the overall revenue and spend makeup of each. 

This is done by adding a temp table that uses case statements to define the segmentation labels for each user ID, and then aggregating all the audience segment labels. The case below uses the lifetime\*value approach to calculating CLTV.

![AudienceSegmentation-Lifetime](/_images/amazon-marketing-cloud/playbooks/CLTV/CLTV_7_Audience_Segmentation.png)

**`CLTV =  Lifetime * Value`**

The SQL query below looks at CLTV by ASIN over the span of the past year. It focuses on users who made a `new_to_brand` purchase during the window:

<details class="details-bar">
  <summary>Click to see: Script for purchase details per order for an individual user</summary> 

```
With user_ntb_cte AS (  
    -- Gets the purchase details per order for individual user  
    SELECT  
       user_id  
       , tracked_asin  
       , max(ntb_flag) as ntb_flag  
       , sum(total_sales) as total_sales  
       , sum(total_units_sold) as total_units_sold  
       , sum(conversion_frequency) as conversion_frequency  
       , min(min_date) as min_date  
       , max(max_date) as max_date  
       , round(SECONDS_BETWEEN(min(min_date),max(max_date)) / 60 / 60 / 24 / 30) as cust_lifetime  
    from (  
            SELECT  
               user_id  
               , tracked_asin  
               , max(case when event_subtype = 'order' and new_to_brand = TRUE then 1 else 0 end) as ntb_flag  
               , sum(total_product_sales) as total_sales  
               , sum(total_units_sold) as total_units_sold  
               , sum(conversions) as conversion_frequency  
               , min(case when event_subtype = 'order' then event_date_utc else null end) as min_date  
               , max(case when event_subtype = 'order' then event_date_utc else null end) as max_date  
            from conversions a  
            where event_subtype = 'order'  
            and user_id is not null  
            group by 1, 2  
              
        union all  
          
            SELECT  
               user_id  
               , tracked_asin  
               , max(case when event_subtype = 'order' and new_to_brand = TRUE then 1 else 0 end) as ntb_flag  
               , sum(total_product_sales) as total_sales  
               , sum(total_units_sold) as total_units_sold  
               , sum(conversions) as conversion_frequency  
               , min(case when event_subtype = 'order' then event_date_utc else null end) as min_date  
               , max(case when event_subtype = 'order' then event_date_utc else null end) as max_date  
            from conversions_non_ad_exposed  
            where event_subtype = 'order'  
            and user_id is not null  
            group by 1, 2  
    )  
    where user_id is not null  
    group by 1, 2  
),  
user_lifetime AS (  
    SELECT ntb.user_id  
    , ntb.tracked_asin  
    , ntb.ntb_flag  
    , ntb.min_date  
    , ntb.max_date  
    , ntb.total_sales  
    , ntb.total_units_sold  
    , ntb.conversion_frequency  
    , ntb.cust_lifetime  
    , ntb.total_units_sold / (avg(ntb.total_units_sold) OVER ()) as purchase_quantity_ratio  
    , ntb.conversion_frequency / (avg(ntb.conversion_frequency) OVER ()) as purchase_frequency_ratio  
    , ntb.cust_lifetime / (avg(ntb.cust_lifetime) OVER ()) as user_lifetime_ratio  
    FROM user_ntb_cte ntb  
    where ntb_flag = 1  
),  
user_cost_cte AS (  
    SELECT user_id  
    , sum(dsp_spend) AS dsp_spend  
    , sum(sponsored_spend) as sponsored_spend  
    , sum(total_spend) as total_spend  
    FROM  (  
        SELECT user_id  
        , 0 as dsp_spend  
        , sum(spend)/ 100000000 as sponsored_spend  
        , sum(spend)/ 100000000 as total_spend  
        from sponsored_ads_traffic  
        where clicks > 0  
        group by 1  
    UNION ALL  
        SELECT user_id  
        , sum(total_cost) / 100000 as dsp_spend  
        , 0 as sponsored_spend  
        , sum(total_cost) / 100000  as total_spend  
        from dsp_impressions  
        where impressions > 0  
        group by 1  
    )  
    group by 1  
),  
value_cte AS (  
    SELECT r.user_id  
    , r.tracked_asin  
    , r.ntb_flag  
    , r.total_sales  
    , r.total_units_sold  
    , r.purchase_quantity_ratio  
    , r.conversion_frequency  
    , r.purchase_frequency_ratio  
    , r.cust_lifetime  
    , r.user_lifetime_ratio  
    , c.dsp_spend  
    , c.sponsored_spend  
    , c.total_spend  
    , ((r.purchase_frequency_ratio) * (r.purchase_quantity_ratio) * (r.user_lifetime_ratio)) as lifetime_score  
    , (r.total_sales - c.total_spend)/c.total_spend AS value_score  
    FROM user_lifetime r  
    LEFT JOIN user_cost_cte c  
        ON c.user_id = r.user_id  
)  
SELECT  
    tracked_asin  
    , sum(ntb_flag) as ntb_customers  
    , avg(purchase_frequency_ratio) as avg_purchase_frequency_ratio  
    , avg(purchase_quantity_ratio) as avg_purchase_quantity_ratio  
    , avg(user_lifetime_ratio) as avg_user_lifetime_ratio  
    , avg(total_sales) as total_sales  
    , sum(total_units_sold) as total_units_sold  
    , sum(dsp_spend) as dsp_spend  
    , sum(sponsored_spend) as sponsored_spend  
    , avg(total_spend) as total_spend  
    , avg(lifetime_score) as avg_lifetime_score  
    , avg(value_score) as avg_value_score  
    , avg(lifetime_score) * avg(value_score) as CLTV  
FROM value_cte  
group by 1
```
</details>

The output for the above query is shown below:

tracked\_asin | ntb\_customers | avg\_purchase\_frequency\_ratio | avg\_purchase\_quantity\_ratio | avg\_user\_lifetime\_ratio | total\_sales | total\_units\_sold | dsp\_spend | sponsored\_spend | total\_spend | avg\_lifetime\_score | avg\_value\_score | cltv 
---|---|---|---|---|---|---|---|---|---|---|---|---
 ASIN110 | 229 | 0\.90598 | 4\.22092 | 0\.31045 | 108\.51528 | 497 | 12\.53398 | 526\.15056 | 3\.61533 | 4\.312 | 190\.64758 | 822\.07266 
 ASIN102 | 204 | 0\.91081 | 4\.15664 | 0\.3236 | 59\.82177 | 436 | 10\.95122 | 502\.1579 | 3\.80081 | 8\.47076 | 91\.57633 | 775\.72093 
 ASIN107 | 226 | 0\.92169 | 3\.90691 | 0\.58419 | 50\.22124 | 454 | 10\.94916 | 630\.86386 | 4\.51981 | 9\.8672 | 70\.93478 | 699\.92777 
 ASIN109 | 234 | 0\.90442 | 4\.3302 | 0\.39062 | 77\.92735 | 521 | 12\.28159 | 624\.76608 | 4\.13667 | 5\.41549 | 114\.28147 | 618\.89047 
 ASIN104 | 221 | 0\.88976 | 3\.9689 | 0\.41359 | 72\.4457 | 451 | 12\.15037 | 515\.75977 | 3\.71768 | 5\.21277 | 98\.23192 | 512\.06079 
 ASIN103 | 210 | 0\.87685 | 4\.06566 | 0\.24181 | 104\.50291 | 439 | 12\.16814 | 532\.28894 | 3\.8614 | 3\.76171 | 130\.63422 | 491\.40778 
 ASIN106 | 197 | 0\.90934 | 3\.9588 | 0\.48976 | 29\.51523 | 401 | 11\.83649 | 525\.2571 | 4\.09995 | 7\.56041 | 34\.13261 | 258\.05662 
 ASIN108 | 221 | 0\.88599 | 4\.1097 | 0\.18382 | 59\.16742 | 467 | 10\.90411 | 554\.05288 | 4\.03541 | 2\.86703 | 79\.68349 | 228\.45463 
 ASIN101 | 207 | 0\.88554 | 4\.46282 | 0\.31891 | 32\.10266 | 475 | 11\.83768 | 496\.87984 | 3\.74057 | 4\.21375 | 44\.98022 | 189\.53535 
 ASIN105 | 203 | 0\.92351 | 4\.44537 | 0\.30018 | 43\.40571 | 464 | 10\.91161 | 560\.51093 | 4\.46424 | 4\.29678 | 43\.98495 | 188\.99361 
 ASIN20 | 383 | 1\.03118 | 0\.24882 | 1\.10045 | 4\.98828 | 49 | 19\.89514 | 1015\.05011 | 4\.09069 | 1\.33207 | 5\.64511 | 7\.51966 
 ASIN8 | 420 | 1\.06532 | 0\.18522 | 1\.23323 | 4\.76095 | 40 | 22\.5383 | 999\.94346 | 3\.78697 | 0\.94043 | 2\.99599 | 2\.81751 
 ASIN6 | 408 | 1\.01701 | 0\.1716 | 1\.18238 | 2\.29324 | 36 | 22\.28696 | 1066\.49577 | 4\.28655 | 0\.72606 | 2\.60674 | 1\.89266 
 ASIN5 | 376 | 0\.96839 | 0\.25345 | 0\.85084 | 5\.27793 | 49 | 19\.70808 | 1053\.85824 | 4\.3819 | 0\.70032 | 2\.29752 | 1\.60899 
 ASIN12 | 373 | 1\.02532 | 0\.20335 | 1\.12996 | 4\.39037 | 39 | 18\.32958 | 1069\.41289 | 4\.64847 | 1\.12511 | 1\.10423 | 1\.24238 
 ASIN4 | 414 | 1\.00025 | 0\.12684 | 1\.11618 | 2\.34717 | 27 | 21\.42943 | 1150\.65383 | 4\.38982 | 0\.87455 | 1\.24665 | 1\.09026 
 ASIN1 | 370 | 1\.06966 | 0\.21551 | 1\.38616 | 1\.43943 | 41 | 20\.03547 | 977\.02747 | 4\.06964 | 1\.08975 | 0\.87438 | 0\.95286 
 ASIN10 | 334 | 1\.06022 | 0\.22127 | 1\.35312 | 1\.70545 | 38 | 18\.67069 | 946\.74329 | 4\.21578 | 0\.59129 | 1\.49816 | 0\.88584 
 ASIN2 | 414 | 1\.04051 | 0\.2114 | 1\.2511 | 2\.17391 | 45 | 20\.46847 | 1071\.01277 | 4\.1188 | 1\.37146 | 0\.59166 | 0\.81143 
 ASIN7 | 434 | 1\.01176 | 0\.11651 | 1\.07644 | 2\.39631 | 26 | 23\.53419 | 1111\.20751 | 4\.17184 | 0\.41712 | 0\.89135 | 0\.3718 
 ASIN19 | 385 | 1\.05179 | 0\.12124 | 1\.18706 | 1\.74483 | 24 | 19\.40021 | 1060\.51299 | 4\.37212 | 0\.08549 | 0\.27055 | 0\.02313 
 ASIN11 | 430 | 0\.97854 | 0\.08594 | 0\.92113 | 0\.50814 | 19 | 22\.59403 | 1157\.54525 | 4\.21478 | 0 | 1\.32518 | 0 
 ASIN17 | 400 | 1\.04984 | 0\.07293 | 1\.34567 | 0\.74963 | 15 | 19\.68967 | 989\.71182 | 4\.00556 | 0 | \-0\.82063 | 0 
 ASIN3 | 370 | 0\.9931 | 0\.14718 | 0\.93325 | 2\.27027 | 28 | 19\.08992 | 1017\.87713 | 4\.3207 | 0 | 6\.59037 | 0 
 ASIN9 | 439 | 1\.06097 | 0\.06645 | 1\.52687 | 0\.54636 | 15 | 23\.86525 | 1245\.53003 | 4\.58265 | 0\.03749 | \-0\.61179 | \-0\.02293 
 ASIN13 | 454 | 1\.03142 | 0\.14993 | 1\.27509 | 2\.92874 | 35 | 23\.71337 | 1158\.50661 | 4\.00753 | 0\.56187 | \-0\.16115 | \-0\.09055 
 ASIN16 | 419 | 1\.02014 | 0\.09747 | 1\.18769 | 0\.72673 | 21 | 20\.03227 | 985\.52624 | 3\.82342 | 0\.54989 | \-0\.33365 | \-0\.18347 
 ASIN18 | 411 | 1\.02377 | 0\.08518 | 1\.13668 | 1\.09489 | 18 | 19\.63217 | 1036\.51697 | 4\.15807 | 0\.32034 | \-0\.6607 | \-0\.21165 
 ASIN14 | 377 | 1\.02991 | 0\.10833 | 1\.17185 | 2\.67374 | 21 | 19\.73581 | 998\.03084 | 4\.15415 | 0\.45836 | \-0\.47717 | \-0\.21872 
 ASIN15 | 393 | 1\.00494 | 0\.15341 | 1\.07245 | 1\.02545 | 31 | 21\.63714 | 1056\.423 | 4\.22769 | 1\.34005 | \-0\.18819 | \-0\.25219 


Note that ASIN110 is associated with the largest customer long-term value among customers who purchased it as their new-to-brand purchase. This is followed by ASIN102, ASIN107, ASIN109, and ASIN104,  all with CLTV values of over 491.

To incorporate audience labeling, you can add the following to the last case statement:

<details class="details-bar">
  <summary>Click to see: additional SQL code to add audience labeling </summary> 
```
 case  
        when  
            (lifetime_score >= (avg(lifetime_score) OVER ()))  
            AND  
            (value_score >= (avg(value_score) OVER ()))  
            THEN 'High CLTV Customer'  
        when  
            (lifetime_score >= (avg(lifetime_score) OVER ()))  
            AND  
            (value_score < (avg(value_score) OVER ()))  
            THEN 'High Loyalty Customer'  
        when  
            (lifetime_score < (avg(lifetime_score) OVER ()))  
            AND  
            (value_score < (avg(value_score) OVER ()))  
            THEN 'Low CLTV Customer'  
        when  
            (lifetime_score < (avg(lifetime_score) OVER ()))  
            AND  
            (value_score >= (avg(value_score) OVER ()))  
            THEN 'High Revenue Potential Customer'  
        else 'Non-Labeled'  
        end as Customer_Audience_Segment
```
</details>

## Analyze audience value

After creating the query, the output now classifies `user_ids` based on their relative revenue and cost configuration into a segment that is based on their overall value to the brand. Before activating the audience, performing an exploratory data analysis in Python will help provide guidance on trends associated with these customer segments. 

### Audience label descriptive statistics

The first case to explore is the descriptive statistics of each audience segment as they relate to ASINs and the purchases that are new-to-brand. This is done by adding `tracked_asin` and a flag for `new-to-brand` in the initial query, and cascading it through the rest of the temp tables. 

<details class="details-bar">
  <summary>Click to see: SQL code for audience segment descriptive status </summary> 

```
With user_ntb_cte AS (  
    -- Gets the purchase details per order for individual customer  
    SELECT  
       user_id  
       , max(ntb_flag) as ntb_flag  
       , sum(total_sales) as total_sales  
       , sum(total_units_sold) as total_units_sold  
       , sum(conversion_frequency) as conversion_frequency  
       , min(min_date) as min_date  
       , max(max_date) as max_date  
       , round(SECONDS_BETWEEN(min(min_date),max(max_date)) / 60 / 60 / 24 / 30) as cust_lifetime  
    from (  
            SELECT  
               user_id  
               , max(case when event_subtype = 'order' and new_to_brand = TRUE then 1 else 0 end) as ntb_flag  
               , sum(total_product_sales) as total_sales  
               , sum(total_units_sold) as total_units_sold  
               , sum(conversions) as conversion_frequency  
               , min(case when event_subtype = 'order' then event_date_utc else null end) as min_date  
               , max(case when event_subtype = 'order' then event_date_utc else null end) as max_date  
            from conversions a  
            where event_subtype = 'order'  
            and total_product_sales > 0  
            and user_id is not null  
            group by 1  
              
        union all  
          
            SELECT  
               user_id  
               , max(case when event_subtype = 'order' and new_to_brand = TRUE then 1 else 0 end) as ntb_flag  
               , sum(total_product_sales) as total_sales  
               , sum(total_units_sold) as total_units_sold  
               , sum(conversions) as conversion_frequency  
               , min(case when event_subtype = 'order' then event_date_utc else null end) as min_date  
               , max(case when event_subtype = 'order' then event_date_utc else null end) as max_date  
            from conversions_non_ad_exposed  
            where event_subtype = 'order'  
            and user_id is not null  
            and total_product_sales > 0  
            group by 1  
    )  
    where user_id is not null  
    group by 1  
),  
user_lifetime AS (  
    SELECT ntb.user_id  
    , ntb.ntb_flag  
    , ntb.min_date  
    , ntb.max_date  
    , ntb.total_sales  
    , ntb.total_units_sold  
    , ntb.conversion_frequency  
    , ntb.cust_lifetime  
    , ntb.total_units_sold / (avg(ntb.total_units_sold) OVER ()) as purchase_quantity_ratio  
    , ntb.conversion_frequency / (avg(ntb.conversion_frequency) OVER ()) as purchase_frequency_ratio  
    , ntb.cust_lifetime / (avg(ntb.cust_lifetime) OVER ()) as user_lifetime_ratio  
    FROM user_ntb_cte ntb  
    where ntb_flag = 1  
),  
user_cost_cte AS (  
    SELECT user_id  
    , sum(dsp_spend) AS dsp_spend  
    , sum(sponsored_spend) as sponsored_spend  
    , sum(total_spend) as total_spend  
    FROM  (  
        SELECT user_id  
        , 0 as dsp_spend  
        , sum(spend)/ 100000000 as sponsored_spend  
        , sum(spend)/ 100000000 as total_spend  
        from sponsored_ads_traffic  
        where clicks > 0  
        group by 1  
    UNION ALL  
        SELECT user_id  
        , sum(total_cost) / 100000 as dsp_spend  
        , 0 as sponsored_spend  
        , sum(total_cost) / 100000  as total_spend  
        from dsp_impressions  
        where impressions > 0  
        group by 1  
    )  
    group by 1  
),  
value_cte AS (  
    SELECT r.user_id  
    , r.ntb_flag  
    , r.total_sales  
    , r.total_units_sold  
    , r.purchase_quantity_ratio  
    , r.conversion_frequency  
    , r.purchase_frequency_ratio  
    , r.cust_lifetime  
    , r.user_lifetime_ratio  
    , c.dsp_spend  
    , c.sponsored_spend  
    , c.total_spend  
    , ((r.purchase_frequency_ratio) * (r.purchase_quantity_ratio) * (r.user_lifetime_ratio)) as lifetime_score  
    , (r.total_sales - c.total_spend)/c.total_spend AS value_score  
    FROM user_lifetime r  
    LEFT JOIN user_cost_cte c  
        ON c.user_id = r.user_id  
)  
    SELECT  
    tracked_asin,  
    case  
        when  
            (lifetime_score >= (avg(lifetime_score) OVER ()))  
            AND  
            (value_score >= (avg(value_score) OVER ()))  
            THEN 'High CLTV Customer'  
        when  
            (lifetime_score >= (avg(lifetime_score) OVER ()))  
            AND  
            (value_score < (avg(value_score) OVER ()))  
            THEN 'High Loyalty Customer'  
        when  
            (lifetime_score < (avg(lifetime_score) OVER ()))  
            AND  
            (value_score < (avg(value_score) OVER ()))  
            THEN 'Low CLTV Customer'  
        when  
            (lifetime_score < (avg(lifetime_score) OVER ()))  
            AND  
            (value_score >= (avg(value_score) OVER ()))  
            THEN 'High Revenue Potential Customer'  
        else 'Non-Labeled'  
        end as Customer_Audience_Segment  
    , count(distinct user_id) as users  
    , avg(purchase_frequency_ratio) as purchase_frequency_ratio  
    , avg(purchase_quantity_ratio) as purchase_quantity_ratio  
    , avg(user_lifetime_ratio) as user_lifetime_ratio  
    , sum(total_sales) as total_sales  
    , sum(total_units_sold) as total_units_sold  
    , sum(total_spend) as total_spend  
    , avg(lifetime_score) * avg(value_score) as CLTV  
    from value_cte  
    group by 1, 2
```
</details>

Given below is the output for the above query:

tracked\_asin | customer\_audience\_segment | users | purchase\_frequency\_ratio | purchase\_quantity\_ratio | user\_lifetime\_ratio | total\_sales | total\_units\_sold | total\_spend | cltv 
---|---|---|---|---|---|---|---|---|---
 ASIN106 | Low CLTV Customer | 598 | 0\.75862 | 0\.72043 | 0\.55123 | 49041\.76 | 1666 | 2814\.3774 | 23\.19514 
 ASIN109 | High CLTV Customer | 67 | 3\.14752 | 3\.25364 | 3\.26482 | 25319\.98 | 843 | 53\.35012 | 55778\.9948 
 ASIN106 | High Revenue Potential Customer | 109 | 0\.92017 | 0\.94896 | 0\.66894 | 13226\.19 | 400 | 26\.83654 | 668\.50701 
 ASIN106 | High Loyalty Customer | 111 | 2\.34006 | 2\.37626 | 3\.24337 | 30508\.77 | 1020 | 685\.11214 | 1340\.59071 
 ASIN109 | Low CLTV Customer | 594 | 0\.33023 | 1\.91211 | 0\.81218 | 19201\.91 | 1991 | 1919\.191 | 34\.9101 
 ASIN109 | High CLTV Customer | 94 | 3\.3222 | 2\.02102 | 2\.90121 | 19291\.12 | 782 | 92\.90121 | 129911\.91021 
 ASIN109 | High Revenue Potential Customer | 202 | 0\.2392 | 0\.98232 | 0\.72372 | 12011\.1212 | 1201 | 34\.19022 | 782\.92023 
 ASIN109 | High Loyalty Customer | 112 | 2\.19201 | 2\.12029 | 3\.32323 | 129011 | 1202 | 723\.29 | 1402\.01212 


### Audience label analysis

Comparing different audience segments, the code block below compares audience performance. The code outputs a graph in Python that groups all the data by the top CLTV customers and High Loyalty customers based on the sum of the lifetime and value scores to calculate CLTV.

<details class="details-bar">
  <summary>Click to see: Python code to compare audience performance </summary> 

```
#read_from = [os.path.join(data_dir, x) for x in os.listdir(data_dir) if re.search(f"/data/20220823_advertiser_abc.csv", x)].pop()  
df3 = pd.read_csv(f"asin_audsegment.csv")  
df3 = df3[df3['cltv']> 0]  

fig = px.histogram(df3, x="tracked_asin", y="cltv",  
             color='customer_audience_segment', barmode='group',  
             height=400)  
fig.show()
```
</details>

![audience_label_analysis](/_images/amazon-marketing-cloud/playbooks/CLTV/CLTV_8_audience_label.png)

If you look at the CLTV grouped by tracked ASIN and audience segment, note that ASIN109, ASIN102 and ASIN104 show the highest overall CLTV while ASIN12, ASIN20, and ASIN15 show a higher CLTV among High Loyalty Customers. Since this is exclusively focused on new-to-brand customers, we could focus on the following ASINs to attract **High CLTV new-to-brand customers**.

- ASIN 109
- ASIN 102
- ASIN 104

#### **Python code - Audience scatter (using Sandbox data)** 

>[WARNING] Since AMC restricts querying individual user IDs, this query works only with Sandbox data. This chart below will illustrate the distribution of lifetime and value customers. 

<details class="details-bar">
  <summary>Click to see: Python code to output Audience scatter </summary> 

```
#read_from = [os.path.join(data_dir, x) for x in os.listdir(data_dir) if re.search(f"path/data/20220823_advertiser_abc.csv", x)].pop()  
df = pd.read_csv(f"lifetimevalue_audiencelabels.csv")  

import plotly.express as px  
fig = px.scatter(df, x="total_sales", y="total_spend", color="customer_audience_segment",  
                 hover_data=['cltv'])  

fig.update_layout(legend=dict(  
    yanchor="top",  
    y=0.99,  
    xanchor="right",  
    x=0.99  
))  

fig.update_xaxes(range=[0, 500])  
fig.update_yaxes(range=[0, 10])  

fig.show('notebook')
```
</details>

![Audience_Scatter](/_images/amazon-marketing-cloud/playbooks/CLTV/CLTV_9_audience_scatter.png)

Here, we can see some trends emerging where the high-loyalty customers and low-CLTV customers are showing a higher spend per user, and the high-revenue potential customer and high CLTV customer are showing a general lower spend per user and shows a higher rate of sales. 

### Audience segment performance

Targeting specifically by audience segment can pay dividends for immediate campaign activation. As you can see below, the high-value customer segment shows a higher conversion rate across campaigns when compared to the low-value customer segment. The query will calculate conversion rate on audience and allow for an analysis in MS Excel. 

<details class="details-bar">
  <summary>Click to see: SQL code to calculate conversion rate on audience </summary> 

```
With user_ntb_cte AS (  
    -- Gets the purchase details per order for individual user  
    SELECT  
       user_id  
       , sum(total_sales) as total_sales  
       , sum(conversion_frequency) as conversion_frequency  
       , min(min_date) as min_date  
       , max(max_date) as max_date  
       , round(SECONDS_BETWEEN(min(min_date),max(max_date)) / 60 / 60 / 24 / 30) as cust_lifetime  
    from (  
            SELECT  
               user_id  
               , sum(total_product_sales) as total_sales  
               , sum(conversions) as conversion_frequency  
               , min(case when event_subtype = 'order' then event_date_utc else null end) as min_date  
               , max(case when event_subtype = 'order' then event_date_utc else null end) as max_date  
            from conversions a  
            where event_subtype = 'order'  
            and total_product_sales > 0  
            and user_id is not null  
            group by 1  
              
        union all  
          
            SELECT  
               user_id  
               , sum(total_product_sales) as total_sales  
               , sum(conversions) as conversion_frequency  
               , min(case when event_subtype = 'order' then event_date_utc else null end) as min_date  
               , max(case when event_subtype = 'order' then event_date_utc else null end) as max_date  
            from conversions_non_ad_exposed  
            where event_subtype = 'order'  
            and user_id is not null  
            and total_product_sales > 0  
            group by 1  
    )  
    where user_id is not null  
    group by 1  
),  
user_lifetime AS (  
    SELECT ntb.user_id  
    , ntb.min_date  
    , ntb.max_date  
    , ntb.total_sales  
    , ntb.conversion_frequency  
    , ntb.cust_lifetime  
    , ntb.total_units_sold / (avg(ntb.total_units_sold) OVER ()) as purchase_quantity_ratio  
    , ntb.conversion_frequency / (avg(ntb.conversion_frequency) OVER ()) as purchase_frequency_ratio  
    , ntb.cust_lifetime / (avg(ntb.cust_lifetime) OVER ()) as user_lifetime_ratio  
    FROM user_ntb_cte ntb  
    where ntb_flag = 1  
),  
user_cost_cte AS (  
    SELECT user_id  
    , sum(total_spend) as total_spend  
    FROM  (  
        SELECT user_id  
        , sum(spend)/ 100000000 as total_spend  
        from sponsored_ads_traffic  
        where clicks > 0  
        group by 1  
    UNION ALL  
        SELECT user_id  
        , sum(total_cost) / 100000  as total_spend  
        from dsp_impressions  
        where impressions > 0  
        group by 1  
    )  
    group by 1  
),  
value_cte AS (  
    SELECT r.user_id  
    , r.total_sales  
    , r.purchase_quantity_ratio  
    , r.conversion_frequency  
    , r.purchase_frequency_ratio  
    , r.cust_lifetime  
    , r.user_lifetime_ratio  
    , c.total_spend  
    , ((r.purchase_frequency_ratio) * (r.purchase_quantity_ratio) * (r.user_lifetime_ratio)) as lifetime_score  
    , (r.total_sales - c.total_spend)/c.total_spend AS value_score  
    FROM user_lifetime r  
    LEFT JOIN user_cost_cte c  
        ON c.user_id = r.user_id  
),  
audience_labeling as (  
    SELECT  
    user_id,  
    case  
        when  
            (lifetime_score >= (avg(lifetime_score) OVER ()))  
            AND  
            (value_score >= (avg(value_score) OVER ()))  
            THEN 'High CLTV Customer'  
        when  
            (lifetime_score < (avg(lifetime_score) OVER ()))  
            AND  
            (value_score < (avg(value_score) OVER ()))  
            THEN 'Low CLTV Customer'  
        else 'Non-Labeled'  
        end as Customer_Audience_Segment  
    from value_cte  
),  
dsp_events AS (  
  SELECT  
    user_id,  
    campaign_id  
  FROM  
    dsp_impressions  
  WHERE user_id IS NOT NULL -- UPDATE: group by the number of attributes you included in the CTE  
  GROUP BY  
    1,  
    2  
),  
conversion_events AS(  
  SELECT  
    user_id,  
    campaign_id  
  FROM  
    amazon_attributed_events_by_traffic_time  
  WHERE user_id IN (  
      SELECT  
        user_id  
      FROM  
        dsp_events  
    ) -- UPDATE: group by the number of attributes you included in the CTE  
    and conversion_event_subtype = 'order'  
  GROUP BY  
    1,  
    2  
)  
SELECT  
  de.campaign_id,  
  a.Customer_Audience_Segment,  
  -- OPTIONAL UPDATE: comment out any of the four dimensions below if they were previously excluded.  
  COUNT(DISTINCT de.user_id) AS unique_users_reached,  
  COUNT(DISTINCT ce.user_id) AS unique_users_purchased,  
  COUNT(DISTINCT ce.user_id) / COUNT(DISTINCT de.user_id) as user_conversion_rate  
FROM  
  dsp_events de  
  Inner Join audience_labeling a on a.user_id = de.user_id  
  LEFT JOIN conversion_events ce ON de.user_id = ce.user_id  
  AND de.campaign_id = ce.campaign_id -- OPTIONAL UPDATE: comment out any of the four dimensions below if they were previously excluded.  
GROUP BY  
  1, 2 
```
</details>

Below is the expected output (data is synthetic)

campaign\_id | customer\_audience\_segment | unique\_users\_reached | unique\_users\_purchased | user\_conversion\_rate 
---|---|---|---|---
 1111 | High Value Customer | 22059 | 1051 | 0\.04764 
 2222 | Low Value Customer | 47303 | 1805 | 0\.03816 
 3333 | High Value Customer | 42049 | 1305 | 0\.03104 
 4444 | Low Value Customer | 7219 | 167 | 0\.02313 
 5555 | High Value Customer | 21412 | 885 | 0\.04133 


Here, you can see the relative value garnered if you target high-value customers specifically over other customer types. 

![Audience_Segment_Conversion_Rate](/_images/amazon-marketing-cloud/playbooks/CLTV/CLTV_10_audience_segmentation_conv_rate.png)

### Activate audiences

To create and activate the queried list of user_id use the [POST method](guides/amazon-marketing-cloud/audiences/rule-based-audiences#create-a-rule-based-audience) of the AMC rule-based Audience API.   

#### **SQL query - High-value audience segments** 

If a customer wants to activate against high-value customers, the SQL query below will pull a list of user_ids that meet the high value criterion. 

> [NOTE] SQL query is only suffixed with the audience segments over a specific time window that the data can be executed upon. 

<details class="details-bar">
  <summary>Click to see: SQL code query user_ids of high-value customers </summary> 

```
With user_ntb_cte AS (  
    -- Gets the purchase details per order for individual User  
    SELECT  
       user_id  
       , max(ntb_flag) as ntb_flag  
       , sum(total_sales) as total_sales  
       , sum(total_units_sold) as total_units_sold  
       , sum(conversion_frequency) as conversion_frequency  
       , min(min_date) as min_date  
       , max(max_date) as max_date  
       , round(SECONDS_BETWEEN(min(min_date),max(max_date)) / 60 / 60 / 24 / 30) as cust_lifetime  
    from (  
            SELECT  
               user_id  
               , max(case when event_subtype = 'order' and new_to_brand = TRUE then 1 else 0 end) as ntb_flag  
               , sum(total_product_sales) as total_sales  
               , sum(total_units_sold) as total_units_sold  
               , sum(conversions) as conversion_frequency  
               , min(case when event_subtype = 'order' then event_date_utc else null end) as min_date  
               , max(case when event_subtype = 'order' then event_date_utc else null end) as max_date  
            from conversions a  
            where event_subtype = 'order'  
            and total_product_sales > 0  
            and user_id is not null  
            group by 1  
              
        union all  
          
            SELECT  
               user_id  
               , max(case when event_subtype = 'order' and new_to_brand = TRUE then 1 else 0 end) as ntb_flag  
               , sum(total_product_sales) as total_sales  
               , sum(total_units_sold) as total_units_sold  
               , sum(conversions) as conversion_frequency  
               , min(case when event_subtype = 'order' then event_date_utc else null end) as min_date  
               , max(case when event_subtype = 'order' then event_date_utc else null end) as max_date  
            from conversions_non_ad_exposed  
            where event_subtype = 'order'  
            and user_id is not null  
            and total_product_sales > 0  
            group by 1  
    )  
    where user_id is not null  
    group by 1  
),  
user_lifetime AS (  
    SELECT ntb.user_id  
    , ntb.ntb_flag  
    , ntb.min_date  
    , ntb.max_date  
    , ntb.total_sales  
    , ntb.total_units_sold  
    , ntb.conversion_frequency  
    , ntb.cust_lifetime  
    , ntb.total_units_sold / (avg(ntb.total_units_sold) OVER ()) as purchase_quantity_ratio  
    , ntb.conversion_frequency / (avg(ntb.conversion_frequency) OVER ()) as purchase_frequency_ratio  
    , ntb.cust_lifetime / (avg(ntb.cust_lifetime) OVER ()) as user_lifetime_ratio  
    FROM user_ntb_cte ntb  
    where ntb_flag = 1  
),  
user_cost_cte AS (  
    SELECT user_id  
    , sum(dsp_spend) AS dsp_spend  
    , sum(sponsored_spend) as sponsored_spend  
    , sum(total_spend) as total_spend  
    FROM  (  
        SELECT user_id  
        , 0 as dsp_spend  
        , sum(spend)/ 100000000 as sponsored_spend  
        , sum(spend)/ 100000000 as total_spend  
        from sponsored_ads_traffic  
        where clicks > 0  
        group by 1  
    UNION ALL  
        SELECT user_id  
        , sum(total_cost) / 100000 as dsp_spend  
        , 0 as sponsored_spend  
        , sum(total_cost) / 100000  as total_spend  
        from dsp_impressions  
        where impressions > 0  
        group by 1  
    )  
    group by 1  
),  
value_cte AS (  
    SELECT r.user_id  
    , r.ntb_flag  
    , r.total_sales  
    , r.total_units_sold  
    , r.purchase_quantity_ratio  
    , r.conversion_frequency  
    , r.purchase_frequency_ratio  
    , r.cust_lifetime  
    , r.user_lifetime_ratio  
    , c.dsp_spend  
    , c.sponsored_spend  
    , c.total_spend  
    , ((r.purchase_frequency_ratio) * (r.purchase_quantity_ratio) * (r.user_lifetime_ratio)) as lifetime_score  
    , (r.total_sales - c.total_spend)/c.total_spend AS value_score  
    FROM user_lifetime r  
    LEFT JOIN user_cost_cte c  
        ON c.user_id = r.user_id  
),  
audience_labeling as (  
    SELECT  
    user_id,  
    case  
        when  
            (lifetime_score >= (avg(lifetime_score) OVER ()))  
            AND  
            (value_score >= (avg(value_score) OVER ()))  
            THEN 'High CLTV Customer'  
        when  
            (lifetime_score >= (avg(lifetime_score) OVER ()))  
            AND  
            (value_score < (avg(value_score) OVER ()))  
            THEN 'High Loyalty Customer'  
        when  
            (lifetime_score < (avg(lifetime_score) OVER ()))  
            AND  
            (value_score < (avg(value_score) OVER ()))  
            THEN 'Low CLTV Customer'  
        when  
            (lifetime_score < (avg(lifetime_score) OVER ()))  
            AND  
            (value_score >= (avg(value_score) OVER ()))  
            THEN 'High Revenue Potential Customer'  
        else 'Non-Labeled'  
        end as Customer_Audience_Segment  
    , count(distinct user_id) as users  
    , avg(purchase_frequency_ratio) as purchase_frequency_ratio  
    , avg(purchase_quantity_ratio) as purchase_quantity_ratio  
    , avg(user_lifetime_ratio) as user_lifetime_ratio  
    , sum(total_sales) as total_sales  
    , sum(total_units_sold) as total_units_sold  
    , sum(total_spend) as total_spend  
    , avg(lifetime_score) * avg(value_score) as CLTV  
    from value_cte  
    group by 1, 2  
)  
select  
   distinct user_id  
from audience_labeling  
where Customer_Audience_Segment = 'High Value Customer'
```
</details>

>[WARNING] The audience could take about 48 hours to activate in Amazon DSP. 

### CLTV Audience segmentation (year-over-year)
 
While AMC has constraints to allow users to query events within 13 months or less, you can use a data lake and audience segment labels to measure how the distribution of your CLTV audience segments have a changing user count over time. This will allow you to measure the overall health of your business and how successful your tactics to move low value customers into being high value customers are. In order to do so, you would just need apply a time stamp component to the table within the workflow manager service on your audience segmentation queries. Then, you can schedule your queries to recalculate CLTV over a regular cadence into the future. A dashboard can be created to visualize your customer base over time.

![CLTV_YoY](/_images/amazon-marketing-cloud/playbooks/CLTV/CLTV_11_yoy.png)

 
