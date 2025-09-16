---
title: Bid adjustments API guide
description: An introduction to the ADSP bid adjustments API and explanation of how it works.
type: guide
interface: api
---

# How does the bid adjustments API work?

## Overview

![Bid Adjustments Overview](/_images/bid-adjustments/bid_adjustments_rule_implementation_flow.png)

## Terminologies

**Bid adjustment term**

A bid adjustment term has 2 primary elements to apply if a bid request matches the criteria:

* Dimension(s) matching criteria.
* Bid adjustment.

**Bid adjustment rule**

A setting that contains 1 or more bid adjustment terms. Each bid adjustment rule is defined in one JavaScript Object Notation (JSON) file uploaded through an Application Programming Interface (API). A bid adjustment rule can be associated with multiple line items. However, only 1 bid adjustment rule can be associated with a line item at the same time. If you try to associate a new bid adjustment rule with an ad line that has an existing bid adjustment rule, the latest bid adjustment rule will take effect and override the older one.

>[NOTE] You can write up to 1000 terms in each bid adjustments rule/file uploaded.

**Bid adjustment**

The value to multiply the predicted impression value with if the bid request matches the bid adjustment dimensions. The minimum value is 0, the maximum is 10.0. Decimals will be truncated to 2 decimal places (Example: 1.25). Bid adjustment of 1 does not change anything.

You can adjust bids across dimensions like domain, app, device, location, and Amazon Audiences.

## Bid adjustment rule schema

Note that **all field names and dimension names are required to use camel case** in API calls. For example,  if `ruleEXPRESSION` is provided instead of `ruleExpression`, or dimension name `devicetype` is provided instead of `deviceType`, then the API will throw a validation error. We require the file size for a bid adjustment rule do not exceed 1MB.

The bid adjustment rule that you create will only be available in the region it was initially created in, based on the regionalized Advertising API you are using. See the [Amazon Ads API overview](reference/api-overview) for a detailed breakdown of the 3 regions (North America \[NA], Europe, \[EU], and Far East \[FE]) and associated endpoints.

| Field Name	      | Purpose of Field	                                                                                                                                                                                                                                                                                                                                                                                                                                         |Data Type	|
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---	|
| ruleDescription	 | Description of what the bid adjustment rule is about. This is metadata for audit/tracking purposes	                                                                                                                                                                                                                                                                                                                                                         |String	|
| ruleExpression	  | The bid adjustment rule expression holds all the bid adjustment terms and how to reconcile multiple bid adjustment terms matching the bid request by "onMultipleMatches". Each term specifies the dimensions to match, and the `bidAdjustment` amount.                                                                                                                                                                                                          | String
| terms	           | Each term object is a **data blob** that specifies the dimension matching criteria and bid adjustment. **Dimension values are list type. You must provide dimension values using list notation `[]`, even if there is only 1 dimension value.** You may also pass multiple dimension values, and the term will apply if any value gets matched ('OR' condition). You can write up to 1000 terms in each bid adjustments rule.	                              |List[Object]
|onMultipleMatches| Specify how to reconcile multiple bid adjustment terms matching the bid request. Default is `APPLY_PRODUCT` and currently this is the **only supported value.** If `APPLY_PRODUCT` is specified, bid adjustment from multiple terms will be multiplied together.                                                                                                                                                                                            | Enum
|bidAdjustment| The value to multiply the predicted impression value with if the bid request matches the bid adjustment dimensions. The minimum value is 0, the maximum is 10.0. Note that **decimals will be truncated to 2 decimal places** (e.g., 1.25)                                                                                                                                                                                                                  |Decimal
|negative (optional)| This is for setting negative bid adjustment. This is a `true/false` field, with `true` indicating that the bid adjustment will be applied when none of the dimensions match (NOT A AND NOT B AND NOT C etc.). For example, if you want to set bid adjustment for all domains other than 'cnn.com' and 'bbc.com', you may set a negative bid adjustment when none of 'cnn.com' and 'bbc.com' matched. This is an **optional** field, and the default is `false`. | Boolean

## How do bid adjustments work?

Bid adjustments work in conjunction with all existing bid strategies and Amazon DSP automated optimization.  You can create either single dimension bid adjustments or multi-dimension bid adjustments.

### Example: Single dimension bid adjustments


It assigns one bid adjustment value to a single dimension value.

|Device type	|Bid adjustment	|
|---	|---	|
|TV	|2	|
|PC	|0.6	|
|Phone	|1.5	|

### Multi-dimension bid adjustments


It assigns one bid adjustment value to an intersection of 2 or more dimensions. Multiple dimensions are used at the same time to set the matching criteria. In below example, when all conditions are satisfied: country = 'US' AND region = 'NY' and city = 'New York', bid adjustment of 1.5 will be applied.

|Country	|Region	|City	|Bid adjustment	|
|---	|---	|---	|---	|
|US	|NY	|New York	|1.5	|

**Negative option**

