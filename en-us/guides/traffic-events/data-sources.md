---
title: Traffic events API data sources
description: Tables explaining data sources for the traffic events API.
type: guide
interface: api
tags:
    - Reporting
keywords:
    - SQL query fields
    - clean room
---

# Traffic events API data sources

Data from your Amazon Ads campaign events are ingested into TCR. This data could be from ad activity on Amazon DSP, any of the Sponsored ads, or a connected Amazon Ads product like the Amazon Ad Server and can be referenced through the following tables:

- [Amazon DSP traffic](#amazon-dsp-traffic-tables)
- [Amazon Ad Server](#amazon-ad-server-tables)
- [Sponsored ads](#sponsored-ads-table)
- [Devices traffic](#devices-traffic-table)

All tables have a flat schema consisting of a list of columns that each have a name, a data type, a column type of metric or dimension, and an aggregation threshold. For details, see [TCR data aggregation thresholds](guides/traffic-events/data-aggregation-thresholds).

Creating or modifying materialized tables is not supported in TCR SQL. The only supported operations are queries that operate on defined tables within TCR. Tables in TCR can be queried like SQL tables.

## Data source tables

### Amazon DSP traffic tables

The following tables contain inputs from all Amazon DSP campaigns from the DSP advertising accounts that you have access to. Includes records from ad product types such as Display, Online Video, Streaming TV, Audio, etc. 

- [Amazon DSP impressions](#dspimpressions)
- [Amazon DSP clicks](#dspclicks)
- [Amazon DSP views](#dspviews)

The following tables contain invalid traffic for DSP. 

- [Amazon DSP invalid impressions](#dspinvalidimpressions) 
- [Amazon DSP invalid clicks](#dspinvalidclicks) 

### Amazon Ad Server tables

The following tables contain inputs from Amazon Ad Server data sources.

- [Ad Server traffic](#adservertraffic)

The following tables contain invalid traffic for Ad Server. 

- [Ad Server invalid traffic](#invalidadservertraffic)

### Sponsored ads table

This table includes both impressions and clicks for sponsored ads programs.

- [Sponsored ads traffic table](#sponsoredadstraffic)

The following tables contain invalid traffic for Sponsored Ads. 

- [Sponsored ads invalid traffic table](#sponsoredadsinvalidtraffic)

### Devices traffic table

This table includes both impressions and clicks for devices traffic. 

- [Devices ads traffic table](#devicestraffic)

>[NOTE]Invalid traffic is not available for devices ads.

## Important notes for querying data sets

### Time Zones

TCR queries are based on UTC time by default, but the advertising console provides reports based on advertiser time zone (e.g. Europe/Zurich). When working with traffic events APIs, to match TCR to the advertiser time zone, the `timeWindowTimeZone` parameter of the `CreateWorkflowExecution` request can be used to specify the time zone.

## Devices Traffic

### devices_traffic

The devices_traffic table captures the details of the traffic event that occurs for a particular campaign on Amazon devices. It also includes other associated information about the advertisement.  

|Field Category	|Name	|Data Type	|Metric / Dimension	|Description	|Aggregation Threshold	|
|---	|---	|---	|---	|---	|---	|
|Advertiser setup	|advertiser	|STRING	|Dimension	|Amazon Devices traffic advertiser name.	|LOW	|
|Advertiser setup	|advertiser_id	|LONG	|Dimension	|Amazon Devices traffic Server advertiser ID.	|LOW	|
|Ad Product Info	|ad\_product\_type	|STRING	|Dimension	|The ad product of the campaign. Example: "sponsored\_products" or "sponsored\_display".	|	|
|Ad Product Info	|ad\_product\_type\_code	|STRING	|Dimension	|The ad product code of the campaign. Example: "SP" or "SB".	|	|
|Campaign setup	|campaign	|STRING	|Dimension	|Amazon Devices traffic Campaign Name, as appears in Amazon Devices (for display and search).	|LOW	|
|Campaign setup	|campaign_id	|LONG	|Dimension	|Amazon Devices traffic Campaign ID (internal Devices traffic IDs for display and search).	|LOW	|
|Click delivery info	|clicks	|LONG	|Metric	|Total clicks served, excluding GIVT. Includes all billable clicks served and tracked by Amazon Ad Server, among them click\_sivt, click\_ssai and click\_noscript.	|NONE	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event_date	|DATE	|Dimension	|Event Date (Amazon Ad Server account time zone).	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event\_date\_utc	|DATE	|Dimension	|Event Date (UTC).	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event_day	|INTEGER	|Dimension	|Event Day of Month (Amazon Devices Traffic account time zone).	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event\_day\_utc	|INTEGER	|Dimension	|Event Day of Month (UTC).	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event_dt	|TIMESTAMP	|Dimension	|Event Timestamp (Amazon Devices Traffic account time zone).	|MEDIUM	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event\_dt\_hour	|TIMESTAMP	|Dimension	|Event Timestamp (Amazon Devices Traffic account time zone) Truncated to Hour.	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event\_dt\_hour_utc	|TIMESTAMP	|Dimension	|Event Timestamp (UTC) Truncated to Hour.	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event\_dt\_utc	|TIMESTAMP	|Dimension	|Event Timestamp (UTC). Example: 2019-06-18T03:45:55.000	|MEDIUM	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event_hour	|INTEGER	|Dimension	|Event Hour of Day (Amazon Devices Traffic account time zone).	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event\_hour\_utc	|INTEGER	|Dimension	|Event Hour of Day (UTC).	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event_source	|STRING	|Dimension	|Event Source.	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event_cost	|LONG	|Dimension	|Event Cost.	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|total_cost	|double	|Dimension	|Total Cost.	|LOW	|
|Impression delivery info	|impressions	|LONG	|Metric	|Total impressions served, excluding GIVT. Includes all billable impressions served and tracked by Amazon Devices Traffic, among them impressions\_sivt, impressions\_ssai and impressions\_noscript.	|NONE	|
|Impression delivery info	|viewable_impressions	|LONG	|Metric	|Number of viewable impression (IAB standard). Note, viewability signals collected only for campaigns where verification data collection mode was enabled. This metric will be 0 if campaign is not enabled for verification data collection.	|NONE	|
|Impression Viewability	|view_definition	|STRING	|Dimension	|For Devices Traffic, view_definition is either NULL or ‘iab’. The value is ‘iab’ when the event was a viewable impression. This field will not contain 'iab' for campaigns that were view aware. Instead, it is only 'iab' when the actual event was a viewable impression. 	|	|
|Line item setup	|line_item	|STRING	|Dimension	|Name of ad group. The value for line\_item matches ad\_group.	|	|
|Line item setup	|line\_item\_id	|LONG	|Dimension	|Customer-facing line item ID. For Sponsored Products, the value for line\_item\_id matches ad_group_id. For Sponsored Display, this value will be NULL. Use ad\_group\_id.	|	|

## Amazon DSP

### dsp_clicks

The dsp\_clicks table is a subset of the dsp\_impressions information that captures the details impressions that were clicked and the associated click related information.

|Field Category	|Name	|Data Type	|Metric / Dimension	|Description	|Aggregation Threshold	|
|---	|---	|---	|---	|---	|---	|
|Advertiser Setup	|advertiser	|STRING	|Dimension	|Advertiser name. Example: “Example Company”	|LOW	|
|Advertiser Setup	|advertiser_id	|LONG	|Dimension	|Customer-facing ID of advertiser. Example: 8443640090301	|LOW	|
|Campaign Setup	|campaign	|STRING	|Dimension	|Name of campaign (called “order” in DSP console)	|LOW	|
|Campaign Setup	|campaign_id	|LONG	|Dimension	|Customer-facing campaign ID	|LOW	|
|Click Delivery Info	|click_date	|DATE	|Dimension	|MM/DD/YYYY (advertiser time zone)	|LOW	|
|Click Delivery Info	|click\_date\_utc	|DATE	|Dimension	|MM/DD/YYYY (UTC)	|LOW	|
|Click Delivery Info	|click_day	|INTEGER	|Dimension	|01 - 31 (advertiser time zone)	|LOW	|
|Click Delivery Info	|click\_day\_utc	|INTEGER	|Dimension	|01 - 31 (UTC)	|LOW	|
|Click Delivery Info	|click_dt	|TIMESTAMP	|Dimension	|Timestamp of click in advertiser time zone. Example: 2019-06-18T03:45:55.000	|MEDIUM	|
|Click Delivery Info	|click\_dt\_hour	|TIMESTAMP	|Dimension	|Timestamp of click in advertiser time zone truncated to hour. Example: 2019-06-18T03:00:00.000	|LOW	|
|Click Delivery Info	|click\_dt\_hour\_utc	|TIMESTAMP	|Dimension	|Timestamp of click in UTC truncated to hour. Example: 2019-06-18T08:00:00.000Z	|LOW	|
|Click Delivery Info	|click\_dt\_utc	|TIMESTAMP	|Dimension	|Timestamp of click in UTC. Example: 2019-06-18T03:45:55.000	|MEDIUM	|
|Click Delivery Info	|click_hour	|INTEGER	|Dimension	|00 – 23 (advertiser time zone)	|LOW	|
|Click Delivery Info	|click\_hour\_utc	|INTEGER	|Dimension	|00 – 23 (UTC)	|LOW	|
|Click Delivery Info	|clicks	|LONG	|Metric	|Number of clicks	|LOW	|
|Creative Setup	|creative	|STRING	|Dimension	|Name of creative. Example: “Example Creative Name”	|LOW	|
|Creative Setup	|creative_id	|LONG	|Dimension	|Customer-facing creative ID. Example: 4301568770201	|LOW	|
|Creative Setup	|creative_type	|STRING	|Dimension	|Type of creative. Example: “Shazam HTML – AAP”	|LOW	|
|Advertiser Setup	|entity_id	|STRING	|Dimension	|Customer-facing ID of entity – can be found in the URL within the DSP console. Example: “ENTITYA6I16E0BHHHY”	|LOW	|
|Line item setup	|line_item	|STRING	|Dimension	|Name of line item (ad)	|LOW	|
|Line item setup	|line\_item\_id	|LONG	|Dimension	|Customer-facing line item ID	|LOW	|
|Supply	|supply_source	|STRING	|Dimension	|Name of supply source (exchange). Example: “Amazon Publisher Services”	|LOW	|

### dsp\_invalid\_clicks

The dsp\_invalid\_clicks table records clicks for the impressions that have been marked invalid. This includes potentially fraudulent, non-human, and other illegitimate traffic.

|Field Category	|Name	|Data Type	|Metric / Dimension	|Description	|Aggregation Threshold	|
|---	|---	|---	|---	|---	|---	|
|Advertiser Setup	|advertiser	|STRING	|Dimension	|Advertiser name. Example: “Example Company”	|MEDIUM	|
|Advertiser Setup	|advertiser_id	|LONG	|Dimension	|Customer-facing ID of advertiser. Example: 8443640090301	|MEDIUM	|
|Campaign Setup	|campaign	|STRING	|Dimension	|Name of campaign (called “order” in DSP console)	|MEDIUM	|
|Campaign Setup	|campaign_id	|LONG	|Dimension	|Customer-facing campaign ID	|MEDIUM	|
|Click Delivery Info	|click_date	|DATE	|Dimension	|MM/DD/YYYY (advertiser time zone)	|MEDIUM	|
|Click Delivery Info	|click\_date\_utc	|DATE	|Dimension	|MM/DD/YYYY (UTC)	|MEDIUM	|
|Click Delivery Info	|invalid_clicks	|LONG	|Metric	|Number of clicks that were removed by the traffic quality filter. This includes potentially fraudulent, non-human, and other illegitimate traffic.	|MEDIUM	|
|Advertiser Setup	|entity_id	|STRING	|Dimension	|Customer-facing ID of entity – can be found in the URL within the DSP console. Example: “ENTITYA6I16E0BHHHY”	|MEDIUM	|
|Line item setup	|line_item	|STRING	|Dimension	|Name of line item (ad)	|MEDIUM	|
|Line item setup	|line\_item\_id	|LONG	|Dimension	|Customer-facing line item ID	|MEDIUM	|

### dsp_impressions

The dsp_impressions table records of all impressions delivered to viewers reached through an Amazon DSP campaign.

|Field Category	|Name	|Data Type	|Metric / Dimension	|Description	|Aggregation Threshold	|
|---	|---	|---	|---	|---	|---	|
|Advertiser Setup	|advertiser	|STRING	|Dimension	|Advertiser name. Example: “Example Company”	|LOW	|
|Advertiser Setup	|advertiser_id	|LONG	|Dimension	|Customer-facing ID of advertiser. Example: 8443640090301	|LOW	|
|Campaign Setup	|campaign	|STRING	|Dimension	|Name of campaign (called “order” in DSP console)	|LOW	|
|Campaign Setup	|campaign_id	|LONG	|Dimension	|Customer-facing campaign ID	|LOW	|
|Creative Setup	|creative	|STRING	|Dimension	|Name of creative. Example: “Example Creative Name”	|LOW	|
|Creative Setup	|creative_id	|LONG	|Dimension	|Customer-facing creative ID. Example: 4301568770201	|LOW	|
|Creative Setup	|creative_type	|STRING	|Dimension	|Type of creative. Example: “Shazam HTML – AAP”	|LOW	|
|Advertiser Setup	|entity_id	|STRING	|Dimension	|Customer-facing ID of entity – can be found in the URL within the DSP console. Example: “ENTITYA6I16E0BHHHY”	|LOW	|
|Impression Delivery Info	|impression_cost	|LONG	|Metric	|Impression cost in millicents	|NONE	|
|Impression Delivery Info	|impression_date	|DATE	|Dimension	|MM/DD/YYYY (advertiser time zone)	|LOW	|
|Impression Delivery Info	|impression\_date\_utc	|DATE	|Dimension	|MM/DD/YYYY (UTC)	|LOW	|
|Impression Delivery Info	|impression_day	|INTEGER	|Dimension	|01 - 31 (advertiser time zone)	|LOW	|
|Impression Delivery Info	|impression\_day\_utc	|INTEGER	|Dimension	|01 - 31 (UTC)	|LOW	|
|Impression Delivery Info	|impression_dt	|TIMESTAMP	|Dimension	|Timestamp of impression in advertiser time zone. Example: 2019-06-18T03:45:55.000	|MEDIUM	|
|Impression Delivery Info	|impression\_dt\_hour	|TIMESTAMP	|Dimension	|Timestamp of impression in advertiser time zone truncated to hour. Example: 2019-06-18T03:00:00.000	|LOW	|
|Impression Delivery Info	|impression\_dt\_hour\_utc	|TIMESTAMP	|Dimension	|Timestamp of impression in UTC truncated to hour. Example: 2019-06-18T08:00:00.000Z	|LOW	|
|Impression Delivery Info	|impression\_dt\_utc	|TIMESTAMP	|Dimension	|Timestamp of impression in UTC. Example: 2019-06-18T03:45:55.000	|MEDIUM	|
|Impression Delivery Info	|impression_hour	|INTEGER	|Dimension	|00 – 23 (advertiser time zone)	|LOW	|
|Impression Delivery Info	|impression\_hour\_utc	|INTEGER	|Dimension	|00 – 23 (UTC)	|LOW	|
|Impression Delivery Info	|impressions	|LONG	|Metric	|Count of each impression. For each impression event, this value will be 1, but the value can be summed, averaged, and so on across various dimensions	|NONE	|
|Line item setup	|line_item	|STRING	|Dimension	|Name of line item (ad)	|LOW	|
|Line item setup	|line\_item\_id	|LONG	|Dimension	|Customer-facing line item ID	|LOW	|
|Supply	|supply_source	|STRING	|Dimension	|Name of supply source (exchange). Example: “Amazon Publisher Services”	|LOW	|
|Costs	|total_cost	|LONG	|Metric	|Impression price plus computed agency fee in milli cents	|LOW	|

### dsp\_invalid\_impressions

The dsp\_invalid\_impressions table records of all invalid impressions delivered to viewers. This includes potentially fraudulent, non-human, and other illegitimate traffic.

|Field Category	|Name	|Data Type	|Metric / Dimension	|Description	|Aggregation Threshold	|
|---	|---	|---	|---	|---	|---	|
|Advertiser Setup	|advertiser	|STRING	|Dimension	|Advertiser name. Example: “Example Company”	|MEDIUM	|
|Advertiser Setup	|advertiser_id	|LONG	|Dimension	|Customer-facing ID of advertiser. Example: 8443640090301	|MEDIUM	|
|Campaign Setup	|campaign	|STRING	|Dimension	|Name of campaign (called “order” in DSP console)	|MEDIUM	|
|Campaign Setup	|campaign_id	|LONG	|Dimension	|Customer-facing campaign ID	|MEDIUM	|
|Advertiser Setup	|entity_id	|STRING	|Dimension	|Customer-facing ID of entity – can be found in the URL within the DSP console. Example: “ENTITYA6I16E0BHHHY”	|MEDIUM	|
|Impression Delivery Info	|impression_date	|DATE	|Dimension	|MM/DD/YYYY (advertiser time zone)	|MEDIUM	|
|Impression Delivery Info	|impression\_date\_utc	|DATE	|Dimension	|MM/DD/YYYY (UTC)	|MEDIUM	|
|Impression Delivery Info	|invalid_impressions	|LONG	|Metric	|The number of impressions removed by a traffic quality filter. This includes potentially fraudulent, non-human, and other illegitimate traffic	|MEDIUM	|
|Line item setup	|line_item	|STRING	|Dimension	|Name of line item (ad)	|MEDIUM	|
|Line item setup	|line\_item\_id	|LONG	|Dimension	|Customer-facing line item ID	|MEDIUM	|

### dsp\_views

The dsp\_views table shows all view events and all measurable events. Measurable events are from impression that were able to be measured. Viewable events represent events that met a viewable standard. The metric `viewable_impressions` is the count of viewable impressions using the IAB standard (50% of the impression's pixels were on screen for at least 1 second). Note the `dsp_views` table records a single impression more than one time if it is both measurable and viewable according to IAB's standards. An impression that is both measurable and viewable according to IAB's standards is recorded in one row as a measurable event and in another row as a viewable event. Note that while all impression events from `dsp_views` are recorded in `dsp_impressions`, not all impression events from `dsp_impressions` are recorded in `dsp_views` because not all impression events can be categorized as viewable, measurable or unmeasurable.

|Field Category	|Name	|Data Type	|Metric / Dimension	|Description	|Aggregation Threshold	|
|---	|---	|---	|---	|---	|---	|
|Advertiser Setup	|advertiser	|STRING	|Dimension	|Advertiser name. Example: “Example Company”	|LOW	|
|Advertiser Setup	|advertiser\_id	|LONG	|Dimension	|Customer-facing ID of advertiser. Example: 8443640090301	|LOW	|
|Campaign Setup	|campaign	|STRING	|Dimension	|Name of campaign (called “order” in DSP console)	|LOW	|
|Campaign Setup	|campaign\_id	|LONG	|Dimension	|Customer-facing campaign ID	|LOW	|
|Creative Setup	|creative	|STRING	|Dimension	|Name of creative. Example: “Example Creative Name”	|LOW	|
|Creative Setup	|creative\_id	|LONG	|Dimension	|Customer-facing creative ID. Example: 4301568770201	|LOW	|
|Creative Setup	|creative\_type	|STRING	|Dimension	|Type of creative. Example: “Shazam HTML – AAP”	|LOW	|
|Advertiser Setup	|entity\_id	|STRING	|Dimension	|Customer-facing ID of entity – can be found in the URL within the DSP console. Example: “ENTITYA6I16E0BHHHY”	|LOW	|
|Event Delivery Info	|event\_date	|DATE	|Dimension	|Event Date (Advertiser Timezone)	|MEDIUM	|
|Event Delivery Info	|event\_date\_utc	|DATE	|Dimension	|Event Date (UTC)	|MEDIUM	|
|Event Delivery Info	|event_dt	|TIMESTAMP	|Dimension	|Event Timestamp (Advertiser Timezone)	|MEDIUM	|
|Event Delivery Info	|event\_dt\_hour	|TIMESTAMP	|Dimension	|Event Timestamp (Advertiser Timezone) Truncated to Hour	|LOW	|
|Event Delivery Info	|event\_dt\_hour\_utc	|TIMESTAMP	|Dimension	|Event Timestamp (UTC) Truncated to Hour	|LOW	|
|Event Delivery Info	|event\_dt\_utc	|TIMESTAMP	|Dimension	|Event Timestamp (UTC). Example: 2019-06- 18T03:45:55.000	|MEDIUM	|
|Event Delivery Info	|event_type	|STRING	|Dimension	|Event Type	|LOW	|
|Event Delivery Info	|events	|LONG	|Metric	|Event Count	|NONE	|
|Line item setup	|line_item	|STRING	|Dimension	|Name of line item (ad)	|LOW	|
|Line item setup	|line\_item\_id	|LONG	|Dimension	|Customer-facing line item ID	|LOW	|
|Impression Viewability	|measurable_impressions	|LONG	|Metric	|Measurable impression count. Note that a single impression that is measurable could also be viewable, but not all measurable impressions are viewable. If an impression is both measurable and viewable, each event for the single impression is listed as a separate row.	|LOW	|
|Impression Viewability	|unmeasurable\_viewable\_impressions	|LONG	|METRIC	|Unmeasurable viewable impression count. These represent impressions at a normally view-aware supply source / placement that could not be measured due to special circumstances (such as using an uncommon browser) but were estimated to be viewable based on the usual view rate of the supply source / placement. Note that impressions which are served to a non-view-aware supply source / placement will NOT show up in this data source - use the display\_impressions data source to query these impressions.	|LOW	|
|Impression Viewability	|view_definition	|LONG	|METRIC	|View Type (For Viewable Impressions)	|LOW	|
|Impression Viewability	|viewable_impressions	|LONG	|Metric	|Viewable impression count (IAB standard). Note that a single impression that is viewable has two events: a viewable impression and a measurable impression. Each event for the single impression is listed as a separate row.	|LOW	|

## Ad Server Traffic

### adserver_traffic

The data source `adserver_traffic` includes impressions, clicks, and interactions (video and view-ability) measured by the Amazon Ad Server. You can query this data source only if you have ad server campaigns.

|Field Category	|Name	|Data Type	|Metric / Dimension	|Description	|Aggregation Threshold	|
|---	|---	|---	|---	|---	|---	|
|Ad setup	|ad_format	|STRING	|Dimension	|Ad Format (in-banner, in-stream video, rich media, etc.).	|LOW	|
|Ad setup	|ad_id	|LONG	|Dimension	|Ad ID, ad that is attached to a placement.	|LOW	|
|Advertiser setup	|ad	|STRING	|Dimension	|Amazon Ad Server brand name. A single Amazon Ad Server advertiser can have multiple brands. One campaign can be associated to a single brand.	|LOW	|
|Advertiser setup	|advertiser	|STRING	|Dimension	|Amazon Ad Server advertiser name.	|LOW	|
|Advertiser setup	|advertiser_id	|LONG	|Dimension	|Amazon Ad Server advertiser ID.	|LOW	|
|Campaign setup	|campaign	|STRING	|Dimension	|Amazon Ad Server Campaign Name, as appears in Ad Server (for display and search).	|LOW	|
|Campaign setup	|campaign_id	|LONG	|Dimension	|Amazon Ad Server Campaign ID (internal Amazon Ad Server IDs for display and search).	|LOW	|
|Click delivery info	|clicks	|LONG	|Metric	|Total clicks served, excluding GIVT. Includes all billable clicks served and tracked by Amazon Ad Server, among them click\_sivt, click\_ssai and click\_noscript.	|NONE	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event_date	|DATE	|Dimension	|Event Date (Amazon Ad Server account time zone).	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event\_date\_utc	|DATE	|Dimension	|Event Date (UTC).	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event_day	|INTEGER	|Dimension	|Event Day of Month (Amazon Ad Server account time zone).	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event\_day\_utc	|INTEGER	|Dimension	|Event Day of Month (UTC).	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event_dt	|TIMESTAMP	|Dimension	|Event Timestamp (Amazon Ad Server account time zone).	|MEDIUM	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event\_dt\_hour	|TIMESTAMP	|Dimension	|Event Timestamp (Amazon Ad Server account time zone) Truncated to Hour.	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event\_dt\_hour\_utc	|TIMESTAMP	|Dimension	|Event Timestamp (UTC) Truncated to Hour.	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event\_dt\_utc	|TIMESTAMP	|Dimension	|Event Timestamp (UTC). Example: 2019-06-18T03:45:55.000	|MEDIUM	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event_hour	|INTEGER	|Dimension	|Event Hour of Day (Amazon Ad Server account time zone).	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event\_hour\_utc	|INTEGER	|Dimension	|Event Hour of Day (UTC).	|LOW	|
|Impression delivery info	|impressions	|LONG	|Metric	|Total impressions served, excluding GIVT. Includes all billable impressions served and tracked by Amazon Ad Server, among them impressions\_sivt, impressions\_ssai and impressions\_noscript.	|NONE	|
|Impression delivery info	|measurable_impressions	|LONG	|Metric	|Number of impressions served in campaigns that were enabled for viewability measurement. Note that only campaigns with verification data collection mode are measured for viewability. This metric will be 0 if campaign is not enabled for verification data collection.	|NONE	|
|Impression delivery info	|recordable\_impressions	|LONG	|Metric	|Number of impressions (out of measurable\_impressions) that were successfully recorded for viewability evaluation. For example, viewability signal can't be collected for impression served in unfriendly iframe and so such impression won't be recorded for viewability. Note that this metric will be 0 if campaign is not enabled for verification data collection.	|NONE	|
|Interaction info	|video\_percentage\_complete	|LONG	|Metric	|Average % of the video viewed. e.g. on avg video viewed to 47% of its completion.	|NONE	|
|Interaction info	|video\_played\_100pc	|LONG	|Metric	|Number of times when video played to its full completion.	|NONE	|
|Interaction info	|video\_played\_100pc\_viewable	|LONG	|Metric	|Number of times when video played to its full completion and impression was viewable.	|NONE	|
|Interaction info	|video\_played\_25pc	|LONG	|Metric	|Number of times when video played only to 25% of its completion.	|NONE	|
|Interaction info	|video\_played\_25pc\_viewable	|LONG	|Metric	|Number of times when video played only to 25% of its completion and impression was viewable.	|NONE	|
|Interaction info	|video\_played\_50pc	|LONG	|Metric	|Number of times when video played only to 50% of its completion.	|NONE	|
|Interaction info	|video\_played\_50pc\_viewable	|LONG	|Metric	|Number of times when video played only to 50% of its completion and impression was viewable.	|NONE	|
|Interaction info	|video\_played\_75pc	|LONG	|Metric	|Number of times when video played only to 75% of its completion.	|NONE	|
|Interaction info	|video\_played\_75pc\_viewable	|LONG	|Metric	|Number of times when video played only to 75% of its completion and impression was viewable.	|NONE	|
|Impression delivery info	|viewable_impressions	|LONG	|Metric	|Number of viewable impression (IAB standard). Note, viewability signals collected only for campaigns where verification data collection mode was enabled. This metric will be 0 if campaign is not enabled for verification data collection.	|	|

### invalid\_adserver\_traffic

|Field Category	|Name	|Data Type	|Metric / Dimension	|Description	|Aggregation Threshold	|
|---	|---	|---	|---	|---	|---	|
|Ad setup	|ad_format	|STRING	|Dimension	|Ad Format (in-banner, in-stream video, rich media, etc.).	|LOW	|
|Ad setup	|ad_id	|LONG	|Dimension	|Ad ID, ad that is attached to a placement.	|LOW	|
|Advertiser setup	|ad	|STRING	|Dimension	|Amazon Ad Server brand name. A single Amazon Ad Server advertiser can have multiple brands. One campaign can be associated to a single brand.	|LOW	|
|Advertiser setup	|advertiser	|STRING	|Dimension	|Amazon Ad Server advertiser name.	|LOW	|
|Advertiser setup	|advertiser_id	|LONG	|Dimension	|Amazon Ad Server advertiser ID.	|LOW	|
|Campaign setup	|campaign	|STRING	|Dimension	|Amazon Ad Server Campaign Name, as appears in Ad Server (for display and search).	|LOW	|
|Campaign setup	|campaign_id	|LONG	|Dimension	|Amazon Ad Server Campaign ID (internal Amazon Ad Server IDs for display and search).	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event_date	|DATE	|Dimension	|Event Date (Amazon Ad Server account time zone).	|LOW	|
|Event (impression, Click, Interaction, Conversion) Delivery Info	|event\_date\_utc	|DATE	|Dimension	|Event Date (UTC).	|LOW	|
|Impression delivery info	|invalid\_clicks	|LONG	|Metric	|Total clicks served, excluding GIVT. Includes all billable clicks served and tracked by Amazon Ad Server, among them clicks\_sivt, clicks\_ssai and clicks\_noscript.	|NONE	|
|Impression delivery info	|invalid\_impressions	|LONG	|Metric	|Total impressions served, excluding GIVT. Includes all billable impressions served and tracked by Amazon Ad Server, among them impressions\_sivt, impressions\_ssai and impressions\_noscript.	|NONE	|
|Impression delivery info	|invalid\_measurable\_impressions	|LONG	|Metric	|Number of impressions served in campaigns that were enabled for viewability measurement. Note that only campaigns with verification data collection mode are measured for viewability. This metric will be 0 if campaign is not enabled for verification data collection.	|NONE	|
|Impression delivery info	|invalid\_recordable\_impressions	|LONG	|Metric	|Number of impressions (out of measurable\_impressions) that were successfully recorded for viewability evaluation. For example, viewability signal can't be collected for impression served in unfriendly iframe and so such impression won't be recorded for viewability. Note that this metric will be 0 if campaign is not enabled for verification data collection.	|NONE	|
|Interaction info	|invalid\_video\_percentage\_complete	|LONG	|Metric	|Average % of the video viewed. e.g. on avg video viewed to 47% of its completion.	|NONE	|
|Interaction info	|invalid\_video\_played\_100pc	|LONG	|Metric	|Number of times when video played to its full completion.	|NONE	|
|Interaction info	|invalid\_video\_played\_100pc\_viewable	|LONG	|Metric	|Number of times when video played to its full completion and impression was viewable.	|NONE	|
|Interaction info	|invalid\_video\_played\_25pc	|LONG	|Metric	|Number of times when video played only to 25% of its completion.	|NONE	|
|Interaction info	|invalid\_video\_played\_25pc\_viewable	|LONG	|Metric	|Number of times when video played only to 25% of its completion and impression was viewable.	|NONE	|
|Interaction info	|invalid\_video\_played\_50pc	|LONG	|Metric	|Number of times when video played only to 50% of its completion.	|NONE	|
|Interaction info	|invalid\_video\_played\_50pc\_viewable	|LONG	|Metric	|Number of times when video played only to 50% of its completion and impression was viewable.	|NONE	|
|Interaction info	|invalid\_video\_played\_75pc	|LONG	|Metric	|Number of times when video played only to 75% of its completion.	|NONE	|
|Interaction info	|invalid\_video\_played\_75pc\_viewable	|LONG	|Metric	|Number of times when video played only to 75% of its completion and impression was viewable.	|NONE	|
|Impression delivery info	|invalid\_viewable\_impressions	|LONG	|Metric	|Number of viewable impression (IAB standard). Note, viewability signals collected only for campaigns where verification data collection mode was enabled. This metric will be 0 if campaign is not enabled for verification data collection.	|NO	|

##  Sponsored Ads

### sponsored\_ads\_traffic

|Field Category	|Name	|Data Type	|Metric / Dimension	|Description	|
|---	|---	|---	|---	|---	|
|Ad Group Setup	|ad_group	|STRING	|Dimension	|Name of ad group. The value for line\_item matches ad\_group.	|
|Ad Product Info	|ad\_product\_type	|STRING	|Dimension	|The ad product of the campaign. Example: "sponsored\_products" or "sponsored\_display".	|
|Ad Product Info	|ad\_product\_type\_code	|STRING	|Dimension	|The ad product code of the campaign. Example: "SP" or "SB".	|
|Advertiser Setup	|advertiser	|STRING	|Dimension	|Advertiser name. Example: “Example Company”	|
|Advertiser Setup	|advertiser\_id\_internal	|LONG	|Dimension	|Internal Advertiser Id. Example: 12345	|
|Campaign Setup	|campaign	|STRING	|Dimension	|Campaign Name. Example: “My Great Campaign”	|
|Click Delivery Info	|clicks	|LONG	|Metric	|Count of each click. For each click event, this value will be 1, but the value can be summed, averaged, and so on across various dimensions	|
|Creative Setup	|creative	|STRING	|Dimension	|Creative Name	|
|Creative Setup	|creative_type	|STRING	|Dimension	|Type of creative. Example: “Shazam HTML – AAP”	|
|Advertiser Setup	|entity_id	|STRING	|Dimension	|Customer-facing ID of entity – can be found in the URL within the advertising console. Example: “ENTITYA6I16E0BHHHY”	|
|Event (impression or Click) Delivery Info	|event_date	|DATE	|Dimension	|Event Date (Advertiser Timezone)	|
|Event (impression or Click) Delivery Info	|event\_date\_utc	|DATE	|Dimension	|Event Date (UTC)	|
|Event (impression or Click) Delivery Info	|event_day	|INTEGER	|Dimension	|Event Day of Month (Advertiser Timezone)	|
|Event (impression or Click) Delivery Info	|event\_day\_utc	|INTEGER	|Dimension	|Event Day of Month (UTC)	|
|Event (impression or Click) Delivery Info	|event_dt	|TIMESTAMP	|Dimension	|Event Timestamp (Advertiser Timezone)	|
|Event (impression or Click) Delivery Info	|event\_dt\_hour	|TIMESTAMP	|Dimension	|Event Timestamp (Advertiser Timezone) Truncated to Hour	|
|Event (impression or Click) Delivery Info	|event\_dt\_hour_utc	|TIMESTAMP	|Dimension	|Event Timestamp (UTC) Truncated to Hour	|
|Event (impression or Click) Delivery Info	|event\_dt\_utc	|TIMESTAMP	|Dimension	|Event Timestamp (UTC). Example: 2019-06- 18T03:45:55.000	|
|Event (impression or Click) Delivery Info	|event_hour	|INTEGER	|Dimension	|Event Hour of Day (Advertiser Timezone)	|
|Event (impression or Click) Delivery Info	|event\_hour\_utc	|INTEGER	|Dimension	|Event Hour of Day (UTC)	|
|Event (impression or Click) Delivery Info	|event_source	|STRING	|Dimension	|Internal system that generated event.	|
|Impression Delivery Info	|impressions	|LONG	|Metric	|Count of each impression. For each impression event, this value will be 1, but the value can be summed, averaged, and so on across various dimensions	|
|Line item setup	|line\_item	|STRING	|Dimension	|Name of ad group. The value for line\_item matches ad\_group.	|
|Line item setup	|line\_item\_id	|LONG	|Dimension	|Customer-facing line item ID. For Sponsored Products, the value for line\_item\_id matches ad\_group\_id. For Sponsored Display, this value will be NULL. Use ad\_group\_id.	|
|Line item setup	|line\_item\_price\_type	|STRING	|Dimension	|The cost structure is identified using line\_item\_price\_type, which has two values for Sponsored Display campaigns: CPC or VCPM. When the value is VCPM, viewability is measured for Sponsored Display. See also 'viewable\_impressions'. Sponsored Products has a cost structure of CPC only. (CPC is cost-per-click and vCPM is cost per thousand viewable impressions.)	|
|Advertiser Setup	|marketplace_id	|INTEGER	|Dimension	|Marketplace Id	|
|Impression Viewability	|view_definition	|STRING	|Dimension	|For Sponsored Display, view\_definition is either NULL or ‘iab’. The value is ‘iab’ when the event was a viewable impression. This field will not contain 'iab' for campaigns that were view aware. Instead, it is only 'iab' when the actual event was a viewable impression. For Sponsored Products, this value is always NULL. See also 'viewable\_impressions', and 'line\_item\_price\_type' for additional information.	|
|Impression Viewability	|unmeasurable\_viewable\_impressions	|LONG	|Metric	|Unmeasurable viewable impression count for Sponsored Display only that could not be measured due to special circumstances (such as using an uncommon browser) but were estimated to be viewable based on the usual view rate of the placement. For every unmeasurable viewable impression event, there is a separate viewable impression event. Therefore, unmeasurable\_viewable\_impressions and viewable\_impressions should not be added to avoid double counting viewable impressions. See also 'viewable\_impressions', and 'line\_item\_price\_type' for additional information.	|
|Impression Viewability	|viewable\_impressions	|LONG	|Metric	|The number of impressions shoppers viewed as per the Media Rating Council (MRC) / Interactive Advertising Bureau (IAB) viewability standard. A display ad is viewable if 50% of pixels were viewable for at least 1 second. Viewable impression counts are available when you use optimize for viewable impressions bidding option. When `line_item_price_type` = 'VCPM', the campaign was charged on a cost per thousand viewable impressions basis and views were measured. If an event is a viewable impression, there is also a separate impression event recorded under 'impressions'. See the instructional query "How to query Sponsored Display traffic" to learn how to calculate viewability rate. Viewable impressions only apply to Sponsored Display. See also 'view\_definition' and 'unmeasurable\_viewable\_impressions'.	|
|Impression Info	|five\_sec\_views	|LONG	|Metric	|The number of impressions where the video was viewed for 5 seconds (or 100% if the video length is less than 5 sec).	|
|Impression Delivery Info	|video\_complete\_views	|LONG	|Metric	|The number of impressions where the video was viewed to 100%.	|
|Impression Delivery Info	|video\_first\_quartile\_views	|LONG	|Metric	|The number of impressions where the video was viewed to 25%.	|
|Impression Delivery Info	|video\_midpoint\_views	|LONG	|Metric	|The number of impressions where the video was viewed to 50%.	|
|Impression Delivery Info	|video\_third\_quartile\_views	|LONG	|Metric	|The number of impressions where the video was viewed to 75%.	|
|Impression Delivery Info	|video_unmutes	|LONG	|Metric	|The number of impressions where a shopper unmuted the video.	|

### sponsored\_ads\_invalid\_traffic

The number of traffic removed by the traffic quality filter. This includes potentially fraudulent, non-human, and other illegitimate traffic.

|Field Category	|Name	|Data Type	|Metric / Dimension	|Description	|
|---	|---	|---	|---	|---	|
|Ad Group Setup	|ad_group	|STRING	|Dimension	|Name of ad group. The value for line\_item matches ad\_group.	|
|Ad Product Info	|ad\_product\_type	|STRING	|Dimension	|The ad product of the campaign. Example: "sponsored\_products" or "sponsored\_display".	|
|Advertiser Setup	|advertiser	|STRING	|Dimension	|Advertiser name. Example: “Example Company”	|
|Campaign Setup	|campaign	|STRING	|Dimension	|Campaign Name. Example: “My Great Campaign”	|
|Click Delivery Info	|invalid_clicks	|LONG	|Metric	|Count of each invalid click. For each invalid click event, this value will be 1, but the value can be summed, averaged, and so on across various dimensions	|
|Advertiser Setup	|entity_id	|STRING	|Dimension	|Customer-facing ID of entity – can be found in the URL within the advertising console. Example: “ENTITYA6I16E0BHHHY”	|
|Event (impression or Click) Delivery Info	|event_date	|DATE	|Dimension	|Event Date (Advertiser Timezone)	|
|Event (impression or Click) Delivery Info	|event\_date\_utc	|DATE	|Dimension	|Event Date (UTC)	|
|Impression Delivery Info	|invalid_impressions	|LONG	|Metric	|Count of each invalid impression. For each invalid impression event, this value will be 1, but the value can be summed, averaged, and so on across various dimensions	|
|Line item setup	|line\_item	|STRING	|Dimension	|Name of ad group. The value for line\_item matches ad\_group.	|
|Line item setup	|line\_item\_id	|LONG	|Dimension	|Customer-facing line item ID. For Sponsored Products, the value for line\_item\_id matches ad\_group\_id. For Sponsored Display, this value will be NULL. Use ad_group_id.	|
