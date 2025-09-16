---
title: Paid features overview
description: An overview of the AMC paid features data sources
type: guide
interface: api
---

# Paid features overview

## What are AMC Paid features?

AMC Paid Features are a set of enhanced datasets that provide more expanded insights into customer behavior and advertising campaign performance than the standard AMC features. For analytics queries, these features require a monthly subscription and unlock access to a variety of signals, allowing for more granular analysis and targeted campaigns. Depending on availability and eligibility, you can you can leverage many of the associated paid features audience tables without a subscription.

## Paid features availability and eligibility

Which Paid features datasets that are available to you depend on:

* The availability of the feature in certain regions and marketplaces. Many paid features are available in US/CA/JP/AU/FR/IT/ES/UK/DE marketplaces; however, some are limited to a subset of these marketplaces.
* The eligibility requirements of each paid feature (for example, Prime Video Channel insights is only available for Amazon advertisers that operate Prime Video Channels).

### Analytics

For analytics datasets, before you can enroll in a paid features subscription or free trial, the paid feature must be available for the region or marketplace associated with your account and your instance must meet certain eligibility requirements . 

If the paid feature is not available for your region or marketplace, you will not see it on the Paid feature tab. If it is available, but you are not eligible, you will not be able to click Subscribe on the Paid features tab.

### Audiences

For audience datasets, if the Paid feature is available in your region or marketplace and your instance is eligible, the audience table will be automatically available in your instance. In this case, you do not have to subscribe to the Paid feature to start generating audiences.

If it is not available or you are not eligible, you will not be see the audience table in your instance and you will not be able to subscribe to that paid feature.

### Signal availability

The signals included in the paid features dataset may vary based on region or marketplace. For example, audiences created with the Amazon Retail Purchases (ARP) dataset will not include signals from the EU region or UK marketplace. These signals, however, are available for measurement queries using ARP. 

### AMC paid features billing

AMC Paid feature subscription enrollments are facilitated through the eligible billing account types listed below. The billing accounts are inferred based on the advertiser resources that are associated with the AMC instance object.

- Sponsored Ads entity billing accounts
- DSP Self-Service entity billing accounts
- Amazon Advertising Incentive Center Measurement Funds

## Flexible Shopping Insights (FSI) as a part of Amazon Insights

**Availability:**  AMC accounts associated with US/CA/JP/AU/FR/IT/ES/UK/DE marketplaces

**Eligibility:** Any AMC advertisers who are brand owners or sellers

**Contains signals from:** All available marketplaces

Flexible Shopping Insights (FSI) is a part of Amazon Insights subscription, and enables advertisers to generate combined, aggregated, and anonymized advertising and shopping insights for brands and products advertised through Amazon Ads

* conversions\_all

For eligible advertisers and instances, the associated audience table can be used for AMC audience creation without a Paid feature subscription:

* conversions\_all\_for\_audiences

Refer to [conversions_all](guides/amazon-marketing-cloud/datasources/conversions_all_paid).

## Audience Segment Insights (ASI) as a part of Amazon Insights

**Availability:**  AMC accounts associated with the US/CA/JP/AU/FR/IT/ES/UK/DE marketplaces

**Eligibility:** Any AMC advertisers

**Contains signals from:** All available marketplaces

Amazon Audience Segment Insights enable advertisers to conduct user-to-segment analysis within Amazon Marketing Cloud (AMC) for all Amazon Ads customers across all datasets published into AMC as well as segment-level analysis for advertiser uploaded datasets.

Amazon Audience Segment Insights is a part of the Amazon Insights subscription for advertisers that sell on Amazon (endemic). Alternatively, Amazon Audience Segment Insights is available for advertisers that do not sell on Amazon (non-endemic) via a la carte subscription.

* audience\_segments\_[amer|apac|eu]\_inmarket 
* audience\_segments\_[amer|apac|eu]\_inmarket\_snapshot 
* audience\_segments\_[amer|apac|eu]\_lifestyle 
* audience\_segments\_[amer|apac|eu]\_lifestyle\_snapshot
* segment\_metadata

For eligible advertisers and instances, the associated audience tables can be used for AMC audience creation without  a Paid feature subscription:

* audience\_segments\_[amer|apac|eu]\_inmarket\_for\_audiences
* audience\_segments\_[amer|apac|eu]\_inmarket\_snapshot\_for\_audiences
* audience\_segments\_[amer|apac|eu]\_lifestyle\_for\_audiences
* audience\_segments\_[amer|apac|eu]\_lifestyle\_snapshot\_for\_audiences
* segment\_metadata\_for\_audiences

Refer to [Amazon Audience Segment Insights](guides/amazon-marketing-cloud/datasources/audience_segment_membership_paid).

## Amazon your garage

**Availability:**  AMC accounts associated with US and CA marketplaces

**Eligibility:** Any AMC advertisers

