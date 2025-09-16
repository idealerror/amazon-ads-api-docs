---
title: Amazon Prime Video Channel Insights
description: Amazon Prime Video Channel Insights
type: guide
interface: api
---

# Amazon Prime Video Channel Insights 

**Analytics tables:** 

* `amazon_pvc_enrollments`
* `amazon_pvc_streaming_events_feed`

**Audience tables:** 

* `amazon_pvc_enrollments_for_audiences`
* `amazon_pvc_streaming_events_feed_for_audiences`

Amazon Prime Video Channel Insights is a collection of two AMC data views that represent Prime Video Channel subscriptions and streaming signals. Amazon Prime Video Channel Insights is a standalone AMC Paid Features resource that is available for trial and subscription enrollments within the AMC Paid Features suite of insight expansion options, powered by Amazon Ads. This resource is available to Amazon Advertisers that operate Prime Video Channels within the supported AMC Paid Features account marketplaces (US/CA/JP/AU/FR/IT/ES/UK/DE).

## amazon\_pvc\_enrollments

Amazon Prime Video Channels enrollment records presented per benefit id. The `pv_subscription_id` column can be converted to metrics via COUNT() or COUNT(distinct) SQL aggregate functions.

| Name | Data Type | Metric/Dimension | Description | Aggregation Threshold |
|------|-----------|------------------|-------------|---------------------|
| marketplace\_id | LONG | Dimension | ID of the marketplace where the Amazon Prime Video Channel enrollment event occurred\. | INTERNAL |
| marketplace\_name | STRING | Dimension | The marketplace associated with the Amazon Prime Video Channel record | LOW |
| no\_3p\_trackers | BOOLEAN | Dimension | Boolean value indicating whether the event can be used for audience creation that is third-party tracking enabled (i.e. whether you can serve creative with third-party tracking when running media against the audience). Third-party tracking refers to measurement tags and pixels from external vendors that can be used to measure ad performance. Possible values for this field are: 'true', 'false'. | LOW |
| pv\_benefit\_id | STRING | Dimension | Amazon Prime Video subscription benefit identifier | INTERNAL |
| pv\_benefit\_name | STRING | Dimension | Amazon Prime Video subscription benefit name | LOW |
| pv\_billing\_type | STRING | Dimension | Amazon Prime Video billing type for the enrollment record | MEDIUM |
| pv\_end\_date | DATE | Dimension | Interval\-specific end date associated with the PVC enrollment record | LOW |
| pv\_enrollment\_status | STRING | Dimension | Status for the PVC enrollment record | LOW |
| pv\_is\_latest\_record | BOOLEAN | Dimension | Indicator of whether the PVC enrollment record is the most recent record within the table | INTERNAL |
| pv\_is\_plan\_conversion | BOOLEAN | Dimension | Indicates whether the PVC enrollment record has converted from Free Trial to a subscription\. This is often associated with a change in the billing type for the PVC subscription | LOW |
| pv\_is\_plan\_start | BOOLEAN | Dimension | Indicates whether the PVC enrollment record is the start of a enrollment\. This is often the opening of a free trial or the first subscription record for the PV subscription ID | LOW |
| pv\_is\_promo | BOOLEAN | Dimension | Indicator of whether the PVC enrollment record is associated with a promotional offer | LOW |
| pv\_offer\_name | STRING | Dimension | Amazon Prime Video subscription offer name | HIGH |
| pv\_start\_date | DATE | Dimension | Interval\-specific start date associated with the PVC enrollment record | LOW |
| pv\_sub\_event\_primary\_key | STRING | Dimension | Unique identifier for the PVC enrollment event | INTERNAL |
| pv\_subscription\_id | STRING | Dimension | Amazon Prime Video subscription id | INTERNAL |
| pv\_subscription\_name | STRING | Dimension | Amazon Prime Video subscription name | LOW |
| pv\_subscription\_product\_id | STRING | Dimension | Amazon Prime Video subscription product identifier | INTERNAL |
| pv\_unit\_price | DECIMAL | Dimension | Unit price for the PVC enrollment record | NONE |
| user\_id | STRING | Dimension | Pseudonymous identifier that connects user activity across different events. The field can contain ad user IDs (representing Amazon accounts), device IDs, or browser IDs. NULL values appear when Amazon Ads is unable to resolve an identifier for an event. The field has a VERY_HIGH aggregation threshold, meaning it cannot be included in final SELECT statements but can be used within Common Table Expressions (CTEs) for aggregation purposes like COUNT(DISTINCT user_id). | VERY\_HIGH |
| user\_id\_type | STRING | Dimension | Type of user ID value. AMC includes different types of user ID values, depending on whether the value represents a resolved Amazon account, a device, or a browser cookie. If Amazon Ads is unable to determine or provide an ID of any kind for an impression event, the `user_id` and `user_id_type` values for that record will be NULL. Possible values include: 'adUserId', 'sisDeviceId', 'adBrowserId', and NULL. | LOW |



## amazon\_pvc\_streaming\_events\_feed

Amazon Prime Video Channel Streaming and engagement events. Provides Amazon Prime Video Channel engagement metrics including content metadata, request context and duration. Events are session based and presented at the PVC benefit-level. The `pv_session_id` columns can be converted to metrics via COUNT() or COUNT(distinct) SQL aggregate functions.

