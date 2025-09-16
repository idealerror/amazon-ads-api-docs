---
title: Getting started with reports (version 3)
description: Understand the steps to request and download a report using version 3 reporting. 
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
---

# Get started with version 3 reports

This tutorial outlines how to asynchronously request and download reports for campaigns. 

Since Amazon Ads API reports are asynchronous, there are three calls you need to make to generate a report:

1. [Request a report](#requesting-reports)
2. [Wait for report to generate](#checking-report-status)
3. [Download the report](#downloading-reports)

>[TIP:Try it in Postman] Follow along with this tutorial using **Reporting** folder of the Amazon Ads API Postman collection. For set up instructions, see our [Postman tutorial](guides/postman).

## Headers

Each request described in this tutorial requires four header parameters: 

| Parameter | Description |
|--------------------------|---------------|
| `Amazon-Advertising-API-ClientId` | The client ID associated with your Login with Amazon application. Used for authentication. |
| `Authorization` | Your [access token](guides/account-management/authorization/access-tokens). Used for authentication. |
| `Amazon-Advertising-API-Scope` | The [profile ID](guides/account-management/authorization/profiles) associated with an advertising account in a specific marketplace. Make sure you are using the [correct base URL](reference/api-overview#api-endpoints) for the marketplace associated to the profile. |
| `Content-Type` | Set to `application/vnd.createasyncreportrequest.v3+json`. |

For an introduction to API authentication, see [Authorization overview](guides/account-management/authorization/overview).

## Requesting reports

Version 3 reports support a variety of report types, time periods, and metrics. For more information on what configurations are supported, see [Report types](guides/reporting/v3/report-types).

All reports requests use a single endpoint: `POST /reporting/reports`. See the [API reference](offline-report-prod-3p#tag/Asynchronous-Reports/operation/getAsyncReport) for details on parameters.

### `timeUnit` and supported columns

`timeUnit` can be set to `DAILY` or `SUMMARY`. If you set `timeUnit` to `DAILY`, you should include `date` in your column list. If you set `timeUnit` to `SUMMARY` you can include `startDate` and `endDate` in your column list.

**Example request body: Summary**

```json
{
    "name":"SP campaigns report 7/5-7/10",
    "startDate":"2022-07-05",
    "endDate":"2022-07-10",
    "configuration":{
        "adProduct":"SPONSORED_PRODUCTS",
        "groupBy":["campaign"],
        "columns":["impressions","clicks","cost","campaignId","startDate","endDate"],
        "reportTypeId":"spCampaigns",
        "timeUnit":"SUMMARY",
        "format":"GZIP_JSON"
    }
}
```

**Example request body: Daily**

```json
{
    "name":"SP campaigns report 7/5-7/10",
    "startDate":"2022-07-05",
    "endDate":"2022-07-10",
     "configuration":{
        "adProduct":"SPONSORED_PRODUCTS",
        "groupBy":["campaign"],
        "columns":["impressions","clicks","cost","campaignId","date"],
        "reportTypeId":"spCampaigns",
        "timeUnit":"DAILY",
        "format":"GZIP_JSON"
    }
}
```

### `groupBy` 

All report requests require the `groupBy` parameter in the report configuration. `groupBy` determines the level of the granularity for the report. See [Report types](guides/reporting/v3/report-types) to check what `groupBy` values are available for each report type. If supported for the report type, you can use multiple `groupBy` values in a request. 

### Filters 

Filters are determined at the `groupBy` level of the request. To understand what filters are available, refer to [Report types](guides/reporting/v3/report-types).

>[NOTE] You can only use a filter that is supported by **all** `groupBy` values included in a report configuration.  

**Example request body using filters**

```json
{
    "name":"SP campaigns report 7/5-7/10",
    "startDate":"2022-07-05",
     "endDate":"2022-07-10",
    "configuration":{
        "adProduct":"SPONSORED_PRODUCTS",
        "groupBy":["campaign"],
        "columns":["impressions","clicks","cost","campaignId","startDate","endDate"],
        "reportTypeId":"spCampaigns",
        "timeUnit":"SUMMARY",
        "format":"GZIP_JSON",
         "filters": [
            {
                "field": "campaignStatus",
                "values": ["ENABLED","PAUSED"]
            }
        ]
    }
}
```

### Sample request

This sample shows a request for a summary Sponsored Products campaign report between July 5th and July 21st. 

>[NOTE] The sample calls in this tutorial use the North American API endpoint (`https://advertising-api.amazon.com`). See [API overview](reference/api-overview#api-endpoints) to understand which regional endpoint to use based on your marketplace.  

```shell
curl --location --request POST 'https://advertising-api.amazon.com/reporting/reports' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--data-raw '{
    "name":"SP campaigns report 7/5-7/10",
    "startDate":"2022-07-05",
    "endDate":"2022-07-10",
    "configuration":{
        "adProduct":"SPONSORED_PRODUCTS",
        "groupBy":["campaign","adGroup"],
        "columns":["campaignId","adGroupId","impressions","clicks","cost","purchases1d","purchases7d","purchases14d","purchases30d","startDate","endDate"],
        "reportTypeId":"spCampaigns",
        "timeUnit":"SUMMARY",
        "format":"GZIP_JSON"
    }
}'
```

### Response

A successful request results in a `200` response that includes the requested report configuration, a `reportId`, and a `status`. Make note of the `reportId` for use in the next step.

```json
{
    "configuration": {
        "adProduct": "SPONSORED_PRODUCTS",
        "columns": [
            "campaignId",
            "adGroupId",
            "impressions",
            "clicks",
            "cost",
            "purchases1d",
            "purchases7d",
            "purchases14d",
            "purchases30d",
            "startDate",
            "endDate"
        ],
        "filters": null,
        "format": "GZIP_JSON",
        "groupBy": [
            "campaign", 
            "adGroup"
        ],
        "reportTypeId": "spCampaigns",
        "timeUnit": "SUMMARY"
    },
    "createdAt": "2022-07-19T14:03:00.853Z",
    "endDate": "2022-07-05",
    "failureReason": null,
    "fileSize": null,
    "generatedAt": null,
    "name": "SP campaigns report 7/5-7/21",
    "reportId": "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx",
    "startDate": "2022-07-05",
    "status": "PENDING",
    "updatedAt": "2022-07-19T14:03:00.853Z",
    "url": null,
    "urlExpiresAt": null
}
```

## Checking report status

Once you have made a successful POST call, report generation can take up to three hours. Note that if you make duplicate requests to create a report with identical parameters, you will see a status code of 425, indicating that it’s too soon to make the additional request. If this happens, you should wait for your existing request to complete before making another request. Alternatively, you can explore other reporting options such as [Amazon Marketing Stream](https://advertising.amazon.com/API/docs/en-us/guides/amazon-marketing-stream/overview), which pushes data to partner or advertiser-owned AWS destinations in near real-time, without needing to call the API repeatedly.

You can check the report generation status by using the `reportId` returned in the initial request to call the GET report endpoint: [GET /reporting/reports/{reportId}](offline-report-prod-3p#/Asynchronous%20Reports/getAsyncReport).

**Example request**

```shell
curl --location --request GET 'https://advertising-api.amazon.com/reporting/reports/xxxxx-xxxxx-xxxxx' \
--header 'Content-Type: application/vnd.createasyncreportrequest.v3+json' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxx'  \
```

Any successful call to the GET status endpoint returns a `200` status code. To check if report generation has completed, check the `status` parameter in the response body. If the report is still generating, `status` is set to `PENDING` or `PROCESSING`.

When your report is ready to download, `status` returns as `COMPLETED`, and you will see an address in the `url` field.

>[TIP] Report generation can take as long as three hours. Repeated calls to check report status may generate a 429 response, indicating that your requests have been throttled. To retrieve reports programmatically, your application logic should institute a delay between requests. For more information, see [Retry logic with exponential backoff](reference/concepts/rate-limiting#use-retry-logic-with-exponential-backoff). 

## Downloading reports

Once your report is ready, the `url` field of a `GET reporting/reports/{reportId}` contains a link to the S3 bucket where you can download your report. You can make a GET call using cURL or enter the URL in your browser.  

## Reading your report

Once you decompress your report file, you can see the raw JSON for the report. A sample report for the example used in this tutorial might resemble:

```json

[
    {
        "purchases7d":2,
        "cost":14.5,
        "purchases30d":3,
        "endDate":"2022-07-10",
        "campaignId":158410630682987,
        "clicks":13,
        "purchases1d":1,
        "impressions":2216,
        "adGroupId":72320882861500,
        "startDate":"2022-07-05",
        "purchases14d":3
    },
    {
        "purchases7d":2,
        "cost":9.45,
        "purchases30d":2,
        "endDate":"2022-07-10",
        "campaignId":158410630682987,
        "clicks":10,
        "purchases1d":2,
        "impressions":3721,
        "adGroupId":55720282058882,
        "startDate":"2022-07-05",
        "purchases14d":2
    }
]

```

This example returns impression, click, and cost data for two Sponsored Products campaigns. 

>[TIP]To get campaign metadata, [request an export](guides/exports/overview), then join the two datasets using `campaignId`.

Now that you can generate a report, learn more about:

- [Metrics](guides/reporting/v3/columns)
- [Report types](guides/reporting/v3/report-types) 


