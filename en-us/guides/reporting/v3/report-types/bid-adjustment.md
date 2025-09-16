---
title: Bid adjustments reports
description: Learn what report types and metrics are available in version 3 reporting for sponsored ads using the Amazon Ads API.
type: guide
interface: api
tags:
    - Reporting
    - DSP
    - Bid adjustments
---

# Bid adjustment reports

The bid adjustment report aims to help advertisers validate their bid adjustments application. It contains bid metrics broken down by bid adjustments rule and bid adjustments matched terms. 

## Configurations

| Configuration	| Amazon DSP                                             |
|----------|--------------------------------------------------------|
| `reportTypeId` | `dspBidAdjustment`                                     | 
| Maximum date range | 31 days                                                | 
| Data retention | 395 days	                                              |
| `timeUnit` | `SUMMARY` or `DAILY`	                                  |
| `groupBy`	| `bidAdjustmentRule` and/or `bidAdjustmentMatchedTerms` |
| `format` | `GZIP_JSON` or `CSV` or `XLSX`                         |

## Amazon DSP

The DSP bid adjustment report is only available for accounts that are actively using bid adjustments on live ADSP campaigns. The report will be filtered for reporting only the line items with bid adjustments. If an advertiser pulls a report which contains line items both with and without bid adjustments, the report will only show the summary metrics for the ones that have bid adjustments. 

At least one groupby (`bidAdjustmentRule` and/or `bidAdjustmentMatchedTerms`) must be added get the report.

* With `bidAdjustmentRule`, the report will be grouped at Advertiser, Order, Line and Rule level, so we recommend keeping those fields in the columns list. Base metrics are available. 
* With `bidAdjustmentMatchedTerms`, the report will be grouped at Advertiser, Order, Line, Rule and Matched Terms level, so we recommend keeping those fields in the columns list. These additional fields are available besides base metrics when using this group by: `bidAdjustmentMatchedTerms`, `bidAdjustmentMatchedTermIds`, `bidAdjustment` and `matchRate`. `matchRate` is only available in daily reports. 

Similar to other reports, the `date` field can be added to the columns list when pulling a DAILY report.

`startDate` and `endDate` are configurable in API calls but are not supported columns to show in SUMMARY report. 

>[NOTE]Any row of record has less than 2 bid counts per hour will be filtered out in both daily and summary report. We provide match rate in daily reports where you would be able to know the percentage of traffic filtered out by subtracting all listed traffic match rate from 100%. 

### Base metrics

[date](guides/reporting/v3/columns#date) (available only for daily report), [advertiserName](guides/reporting/v3/columns#advertiserName), [advertiserId](guides/reporting/v3/columns#advertiserId), [orderName](guides/reporting/v3/columns#orderName), [orderId](guides/reporting/v3/columns#orderId), [lineItemName](guides/reporting/v3/columns#lineItemName), [lineItemId](guides/reporting/v3/columns#lineItemId), [bidAdjustmentRule](guides/reporting/v3/columns#bidadjustmentrule), [bidAdjustmentRuleId](guides/reporting/v3/columns#bidadjustmentruleid), [bids](guides/reporting/v3/columns#bids), [winRate](guides/reporting/v3/columns#winrate)

### Group by `bidAdjustmentRule`

Filters: [advertiserId](guides/reporting/v3/columns#advertiserId)

### Group by `bidAdjustmentMatchedTerms`

**Additional metrics:** [bidAdjustmentMatchedTerms](guides/reporting/v3/columns#bidadjustmentmatchedterms), [bidAdjustmentMatchedTermIds](guides/reporting/v3/columns#bidadjustmentmatchedtermids), [bidAdjustment](guides/reporting/v3/columns#bidadjustment), [matchRate](guides/reporting/v3/columns#matchrate) (available only in DAILY report)

Filters: [advertiserId](guides/reporting/v3/columns#advertiserId)

## Sample requests

### Summary CSV report to get bids and win rate per Bid Adjustment Rule

```json
curl -L 'https://<Regionalized API endpoint>/reporting/reports'
  -H 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
  -H 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
  -H 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
  -H 'Authorization: Bearer Atza|xxxxxxxxxx' \
  -d '{
        "name": "<your report name>",
        "startDate": "2025-03-26",
        "endDate": "2025-03-31",
        "configuration": {
            "adProduct": "DEMAND_SIDE_PLATFORM",
            "groupBy": ["bidAdjustmentRule"],
            "columns": [
                "advertiserId", 
                "orderId",
                "lineItemId"
                "bidAdjustmentRule", 
                "bidAdjustmentRuleId", 
                "bids", 
                "winRate"],
            "reportTypeId": "dspBidAdjustments",
            "timeUnit": "SUMMARY",
            "format": "CSV",
            "filters": [{
                "field": "advertiserId",
                "values": ["1234567890"]
            }]
        }
    }' 
```

### Daily CSV report to get bids and win rate per Bid Adjustment Rule

```
curl -L 'https://<Regionalized API endpoint>/reporting/reports'
  -H 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
  -H 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
  -H 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
  -H 'Authorization: Bearer Atza|xxxxxxxxxx' \
  -d '{
        "name": "<your report name>",
        "startDate": "2025-03-26",
        "endDate": "2025-03-31",
        "configuration": {
            "adProduct": "DEMAND_SIDE_PLATFORM",
            "groupBy": ["bidAdjustmentMatchedTerms"],
            "columns": [
                "date",
                "advertiserId", 
                "orderId",
                "lineItemId"
                "bidAdjustmentRule", 
                "bidAdjustmentRuleId", 
                "bidAdjustmentMatchedTerms",
                "bidAdjustmentMatchedTermIds",
                "bidAdjustment",
                "matchRate",
                "bids", 
                "winRate"],
            "reportTypeId": "dspBidAdjustments",
            "timeUnit": "DAILY",
            "format": "CSV",
            "filters": [{
                "field": "advertiserId",
                "values": ["1234567890"]
            }]
        }
    }' 
```
