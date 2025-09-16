---
title: Amazon Marketing Cloud - Keyword clustering
description: Keyword clustering
type: guide
interface: api
---
# Keyword clustering

This playbook provides a comprehensive, step-by-step guide for advertisers to leverage AMC data and optimize their Amazon advertising campaigns through strategic keyword clustering and analysis.

## What is keyword clustering?

Keyword clustering is a strategic approach that groups similar search terms together based on the products (ASINs) they drive traffic and sales to. By leveraging customer purchase data from Amazon Marketing Cloud (AMC), advertisers can identify which keywords are associated with the highest-revenue products, enabling more precise targeting and efficient budget allocation.
Keyword clustering operates on the fact that certain search terms consistently lead to the purchase of specific products. By organizing keywords into cohesive groups, advertisers can simplify the management of large, complex keyword portfolios, improve the relevance of their campaigns, and enhance the overall effectiveness of their Amazon advertising efforts.
The playbook's strategy combines proven SEO methodologies, such as keyword relevance, content clustering, and continuous optimization, with customer purchase data from AMC. This data-driven approach allows advertisers to identify the most effective ASINs for each keyword and prioritize them based on revenue potential. By clustering keywords that consistently drive revenue to the same ASINs, the strategy ensures a strategic alignment between search terms and high-performing products, optimizing ad spend and improving campaign effectiveness.

## Benefits

Key benefits of keyword clustering include:

* **Targeted advertising for better results**: By grouping similar keywords into clusters, this playbook enables you to create highly targeted campaigns. Each cluster is aligned with specific products (ASINs) that have a proven track record of driving sales, ensuring your ads reach the right audience.
* **Efficient budget utilization**: The playbook guides you in allocating your advertising budget to the most impactful keyword clusters, minimizing wasteful spending and maximizing your return on ad spend (ROAS).
* **Streamlined campaign management**: This playbook simplifies the process by organizing keywords into manageable clusters, reducing the complexity of campaign management.
* **Data-driven decision making**: Integrating AMC data, the playbook empowers you with insights based on actual customer purchase behavior, helping you make informed decisions and optimize bids.
* **Enhanced visibility and brand growth**: For advertisers using Sponsored Products and Sponsored Brands, this playbook offers strategies to increase product visibility and brand awareness, driving both immediate sales and long-term brand loyalty.
* **Versatility across campaign types**: Whether your focus is on Sponsored Products or Sponsored Brands, the principles outlined in this playbook can be adapted to your specific campaign type, providing a versatile approach to keyword targeting and optimization.

## Prerequisites

To get the most out of the playbook, it is recommended you have the following technical skills and knowledge:

