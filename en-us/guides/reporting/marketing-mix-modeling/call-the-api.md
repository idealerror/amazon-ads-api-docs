---
title: Calling the MMM API
description: How to call the MMM API
type: guide
interface: api
tags:
  - Reporting
  - Marketing Mix Modeling
---
# Calling the MMM API

This tutorial outlines how to request and download reports for MMM.

Because MMM APIs are asynchronous, there are four steps to using the APIs:

1. [Get brand groups](#listing-brand-groups)
1. [Request a report](#requesting-reports)
1. [Wait for the report to generate](#checking-report-status)
1. [Download the report](#downloading-reports)

## Before you begin
- Complete required steps to [get started with the MMM API](guides/reporting/marketing-mix-modeling/get-started).
- Learn more about the inputs and outputs of the [MMM data feed](guides/reporting/marketing-mix-modeling/mmm-data-feed).
- Ensure you're using the North America (NA) API endpoint (`https://advertising-api.amazon.com`).

## Headers
In addition to the individual API parameters, the following header parameters are required for most of your calls.

| Header                                 | Description                                               |
|:---------------------------------------|:----------------------------------------------------------|
| Amazon-Advertising-API-ClientId        | The client ID related to a Login with Amazon application. |
| Amazon-Advertising-API-Manager-Account | The ID of the manager account you registered with MMM.    |
| Authorization                          | Login with Amazon token in the form of Bearer token.     |
| Content-type                           | application/json                                          |

## Listing brand groups
The first step is to get the brand group you would like to request data for. You will need to call the `POST: /mmm/v1/brandGroups/list` endpoint.

### Sample request
This sample shows a request for your approved brand groups.

```shell
curl --location 'https://advertising-api.amazon.com/mmm/v1/brandGroups/list' \
-H 'Amazon-Advertising-API-ClientId: {CLIENT_ID}' \
-H 'Amazon-Advertising-API-Manager-Account: {MANAGER_ACCOUNT_ID}' \
-H 'Authorization: Bearer {ACCESS_TOKEN}' \
-H 'Content-Type: application/json' \
-d '{
  "countryCodeFilter": {
    "include": ["US"]
  }
}'
```

### Sample response
A successful request results in a `200` response that includes an array of brand groups. Please note down a `brandGroupId` and we will use this in the following step to create a report.

```json
{
  "nextToken": "AQICAHjoFsfJegSMXvBg1W83nA_9Uuw1qcYHscheWy3IKgoUOwGIx2Wlj4SERv2hj_KIZEeUAAAB7DCCAegGCSqGSIb3DQEHBqCCAdkwggHVAgEAMIIBzgYJKoZIhvcNAQcBMB4GCWCGSAFlAwQBLjARBAynar294mm0_8RdsqwCARCAggGfc6eHTvM9QGJu4Iq-J-e2RjTXkZfrF7zKDseiEAE2XiisGMLDpvI_t1qEK750kt5fj-_T2KgiDBLgLGtMYAMbFlM03zhIQHIKseVFddZvOVjWf2ZnGgDqJrjfIRtiE_yiQTmA51aJgrKKcYBWQrEfPNn-PvyIyrO7t9qO7ctbuV4bKD2YKE1zWYvhEjRlZdEXxMu_6XvPEmfeckb1_nNYOLB5NxYAkbMknGiQby-kiWH9Yi5btdn_rSCxYPTVbC1ETqjl4FwtNBqBuxTSm_MnCbb0Ld-ETWtfiaRE32oYDW4Uo29q8xePE8ruJjqI60yaBHgNis2oPweP5DQjvXpajjQRpSyF8rI_VAjEWXAgmlYDiUle-lZ4SArdtoM9UPePdL_Mb1L1STUO2SimGJZdCE3IYbc54dOabfRXvhlHfIeWdCx8C23DUiTWxLPcOb36S1ViPb7mU7f6ZY7PQSz95GKGwEBlk0DemsJGIoFi3pfLomOhm-Xx6FUWx5ostjrgNAkY796vN-DEjJ7uzS-bcIx4MrofETEZRDqH8l-85Q==",
  "brandGroups": [
    {
      "brandGroupId": "68d71a07-a3ea-47f2-9051-df7fd08a64da",
      "brandGroupName": "Continuous Protection Sunscreen Lotion for Kids",
      "advertiserName": "Jack & Jill",
      "countryCode": "US"
    },
    {
      "brandGroupId": "d0145381-b4a9-41a0-83de-e56c8f07d712",
      "brandGroupName": "Running Shoes",
      "advertiserName": "Accent Athletics",
      "countryCode": "US"
    },
    {
      "brandGroupId": "23d7c364-0124-410b-b899-8475eddbafef",
      "brandGroupName": "Apparel",
      "advertiserName": "Accent Athletics",
      "countryCode": "US"
    }
  ]
}
```

## Requesting reports
MMM reports support a variety of different input parameters. To create a report, use the  `POST /mmm/v1/reports` endpoint. For more information, see the [API reference](marketing-mix-modeling#tag/Reports/operation/createMmmReport) for available parameters and the [MMM data feed](guides/reporting/marketing-mix-modeling/mmm-data-feed) guide.

### Sample request
This sample shows a report request for media and sales data ranging from 2024-01-01 to 2024-03-31, aggregated to a weekly time unit and postal code geography.

```shell
curl --location 'https://advertising-api.amazon.com/mmm/v1/reports' \
-H 'Amazon-Advertising-API-ClientId: {CLIENT_ID}' \
-H 'Amazon-Advertising-API-Manager-Account: {MANAGER_ACCOUNT_ID}' \
-H 'Authorization: Bearer {ACCESS_TOKEN}' \
-H 'Content-Type: application/json' \
-d '{
  "reportName": "Accent Athletics US Running Shoes Q1 2024",
  "description": "Weekly media & sales by ZIP Code.",
  "configuration": {
    "brandGroupId": "d0145381-b4a9-41a0-83de-e56c8f07d712",
    "metricsType": "MEDIA_AND_SALES",
    "geoDimension": "POSTAL_CODE",
    "timeUnit": "WEEKLY"
  },                    
  "startDate": "2024-01-01",
  "endDate": "2024-03-31",
  "dueDate": "2024-04-15"
}'
```

### Sample response
A successful request results in a `200` response that includes the report request. Note down the `reportId` for use in the following step to check the generation status.

```json
{
  "reportId": "3190da6e-5633-4b9a-b588-acfcdcf4d47e",
  "reportName": "Accent Athletics US Running Shoes Q1 2024",
  "description": "Weekly media & sales by ZIP Code.",
  "configuration": {
    "brandGroupId": "d0145381-b4a9-41a0-83de-e56c8f07d712",
    "metricsType": "MEDIA_AND_SALES",
    "geoDimension": "POSTAL_CODE",
    "timeUnit": "WEEKLY"
  },
  "startDate": "2024-01-01",
  "endDate": "2024-03-31",
  "dueDate": "2024-04-15",
  "status": "PENDING",
  "urls": null,
  "urlsExpireAt": null,
  "failureCode": null,
  "failureMessage": null
}
```

## Checking report status
After you have made a successful `POST` call, report generation may take up to 24 hours. Check the report generation status by using the `reportId` from the create request's response. You will call the `GET /mmm/v1/reports/{reportId}` endpoint.

### Sample request
```shell
curl --location 'https://advertising-api.amazon.com/mmm/v1/reports/3190da6e-5633-4b9a-b588-acfcdcf4d47e' \
-H 'Amazon-Advertising-API-ClientId: {CLIENT_ID}' \
-H 'Amazon-Advertising-API-Manager-Account: {MANAGER_ACCOUNT_ID}' \
-H 'Authorization: Bearer {ACCESS_TOKEN}' \
-H 'Content-Type: application/json'
```

A successful call to the `GET` endpoint returns a `200` status code. You can look at the `status` field of the response to check if your report has finished generating. Below is a table of possible statuses:

| Status     | Description                                                                         |
|:-----------|:------------------------------------------------------------------------------------|
| PENDING    | Report has been created and is awaiting processing.                                          |
| PROCESSING | Report is processing.                                                               |
| SUCCEEDED  | Report is complete. Check `urls` for the output files.                             |
| FAILED     | Report processing failed. Check the `failureCode` and `failureMessage` for details. |
| CANCELED   | Report is canceled. Contact <mmm-support@amazon.com> if this is unexpected.         |

## Downloading reports
After your report's status is `SUCCEEDED`, it is ready. The `urls` field of the report's response body contains links to download your report's files. You can make a `GET` call using cURL or enter the URL in your browser to download the file.

### Sample response

```json
{
  "reportId": "3190da6e-5633-4b9a-b588-acfcdcf4d47e",
  "reportName": "Accent Athletics US Running Shoes Q1 2024",
  "description": "Weekly media & sales by ZIP Code.",
  "configuration": {
    "brandGroupId": "d0145381-b4a9-41a0-83de-e56c8f07d712",
    "metricsType": "MEDIA_AND_SALES",
    "geoDimension": "POSTAL_CODE",
    "timeUnit": "WEEKLY"
  },
  "startDate": "2024-01-01",
  "endDate": "2024-03-31",
  "dueDate": "2024-04-15",
  "status": "SUCCEEDED",
  "urls": [
    "https://example.com/3190da6e-5633-4b9a-b588-acfcdcf4d47e/AggregatedSummaryReport.xlsx",
    "https://example.com/3190da6e-5633-4b9a-b588-acfcdcf4d47e/CreativeGrainNationalCampaignPerformanceMetrics.tsv.zip",
    "https://example.com/3190da6e-5633-4b9a-b588-acfcdcf4d47e/SSPANationalCampaignPerformanceMetrics.tsv.zip"
  ],
  "urlsExpireAt": "2024-04-12T09:15:45Z",
  "failureCode": null,
  "failureMessage": null
}
```