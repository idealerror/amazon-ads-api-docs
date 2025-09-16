---
title: AMC sandbox
description: Working in the sandbox environment
type: guide
interface: api
---
# Working with AMC sandbox

AMC sandbox provides functionalities identical to what's available in AMC advertiser instances, and contains only
synthetic signals generated to mimic the structure and statistical properties of AMC signals, which include Amazon ads signals, and paid features signals from Amazon and third-parties.

> [NOTE] Instances created using the instant self-service APIs will not include a Sandbox instance. For more information, refer to [Instant self-service access to AMC](guides/amazon-marketing-cloud/get-started/instant-access-to-amc).

In addition, sandbox is also integrated with other AMC features, such as:

- [Template analytics (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries/6c6b7ed9b3b9433fd1fb23914444af4b41dd86f1ac5126fbe3326d6ec1f7ae2a)
- [Custom parameters (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries/658a85661c3f46d7ea56d9e25c79ac484ca62ceba2991b8c4cf94337995f0785)
- [Instructional query library (IQL) (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries)
- Data upload and more

> [NOTE] The links above reference AMC Instructional query library (IQL) content available only to users of the AMC UI. If you have an [AMC UI account](https://advertising.amazon.com/solutions/products/amazon-marketing-cloud/), you must log in to access this content.

You can now simulate using AMC for various activities using AMC sandbox, without affecting your live advertiser instances.

AMC sandbox can help you with the following use cases:

- **Hands-on learning**: Use AMC sandbox along with other AMC trainings to experiment features and use cases, without the risk of causing disruptions to live executions.
- **Signal investigation**: Query with Aggregation Controls turned off and inspect non-aggregated reporting in details to inform query understanding and optimization.
- **Query iteration**: Test and optimize queries constructed in sandbox using synthetic datasets that are lesser in volume, and then validate queries developed in sandbox instance in advertiser instances.
- **Paid features exploration**: Explore the schema and potential use cases of AMC paid features (beta) outside of the 30-day trial period.
- **Software development**: Enable software and tool developers with or without existing access to advertiser instances to start building on AMC.

### Limitations of working in a sandbox instance

AMC sandbox is designed to provide datasets that would work for most use cases, but there are some use cases where sandbox will not prove to be an optimal environment for query testing when compared to the advertiser instances. Some examples include: 

- Queries that call a specific campaign or ASIN identifier or other custom parameters.
- Queries that are attempt to retrieve extremely granular conditions as its output. Such details may not exist in the synthetic signals of a sandbox instance.
- Queries that detect highly varied distribution, or outliers that are not built into the sandbox data set.
- Queries that test to see if the compute will fail on a query with a certain time window.

### Signals available in AMC sandbox

We offer a synthetic version of the same signals from an AMC advertiser instance in the sandbox. We are onboarding new signals to sandbox on an ongoing basis and will update this table to reflect the additions. Both Amazon Ads and Subscription signals are readily available in sandbox for you to explore. There is 13 months of backfilled signals available in sandbox and new data will be added on a rolling time window. Sandbox has both Amazon Ads and Paid features available. 

| Data sets                                                           | Available in advertiser instances | Available in sandbox now (Synthetic version\) |
| ------------------------------------------------------------------- | --------------------------------- | --------------------------------------------- |
| dsp\_impressions                                                    | Yes                               | Yes                                           |
| dsp\_clicks                                                         | Yes                               | Yes                                           |
| amazon\_attributed\_events\_by\_conversion\_time                    | Yes                               | Yes                                           |
| amazon\_attributed\_events\_by\_traffic\_time                       | Yes                               | Yes                                           |
| sponsored\_ads\_traffic                                             | Yes                               | Yes                                           |
| dsp\_impressions\_by\_matched\_segments                             | Yes                               | Yes                                           |
| dsp\_views                                                          | Yes                               | Yes                                           |
| dsp\_impressions\_by\_user\_segments                                | Yes                               | Yes                                           |
| conversions                                                         | Yes                               | Yes                                           |
| conversions\_with\_relevance                                        | Yes                               | Yes                                           |
| adserver\_conversions                                               | Yes                               | No                                            |
| adserver\_conversions\_with\_brands                                 | Yes                               | No                                            |
| adserver\_traffic                                                   | Yes                               | No                                            |
| adserver\_traffic\_by\_user\_segments                               | Yes                               | No                                            |
| conversions\_all \(Amazon Flexible Shopping Insights\)              | Yes\(free trial or subscription\) | Yes                                           |
| experian\_vehicle\_purchases \(Experian vehicle purchase insights\) | Yes\(free trial or subscription\) | Yes                                           |
| foursquare\_visits \(Foursquare Store Visit Insights\)              | Yes\(free trial or subscription\) | Yes                                           |
| audience\_segments\_\* \(Audience Segments Insights\)               | Yes\(free trial or subscription\) | No                                            |

For more information regarding these tables, refer to [AMC Data sources](guides/amazon-marketing-cloud/datasources/overview)

## Differences between AMC sandbox signals and AMC advertiser signals

AMC sandbox signals are synthetic, programmatically generated to mimic the structure and statistical properties of AMC signals. Sandbox signals and results may look realistic since they are generated based on the business logic of Amazon Ads data. For example, it has the same logical inter-field and cross-table relationship. It also includes a variety of ad product types, supply sources and devices. However, AMC sandbox data is not tied to any advertiser campaign, customer identifier, or ASIN product.

As such, you should not use the results of sandbox signals to inform media strategy or implementation. You could use it for testing, demonstration, or as an inspiration on the insights you could possibly get from your advertiser signals. 

**Paid features:** This is AMC's premium subscription model, that provides options for deeper analysis of your audiences, campaigns, and outcomes. **We have a synthetic version of these datasets readily available in sandbox for you to explore.** However, the subscription capability is disabled in the sandbox instance. To subscribe to Paid features offerings, log in to the AMC console and access your advertiser instance, navigate to the Paid features tab and subscribe.

### Aggregation controls

In sandbox, aggregation controls are managed by the `disableAggregationControls ` parameter.

You can **disable aggregation controls** to output event-level information of synthetic signals as part of your query outputs. This helps you to inspect the underlying composition of the reports.

> [WARNING] This view is NOT available in the advertiser instances.**

With **aggregation control on,** you can generate aggregated and anonymous report based on synthetic signals. This simulates the advertiser analytic reporting experience of using AMC.

![SandboxAggregation](/_images/amazon-marketing-cloud/amc_sandbox_aggregation.png)

For detailed information, see [Aggregation thresholds](guides/amazon-marketing-cloud/aggregation-threshold).

## Access the AMC sandbox through APIs

To access the AMC sandbox programmatically, complete the following steps:

- **Apply for permission to access the API.** The application form includes questions about how your business intends to use the Amazon Ads API, as well as information about the [Amazon Ads API License Agreement](https://advertising.amazon.com/API/docs/license-agreement), the [Data Protection Policy](https://advertising.amazon.com/API/docs/policy/en_US), and [Amazon Marketing Cloud Terms](https://advertising.amazon.com/marketing-cloud/terms/AMC_Terms.html). Visit [the Amazon Ads Partner page](https://advertising.amazon.com/partner-network/register-api?ref_=a20m_us_api_drctad) for information on how apply for access to the API.
- **Create a Login with Amazon (LwA) application** and submit your application Id (Client identifier) for approval. After approval, the Amazon
  Ads API will handle authentication, authorization, and routing of requests to the appropriate entity or instance. For more information, see the [Create a Login with Amazon application](guides/onboarding/create-lwa-app).
- **Obtain approved access for the Amazon Ads APIs**, with the "Campaign Management"  scope associated with your developer credentials (Client identifier). [Amazon Ads Partner Network account](https://advertising.amazon.com/partners/network). (Or, submit a [Jira](guides/onboarding/data-provider-api) with your request to approve your access with the Data provider scope).
- **Set up your environment with the required [header parameters](guides/amazon-marketing-cloud/get-started/get-started#header-parameters)** required to run the sandbox APIs.

You can now use AMC sandbox programmatically to access synthetic signals, schedule a workflow you created, and execute your workflows to get outputs that you can analyze.

### Parameters to enable access to synthetic data

To enable APIs work in the sandbox environment, the following new parameters must be added to resources as described below:

- The first parameter is named `requireSyntheticData`, and it must be defined as true for all sandbox queries for the POST operations of Schedule or WorkflowExecutions resource.
- The second parameter is `disableAggregationControls`, is specified when creating a schedule or a workflow execution. This parameter cannot be used on non-synthetic data. The values for `disableAggregationControls ` parameter can be set to `true `or `false`.
  - When defined as `true`, you can send queries to see event-level information.
  - When defined as `false` or when unspecified, you can query the sandbox data with all the aggregation controls in place.

## Workflows

Workflows are queries with associated metadata that use synthetic data sources as input and use them to generate a given output (aka "report"). Once defined and saved, a workflow on its own will not produce any output data; you will have to either execute it on demand or schedule it to be run at a specified time.

Workflows are created with the following mandatory body parameters:

- **workflowId**: This is an alphanumeric string that you will specify. Workflow identifiers can only contain alphanumeric characters, periods, dashes, and underscores, without any spaces. This identifier be used to identify the workflow when scheduling or executing on demand.
- **sqlQuery:** The SQL script to use to construct and output your report. The query must be a single-line query and stripped of tabs and new-line delimiters before added to the JSON object. Semicolons are not required at the end of the query.

When creating workflows, the **sqlQuery** parameter for advertiser instances accepts only queries that generate aggregated results. However, in your sandbox environment, your **sqlQuery** accepts non-aggregated queries for execution.

For example, the following query, when executed will return campaign IDs from the dsp_impressions table, sourcing details from the synthetic data sources.

```
SELECT
campaign_id, impressions 
AS impressions
FROM dsp_impressions
```

Know that the above query will fail to run in an advertiser instance, mandating you to aggregate the results. For example, aggregate the result set by the IMPRESSIONS column.

```
SELECT campaign_id,
SUM(impressions) 
AS impressions
FROM dsp_impressions
GROUP BY impressions
```

Now that we know the difference between constructing a query to run in a sandbox environment versus the advertiser environment, let us use the above example to create the body parameters required for a new workflow.

To pass the above query in the `sqlQuery` parameter, remove all line breaks so that the query is a single line query that JSON will be able to support.

Provide a unique name for the workflow you are creating, for example, `my-sandbox-workflow`.

For example,

```
{
"workflowId": "my-sandbox-workflow",
"sqlQuery": "SELECT campaign_id,impressions AS impressions FROM dsp_impressions"
}
```

Now, with the above snippet as the body parameter, use the POST operation of the `workflows ` resource to create a workflow resource with the `instanceId `as the path parameter.

```
POST /amc/reporting/{instanceId}/workflows
```

A successful call returns a HTTP status code: 200 OK. You can use the `workflowId ` to schedule an execution for the workflow or retrieve information on the workflow.

The workflows resource allows you to perform the following operations:

Modify parameters using the PUT operation (along with the unique workflow identifier as the path parameter):

```
PUT /amc/reporting/{instanceId}/workflows/my-sandbox-workflow
```

Retrieve details of a specific workflow using the GET operation:

```
GET /amc/reporting/{instanceId}/workflows/my-sandbox-workflow
```

List all workflows defined in your sandbox operation using the GET operation.

```
GET /amc/reporting/{instanceId}/workflows
```

Delete a specific workflow using the DELETE operation.

```
DELETE /amc/reporting/{instanceId}/workflows/my-sandbox-workflow
```

## Schedules

The Schedule resource allows you to define when a workflow will run. Creating a schedule for a workflow helps define the specificity of the run by allowing you to set the specific time interval, day of the week, and time of day to execute the workflow.

When the `schedule` is triggered, it will run the workflow and place the results in the instance's associated S3 bucket for the given workflow.

Use the POST operation of the schedules resource for creating a schedule for the workflow you created, and include the `requireSyntheticData`to the request body and set the parameter to true to indicate that the schedule is to be created in the sandbox instance. If you want to view event-level data, set disableAggregationControls to `true`. For details on these parameters, see [Parameters to enable access to synthetic data](#parameters-to-enable-access-to-synthetic-data)

```
POST /amc/reporting/{instance_id}/schedules
```

#### Sample request body

```
{
    "scheduleId": "my-sandbox-schedule",
    "workflowId": "my-sandbox-workflow",
    "aggregationHourUtc": 12,
    "aggregationPeriod": "Weekly",
    "aggregationStartDay": "Monday",
    "scheduleEnabled": true,
    "requireSyntheticData": true,
    "disableAggregationControls": true
}

```

The defined schedule will trigger a run of the '`my-sandbox-workflow`' workflow at an eight-hour offset from midnight in UTC every week on Monday. The interval and amount of data contained within the report are the same, so this would generate a report for the data from the previous Monday at the specified offset from UTC until the current Monday at the same hour offset. The schedule will be applied on synthetic data and with `disableAggregationControls `set to true, your query will retrieve event-level information.

In order to determine the correct value of the hour offset, you may use the aggregationHourUtc parameter. Convert your desired start time to Coordinated Universal Time (UTC) and calculate the offset in hours from 12 a.m. midnight UTC. For example, 6 a.m. MST is 12 p.m. UTC, which is 12 hours from midnight UTC. So, to run the SQL query in the
'my-first-workflow' every Tuesday at 6 am, the value for the `aggregationHourUtc` parameter will be 12.

To enable a schedule, set the `scheduleEnabled` parameter to `true`. When this value is set to '`false`' the workflow will not be queued for execution even with an associated schedule.

> [NOTE] If not specified, `aggregationStartDay` will default to the day that the schedule is created in the case of 'Weekly' aggregation time frame. If the "`aggregationPeriod`" is `Daily` then the `aggregationStartDay` field is not used.

A successful call returns a HTTP status code `200 OK` and queues the workflow for execution as per the defined schedule.

For more details on working with the Schedules resource, see [Schedules](guides/amazon-marketing-cloud/reporting/workflow-management-service##schedules)

## Workflow execution

The `WorkflowExecutions ` resource allows you to run a previously saved workflow. The resource also allows you to create a workflow and execute it on demand (ad-hoc workflow executions). For both types of workflows, use the POST operation of the `WorkflowExecutions `resource.

For details, see [Workflow management service.](guides/amazon-marketing-cloud/reporting/workflow-management-service#workflow-executions)

For querying synthetic signals, include the following parameters:

- The first parameter is named `requireSyntheticData`, and it must be defined as true for all sandbox queries.
- The second parameter is `disableAggregationControls`, and is available exclusively in AMC sandbox.

```
POST /amc/reporting/{instanceId}/workflowExecutions
```

The response object contains the created workflow execution which represents the status of the execution.

### Sample query for ad hoc workflow execution

An ad hoc workflow is an [on-demand workflow execution](guides/amazon-marketing-cloud/get-started/make-your-first-call#on-demand-workflow-execution) request that allows you to define the SQL query in the request.

`requireSyntheticData` is always set true for sandbox queries. In the example below, `disableAggregationControls` is set to true, which means users can query and view event-level data, without the aggregation requirements. When `disableAggregationControls` is set to false, users can query the sandbox data with all the aggregation controls in place.

```
{
 "workflow": {
 "workflowId": "analytics-c3dc7b65-ddb7-4b11-af9f-b1409e1b4faf",
 "sqlQuery": "SELECT advertiser,campaign_id,campaign,campaign_start_date,campaign_end_date,impressions AS impressions FROM dsp_impressions",
 },
 "timeWindowStart": "2023-02-07T00:00:00",
 "timeWindowEnd": "2023-02-08T00:00:00",
 "timeWindowTimeZone": "UTC",
 "timeWindowType": "EXPLICIT",
 "requireSyntheticData": true,
 "disableAggregationControls": true, 
}

```

Hit send on the request. A status code of HTTP `200 OK` in the response indicates successful request/response.

> [NOTE] If the settings for executing your workflow in the sandbox environment are not set correctly, the request might still be rejected later during the compilation phase despite a successful response. For example, requireSyntheticData is not set to true, your request may return a response with status code HTTP 200 OK but will finally not compile. You can check the status of a workflow by running a GET request including the workflowExecutionId.

### Viewing your query results

Results of your workflow executions and scheduled workflows are stored in an Amazon S3 bucket tied to your AMC instance.

You can check the Amazon S3 bucket associate with your sandbox instance by going to the AMC UI, and click on the instance info tab. You may also edit the AWS account and the S3 bucket details in the same tab (AMC account admin access required).

For more details on the S3 bucket and accessing your results, see:

[Set up S3 Buckets](guides/amazon-marketing-cloud/get-started/get-started#set-up-s3-buckets)

[Get your results](guides/amazon-marketing-cloud/get-started/get-your-results#view-workflow-output-in-the-amazon-s3-bucket)

### Example queries

Following are some example queries to test AMC using sandbox data. To help you get acquainted with how AMC works, we have crafted queries for various basic use cases in this section.

> [NOTE] For more example queries to help you get started, log in to your [AMC account](https://advertising.amazon.com/solutions/products/amazon-marketing-cloud/) and refer to [Instructional Query: Introduction to AMC sandbox IQ (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries/4c77137c-08b7-4326-9d45-b4f7639d726c). Know that this Instructional Query is only available for users of the AMC UI.

To execute any of the queries listed below:

* Copy the example query you wish to execute. Convert the query to a single-line query and supply it as the input to the sqlQuery parameter to the POST operation of your workflow or workflowExecution resource.

## Basic example queries

**Example query 1**

```
-- Sandbox Example Query: DSP impressions --
SELECT
  advertiser,
  campaign_id,
  campaign,
  campaign_start_date,
  campaign_end_date,
  SUM(impressions) AS impressions
FROM
  dsp_impressions
GROUP BY
  1,
  2,
  3,
  4,
  5

```

**Example query 2**

```
-- Sandbox Example Query: sponsored ads purchases ---
SELECT
  advertiser,
  advertiser_id,
  campaign_id,
  campaign,
  ad_product_type,
  SUM(purchases) AS purchases
FROM
  amazon_attributed_events_by_traffic_time
WHERE
    ad_product_type IS NOT NULL
GROUP BY
  1,
  2,
  3,
  4,
  5

```

**Example query 3**

```
-- Sandbox Example Query: DSP supply sources event level – 
-- UPDATE make sure to disable Aggregation controls (disableAggregationControls to true)when running the following query --  

SELECT
 advertiser_id,
 campaign_id,
 supply_source,
 line_item,
 line_item_id
FROM 
 dsp_impressions 

```

**Example query 4**

```
-- Sandbox Example Query: DSP GEO --
SELECT
 iso_country_code,
 iso_state_province_code,
 dma_code,
 postal_code,
 SUM(impressions) AS impressions,
 COUNT(distinct user_id) AS distinct_user_id
FROM 
 dsp_impressions 
GROUP BY 
  1,
  2,
  3,
  4

```

### Examples that join multiple data sets

**Example query 5**

Refer to [this instructional query (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries/f3368ba2a60ef5e7a24c7951ee84eabdce7de6cc5b739f59a772cfcd87b70837) to learn more about joining impressions and clicks using UNION ALL. Know that this Instructional Query is only available for users of the AMC UI. If you have an [AMC UI account](https://advertising.amazon.com/), you must log in to access this Instructional Query.

> [NOTE] The link above references AMC Instructional Query Library (IQL) content available to only to users of the AMC UI. Log in to your [AMC UI account](https://advertising.amazon.com/solutions/products/amazon-marketing-cloud/) to access this content.

```

-- Instructional Query: Join Impressions and Clicks using UNION ALL --

WITH imp AS (
SELECT
campaign_id,
campaign,
SUM(impressions) AS impressions,
0 AS clicks
FROM
dsp_impressions
GROUP BY
1,
2
),
clicks AS (
SELECT
campaign_id,
campaign,
0 AS impressions,
SUM(clicks) AS clicks
FROM
dsp_clicks
GROUP BY
1,
2
),
combined AS (
SELECT
campaign_id,
campaign,
impressions,
clicks
FROM
imp
UNION ALL
SELECT
campaign_id,
campaign,
impressions,
clicks
FROM
clicks
)
SELECT
campaign_id,
campaign,
SUM(impressions) AS impressions,
SUM(clicks) AS clicks
FROM
combined
GROUP BY
1,
2
```

## Upload advertiser data in sandbox

To help you learn about uploading first-party signals, AMC sandbox supports advertiser first-party signals upload to your sandbox instance. You can then use the uploaded signals to create workflows and develop queries on the joined Amazon ads datasets.

This section will outline the unique features of data upload for sandbox. Overall, sandbox data upload leverages the same process as the data upload for the advertiser instances. For detailed information. Refer to [AMC data upload](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-overview) to learn more.

Sandbox provides customers the ability to join both fact and dimension datasets to sandbox and match them with the sandbox datasets.

For fact datasets, the uploaded signals are resolved at the user-level with the sandbox records at a pre-set match rate of 80%. For example, if you uploaded a file that contains 100 distinct hashed user records, then 80 of these hashed user records would match with sandbox signals.

> [NOTE] The pre-set rate is for illustrative purposes and there is no guarantee of an 80% match rate when joining with the actual Amazon Ads datasets. The match rate depends on various factors, including data quality, recency, and number of attributes used.

Additionally, you have the option to upload files and merge records using sandbox signals based on other dimensions such as ASIN, postal code, campaign name and more. However, **sandbox does not automatically resolve these join keys**. You will need to include those sandbox values in your data file to successfully join with synthetic datasets. The desired match rate can be achieved by appending the appropriate amount of sandbox data to your data file.

### Fact datasets

**Example use case: Off-Amazon conversions using email hashed identifier**

Refer to this [sample file](https://public-file-hosting-repository.s3.amazonaws.com/example_off_amazon_conversions_hashed.xlsx).

> [NOTE] Click [here](https://public-file-hosting-repository.s3.amazonaws.com/example_off_amazon_conversions_hashed.csv) to download the sample file in .csv format.

This file has 500 distinct hashed emails, when you upload this to sandbox, 400 (80%) hashed emails will be joined with sandbox datasets.

This is an example of the API call to create a fact dataset using the example file.

```
POST {api_url}/amc/advertiserData/{instanceId}/dataSets

```

**Sample request body payload**

```
{
  "dataSet": {
    "columns": [
      {
        "name": "hashed_email",
        "description": "User hashed email",
        "dataType": "STRING",
        "columnType": "METRIC",
        "externalUserIdType": {
          "hashedExternalIdentity": {
          "identifierType": "EMAIL"
          }
        }
      },
      {
        "name": "product_name",
        "description": "Name of the product",
        "dataType": "STRING",
        "columnType": "DIMENSION"
      },
      {
          "name": "product_sku",
          "description": "Stock Keeping Unit of the product",
          "dataType": "STRING",
          "columnType": "METRIC"
      },  
      {
      "name": "purchase_quantity",
      "description": "Quantity of the product purchased",
      "dataType": "INTEGER",
      "columnType": "METRIC"
      },
      {
      "name": "purchase_value",
      "description": "Total value of the product purchased",
      "dataType": "DECIMAL",
      "columnType": "METRIC"
      },
      {
      "name": "purchase_time",
      "description": "Timestamp of the purchase",
      "dataType": "TIMESTAMP",
      "columnType": "METRIC"
      }
    ],
    "dataSetId": "off_amazon_conversions_using_email_hashed_ids",
    "description": "My dataset"
  }
}

```

#### Upload the fact dataset

```
POST {api_url}/amc/advertiserData/{instanceId}/uploads/{dataSetId}

```

**Sample request body payload**

```
{
  "updateStrategy": "ADDITIVE",
  "compressionFormat": "NONE",
  "dataSource": {
    "sourceS3Prefix": "hashed_identifier",
    "sourceS3Bucket": "sandbox-data-upload"
    },
  "fileFormat": {
    "csvDataFormat": {
       "fieldDelimiter": ","
      }
  }
}

```

**Sample response**:

The response will include an identifier for the upload (`uploadId`) :

```
{
  "status": "SUBMITTED",
  "uploadId": "b03899ae-726f-44b7-9cf2-5a86a308d489"
}

```

#### Check Upload Status

The `uploadId` (generated in the response of the POST request) can be used to poll the status, as indicated here.

```
GET {api_url}/amc/advertiserData/{instanceId}/uploads/{dataSetId}/{uploadId}
```

A **sample response** is shown below:

```
{
  "upload": {
    "createdAt": "2023-09-29T17:36:32Z",
    "dataSetId": "off_amazon_conversions_using_email_hashed_ids",
    "dataSource": {
        "sourceS3Prefix": "hashed_identifier",
        "sourceS3Bucket": "sandbox-data-upload"
    },
    "instanceId": "amcinstanceid",
    "metrics": {
        "OutputRowCount": 400
    },
    "status": "COMPLETED",
    "updatedAt": "2023-09-29T17:51:30Z",
    "uploadId": "2dd474b2-4029-4ede-b0b7-eafe9ccc275c"
  }
}
```

#### Querying Advertiser Data

Once your data is uploaded to sandbox, you may start querying the joined datasets.

There are some limitations of querying advertiser data that can be bypassed by turning the aggregation controls off in sandbox.
Refer to [AMC data upload](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-overview) to learn more about limitations on querying in the live advertiser instances.

### Dimension datasets

Dimension datasets can be used to upload a static table, or any information which is not time-bound. Some examples include CRM audience lists, campaign metadata, mapping tables, and product metadata (such as a table mapping ASINs to external product names, or sensitive cost-of-goods-sold data). When uploading data to a dimension table, each upload is treated as a full replace – AMC queries will also use data from the last file uploaded. Dimension datasets do not require a main event time (`mainEventTime`) column. AMC will always use the most recent file uploaded (full-replace method of updating data).

In sandbox, the dimension joins are not automatically resolved. You may still upload a dimension file to join with the sandbox datasets by using the following workaround.

1. Get the dimension values from sandbox by running queries like below.
2. Append the sandbox dimension values to your file. You may also adjust the match rate  by using sandbox values for only a part of your file. For example, if you want to test out 50% match rate, append the sandbox values to half of the distinct dimension identifiers.

Example query to identify ASINs from sandbox

```
SELECT tracked_asin
FROM amazon_attributed_conversions_by_conversion_time
GROUP BY 1
UNION
SELECT tracked_asin
FROM amazon_attributed_conversions_by_traffic_time
GROUP BY 1
UNION
SELECT tracked_asin
FROM conversions
GROUP BY 1
UNION
SELECT tracked_asin
FROM conversions_all
GROUP BY
1

```

Example query to identify postal codes from sandbox

```
SELECT postal_code
FROM dsp_impressions
GROUP BY 1

```

Example use case: **ASIN based off-Amazon conversions**
This example file contains the ASIN values from a sandbox dataset.

```
{
  "dataSet": {
    "columns": [
      {
        "name": "product_asin",
        "columnType": "METRIC",
        "description": "ASIN of the product",
        "dataType": "STRING"
      },
      {
        "name": "product_sku",
        "columnType": "METRIC",
        "description": "SKU of the product",
        "dataType": "STRING"
      },
      {
        "name": "purchase_quantity",
        "columnType": "METRIC",
        "description": "Quantity of product purchased",
        "dataType": "INTEGER"
      },
      {
        "name": "purchase_value",
        "columnType": "METRIC",
        "description": "Value of the purchase",
        "dataType": "DECIMAL"
      },
      {
        "name": "coupon_code",
        "columnType": "METRIC",
        "description": "Coupon code used",
        "dataType": "STRING"
      },
      {
        "name": "store",
        "columnType": "METRIC",
        "description": "Store where the purchase was made",
        "dataType": "STRING"
      }
    ],
    "dataSetId": "appended_asin",
    "description": "My dataset"
  }
}

```

#### Upload dimension dataset

```
POST {api_url}/amc/advertiserData/{instanceId}/uploads/{dataSetId}

```

Request body payload

```

{
  "updateStrategy": "FULL_REPLACE",
  "compressionFormat": "NONE",
  "dataSource": {
    "sourceS3Prefix": "appended_asin",
    "sourceS3Bucket": "sandbox-test"
  },
  "fileFormat": {
    "csvDataFormat": {
       "fieldDelimiter": ","
    }   
  }
}

```

The response will include an upload Identifier (`uploadId`):

```
{
  "status": "SUBMITTED",
  "uploadId": "b03899ae-726f-44b7-9cf2-5a86a308d489"
}

```

#### Check Upload Status

The `uploadId` returned from the POST operation can be used to poll the status using the following operation:

```
GET {api_url}/amc/advertiserData/{instanceId}/uploads/{dataSetId}/{uploadID}

```

```
{
  "upload": {
    "createdAt": "2023-09-29T17:36:32Z",
    "dataSetId": "dim_amazon_conversions",
    "dataSource": {
        "sourceS3Prefix": "appended_asin",
        "sourceS3Bucket": "sandbox-test"
    },
    "instanceId": "amcinstanceid",
    "metrics": {
        "OutputRowCount": 400
    },
    "status": "COMPLETED",
    "updatedAt": "2023-09-29T17:51:30Z",
    "uploadId": "2dd474b2-4029-4ede-b0b7-eafe9ccc275c"
  }
}
```

#### Querying Dimension Data

When an upload is performed to a given dimension dataset, the data is available to use for at all times. Regardless of the query date range specified in the AMC UI or in the API, AMC will always use the most recent upload when querying a Dimension dataset. If you need to update a dimension dataset, perform a new upload. The new upload will replace the previous file.
