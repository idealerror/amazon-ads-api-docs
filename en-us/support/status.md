---
title: Known issues
description: View the current statuses for recent known issues related to Amazon Ads advanced tool.  
type: guide
interface: general
tags:
    - Operations
keywords:
    - known issues
    - help
    - troubleshooting
---
# Amazon Ads Status

[Amazon Ads Status](https://status.ads.amazon.com/) provides all advertisers with the latest status information on Amazon Ads services. Advertisers can see the current status of services like campaign creation, campaign management, ad delivery, and reporting on sponsored ads and Amazon DSP. Should there be a service interruption, advertisers can see more details for the event, including the description and status updates for that service.

In the past, there has not been a dedicated location where advertisers or API integrators could look to get the current status of Amazon Ads services. During any service interruption, advertisers could only get that information after they log in to the advertising console or Amazon DSP. With the introduction of Amazon Ads Status, users can now view the current status of Amazon Ads services and be updated with the latest information during any potential service disruptions, without the need to log in to the advertising console or Amazon DSP.

## Programmatic access to Amazon Ads Status

The ads status information can be accessed programmatically using https://status.ads.amazon.com/status.json, which returns Amazon Ads Status in JSON format. 

An empty `status` (indicated by `[]`) means there are no active alerts. If there are active alerts, you will see them listed within the `status` array, with separate items for region and category. You can refer to the [JSON schema](https://status.ads.amazon.com/status.schema.json) to understand the expected structure of each alert type.

## Status history

Advertisers can also review communications and status updates shared by Amazon Ads spanning the past 6 months. The status history can be accessed programmatically using https://status.ads.amazon.com/history.json. 

You can refer to the [JSON schema](https://status.ads.amazon.com/history.schema.json) to understand the expected structure of the status history.

## Language of preference

Translated status messages and history can be accessed programmatically by replacing `en-US` in the following URLs with the correct locale string for any of the languages available in the status console view:

- Status: https://status.ads.amazon.com/translatedJson/status/en-US
- History: https://status.ads.amazon.com/translatedJson/history/en-US
