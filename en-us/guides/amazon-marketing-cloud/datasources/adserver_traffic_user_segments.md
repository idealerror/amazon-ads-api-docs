---
title: Ad Server traffic user segments
description: Data source schema for the Ad Server traffic user segments table 
type: guide
interface: api
tags:
    - Reporting
keywords:
    - ad server traffic
---
# Amazon Ad Server traffic user segments

> [NOTE] Amazon Ad Server was sunset in Q4 2024, please visit [this page](https://support.sizmek.com/hc/en-us/articles/21193881095565-AAS-offboarding-package) (AAS offboarding information) for offboarding support resources and sunset FAQs.

## adserver\_traffic\_by\_user\_segments

The data source `adserver _traffic_by_user_segments` includes Amazon audience segment-level information for impressions and clicks that are measured by the Amazon Ad Server and matched to the Amazon user ID value. This data source has the same structure as `adserver_traffic` with the addition of Amazon audience segment fields. Each traffic event (impression and click) appears multiple times in this data source. The number of times each event appears in the data source corresponds to the number of segments identified for each user ID value.

The following table shows columns in addition to the ones listed for [adserver_traffic](guides/amazon-marketing-cloud/datasources/adserver_traffic).

| Field  Category                                                  | Name                           | Data Type | Metric / Dimension | Description                                                                                                                                                               | Aggregation Threshold |
| ---------------------------------------------------------------- | ------------------------------ | --------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| Advertiser  setup                                                | adserver\_brand                | STRING    | Dimension          | Amazon Ad  Server brand name. A single Amazon Ad Server advertiser can have multiple  brands. One campaign can be associated to a single brand.                           | LOW                   |
| Advertiser setup                                                 | adserver\_brand\_id            | LONG      | Dimension          | Amazon  Ad Server brand ID. A single Amazon Ad Server advertiser can have multiple  brands. One campaign can be associated to a single brand.                             | LOW                   |
| Event (impression, Click, Interaction, Conversion) Delivery Info | adserver\_session\_id          | STRING    | Dimension          | Amazon  Ad Server session ID.                                                                                                                                             | VERY\_HIGH            |
| Site setup                                                       | adserver\_site                 | STRING    | Dimension          | Amazon  Ad Server site name. Represents an entity that buys or sells media (can be a  publisher or DSP name; this is not a domain name of site where the ad was  served). | LOW                   |
| Segments                                                         | behavior\_segment\_description | STRING    | Dimension          | User behavior segment description                                                                                                                                         | LOW                   |
| Segments                                                         | behavior\_segment\_id          | INTEGER   | Dimension          | User behavior segment identifier                                                                                                                                          | LOW                   |
| Segments                                                         | behavior\_segment\_name        | STRING    | Dimension          | User behavior segment name                                                                                                                                                | LOW                   |
