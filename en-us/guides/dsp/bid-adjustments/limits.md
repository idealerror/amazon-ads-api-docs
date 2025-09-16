---
title: Rate and size limits for the ADSP bid adjustments API
description: A description of request limits for the ADSP bid adjustments API
type: guide
tags: 
    - Amazon DSP
interface: api
---

# DSP bid adjustments rate and size limits

## Request quotas for each Bid Adjustments API operation

The Bid Adjustments API establishes quotas for the number of API operations requested in each second. The request quotas differ for the API operations. When you exceed an API request quota, the service [throttles the request](reference/concepts/rate-limiting).


|Endpoint	|Default value (requests per second) 	|
|---	|---	|
|POST /dsp/rules/bidmodifier	|10	|
|POST /dsp/rules/bidmodifier/list	|5	|
|DELETE /dsp/rules/bidmodifier/{bidModifierRuleId}	|10	|
|GET /dsp/rules/bidmodifier/{bidModifierRuleId}	|10	|
|DELETE /dsp/rules/bidmodifier/{bidModifierRuleId}/associations	|10	|
|PUT /dsp/rules/bidmodifier/{bidModifierRuleId}/associations	|10	|
|POST /dsp/rules/bidmodifier/{bidModifierRuleId}/associations/list	|10	|
|POST /dsp/rules/bidmodifier/{bidModifierRuleId}/associations/validate	|10	|

## System limits

| Limit type	           | Maximum value	 | Description	                                                                              |
|-----------------------|----------------|-------------------------------------------------------------------------------------------|
| Size limit	           | 1 MB	          | Total payload size for a bid adjustment rule	                                             |
| Terms in rule	        | 1,000	         | Number of terms (e.g., Term 1: mobile devices, Term 2: regions, ... Term 1000: browsers)	 |
| Values per dimension	 | 1,000	         | Number of distinct values allowed per dimension	                                          |
| Rule description	     | 1,024 chars	   | Character limit for rule descriptions	                                                    |
| Ad group IDs	         | 1,000	         | Number of ad groups that can be associated in one API call	                               |
| Bid modifiers	        | 1,000	         | Maximum number of rules returned in List API	                                             |
| Number of Rules       | 2,000          | Maximum number of rules that can be created by advertiser                                 |

## Increasing Quotas

To increase a quota, follow these steps and it will be reviewed on a case-by-case basis.

1. Visit the [Support page](support/overview).
2. Reach out to support and request an increase. Make sure to include in request:
    * Advertiser ID
    * Use case
    * Requested limit
    * Justification for increase
    * Subject Line: ADSP Bid Adjustments API Quota Increase for [Limit Type]



