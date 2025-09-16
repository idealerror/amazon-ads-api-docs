---
title: Overview - Amazon Marketing Stream
description: A guide to help you get started with Amazon Marketing Stream, including key features, datasets, and General FAQs
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
    - stream FAQs 
---

# Amazon Marketing Stream

This article provides a high-level overview of Amazon Marketing Stream, which is available for integrated partners and other advertisers. We’ll cover key features, limitations, FAQs, common use cases, and a sample query. When you're ready to get started with Amazon Marketing Stream, check out the [Onboarding guide](guides/amazon-marketing-stream/onboarding).

Jump to [Key features](#key-features) | [Destinations](#supported-destinations) | [Datasets](#amazon-marketing-stream-datasets)  | [Limitations](#limitations) | [Geographic availability](#geographic-availability) | [General FAQ](#general-faq) | [Sample query](#sample-query) | [AWS Workshop](#aws-workshop) |

## Overview
Amazon Marketing Stream is a data-streaming service that partners or advertisers can use to subscribe to advertising data sets. Amazon Marketing Stream delivers the data to partner or advertiser-owned AWS destinations in near real-time, without needing to call the API repeatedly throughout the day. With Amazon Marketing Stream, partners and advertisers can use AWS services natively to receive and process advertising data. Amazon Marketing Stream can help improve data integration patterns and operational efficiency by reducing the number of API calls that are typically required for intra-day campaign optimization. 

## Key features 
Amazon Marketing Stream offers new capabilities compared with an API-based reporting approach:

* **Improved data granularity**: Traffic and conversion data is summarized hourly, compared with a daily granularity if you’re using the reporting API. The data is streamed automatically to your AWS account, which you’ll provide when you subscribe to the Amazon Marketing Stream data set. 
* **No throttling**: With Amazon Marketing Stream data streaming, you no longer need to make frequent API calls for intra-day updates. This helps to prevent throttling while also increasing data accuracy.
* **Near real-time messaging**: Get notified about budget consumption, such as when campaigns are running out of budget. 
* **More reporting dimensions**: While API-based reports can show performance at the placement or the ad level, Amazon Marketing Stream can show all dimensions available in a single dataset. While API-based reports can show performance at the target ID, placement or the ad level, Amazon Marketing Stream can show data for these dimensions all together. This allows you to see granular data such as keyword performance for a given placement that isn’t available without Amazon Marketing Stream. This gives you more insights to help you improve campaign efficiency. 
* **Insights on deltas instead of aggregates**: Amazon Marketing Stream allows you to better understand how campaigns are performing by pushing the delta values instead of aggregates. This helps you see how campaign performance is changing over time. The [sample query below](#sample-query) demonstrates what this might look like using Amazon Marketing Stream. 
* **Simplified workflow**: With Amazon Marketing Stream, you can access all of your advertising data from your AWS account natively. Additionally, Amazon Marketing Stream pushes changes to your data instead of snapshots. This simplifies the extract, transform, and load (ETL) process by showing you exactly what has changed when new data arrives.  Without Amazon Marketing Stream, you need to pull a snapshot report, and then compare it with your native data store to know what changed—a process that is less efficient.

## Supported destinations

Amazon Marketing Stream currently supports two destinations: Amazon Data Firehose and AWS Simple Queue Service (SQS).

### Amazon Data Firehose

Firehose supports a number of data outputs (including S3, Amazon Redshift, Snowflake, Splunk, OpenSearch Service) for increased data storage flexibility. 

- [Firehose overview](guides/amazon-marketing-stream/onboarding/firehose/overview)
- [Getting started guide: Firehose and Amazon Marketing Stream](guides/amazon-marketing-stream/onboarding/firehose/get-started)
- [AWS documentation](https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html)

### SQS

- [Getting started guide: SQS and Amazon Marketing Stream](guides/amazon-marketing-stream/onboarding/sqs/get-started)
- [AWS documentation](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html)
- [Benefits of SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-benefits.html)

## Amazon Marketing Stream datasets
Currently, Amazon Marketing Stream includes two categories of data: reporting and messaging. For a full description of all data available through Amazon Marketing Stream, see the [Amazon Marketing Stream data guide](guides/amazon-marketing-stream/data-guide).

### Reporting data
Amazon Marketing Stream reporting includes an advertiser’s processed (non-raw, summarized) performance data, including traffic and conversions sent hourly, along with data dimensions necessary for analytics. 

### Messaging data
Amazon Marketing Stream messaging sends notifications on entity state changes and other events in near real time. Currently, this messaging includes campaign, ad group, ad, and targeting entity changes, as well as budget consumption updates, which notify you whenever your budget allotment changes by 5% or more. In the future, we plan to expand the messaging to include notifications about product eligibility, bid recommendations, and other events. 

### Recommendations 
Amazon Marketing Stream recommendations include suggestions for optimizing existing ad campaigns or creating new ad campaigns to improve the performance of your sponsored ads portfolio.

## Dataset changes

Amazon Marketing Stream datasets may expand over time to add new fields or supported values. You should plan for potential additions to dataset schema as part of your integration. Any changes to datasets will be communicated through the [release notes](release-notes/index).

## Limitations

Current high-level limitations include: 

* **Data type**: Available for advertising data only. Retail data is not available in Amazon Marketing Stream. 
* **Queues**: Amazon Marketing Stream only allows simple SQS queues. FIFO queues are not supported. 

## Geographic availability

Amazon Marketing Stream is available for advertisers worldwide. The marketplace of the advertiser determines which AWS region you should use when creating queues and subscribing to datasets. Use the table below to understand what countries are supported, and which API endpoint and AWS region to use for each. 

| Marketplace | Region | API endpoint | AWS region|
|-------|------|------|------|
| United Arab Emirates (AE), Germany (DE), Egypt (EG), Spain (ES), France (FR), Belgium (BE), United Kingdom (UK), India (IN), Italy (IT), Netherlands (NL), Poland (PL), Saudi Arabia (SA), Sweden (SE), Turkey (TR) | EU | `https://advertising-api-eu.amazon.com` | eu-west-1 |
| Brazil (BR), United States (US), Canada (CA), Mexico (MX) | NA | `https://advertising-api.amazon.com` | us-east-1|
| Singapore (SG), Australia (AU), Japan (JP) | FE | `https://advertising-api-fe.amazon.com` | us-west-2 |

## General FAQ
<details>
<summary> Is there a cost associated with using Amazon Marketing Stream? </summary>
Maybe. Amazon will not charge advertisers any fees to use Amazon Marketing Stream or to store your data in an Amazon-managed service. However, you might incur costs associated with using AWS, depending on the scope of your data. [Learn more about AWS pricing](https://aws.amazon.com/pricing/) 
</details>
<br>
<details>
<summary>Can anyone who advertises with Amazon use Amazon Marketing Stream? </summary>
Yes. You can request access to Amazon Marketing Stream whether you’re an integrated partner, agency, or a self-service advertiser. Learn more about requesting access in the [Amazon Marketing Stream Onboarding Guide(guides/amazon-marketing-stream/onboarding). 
</details>
<br>
<details>
<summary>I’m in the process of integrating as an Amazon Ads partner. Can I use my existing API-based authentication, or do I need new credentials specifically for Amazon Marketing Stream? </summary>
If you have API credentials, you do *not* need additional authentication. You can use your existing credentials to call the Amazon Marketing Stream API. </details>
<br>
<details>
<summary>Are developer resources, or other specialized skills, required to use Amazon Marketing Stream? </summary>
Yes—at least initially, you’ll need a developer resource to establish the Amazon Marketing Stream Subscription API connection. Amazon Marketing Stream data is streamed to a partner or advertiser-owned SQS queue. Your developers will use the Subscription API to create, confirm, and manage subscriptions. Once authenticated, you will receive a URL to access the data, and API calls are no longer required.  
    In addition, we recommend a solid understanding of how AWS works in order to use Amazon Marketing Stream most efficiently.
</details>
<br>
<details>
<summary>How can Amazon ensure data security when it is transmitted through Amazon Marketing Stream? </summary>
Amazon Marketing Stream ensures that data is delivered only to the subscriptions which are authorized to have access to advertiser data.
</details>
<br>
<details>
<summary>How can I get started with the Amazon Marketing Stream onboarding process? </summary>
When you’re ready to get started, the [Amazon Marketing Stream Onboarding Guide](guides/amazon-marketing-stream/onboarding) walks through the process of getting API access, integrating with AWS, and subscribing to datasets using the Amazon Marketing Stream API. 
</details>
<br>
For additional FAQ on querying your data, see the [Amazon Marketing Stream querying FAQ](guides/amazon-marketing-stream/querying-data#faq).

## Use cases
The main use cases for Amazon Marketing Stream involve the ability to have intra-day insights on performance data over time. This data can help you understand how to adjust bids and budgets throughout the day for better campaign optimization. You can also use Amazon Marketing Stream to leverage near real-time performance data during high traffic events such as Prime Day, so you can change bids and budgets accordingly:

* **Adjust bids**: Based on historical hourly traffic data, you can place bids by the hour of the day instead of once daily. For example, if you see that a particular keyword has high traffic on a certain day and time window, you can place bids more aggressively during that time.
* **Adjust budgets**: Based on ad spend, you can adjust campaign budgets to avoid running out of budget. Conversely, you can be more aggressive with your bids if ad spend has been too low compared with your budget allocation. In addition, Amazon Marketing Stream can help you identify the times of day that are critical hours for your business—for example, when conversions are high and CPC is low. These granular insights can help you ensure that campaigns stay in budget.  

## Sample query
After you subscribe and store Amazon Marketing Stream data in your database, you can run queries based on the data parameters you want to see. Below is an example of what these queries might look like if you want to pull traffic data for a specific time window for a specific keyword. For additional examples and to learn more, see [Querying data with Amazon Marketing Stream](guides/amazon-marketing-stream/querying-data). 

### Pull traffic data for keywords

#### Query

This query shows the delta of a keyword's performance during the 08:00 hour.

>[WARNING] This query is based on a hypothetical setup that assumes Amazon Marketing Stream data is stored in a table called `amazon-marketing-stream.sp-traffic`. Your implementation may use a different database structure or naming convention, so be sure to update the WHERE clause before attempting to run this query.

```
SELECT * 
FROM "amazon-marketing-stream"."sp-traffic" 
WHERE keyword_id = '12345678'and time_window_start = '2022-02-01T10:00:00-08:00' 
ORDER BY day, hour DESC;
```

#### Sample output

The snippet below shows the tabular data that the query might return. Based on this query, you can understand placements (Column I), or where the related product ad was shown during the selected time window. In this example, we’ve filtered to show “Detail Page on-Amazon.” If you look at the metrics fields (Columns N - P), you can see the delta, or how the data is changing as the traffic is validated and corrected:

![Snippet view of sample output](/_images/amazon-marketing-stream/stream-sample-output.png)

For additional examples and to learn more, see the [Amazon Marketing Stream data guide](guides/amazon-marketing-stream/data-guide). 

