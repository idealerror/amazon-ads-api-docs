---
title: Amazon Marketing Cloud - Enhanced scoring for overlapping AMC audiences
description: Enhanced scoring for overlapping AMC audiences
type: guide
interface: api
---
# Enhanced scoring for overlapping AMC audiences

This playbook combines the powerful capabilities of Amazon Marketing Cloud (AMC) with the [persona builder API](persona-builder#persona-builder-api) to deliver comprehensive audience insights. This integration allows marketers to leverage a wide array of data sources and analytical tools for more effective audience targeting and campaign optimization.

## Introduction

The persona builder and AMC currently function independently, each utilizing its own distinct API set. This playbook will guide you through the process of integrating insights from both APIs to create a comprehensive and scalable solution.

### What is audience overlap?

Audience overlap analysis is a powerful tool that helps advertisers understand how their target audiences intersect and relate to one another. Including an affinity score to the “intersected” or overlapped audiences adds an additional level of depth to analyzing the overlapped segments. Identifying audience overlap with the scoring methodology will help you in strategizing your audience targeting, optimizing ad spend, improve targeting accuracy, and avoids audience saturation.

![flywheel_intro)](/_images/amazon-marketing-cloud/playbooks/overlap/pb_1.png)

**Leverage AMC and the persona builder API for enhanced targeting**
You can perform measurement and analytics using AMC to identify audiences and leverage the persona builder API to identify additional audience attributes across top retail categories, top overlapping audiences, estimated size , top demographics and Prime Video insights. All the results are accompanied with an affinity score that refers to the likelihood and a percentage score that refers to the percentage of the customers on the above categories.

For instance, consider a top audience category with an affinity of 5 and a percentage of 5:

- Affinity of 5: This means that customers in the persona are five times more likely to belong to this audience compared to the average Amazon customer.
- Percentage of 5: This indicates that 5% of the customers within the persona belong to this specific audience.

This interpretation helps quantify both the relative strength of the audience association (affinity) and its prevalence within the persona (percentage). Such data can be valuable for targeted marketing strategies and understanding customer segments more effectively.

These additional insights like overlapping audiences, overlapping retail categories, estimated size of the audiences, Prime Video insights, overlapping demographics information for your audiences combined with AMC results will help you build a holistic approach of audience performance for an advertiser and provide inputs for strategic media planning and optimize audience targeting groups for new campaigns or improve existing campaigns. This analysis adds added value by identifying the key audience groups and their behavioral characteristics that might fall under either a lookalike audience or a rule-based audience group.

### Benefits

AMC has audience analytics features where you can build rule-based audiences, lookalike audiences, use the Audience segment insights for identifying the segments for organic users. In addition to details from AMC, you can discover and explore the best performing audience by understanding audience behavior powered with actual purchase and streaming behaviors, with data based on consumer panels and research surveys from Amazon using Amazon’s persona builder APIs.

This playbook will answer the following questions:

- How to use the different endpoints of the persona builder API with AMC results
- How to analyze the affinity and percentage for AMC audiences that overlap with persona builder API categories?
- How to identify additional demographics attributes for existing AMC Audiences?
- What are the various use cases where information from the persona builder API and the AMC API can be combined effectively?
- How to use affinity and percentage metrics to inform audience targeting strategies?
- What do affinity and percentage scores mean for overlapping audiences?

### Intended audience

This playbook is designed for data-driven professionals adept at converting AMC performance insights into effective media activations. It targets the following personas:

- Media/AdTech analyst: Possesses media knowledge and experience managing DSP campaigns.
- Data analyst: Proficient in SQL, capable of customizing queries to meet specific advertiser needs.
- Data engineer: Skilled in constructing automated data flow pipelines for segment measurement and activation.
- Data scientist: Well-versed in scientific methodologies and assumptions necessary for conducting and applying test results.

### Prerequisites