| Name | Data Type | Metric/Dimension | Description | Aggregation Threshold |
|------|-----------|------------------|-------------|---------------------|
| marketplace\_id | LONG | Dimension | Marketplace ID of the event | INTERNAL |
| marketplace\_name | STRING | Dimension | Marketplace name of the event | LOW |
| no\_3p\_trackers | BOOLEAN | Dimension | Boolean value indicating whether the event can be used for audience creation that is third-party tracking enabled (i.e. whether you can serve creative with third-party tracking when running media against the audience). Third-party tracking refers to measurement tags and pixels from external vendors that can be used to measure ad performance. Possible values for this field are: 'true', 'false'. | LOW |
| pv\_access\_type | STRING | Dimension | Content access type \(e\.g\., free, prime subscription, rental, purchase\) | LOW |
| pv\_gti\_cast | ARRAY | Dimension | Content cast members | LOW |
| pv\_gti\_content\_entity\_type | STRING | Dimension | Content entity type \(e\.g\., Short Film, Educational\) | LOW |
| pv\_gti\_content\_rating | STRING | Dimension | Content rating | LOW |
| pv\_gti\_content\_type | STRING | Dimension | Content type \(e\.g\., TV Episode, Movie, Promotion\) | LOW |
| pv\_gti\_director | ARRAY | Dimension | Content directors | LOW |
| pv\_gti\_episode | STRING | Dimension | Series episode | LOW |
| pv\_gti\_event\_context | STRING | Dimension | Content event context | LOW |
| pv\_gti\_event\_item | STRING | Dimension | Content event item | LOW |
| pv\_gti\_event\_league | STRING | Dimension | Content event league | LOW |
| pv\_gti\_event\_name | STRING | Dimension | Content event name | LOW |
| pv\_gti\_event\_sport | STRING | Dimension | Content event sport | LOW |
| pv\_gti\_genre | ARRAY | Dimension | Content genre | LOW |
| pv\_gti\_is\_live | BOOLEAN | Dimension | Indicates if content is live | LOW |
| pv\_gti\_release\_date | DATE | Dimension | Content release date | LOW |
| pv\_gti\_season | STRING | Dimension | Series season | LOW |
| pv\_gti\_series\_or\_movie\_name | STRING | Dimension | Series or movie name | LOW |
| pv\_gti\_studio | STRING | Dimension | Content studio | LOW |
| pv\_gti\_title\_name | STRING | Dimension | Content title \(e\.g\., episode name\) | LOW |
| pv\_is\_avod | BOOLEAN | Dimension | Indicates if content is advertiser\-supported VOD | LOW |
| pv\_is\_channels | BOOLEAN | Dimension | Indicates if content is a channel | LOW |
| pv\_is\_hh\_share | BOOLEAN | Dimension | Indicates if content is shared within household | LOW |
| pv\_is\_svod | BOOLEAN | Dimension | Indicates if content is subscription VOD | LOW |
| pv\_is\_user\_initiated | BOOLEAN | Dimension | Indicates if content playback was user\-initiated | LOW |
| pv\_material\_type | STRING | Dimension | Video material type \(e\.g\., full, live, promo, trailer\) | LOW |
| pv\_offer\_group | STRING | Dimension | Content offer group \(e\.g\., free, prime, rental, purchase\) | LOW |
| pv\_playback\_date\_utc | DATE | Dimension | Date of the event in UTC | LOW |
| pv\_playback\_dt\_utc | TIMESTAMP | Dimension | Timestamp of the event in UTC | MEDIUM |
| pv\_seconds\_viewed | DECIMAL | Metric | Seconds of view time associated with streaming event | MEDIUM |
| pv\_session\_id | STRING | Dimension | Unique identifier for streaming session | VERY\_HIGH |
| pv\_stream\_type | STRING | Dimension | Type of stream content \(e\.g\., linear TV, live\-event, VOD\) | LOW |
| pv\_streaming\_asin | STRING | Dimension | Amazon Standard Identification Number \(ASIN\) | LOW |
| pv\_streaming\_geo\_country | STRING | Dimension | Country where event was accessed | LOW |
| pv\_streaming\_gti | STRING | Dimension | Global title information | LOW |
| pv\_streaming\_language | STRING | Dimension | Language of the event | LOW |
| user\_id | STRING | Dimension | Pseudonymous identifier that connects user activity across different events. The field can contain ad user IDs (representing Amazon accounts), device IDs, or browser IDs. NULL values appear when Amazon Ads is unable to resolve an identifier for an event. The field has a VERY_HIGH aggregation threshold, meaning it cannot be included in final SELECT statements but can be used within Common Table Expressions (CTEs) for aggregation purposes like COUNT(DISTINCT user_id). | VERY\_HIGH |
| user\_id\_type | STRING | Dimension | Type of user ID value. AMC includes different types of user ID values, depending on whether the value represents a resolved Amazon account, a device, or a browser cookie. If Amazon Ads is unable to determine or provide an ID of any kind for an impression event, the `user_id` and `user_id_type` values for that record will be NULL. Possible values include: 'adUserId', 'sisDeviceId', 'adBrowserId', and NULL. | LOW |
