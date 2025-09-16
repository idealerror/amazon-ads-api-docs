---
title: Amazon Marketing Stream data guide
description: Guide to Amazon Marketing Stream datasets to help you understand schema, how to apply a resource-based IAM policy, performance metrics, and campaign metadata
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Display
    - Onboarding
keywords:
    - subscribe to data
    - IAM policy
    - SQS queue
    - schema
    - marketplace identifier
---

# Amazon Marketing Stream data guide

Amazon Marketing Stream supports datasets that advertisers can subscribe to. Each dataset supports different performance metrics or campaign metadata. 

For each dataset you subscribe to, make sure you are use the correct subscription endpoint and [confirm the subscription in SQS](guides/amazon-marketing-stream/onboarding#step-4-confirm-your-subscription-in-sqs). 

## Amazon DSP performance

|Dataset ID	|NA	|EU	|FE	|Documentation	|Subscription endpoint	|
|---	|---	|---	|---	|---	|---	|
|adsp-traffic	|x	|x	|x	|[Link](guides/amazon-marketing-stream/datasets/adsp-performance#amazon-dsp-traffic-dataset)	|[POST /dsp/streams/subscriptions](amazon-marketing-stream/openapi#tag/DSP-Stream-Subscription/operation/CreateDspStreamSubscription)	|
|adsp-conversion	|x	|x	|x	|[Link](guides/amazon-marketing-stream/datasets/adsp-performance#amazon-dsp-conversion-dataset)	|[POST /dsp/streams/subscriptions](amazon-marketing-stream/openapi#tag/DSP-Stream-Subscription/operation/CreateDspStreamSubscription)	|
|adsp-clickstream	|x	|x	|x	|[Link](guides/amazon-marketing-stream/datasets/adsp-performance#amazon-dsp-clickstream-dataset)	|[POST /dsp/streams/subscriptions](amazon-marketing-stream/openapi#tag/DSP-Stream-Subscription/operation/CreateDspStreamSubscription)	|
|adsp-rich-media	|x	|x	|x	|[Link](guides/amazon-marketing-stream/datasets/adsp-performance#amazon-dsp-rich-media-datase)	|[POST /dsp/streams/subscriptions](amazon-marketing-stream/openapi#tag/DSP-Stream-Subscription/operation/CreateDspStreamSubscription)	|


## Sponsored Products performance

| Dataset ID | NA | EU | FE | Documentation | Subscription endpoint |
|-----|-------|----|---|-----|----|
| sp-traffic | x | x | x | [Link](guides/amazon-marketing-stream/datasets/sp-performance#sponsored-products-traffic-dataset) | [POST /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription)|
| sp-conversion | x | x | x | [Link](guides/amazon-marketing-stream/datasets/sp-performance#sponsored-products-conversions-dataset) | [POST /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription)|

## Sponsored Display performance

| Dataset ID | NA | EU | FE | Documentation | Subscription endpoint |
|-----|-------|----|---|-----|----|
| sd-traffic | x | x | x | [Link](guides/amazon-marketing-stream/datasets/sd-performance#sponsored-display-traffic-dataset)| [POST /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription)|
| sd-conversion | x | x | x | [Link](guides/amazon-marketing-stream/datasets/sd-performance#sponsored-display-conversions-dataset)| [POST /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription)|

## Sponsored Brands performance

| Dataset ID | NA | EU | FE | Documentation | Subscription endpoint |
|-----|-------|----|---|-----|----|
| sb-traffic | x | x | x | [Link](guides/amazon-marketing-stream/datasets/sb-performance#sponsored-brands-traffic-dataset-beta)| [POST /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription)|
| sb-conversion | x | x | x | [Link](guides/amazon-marketing-stream/datasets/sb-performance#sponsored-brands-conversion-dataset-beta)| [POST /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription)|
| sb-clickstream | x | x | x | [Link](guides/amazon-marketing-stream/datasets/sb-performance#sponsored-brands-clickstream-dataset-beta)| [POST /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription)|
| sb-rich-media | x | x | x | [Link](guides/amazon-marketing-stream/datasets/sb-performance#sponsored-brands-rich-media-dataset-beta)| [POST /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription)|


## Sponsored ads budget usage

| Dataset ID | NA | EU | FE | Documentation | Subscription endpoint |
|-----|-------|----|---|-----|----|
| budget-usage |  x | x | x | [Link](guides/amazon-marketing-stream/datasets/budget-usage)| [POST /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription)|


## Sponsored ads recommendations

| Dataset ID | NA | EU | FE | Documentation | Subscription endpoint |
|-----|-------|----|---|-----|----|
| sponsored-ads-campaign-diagnostics-recommendations |  x | x | x | [Link](guides/amazon-marketing-stream/datasets/sponsored-ads-campaign-diagnostics-recommendations)| [POST /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription)|

## Sponsored Products budget recommendations and missed opportunities

| Dataset ID | NA | EU | FE | Documentation | Subscription endpoint |
|-----|-------|----|---|-----|----|
| sp-budget-recommendations |  x | x | x | [Link](guides/amazon-marketing-stream/datasets/budget-recs-missed-opportunities)| [POST /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription)|


## Sponsored ads campaign management

| Dataset ID | NA | EU | FE | Documentation | Subscription endpoint |
|-----|-------|----|---|-----|----|
| campaigns | x | x | x | [Link](guides/amazon-marketing-stream/datasets/sponsored-ads-campaign-management#campaigns-dataset-beta)| [POST /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription)|
| adgroups | x | x | x | [Link](guides/amazon-marketing-stream/datasets/sponsored-ads-campaign-management#ad-groups-dataset-beta)| [POST /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription)|
| ads | x | x | x | [Link](guides/amazon-marketing-stream/datasets/sponsored-ads-campaign-management#ads-dataset-beta)| [POST /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription)|
| targets | x | x | x | [Link](guides/amazon-marketing-stream/datasets/sponsored-ads-campaign-management#targets-dataset-beta)| [POST /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription)|


## Amazon DSP campaign management

| Dataset ID | NA | EU | FE | Documentation | Subscription endpoint |
|-----|-------|----|---|-----|----|
| adsp-campaigns | x | x | x | [Link](guides/amazon-marketing-stream/datasets/dsp-campaign-management#amazon-dsp-campaigns-beta)| [POST /dsp/streams/subscriptions](amazon-marketing-stream/openapi#tag/DSP-Stream-Subscription/operation/CreateDspStreamSubscription)|
| adsp-campaign-flights | x | x | x | [Link](guides/amazon-marketing-stream/datasets/dsp-campaign-management#amazon-dsp-campaign-flights-beta)| [POST /dsp/streams/subscriptions](amazon-marketing-stream/openapi#tag/DSP-Stream-Subscription/operation/CreateDspStreamSubscription)|
| adsp-adgroups | x | x | x | [Link](guides/amazon-marketing-stream/datasets/dsp-campaign-management#amazon-dsp-ad-groups-beta)| [POST /dsp/streams/subscriptions](amazon-marketing-stream/openapi#tag/DSP-Stream-Subscription/operation/CreateDspStreamSubscription)|
| adsp-adgroup-targets | x | x | x | [Link](guides/amazon-marketing-stream/datasets/dsp-campaign-management#amazon-dsp-ad-group-targets-beta)| [POST /dsp/streams/subscriptions](amazon-marketing-stream/openapi#tag/DSP-Stream-Subscription/operation/CreateDspStreamSubscription)|

>[TIP] What new datasets would you like to see in Amazon Marketing Stream? If you have any feedback, please start a [GitHub discussion](https://github.com/amzn/ads-advanced-tools-docs/discussions/new?category=ideas) to let us know what data you would like to see, and how it would add value to your business. 

## Learn more

- [Stream onboarding guide](guides/amazon-marketing-stream/onboarding)
- [Managing Stream subscriptions](guides/amazon-marketing-stream/managing-subscriptions)