It is recommended you understand the following concepts to get the most out of the playbook:

- **AMC concepts**: Specifically, concepts such as data aggregation thresholds, privacy limitations, and other restrictions listed in the [AMC documentation](guides/amazon-marketing-cloud/overview).
- **Knowledge of [AMC SQL](guides/amazon-marketing-cloud/amc-sql/overview).** The process outlined here relies on altering SQL code to match your purposes.
- **Basic Python**: This playbook is built using Python, and may require some scripting to adapt it to your use case.
- **Basic Amazon DSP** knowledge: You’ll need to know how to query audiences to activate upon in Amazon DSP. 

> [NOTE] This playbook assumes you have already [onboarded to the API](guides/onboarding/overview) and have a working knowledge of the AMC APIs and persona builder APIs.

### Playbook steps

We will explore several use cases to demonstrate the implementation and practical application of enhanced scoring for overlapping audiences. Each use case will follow a structured approach outlined in the steps below.

> [NOTE] The following steps describe a one-time setup process. If you intend to develop a scalable solution, use this as a blueprint for your architecture. As you develop your solution, you'll need to adapt and expand upon these concepts to meet your specific requirements and scale effectively. This may involve refining your data architecture and optimizing the integration of results from AMC APIs, audience discover APIs, and the persona builder API.

