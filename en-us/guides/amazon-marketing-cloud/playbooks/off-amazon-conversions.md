---
title: Amazon Marketing Cloud - Off-Amazon conversions
description: Off-Amazon conversions
type: guide
interface: api
---
# Off-Amazon conversions

Off-Amazon event conversions from first-party data (1P data) upload, Conversion API (CAPI), Amazon Ad Tags (AAT), or third-party data sets, such as Experian or Foursquare provide for precise measurement for off-Amazon conversions. 

## What are off-Amazon conversions?

Off-Amazon conversions in the context of this playbook include: sales, website visits, booking an appointment, visiting a physical store location, signing up for a mailing list, and many others. When this data is available in AMC, it allows for tracking the performance of campaigns across many KPIs and unlocks several use cases, such as Multi-Touch Attribution (MTA) and customer long term value (CLTV). Dimensional data is also supported, opening up use cases such as ASIN groupings, margin, etc., or user-level segmentation using off-Amazon segmentation.

This playbook provides several AMC Instructional Queries (IQs) originally designed for on-Amazon conversions, but modified for off-Amazon conversions. There are caveats to using off-Amazon data; the code samples used in here have been adjusted for successful implementation. Both event-level and non-event data are supported.

> [NOTE] This playbook references several AMC Instructional Queries. Links to the AMC Instructional Query Library (IQL) content is available only to users of the AMC UI. If you have an [AMC UI account](https://advertising.amazon.com/solutions/products/amazon-marketing-cloud/), you must log in to access this content.

This playbook is mainly focused around off-Amazon conversions, but in the SQL logic, data from both the off-Amazon conversion table and on-Amazon conversions could be UNION-ed together to create one larger success table.

## Use cases covered in this playbook

This playbook contains four "swim lanes" each with one or more use cases that answer specific questions and have direct actions. Each use case should be treated as a starting point; the SQL code requires customization to fit the particular off-Amazon data available and the specific problems trying to be solved. 

There are use cases for both fact and dimensional data; sample schema for uploaded data is provided in [Schema assumptions](#schema-assumptions). The fact data schema is assumed to have user information, timestamp, and conversion type. While the dimension data will have categorical data about ASINs or users. 
 

| Swim lane                                   | Answers questions like:                                                   |
|---------------------------------------------|---------------------------------------------------------------------------|
| #1: Amazon DSP Customer Segment Finder      | What are the demographics and shopping habits of my off-Amazon customers? |
|                                             | What are Amazon DSP audience that I should use?                           |
| #2: Measure and optimize existing campaigns | How can I use off-Amazon data with existing campaigns?                    |
|                                             | How are my campaigns performing?                                          |
|                                             | How can I optimize my campaign?                                           |
| #3: Create audiences                      | How can I activate using my off-Amazon data?                              |
| #4: Analyze segments                            | What can be done with categorical data?                                   |
|                                             | Which product categories are performing well?                             |
|                                             | Which of my off-Amazon user segments are performing the best?             |




### Example visual

Using off-Amazon data, you can calculate the conversion rate by campaign and device type. An example visual that can come from this playbook is:

![Example visual](/_images/amazon-marketing-cloud/playbooks/offamazon_conversions/playbook_off_amazon_conversions_visual.png)

In the visual above, it’s clear that the campaign “Jack & Jill test campaign” has a strong device type component. A possible action would be to modify the Amazon DSP campaign to focus on Phone vs PC. See the [Use case: Device analysis](#use-case-device-analysis) for more details. 

## Prerequisites

### Technical

- **AMC concepts**, specifically data aggregation thresholds, privacy limitations, and other restrictions listed in the [AMC documentation](https://advertising.amazon.com/API/docs/en-us/guides/amazon-marketing-cloud/overview).
- **Knowledge of [AMC SQL (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries/f53b1ad1359ef811fcdccdc8c6e69422b83bf06a163da53fddf79b6ca729edb6).** The processes outlined here relies on altering SQL code to match your purposes.
- **AMC Data Upload Knowledge** Understand data upload to AMC, user resolution, and how to query uploaded data.
- **Basic knowledge of Amazon DSP** to activate upon audience segmentation in the advertising market based on performance. You will need to know how to query audiences to activate in Amazon DSP.

### Data

- **Off-Amazon Data** Conversions not tracked by Amazon, multiple off-Amazon data sets can be UNION-ed together, or combined with on-Amazon conversions to create a holistic dataset.

### Marketing

- **Attribution Time Window** Knowledge of the appropriate lookback window for customer journeys as it relates to your specific use case. The default is 14 days in AMC.

### Related Playbooks

- [Programmatic audience framework](guides/amazon-marketing-cloud/playbooks/programmatic_audience_framework) - Provides an API framework for creating, monitoring, and measuring audience performance. 
- [Customer long-term value](guides/amazon-marketing-cloud/playbooks/CLTV) - Allows you to assesses customer value over the course of the brand relationship. Also, you can refer to [Use case: Customer long-term value](#use-case-customer-long-term-value-cltv) in this playbook.

## Assumptions

We assume that you have an understanding of the process to create multiple audiences by submitting multiple API calls and track the audience creation manually, which is described in the [Rule based audience creation](guides/amazon-marketing-cloud/audiences/rule-based-audiences).

> [NOTE] Rules-based audiences require a few constraints for the audiences to meet the size minimum requirements to be activated in the marketplace. Currently, the minimum is 2000 distinct `user_id`s for a rules-based audience to be activated in the market. Additionally, when activating the audiences, there are metrics for affinity and audience size, but that does not necessarily indicate an overlap between audiences.

**Costs associated with implementing this playbook**

- Submitting an audience to be created is not charged. There is no cost until the audience is activated in Amazon DSP.

## Playbook steps

1. [Upload data](#1-upload-data)
2. [Learn how to use 1P data in queries](#2-learn-how-to-use-1p-data-in-queries)
3. [Pre-swim lanes: First-party data overview](#3-pre-swim-lanes-first-party-data-overview)
4. [Use cases and actions](#4-use-cases-and-actions):

  - [Swim Lane #1: Find Amazon DSP customer segment](#swim-lane-1-find-amazon-dsp-customer-segment)
  - [Swim Lane #2: Measure and optimize existing campaigns](#swim-lane-2-measure-and-optimize-existing-campaigns)
  - [Swim Lane #3: Create audiences](#swim-lane-3-create-audiences)
  - [Swim Lane #4: Analyze segments](#swim-lane-4-analyze-segments)

![Swim lanes](/_images/amazon-marketing-cloud/playbooks/offamazon_conversions/NonEndemicPlaybook-External_swimlanes_all.png)

## 1. Upload data

To upload off-Amazon data to AMC, you must first upload a file containing the data to an AWS S3 bucket and then, using [AMC Advertiser data upload APIs](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-overview) or an [Amazon Marketing Cloud Uploader from AWS](https://aws.amazon.com/solutions/implementations/amazon-marketing-cloud-uploader-from-aws/), upload the data in the file to an AMC instance. The following section will run through the process and options. 

### Fact vs dimension

There are two types of data that can be loaded into AMC: fact and dimension. 

- **Fact:** In AMC, fact data is defined as any dataset that has an event timestamp. Fact data is generally transactional data; examples include sales, store visits, website visits, etc., with an exact timestamp for the event. 
- **Dimension:** In AMC dimensional data is any dataset without a timestamp. Some examples of dimensional data are: ASIN metadata, user segmentation etc. Any dataset where time is not an element. 

Most use cases are built around fact data, where the timestamp can be used to determine where in the touchpoint chain an event occurred, which will be used for attribution.

> [NOTE]  Although neither fact nor dimension require user information, it is very common for fact data to include user data. If not, the use cases are limited if user-data is not available.

### Data preparation

To resolve a user to an Amazon `user_id`, user identifiers must be standardized, hashed, and uploaded. There are two tools to help with the data preparation process:

- File preparation tool 
  - Focuses on data preparation (SHA-265 hashing). Refer to [Create an advertiser hashed audience](https://advertising.amazon.com/help/GEJKNQAJBWJXN9X3) in the Amazon DSP section of Support Center. This link is only accessible when you are logged into Amazon DSP in the advertising console.
- [Amazon Marketing Cloud Uploader from AWS](https://aws.amazon.com/solutions/implementations/amazon-marketing-cloud-uploader-from-aws/) 
  - Supports end-to-end data upload process, see next sections for more details.

> [NOTE]  If data is not properly prepared it can lower the number of matched identities.

### Supported user identifiers

ID resolution is the process of matching the identifiers in the uploaded file to a corresponding Amazon user record. This allows you to join your uploaded data with Amazon data at the user level. Refer to the [Data preparation](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-prepare-data#id-resolution-and-supported-identifiers) page for a list of supported identifiers. 

> [NOTE]  The identifiers must be standardized, hashed and uploaded according to the [Data preparation](#data-preparation) section. 

#### LiveRamp and MAIDs

LiveRamp requirements

| Provider | Identifier type   | Encoding               | Example                                      |
|----------|-------------------|------------------------|----------------------------------------------|
| LiveRamp | LiveRamp (RampID) | Must be Amazon-encoded | XY1975TDDOTfZgJdgiYmO2F1XzdrmpoyIEb7ezpCqsui |

When a dataset is defined with a LiveRamp column, AMC will attempt to resolve those IDs to an Amazon ID. The Amazon ID will be used to join with existing datasets.
 
1. At this time, AMC does not have direct integration with LiveRamp. Customers must work directly with LiveRamp to generate offline files.
2. AMC can only resolve Amazon-encoded RampIDs. Please work with your LiveRamp representative to ensure your data is properly encoded before uploading to AMC.
3. LiveRamp is uploaded using AMC's data upload APIs.

### Data upload steps

There are two ways to upload data to AMC: [AMC Advertiser data upload (ADU) APIs](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-overview) or the [Amazon Marketing Cloud Uploader from AWS](https://aws.amazon.com/solutions/implementations/amazon-marketing-cloud-uploader-from-aws/). The uploader is built on AMC ADU APIs but provides automation and UI elements. The table below offers a comparison between the options: 

|   Step #      |      Step Description                                |   API - Work Type              |   API - Coding Required?                     |       AMC Uploader from AWS - Work Type                            |  AMC Uploader from AWS - Coding Required?                                        |
|--------|--------------------------------------|-----------------|------------------------|-----------------------------------|------------------------------------------|
| 1      | Create S3 Bucket                     | Manual process  | No                     | Manual process                    | No                                       |
| 2      | Apply proper S3 Bucket Policy        | Manual process  | No                     | Manual process                    | No                                       |
| 3      | Upload data to S3                    | Manual process  | No                     | Manual process                    | No                                       |
| 4      | Prepare data                         | -               | -                      | -                                 | -                                        |
| 4.1    | Hash Data                            | Manual process  | Yes                    | Automated                         | No                                       |
| 4.2    | Standardize                          | Manual process  | Yes                    | Automated                         | No                                       |
| 5      | Create "data set" (schema of data)   | Manual process  | Yes                    | User interface                    | No                                       |
| 6      | Trigger Upload                       | Manual process  | Yes                    | User interface                    | No                                       |
| 7      | Select update strategy               | Manual process  | Yes                    | User interface                    | No                                       |
| 8      | Monitor upload                       | Manual process  | Yes                    | User interface                    | No                                       |
| 9      | Event-driven architecture (Optional) | Manual process  | Yes                    | Automated                         | No                                       |

### Expected match rate

There is no expected match rate when uploading user data. Match rates can be as low as 10%, or upwards of 90%, it depends on whether Amazon can resolve the information to a user. There is not much that can be done to troubleshoot, or diagnose match rates that are unexpected. Proper preparation of data can help. Refer to [Prepare data](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-prepare-data) for the complete steps. If the AMC Uploader from AWS was used, these steps have been handled automatically.

### Updating data

A common approach for keeping the data in AMC fresh is to use an “event-driven architecture”. With this approach, when a new file is placed in a specific location or follows a specific naming convention, a pipeline is automatically kicked off. In this scenario, when you place a new file in the S3 bucket, it will be automatically uploaded to the correct AMC table. The Amazon Marketing Cloud Uploader from AWS supports this method, otherwise it will need to be built out to meet the specific use case required. 

If event-driven is not used, data can be updated using ad-hoc uploads or any other framework. 

### Update strategies

The [updateStrategy](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-upload#update-strategies) parameter allows you to specify how the new data will be merged with existing data in the dataset. The best approach will depend on the data uploaded, how it is generated, intended functionality and many other factors. The best update strategy will depend fully on the situation.

## 2. Learn how to use 1P data in queries

Once 1P data is loaded into AMC, it can be treated just like any other AMC table. You can use JOINs, aggregations, SELECT statements, GROUP BY statements, etc. on any fields to build a data model. The same aggregation and output restrictions apply. The table is available in the UI under the **Advertiser uploaded** section of the Schema explorer in the AMC UI.

![Example first-party table in AMC](/_images/amazon-marketing-cloud/playbooks/offamazon_conversions/1pd_table.png)

See a simplified data model of the [Geography](#use-case-geography) use case as an example of how data from AMC can be combined with 1P data to create an analysis:

![Example combining data](/_images/amazon-marketing-cloud/playbooks/offamazon_conversions/combining_data.png)

### Schema assumptions

The challenge with 1P data is that every situation will be different. Each situation will have different dimensions, metrics, aggregation levels and use cases. The following section provides important call-outs for 1P data that should be known up front, as well as a baseline of what 1P data is expected. All use cases and queries are built assuming the schema below; this will likely not exactly match the 1P data you are using. Queries should be updated and modified to match each specific situation. 
 
#### Caution

- All 1P data is treated with sensitivity of “high”, which means the [aggregation threshold](guides/amazon-marketing-cloud/aggregation-threshold) is 100 distinct users. Going down to too granular of level can cause redaction.
- Depending on the use case, AMC dimensions, and number of resolved users, some dimensions will be potentially problematic with redaction. Some examples are:

  - Line Item
  - Creative
  - Postal code
  - Timestamps by hour or day

- Not all off-Amazon users will resolve to an Amazon `user_id`. Use [Pre-swim lane: First-party data overview](#3-pre-swim-lanes-first-party-data-overview) to gain insight into identity resolution rates. 
- Experian and Foursquare are currently not able to be used in custom audiences (rule-based audiences and lookalike audiences). 
- This playbook will not cover Amazon DSP integrations. Automated integration could be built using [Amazon DSP APIs](guides/dsp/developer-guide) or by using the Amazon DSP UI to modify campaigns. 
- Events Manager data - AAT and CAPI are both attributed in the amazon_attributed_events_by_* tables. If there is no 1P data uploaded to AMC, some of these use cases can be adjusted to use Amazon attribution tables to simplify the SQL code. 

### First-party data schema

#### Fact data - Event-level data with a timestamp

Uploaded in a format similar to:

| hashed_email | hashed\_first\_name | hashed\_last\_name | other\_hashed\_user\_information | conversion\_name | ConversionTimeStamp | Sale price - Optional |
|--------------|-------------------|------------------|-------------------------------|-----------------|---------------------|-----------------------|
| ----------   | ----------        | ----------       | ----------                    | Store Visit     | YYYY-MM-DD HH:MM:SS |                       |
| ----------   | ----------        | ----------       | ----------                    | Purchase        | YYYY-MM-DD HH:MM:SS | 25.99                 |
| ----------   | ----------        | ----------       | ----------                    | Website Visit   | YYYY-MM-DD HH:MM:SS |


#### Dimension data - Categorical data without a timestamp

Uploaded in a format similar to:

| campaign_id   | Campaign Category | Campaign Owner | Vertical | PromotedAsin   | ... |
|---------------|-------------------|----------------|----------|----------------|-----|
| 1316262122567 | Evergreen         | 123@123.com    | Kitchen  | ASIN1          | ... |
| 4077043708656 | Evergreen         | 123@123.com    | Kitchen  | ASIN2          | ... |
| 7826615377607 | Prime Day         | 123@123.com    | Sports   | [ASIN3,ASIN4]  | ... |
| 4832407182165 | Prime Day         | 123@123.com    | Sports   | [ASIN1, ASIN2] | ... |


#### Events Manager schema

Can be queried from AMC in a format:

| conversion\_dt\_utc        | conversion\_event\_source\_name | conversion\_event\_name | event\_type\_description | user_id | off\_amazon\_conversion\_value | off\_amazon\_product\_sales |
|--------------------------|------------------------------|-----------------------|------------------------|---------|-----------------------------|--------------------------|
| 2024-02-21T23:59:59.959Z | Conversions API              | AddToCart             | Add to Shopping Cart   | xxxx    | 10.99                       |                          |
| 2024-02-21T23:59:59.959Z | Amazon Ad Tag                | Off-AmazonPurchases   | Product Purchased      | xxxx    |                             | 10.99                    |
| 2024-02-21T23:59:59.959Z | Amazon Ad Tag                | Off-AmazonPurchases   | Product Purchased      | xxxx    |                             | 15                       |
| 2024-02-21T23:59:59.959Z | Conversions API              | AddToCart             | Add to Shopping Cart   | xxxx    | 15                          |

#### Paid Features schema

Can be queried from AMC once signed up. 

**Experian**

| purchase\_date | user\_id | conversion\_name |
|---------------|---------|-----------------|
| 1/25/2024     | xxxxx   | New             |
| 1/25/2024     | xxxxx   | Used            |

**Foursquare**

| visit\_time | user\_id | conversion\_name |
|------------|---------|-----------------|
| 1/25/2024  | xxxxx   | chain_id123     |
| 1/25/2024  | xxxxx   | chain_id456     |

#### Combined data

| Source        | conversion\_dt\_utc | off\_amazon\_product\_sales | conversion\_name | user\_id |
|---------------|-------------------|--------------------------|-----------------|---------|
| Events Manager | 1/25/2024         | 0                        | Website Visit   | xxxxx   |
| Events Manager | 1/25/2024         | 25.99                    | Purchase        | xxxxx   |
| Events Manager | 1/15/2024         | 0                        | add to cart     | xxxxx   |
| Data Upload   | 1/15/2024         | 0                        | Store Visit     | xxxxx   |
| Data Upload   | 1/15/2024         | 13                       | Purchase        | xxxxx   |
| Data Upload   | 1/15/2024         | 0                        | Website Visit   | xxxxx   |
| Foursquare    | 1/25/2024         | 0                        | Walmart         | xxxxx   |
| Foursquare    | 1/25/2024         | 0                        | Walmart         | xxxxx   |
| Experian      | 1/25/2024         | 0                        | New             | xxxxx   |
| Experian      | 1/25/2024         | 0                        | Used            | xxxxx   |



## 3. Pre-swim lanes: First-party data overview

**Overview:**

Provide insight into the off-Amazon data by conversion name and user resolution to Amazon `user_id`s. This information is valuable as a minimum threshold is required to make rule-based audiences and lookalike audiences. Along with audiences, data that is sparse will face redaction issues with other use cases. First-party data overview is pre-swim lanes as it is relevant to all future use cases. 

**Questions Answered:**

- What does my off-Amazon data look like?
- How many users resolved for each conversion type?
- Do I have enough users for custom audiences? 

**Actions:**

- Create lookalike or rule-based audiences using one or many conversion types
- Select use cases and dimensions that are likely to succeed given the resolved user density 

**Integrations:**

- Lookalike and rule-based audiences (RBA) API integration

**Example:**

| Source      | conversion\_name | Total Records | Resolved IDs | Unique Resolved IDs | % Resolved | SufficientForRBA | SufficientForLAL\_min | SufficientForLAL\_max |
|-------------|-----------------|---------------|--------------|---------------------|------------|-----------------|---------------------|---------------------|
| Data Upload | Store Visit     | 10,000        | 2,000        | 1,900               | 20.00%     | N               | Y                   | Y                   |
| Data Upload | Purchase        | 20,000        | 9,000        | 8,000               | 45.00%     | Y               | Y                   | Y                   |
| Data Upload | Website Visit   | 50,000        | 25,000       | 19,000              | 50.00%     | Y               | Y                   | Y                   |

**SQL**

<details class="details-bar">
  <summary>Click to see SQL code</summary>

```
/*
AMC Playbook: Off-Amazon conversions
Use case: Pre-swim lanes: First-party data overview
1. Replace [1PD_Table] with your first-party table name.
2. Replace [conversion_name] with the conversion column in the 1PD_Table
3. This query requires a user_id column in the 1PD_Table
*/

--- USE CTE to prevent duplicate calculations
WITH pre_calc AS (
SELECT 
[conversion_name], 
COUNT(1) as totalRecords,
SUM(CASE WHEN user_id is not null then 1 else 0 END) as ResolvedIDs,
COUNT(DISTINCT USER_ID) as UniqueResolvedIDs
FROM 
[1PD_Table]
GROUP BY 1)

SELECT 
[conversion_name],
totalRecords,
ResolvedIDs,
UniqueResolvedIDs,
ResolvedIDs/ totalRecords as PercentResolved,
CASE WHEN UniqueResolvedIDs > 2500 then 'Y' else 'N' END AS SufficientForRBA,
CASE WHEN UniqueResolvedIDs > 1000 then 'Y' else 'N' END AS SufficientForLAL_min,
CASE WHEN UniqueResolvedIDs < 50000 then 'Y' else 'N' END AS SufficientForLAL_max
FROM pre_calc
```

</details>

## 4. Use cases and actions

### Swim Lane #1: Find Amazon DSP customer segment

![Swim lane 1](/_images/amazon-marketing-cloud/playbooks/offamazon_conversions/NonEndemicPlaybook-External_swimlanes_1.png)

#### Use case: Overlap analysis

**Overview:**

Provides insight into off-Amazon data around user demographics and behaviors, allowing for insight into existing customer base. This use case also provides insight into which Amazon DSP audiences are most likely to be high-value or expand reach. 

There are two options for pulling data: 

- [Using Persona builder API](#overlap-analysis-using-persona-api)
- [AMC Overlap using paid data set Audience Segments Insights (ASI)](#overlap-analysis-using-asi-part-of-fsi---paid-features)

**Questions Answered:**

- Who are my customers?
- What do my customer purchase?
- How do I identify high potential audiences?

**Suggested user counts:**

- Greater than 2,000 unique users to create a rule-based audience

**Actions:**

- Identify which existing Amazon DSP audiences are high potential. 
- Activate in Amazon DSP using newly identified audiences, then measure and repeat. 
- For advertisers new to AMC:
  - Provide a starting point for audiences to activate. 
  - Solve the cold start problem.

**Integrations:**

- Persona builder API - Optional

#### Overlap analysis using Persona builder API

**Overview**

[Persona builder API](guides/dsp/persona-builder) providers information on overlapped audiences, demographics, and retail categories on any active Amazon DSP audience. The API can be used on both Amazon audiences and custom audiences. To get the metrics for off-Amazon data the recommended approach would be to create a rule-based audience around each conversion type and then run the Persona builder API against each of those custom Amazon DSP audiences. 

> [NOTE] There is no cost to creating rule-based audiences; only cost associated with activating the audience. This step can be done without intent to activate or cost.

**Reference documentation**

- [Persona builder API documentation](guides/dsp/persona-builder)

**First-party data overview** 

1P data terms to know:

- **Affinity:** Affinity is a measure of how likely customers in the persona are to belong to this audience. An affinity of 5 indicates that customers in the persona are 5 times as likely to belong to this audience than the average of customers on Amazon.
- **Percentage:** Percentage of customers in the persona who belong to this audience. For example, a value of 5 means 5% of customers in the persona belong to the specified audience.

The following tables provide example Persona builder API results.

**Top Overlapping Audiences - example results**

| category     | id      | name                        | Affinity | Overlap Percentage |
|--------------|---------|-----------------------------|---------|--------------------|
| Custom-built | 111111  | AMC RBA123345               | 4170.61 | 80                 |
| Custom-built | 222222  | AMC LAL - ABCDE             | 81.62   | 18                 |
| Lifestyle    | 333333  | LS - Toothbrush             | 6.18    | 31                 |
| In-market    | 4444444 | IM - Ginger Ale Soft Drinks | 6.15    | 17                 |

**Demographics - example results**

| percentage | affinity | attribute | key               |
|------------|----------|-----------|-------------------|
| 20         | 3        | 18-24     | age               |
| 7          | 0        | 25-34     | age               |
| 13         | 3        | 45-54     | age               |
| 41         | 1        | 55-64     | age               |
| 21         | 1        | 35-44     | age               |
| 41         | 1        | Owning    | propertyOwnership |
| 36         | 0        | Renting   | propertyOwnership |
| 12         | 0        | Male      | gender            |
| 45         | 3        | Female    | gender            |
| 29         | 1        | Couple    | relationship      |
| 3          | 3        | Single    | relationship      |
| 18         | 2        | 100,000−149,999    | Income   |

**Retail Categories (Top Categories Purchased) - - example results**

| percentage | affinity | id     | path                                              | name                |
|------------|----------|--------|---------------------------------------------------|---------------------|
| 42         | 7.99     | 11111  | [Health & Household, Products, Oral Care, Toot... | Manual Toothbrushes |
| 12         | 4.44     | 222222 | [Beauty & Personal Care, Products, Tools & Acc... | Cotton Swabs        |
| 10         | 4.22     | 333333 | [Beauty & Personal Care, Products, Shave & Hai... | Disposable Razors   |
| 11         | 3.7      | 44444  | [Health & Household, Products, Household Suppl... | Dryer Sheets        |

**Prime Video Insights**
Results from this endpoint are split into categories: actors, directors, genres, movies, series.

**Example values:**
Actors

| Actor              | Affinity | OverlapPercentage |
|--------------------|----------|-------------------|
| Christopher Briney | 1.61     | 13                |
| Terry Crews        | 1.59     | 18                |
| Christine Baranski | 1.59     | 16                |
| Lola Tung          | 1.59     | 15                |
| Jackie Chung       | 1.58     | 15                |
| Adam Devine        | 1.58     | 14                |

Genres

| Genre                           | Affinity | OverlapPercentage |
|---------------------------------|----------|-------------------|
| Sci-Fi/Fantasy > Sci-fi - Alien | 29.94    | 0                 |
| Faith and Spirituality - Islam  | 29.79    | 0                 |
| Horror > Thriller               | 28.17    | 0                 |
| Sport > Water Sports            | 15.41    | 0                 |


 
#### Overlap analysis using ASI

**Overview**

To use the overlap report using Audience Segment Insights (ASI), an active FSI subscription is required. The ASI based analysis will provide overlap between 1P data table and Amazon DSP segments. 

> [NOTE]  You must update the SQL code use or UNION the 1P data table instead of “conversions”.

**Reference IQs**
 
- [Introduction to Audience Segment Insights (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries/1d64b30c50021f173b35384393cb07964bb3f226cf348f58cd0d53ce7542064f)
- [Audience Overlap Analysis (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries/47c61ae2fe6b61ff70137001cefd3825c4d7f664bb5d9ada57ab2c663d03c973)


| conversion_name | segment_name                          | totalAdExposedUsersPerSegment | distinctusers | totalconversions | totalimpressions | totalclicks | totalcost | costperclick | conversionrate | costperconversion |
|-----------------|---------------------------------------|-------------------------------|---------------|------------------|------------------|-------------|-----------|--------------|----------------|-------------------|
| Purchase        | IM - Toothpaste                       | 1,000,000                     | 100,000       | 30,000           | 1,500,000        | 12,000      | $45,000   | $3.75        | 10%            | $1.50             |
| Store Visit     | IM - Manual Toothbrushes              | 400,000                       | 40,000        | 100,000          | 450,000          | 15,000      | $10,000   | $0.67        | 10%            | $0.10             |
| Purchase        | IM - Children's Dental Care Products  | 300,000                       | 30,000        | 85,000           | 310,000          | 11,000      | $1,000    | $0.09        | 10%            | $0.01             |
| Store Visit     | IM - Children's Electric Toothbrushes | 10,000                        | 800           | 25,000           | 100,000          | 1,000       | $3,500    | $3.50        | 8%             | $0.14             |

**SQL**

> [NOTE]  All metrics will be duplicated across the number the conversion names and Amazon DSP segments. This analysis is for finding overlap only and not reporting due to the duplication.

<details class="details-bar">
  <summary>Click to see SQL code</summary>

```
/*
AMC Playbook: Off-Amazon conversions
Use case: Overlap analysis using ASI
1. This query requires a subscription to the Amazon insights paid feature (audience_segments_amer_inmarket table)
2. Replace [1PD_Table] with your first-party table name.
3. This query requires user_id and event_subtype columns in the 1PD_Table.
*/

WITH conv AS (
  SELECT
  user_id, 
  event_subtype as conversion_name,
  SUM(1) as totalConversions
  --SUM(sale_price) as totalSales -- If included
  FROM 
    [1PD_Table]
    WHERE conversion_name IN ('', '')
    GROUP BY 1,2
),

dsp_events AS (
  SELECT
    user_id,
    SUM(impressions) as totalImpressions,
    0 as totalClicks,
    SUM(total_cost)/ 100000 as totalCost
  FROM
    dsp_impressions
  WHERE
    user_id IS NOT NULL
    -- AND campaign_id IN (
    --   SELECT
    --     campaign_id
    --   FROM
    --     dsp_campaigns
    -- ) -- Read DSP click events
    GROUP BY 1
    
  UNION ALL
  SELECT
    user_id,
    0 as totalImpressions,
    SUM(clicks) as totalClicks,
    0 as totalCost
  FROM
    dsp_clicks
  WHERE
    user_id IS NOT NULL
    -- AND campaign_id IN (
    --   SELECT
    --     campaign_id
    --   FROM
    --     dsp_campaigns
    -- ) -- Read DSP pixel conversion events.
     GROUP BY 1
),

dsp_events_agg AS (
SELECT 
user_id,
SUM(totalClicks) as totalClicks,
SUM(totalImpressions) as totalImpressions,
SUM(totalCost) as totalCost
FROM dsp_events
GROUP BY 1
),

segments AS (
SELECT 
DISTINCT
segment_name, 
user_id
FROM 
audience_segments_amer_inmarket 
----US MarketPlaceID
WHERE segment_marketplace_id = 1

UNION ALL 

SELECT DISTINCT
segment_name, 
user_id
FROM 
audience_segments_amer_lifestyle 
----US MarketPlaceID
WHERE segment_marketplace_id = 1
),

ad_exposed_users_per_segment AS (
SELECT
segment_name,
COUNT(DISTINCT s.USER_ID) as totalAdExposedUsersPerSegment
FROM 
segments s
INNER JOIN 
(select distinct user_id from dsp_events_agg) d
ON s.user_id = d.user_id
GROUP BY 1
),

conv_seg AS (
SELECT 
c.user_id,
c.conversion_name,
s.segment_name,
totalConversions
FROM conv c
inner join segments s
ON 
c.user_id = s.user_id
),

aggregate AS (
SELECT 
a.[conversion_name],
a.segment_name,
p.totalAdExposedUsersPerSegment,
COUNT(distinct a.user_id) as distinctUsers,
SUM(a.totalConversions) as totalConversions,
SUM(d.totalImpressions) as totalImpressions,
SUM(d.totalClicks) as totalClicks,
SUM(d.totalCost) as totalCost
FROM conv_seg a
LEFT JOIN dsp_events_agg d
on a.user_id = d.user_id
INNER JOIN ad_exposed_users_per_segment p
ON a.segment_name = p.segment_name
GROUP BY 1,2,3

)

SELECT
conversion_name,
segment_name,
totalAdExposedUsersPerSegment,
distinctUsers,
totalConversions,
totalImpressions,
totalClicks,
totalCost,
totalCost/totalClicks as costPerClick,
distinctUsers/totalAdExposedUsersPerSegment as conversionRate,
totalCost/totalConversions as costPerConversion
FROM aggregate
```

</details>


### Swim Lane #2: Measure and optimize existing campaigns

![Swim lane 1](/_images/amazon-marketing-cloud/playbooks/offamazon_conversions/NonEndemicPlaybook-External_swimlanes_2.png)

#### Use case: Campaign measurement

**Overview:**

Measure advertising performance using off-Amazon data to calculate KPIs. Off-Amazon conversions unlocks new KPI’s, depending on what data is available, like: conversion rate, ROAS, revenue, and more. 

**Reference IQs**

- [Measure Ad-Attributed Vehicle Purchases from Experian Auto (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries/3b52de99f4c16cc01a3fac9e0e81634bafae552ee7e9bf71dba45dc1b8fd28aa)
- [Measure ad-attributed off-Amazon conversions from first-party data (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries/26b0f00cc50a0aeec2971ab63ce48c702edd941763d8629bb3097222a642cd9f)

**Questions Answered:**

- How are my campaigns performing? 
- Which audiences are performing well?
- Which campaigns/line items/audiences are under-performing? 

**Suggested user counts:**

- Low if aggregating to campaign 
- Medium if aggregating to line-item

**Actions:**

- Budget allocation
- Campaign optimization
- Audience optimization

**Example:**

Notes: 

- You must embed “Conversion Name” into the SQL code as a “conversion” condition in the CTE “fpd\_conversions” (`AND [conversion_name] IN ('Purchase', 'Store Visit')`). If you add “Conversion Name” into the SELECT statement, this will introduce duplication. 
- The query is first/last touch with "Clicks" always getting attribution before impressions. If there are multiple clicks then it will select the first/last based on attribution of choice. This can be modified in the SQL code to be true first/last touch if needed.

| attribution_model | campaign                         | impressions | clicks | total_spend | unique_reach | unique\_purchasing\_users | number\_of\_conversions | conversion\_click\_event | conversion\_impression\_event | conversions_rate | spend\_per\_conversion |
|-------------------|----------------------------------|-------------|--------|-------------|--------------|-------------------------|-----------------------|------------------------|-----------------------------|------------------|----------------------|
| Last Touch        | Iris\_test\_campaign_804549        | 2049        | 587    | $5,980      | 1960         | 130                     | 796                   | 714                    | 82                          | 6.63%            | $7.51                |
| Last Touch        | Iris\_test\_campaign_919915        | 2163        | 653    | $6,920      | 2240         | 186                     | 2081                  | 1822                   | 259                         | 8.30%            | $3.33                |
| Last Touch        | Organique\_test\_campaign_292498   | 924         | 299    | $2,090      | 1650         | 146                     | 1394                  | 1202                   | 192                         | 8.85%            | $1.50                |
| Last Touch        | Jack & Jill\_test\_campaign_740213 | 1826        | 548    | $5,770      | 1890         | 132                     | 829                   | 758                    | 71                          | 6.98%            | $6.96                |


If revenue is uploaded:

| attribution_model | Revenue | ROAS |
|-------------------|---------|------|
| Last Touch        | $7,000  | $1   |
| Last Touch        | $80     | $0   |
| Last Touch        | $90,000 | $16  |
| Last Touch        | $1,500  | $2   |

**SQL**

<details class="details-bar">
  <summary>Click to see SQL code</summary>

```
/*
AMC Playbook: Off-Amazon conversions
Use case: Campaign measurement
1. Replace [1PD_Table] with your first-party table name.
2. This query requires user_id, conversion_date, and conversion_name columns in the 1PD_Table.
*/

-- Read DSP impression events
WITH dsp_events AS (
  SELECT
    user_id,
    campaign_id,
    campaign,
    impression_dt AS event_dt,
    'IMP' AS event_type,
    impressions,
    total_cost,
    0 AS clicks,
    0 AS conversions
  FROM
    dsp_impressions
  WHERE
    user_id IS NOT NULL
    AND campaign_id IN (
      SELECT
        campaign_id
      FROM
        dsp_campaigns
    ) -- Read DSP click events.
  UNION ALL
  SELECT
    user_id,
    campaign_id,
    campaign,
    click_dt AS event_dt,
    'CLICK' AS event_type,
    0 AS impressions,
    0 AS total_cost,
    clicks,
    0 AS conversions
  FROM
    dsp_clicks
  WHERE
    user_id IS NOT NULL
    AND campaign_id IN (
      SELECT
        campaign_id
      FROM
        dsp_campaigns
    ) -- Read DSP pixel conversion events.
),
-- Use 1P data conversions
fpd_conversions AS (
  SELECT
    user_id,
    CAST(conversion_date AS TIMESTAMP) AS event_dt,
    UUID() AS conversion_id,
    1 AS number_of_conversions
  FROM
    /* OPTIONAL UPDATE: Attribution Window: Include conversions from up to X days after impressions and clicks. The setting below is 60 days. */
  TABLE(
      EXTEND_TIME_WINDOW('[1PD_Table]', 'P0D', 'P60D')
    )
  WHERE
    user_id IS NOT NULL
    AND conversion_name IN ('Purchase', 'Store Visit')
),

matched_conversion_events AS (
  -- matched DSP conversions and traffic
  SELECT
    fpd.user_id,
    fpd.conversion_id,
    d.campaign_id,
    d.campaign,
    -- OPTIONAL UPDATE DSP Event Priority: Update the match_type values below to change priority for attribution
    -- The lowest value will have the highest priority
    CASE
      WHEN d.event_type = 'CLICK' THEN 1 -- Prioritize clicks over impressions
      WHEN d.event_type = 'IMP' THEN 2
      ELSE 3
    END AS match_type,
    SECONDS_BETWEEN(d.event_dt, fpd.event_dt) AS match_age,
    fpd.number_of_conversions,
    IF(d.event_type = 'CLICK', 1, 0) AS conversion_click_event,
    IF(d.event_type = 'IMP', 1, 0) AS conversion_impression_event
  FROM
    fpd_conversions fpd
    JOIN dsp_events d ON fpd.user_id = d.user_id

),
-- For each conversion event, rank all the matching traffic events based on match type and age, and filter out traffic outside of the lookback window.
ranked_conversion_event AS (
  SELECT
    user_id,
    campaign_id,
    campaign,
    ROW_NUMBER() OVER(
      PARTITION BY conversion_id
      ORDER BY
        match_type ASC,
        -- OPTIONAL UPDATE Attribution Model: change next line to 'match_age ASC' to use a last-touch attribution model
        match_age DESC
    ) AS match_rank,
    number_of_conversions,
    conversion_click_event,
    conversion_impression_event
  FROM
    matched_conversion_events
  WHERE
    /* OPTIONAL UPDATE Attribution Window: Filter out matches where the traffic event is after or more than 60 days before the conversion event. */
    match_age BETWEEN 0 AND 60 * 24 * 60 * 60
),
-- Filter to only the best matching traffic event for each conversion event.
-- Calculate conversion metric sums for each campaign.
attributed_conversion_event AS (
  SELECT
    campaign_id,
    campaign,
    COUNT(DISTINCT user_id) AS unique_purchasing_users,
    SUM(number_of_conversions) AS number_of_conversions,
    SUM(conversion_click_event) AS conversion_click_event,
    SUM(conversion_impression_event) AS conversion_impression_event
  FROM
    ranked_conversion_event
  WHERE
    -- Filter to only the best matching traffic event.
    match_rank = 1
  GROUP BY
    1,
    2
),
-- Include campaign impressions, clicks and user reach info.
dsp_traffic_and_conversions AS (
  SELECT
    campaign_id,
    campaign,
    COUNT(DISTINCT user_id) unique_reach,
    SUM(total_cost) / 100000 AS total_spend,
    SUM(impressions) AS impressions,
    SUM(clicks) AS clicks,
    SUM(conversions) AS pixel_conversions
  FROM
    dsp_events
  GROUP BY
    1,
    2
)
SELECT
  -- OPTIONAL UPDATE Attribution Model: Change below value to 'Last Touch' if using last-touch attribution model
  'First Touch' AS attribution_model,
  d.campaign_id,
  d.campaign,
  d.impressions,
  d.clicks,
  d.total_spend,
  d.unique_reach,
  v.unique_purchasing_users,
  v.number_of_conversions,
  v.conversion_click_event,
  v.conversion_impression_event,
  v.unique_purchasing_users / d.unique_reach AS conversions_rate,
  d.total_spend / v.number_of_conversions AS spend_per_conversion
FROM
  attributed_conversion_event v
  JOIN dsp_traffic_and_conversions d ON v.campaign_id = d.campaign_id
```

</details>

#### Use case: Geography
**Overview:**

For advertisers who might not know where users are coming from and would like to narrow down the scope of DSP marketing to specific market. For example, if a national brand has a newsletter sign-up, that advertiser might not know any information about where the users geographically. Combined with the [First-party data overview](#3-pre-swim-lanes-first-party-data-overview) can provide additional information on demographics and purchase behavior. 

**Questions Answered:**

- Where are my off-Amazon users? 
- Which geographical areas are most active and have the highest potential? 
- Which geographical areas are underserved? 

**Actions:**

- Localized marketing in Amazon DSP for both high potential or underserved areas. Location can be set at the line-item level at the city, state, country, DMA, or postal code level. 

**Suggested user counts:**

- Medium if aggregating to DMA
- Very high for `postal_code` 
- Low for State

> [NOTE] Redaction could be problematic depending on the quantity of resolved users. See the last row in the table below for example of what redaction will look like as it does not meet the 100 users requirement for 1P data. If redaction is an issue, remove the most granular geo level and move to a less granular level until redaction is at an acceptable level. Order of geo granularity from most to least is: postal_code -> dma_code -> iso\_state\_province\_code -> iso\_country\_code.

The query below selects the one location per `user_id` that is most common for that user.

**Example:**

| conversion_name | dma_code  | postal_code | totalconversions | distinctusers |
|-----------------|-----------|-------------|------------------|---------------|
| Purchase        | US-DMA627 | US-73501    | 109              | 109           |
| Purchase        | US-DMA602 | US-60106    | 250              | 223           |
| Newsletter      | US-DMA813 | US-97527    | 213              | 211           |
|                 |           |             | 50               | 50            |

**SQL**

<details class="details-bar">
  <summary>Click to see SQL code</summary>

```
/*
AMC Playbook: Off-Amazon conversions
Use case: Geography
1. Replace [1PD_Table] with your first-party data table name.
2. This query requires user_id and conversion_name columns in the 1PD_Table.
*/


WITH user_geo AS (
SELECT
user_id,
dma_code, 
postal_code, 
sum(impressions) as frequency
FROM
dsp_impressions 
GROUP BY 1,2,3
)

, ranking AS (
SELECT
user_id,
dma_code, 
postal_code, 
frequency,
ROW_NUMBER() OVER (PARTITION BY USER_ID ORDER BY frequency DESC) as geo_rank
FROM user_geo
)

, users_most_common_geo AS (
SELECT 
user_id,
dma_code, 
postal_code
FROM ranking
WHERE geo_rank = 1
),

unique_conversions AS (
SELECT 
user_id,
conversion_name,
COUNT(user_id) as totalConversions
FROM
[1PD_Table]
GROUP BY 1,2
)

SELECT 
[conversion_name], 
dma_code, 
postal_code,
SUM(totalConversions) as totalConversions,
COUNT(DISTINCT g.USER_ID) as distinctUsers
FROM 
users_most_common_geo g
INNER JOIN unique_conversions c
on g.user_id = c.user_id
GROUP BY 
1,2,3
```
</details>

#### Use case: Frequency analysis

**Overview:**

Identify the frequency of impressions before conversion using off-Amazon conversions as the metric. The output can be used to set frequency caps at either the line-item or campaign level. The result is to increase efficiency by increasing reach or increasing frequency with the same budget.

**Reference IQ**

- [DSP Impression Frequency and Conversions (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries/2b13b611c86b51a98ea3027822557df0b8a1a994272e2173a3b426214cf8f714)
 
**Questions Answered:**

- How many impressions are my customer getting?
- What is a reasonable frequency cap? 

**Suggested user counts:**

- High - when splitting out by frequency bucket user numbers can reduce quickly

**Actions:**

- Modify frequency cap to increase performance. Frequency caps can be set at the line-item or campaign. 

**Example:**

| advertiser_n | frequency_buckets | unique\_user\_count | conversion_count | conversion_rate |
|--------------|-------------------|-------------------|------------------|-----------------|
| Campaign_A   | 1                 | 4744              | 296              | 6.24%           |
| Campaign_A   | 2                 | 3043              | 360              | 11.83%          |
| Campaign_A   | 3                 | 2289              | 429              | 18.74%          |
| Campaign_A   | 4                 | 1874              | 325              | 17.34%          |
| Campaign_A   | 5                 | 1576              | 408              | 25.89%          |
| Campaign_A   | 6                 | 1341              | 431              | 32.14%          |
| Campaign_A   | 7                 | 1144              | 394              | 34.44%          |
| Campaign_A   | 8                 | 1018              | 306              | 30.06%          |
| Campaign_A   | 9                 | 902               | 219              | 24.28%          |
| Campaign_A   | 10                | 795               | 315              | 39.62%          |

**SQL**

<details class="details-bar">
  <summary>Click to see SQL code</summary>

```

/*
AMC Playbook: Off-Amazon conversions
Use case: Frequency analysis
1. Replace [1PD_Table] with your first-party data table name.
2. This query requires user_id, total_product_sales, and event_date_utc columns in the 1PD_Table.
*/

WITH campaign_id_string (campaign_id_string) AS (
VALUES
(1111111111111),
(2222222222222),
(3333333333333),
(4444444444444)
),

events AS (
  SELECT
    i.impression_date,
    i.user_id,
    SUM(i.impressions) as impressions,
    0 AS product_sales, 
    SUM(i.total_cost/100000) as total_cost     
  FROM
    dsp_impressions i 
  INNER JOIN campaign_id_string c ON c.campaign_id_string = i.campaign_id_string
  GROUP BY
    1,
    2
  UNION ALL

SELECT 
    s.event_date as impression_date,
    s.user_id, 
    SUM(s.impressions) as impressions,
    0 AS product_sales, 
    
    SUM(spend/100000000) as total_cost   
FROM sponsored_ads_traffic s
    INNER JOIN campaign_id_string c ON c.campaign_id_string = s.campaign_id_string
group by 1, 2

UNION ALL

SELECT 
    a.event_date_utc  as impression_date,
    b.user_id, 
    0 AS impressions, 
    SUM(b.total_product_sales) as product_sales,

    0 as total_cost  

FROM [1PD_Table] b
inner join sponsored_ads_traffic  a on a.user_id = b.user_id
INNER JOIN campaign_id_string c ON c.campaign_id_string = a.campaign_id_string

WHERE  SECONDS_BETWEEN(a.event_date, b.event_date_utc) < 60 * 60 * 24 *15
GROUP BY 1, 2

UNION ALL

SELECT 
    a.impression_date  as impression_date,
    b.user_id, 
    0 AS impressions, 
    SUM(b.total_product_sales) as product_sales,

    0 as total_cost  

FROM [1PD_Table] b
inner join dsp_impressions  a on a.user_id = b.user_id
INNER JOIN campaign_id_string c ON c.campaign_id_string = a.campaign_id_string

WHERE  SECONDS_BETWEEN(a.impression_date, b.event_date_utc) < 60 * 60 * 24 *15
GROUP BY 1, 2
    
),
events_by_user AS (
  SELECT
    impression_date,
    user_id,
    IF(
      SUM(impressions) > 0,
      SUM(impressions),
      0
    ) AS impressions,
    IF(
      SUM(impressions) > 0, 
      SUM(impressions), 
      0
    ) AS frequency,
    SUM(product_sales) AS product_sales,
    SUM(total_cost) as cost
  FROM
    events
  GROUP BY
    1,
    2
),
frequency_by_user AS (
  SELECT
    impression_date,
    impressions,
    frequency AS frequency_bucket,
    product_sales,
    cost,
    user_id
  FROM
    events_by_user
  WHERE
    user_id IS NOT NULL
)
, freq_bucket as (
SELECT
  impression_date,
  frequency_bucket,
  SUM(product_sales) AS product_sales,
  SUM(cost) AS cost
FROM
  frequency_by_user
GROUP BY
  1,
  2
  ),
freq_bucket_avg as (
SELECT
  frequency_bucket,
  median(product_sales) AS product_sales,
  median(cost) AS cost
FROM
  freq_bucket
WHERE frequency_bucket <= 25 and frequency_bucket > 0
--Where frequency_bucket <= 25
GROUP BY
  1
  ),
cumulative_total as (
select
    c1.frequency_bucket,
    c1.product_sales,
    sum(c2.product_sales) over (partition by c1.frequency_bucket) as Cumulativeproduct_sales,
    c1.cost,
    sum(c2.cost) over (partition by c1.frequency_bucket) as CumulativeCost
From freq_bucket_avg c1
left join freq_bucket_avg c2
on c1.frequency_bucket >= c2.frequency_bucket 
    ), 
percent_change as (
select
    frequency_bucket,
    cost,
    CumulativeCost,
    CumulativeCost / max(CumulativeCost) over () as cumulative_cost_perc,
    product_sales,
    Cumulativeproduct_sales,
    Cumulativeproduct_sales / max(Cumulativeproduct_sales) over () as cumulative_product_sales_perc,
    (Cumulativeproduct_sales / max(Cumulativeproduct_sales) over ()) - (CumulativeCost / max(CumulativeCost) over ()) as percent_sales_diff
from cumulative_total
)
select 
    a.frequency_bucket,
    avg(a.cost) as cost,
    avg(a.CumulativeCost) as CumulativeCost,
    avg(a.cumulative_cost_perc) as cumulative_cost_perc,
    avg(a.product_sales) as product_sales,
    avg(a.Cumulativeproduct_sales) as Cumulativeproduct_sales,
    avg(a.cumulative_product_sales_perc) as cumulative_product_sales_perc,
    b.percent_sales_diff - a.percent_sales_diff as sales_percent_change
from percent_change a
left join percent_change b
on a.frequency_bucket = b.frequency_bucket - 1 
group by 1, 8

```

</details>

![Frequency buckets chart](/_images/amazon-marketing-cloud/playbooks/offamazon_conversions/frequency_buckets.png)

Here, you can gather that the maximum frequency cap for this campaign would be at around 11 where the overall line passes zero. 

#### Use case: Device analysis

**Overview:**

Measure which devices are most effective with each campaign. Different campaigns with different goals might have more success on certain devices. For example, new-to-brand campaigns might be more successful on PC vs mobile. 

**Reference IQ**

- [Measure Ad-Attributed Vehicle Purchases from Experian Auto (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries/3b52de99f4c16cc01a3fac9e0e81634bafae552ee7e9bf71dba45dc1b8fd28aa)

**Questions Answered:**

- Which devices are most effective with different campaign goals?

**Suggested user counts:**

- Low if aggregating to campaign 
- Medium if aggregating to line-item

**Actions:**

- Modify device types in the Amazon DSP for line items. Device can be set to be: “Desktop and mobile”, “Desktop”, “Mobile”, “Connected TV” depending on the line item type. 

Notes: 

- This SQL code is very similar to [Campaign measurement](#use-case-campaign-measurement), with device propagated through to the end SELECT. This will increase redaction as there will be more records without sufficient distinct users.
- Embed “Conversion Name” into the SQL code as a “conversion” condition. See SQL code below in CTE “fpd_conversions” for reference. Adding “Conversion Name” into the select statement will introduce duplication. 
- The query is first/last touch with "Clicks" always getting attribution before impressions. If there are multiple clicks then it will select the first/last based on attribution of choice. This can be modified in the SQL code to be true first/last touch if needed.

**Example:**

| attribution\_model | campaign\_id | campaign                                | device\_type | impressions | clicks | total\_spend | unique\_reach | unique\_purchasing\_users | number\_of\_conversions | conversion\_click\_event | conversion\_impression\_event | conversions_rate | spend\_per\_conversion |
|-------------------|-------------|-----------------------------------------|-------------|-------------|--------|-------------|--------------|-------------------------|-----------------------|------------------------|-----------------------------|------------------|----------------------|
| Last Touch        | 11111       | Jack & Jill\_test\_campaign\_4832407182165 | PC          | 19664       | 4570   | 214.68464   | 3555         | 540                     | 1776                  | 1727                   | 49                          | 0.1519           | 0.12088              |
| Last Touch        | 1111        | Jack & Jill\_test\_campaign\_4832407182165 | TV          | 19712       | 4708   | 213.66262   | 3559         | 540                     | 1776                  | 1727                   | 49                          | 0.15173          | 0.12031              |


A sample output visual from this analysis is:

![Example visual](/_images/amazon-marketing-cloud/playbooks/offamazon_conversions/playbook_off_amazon_conversions_visual.png)

In the visual above, it’s clear that the campaign “Jack & Jill test campaign” has a strong device type competent. A possible action would be to modify the DSP campaign to focus on Phone and not include PC. 

**SQL**

<details class="details-bar">
  <summary>Click to see SQL code</summary>

```
/*
AMC Playbook: Off-Amazon conversions
Use case: Device analysis
1. Replace [1PD_Table] with your first-party data table name.
*/

WITH
dsp_campaigns (campaign_id) AS (
  VALUES
    (1111111111),
    (2222222222),
    (3333333333),
    (4444444444)
),

-- Read DSP impression events.
dsp_events AS (
  SELECT
    user_id,
    campaign_id,
    campaign,
    device_type, 
    impression_dt AS event_dt,
    'IMP' AS event_type,
    impressions,
    total_cost,
    0 AS clicks,
    0 AS conversions
  FROM
    dsp_impressions
  WHERE
    user_id IS NOT NULL
    AND campaign_id IN (
      SELECT
        campaign_id
      FROM
        dsp_campaigns
    ) -- Read DSP click events.
  UNION ALL
  SELECT
    user_id,
    campaign_id,
    campaign,
    device_type, 
    click_dt AS event_dt,
    'CLICK' AS event_type,
    0 AS impressions,
    0 AS total_cost,
    clicks,
    0 AS conversions
  FROM
    dsp_clicks
  WHERE
    user_id IS NOT NULL
    AND campaign_id IN (
      SELECT
        campaign_id
      FROM
        dsp_campaigns
    ) -- Read DSP pixel conversion events.
),
-- Use 1P data conversions
fpd_conversions AS (
  SELECT
    user_id,
    CAST(event_dt_utc AS TIMESTAMP) AS event_dt,
    UUID() AS conversion_id,
    1 AS number_of_conversions
  FROM
    /* OPTIONAL UPDATE Attribution Window: Include conversions from up to X days after impressions and clicks. The setting below is 7 days. */
  TABLE(
      EXTEND_TIME_WINDOW('[1PD_Table]', 'P0D', 'P7D')
    )
  WHERE
    user_id IS NOT NULL
    AND event_subtype IN ('newsletter', 'purchase')
),

matched_conversion_events AS (
  -- matched DSP conversions and traffic
  SELECT
    fpd.user_id,
    fpd.conversion_id,
    d.campaign_id,
    d.campaign,
    d.device_type, 
    -- OPTIONAL UPDATE DSP Event Priority: Update the match_type values below to change priority for attribution
    -- The lowest value will have the highest priority
    CASE
      WHEN d.event_type = 'CLICK' THEN 1 -- Prioritize clicks over impressions
      WHEN d.event_type = 'IMP' THEN 2
      ELSE 3
    END AS match_type,
    SECONDS_BETWEEN(d.event_dt, fpd.event_dt) AS match_age,
    fpd.number_of_conversions,
    IF(d.event_type = 'CLICK', 1, 0) AS conversion_click_event,
    IF(d.event_type = 'IMP', 1, 0) AS conversion_impression_event
  FROM
    fpd_conversions fpd
    JOIN dsp_events d ON fpd.user_id = d.user_id

),
-- For each conversion event, rank all the matching traffic events based on match type and age, and filter out traffic outside of the lookback window.
ranked_conversion_event AS (
  SELECT
    user_id,
    campaign_id,
    campaign,
    device_type,
    ROW_NUMBER() OVER(
      PARTITION BY conversion_id
      ORDER BY
        match_type ASC,
        -- OPTIONAL UPDATE Attribution Model: change next line to 'match_age ASC' to use a last-touch attribution model
        match_age DESC
    ) AS match_rank,
    number_of_conversions,
    conversion_click_event,
    conversion_impression_event
  FROM
    matched_conversion_events
  WHERE
    /* OPTIONAL UPDATE Attribution Window: Filter out matches where the traffic event is after or more than 60 days before the conversion event. */
    match_age BETWEEN 0 AND 60 * 24 * 60 * 60
),
-- Filter to only the best matching traffic event for each conversion event.
-- Calculate conversion metric sums for each campaign.
attributed_conversion_event AS (
  SELECT
    campaign_id,
    campaign,
    device_type,
    COUNT(DISTINCT user_id) AS unique_purchasing_users,
    SUM(number_of_conversions) AS number_of_conversions,
    SUM(conversion_click_event) AS conversion_click_event,
    SUM(conversion_impression_event) AS conversion_impression_event
  FROM
    ranked_conversion_event
  WHERE
    -- Filter to only the best matching traffic event.
    match_rank = 1
  GROUP BY
    1,
    2,
    3
),
-- Include campaign impressions, clicks and user reach info.
dsp_traffic_and_conversions AS (
  SELECT
    campaign_id,
    campaign,
    device_type,
    COUNT(DISTINCT user_id) unique_reach,
    SUM(total_cost) / 100000 AS total_spend,
    SUM(impressions) AS impressions,
    SUM(clicks) AS clicks,
    SUM(conversions) AS pixel_conversions
  FROM
    dsp_events
  GROUP BY
    1,
    2,3
)
SELECT
  -- OPTIONAL UPDATE Attribution Model: Change below value to 'Last Touch' if using last-touch attribution model
  'First Touch' AS attribution_model,
  d.campaign_id,
  d.campaign,
  d.device_type,
  d.impressions,
  d.clicks,
  d.total_spend,
  d.unique_reach,
  v.unique_purchasing_users,
  v.number_of_conversions,
  v.conversion_click_event,
  v.conversion_impression_event,
  v.unique_purchasing_users / d.unique_reach AS conversions_rate,
  d.total_spend / v.number_of_conversions AS spend_per_conversion
FROM
  attributed_conversion_event v
  JOIN dsp_traffic_and_conversions d 
  ON v.campaign_id = d.campaign_id
  and v.device_type = d.device_type
```

</details>


### Swim Lane #3: Create audiences

![Swim lane 1](/_images/amazon-marketing-cloud/playbooks/offamazon_conversions/NonEndemicPlaybook-External_swimlanes_3.png)

#### Use case: Customer long-term value (CLTV)

**Overview:**

Assess customer value over the course of a brand relationship. Allow for segmenting of users into high value, high potential and low value. CLTV allows for more targeted advertising strategies that focus on customer retention and loyalty, which can lead to a more profitable business model over time. The custom audiences that can be created from this use case allow for higher performing campaigns through marketing to valuable audiences and excluding low value audiences.

**Reference Playbook**

- [Customer long-term value](guides/amazon-marketing-cloud/playbooks/CLTV)

**Questions Answered:**

- Who are my loyal and high value customers? How can I market to those users?
- Who are my under-served and do they have potential? How can I market to those users? 
- How can I exclude low long-term value customers?

**Actions:**

- Create rule-based audiences of high-value and high potential users
- Create rule-based audiences of low value users to exclude
- Lookalike audiences based on high value customers
- Activate or exclude custom audiences in Amazon DSP

Notes:

- If off-Amazon conversions do not contain spend values. Each off-Amazon conversions type could be hardcoded with a value in order to make the analysis proxy long-term value. For example, store visits could be assigned a value of $10 if it’s determined that a store visit equates to $10 in value for the brand. The result would be the output metrics should be disregarded are they are proxies, but the resulting segmentation of users into categories can still be used.
- The audiences created can be used with the Persona API in Swim Lane #1 to get insights into high value or low value user segments. 

**Example:**

| customer\_audience\_segment         | num\_users | total\_sales | total\_spend | cltv   |
|-----------------------------------|-----------|-------------|-------------|--------|
| Higher Revenue Potential Customer | 14,514    | $18.57      | $5.41       | $13.16 |
| High Growth Potential Customer    | 14,180    | $9.53       | $5.64       | $3.89  |
| Low Value Customer                | 607,585   | $9.30       | $4.11       | $5.18  |
| High Value Customer               | 3,133,429 | $14.21      | $2.76       | $11.45 |
 
#### Use case: Events Manager specific

**Overview:**

All Events Manager events that can resolve to Amazon `user_id`s are available in AMC. Both ad-exposed and non-ad-exposed users are available, which opens the door for specific analysis on non-ad-exposed users. Users who had a non-ad-exposed conversion event are likely candidates for marketing, as they are interested in the brand without Amazon DSP advertising.

**Reference IQ**

- [Introduction to Events Manager IQ (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries/f28930f6d139cc028439497753c1eb1e6cbef52e49a108df56072c1f6b36d404)

**Questions Answered:**

- How many Events Manager events are ad driven?
- What can I do with my non-ad-exposed users? 
- Are non-ad-exposed users with off-Amazon conversions high potential?

**Actions:**

- Create a rule-based audience on users who off-Amazon added to cart but did not off-Amazon convert.
- Create a rule-based audience of users who had a conversion event but are not ad-exposed. A potentially under served user group.
- Create a rule-based audience of users who non-ad-exposed bought A to be re-marketed B

**Example:**

| conversion\_event\_source\_name | conversion\_name     | event\_type\_description                                                | ad\_exposure    | off\_amazon\_conversion\_value | off\_amazon\_product\_sales | number\_of\_events | distinct_users |
|------------------------------|---------------------|-----------------------------------------------------------------------|----------------|-----------------------------|--------------------------|------------------|----------------|
| Amazon Ad Tag                | AddToCart           | Add to Shopping Cart                                                  | ad-exposed     | 10,000                      |                          | 400              | 150            |
| Amazon Ad Tag                | AddToCart           | Add to Shopping Cart                                                  | non-ad-exposed | 45,000                      |                          | 1,500            | 500            |
| Amazon Ad Tag                | Off-AmazonPurchases | Product purchased                                                     | ad-exposed     |                             | 3,000                    | 100              | 60             |
| Amazon Ad Tag                | Off-AmazonPurchases | Product purchased                                                     | non-ad-exposed |                             | 9,000                    | 300              | 200            |
| Amazon Ad Tag                | Off-AmazonPurchases | Kitchen\_Conversion\_tracking\_Off-Amazon Purchases\_SpoonsPurchases_2023 | ad-exposed     |                             | 1,000                    | 30               | 30             |
| Amazon Ad Tag                | Off-AmazonPurchases | Kitchen\_Conversion\_tracking\_Off-Amazon Purchases\_SpoonsPurchases\_2023 | non-ad-exposed |                             | 12,000                   | 250              | 195            |

Which can be pivoted outside of AMC:

| -                            | -                   | -                      | Sum of off\_amazon\_conversion\_value | Sum of off\_amazon\_conversion\_value | Sum of off\_amazon\_conversion\_value | Sum of off\_amazon\_product\_sales | Sum of off\_amazon\_product\_sales | Sum of off\_amazon\_product\_sales | Sum of number\_of\_events | Sum of number\_of\_events | Sum of number\_of\_events | Sum of distinct\_users | Sum of distinct\_users | Sum of distinct\_users |
|------------------------------|---------------------|------------------------|------------------------------------|------------------------------------|------------------------------------|---------------------------------|---------------------------------|---------------------------------|-------------------------|-------------------------|-------------------------|-----------------------|-----------------------|-----------------------|
| conversion\_event\_source\_name | conversion\_name     | event\_type\_description | ad-exposed                         | non-ad-exposed                     | % Ad Exposed                       | ad-exposed                      | non-ad-exposed                  | % Ad Exposed                    | ad-exposed              | non-ad-exposed          | ad-exposed              | ad-exposed            | non-ad-exposed        | ad-exposed            |
| Amazon Ad Tag                | AddToCart           | Add to Shopping Cart   | 10,000                             | 45,000                             | 18%                                | -                               | -                               | -                               | 400                     | 1500                    | 21%                     | 150                   | 500                   | 30%                   |
| Amazon Ad Tag                | Off-AmazonPurchases | Product purchased      | -                                  | -                                  | -                                  | 3,000                           | 9,000                           | 25%                             | 100                     | 300                     | 25%                     | 80                    | 200                   | 40%                   |


**SQL**

<details class="details-bar">
  <summary>Click to see SQL code</summary>

```
/*
AMC Playbook: Off-Amazon conversions
Use case: Events Manager specific
*/

WITH EVENTMANAGER AS (
SELECT 
USER_ID,
conversion_event_source_name,
conversion_event_name,
event_type_description,
SUM(off_amazon_conversion_value) as off_amazon_conversion_value,
SUM(off_amazon_product_sales) AS off_amazon_product_sales,
COUNT(USER_ID) AS number_of_events
FROM conversions
WHERE event_category = 'off-Amazon'
GROUP BY
1,2,3,4
)

, AD_EXPOSED AS (
SELECT 
USER_ID,
'DSP' as SOURCE
FROM
----Look 14 days before time window. 14 day "ad-exposed" window
 TABLE(
      EXTEND_TIME_WINDOW('dsp_impressions', 'P14D', 'P0D')
    )
GROUP BY 1,2

UNION ALL

SELECT 
USER_ID,
'SA' as SOURCE
FROM
----Look 14 days before time window. 14 day "ad-exposed" window
 TABLE(
      EXTEND_TIME_WINDOW('sponsored_ads_traffic', 'P14D', 'P0D')
    )
GROUP BY 1,2
),

AD_EXPOSED_AGG AS (
SELECT
USER_ID,
COLLECT(DISTINCT SOURCE) as SOURCE
FROM AD_EXPOSED
GROUP BY 1
)

, join_ad_exposure AS (
SELECT 
e.USER_ID,
e.conversion_event_source_name,
e.conversion_event_name,
e.off_amazon_conversion_value,
e.off_amazon_product_sales,
e.number_of_events,
e.event_type_description,
CASE WHEN A.user_id IS NULL then 'non-ad-exposed' else 'ad-exposed' END AS ad_exposure
FROM EVENTMANAGER E
LEFT JOIN AD_EXPOSED_AGG A
ON e.user_id = a.user_id
)

SELECT
conversion_event_source_name,
conversion_event_name as [conversion_name],
event_type_description,
ad_exposure,
SUM(off_amazon_conversion_value) as off_amazon_conversion_value,
SUM(off_amazon_product_sales) AS off_amazon_product_sales,
SUM(number_of_events) AS number_of_events,
COUNT(DISTINCT USER_ID) as distinct_users
FROM join_ad_exposure
GROUP BY 1,2,3,4
```

</details>

> [NOTE] Most non-endemic advertisers will probably not use sponsored ads. In the case there is Sponsored Display (SD) data, the SQL code is included. The query can also be split out by ad type (DS, Amazon DSP) for a more detailed breakdown. 

#### Use case: Audience creation

**Overview:**

Create valuable custom DSP audiences using both rule-based audiences (RBA) or lookalike (LAL) audiences using a combination of off-Amazon data and Amazon advertising signals. The combination of data sets allows for specific high value audiences to be created. The [Programmatic audience playbook](guides/amazon-marketing-cloud/playbooks/programmatic_audience_framework) can be used to provide a framework for creating and monitoring these custom audiences. 

**Reference Documentation**
 
- [Amazon Ads Advanced Tools Center](https://advertising.amazon.com/API/docs/en-us)

**Questions Answered:**

- How can I activate my data?
- How can I activate 1P data with AMC data? 
- Can I remarket to user who had one conversion and not another?
- What use cases are there to use with my existing data?

**Actions:**

- Create a rule-based audience
- Create a lookalike 

**Integrations:**

- Lookalike and rule-based audience API integration

Use Cases:

- CLTV high value audiences
- CLTV low value audiences - for exclusion
- Rule-based audience and lookalike audience based off of off-Amazon conversions `user_id` lists
  - Both inclusion and excluding existing customers
- Users who clicked DSP ad/added to cart/viewed detail page but never converted using off-Amazon conversions
- Events Manager only:
  - Events Manager conversion that were not ad-driven
- User who converted X amount of time ago and are in time frame to re-convert
- User who converted on X remarketed for Y
- Users who purchased at the last seasonal event. Ex: Christmas, Prime Day, Halloween.

### Swim Lane #4: Analyze segments

![Swim lane 1](/_images/amazon-marketing-cloud/playbooks/offamazon_conversions/NonEndemicPlaybook-External_swimlanes_4.png)

#### Use case: Campaign category reporting
**Overview:**

This use case is designed to be used along with 1P data or by hybrid advertisers who sell both on and off Amazon. The output of this use case is campaigns aggregated up to their goals or campaign types. For example, Black Friday campaigns, evergreen campaigns, Prime Day, etc. By joining in campaign metadata, analytics can be run at an aggregate level. 

> [NOTE] Campaign group reporting will not require any type of `user_id` identity resolution, as it will join on `campaign_id`, not `user_id`. [First-party data overview](#3-pre-swim-lanes-first-party-data-overview) is not relevant for dimensional use cases not using identity data. 

There is an additional hybrid advertiser use case in the appendix. 

**Questions Answered:**

- How are my different campaign types performing? 

**Actions:**

- Adjust budget to campaign types that are performing strongly.
- Adjust spend of campaigns to reduce overlap.
- Create more campaigns in a high performing category. 

**Example:**

| campaign_type | impressions | total_cost | clicks  | costperclick | clickthroughrate | distinctusers | subCampaigns |
|---------------|-------------|------------|---------|--------------|------------------|---------------|--------------|
| evergreen     | 313,112     | $3,393     | 114,519 | $0.03        | 0.36574          | 6,825         | 3            |
| PrimeDay      | 381,503     | $4,202     | 99,228  | $0.04        | 0.2601           | 6,839         | 6            |


**SQL**

> [NOTE]  This same mapping can be combined with queries from [Swim Lane #2](#swim-lane-2-measure-and-optimize-existing-campaigns) to measure campaign KPIs at an aggregate level. Additional campaign meta data can be propagated through to query for measurement.

<details class="details-bar">
  <summary>Click to see SQL code</summary>

```

/*
AMC Playbook: Off-Amazon conversions
Use case: Campaign category reporting
1. Replace [1PD_Table] with your first-party data table name.
*/

WITH off_amazon_campaign as (
SELECT 
campaign_id,
campaign_type
FROM 
[1PD_Table]
GROUP BY 1,2

),

-- Read DSP impression events.
dsp_events AS (
  SELECT
    campaign_id,
    campaign,
    user_id,
    'IMP' AS event_type,
    SUM(impressions) as impressions,
    sum(total_cost) as total_cost,
    0 AS clicks
    FROM
    dsp_impressions
  WHERE
    user_id IS NOT NULL
    -- AND campaign_id IN (
    --   SELECT
    --     campaign_id
    --   FROM
    --     dsp_campaigns
    -- ) -- Read DSP click events.
    GROUP BY 1,2,3
  UNION ALL
  SELECT
    campaign_id,
    campaign,
    user_id,
    'CLICK' AS event_type,
    0 AS impressions,
    0 AS total_cost,
    sum(clicks) as clicks
    FROM
    dsp_clicks
  WHERE
    user_id IS NOT NULL
    -- AND campaign_id IN (
    --   SELECT
    --     campaign_id
    --   FROM
    --     dsp_campaigns
    -- ) -- Read DSP pixel conversion events.
    GROUP BY 1,2,3
),

dsp_events_agg AS (
SELECT 
campaign_id,
campaign,
user_id,
SUM(impressions) as impressions,
SUM(total_cost)/100000 as total_cost,
SUM( clicks) as clicks
FROM DSP_EVENTS d
group by 1,2,3
),

calculate AS (
SELECT 
c.campaign_type,
SUM(d.impressions) as impressions,
SUM(d.total_cost) as total_cost,
SUM(d.clicks) as clicks,
COUNT(DISTINCT d.USER_ID) as distinctUsers,
COUNT(DISTINCT campaign_id) as subCampaigns
FROM 
dsp_events_agg d
INNER JOIN 
off_amazon_campaign c
on d.campaign_id = c.campaign_id
GROUP BY 
1
)

SELECT 
campaign_type,
impressions,
total_cost,
clicks,
total_cost/clicks as costPerClick,
clicks/impressions as clickThroughRate,
distinctUsers,
subCampaigns
FROM calculate
```
</details>

#### Use case: Off-Amazon user segments
**Overview:**

If users are segmented on off Amazon platforms such as a CDP or any other data warehouse solution. Those segments can be loaded into AMC, there two methods to load CDP data:

- AMC direct upload covered in section #1. 
- CDP direct integration with Amazon Ads. Check with your CDP provider to see if an AMC integration is supported.

**Questions Answered:**

- How do my off-Amazon user segments perform?
- How can I use my off-Amazon user segments?
- How do my off-Amazon segments perform with DSP spend?

**Actions:**

- Create audiences based on off-Amazon segments
- Exclude users from off-Amazon segments
- Assuming 1P sales data is available - create an audience around users who are in off-Amazon segment A but did not purchase ASIN 1. Create an audience to remarket those users for ASIN 1. 

**Example:**

| campaign_id | campaign                                | external\_audience\_segment\_name | impressions | total_cost | clicks | costperclick | clickthroughrate | distinctUsers |
|-------------|-----------------------------------------|--------------------------------|-------------|------------|--------|--------------|------------------|---------------|
| 1111        | Jack & Jill\_test\_campaign\_4832407182165 | Segment1                       | 38,535      | $419.86    | 9,044  | $0.05        | 0.23             | 488           |
| 2222        | Iris\_test\_campaign\_8784040388124        | Segment1                       | 37,695      | $410.57    | 8,681  | $0.05        | 0.23             | 481           |
| 1111        | Jack & Jill\_test\_campaign\_4832407182165 | Segment2                       | 22,489      | $244.49    | 5,346  | $0.05        | 0.24             | 357           |
| 2222        | Iris\_test\_campaign\_8784040388124        | Segment2                       | 19,655      | $214.24    | 4,553  | $0.05        | 0.23             | 377           |

**SQL**

> [NOTE]  This SQL code is a many-to-many join. It will be duplicated by `external_audience_segment_name` and the number of campaigns. The results will still provide an overview of DSP spend and activity by off-Amazon segment. But any type of aggregation will be duplicated. 

This will only reflect the cost/impressions/clicks of matched users. It will most likely reflect a small subset of the total cost and impressions.

<details class="details-bar">
  <summary>Click to see SQL code</summary>

```
/*
AMC Playbook: Off-Amazon conversions
Use case: Off-Amazon user segments
1. Replace [off_amazon_segment_table] with the Off-Amazon segment table
*/

WITH off_amazon_segments as (
SELECT 
[external_audience_segment_name],
USER_ID 
FROM [off_amazon_segment_table]
GROUP BY 1,2

),

-- Read DSP impression events.
dsp_events AS (
  SELECT
    user_id,
    campaign_id,
    campaign,
    'IMP' AS event_type,
    impressions,
    total_cost,
    0 AS clicks
    FROM
    dsp_impressions
  WHERE
    user_id IS NOT NULL
    -- AND campaign_id IN (
    --   SELECT
    --     campaign_id
    --   FROM
    --     dsp_campaigns
    -- ) -- Read DSP click events.
  UNION ALL
  SELECT
    user_id,
    campaign_id,
    campaign,
    'CLICK' AS event_type,
    0 AS impressions,
    0 AS total_cost,
    clicks
    FROM
    dsp_clicks
  WHERE
    user_id IS NOT NULL
    -- AND campaign_id IN (
    --   SELECT
    --     campaign_id
    --   FROM
    --     dsp_campaigns
    -- ) -- Read DSP pixel conversion events.
),

dsp_events_agg AS (
SELECT 
user_id,
campaign_id,
campaign,
SUM(impressions) as impressions,
SUM(total_cost)/100000 as total_cost,
SUM( clicks) as clicks
FROM DSP_EVENTS
group by 1,2,3
),

calculate AS (
SELECT 
d.campaign_id,
d.campaign,
c.[external_audience_segment_name],
SUM(d.impressions) as impressions,
SUM(d.total_cost) as total_cost,
SUM(d.clicks) as clicks,
COUNT(DISTINCT d.USER_ID) as distinctUsers
FROM 
dsp_events_agg d
INNER JOIN 
off_amazon_segments c
on d.user_id = c.user_id
GROUP BY 
1,2,3
)

SELECT 
campaign_id,
campaign,
[external_audience_segment_name],
impressions,
total_cost,
clicks,
total_cost/clicks as costPerClick,
clicks/impressions as clickThroughRate,
distinctUsers
FROM calculate
```
</details>

## Appendix

### Hybrid use case: ASIN category reporting

This use case is designed for hybrid sellers who might sell both on and off of Amazon. This use case will allow advertisers to join their internal product metadata with the ASINs in AMC to be able to measure performance among specific product categories or verticals that may exist within an AMC instance. Examples include: ASIN subcategory, ASIN category, ASIN profit margin, etc. This data opens up reporting by the aggregate levels. 

> [NOTE] ASIN grouping reporting will not require any type of `user_id` identity resolution. Instead, it will focus on resolving the ASIN id with each product to a non-endemic dataset. [First party-data overview](#3-pre-swim-lanes-first-party-data-overview) is not relevant for dimensional use cases not using identity data. 

**Questions Answered:**

- How are my product lines performing? 
- Considering a fixed cost per product, how many sales are needed to turn a profit on them?
- How can I compare advertising cost to my product margin?

**Actions:**

- Plan campaigns around the unique customer journeys associated with product categories
- Identify which product categories to invest in based on their ROAS