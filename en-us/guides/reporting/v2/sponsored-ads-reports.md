---
title: Getting started with reports for sponsored ads (version 2)
description: Understand the steps to request and download a report for sponsored ads campaigns using version 2 reporting. 
type: guide
interface: api
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Brands
    - Sponsored Display
---

# Get started with sponsored ads reports

This tutorial outlines how to asynchronously request and download reports for Sponsored Brands, and Sponsored Display campaigns. 

Since Amazon Ads API reports are asynchronous, there are three calls you need to make to generate a report:

1. [Request a report](#requesting-reports)
2. [Wait for report to generate](#checking-report-status)
3. [Download the report](#downloading-reports)

>[TIP:Try it in Postman] Follow along with this tutorial using **Reporting** folder of the Amazon Ads API Postman collection. For set up instructions, see our [Postman tutorial](guides/postman).

>[WARNING] As of March 30, 2023, version 2 reporting does not support Sponsored Products reports. For more information, see [Get started with version 3 reporting](guides/reporting/v3/get-started).

## Headers

Each request described in this tutorial requires four header parameters: 

| Parameter | Description |
|--------------------------|---------------|
| `Amazon-Advertising-API-ClientId` | The client ID associated with your Login with Amazon application. Used for authentication. |
| `Authorization` | Your [access token](guides/account-management/authorization/access-tokens). Used for authentication. |
| `Amazon-Advertising-API-Scope` | The [profile ID](guides/account-management/authorization/profiles) associated with an advertising account in a specific marketplace. |
| `Content-Type` | Set to `application/json`. |

For an introduction to API authentication, see [Authorization overview](guides/account-management/authorization/overview).

## Requesting reports

First, define the report specifications including ad and report type, date, and metrics.

1. Choose an ad type for your report. Each type of sponsored ad has a designated request endpoint, so make sure to use the right endpoint based on the sponsored ad type you need. 

    To get performance data for both Sponsored Brands and Sponsored Display campaigns, make a separate request for each type. 
    
    >[NOTE] There are differences in the request body parameters for each endpoint; check the reference for more information.

    | Ad type | Endpoint | Reference |
    |------------------------|----------------------------|------|
    | Sponsored Products | POST /v2/sp/{recordType}/report | [DEPRECATED]|
    | Sponsored Brands | POST /v2/hsa/{recordType}/report | [Link](sponsored-brands/3-0/openapi#tag/Reports) |
    | Sponsored Display | POST /sd/{recordType}/report | [Link](sponsored-display/3-0/openapi#tag/Reports/operation/requestReport) |
2. Choose a report type. Report type determines how your data is broken down in the report (for example, at the ad group or campaign level). Each sponsored ad type supports different report types. Enter the desired report type into the path in place of `recordType`. 

    Learn more about [Report types](guides/reporting/v2/report-types). 
3. Build your request body. All ad types require `reportDate` and `metrics`. 
    
    - **Report date**: Each report contains a single day's worth of data. For data over a longer time period, make multiple report requests.
    - **Metrics**: Metrics represent the key-value pairs in your report. Each combination of ad type and report type support different metrics. Enter metrics in a comma-separated list with no spaces. Learn more about available [Metrics](guides/reporting/v2/metrics).
    
    >[NOTE] Sponsored Display report requests also require the request body parameter `tactic`, which segments your report data based on the type of targeting (product or audience) used in the campaign. If you run campaigns using both product and audience targeting, make sure you request two versions of each report to receive the full set of data. <br/><br/> For more information, see the [Sponsored Display reporting reference](sponsored-display/3-0/openapi#tag/Reports/operation/requestReport). 
    
    Depending on the type of report you are creating, there may be other optional or required parameters you can include to further segment your report. Check the endpoint reference for all available parameters.
4. To request a report, call the desired POST endpoint (see the table in step 1) with your assembled request body. 

     <details class="details-bar">
        <summary><strong>Example</strong></summary>

    <p>The code snippet below requests a Sponsored Display report at the campaign level. The report will include each sponsored products campaign along with the associated impressions, clicks, and cost for November 2, 2021.</p>

    <p>If you are copying the example request, make sure to use the correct URL for your geography and valid authentication credentials for your account.</p>

    ```shell
    curl --request POST 'https://advertising-api.amazon.com/v2/sd/campaigns/report' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer xxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxx' \
    --data-raw '{
    "reportDate": "20211102",
    "metrics": "campaignId,impressions,clicks,cost"
    }'
    ```
    </details>
5. Successful calls result in a `202` response that includes a `reportId` and a `status` of `IN_PROGRESS`. Take note of the `reportId` for use in the next step. 

    A successful response body should resemble:

    ```
    {
        "reportId":"amzn1.clicksAPI.v1.t1.xxxxxxxxxxxxx",
        "recordType":"campaign",
        "status":"IN_PROGRESS",
        "statusDetails":"Report is being generated."
    }
    ```

## Checking report status

Once you have made a successful POST call, report generation can take up to 15 minutes. 

You can check the report generation status by using the `reportId` returned in step 4 to call the GET report endpoint.

> GET [/v2/reports/{reportId}](sponsored-display/3-0/openapi#tag/Reports/operation/requestReport)

 <details class="details-bar">
    <summary><strong>Example request</strong></summary>

    <p>If you are copying the example request, make sure to use the correct URL for your geography, valid authentication credentials for your account, and your unique report ID.</p>

    ```shell
    curl --request GET 'https://advertising-api.amazon.com/v2/reports/amzn1.clicksAPI.xxxxxx' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer Atza|xxxxxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
    ```
</details>

Any successful call to the GET status endpoint returns a `200` status code. To check if report generation has completed, check the `status` parameter in the response body. If the report is still generating, `status` is set to `IN_PROGRESS`:

```
{
    "reportId":"amzn1.xxxxxxxxxx",
    "status":"IN_PROGRESS",
    "statusDetails":"Report generation is in progress."
}
```

When your report is ready to download, the `status` returns as `COMPLETE`:

```
{
    "reportId":"amzn1.xxxxxxxxxx",
    "status":"SUCCESS",
    "statusDetails":"Report has been successfully generated.",
    "location":"https://ad-api.integ.amazon.com/v1/reports/amzn1.xxxxxxxxxx/download",
    "fileSize":150,
    "expiration":1651536000000
}
```

>[TIP] Report generation can take as long as 15 minutes. Repeated calls to check report status may generate a 429 response, indicating that your requests have been throttled. To retrieve reports programmatically, your application logic should institute a delay between requests. For more information, see [Retry logic with exponential backoff](reference/concepts/rate-limiting#use-retry-logic-with-exponential-backoff). 

## Downloading reports

Once your report is ready, use the `reportId` to call the download endpoint:

> GET [/v2/reports/{reportId}/download](sponsored-display/3-0/openapi#tag/Reports/operation/getReportStatus)

A successful call returns a `307` redirect response. The redirect link points you to the S3 bucket where you can download your report file. The report file downloads as JSON compressed in gzip format. 

 <details class="details-bar">
    <summary><strong>Example request</strong></summary>

    <p>If you are copying the example request, make sure to use the correct URL for your geography, valid authentication credentials for your account, and your unique report ID.</p>

    <p>If using a cURL request, you should use the `--output` flag to specify a file for your report data.</p>

    ```shell
    curl --request GET 'https://advertising-api.amazon.com/v2/reports/amzn1.clicksAPI.xxxxxx/download' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer Atza|xxxxxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
    --output "myReport.json"
    ```
</details>

## Reading your report

Once you decompress your report file, you can see the raw JSON for the report. A sample report for the example used in this tutorial might resemble:

```json
[
    {
        "campaignId":123456789,
        "impressions":55,
        "clicks":2,
        "cost":3.5
    },
    {
        "campaignId":123456780,
        "impressions":32,
        "clicks":3,
        "cost":5.3
    }
]
```

This example returns impression, click, and cost data for two Sponsored Display campaigns. 

>[TIP]To get campaign metadata, [request an export](guides/exports/overview), then join the two datasets using `campaignId`.

Now that you can generate a report, learn more about:

- [Metrics](guides/reporting/v2/metrics)
- [Reporting differences between the API and advertising console](guides/reporting/v2/advertising-console)
- [Report types](guides/reporting/v2/report-types) 


