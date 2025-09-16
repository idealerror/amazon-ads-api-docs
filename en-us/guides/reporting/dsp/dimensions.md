---
title: DSP reporting dimensions
description: Understand what dimensions are available for request in DSP reports. 
type: guide
interface: api
tags:
    - DSP
    - Reporting
---

# Dimensions

Dimensions determine the aggregation level of your report data. The available dimensions depend on the chosen report type. Each dimension also adds certain metrics to your report automatically. Use the [table below](#summary-table) to understand which dimensions are available for each report type and which metrics are automatically added.

>[NOTE] Order is the default dimension if none is specified. 

### Summary table

|Dimension	|Report types	|Default metrics	|
|---	|---	|---	|
|Order	|[All](guides/reporting/dsp/report-types)	|[orderName](guides/reporting/dsp/metrics#orderName)<br/>[orderId](guides/reporting/dsp/metrics#orderId)<br/>[orderStartDate](guides/reporting/dsp/metrics#orderStartDate)<br/>[orderEndDate](guides/reporting/dsp/metrics#orderEndDate)<br/>[orderBudget](guides/reporting/dsp/metrics#orderBudget)<br/>[orderExternalId](guides/reporting/dsp/metrics#orderExternalId)<br/>[orderCurrency](guides/reporting/dsp/metrics#orderCurrency)	|
|Line_item	|[All](guides/reporting/dsp/report-types)	|[lineItemName](guides/reporting/dsp/metrics#lineItemName)<br/>[lineItemId](guides/reporting/dsp/metrics#lineItemId)<br/>[lineItemStartDate](guides/reporting/dsp/metrics#lineItemStartDate)<br/>[lineItemEndDate](guides/reporting/dsp/metrics#lineitemEndDate)<br/>[lineItemBudget](guides/reporting/dsp/metrics#lineItemBudget)<br/>[lineItemExternalId](guides/reporting/dsp/metrics#lineItemExternalId)	|
|Creative	|[Campaign](guides/reporting/dsp/report-types#campaign)<br/>[Conversion_source](guides/reporting/dsp/report-types#conversion-source)	|[creativeName](guides/reporting/dsp/metrics#creativeName)<br/>[creativeID](guides/reporting/dsp/metrics#creativeID)<br/>[creativeType](guides/reporting/dsp/metrics#creativeType)<br/>[creativeSize](guides/reporting/dsp/metrics#creativeSize)<br/>[creativeAdId](guides/reporting/dsp/metrics#creativeAdId) (not available for `CONVERSION_SOURCE` reports)	|
|Site	|[Inventory](guides/reporting/dsp/report-types#inventory)	|[siteName](guides/reporting/dsp/metrics#siteName)	|
|Supply	|[Inventory](guides/reporting/dsp/report-types#inventory)	|[supplySourceName](guides/reporting/dsp/metrics#supplySourceName)	|
|Deal	|[Inventory](guides/reporting/dsp/report-types#inventory)	|[deal](guides/reporting/dsp/metrics#deal)<br/>[dealID](guides/reporting/dsp/metrics#dealID)	|
|Country	|[Geography](guides/reporting/dsp/report-types#geography)	|[country](guides/reporting/dsp/metrics#country)	|
|State\_country\_region	|[Geography](guides/reporting/dsp/report-types#geography)	|[region](guides/reporting/dsp/metrics#region)	|
|City	|[Geography](guides/reporting/dsp/report-types#geography)	|[city](guides/reporting/dsp/metrics#city)	|
|DMA	|[Geography](guides/reporting/dsp/report-types#geography)	|[designatedMarketAreaCode](guides/reporting/dsp/metrics#designatedMarketAreaCode)<br/>[designatedMarketAreaName](guides/reporting/dsp/metrics#designatedMarketAreaName)	|
|Postal_code	|[Geography](guides/reporting/dsp/report-types#geography)	|[postalCode](guides/reporting/dsp/metrics#postalCode)	|
|Operating_system	|[Technology](guides/reporting/dsp/report-types#technology)	|[operatingSystem](guides/reporting/dsp/metrics#operatingSystem)	|
|Browser_type	|[Technology](guides/reporting/dsp/report-types#technology)	|[browser](guides/reporting/dsp/metrics#browser)	|
|Browser_version	|[Technology](guides/reporting/dsp/report-types#technology)	|[browserVersion](guides/reporting/dsp/metrics#browserVersion)	|
|Device_type	|[Technology](guides/reporting/dsp/report-types#technology)	|[device](guides/reporting/dsp/metrics#device)	|
|Environment	|[Technology](guides/reporting/dsp/report-types#technology)	|[environmentType](guides/reporting/dsp/metrics#environmentType)	|
## Default metrics for each dimension

Each dimension automatically includes some default metrics in your report data. If you try to specify these default metrics in the metrics list in your request body, you will receive an error. 


### Example error from including default metric in request body 

This erroneous request has report type `Geography` and dimension `DMA`:

```
curl --location --request POST 'https://advertising-api.amazon.com/accounts/ID123456789/dsp/reports' \
     --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxxx' \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
     --header 'Accept: application/vnd.dspcreatereports.v3+json' \
     --data-raw '{
         "type": "GEOGRAPHY"
         "dimensions" ["DMA"],
         "metrics": ["designatedMarketAreaCode"],
         "startDate": "2022-12-05",
         "endDate": "2022-12-19"
     }'
```

The API returns an error due to including  `designatedMarketAreaCode`, a default metric for the `DMA` dimension, in the `metrics` field:

```
{
    "message": "'1' validation error(s) occurred. Please correct them and request again.",
    "requestId": "c4e391f6-cdcc-4ee1-a4df-3ff50a8f25d7",
    "errors": [
    {
        "errorType": "REQUEST_BODY_FIELD_INVALID_METRICS",
        "field": "metrics",
        "message": "Request body field 'metrics' has invalid values: [designatedMarketAreaCode]"
        }
    ]
}
```