- **Step 1**: Begin with an exploratory query using AMC or use a custom query to suit your use case. This initial step involves analyzing historical campaign performance, either by leveraging the [Instructional Query Library](https://advertising.amazon.com/marketing-cloud/instructional-queries) or writing a custom query tailored to your specific use case. This helps analyze your campaign’s historical performance and identify an audience that warrant further attention and optimization . To help you get started, we have illustrated the implementation of overlapping audiences through a set of [use cases](#use-cases).
- **Step 2**: After executing step 1, use the results from AMC, to identify potential audience segments to obtain more detailed scoring, which will help to fine-tune your marketing strategy by understanding the nuances of each segment, ultimately leading to more effective and efficient campaign performance.
- **Step 3**: With the audiences identified in step 2, utilize the [audience discovery API](audiences#audiences) to extract canonical IDs for these segments.
- **Step 4**: Use the [persona builder API](persona-builder#persona-builder-api) to gain deeper insights into your audiences. By querying the API with the canonical IDs (from step 3), you'll can dive deeper into overlapping audiences, demographic details, retail category interests, Prime Video streaming interests, and estimated audience sizes.

> [NOTE] Steps 3 through Step 4 can be programmatically executed using the [AMC_PersonaAPI.zip](https://github.com/amzn/ads-advanced-tools-docs/tree/main/amazon_marketing_cloud/amc_playbooks) package containing a Jupyter notebook that contains the required code for the various use cases listed here. First download and extract the `AMC_PersonaAPI.zip` package. Make the required updates in the `configuration.ipynb` to set up the security credentials required to execute the Amazon Ads APIs. For each specific use case, execute the relevant cells within the `persona_builder.ipynb` as required.

- **Step 5**: Transform and utilize the data from step 4 to output for use in your existing platforms, or normalize to create visualizations to understand the insights. For example, create files per category of audience such as overlapped audience , demographic details , estimated size, retail categories etc. Remove all null values to ensure data integrity and standardize data types within each file for consistency.
- **Step 6**: Analyze your transformed data (step 5) to identify audiences using affinity scores and percentages to classify those as high or low value. Use the classified audiences to:

  - Target or exclude audiences in Amazon DSP campaigns
  - Inform future media planning
  - Strategize existing campaigns

  ![flywheel_intro)](/_images/amazon-marketing-cloud/playbooks/overlap/steps.png)

## Use cases

- [Identify the top 3 overlapping audiences with your AMC audience](#identify-the-top-3-overlapping-audiences-with-your-amc-audience)
- [Analyze additional audience attributes on audiences built using AMC’s rule-based audience](#analyze-additional-audience-attributes-on-audiences-built-using-amcs-rule-based-audience)
- [Leverage the Audience Segment Insights dataset to identify non ad-exposed users and their demographic audiences](#leverage-the-audience-segment-insights-dataset-to-identify-non-ad-exposed-users-and-their-demographic-audiences)
- [Gain audience insights from Prime Video data to enhance targeting strategies](#gain-audience-insights-from-prime-video-data-to-enhance-targeting-strategies)

### Identify the top 3 overlapping audiences with your AMC audience

To get you started, we'll identify the advertiser's top 3 performing audiences using AMC, create a generic persona using the persona builder API. The process then extends to exploring alternative audiences, evaluating them based on their affinity score and percentage .This comprehensive analysis will provide insights into additional attributes of existing high-value segments and identify potential new targeting opportunities, ultimately refining the advertiser's audience strategy and improving campaign performance.

##### Potential customer questions answered

- What are the overlapping audiences with my top-performing audiences that are not a part of your campaign strategy?
- Can I build a dashboard of demographic details of my top 3 performing audiences?

##### Insights and actions

1. Identify alternate audiences with high affinity to make a purchase and use them in targeting your campaign.
2. Build knowledge on existing audiences and analyze the audiences within top retail categories, demographics and lookalike audiences.
3. Use the audiences to perform targeting on Amazon DSP/Sponsored display to obtain wider reach.

#### Step 1: Query: Identify un-targeted segments for high-value customers.

Analyze the segments to identify top 3 audiences that you want to analyze the overlapping audiences for with additional demographic information and affinity scores. Use these set of audiences as an input to the persona API.

The use cases can be expanded to multiple use cases where you want to identify audience segments. You can use the following IQs to identify audiences:

- [New to brand customers](https://advertising.amazon.com/marketing-cloud/instructional-queries/1d572ebe8ca7baab0d45003ced0eb34114b565eca699a17dc6463371e0e2dbc2)
- [Users with high purchase rate](https://advertising.amazon.com/marketing-cloud/audiences/instructional-queries/8a57fb8aae239db378f43bd88dded83a0a18954000e57320ff65e4c3d42ede8e?)
- [Users purchasing during tent pole events](https://advertising.amazon.com/marketing-cloud/instructional-queries/2863325acae4bc8c36b5f04743be92f5f1f01e43040a7be1f416aac3e067d338)
- [Users that take least time to convert](https://advertising.amazon.com/marketing-cloud/instructional-queries/e4b1c626c51e5fb3365f76c54d3e3ec6da9c739b86c4a8b6614b776e208d9368)

> [NOTE] Explore the IQL for the above use cases and more, modify the IQLs by using `dsp_impressions_by_user_segments` table and bringing in the segment information.

Illustrated below is an example query that was modified using the IQL [identify audience segments for high-value customers](https://advertising.amazon.com/marketing-cloud/instructional-queries/7ba78510f7dd5c16ab8f2763c4aab180d9974efa44ee80cb747afc599dfa9ca3)

```
-- Instructional Query: High Value Custom Segments --
/*
 ------- Customization Instructions -------
 1. Update the tracked_asin filter in the purchase CTE.
 2. Either provide an advertiser to include all the ASINs for
 or provide a specific set of ASINs.
 */
WITH purchase AS (
  SELECT
    user_id,
    SUM(total_product_sales) AS total_product_sales
  FROM
    conversions_all_for_audiences
  WHERE
    total_product_sales > 0
    /* This condition helps filter in only 
     relevant purchase conversions and filter out all others */
    /* REQUIRED UPDATE [1 OF 2]: Use either one of the below ASIN filters 
     based on your needs and comment out the other one. */
    -- AND tracked_asin IN (
    --   SELECT
    --     DISTINCT tracked_asin
    --   FROM
    --     amazon_attributed_events_by_conversion_time_for_audiences
    --   WHERE
    --     advertiser_id IN ('111111111')
    --     /* Update with actual advertiser_id */
    -- )
    -- AND tracked_asin IN ('111111111', '222222222')
    /* Update with actual ASIN values */
    AND user_id IS NOT NULL
  GROUP BY
    1
),
-- Calculate rank percentile
percentile_calc AS (
  SELECT
    user_id,
    total_product_sales,
    RANK() OVER (
      ORDER BY
        total_product_sales
    ) AS rnk,
    (
      RANK() OVER (
        ORDER BY
          total_product_sales
      ) * 100
    ) / COUNT(user_id) OVER () AS Percentile
  FROM
    purchase
)
SELECT
behavior_segment_name as audience_name,
Percentile,
sum(total_product_sales) as total_product_sales,
 count( distinct c.user_id ) as reach
FROM
  percentile_calc c
  join dsp_impressions_by_user_segments d on c.user_id = d.user_id
WHERE
  /* REQUIRED UPDATE [2 OF 2]: : To modify the percentile, 
   set '50' to another integer value (between 1 and 100).
   Values outside of this range with result in a NULL resultset. */
  behavior_segment_matched = 0
  group by 1,2
  
```

**Sample output**

![top3](/_images/amazon-marketing-cloud/playbooks/overlap/pb_2.png)

#### Step 2: Identify audience from result

Identify top 3 audiences based on total product sales, reach, and percentile from the exploratory query results.

#### Step 3: Extract canonical IDs

With the audiences identified, utilize the [audience discovery API](audiences#audiences) (from the [persona_builder.ipynb](https://github.com/amzn/ads-advanced-tools-docs/tree/main/amazon_marketing_cloud/amc_playbooks)) to extract canonical IDs for these segments.

#### Step 4: Gain deeper insights using personal builder API

Then run code snippets from the `persona_builder.ipynb` to get data on:

- Overlapping audiences
- Demographic audiences
- Retail categories
- Estimated audience size

#### Step 5: Transform data

- Normalize the affinity and percentage scores to a common scale, such as 0-1 or 0-100%, to facilitate comparison.
- Address any outliers or skewed distributions at this stage to prevent them from distorting your results.
- Create visualizations to understand the output. Create scatter plots to illustrate the relationship between affinity and percentage scores or use stacked bar charts to compare percentage scores of multiple audiences.

#### Step 6: Choose appropriate audience

Choose clusters of high-performing audiences (high-affinity, high-percentage audiences). Interpret these results within the context of your campaign objectives, draw actionable insights for more effective targeting in your Amazon DSP campaigns.

![overlap)](/_images/amazon-marketing-cloud/playbooks/overlap/pb_3.png)

### Analyze additional audience attributes on audiences built using AMC's rule-based audience

Use AMC's rule-based audience feature to construct custom audience tailored to your campaign objectives. After you establish your audience, use the persona API to build personas. With these personas, you can then explore overlapping audiences to determine their affinity and percentage scores. You can expand this use case to build any custom audience leveraging the existing IQLs and building a custom rule-based audiences example leverage the [Introduction to Events Manager IQ](https://advertising.amazon.com/marketing-cloud/instructional-queries/f28930f6d139cc028439497753c1eb1e6cbef52e49a108df56072c1f6b36d404) and build an audience for users who perform off-Amazon conversions and perform the steps below to identify additional audience attributes.

##### Potential customer questions answered

- Can refining audience lists eliminate spend waste in campaigns?
- What's the persona detail of my cart-abandoner audience?
- Can affinity and percentage overlap help expand seed audiences for lookalikes?

#####  Insights and actions

- Use high affinity and percentage overlaps to find similar to custom audiences you created (e.g., cart abandoners).
- Analyze demographic details of custom audiences (e.g., top purchasers).
- Identify similar audiences with high affinity and percentage to negatively target those audiences (e.g., users who have seen the ad multiple times but have not made a purchase).

#### Step 1: Use the IQ base to build rules-based or lookalike audiences using AMC audiences

Navigate to the Instructional queries tab of the AMC console filter through Audiences IQs to find a use case that aligns with your specific requirements.

#### Step 2: Select an audience IQ, and build an audience to suit your requirement.

For more details, see [Create a rule-based audience](https://advertising.amazon.com/help/GXCLLP4FBHRE2PDD).

#### Step 3: Extract canonical IDs

With the audiences identified, utilize the [audience discovery API](audiences#audiences) (from the persona_builder.ipynb) to extract canonical IDs for these segments.

#### Step 4: Gain deeper insights using personal builder API

Then run code snippets from the `persona_builder.ipynb` to get data on:

- Overlapping audiences
- Retail categories
- Demographics
- Prime Video

#### Step 5: Transform data

- Normalize affinity and percentage scores to a common scale (e.g., 0-100) if necessary and addressing any outliers or anomalies in the data.
- Create visual representations to illustrate the relationships and overlaps between audiences.

This transformation will help to understand the affinity scores and analyze percentage scores to quantify the overlap between audiences.

The image below is a representation of a rule-based audience of **AMC Repurchases - Cross Brand** overlapped with Amazon Demographic audiences.

![cross_brand](/_images/amazon-marketing-cloud/playbooks/overlap/pb_4.png)

#### Step 6: Choose appropriate audience

Select high-affinity, high-percentage audiences for DSP targeting. Incorporate top-converting audiences while also including broader, high-affinity audiences to expand campaign visibility.

### Leverage the Audience Segment Insights dataset to identify non ad-exposed users and their demographic audiences

Identify non ad-exposed users from the [Audience Segment Insights (ASI) dataset](guides/amazon-marketing-cloud/datasources/audience_segment_membership_paid). Create a persona for this group using the persona API to determine their demographics. Analyze the affinity and percentage scores targeting opportunities for these previously unreached audiences.

##### Potential customer questions answered

- Can I measure demographic details of the lookalike audiences I created?
- Can I increase the affinity of an audience over time?

##### Insights and actions

- Identify high-affinity, high-percentage alternative audiences for non-ad-exposed and untargeted groups.
- Use Persona API to identify demographics, overlapping audiences, and retail categories for these groups.
- Leverage affinity and percentage data to target audiences based on campaign strategy, especially for creating wider reach, awareness, and consideration.
- Adjust Amazon DSP retail category targeting based on these insights.

#### Step 1: Instructional Query: Audience Segment Insights

Audience Segment Insights (ASI) consists of datasets that provide information on public Amazon standard catalog audiences, which include Amazon audience segments across in-market, lifestyle, interests, and life events categories.

Refer to the [Introduction to Audience Segment Insights IQ](https://advertising.amazon.com/marketing-cloud/instructional-queries/1d64b30c50021f173b35384393cb07964bb3f226cf348f58cd0d53ce7542064f) to get started.

> [NOTE] You must be subscribed to **Amazon insights** to use the datasets the IQ references. In the AMC console, navigate to the Paid features tab to subscribe to Amazon insights. Paid features are only available in certain regions. If you're unable to see the Paid features tab, reach out to your AdTech Account Executive.

#### Step 2: Identify non ad-exposed users from the Audience Segment Insights dataset

Use the query in the IQ to identify audiences to include in your campaigns.

#### Step 3: Extract canonical IDs

With the audiences identified, utilize the [audience discovery API](audiences#audiences) (from the persona_builder.ipynb) to extract canonical IDs for these segments.

#### Step 4: Gain deeper insights using personal builder API

Then run code snippets from the `persona_builder.ipynb` to get data on:

- Demographics
- Overlapping audiences
- Retail categories

#### Step 5: Transform data

- Normalize affinity and percentage scores to a common scale (e.g., 0-100) if necessary and addressing any outliers or anomalies in the data.
- Create visual representations to illustrate the relationships and overlaps between audiences.

This transformation will help to understand the affinity scores and analyze percentage scores to quantify the overlap between audiences.

**Sample output : Retail categories**

![retail_categories](/_images/amazon-marketing-cloud/playbooks/overlap/pb_5.png)

#### Step 6: Choose appropriate audience

Choose high-affinity, high-percentage audiences for more effective targeting in your DSP campaigns.

### Gain audience insights from Prime Video data to enhance targeting strategies

To enhance targeting strategies using Prime Video data, begin by constructing exploratory queries to analyze Prime Video campaigns and their reached audiences. Analyze these audiences to gather detailed viewership signals for these audiences, including their preferences for specific actors, directors, genres, movies, and series.

For each audience, evaluate the affinity and percentage scores associated with these viewership signals. Use this analysis to understand audience preferences and behaviors, and refine your targeting strategies for more effective campaign performance.

##### Insights and actions

You can leverage signals in AMC to understand the performance of your existing audience reached by Prime Video ad campaigns. Enhance targeting by analyzing the overlapping performance across stars, series, movies, and genres.

##### Potential customer questions

- What kind of Prime Video insights are available using the persona API?
- How can I apply these insights to enhance my existing Prime Video campaigns?

#### Step 1: Prime Video exploratory query

Refer to the instructional query on Prime Video ads. Below is a sample to build a lookalike audience reached through Prime Video campaigns and identifying the viewership details for that audience.

Use this query below to create a lookalike audience for users reached by Prime Video ads and analyze their attributes to target those audiences in the new or existing campaigns.

> [NOTE] This is an exploratory query and needs to be updated to be used in AMC’s audiences’ query editor.

```
--Identify the reach for STV campaigns

WITH stv_reach AS (
SELECT
DISTINCT user_id
FROM
dsp_impressions
WHERE

line_item_type = 'AAP_VIDEO_CPM'

),

--Identify the reach for prime video campaigns 
--This can be expaned to detail page views, customer reviews etc.
pva_reach AS (
SELECT
DISTINCT a.user_id
FROM
dsp_impressions a join amazon_attributed_events_by_conversion_time b on a.user_id = b.user_id
WHERE
a.supply_source = 'Prime Video ads'

)
-- Create an audience for the incremental reach audiences for cart abandoners
SELECT
COUNT(user_id) AS pva_incremental_reach

FROM
pva_reach
```

**Sample Output:**

![pv](/_images/amazon-marketing-cloud/playbooks/overlap/pb_6.png)

#### Step 2: Gather detailed viewership signals for these audiences

Include preferences for:

- Actors
- Directors
- Genres
- Movies
- Series

#### Step 3: Extract canonical IDs

With the audiences identified, utilize the  [audience discovery API](audiences#audiences) (from the persona_builder.ipynb) to extract canonical IDs for these segments.

#### Step 4: Gain deeper insights using personal builder API

Then run code snippets from the `persona_builder.ipynb` to get data on:

Audience preferences for specific actors, directors, genres, movies, and series.

#### Step 5: Transform data

Transform data focusing on affinity and percentage scores.

- Analyze the dataset
- Identify any null values
- Clean and sort the dataset
- Eliminate any audiences with over-indexing or under-indexing affinity and percentage scores.

#### Step 6: Choose appropriate audience

Analyze these audiences based on affinity scoring and percentage classify the audiences as high value or low value

Evaluate affinity and percentage scores for each viewership signal per audience and realign your targeting.

#### Next steps

After you have finalized on audiences, use  POST operation of the `/amc/audiences/query resource to create the audience`. After your audience is created and activated, you can [view and use your audience](guides/amazon-marketing-cloud/audiences/rule-based-audiences#view-and-use-your-custom-audiences) in Amazon DSP or in your sponsored ads accounts.