This is a `true`/`false` field and default is `false`. One can use a negative field to ‘exclude’ certain dimension values. In below example, if 'negative' is set to be true, it means if domain is not foo.com, bid adjustment of 1.1 will be applied.

|Domain	|Bid adjustment	|Negative	|
|---	|---	|---	|
|[foo.com](http://foo.com/)	|1.1	|TRUE	|

**Multi-value input**

You can use a list of values for mutli-value input for a specific dimension. Use brackets to contain the list of values and each value will need to be quoted and  separated by comma. In above example, when either domain is matched, bid adjustment of 1.25 will be applied.

|Domain	|Bid adjustment	|	
|---	|---	|		
|["foo.com", "foo1.com",  "foo2.com", "foo3.com"]	|1.25	|	

Above is equivalent to the following:

|Domain	|Bid adjustment	|
|---	|---	|
|[foo.com](http://foo.com/)	|1.25	|
|[foo1.com](http://foo1.com/)	|1.25	|
|[foo2.com](http://foo2.com/)	|1.25	|
|[foo3.com](http://foo3.com/)	|1.25	|

## How are bid adjustments calculated?

***Example 1  Single dimension and multi-dimension bid adjustment***

Suppose you have bid adjustments like the ones below. You have 1 single dimension bid adjustment for domain and 1 multi-dimension bid adjustment with device type and operating system.

|Domain	|Device type	|Operating system	|Bid adjustment	|
|---	|---	|---	|---	|
|[foo.com](http://foo.com/)	|	|	|0.75	|
|	|["Phone", "Tablet"]	|iOS	|1.25	|

How the final bid adjustment is calculated based on the above bid adjustment terms:

|Device  type	|Operating system	|Domain	|Final bid adjustment	|
|---	|---	|---	|---	|
|Phone	|iOS	|[foo2.com](http://foo2.com/)	|bid adjustment = bid  adjustment \* 1.25	|
|Tablet	|iOS	|[foo1.com](http://foo1.com/)	|bid adjustment = bid  adjustment \* 1.25	|
|Tablet	|iOS	|[foo.com](http://foo.com/)	|bid adjustment = bid  adjustment \* 1.25 \* 0.75	|
|Phone	|Android	|[foo.com](http://foo.com/)	|bid adjustment = bid  adjustment \* 0.75	|
|Phone	|Android	|[foo2.com](http://foo2.com/)	|No match, bid adjustment  unchanged	|
|Tablet	|Android	|[foo2.com](http://foo2.com/)	|No match, bid adjustment  unchanged	|
|PC	|iOS	|[foo1.com](http://foo1.com/)	|No match, bid adjustment  unchanged	|

***Example 2  Single dimension bid adjustment with negative field***

For  single dimension bid adjustments, including the negative field means that the  bid adjustment will be applied if the dimension value does not match in the  bid request.

|Domain	|Bid adjustment	|Negative	|
|---	|---	|---	|
|["foo.com","foo1.com"]	|1.5	|TRUE	|
|["foo2.com","foo3.com"]	|2	|	|

How the final bid adjustment will be calculated based on above terms:

|Domain	|Final bid adjustment	|
|---	|---	|
|[foo.com](http://foo.com/)	|No match, bid adjustment unchanged	|
|[foo1.com](http://foo1.com/)	|No match, bid adjustment unchanged	|
|[foo2.com](http://foo2.com/)	|bid adjustment = bid adjustment \* 1.5 \* 2	|
|[foo3.com](http://foo3.com/)	|bid adjustment = bid adjustment \* 1.5 \* 2	|
|[foo4.com](http://foo4.com/)	|bid adjustment = bid adjustment \* 1.5	|
|[foo5.com](http://foo5.com/)	|bid adjustment = bid adjustment \* 1.5	|

***Example 3 Multi-dimension bid adjustment with negative field***

For multi-dimension bid adjustments, including the negative field means that the bid adjustment will only be applied if none of the dimensions match in the bid request. One could take this as negative term for each dimension to be satisfied: if (NOT A) AND (NOT B) AND (NOT C), apply bid adjustment.

|Domain	|Device type	|Operating system	|Bid adjustment	|Negative	|
|---	|---	|---	|---	|---	|
|["foo.com","foo1.com"]	|Phone	|iOS	|1.5	|TRUE	|
|["foo2.com","foo3.com"]	|	|	|2	|	|

How the final bid adjustment will be calculated based on above terms:

|Domain	|Device type	|Operating system	|Final bid adjustment	|
|---	|---	|---	|---	|
|[foo.com](http://foo.com/)	|Tablet	|Android	|No match, bid adjustment unchanged	|
|[foo4.com](http://foo4.com/)	|Tablet	|Android	|bid adjustment = bid adjustment \* 1.5	|
|[foo2.com](http://foo2.com/)	|Tablet	|Android	|bid adjustment = bid adjustment \* 1.5 \* 2	|
|[foo2.com](http://foo2.com/)	|Phone	|iOS	|bid adjustment = bid adjustment \* 2	|
|[foo5.com](http://foo5.com/)	|Phone	|iOS	|No match, bid adjustment unchanged	|