**Contains signals from:** North American region (US/CA/MX marketplaces)

Additionally, the Amazon Your Garage paid feature provides user-to-vehicle association details that describe vehicle type, the make and model of the vehicle, as well as detailed vehicle configuration attributes expressed in the context of an AMC `user_id`.

* amazon\_your\_garage

For eligible advertisers and instances, the associated audience table can be used for AMC audience creation without  a Paid feature subscription:

* amazon\_your\_garage\_for\_audiences

Refer to [Amazon your garage](guides/amazon-marketing-cloud/datasources/amazon_your_garage_paid).

## Amazon Brand Store insights (BSI)

**Availability:**  AMC accounts associated with US/CA/JP/AU/FR/IT/ES/UK/DE marketplaces

**Eligibility:** Advertisers with at least one active brand store

**Contains signals from:** US/CA/JP/AU/FR/IT/ES/UK/DE marketplaces

Amazon Brand Store Insights (BSI) is an AMC paid feature that allows advertisers to tap into page visit, dwell time, and interaction signals from Amazon Brand Stores. 

* amazon\_brand\_store\_page\_views
* amazon\_brand\_store\_engagement\_events

For eligible advertisers and instances, the associated audience table can be used for AMC audience creation without  a Paid feature subscription:

* amazon\_brand\_store\_page\_views(\_non\_endemic)\_for\_audiences
* amazon\_brand\_store\_engagement\_events(\_non\_endemic)\_for\_audiences

Refer to [Amazon brand store insights](guides/amazon-marketing-cloud/datasources/brand_store_insights_paid).

## Amazon retail purchases (ARP)

**Availability:** AMC accounts/instances associated with US/CA/JP/AU/UK/FR/DE/ES/IT marketplaces

**Eligibility:**  Must sell their products in the Amazon Store and have associated their selling partner accounts (Vendor Central or Seller Central) with one or more of their Amazon Ads account that are associated with your AMC instance configuration.

**Contains signals from:** US/CA/JP/AU/UK/FR/DE/ES/IT regions for measurement and US/CA/JP/AU for audiences (EU and UK signals are excluded from the audiences table)

*  For Vendor Central accounts, the scope of data will match what is shown in Vendor Central "Reports > Retail Analytics > Sales > Distributor View = Manufacturing". 
*  For Seller Central accounts, the data scope should match what appears in Seller Central "Reports > Business Reports > By ASIN Detail" report.

Amazon Retail Purchases is a paid feature dataset available in AMC that contains up to five years (60 months) of historical Amazon store purchase data.

* amazon\_retail\_purchases

If the instance contains signals for US/CA/JP/AU, the associated audience table can be used for AMC audience creation without  a Paid feature subscription:

* amazon\_retail\_purchases\_for\_audiences

Refer to [Amazon retail purchases](guides/amazon-marketing-cloud/datasources/amazon_retail_purchase_paid).

## Amazon Prime Video Channel insights (PVCI)

**Availability:** AMC accounts/instances associated with the US/CA/JP/AU/FR/IT/ES/UK/DE marketplaces

**Eligibility:**  Only Amazon advertisers that operate Prime Video Channels

**Contains signals from:** US/CA/JP/AU/FR/IT/ES/UK/DE marketplaces

Amazon Prime Video Channel Insights is a collection of two AMC data views that represent Prime Video Channel subscriptions and streaming signals.

* amazon\_pvc\_enrollments
* amazon\_pvc\_streaming\_events\_feed

For eligible advertisers and instances, the associated audience table can be used for AMC audience creation without  a Paid feature subscription:

* amazon\_pvc\_enrollments\_for\_audiences
* amazon\_pvc\_streaming\_events\_feed\_for\_audiences

Refer to [Amazon Prime Video Channel insights](guides/amazon-marketing-cloud/datasources/prime_video_channel_insights_paid).


## NCS CPG Insights Stream

**Availability:** AMC accounts/instances associated with the US AMC account marketplace

**Eligibility:** Any advertiser

**Contains signals from:** US marketplace

The NCS CPG Insights Stream data source provides modeled offline transaction signals for all US households.

* ncs\_cpg\_insights\_stream

There is no associated audience table.

Refer to [NCS CPG Insights Stream](guides/amazon-marketing-cloud/datasources/cpg_insights_stream_paid).

## Experian vehicle purchases insights

**Availability:**  AMC accounts associated with the US marketplace
**Eligibility:** Any advertiser
**Contains signals from:** US marketplace

Experian Vehicle Purchase Insights allows you to measure the impact of their advertising on actual vehicle purchases, better understand path to purchase, and learn about how they can reach new audiences.

* experian\_vehicle\_purchases

There is no associated audience table.

Refer to [Experian Vehicle Purchase Insights](guides/amazon-marketing-cloud/datasources/experian_paid).