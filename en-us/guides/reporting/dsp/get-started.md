---
title: Getting started with DSP reporting
description: Understand the steps to request and download a DSP report using the Amazon Ads API. 
type: guide
interface: api
tags:
    - DSP
    - Reporting
---

# Create DSP reports

This tutorial outlines how to asynchronously request and download reports for DSP campaigns.

To generate a report:

1. [Retrieve your accountId](#retrieve-your-accountid)
2. [Request a report](#request-a-report)
3. [Wait for report to generate](#wait-for-report-to-generate)
4. [Download the report](#download-the-report)

>[TIP:Try it in Postman] Follow along with this tutorial using Reporting folder of the Amazon Ads API Postman collection. For set up instructions, see our [Postman tutorial](guides/postman).


## Headers

Each request described in this tutorial requires four header parameters:

| Parameter | Description |
|--------------------------|---------------|
| `Amazon-Advertising-API-ClientId` | The client ID associated with your Login with Amazon application. Used for authentication. |
| `Authorization` | Your [access token](guides/account-management/authorization/access-tokens). Used for authentication. |
| `Accept` | Specifies the content we expect in response; depends on the request endpoint. <br/><br/>Set to `application/vnd.dspcreatereports.v3+json` for requesting a report and `application/vnd.dspgetreports.v3+json` for checking the report status. |
| `Content-Type` | Set to `application/json`. |

For an introduction to API authentication, see [Authorization overview](guides/account-management/authorization/overview).

## Retrieve your `accountId`

You need your `accountId` whether you are generating or retrieving reports. `accountId` is used for 
authentication and for determining the scope of your report. 

How you retrieve your `accountId` depends on whether you have a managed or self-service account. For more information, see [Reporting by account type](guides/reporting/dsp/reporting-by-account-type). 
 
## Generate a report

### Request a report
 
DSP reports support a variety of report types and metrics. For more information on what configurations are supported, see [Report types](guides/reporting/dsp/report-types).

All reports requests use a single endpoint: `POST /accounts/{accountId}/dsp/reports`. See the [API reference](dsp-reports-beta-3p/#tag/Reports) for details on parameters.

#### `timeUnit`

`timeUnit` can be set to `DAILY` or `SUMMARY`. `DAILY` aggregates data for each day in the time period while `SUMMARY` aggregates data for the entire time period. `DAILY` `timeUnit` is not supported for the `AUDIENCE` report type. The maximum date range between `startDate` and `endDate` is 31 days. The `endDate` can be up to 90 days before today.

**Sample request body (`timeUnit` set to `DAILY`)**

```
{
    "dimensions" ["ORDER", "LINE_ITEM"],
    "timeUnit": "DAILY",
    "startDate": "2022-12-05",
    "endDate": "2022-12-19"
}
```

**Sample request body (`timeUnit` set to `SUMMARY`)**

```
{
    "dimensions" ["ORDER", "LINE_ITEM"],
    "timeUnit": "SUMMARY",
    "startDate": "2022-12-05",
    "endDate": "2022-12-19"
}
```

#### `dimensions`

Dimensions determine the aggregation level of your report. Adding a dimension automatically adds the metrics for that dimension in the report. If you add multiple dimensions, the DSP reporting API uses the lowest level dimension for data aggregation and includes the dimensional metrics for every added dimension in your report. For more information, see [Dimensions](guides/reporting/dsp/dimensions).

#### `advertiserIds`

Self-service DSP entity owners may use the `advertiserIds` field to request data from specific advertisers. For more information, see [Reporting by account type](guides/reporting/dsp/reporting-by-account-type#self-service-accounts).

>[NOTE] You can only request up to 100 advertisers per report. If you need to report on more than 100 advertisers, request multiple reports. 

#### Default values 

By default, in the `POST /accounts/{accountId}/dsp/reports` request body:

- The report type is set to `CAMPAIGN`
- The format is set to `JSON`
- `timeUnit` is set to `SUMMARY` and aggregated for the time period specified (`startDate` to `endDate`)
- The dimension is set to `ORDER`

You must specify a `startDate` and `endDate` in your request.


#### Sample request

This sample shows a request for a summary DSP campaign report between December 5th, 2022 and December 19th, 2022 for the advertiser with accountId `ID123456789`. 

>[NOTE] The sample calls in this tutorial use the North American API endpoint (`https://advertising-api.amazon.com`). See [API overview](reference/api-overview#api-endpoints) to understand which regional endpoint to use based on your marketplace.

```
curl --location --request POST 'https://advertising-api.amazon.com/accounts/ID123456789/dsp/reports' \
     --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxxx' \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
     --header 'Accept: application/vnd.dspcreatereports.v3+json' \
     --data-raw '{
         "metrics": ["totalFee", "totalCost", "clickThroughs", "purchasesClicks14d", "eCPM", "impressions", "ROAS14d"],
         "dimensions": ["ORDER", "LINE_ITEM"],
         "startDate": "2022-12-05",
         "endDate": "2022-12-19"
     }'
```

#### Sample response

A successful request response includes a `reportId` and a `status` field. 

```
{
    "reportId": "b2230738-5b81-2423-a3a6-380d5522ba78",
    "type": "CAMPAIGN",
    "format": "JSON",
    "status": "IN_PROGRESS",
    "statusDetails": "In progress",
    "location": "",
    "expiration": "2023-02-01T20:25:13.356Z"
}
```

### Wait for report to generate

Once you have made a successful POST call, report generation can take up to three hours.

You can check the report generation status by using the `reportId` returned in the initial request to call the GET reports endpoint: [GET /accounts/{accountId}/dsp/reports/{report-id}](dsp-reports-beta-3p/#tag/Reports/operation/getCampaignReportV3).
 
#### Sample request

```
curl --location --request GET /accounts/{accountId}/dsp/reports/{report-id} \
     --header 'Content-Type: application/vnd.dspgetreports.v3+json' \
     --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
     --header 'Authorization: Bearer Atza|xxxxxxxxxxx'  \
```

Any successful call to the GET status endpoint returns a 200 status code. To check if report generation has completed, check the `status` parameter in the response body. If the report is still generating, `status` is set to `PENDING` or `PROCESSING`.
 
When your report is ready to download, `status` returns as `SUCCESS`, and you will see an address in the `location` field.
 

#### Sample response (report generation in progress)

```
{
    "reportId": "TEST123",
    "type": "CAMPAIGN",
    "format": "JSON",
    "status": "IN_PROGRESS",
    "statusDetails": "In progress",
    "location": "",
    "expiration": "2022-12-28T19:30:25.632Z"
}
```

#### Sample response (report generation completed)

```
{
    "reportId": "TEST123",
    "type": "INVENTORY",
    "format": "JSON",
    "status": "SUCCESS",
    "statusDetails": "Success",
    "location": "https://corvo-reports.s3.amazonaws.com/DSP_API/2023-02-13/TEST123/inventory-report-TEST123.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20230213T231410Z&X-Amz-SignedHeaders=host&X-Amz-Expires=3600&X-Amz-Credential=TEST123%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=TEST123",
    "expiration": "2023-02-14T00:14:10.562Z"
}
```

### Download the report

Once your report is ready, the location field of a `GET /accounts/{accountId}/dsp/reports/{report-id}` contains a link to the S3 bucket where you can download your report. You can make a GET call using cURL or enter the URL in your browser.
 
 
## Read your report

View the raw JSON file of your report. Within the report, the aggregation level of your data depends on your specified dimension. A sample JSON, `ORDER` level report may look like:

```
[
  {
    "orderExternalId": "6677889900",
    "orderEndDate": 1690862340000,
    "orderId": 123042566401569500,
    "orderStartDate": 1633562280000,
    "orderCurrency": "USD",
    "intervalEnd": 1674432000000,
    "orderBudget": 10000,
    "entityId": "TEST123",
    "intervalStart": 1674345600000,
    "advertiserName": "Test Account One",
    "advertiserId": 123123123,
    "orderName": "One Test Order"
  },
  {
    "orderExternalId": null,
    "orderEndDate": 1702627140000,
    "orderId": 123582641277101200,
    "orderStartDate": 1650265200000,
    "orderCurrency": "USD",
    "intervalEnd": 1674432000000,
    "orderBudget": 50000,
    "entityId": "TEST123",
    "intervalStart": 1674345600000,
    "advertiserName": "Test - Audio",
    "advertiserId": 123456789,
    "orderName": "Audio test order"
  }
]
```

## Learn more

- [DSP Reporting metrics](guides/reporting/dsp/metrics)
- [FAQ](guides/reporting/dsp/faq)
