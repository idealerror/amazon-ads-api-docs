---
title: Amazon audience segment membership
description: The audience segment membership table
type: guide
interface: api
---
# Amazon audience segment membership tables

**Analytics tables:**

- `audience_segments_amer_inmarket`
- `audience_segments_amer_lifestyle`
- `audience_segments_apac_inmarket`
- `audience_segments_apac_lifestyle`
- `audience_segments_eu_inmarket`
- `audience_segments_eu_lifestyle`
- `segment_metadata`
- `audience_segments_amer_inmarket_snapshot`
- `audience_segments_amer_lifestyle_snapshot`
- `audience_segments_apac_inmarket_snapshot`
- `audience_segments_apac_lifestyle_snapshot`
- `audience_segments_eu_inmarket_snapshot`
- `audience_segments_eu_lifestyle_snapshot`

**Audiences tables:**

- There are associated audiences tables for each of the tables above.

> [WARNING] Workflows that use these tables will time out when run over extended periods of time.

Amazon audience segment membership is presented across multiple data views, the default data views are DIMENSION dataset type, whereas the historical snapshots are FACT dataset type. Based on this implementation detail, the default data view presents the most recent user-to-segment associations, whereas the historical snapshots present the user-to-segment associations as records on the first Thursday of each calendar month.

> [NOTE] The default data views will be the most-recent available user-to-segment association and segment metadata file, these datasets are presented as DIMENSION datasets type and refreshed then published into Amazon Marketing Cloud daily. The historical _snapshot data views will contain previous user-to-segment association signals, which are logged monthly (first Thursday per calendar month) and presented as a FACT dataset type.

Audience segment membership is broken-out by region (AMER/EU/APAC) and Amazon audience subcategory (Lifestyle, Life event, In-market and Interests).


## audience\_segments\_\<region>\_\<category\>

Records of all active `user-to-segment` association. This table presents each `user-to-segment` association as a distinct row. The `segment_id` value is not globally unique, however the `segment_id` is unique per `segment_marketplace_id`.
AMC defined best practices and recommendations for querying the audience segment membership tables are listed below:

- Never query more than 1 `audience_segments_<region>_<category>` table within a single SQL query.
- Never query audience segment membership tables from multiple regions (AMER/EU/APAC) within a single SQL query.
- Include SQL filtering using `segment_marketplace_id` for all queries.
- Include SQL filtering using `segment_id` whenever the scope of analysis can be proactively refined to subset of all segment\_ids.
- When querying the historical `snapshot` data views limit your analysis date-range, and segment-scope through SQL filtering. This proactive filtering should increase query processing speed and query success rate.
- When querying the historical `snapshot` data views ensure that the query date range includes the first Thursday of the calendar month associated with your analysis.
- When querying the historical `snapshot` data views and joining against traffic or conversion datasets, please restrict the analysis to 1 historical snapshot per SQL analysis. Workflows that use these tables will time out when run over extended periods of time.

| Field category              | Name                     | Data type | Metric / dimension | Description                                                                      | Aggregation threshold |
| --------------------------- | ------------------------ | --------- | ------------------ | -------------------------------------------------------------------------------- | --------------------- |
| Audience Segment Membership | no\_3p\_trackers         | BOOLEAN   | Dimension          | Is this item not allowed to use 3P tracking?                                     | NONE                  |
| Audience Segment Membership | segment\_id              | INTEGER   | Dimension          | Identification code for the segment.                                             | LOW                   |
| Audience Segment Membership | segment\_marketplace\_id | LONG      | Dimension          | Marketplace the segment belongs to; segments can belong to multiple.marketplaces | LOW                   |
| Audience Segment Membership | segment\_name            | STRING    | Dimension          | Name of the segment the user\_id is tagged to.                                   | LOW                   |
| Audience Segment Membership | user\_id                 | STRING    | Dimension          | User ID of the customer.                                                         | VERY\_HIGH            |
| Audience Segment Membership | user\_id\_type           | STRING    | Dimension          | Type of user ID.                                                                 | LOW                   |
| Audience Segment Membership | snapshot_datetime        | DATE      | Dimension          | The date of when snapshot was taken.                                             | LOW                   |

## segment\_metadata

Metadata that describes the `segment_id` AND `segment_marketplace_id` from the `audience_segments_<region>_<category>` tables.

**Recommendation:** When joining `segment_metadata` table with `audience_segments_<region>_<category>` tables, use a SQL double join on `segment_id` AND `segment_marketplace_id`.

| Field Category              | Name                     | Data Type | Metric / Dimension | Description                                                                           | Aggregation Threshold |
| --------------------------- | ------------------------ | --------- | ------------------ | ------------------------------------------------------------------------------------- | --------------------- |
| Audience Segment Membership | category\_level\_1       | STRING    | Dimension          | Top level of the audience segment taxonomy.                                           | LOW                   |
| Audience Segment Membership | category\_level\_2       | STRING    | Dimension          | Second level of the audience segment taxonomy.                                        | LOW                   |
| Audience Segment Membership | category\_path           | STRING    | Dimension          | Full path of the audience segment taxonomy.                                           | LOW                   |
| Audience Segment Membership | no\_3p\_trackers         | BOOLEAN   | Dimension          | Is this item not allowed to use 3P tracking?                                          | NONE                  |
| Audience Segment Membership | segment\_description     | STRING    | Dimension          | Description of the segment.                                                           | LOW                   |
| Audience Segment Membership | segment\_id              | INTEGER   | Dimension          | Identification code for the segment                                                   | LOW                   |
| Audience Segment Membership | segment\_marketplace\_id | LONG      | Dimension          | The marketplace the segment belongs to; segments can belong to multiple marketplaces. | LOW                   |
| Audience Segment Membership | segment\_name            | STRING    | Dimension          | Name of the segment.                                                                  | LOW                   |
