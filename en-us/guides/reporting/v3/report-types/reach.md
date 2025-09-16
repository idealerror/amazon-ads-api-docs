---
title: Reach and frequency reports
description: Learn about requesting reach and frequency reports using the Amazon Ads API.
type: guide
interface: api
tags:
    - Reporting
    - Amazon DSP
---

# Reach and frequency reports

Reach and frequency reports contain reach and frequency metrics broken down by advertiser, supply source and frequency group levels.

## Configurations

|Configuration	| Amazon DSP	                                            |
|---	|--------------------------------------------------------|
|`reportTypeId`	| dspReachFrequency	                                     |
|Maximum date range	| 31 days	                                               |
|Data retention	| 95 days	                                               |
|`timeUnit`	| `SUMMARY` or `DAILY`	                                  |
|`groupBy`	| `["advertiser"]` or `["advertiser", "supplySource"]` or `["advertiser", "frequencyGroup"]` |
|`format`	| `GZIP_JSON` or `CSV`	                                  |

## Amazon DSP

### Base metrics

[date](guides/reporting/v3/columns#date), [intervalStart](guides/reporting/v3/columns#intervalStart), [intervalEnd](guides/reporting/v3/columns#intervalEnd), [advertiserName](guides/reporting/v3/columns#advertiserName), [advertiserId](guides/reporting/v3/columns#advertiserId), [reach](guides/reporting/v3/columns#reach), [frequencyAverage](guides/reporting/v3/columns#frequencyAverage)

### Group by `advertiser`

Additional metrics: N/A

Filters: [advertiserId](guides/reporting/v3/columns#advertiserId)

### Group by `supplySource`

Additional metrics: [supplySource](guides/reporting/v3/columns#supplySource), [supplySourceId](guides/reporting/v3/columns#supplySourceId), [amazonExclusiveReachRate](guides/reporting/v3/columns#amazonExclusiveReachRate)

Filters: [advertiserId](guides/reporting/v3/columns#advertiserId)

### Group by `frequencyGroup`

Additional metrics: [frequencyGroupId](guides/reporting/v3/columns#frequencyGroupId), [frequencyGroupName](guides/reporting/v3/columns#frequencyGroupName)

Filters: [advertiserId](guides/reporting/v3/columns#advertiserId)

## Sample calls

#### Amazon DSP: Reach and frequency report grouped by advertiser

```
curl --location 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--data '{
    "name":"dspReachFrequency",
    "startDate":"2024-02-05",
    "endDate":"2024-02-10",
    "configuration":{
        "adProduct": "DEMAND_SIDE_PLATFORM",
        "columns": ["advertiserId","advertiserName","reach","frequencyAverage"],
        "format": "CSV",
        "reportTypeId": "dspReachFrequency",
        "groupBy": [
            "advertiser"
        ],
        "filters": [{"field":"advertiserId","values":["xxxxxxxxxx"]}],
        "timeUnit": "SUMMARY"
    }
}'
```

#### Amazon DSP: Reach and frequency report grouped by advertiser and supplySource

```
curl --location 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--data '{
    "name":"dspReachFrequency",
    "startDate":"2024-02-05",
    "endDate":"2024-02-10",
    "configuration":{
        "adProduct": "DEMAND_SIDE_PLATFORM",
        "columns": ["advertiserId","advertiserName","supplySourceId", "supplySource","reach","frequencyAverage","amazonExclusiveReachRate"],
        "format": "CSV",
        "reportTypeId": "dspReachFrequency",
        "groupBy": [
            "advertiser", "supplySource"
        ],
        "filters": [{"field":"advertiserId","values":["xxxxxxxxxx"]}],
        "timeUnit": "SUMMARY"
    }
}'
```

#### Amazon DSP: Reach and frequency report grouped by frequencyGroup and advertiser

```
curl --location 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--data '{
    "name":"dspReachFrequency",
    "startDate":"2024-02-05",
    "endDate":"2024-02-10",
    "configuration":{
        "adProduct": "DEMAND_SIDE_PLATFORM",
        "columns": ["advertiserId","advertiserName","frequencyGroupId", "frequencyGroupName","reach","frequencyAverage"],
        "format": "CSV",
        "reportTypeId": "dspReachFrequency",
        "groupBy": [
            "advertiser", "frequencyGroup"
        ],
        "filters": [{"field":"advertiserId","values":["xxxxxxxxxx"]}],
        "timeUnit": "DAILY"
    }
}'
```