- **AMC concepts**, specifically data aggregation thresholds, privacy limitations, and other restrictions listed in the [AMC documentation](https://advertising.amazon.com/API/docs/en-us/guides/amazon-marketing-cloud/overview).
- **Knowledge of [AMC SQL (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries/f53b1ad1359ef811fcdccdc8c6e69422b83bf06a163da53fddf79b6ca729edb6).** The processes outlined here relies on altering SQL code to match your purposes.
- **Basic AWS knowledge** of [Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html) in specific. You will need to know how to set up an Amazon SageMaker Notebook in accordance with your company's standards.
- **Proficient with Python**: A large portion of the solution described here takes place on Python, so a proficiency with Python is a must.
- **Clustering concepts**: Requires a basic understanding of how clustering algorithms, such as such as how K-means, hierarchical clustering, etc. work.
- **Data-driven decision-making and performance measurement** and a penchant for turning insights into action with media activations from AMC performance measures.

### Recommended Architecture

We also recommend that you deploy Amazon Marketing Cloud Insights on AWS or a similar custom solution. The AMC insights on AWS provides several solutions like:

- **Workflow manager**: Service to create and schedule AMC query executions.
- **Data Lake**: Service to take signals from the source bucket, transform into parquet and catalogue in Athena. This allows for signals to pulled via Athena. This is a standard offering with AWS insights product.

### Costs associated with implementing this playbook

- The costs associating with the implementation outlined here assume the standard pricing for Amazon S3, AWS Lambda, and Amazon SageMaker.
- Costs to run per advertiser on a monthly basis have been reported at less than $1 a month but will vary depending on data load, architecture, frequency, and so on.

## Playbook steps

The playbook is structured in the following order:

**Step 1: Data collection from AMC**: Retrieve essential data from conversion tables in AMC,  with the help workflow manager, to gather information on user interactions and conversions.

**Step 2: Keyword clustering:** Leverage Amazon SageMaker to process the collected data and generate cluster IDs based on defined parameters; group relevant keywords for specific Amazon Standard Identification Numbers (ASINs) for data manipulation and grouping; and organize data around search terms and products.

**Step 3: Analysis**: Utilize scatter plots to visually analyze clusters, apply the 80-20 rule; filter out less relevant clusters.

**Step 4: Upload and action the generated insights:** Upload the enriched data and action those for further insights and optimization.

![process](/_images/amazon-marketing-cloud/playbooks/keyword_clustering/kc_1_process.png)

## Data collection from AMC


The following AMC query aggregates performance KPIs based on what shoppers search for (`customer_search_term`) and the products (`tracked_item`) they finally buy, with dimensions such as marketplace (`marketplace_name`). The query also derives the traffic and sales that each search term brings. This data helps derive the relationship between search behavior and product selection.

>[NOTE] We recommend using **Amazon Marketing Cloud Insights on AWS** to run queries and store the results. Additionally, the queries  can be scheduled for automated refresh. Alternatively, manually run the query in the AMC UI, download the result set, and perform the analysis.

<details class="details-bar">
  <summary>Click to see script: aggregate performance-based KPI purchase</summary>

```
WITH ad_products (ad_product_type) AS (
    VALUES
        /*OPTIONAL UPDATE: Remove one ad product type to only focus on one.*/
        ('sponsored_products'),
        ('sponsored_brands')
),
attributed_events AS (
    SELECT traffic_event_id,
        user_id,
        conversion_event_marketplace_name AS marketplace_name,
        customer_search_term,
        tracked_item,
        MAX(
            CASE
                WHEN new_to_brand THEN 1
            END
        ) AS ntb_flag,
        SUM(detail_page_view) AS detail_page_view,
        SUM(add_to_cart) AS add_to_cart,
        SUM(purchases) AS purchases,
        SUM(units_sold) AS units_sold,
        SUM(product_sales) AS product_sales,
        SUM(new_to_brand_purchases) AS ntb_purchases,
        SUM(new_to_brand_units_sold) AS ntb_units_sold,
        SUM(new_to_brand_product_sales) AS ntb_product_sales,
        SUM(total_detail_page_view) AS total_detail_page_view,
        SUM(total_add_to_cart) AS total_add_to_cart,
        SUM(total_purchases) AS total_purchases,
        SUM(total_units_sold) AS total_units_sold,
        SUM(total_product_sales) AS total_product_sales,
        SUM(new_to_brand_total_purchases) AS total_ntb_purchases,
        SUM(new_to_brand_total_units_sold) AS total_ntb_units_sold,
        SUM(new_to_brand_total_product_sales) AS total_ntb_product_sales
    FROM amazon_attributed_events_by_conversion_time
    WHERE (1 = 1)
        AND ad_product_type IN (
            SELECT ad_product_type
            FROM ad_products
        )
        AND customer_search_term IS NOT NULL
        AND tracked_item IS NOT NULL
    GROUP BY 1,
        2,
        3,
        4,
        5
)
SELECT BUILT_IN_PARAMETER('TIME_WINDOW_START') AS start_date,
    BUILT_IN_PARAMETER('TIME_WINDOW_END') AS end_date,
    CUSTOM_PARAMETER('report_dt') AS report_dt,
 /*OPTIONAL UPDATE: Remove above when you are running it directly on AMC via Query editor 
accordingly update the Group by clause*/
    marketplace_name,
    customer_search_term,
    tracked_item,
    COUNT(
        DISTINCT CASE
            WHEN detail_page_view > 0 THEN user_id
        END
    ) AS users_dpv,
    COUNT(
        DISTINCT CASE
            WHEN purchases > 0 THEN user_id
        END
    ) AS users_purchased,
    COUNT(
        DISTINCT CASE
            WHEN (
                ntb_flag > 0
                AND purchases > 0
            ) THEN user_id
        END
    ) AS users_ntb,
    SUM(detail_page_view) AS detail_page_view,
    SUM(add_to_cart) AS add_to_cart,
    SUM(purchases) AS purchases,
    SUM(units_sold) AS units_sold,
    SUM(product_sales) AS product_sales,
    SUM(ntb_purchases) AS ntb_purchases,
    SUM(ntb_units_sold) AS ntb_units_sold,
    SUM(ntb_product_sales) AS ntb_product_sales,
    SUM(total_detail_page_view) AS total_detail_page_view,
    SUM(total_add_to_cart) AS total_add_to_cart,
    SUM(total_purchases) AS total_purchases,
    SUM(total_units_sold) AS total_units_sold,
    SUM(total_product_sales) AS total_product_sales,
    SUM(total_ntb_purchases) AS total_ntb_purchases,
    SUM(total_ntb_units_sold) AS total_ntb_units_sold,
    SUM(total_ntb_product_sales) AS total_ntb_product_sales
FROM attributed_events
GROUP BY 1,
    2,
    3,
    4,
    5,
    6
HAVING (1 = 1)
    AND SUM(total_purchases) > 0
```

</details>

### Load query results into Python

We need to read the data that has been retrieved by the above query from [Amazon Athena](https://docs.aws.amazon.com/athena/).

>[NOTE] All the code has been written in Amazon SageMaker using Python

<details class="details-bar">
 <summary>Click to see script: Load query results to Python</summary>

```
import pandas as pd
import awswrangler as wr

# No need to create a Boto3 session in SageMaker, it uses the default session with its role
# Read data from an Athena query result into a DataFrame using awswrangler
df = wr.athena.read_sql_query(
    sql=sql,  # The SQL query to be executed
    database="database_name"  # The Athena database name
)

# Display the DataFrame
df

```

</details>

Or, read the data manually from a local .csv file downloaded from AMC

<details class="details-bar">
<summary>Click to see script:  Read query results </summary>

```

# Import necessary libraries
import boto3  # Importing boto3 library for AWS services
import pandas as pd  # Importing pandas library for data manipulation
import plotly.graph_objects as go  # Importing plotly library for data visualization
from tqdm.notebook import tqdm  # Importing tqdm library for progress tracking

import math  # Importing math library for mathematical functions

# Read the data from CSV file into a pandas DataFrame
df_AMCData = pd.read_csv('Data/keyword_clustering_data.csv') 
df = pd.DataFrame(df_AMCData)  # Convert the DataFrame into a pandas DataFrame object
df  # Display the DataFrame

```

</details>


 start\_date | end\_date | customer\_search\_term | tracked\_item | users\_dpv | users\_purchased | users\_ntb | detail\_page\_view | add\_to\_cart | purchases | units\_sold | product\_sales | ntb\_purchases | ntb\_units\_sold | ntb\_product\_sales | total\_detail\_page\_view | total\_add\_to\_cart | total\_purchases | total\_units\_sold | total\_product\_sales | total\_ntb\_purchases | total\_ntb\_units\_sold | total\_ntb\_product\_sales | filtered | customer\_hash | export\_year | export\_month | file\_last\_modified 
---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | nan | nan | 1 | 1 | 1 | 4 | 0 | 1 | 1 | 42\.74 | 1 | 1 | 15\.76 | 4 | 0 | 1 | 1 | 22\.04 | 1 | 1 | 28\.74 | TRUE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | smartphone | B866GT6WQH | 3 | 1 | 1 | 6 | 0 | 1 | 1 | 13\.32 | 1 | 1 | 18\.32 | 6 | 0 | 1 | 1 | 15\.32 | 1 | 1 | 23\.32 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | high performance smartphone | BGRQ2VPZPF | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | nan | nan | 1 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | TRUE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | nan | nan | 0 | 1 | 0 | 0 | 0 | 1 | 1 | 37\.91 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 7\.41 | 0 | 0 | 0 | TRUE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | budget smartphone | BF37222 | 5 | 2 | 1 | 8 | 0 | 2 | 2 | 2\.56 | 1 | 1 | 43\.24 | 8 | 0 | 2 | 2 | 24\.56 | 1 | 1 | 23\.24 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | nan | nan | 1 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | TRUE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | nan | nan | 1 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | TRUE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | basic smartphone model | B0BPS3W | 2 | 0 | 0 | 3 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 3 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | nan | nan | 1 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | TRUE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | nan | nan | 1 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | TRUE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | nan | nan | 0 | 1 | 0 | 0 | 0 | 1 | 1 | 2\.97 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 21\.97 | 0 | 0 | 0 | TRUE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B954030UGLS | B0030HIJN3U | 3 | 1 | 0 | 4 | 0 | 1 | 1 | 8\.49 | 0 | 0 | 0 | 4 | 0 | 1 | 1 | 7\.49 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B5330UGO6G | B730UGMQI | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 3 | 0 | 1 | 1 | 4\.32 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 



The output of this query provides a comprehensive analysis of Amazon advertising performance metrics by organizing data around customer search terms and tracked products (ASINs) within a specified time frame. It identifies the marketplace, aggregates user engagement metrics such as detail page views, add-to-cart actions, and purchases, and distinguishes between regular and new-to-brand customers. Additionally, the query highlights key performance indicators like total sales and revenue, helping to pinpoint high-performing keywords and products.

This detailed data serves as the foundational step for keyword clustering, enabling strategic alignment of search terms with successful products to optimize advertising campaigns effectively. By aggregating metrics such as user engagement, purchases, and revenue for each search term and associated product, the data allows for the identification of high-performing keywords. These keywords can then be grouped together based on their effectiveness in driving traffic and sales to specific products. This clustering helps in creating targeted advertising campaigns that are aligned with proven customer search behavior, optimizing budget allocation and enhancing overall campaign effectiveness.

### Refine data and rank product

This phase filters the data based on Amazon's criteria, with a particular focus on user counts. Additionally, a ranking system is implemented, prioritizing products with the highest sales volume. To simplify the analysis, only the top 10 results per search term are considered. This ensures that the focus remains on the most impactful products in terms of sales, offering a streamlined approach to studying their performance.

<details class="details-bar">
<summary>Click to see script:  Filter records not meeting threshold</summary>

```
# Filter out lines that do not meet AMC thresholds
df = df[df.filtered.eq(False)]

# Calculate the rank of products within each 'customer_search_term' group based on 'product_sales'
df['ASIN_rank'] = df.groupby("customer_search_term")['product_sales'].rank(method='first', ascending=False)

# Filter the DataFrame to keep only the top 10 selling products for each customer search term
df = df[df['ASIN_rank'] <= 10]

# Example print to display the columns of the DataFrame
print(df.columns)

# Print the rows of the DataFrame where 'customer_search_term' is "cat food", sorted by 'ASIN_rank'
print(df[df.customer_search_term == "cat food"].sort_values(by="ASIN_rank"))
```

</details>


 start\_date | end\_date | customer\_search\_term | tracked\_item | users\_dpv | users\_purchased | users\_ntb | detail\_page\_view | add\_to\_cart | purchases | units\_sold | product\_sales | ntb\_purchases | ntb\_units\_sold | ntb\_product\_sales | total\_detail\_page\_view | total\_add\_to\_cart | total\_purchases | total\_units\_sold | total\_product\_sales | total\_ntb\_purchases | total\_ntb\_units\_sold | total\_ntb\_product\_sales | filtered | customer\_hash | export\_year | export\_month | file\_last\_modified | ASIN\_rank 
---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | largerscreen smartphones | B096GT6QH | 3 | 1 | 1 | 5 | 0 | 1 | 1 | 11\.32 | 1 | 1 | 11\.32 | 6 | 0 | 1 | 1 | 14\.32 | 1 | 1 | 3\.32 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 1 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | budget phone | B07Q2ZPF | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 2 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | large screen smartphone | B074F222 | 5 | 2 | 1 | 8 | 0 | 2 | 2 | 21 | 1 | 1 | 13\.24 | 8 | 0 | 2 | 2 | 22\.56 | 1 | 1 | 3\.24 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 3 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | smartmobile | B0BPFS3W | 2 | 0 | 0 | 3 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 3 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 4 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B0030GLS2 | B003GN3U | 3 | 1 | 0 | 4 | 0 | 1 | 1 | 8\.49 | 0 | 0 | 0 | 4 | 0 | 1 | 1 | 8\.49 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 5 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B0030UGO | B0030UQI | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 3 | 0 | 1 | 1 | 3\.32 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 6 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B003674G | B0036QA8 | 2 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 7 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B003MYJEA | B0BQ3SJV | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 6 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 8 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B00XN3E8 | B01N2ROR | 9 | 1 | 1 | 14 | 0 | 1 | 1 | 4\.41 | 1 | 1 | 4\.41 | 18 | 0 | 1 | 1 | 4\.41 | 1 | 1 | 4\.41 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 9 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B003ZHVC | B0BNZKRPG | 14 | 6 | 4 | 17 | 0 | 6 | 7 | 158\.69 | 4 | 5 | 11\.54 | 33 | 0 | 6 | 7 | 78\.69 | 4 | 5 | 11\.54 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 10 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B003ZGE6 | B0030UGG | 2 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 11 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B003ZTR6 | B097TR5Y | 2 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 12 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B004G90W | B0BQNB2N8 | 2 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 13 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B004T3F1 | B077GQC3 | 2 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 14 



## Keyword clustering

The process involves inputting data into a user defined function to cluster keywords based on overlapping ASINs. This function generates cluster IDs based on parameters and groups keywords relevant to specific ASINs. The iteration is driven by sorting data by key performance indicators (KPIs) like total  new to brand customers, page views, revenue. The function starts with the most relevant keyword, extracts ASINs, and groups similar keywords based on the overlap threshold. This process continues, creating clusters with decreasing relevance. The final step transforms the dataset for further analysis. The function reports the number of keywords grouped in each cluster and the associated ASINs. This process efficiently organizes keywords into clusters, highlighting commonalities and helping to identify patterns among them. It simplifies complex data, enabling a clearer understanding of relevant keyword groups and their impact on ASINs or outcomes.

### Strategic analysis

The strategy adopted in this playbook analyzes the journey consumers take when searching and ultimately purchasing products on Amazon.In this mapping:

- **Search Phrases**: Represented by bubbles at the top (e.g., "Smartphone").
- **Product Purchases**: Represented by smaller circles connected by arrows, indicating the products consumers end up buying.
  This visualization helps in understanding the path from search phrases to actual purchases, revealing key insights for optimizing Amazon campaigns.

![cluster](/_images/amazon-marketing-cloud/playbooks/keyword_clustering/kc_2_cluster.png)

The diagram illustrates the process of keyword clustering in action. Each bubble at the top represents a search term (e.g., "smartphone"), and the smaller circles below represent products that consumers end up purchasing after using that search term. The lines connecting them show how multiple search terms can lead to overlapping product outcomes, which is the essence of keyword clustering. In this mapping:

- **Cluster 1** groups search terms like "smartphone with large screen," "high-performance smartphone," and "latest smartphone model" that lead to similar products. These terms cluster together because they generate clicks or purchases of overlapping ASINs, making them part of the same high-performing product group.
- **Cluster 2** includes search terms like "budget smartphone," "affordable mobile phone," and "basic smartphone model," which similarly lead to a different set of products but follow the same logic.
- **New Cluster** is created if a search term doesn’t align with existing clusters, meaning it doesn’t match already identified products.

By analyzing these clusters, advertisers can gain insights into the relationship between search behavior and product purchases. This understanding allows them to optimize campaigns by focusing on high-performing keywords and aligning them with product groups that drive the most engagement or sales. In turn, this strategy enables more efficient campaign management, helping advertisers uncover new keyword opportunities and prioritize keywords that yield the highest return.

The search term query involves extracting conversion tables from AMC and aggregating data around search terms and products.

### Clustering considerations and best practices


| Component                             | Explanation                                                                                                                                                                                                                                                                               | Best practice                                                                                                                                                                                                                                        |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Challenges with K\-Means              | Traditional K\-Means clustering poses challenges in defining the right number of clusters\. Too few clusters lead to broad groupings, while too many result in thin, less meaningful clusters\. Additionally, K\-Means' random starting point causes inconsistency in cluster formation\. | Refine the starting point by selecting the most relevant keyword for sales volume\. This ensures that clusters are aligned with high\-impact keywords from the outset\. Balance the number of clusters by starting small and adjusting iteratively\. |
| Starting point                        | The starting point\(or "seed"\) is critical in determining how clusters are formed\. Randomly selected starting points in K\-Means lead to inconsistent and unrepeatable clusters\.                                                                                                       | Begin with the most relevant keyword for sales volume to ensure a structured and repeatable process, leading to more controlled and consistent clusters\.                                                                                            |
| Reproducibility and tailored Focus    | Ensuring reproducibility in the clustering process is essential for consistency\. A random starting point introduces variability, leading to different outcomes in each run\.                                                                                                             | Use a consistent starting point based on sales volume, ensuring reproducibility and alignment with strategic goals\. This approach helps focus efforts on high\-priority areas\.                                                                     |
| Long-tail approach                    | To avoid overwhelming the media team with too many clusters, focus on a manageable number of keyword groups that provide the highest impact\.                                                                                                                                             | Aim for 15\-20 clusters, ensuring the media team can focus on specific areas, optimizing both time and budget allocation\.                                                                                                                           |
| Customer experience                   | Ensuring alignment between keywords and products is key to enhancing customer experience, as it affects relevance and engagement\.                                                                                                                                                        | Group keywords based on purchase behavior and ASIN relevance to enhance customer satisfaction and the overall ad campaign experience\.                                                                                                               |
| Ranking criteria                      | Keywords are ranked based on their contribution to total revenue\. However, it's important to balance this with visibility and volume potential\.                                                                                                                                         | Rank keywords based on a combination of revenue contribution, visibility, and potential growth opportunities for a balanced strategy\.                                                                                                               |
| Balancing sales volume and visibility | While high sales volume indicates efficient keywords, it’s important to also capture those with visibility potential for future growth\.                                                                                                                                                 | Ensure clusters include both high\-sales and high\-visibility keywords for a more balanced and growth\-oriented approach\.                                                                                                                           |
| Different views of clustering         | Clustering can focus on different objectives, such as increasing volume, conversion rates, or customer lifetime value, depending on the marketing strategy\.                                                                                                                              | Align the clustering strategy with the specific marketing goals, whether it's maximizing volume, improving conversions, or focusing on customer retention\.                                                                                          |
| Purchase behavior                     | Keywords that consistently lead to the same product purchases should be grouped together to reflect consistent customer journeys\.                                                                                                                                                        | Use purchase behavior patterns to form clusters that align with how customers search and purchase, improving the effectiveness of the ad targeting\.                                                                                                 |
| New cluster formation                 | If a search term doesn't fit into an existing cluster, it forms a new group\. This ensures flexibility and adaptability to new trends\.                                                                                                                                                   | Allow flexibility to create new clusters when search terms don’t fit into existing ones, ensuring adaptability to evolving customer behavior\.                                                                                                      |
| Influencing KPIs                      | The clustering process considers key performance indicators\(KPIs\) such as incrementality, visibility, and efficiency to guide optimization\.                                                                                                                                            | Customize clusters based on KPIs that align with business goals, such as new\-to\-brand metrics, click\-through rates, and return on ad spend \(ROAS\)\.                                                                                             |

### Analysis steps

The following steps outline the process for clustering keywords based on their associated ASINs. This method ensures that keywords are grouped efficiently to optimize your Amazon ad campaigns.

1. Input data: The function takes a dataframe sorted by a specific KPI.
2. Initial keyword selection: Begins with the most relevant keyword, extracting its ASINs.
3. Cluster formation: Keywords with ASINs overlapping beyond a set threshold are grouped together, forming clusters.
4. Iteration: Continues with decreasing relevance, creating additional clusters until all keywords are grouped.
5. Final transformation: The dataset is transformed for further analysis, providing a report of the number of keywords in each cluster and their associated ASINs.

<details class="details-bar">
<summary>Click to see script:  </summary>

```
def cluster_keywords_by_overlap(df, sort_by='ntb_product_sales', overlap_threshold=0.40, group_method='soft'):
    """
    Function to cluster keywords based on overlapping ASINs.
  
    Args:
    - df (DataFrame): The input dataframe.
    - sort_by (str): Column by which to sort. Default is 'new_to_brand_product_sales'.
    - overlap_threshold (float): Threshold for the overlap ratio to be considered. Default is 0.40.
    - group_method (str): Method of grouping - 'soft' or 'hard'. Default is 'soft'.
  
    Returns:
    - dict: Clustered containing keywords & asins.
    """
    # Validate the sort_by parameter
    if sort_by not in ('ntb_product_sales', 'product_sales', 'detail_page_view'):
        raise ValueError(f"Unexpected value for 'sort_by': {sort_by}")

    clusters = {}  # Dictionary to hold the clusters
    clustered_keywords = set()  # Set to keep track of already clustered keywords
    cluster_id = 1  # Unique identifier for each cluster
  
    # Sort keywords by the specified column
    unique_keywords = (
        df.groupby('customer_search_term')[sort_by]
        .sum()
        .sort_values(ascending=False)
        .index
        .unique()
    )

    # Map each unique keyword to its set of ASINs
    keyword_asins = {
        keyword: set(df[df['customer_search_term'] == keyword]['tracked_item'])
        for keyword in unique_keywords
    }

    # Iterate through each unique keyword
    for keyword in unique_keywords:
        # Skip the keyword if it has already been clustered
        if keyword in clustered_keywords:
            continue
  
        current_cluster_keywords = {keyword}  # Set to hold keywords in the current cluster
        current_cluster_asins = set()  # Set to hold ASINs in the current cluster
        clustered_keywords.add(keyword)  # Mark the keyword as clustered

        # Get ASINs for the current keyword
        asins = keyword_asins[keyword]

        # Inner loop to compare the current keyword with other keywords
        for keyword2 in unique_keywords:
            # Skip if the keywords are the same or if the second keyword is already clustered
            if keyword == keyword2 or keyword2 in clustered_keywords:
                continue

            # Get ASINs for the second keyword
            asins2 = keyword_asins[keyword2]
            max_asins = max(len(asins), len(asins2))
    
            # Calculate the overlap ratio based on the grouping method
            if group_method == 'soft':
                overlap = asins.intersection(asins2)
                overlap_ratio = len(overlap) / max_asins
        
                # If the overlap ratio meets the threshold, add to the current cluster
                if overlap_ratio >= overlap_threshold:
                    current_cluster_keywords.add(keyword2)
                    current_cluster_asins.update(overlap)
                    clustered_keywords.add(keyword2)

            elif group_method == 'hard':
                if current_cluster_asins:
                    overlap = current_cluster_asins.intersection(asins2)
                else:
                    overlap = asins.intersection(asins2)
  
                overlap_ratio = len(overlap) / max_asins

                # If the overlap ratio meets the threshold, add to the current cluster
                if overlap_ratio >= overlap_threshold:
                    current_cluster_keywords.add(keyword2)
                    current_cluster_asins = overlap
                    clustered_keywords.add(keyword2)
            
            else:
                # Raise an error if the grouping method is invalid
                raise ValueError(f"Unexpected value for 'group_method': {group_method}")

        # Add the current cluster to the clusters dictionary
        clusters[cluster_id] = {
            'keywords': list(current_cluster_keywords),
            'asins': list(current_cluster_asins),
        }
        cluster_id += 1  # Increment the cluster ID for the next cluster
  
    return clusters

# Example usage
clusters = cluster_keywords_by_overlap(df, sort_by='ntb_product_sales')
# Identify clusters that do not contain any ASINs
empty_asin_clusters = [cluster_id for cluster_id, cluster in clusters.items() if not cluster['asins']]

# Check if there are any clusters without ASINs
if empty_asin_clusters:
    # Find the smallest cluster_id among the empty ASIN clusters
    smallest_empty_asin_cluster = min(empty_asin_clusters)
    print(f"The smallest cluster_id with no ASINs is: {smallest_empty_asin_cluster}")
else:
    print("All clusters have at least one ASIN.")

# Count the number of clusters that contain at least one ASIN
clusters_count = len([cluster for cluster in clusters.values() if cluster['asins']])
print(f"Found {clusters_count} out of {len(clusters)} clusters with at least one ASIN:")

# Iterate through the clusters and print details for clusters with ASINs
for key, value in clusters.items():
    if len(value['asins']) > 0:
        print(f"cluster {key}: {len(value['keywords'])} keywords, {len(value['asins'])} asins")
```

</details>

![cluster_output](/_images/amazon-marketing-cloud/playbooks/keyword_clustering/kc_3_clusteroutput.png)

### Function overview

* Purpose: To cluster keywords by examining the overlap of ASINs associated with those keywords.
* Sorting: Starts with the most relevant keyword, based on the chosen KPI, and extracts the associated ASINs.
* Grouping: Groups similar keywords based on an overlap threshold, which determines how much overlap in ASINs is needed to group keywords together.
* Method: Can use a 'soft' grouping method (considering overlap between any two keywords) or a 'hard' grouping method (considering overlap within the current cluster).
  Grouping approaches and method

#### Overlap_threshold

This parameter represents the minimum ratio of overlap required between two sets of ASINs for their corresponding keywords to be considered part of the same cluster.

Value Range: It is a floating-point number between 0 and 1.
Purpose: It sets the sensitivity of the clustering. A higher threshold means that only keywords with a high proportion of shared ASINs will be clustered together, while a lower threshold allows for more loosely related keywords to be grouped.

#### Group_method

This parameter determines how the function handles the clustering process, particularly how it incorporates new keywords into existing clusters.
Purpose: It defines the criteria for adding keywords to clusters and thus affects the final clustering results.

  Approach | Method | Parameter | Parameter | Inclusion Condition | Resulting Cluster Size 
---|---|---|---|---|---
 Group\_method | 'soft' | Allows inclusion of keywords into the cluster as long as they meet the overlap threshold with any keyword already in the cluster\. | Each keyword is compared individually against the keywords already in the cluster\. | If the overlap ratio with any keyword in the cluster meets the threshold, the keyword is added to the cluster\. | Tends to create larger, more inclusive clusters\. 
  | | 'hard' | Only includes a new keyword into the cluster if the overlap threshold is met with the intersection of ASINs of all keywords currently in the cluster\. | The intersection of ASINs for all keywords currently in the cluster is considered\. | A new keyword is only added if it meets the overlap threshold with this intersection\. | Tends to create smaller, more tightly\-knit clusters\. 

**Example scenarios**

Scenario 1: `overlap_threshold` = 0.4 and group_method = 'soft'

Threshold: 40% overlap required.
Behavior: Keywords will be grouped together if they share at least 40% of ASINs with any keyword in the cluster.
Result: More inclusive clusters where the connection can be indirect.

Scenario 2: `overlap_threshold` = 0.4 and group_method = 'hard'

Threshold: 40% overlap required.
Behavior: Keywords will only be added to a cluster if they share at least 40% of ASINs with the intersection of ASINs of all keywords currently in the cluster.
Result: Smaller clusters with stronger connections.

##### Reason for using group method “soft”

The "soft" group method:

* Allows broader, more inclusive clusters by only requiring keywords to overlap with any cluster member, not the entire set.
* Accommodates indirect keyword relationships through intermediate terms.
* Lowers the barrier for keywords to be grouped, beneficial for capturing many related terms.
* Handles noisy real-world data better, leading to more robust clusters.
* Effective for markets with related but not identical products.

### Joining back to initial data sets

The following code will merge the clustered data with the original dataset. The aggregated data, grouped by cluster ID, includes:

* Traffic volume
* Sales volume
* ASIN count
* Keyword count

The results display cluster-level insights such as:

* Cluster IDs
* Page views
* Product sales
* Keyword and ASIN counts
* Sales-to-traffic ratio

This provides visibility into the efficiency and performance of each keyword cluster and aligns with expectations for a multi-brand advertising account.

Run the following code in Amazon SageMaker

<details class="details-bar">
<summary>Click to see script: map keywords </summary>

```
# Create a mapping of each keyword to its cluster_id and associated ASINs
keyword_to_cluster_id = {}  # Dictionary to map keywords to their cluster IDs
keyword_to_asins = {}  # Dictionary to map keywords to their associated ASINs

# Populate the dictionaries by iterating through the clusters
for cluster_id, cluster_data in clusters.items():
    for keyword in cluster_data['keywords']:
        keyword_to_cluster_id[keyword] = cluster_id  # Map keyword to cluster ID
        keyword_to_asins[keyword] = cluster_data['asins']  # Map keyword to its ASINs

# Convert the keyword to cluster ID mapping to a DataFrame
cluster_df = pd.DataFrame(list(keyword_to_cluster_id.items()), columns=['customer_search_term', 'cluster_id'])

# Join the original DataFrame (df) with the cluster DataFrame (cluster_df) based on the customer_search_term column
df = df.merge(cluster_df, on='customer_search_term', how='left')

# Create a new column 'is_hard_product_match' to check if the tracked_item is in the ASINs associated with the keyword
df['is_hard_product_match'] = df.apply(
    lambda row: row['tracked_item'] in keyword_to_asins.get(row['customer_search_term'], []), 
    axis=1
)

# Display the updated DataFrame
df
```

</details>

 start\_date | end\_date | customer\_search\_term | tracked\_item | users\_dpv | users\_purchased | users\_ntb | detail\_page\_view | add\_to\_cart | purchases | units\_sold | product\_sales | ntb\_purchases | ntb\_units\_sold | ntb\_product\_sales | total\_detail\_page\_view | total\_add\_to\_cart | total\_purchases | total\_units\_sold | total\_product\_sales | total\_ntb\_purchases | total\_ntb\_units\_sold | total\_ntb\_product\_sales | filtered | customer\_hash | export\_year | export\_month | file\_last\_modified | asin\_rank | cluster\_id | is\_hard\_product\_match 
---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | largerscreen smartphones | B096GT6QH | 3 | 1 | 1 | 5 | 0 | 1 | 1 | 11\.32 | 1 | 1 | 11\.32 | 6 | 0 | 1 | 1 | 14\.32 | 1 | 1 | 3\.32 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 1 | 1 | TRUE 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | budget phone | B07Q2ZPF | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 2 | 5 | FALSE 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | large screen smartphone | B074F222 | 5 | 2 | 1 | 8 | 0 | 2 | 2 | 21 | 1 | 1 | 13\.24 | 8 | 0 | 2 | 2 | 22\.56 | 1 | 1 | 3\.24 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 3 | 8 | TRUE 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | smartmobile | B0BPFS3W | 2 | 0 | 0 | 3 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 3 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 4 | 17 | TRUE 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B0030GLS2 | B003GN3U | 3 | 1 | 0 | 4 | 0 | 1 | 1 | 8\.49 | 0 | 0 | 0 | 4 | 0 | 1 | 1 | 8\.49 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 5 | 4 | FALSE 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B0030UGO | B0030UQI | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 3 | 0 | 1 | 1 | 3\.32 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 6 | 212 | FALSE 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B003674G | B0036QA8 | 2 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 7 | 122 | TRUE 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B003MYJEA | B0BQ3SJV | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 6 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 8 | 51 | FALSE 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B00XN3E8 | B01N2ROR | 9 | 1 | 1 | 14 | 0 | 1 | 1 | 4\.41 | 1 | 1 | 4\.41 | 17 | 0 | 1 | 1 | 4\.41 | 1 | 1 | 4\.41 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 9 | 45 | TRUE 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B003ZHVC | B0BNZKRPG | 14 | 6 | 4 | 17 | 0 | 6 | 7 | 158\.69 | 4 | 5 | 11\.54 | 31 | 0 | 6 | 7 | 78\.69 | 4 | 5 | 11\.54 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 10 | 76 | FALSE 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B003ZGE6 | B0030UGG | 2 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 11 | 18 | TRUE 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B003ZTR6 | B097TR5Y | 2 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 12 | 32 | TRUE 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B004G90W | B0BQNB2N8 | 2 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 13 | 86 | FALSE 
 2023\-08\-01T04:00:00\.000Z | 2023\-11\-01T04:00:00\.000Z | B004T3F1 | B077GQC3 | 2 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | FALSE | XXX | 2023 | 11 | 2023\-11\-15T14\-08\-54 | 14 | 27 | TRUE 


#### Aggregation

<details class="details-bar">
<summary>Click to see script: aggregation </summary>

```
def aggregate_group(group):
    """
    Function to aggregate data for a given group.

    Args:
    - group (DataFrame): The grouped DataFrame.

    Returns:
    - Series: Aggregated data for the group.
    """
    # Calculate the sum of detail_page_view for the group
    detail_page_view = group['detail_page_view'].sum()
  
    # Calculate the sum of product_sales for the group
    product_sales = group['product_sales'].sum()
  
    # Calculate the number of unique customer_search_terms in the group
    keyword_count = group['customer_search_term'].nunique()
  
    # Calculate the number of unique tracked_items (ASINs) in the group
    asin_count = group['tracked_item'].nunique()
  
    # Calculate the number of unique tracked_items (ASINs) that are hard product matches
    hard_match_asins_count = group[group['is_hard_product_match']]['tracked_item'].nunique()

    # Return the aggregated results as a Series
    return pd.Series({
        'detail_page_view': detail_page_view,
        'product_sales': product_sales,
        'keyword_count': keyword_count,
        'asin_count': asin_count,
        'hard_match_asins_count': hard_match_asins_count
    })

# Apply the aggregate_group function to each group in the DataFrame, grouped by 'cluster_id'
agg_df = df.groupby('cluster_id').apply(aggregate_group).reset_index()

# Sort the aggregated DataFrame by product_sales in descending order
agg_df = agg_df.sort_values("product_sales", ascending=False)

# Calculate the sales to traffic ratio and add it as a new column in the DataFrame
agg_df['sales_to_traffic_ratio'] = agg_df['product_sales'] / agg_df['detail_page_view']

# Display the aggregated and sorted DataFrame with the new sales to traffic ratio column
agg_df
```

</details>

cluster\_id | detail\_page\_view | product\_sales | keyword\_count | asin\_count | hard\_match\_asins\_count | sales\_to\_traffic\_ratio 
---|---|---|---|---|---|---
 0 | 2 | 2928 | 34573\.76 | 4 | 17 | 6 | 7\.26 
 1 | 5 | 343018 | 235974\.76 | 133 | 85 | 9 | 5\.65 
 5 | 3 | 27348 | 196267\.97 | 168 | 150 | 9 | 4\.23 
 8 | 8 | 27313 | 74552\.03 | 182 | 96 | 9 | 4\.06 
 532 | 812 | 44 | 0 | 9 | 3 | 1 | 0 
 823 | 579 | 51 | 0 | 1 | 7 | 0 | 0 

## Analysis

This post processing phase  involves conducting descriptive analyses for each cluster, evaluating factors like revenue, traffic, and conversion rates.

The data analysis uses a scatter plot to visualize clusters in terms of traffic and sales. 
The plot reveals significant clusters in the top right, indicating a positive correlation between traffic and sales, as expected in similar categories, however, the issue lies with smaller clusters in the bottom left which shows which keywords that are not performing well in terms of traffic and sales prompting the application of keyword optimization and keyword strategies. By analyzing and optimizing the keywords associated with these low-performing clusters, you can improve their visibility and relevance, driving more traffic and potentially increasing sales.

<details class="details-bar">
<summary>Click to see script: prepare data for analysis </summary>

```
# Filter out rows with NaN values in the 'sales_to_traffic_ratio' column
filtered_df = agg_df.dropna(subset=['sales_to_traffic_ratio']).copy()

# Scale the sales_to_traffic_ratio for visualization (factor can be adjusted if needed)
filtered_df['scaled_ratio'] = filtered_df['sales_to_traffic_ratio'] * 1

# Create a scatter plot using Plotly
scatter = go.Figure()

# Add a trace to the scatter plot
scatter.add_trace(go.Scatter(
    x=filtered_df['detail_page_view'],  # X-axis: Traffic volume
    y=filtered_df['product_sales'],  # Y-axis: Total sales volume
    mode='markers',  # Plot markers only (no lines)
    text=filtered_df['keyword_count'],  # Text to display on hover: number of keywords
    hoverinfo="x+y+text",  # Information to display on hover
    hovertemplate="Traffic: %{x} <br>Sales: %{y} <br>Keywords: %{text}"  # Custom hover template
))

# Update the layout of the scatter plot
scatter.update_layout(
    title='Traffic vs Sales - Cluster Scatter',  # Title of the plot
    xaxis_title='Traffic Volume',  # X-axis title
    yaxis_title='Total Sales Volume',  # Y-axis title
    hovermode='closest',  # Show hover information for the closest data point
    xaxis=dict(range=[0, filtered_df['detail_page_view'].quantile(0.999)]),  # Adjust X-axis range to exclude outliers
    yaxis=dict(range=[0, filtered_df['product_sales'].quantile(0.999)])  # Adjust Y-axis range to exclude outliers
)

# Display the scatter plot
scatter.show()

```

</details>

The ranges of both axes are adjusted to focus on most of the data while excluding extreme outliers. This is done using the 99.9th percentile of the data, ensuring the plot is not skewed by very extreme values.

![postprocessing](/_images/amazon-marketing-cloud/playbooks/keyword_clustering/kc_4_cluster_scatter.png)

What the plot shows:

* **Correlation insight**: The scatter plot helps to identify if there is a correlation between the amount of traffic a webpage receives and the sales it generates. For instance, if most points are moving in an upward trend from left to right, it suggests that more traffic tends to result in more sales.
* **Clusters**: If the points form clusters, it can indicate specific patterns or groups within the data. For example, certain keywords might be driving traffic and sales more effectively.
* **Outliers**: Points that are far from the rest may represent outliers, which can be crucial for further analysis. These could be cases with unusually high traffic but low sales or vice versa.

### Applying the 80/20 rule

In the context of sales data analysis, the 80/20 rule (Pareto principle) helps identify which clusters of keywords are most impactful in terms of revenue generation. The visualization of sales distribution across different clusters enables better understanding of where to focus optimization efforts.

<details class="details-bar">
<summary>Click to see script: 80/20 rule </summary>

```

# For the 80/20 rule
cumulative_sales = agg_df["product_sales"].cumsum()
eighty_percent_sales = agg_df["product_sales"].sum() * 0.8
mask_80 = cumulative_sales <= eighty_percent_sales
clusters_80 = agg_df[mask_80]
clusters_20 = agg_df[~mask_80]
bar = go.Figure()
# Define x positions for bars. This helps in placing bars side by side.
x_positions_80 = [
    "Top 80% Revenue - Revenue",
    "Top 80% Revenue - Cluster Counter",
]
x_positions_20 = [
    "Bottom 20% Revenue - Revenue",
    "Bottom 20% Revenue - Cluster Counter",
]
bar = go.Figure()
# Define positions for bars. This helps in placing bars side by side.
positions_top80 = ["Top 80% Revenue", "Top 80% Clusters"]
positions_bottom20 = ["Bottom 20% Revenue", "Bottom 20% Clusters"]
# Revenue bars for Top 80%
bar.add_trace(
    go.Bar(
        name="Revenue",
        x=[positions_top80[0]],
        y=[clusters_80["product_sales"].sum()],
    )
)
# Clusters bars for Top 80%
bar.add_trace(
    go.Bar(
        name="Cluster Counter",
        x=[positions_top80[1]],
        y=[len(clusters_80)],
        yaxis="y2",
    )
)
# Revenue bars for Bottom 20%
bar.add_trace(
    go.Bar(
        name="Revenue",
        x=[positions_bottom20[0]],
        y=[clusters_20["product_sales"].sum()],
    )
)
# Clusters bars for Bottom 20%
bar.add_trace(
    go.Bar(
        name="Cluster Counter",
        x=[positions_bottom20[1]],
        y=[len(clusters_20)],
        yaxis="y2",
    )
)
# Layout adjustments
bar.update_layout(
    title="80/20 Sales Distribution",
    yaxis_title="Revenue",
    yaxis2=dict(title="Number of Clusters", overlaying="y", side="right"),
    xaxis_title="Cluster Type",
    barmode="group",
    legend=dict(x=0.1, y=1.2, orientation="h"),
)
bar.show()

```

</details>

![postprocessing](/_images/amazon-marketing-cloud/playbooks/keyword_clustering/kc_4_cluster_scatter.png)

The bar chart shows the distribution of revenue and the number of clusters contributing to the top 80% and bottom 20% of sales:

- **Top 80% revenue**:
    Represents the total revenue generated by the top-performing 80% of clusters, with the most significant impact on sales.

- **Top 80% clusters**: Show the number of clusters that make up the top 80% of sales. It highlights the relatively small number of clusters that are driving the majority of the revenue.

-  **Bottom 20% revenue**: This bar represents the total revenue generated by the remaining clusters, which make up the bottom 20% of sales, providing insight into areas that may need improvement or reevaluation.

-	**Bottom 20% clusters**: This bar shows the number of clusters that contribute to the bottom 20% of sales. It illustrates the larger number of clusters that have a smaller impact on sales.

Focusing on the clusters that generate the most revenue (the top 80%) allows for prioritized optimization of these high-impact areas. Conversely, analysis of the bottom 20% of clusters reveals opportunities for improvement, such as refined keyword strategies, enhanced product pages, or reallocated marketing resources.

#### Boxplot

The above information in a boxplot can help in understanding the data's distribution, identifying outliers, and seeing how concentrated or spread out the data is.

<details class="details-bar">
<summary>Click to see script: boxplot  </summary>

```
# Filter the aggregated DataFrame to exclude rows where product_sales are 250,000 or more
agg_df_filtered = agg_df[agg_df['product_sales'] < 250000]

# Create a box plot using Plotly
box = go.Figure()

# Add a box trace to the plot
box.add_trace(go.Box(
    y=agg_df_filtered['product_sales'],  # Y-axis data: Sales volume from the filtered DataFrame
    boxmean=True,  # Display the mean as a dashed line
    name="Sales Volume"  # Name of the trace
))

# Update the layout of the box plot
box.update_layout(
    title="Distribution of Sales Volume without Outliers"  # Title of the plot
)

# Display the box plot
box.show()
```

</details>

![postprocessing](/_images/amazon-marketing-cloud/playbooks/keyword_clustering/kc_6_sales_vol_distribution.png)

**Interpreting for 80/20 rule**:

Top 20% clusters: The top part of the box plot are likely the clusters that contribute the most to the total sales.

Bottom 80% clusters: The bottom part of the box plot represents the clusters that contribute less.

By analyzing the box plot, you can see if a small number of clusters (top 20%) are indeed contributing to the majority (80%) of sales. This can help in validating the 80/20 rule visually and understanding the distribution of your data more comprehensively.

<details class="details-bar">
<summary>Click to see script: Pareto chart </summary>

```


import plotly.graph_objects as go
import pandas as pd

# Example aggregated data for demonstration
# Replace this with your actual filtered DataFrame
agg_df_filtered = agg_df[agg_df["product_sales"] < 250000]

# Sort the DataFrame by sales volume in descending order
agg_df_filtered = agg_df_filtered.sort_values(
    by="product_sales", ascending=False
)

# Calculate cumulative percentage
agg_df_filtered["cumulative_percentage"] = (
    agg_df_filtered["product_sales"].cumsum()
    / agg_df_filtered["product_sales"].sum()
    * 100
)

# Create the Pareto chart
pareto_chart = go.Figure()

# Add bar trace for sales volume
pareto_chart.add_trace(
    go.Bar(
        x=agg_df_filtered.index,  # Replace with a meaningful label column if available
        y=agg_df_filtered["product_sales"],
        name="Sales Volume",
        marker_color="blue",
        text=agg_df_filtered["product_sales"],
        textposition="outside",
        texttemplate="%{text:.2s}",
    )
)

# Add line trace for cumulative percentage
pareto_chart.add_trace(
    go.Scatter(
        x=agg_df_filtered.index,  # Replace with a meaningful label column if available
        y=agg_df_filtered["cumulative_percentage"],
        name="Cumulative Percentage",
        mode="lines+markers",
        line=dict(color="red", width=2),
        marker=dict(size=6),
        text=agg_df_filtered["cumulative_percentage"],
        textposition="top center",
        texttemplate="%{text:.1f}%",
    )
)

# Update layout
pareto_chart.update_layout(
    title="Pareto Chart: Sales Volume Distribution",
    xaxis=dict(
        title="Clusters/Keywords"
    ),  # Update the axis title based on the x-axis data
    yaxis=dict(title="Sales Volume"),
    yaxis2=dict(
        title="Cumulative Percentage",
        overlaying="y",
        side="right",
        range=[0, 110],
    ),
    legend=dict(orientation="h", xanchor="center", x=0.5, y=-0.2),
    template="plotly_white",
)

# Show the chart
pareto_chart.show()

```

</details>

#### Scatter plot

A scatter plot will help visualize the relationship between webpage traffic and product sales. Data is filtered to focus on the clusters that generate the top 80% of revenue. Additionally, the sales_to_traffic_ratio will highlight clusters that are highly efficient in converting traffic into sales.

<details class="details-bar">
<summary>Click to see script: scatter plot </summary>

```

# Remove the top outlier based on the product_sales
clusters_80_without_outlier = clusters_80[clusters_80['product_sales'] < clusters_80['product_sales'].max()]

# Create a scatter plot for the top 80% clusters without the top outlier
scatter_80_no_outlier = go.Figure()

# Add a scatter trace to the plot
scatter_80_no_outlier.add_trace(go.Scatter(
    x=clusters_80_without_outlier['detail_page_view'],  # X-axis data: Traffic volume
    y=clusters_80_without_outlier['product_sales'],  # Y-axis data: Sales volume
    mode='markers',  # Plot markers only (no lines)
    marker=dict(
        size=clusters_80_without_outlier['sales_to_traffic_ratio'] * 7,  # Size of markers based on sales to traffic ratio, scaled up by a factor of 7
        sizemode='diameter',  # Use diameter to define the size of the markers
    ),
    hoverinfo="text",  # Display custom hover text
    hovertext=clusters_80_without_outlier.apply(lambda row: f"Cluster ID: {row['cluster_id']}<br>ASINs: {row['asin_count']}<br>Hard Match ASINs: {row['hard_match_asins_count']}<br>Keywords: {row['keyword_count']}", axis=1),  # Custom hover text
))

# Update the layout of the scatter plot
scatter_80_no_outlier.update_layout(
    title="Top 80% Clusters (Without Top Outlier): Traffic vs Sales Volume",  # Title of the plot
    xaxis=dict(title="Traffic Volume"),  # X-axis title
    yaxis=dict(title="Sales Volume"),  # Y-axis title
    showlegend=False  # Hide the legend
)

# Display the scatter plot
scatter_80_no_outlier.show()
```

</details>

![postprocessing](/_images/amazon-marketing-cloud/playbooks/keyword_clustering/kc_7_traffic_vol.png)

The above scatter plot illustrates the 17 remaining clusters, providing details such as cluster ID, associated asins, and keyword count. The bubble size corresponds to the sales-to-traffic ratio, offering insights into each cluster's conversion efficiency. Notably, bigger cluster stands out with a higher sales-to-traffic ratio, suggesting it may involve premium or high-sales products. This cluster could be a priority for increasing top-line revenue, especially if scaling traffic is part of the strategy. On the contrary, clusters with less favorable sales-to-traffic ratios, despite their size, may not be the optimal focus for optimizing neutral brand revenue. This analysis guides strategic decisions by pinpointing clusters with the potential for impactful revenue optimization.

## Upload and action insights

### Export Insights

The analysis involves three datasets: Description, Search term mapping, and Product mapping dataset. These datasets are uploaded to AMC using the 1p data upload process. The 1p data upload connects user searches and product purchases with clusters. When users search for specific keywords or buy certain products, these actions are identified and mapped to relevant clusters. 'Relevant clusters' refers to predefined groups of related keywords and products that are significant for revenue generation and strategic marketing insights.

The enriched 1p data becomes the foundation for comprehensive queries, empowering the dashboard with insights like top scorecards, funnel performance, keyword details, audience insights, and more. This information is vital for executing successful Amazon campaigns and refining strategies based on real user interactions. The data reveals the right ASINs, optimal keywords, and many other deep data insights to define budget & forecast business outcomes. These insights provide everything teams need to structure, scale, and optimize their campaigns.


<details class="details-bar">
<summary>Click to see script:  export data </summary>

```

# Initialize empty lists to store cluster information, keywords mapping, and products mapping
clusters_list = []  # List to store cluster information
clusters_keywords_list = []  # List to store clusters-keywords mapping
clusters_products_list = []  # List to store clusters-products mapping

# Iterate over unique cluster_ids in clusters_80 DataFrame
for cluster_id in clusters_80['cluster_id'].unique():
    # Filter the original DataFrame df for data related to the current cluster_id
    x = df[df.cluster_id == cluster_id]
  
    # Collect information for clusters table
    clusters_list.append({
        'cluster_id': cluster_id,
        'name': f'placeholder_name_{cluster_id}',  # Placeholder name (can be generated by classification AI)
        'description': f'placeholder_description_{cluster_id}',  # Placeholder description (can be generated by classification AI)
        'keyword_count': x['customer_search_term'].nunique(),  # Number of unique keywords in the cluster
        'product_count': x['tracked_item'].nunique(),  # Number of unique tracked items (products) in the cluster
        'hard_match_product_count': x[x['is_hard_product_match']]['tracked_item'].nunique(),  # Number of hard match products in the cluster
        'paid_search_traffic': x['detail_page_view'].sum(),  # Total paid search traffic for the cluster
        'paid_search_product_sales': x['product_sales'].sum(),  # Total paid search product sales for the cluster
    })
  
    # Collect information for clusters-keywords mapping table
    for keyword in x.customer_search_term.unique():
        clusters_keywords_list.append({
            'cluster_id': cluster_id,
            'customer_search_term': keyword,
        })
  
    # Collect information for clusters-products mapping table
    for product in x['tracked_item'].unique():
        clusters_products_list.append({
            'cluster_id': cluster_id,
            'tracked_item': product,
            'is_hard_match': product in x[x['is_hard_product_match']]['tracked_item'].unique(),
        })

# Create DataFrames from the collected lists
df_clusters = pd.DataFrame(clusters_list)  # DataFrame for cluster information
df_clusters_keywords = pd.DataFrame(clusters_keywords_list)  # DataFrame for clusters-keywords mapping
df_clusters_products = pd.DataFrame(clusters_products_list)  # DataFrame for clusters-products mapping

# Display the DataFrames
display(df_clusters, df_clusters_keywords, df_clusters_products)

```

</details>

**Description dataset**

- 	Contains cluster details includes keyword count.
-	Relevance: Helps identify which clusters and keywords are most critical for revenue generation (indicating how important each keyword/cluster is in driving sales).

![postprocessing](/_images/amazon-marketing-cloud/playbooks/keyword_clustering/kc_4_cluster_scatter.png)

**Search term mapping dataset**

-  Maps clusters to specific search terms.
-  Relevance: Enables understanding of which search terms are associated with each cluster, helping to align search behavior with cluster strategy.

| \-   | cluster\_id | customer\_search\_term      |
|------|-------------|-----------------------------|
| 0    | 1           | B3RDIJD                     |
| 1    | 1           | B00GVDDFSH                  |
| 2    | 1           | B00NE8DH8P4                 |
| 3    | 1           | B3\-2JKMLCRU                |
| 4    | 1           | B0JW9DJHR                   |
| 1671 | 17          | smartphone                  |
| 1672 | 17          | high performance smartphone |
| 1673 | 17          | large smarphone             |
| 1674 | 17          | large screen smartphone     |
| 1675 | 17          | latest smartphone model     |



**Product mapping dataset**

- Maps clusters to specific products.
- Relevance: Connects products to clusters, facilitating analysis of product performance within each cluster and aiding in product-specific marketing strategies.


| \-    | cluster\_id | tracked\_item | is\_hard\_match |
|-------|-------------|---------------|-----------------|
| 0     | 1           | BSJHE43S3W    | TRUE            |
| 1     | 1           | B04O3JFSFHI   | FALSE           |
| 2     | 1           | B04KCNY6FG    | TRUE            |
| 3     | 1           | B059ENOCB7    | FALSE           |
| 4     | 1           | B04NFJCPL21   | TRUE            |
| 1456  | 17          | B08DHJDJBF5   | FALSE           |
| 1528  | 17          | B0FJFZ2P9G    | FALSE           |
| 1632  | 17          | B0884NFRL     | TRUE            |
| 5283  | 17          | B085IDND5M    | FALSE           |
| 6584  | 17          | B09KFJIRK6R   | FALSE           |  



### Action the insights

We can derive several actionable insights that can enhance the effectiveness of Amazon ad campaigns. Here are some action items advertisers can take:

- **ASIN clustering for enhanced product categorization**:

    - Provide a product category label or product name to each ASIN.
    - Upload this data into AMC to unlock analysis at the product category level, allowing for more granular insights and targeted strategies.

- **Keyword clustering for strategic campaign planning**:
    - Group customer search terms into relevant categories associated with the brand or product.
    - Identify seasonal trends, such as when customers are searching for specific product categories more often (e.g., increased searches for iPhones during the holiday season).
    - Adjust marketing strategies and budget allocation based on these seasonal trends to maximize engagement and sales.

- **Optimized budget allocation**:

    - Use the Description Dataset to identify clusters and keywords that are most critical for revenue generation.
    - Allocate a higher budget to campaigns targeting these high-performing keywords to maximize ROI.

- **Under performing keyword reallocation**:
    - Identify clusters and keywords that are under performing.
    - Reallocate budget from under-performing keywords to high-performing ones or explore alternative strategies to improve their performance.

- **Product-specific marketing strategies**:
    - Use the Product Mapping Dataset to analyze product performance within each cluster.
    - Develop targeted marketing strategies for products that perform well within specific clusters, emphasizing their strengths in ad copy and creative.

- **Inventory management**:
    - Align product inventory with high-performing clusters to ensure availability during peak demand periods identified through keyword clustering analysis.

- **Improved ad copy and creative development**:
    - Regularly update keyword and product clusters based on new data to ensure campaigns remain relevant and effective.
    - Implement an iterative optimization process where campaign performance is continuously monitored and adjustments are made based on real-time insights.

- **Cross-selling and upselling opportunities**:
    - Identify complementary products within the same cluster and create cross-selling or upselling campaigns.
    - Use insights from product clusters to recommend related products to customers who have shown interest in a specific product category.

- **Enhanced customer insights**:
    - Analyze search term and product mapping data to gain deeper insights into customer behavior and preferences.
    - Use these insights to inform broader marketing strategies, product development, and customer engagement initiatives.
