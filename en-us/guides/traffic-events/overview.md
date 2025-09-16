---
title: Traffic events API overview
description: Introduction to and explanation of the traffic events API, including frequently asked questions.
type: guide
interface: api
tags:
    - Reporting
keywords:
    - SQL query
    - clean room
    - FAQ
---

# Traffic events API

The traffic events API is a data clean room-based service that helps advertisers access event-level data for eligible campaigns, such as impressions and clicks. This data can be queried using standard SQL syntax within the body of the traffic events API call. 

To call the traffic events API, you must first establish (Login With Amazon) LWA credentials for reporting access to one or more of our ad serving products: Amazon Demand Side Platform, Sponsored Ads, Amazon Ad Server. The traffic events API will return results if the advertiser account ID you include in the API call is associated with any eligible campaigns.

### Key features

- **Improved data granularity.** Traffic events data is available at the event level with aggregation levels designed to protect customer privacy.
- **More reporting dimensions.** More reporting dimensions in addition what is already available in current invalid traffic reporting.
- **API access.** You can integrate our API with your reporting applications for ease of use.

## Where is the traffic events API available?

- **North America:** United States, Canada, Mexico
- **South America**: Brazil
- **Europe:** Germany, Spain, France, Italy, Netherlands, United Kingdom, Sweden, Turkey
- **Middle East:** Saudi Arabia, United Arab Emirates
- **Asia Pacific:** Australia, India, Japan

>[NOTE]The traffic events API only contains data for ad campaigns currently serving impressions in EU.

## How the traffic events API works

Pseudonymized inputs from Amazon Ads campaign events -- such as ad impressions, ad clicks, and ad views that span across media including streaming TV, audio, video, display, and sponsored ads -- are uploaded into the traffic events clean room (TCR).

All analysis takes place within the clean room -- across dimensions like time, creative, or campaigns, to name a few -- and results are aggregated to generate anonymous reports that you can export.

TCR prioritizes privacy and security by design by accepting only pseudonymized information. All information in TCR is handled in strict accordance with Amazon's privacy notice, and raw event information cannot be exported.

TCR will receive data within 72 hours of the impression being served. Starting from the time the data reaches TCR, it will be retained for a period of 30 days.

## Accessing the traffic events API

To access the traffic events API, you will use a combination of the Marketplace ID and the Sponsored Ads Entity ID or the Amazon DSP Advertiser ID that you wish to query data from. In order to query data for a specific advertiser, you must have specific permissions on that account. See [Advertiser account management for traffic events API access](guides/traffic-events/account-management).

## Rate limits

Traffic events sets API rate limits to ensure all clients can use traffic events effectively. API rate limits are defined as a maximum number of calls a client can make to each endpoint within a time period. If your app makes a lot of API requests in a short period of time then it may receive a 429 error response. Traffic events also sets limits at the workflow execution level that will return a 402 status code. An advertiser may have up to 5 concurrent running workflow executions. If you attempt to create new execution while there are 5 workflow executions already running, you will receive 402 status code.

## FAQ

<details>
<summary><strong>Is there a cost associated with using the traffic events API?</strong></summary>
 The traffic events API is available at no cost to eligible advertisers via API.
</details>

<details>
<summary><strong>Can anyone who advertises with Amazon Ads use the traffic events API?</strong></summary>
 Yes, any eligible Amazon Ads advertiser can use traffic events API. 
</details>

<details>
<summary><strong>Who can access the traffic events API?</strong></summary>
 If you have reporting access to any of our ad-serving products Amazon Demand Side Platform, Sponsored Ads, Amazon Ad Server, you will be able to access the traffic events API with your Login With Amazon (LWA) credentials.
</details>

<details>
<summary><strong>I’m in the process of integrating as an Amazon Ads partner. Can I use my existing API-based authentication, or do I need new credentials specifically for the traffic events API?</strong></summary>
 You will not need new credentials for the traffic events API.
</details>

<details>
<summary><strong>Are developer resources, or other specialized skills, required to use the traffic events API?</strong></summary>
 You need to know SQL to query data from the database and be able to make an API call. Detailed instructions and illustrative examples are provided in this guide to help you.
</details>

<details>
<summary><strong>How can I get started with the traffic events API onboarding process?</strong></summary>
    You will need to follow the steps defined in the [Getting started guide](guides/traffic-events/getting-started).
</details>

<details>
<summary><strong>How many calls can I make to the traffic events API at the same time?</strong></summary>
 You can make up to 5 concurrent calls.
</details>

<details>
<summary><strong>Why did my workflow execution produce an empty result?</strong></summary>
 
If your workflow execution is in a SUCCEEDED status but the results file is empty, this could be due to a number of reasons related to the way you constructed your query.
    <ul>
            <li>The campaign you queried did not have any traffic in an eligible locale.</li>
            <li>The constraints you placed on the query did not align with any traffic data. For example if your query’s time window was set for a time when your campaign was not actively serving eligible traffic then it is expected to return no data. Another example could be that you selected a specific ad id which was actively serving traffic, but selected a creative id which was not being used during your specified time window.</li>
            <li>If your workflow execution is in a status other than SUCCEEDED you may need to wait for it to complete to fetch the results or see the ‘WorkflowExecutionStatusReason’ for more information.</li>
    </ul>
</details>

<details>
<summary><strong>I ran my query before and it returned results but when I run it now it does not, why?</strong></summary>
 Assuming you are running the exact same query:
    <ul>
        <li>The data which was queried when you previously ran the workflow execution could have been deleted from our system. Check the time range of your query to ensure the start and end date are within the last 30 days.</li>
        <li>If you have altered the query since you last ran it then see the answer for “Why did my workflow execution produce an empty result?”</li>
    </ul>
</details>

<details>
<summary><strong>I use Managed Services, how can I get access to the traffic events API?</strong></summary>
 Please contact your AE or AM to get you set up with self-service reporting access. Once you have the credentials to successfully access the reporting console, use the same credentials to make the traffic events API call. If you would like to get standard traffic events API data set for your EU campaigns, you can request your AE or AM to pull this data for you.
</details>

## Next steps

To make your first call to the traffic events API, see [Getting started](guides/traffic-events/getting-started).
