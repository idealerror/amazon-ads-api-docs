---
title: Audio and video reports
description: Learn about requesting audio and video reports using the Amazon Ads API.
type: guide
interface: api
tags:
    - DSP
---

# Audio and video reports

Audio and video reports contain data based on content genre and content rating.

## Configurations

| Configuration | Amazon DSP | 
|----------|-----|
| `reportTypeId` | `dspAudioAndVideo` | 
| Maximum date range | 395 days | 
| Data retention | 395 days | 
| `timeUnit` | `SUMMARY` | 
| `groupBy` | `campaign`, `ad`, `supplySource`, `creative`, `content` | 
| `format` | `GZIP_JSON`, `XLSX`, or `CSV` | 

## Amazon DSP

### Base metrics

[intervalStart](guides/reporting/v3/columns#intervalStart), [intervalEnd](guides/reporting/v3/columns#intervalEnd), [advertiserName](guides/reporting/v3/columns#advertiserName), [advertiserId](guides/reporting/v3/columns#advertiserId), [impressions](guides/reporting/v3/columns#impressions), [grossImpressions](guides/reporting/v3/columns#grossImpressions), [invalidImpressions](guides/reporting/v3/columns#invalidImpressions), [costPerThousandImpressions](guides/reporting/v3/columns#costPerThousandImpressions)

### Group by `campaign`

Additional metrics: [orderName](guides/reporting/v3/columns#orderName), [orderId](guides/reporting/v3/columns#orderId)

Filters: [advertiserId](guides/reporting/v3/columns#advertiserId)

### Group by `ad`

Additional metrics: [lineItemName](guides/reporting/v3/columns#lineItemName), [lineItemId](guides/reporting/v3/columns#lineItemId)

Filters: [advertiserId](guides/reporting/v3/columns#advertiserId)

### Group by `creative`

Additional metrics: [creativeName](guides/reporting/v3/columns#creativeName), [creativeId](guides/reporting/v3/columns#creativeId)

Filters: [advertiserId](guides/reporting/v3/columns#advertiserId)

### Group by `supplySource`

Additional metrics: [supplySource](guides/reporting/v3/columns#supplySource), [supplySourceId](guides/reporting/v3/columns#supplySourceId)

Filters: [advertiserId](guides/reporting/v3/columns#advertiserId)

### Group by `content`

Additional metrics: [contentGenre](guides/reporting/v3/columns#contentGenre), [contentRating](guides/reporting/v3/columns#contentRating), [contentTitle](guides/reporting/v3/columns#contentTitle), [contentType](guides/reporting/v3/columns#contentType)

Filters: [advertiserId](guides/reporting/v3/columns#advertiserId)

## Sample calls

#### Amazon DSP: Audio and video report grouped by content

```shell
curl --location 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--data '{
    "name":"dsp 7/5-7/10",
    "startDate":"2024-02-05",
    "endDate":"2024-02-10",
    "configuration":{
        "adProduct": "DEMAND_SIDE_PLATFORM",
        "columns": ["contentGenre"],
        "format": "CSV",
        "reportTypeId": "dspAudioAndVideo",
        "groupBy": [
            "content"
        ],
        "filters": [{"field":"advertiserId","values":["xxxxxxxxxx"]}],
        "timeUnit": "SUMMARY"
    }
}'
```