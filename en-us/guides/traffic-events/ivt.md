---
title: invalid traffic vendor
description: Querying invalid traffic vendor report.
type: guide
interface: api
---
# Querying invalid traffic vendor report

Advertisers can run invalid traffic vendor (IVT) queries on ad events served by sponsored ads or Amazon demand-side platform (Amazon DSP) against a third-party vendor's data sets.

To initiate an IVT query, use the `POST /avc/workflowexecutions` endpoint with the request body structured as below:

```
{
  "templatedReport":{
    "type":"IVT_REPORT",
    "attributes":{
      "measurementSelections":{
        "measurementVendors":[{"vendor":"VendorName"}]
      }
    }
  },
  "timeWindow":{
    "type":"CURRENT_MONTH"
  }
}
```

**Eligible vendors**

Replace `VendorName` with the appropriate vendor name in the request. Currently, the following vendors are supported:

* AdLoox
* IAS (Integral Ad Science)

**Supported timeWindow types**

The following types of time windows can be used for specifying input data for the IVT workflow execution.

- **`EXPLICIT`** : The start and end date of the time window must be explicitly provided in the request. Note that the start date-time and end date-time must be in the following format `00:00:00`

* **`MOST_RECENT_WEEK`** : The time window will be the most recent 1-week window for which the instance is likely to have data, aligned to day boundaries.

- **`CURRENT_MONTH`** : The time window will be the start of the current month up to the most recent time for which the instance is likely to have data.

> [NOTE] TCR [data availability and retention rules](guides/traffic-events/overview#how-the-traffic-events-api-works) apply.

### Sample request body

```
curl --location
'https://advertising-api.amazon.com/avc/workflowexecutions'\--header 'Authorization: XXXXX'
--header 'Amazon-Advertising-API-ClientID:
amzn1.application-oa2-client.XXXXX'
--header 'Amazon-Advertising-API-Entity-Id: XXXXX'
--header 'Amazon-Advertising-API-MarketplaceId: ATVPDKIKX0DER'
--data '
    {
        "templatedReport": {
            "type": "IVT_REPORT",
            "attributes": {
                "measurementSelections": {
                    "measurementVendors": [{"vendor": "VendorName"}]
                }
            }
        },
        "timeWindow": {
            "type": "CURRENT_MONTH"
        }
}'
```

### Sample response

```
{
    "workflowExecution": {
        "advertiserId": "XXXXX",
        "createTime": "2024-10-22T18:42:48.339Z",
        "lastUpdatedTime": "2024-10-22T18:42:48.339Z",
        "status": "PENDING",
        "timeWindow": {
            "end": "2024-10-20T00:00:00Z",
            "start": "2024-10-01T00:00:00Z",
            "timeZoneOriginal": "UTC"
        },
        "workflowExecutionId": "30eb7e3a-4ec7-4dae-9817-85d651a9cc34"
    }
}
```

## Report eligibility

To be able to run IVT reports, advertisers need to have a relationship with the IVT vendors.

- To be able to run reports with AdLoox, reach out to [amazon@adloox.com](mailto:amazon@adloox.com).
- To be able to run reports with IAS, reach out to [AmazonSupport@integralads.com](mailto:AmazonSupport@integralads.com).

If you have further question on eligibility and vendor selection please [contact us](support/overview#traffic-events-api).

## Data availability

Data becomes available in TCR within 96 hours after vendors share relevant advertiser IDs with Amazon.

> [WARNING] Data cannot be made retroactively available in IVT reports.

## Report outputs

### Sponsored ads campaigns

| advertiser\_name    | advertiser\_cfid | campaign\_name                            | campaign\_cfid | vendor | event\_date\_utc | gross\_impressions | adloox\_invalid\_impressions | report\_run\_date | filtered\_reason |
| ------------------- | ---------------- | ----------------------------------------- | -------------- | ------ | ---------------- | ------------------ | ---------------------------- | ----------------- | ---------------- |
| Accent Athletics SA |                  | Accent Athletics sponsored ads campaign1 |                | AdLoox | 10/1/2024        | 1750               | 2                            | 10/17/2024        |                  |
| Accent Athletics SA |                  | Accent Athletics sponsored ads campaign1 |                | AdLoox | 10/2/2024        | 176                | 0                            | 10/17/2024        |                  |
| Accent Athletics SA |                  | Accent Athletics sponsored ads campaign2 |                | AdLoox | 10/1/2024        | 12215              | 7                            | 10/17/2024        |                  |
| Accent Athletics SA |                  | Accent Athletics sponsored ads campaign2 |                | AdLoox | 10/2/2024        | 12196              | 21                           | 10/17/2024        |                  |

### Amazon DSP campaigns

| advertiser\_name     | advertiser\_cfid | campaign\_name                        | campaign\_cfid | vendor | event\_date\_utc | gross\_impressions | IAS\_invalid\_impressions | report\_run\_date | filtered\_reason |
| -------------------- | ---------------- | ------------------------------------- | -------------- | ------ | ---------------- | ------------------ | ------------------------- | ----------------- | ---------------- |
| Accent Athletics DSP | 123456789        | Accent Athletics DSP campaign name 1 | 987654321      | IAS    | 10/1/2024        | 1750               | 2                         | 10/17/2024        |                  |
| Accent Athletics DSP | 123456789        | Accent Athletics DSP campaign name 1  | 987654321      | IAS    | 10/2/2024        | 176                | 0                         | 10/17/2024        |                  |
| Accent Athletics DSP | 123456789        | Accent Athletics DSP campaign name 2 | 9876543210     | IAS    | 10/1/2024        | 3132               | 10                        | 10/17/2024        |                  |
| Accent Athletics DSP | 123456789        | Accent Athletics DSP campaign name 2  | 9876543210     | IAS    | 10/2/2024        | 9526               | 85                        | 10/17/2024        |                  |

### Report column name definitions

| Column name                  | Description                                                                                                                                                              |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| advertiser\_name             | Name of the sponsored ads or Amazon DSP advertiser specified in the report header.                                                                                       |
| advertiser\_cfid             | ID of the Amazon DSP advertiser specified in the report header.                                                                                                          |
| campaign\_name               | Name of the sponsored ads or Amazon DSP campaign.                                                                                                                        |
| campaign\_cfid               | ID of Amazon DSP campaign.                                                                                                                                               |
| vendor                       | Name of the vendor specified in the report query.                                                                                                                        |
| event\_date\_utc             | Date of the impression events, in Coordinated Universal Time\(UTC\).                                                                                                     |
| gross\_impressions           | Total number of impressions against which the query was run.                                                                                                             |
| adloox\_invalid\_impressions | If the vendor specified in the query was AdLoox, this column will show the number of impressions that were classified as invalid according to AdLoox.                    |
| ias\_invalid\_impressions    | If the vendor specified in the query was IAS, this column will show the number of impressions that were classified as invalid according to IAS.                          |
| report\_run\_date            | Date when the report was run.                                                                                                                                            |
| filtered\_reason             | This column will be empty unless the number of events for a particular campaign did not meet the [aggregation threshold](guides/traffic-events/data-aggregation-thresholds). |
