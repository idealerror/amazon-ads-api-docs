---
title: Amazon Marketing Stream release notes
description: Release notes for Amazon Marketing Stream
type: release-note
interface: amazon-marketing-stream
tags:
    - Sponsored Products
    - Sponsored Display
keywords:
    - stream 
    - release notes 
---
# Amazon Marketing Stream release notes

This document contains release notes related to the Amazon Marketing Stream before 2023. For more recent announcements, see [Release notes](release-notes/index).

## December 2022

### Sponsored Display now available in Amazon Marketing Stream (beta) in North America

Amazon Marketing Stream (beta) has now expanded to include Sponsored Display in North America, allowing partners and advertisers to receive performance metrics and information on campaign changes in near real time. Information is pushed to advertisers and partners through the Amazon Ads API, eliminating the need to aggregate metrics over time to understand the changes between hourly performance, and enabling timely notifications on campaign changes. 

Amazon Marketing Stream can help you improve Sponsored Display performance with traffic and performance metrics summarized hourly instead of daily, informing intra-day optimizations. Advertisers and partners will also receive timely notifications on campaign changes that can help you take ** actions in near real time, such as a campaign going out of budget.  

New metrics available through this Sponsored Display launch include new-to-brand metrics and view-based attribution for vCPM (cost-per-thousand viewable impressions) campaigns, summarized hourly. Amazon Marketing Stream can help you understand Sponsored Display campaign KPIs such as impressions, clicks, attributable sales, orders, and new-to-brand sales, broken down by hour of day. For example, you may learn that new-to-brand attributed sales, units and orders are higher during hours 8pm-10pm, informing hourly bid adjustments. You can also determine when views or clicks (vCPM) lead to more conversions, by hour of day.

When you’re ready to get started, visit the Amazon Marketing Stream [onboarding guide](guides/amazon-marketing-stream/onboarding).

## October 2022

### Amazon Marketing Stream (beta) global launch (except IN)

We are excited to expand Amazon Marketing Stream (beta) to all countries currently supported by the Amazon Ads API, with the exception of India. Amazon Marketing Stream is a new service that delivers Amazon Ads campaign metrics and information to advertisers’ or integrators’ AWS accounts via a push-based model in near real time. You can subscribe to Amazon Marketing Stream datasets using your API token and by providing your AWS account details. Once you are subscribed, Amazon Marketing Stream delivers hourly, near real-time performance metrics, with details such as targeting expression performance by placement, and budget consumption messages Other campaign datasets will be available for subscription in the coming months. The service, which was first launched for North America (United States, Canada, and Mexico) on June 21, 2022, is now available globally (except in India).

Amazon Marketing Stream can help in the following ways:

* _Optimize campaigns more effectively:_ Hourly performance metrics provide intra-day insights for advanced campaign optimization. For example, during certain hours of the day when your conversion rate is high, you may want to increase bid amounts or make sure your campaign stays in budget during that time. Sponsored Products performance metrics from Amazon Marketing Stream enables new dimensions such as the combination of targeting expression and ad placement, so you can not only optimize on your keywords but keywords by placement.
* _Respond quickly to campaign changes:_ Near real-time information such as budget consumption can help advanced API users write responsive applications for further optimization. For example, they may want to increase the campaign budget before it runs out. 
* _Improve_ _operational efficiency:_ Push-based information delivery eliminates API throttling. Moreover, Amazon Marketing Stream sends you performance metric and campaign information changes in near real time, instead of aggregated over a period of time. You can simplify complex technical operations — which today may entail comparing new aggregated API metrics with the previously pulled metrics, to understand the delta between the two. Instead, Amazon Marketing Stream delivers that delta to you.

Since its launch in North America, customers have been keen to expand the value of Stream globally, and we are excited to address this request. While the above are some common ways in which we believe our customers - agencies, tool providers, and advertisers - will find Amazon Marketing Stream valuable, the ingenuity in using our products to solve problems in unimagined ways has been inspiring. We look forward to learning from our customers and continuing to evolve the product.

Amazon Marketing Stream will be only available via the Amazon Ads API. To learn more about how to onboard onto Amazon Marketing Stream, read the [Onboarding guide](guides/amazon-marketing-stream/onboarding).

## July 2022

### Amazon Marketing Stream Postman collection

The Amazon Ads API Postman collection now supports the Amazon Marketing Stream [subscription APIs](amazon-marketing-stream/openapi). Advertisers and integrators can use the collection to create and manage Amazon Marketing Stream subscriptions, and view example calls to the API. 

To learn more about getting started with Postman, see our [Postman tutorial](guides/postman). 

### Amazon Marketing Stream CloudFormation template

You can now use a CloudFormation template to streamline the Amazon Marketing Stream onboarding process. AWS CloudFormation provides an Infrastructure as Code (IaC) approach to managing your SQS queues and policies. With the template, you can use CloudFormation to programatically create queues and apply the relevant IAM policy. 

To get started using CloudFormation, read our [CloudFormation onboarding guide](guides/amazon-marketing-stream/cloud-formation). You can find the CloudFormation template in the ads-advanced-tools [GitHub repository](https://github.com/amzn/ads-advanced-tools-docs/blob/main/amazon_marketing_stream/Stream_SQS%20_CF_Template.yaml).

## June 2022

### Amazon Marketing Stream (Beta)

Regions: US, CA, MX

Amazon Marketing Stream has launched to open beta, allowing partners and advertisers to receive campaign data in near real time, without needing to repeatedly call the API. Amazon Marketing Stream offers new capabilities compared with an API-based reporting approach, including traffic and conversion data summarized hourly instead of daily, less throttling, additional reporting dimensions, and better insights on deltas instead of aggregates. 

In the past, access to scaled campaign reporting was limited to pull-based API calls that provided daily performance. In addition, the API needed to be called multiple times throughout the day, which led to throttling and a less efficient workflow.  

Amazon Marketing Stream is available for Sponsored Products campaigns and sponsored ads budget usage via the Amazon Ads API for agencies, tool providers, and direct advertisers, including vendors and sellers in Canada, Mexico, and the United States. When you’re ready to get started, visit the Amazon Marketing Stream [onboarding guide](guides/amazon-marketing-stream/onboarding).
