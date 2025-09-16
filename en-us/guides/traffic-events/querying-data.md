---
title: Querying data in the traffic events API data
description: How to use SQL to query data in the traffic events API.
type: guide
interface: api
tags:
    - Reporting
keywords:
    - SQL query fields
    - clean room
    - workflow
---

# Querying data with the traffic events API

Traffic events API provides you with the ability to execute SQL Queries, referred to as "Workflow Executions".

## Workflow executions

The Workflow is a SQL query that is mapped to an identifier (workflowId) that you provide. This workflowId is defined by you and can include numbers and letters (e.g. 'StreamingTVPerformanceReport2023'). A workflow execution is an instance of the workflow that you will run to produce the report.

### Create a workflow execution

To execute a workflow, you will need to pass the workflow definition as a mandatory parameter to the Create Workflow Execution API. You can also include some optional parameters as part of the body of the request to further customize the report of your query/workflow outputs.

>[WARNING]Before you execute your workflow, make sure to understand the optional parameters you can pass in your workflow execution request that will shape your output.

For this tutorial, to create a workflow execution use the POST operation of the workflowexecutions resource.

```
POST /avc/workflowexecutions
```

#### Mandatory parameters

The workflow definition that you include in your request has the following mandatory parameters:

* **workflowId**: This is an alphanumeric string that you will specify. Workflow identifiers can only contain alphanumeric characters, periods, dashes, and underscores, without any spaces.
* **sqlQuery**: The SQL script to use to construct and output your report. The query must be a single-line query and stripped of tabs and new-line delimiters before added to the JSON object. Semicolons are not required at the end of the query.

##### SQL query

Let's assume you want to create workflow execution with a basic query and include unique name that you will refer to in order to identify the workflow later.
We'll construct the workflow with following SQL query that will return the total number of impressions, clicks, and viewable impressions per campaign.

```
SELECT 
campaign,
sum(impressions) as impressions, 
sum(clicks) as clicks, 
sum(viewable_impressions) as viewable_impressions 
FROM sponsored_ads_traffic
GROUP BY campaign
```

To pass the above query in the `sqlQuery` parameter, remove all line breaks so that the query is a single line query that JSON will be able to support.

We will name this workflow `my-first-workflow`.

In this example, we will not include the optional `timeWindow` parameter, so the default time window of `MOST_RECENT_DAY` will be used.

In our example, we will append aggregation threshold columns as additional body parameters.

**Sample request**

```bash
curl --location
'https://advertising-api.amazon.com/avc/workflowexecutions'\

--header 'Authorization: Bearer XXXXX' \
-header 'Amazon-Advertising-API-ClientID:
amzn1.application-oa2-client.XXXXX' \
--header 'Amazon-Advertising-API-Entity-Id: XXXXX' \
--header \'Amazon-Advertising-API-MarketplaceId: ATVPDKIKX0DER' \
--data '{
    "workflow": {
        "filteredMetricsDiscriminatorColumn": "filtered",
        "sqlQuery": "SELECT 
            campaign,
            sum(impressions) as impressions, 
            sum(clicks) as clicks, 
            sum(viewable_impressions) as viewable_impressions 
            FROM sponsored_ads_traffic
            GROUP BY campaign",
        "workflowId": "my-first-workflow"
    }
}'
```

The result of running this workflow will output the columns `campaign`, `impressions`, `clicks`, and `viewable_impressions`, listing the names of the campaigns with a total impressions, clicks, and viewable impressions for each campaign in the last day.

A successful call returns a HTTP status code: `200 OK`. You can use the `workflowExecutionId`to schedule retrieve information on the workflow execution and to download the resulting report once the workflow execution has entered a 'SUCCEEDED' state.

##### Append aggregation threshold columns

Different fields (columns) in the traffic events API have different requirements in terms of minimum user granularity counts container in the aggregated query results.

After you define your basic SQL query, the workflows resource also accepts parameters to further qualify the output of your query. Column outputs are classified into categories based on the combination of the aggregation threshold level and the filtering restrictions, which we refer to as [aggregation threshold](guides/traffic-events/data-aggregation-thresholds). When defining your workflow, you can append these aggregation threshold columns with the help of the `filteredMetricsDiscriminatorColumn` parameter.

###### Filtered output

By default, rows that do not meet the minimum distinct user count requirements will be completely filtered out of the workflow output.

If the '**filteredMetricsDiscriminatorColumn'** parameter is included when defining workflow, then rows that not meet the minimum distinct user count requirements will have values removed and then the output will be re-aggregated with AggregationType.SUM.
This value you provide for the filteredMetricsDiscriminatorColumn parameter becomes the heading of the Boolean column that will be added to the workflow output. This Boolean column identifies rows with and without values in columns removed. Unchanged rows will have a value of FALSE for this column, while rows with fields removed will have a value of TRUE.

This field is entered in the workflow definition as a top-level field:

```json
{
    "workflowId": "DisplayImpressionsByAd",
    "filteredMetricsDiscriminatorColumn": "filtered",
    "sqlQuery": 
    {
    ...
  
    }
}

```

In the above example, the workflow will output a column titled "filtered" that will contain Boolean values.

**Example: Filtering behavior with and without filteredMetricsDiscriminatorColumn**

Example Raw Data

|impressions (metric) (NONE)	|clicks (metric) NONE	|event_date (dimension) (LOW)	|impressions distinct user count	|clicks distinct user count	|
|---	|---	|---	|---	|---	|
|5	|1	|10/1/2023	|5	|1	|
|10	|2	|10/2/2023	|7	|2	|
|4	|2	|10/3/2023	|1	|1	|
|5	|1	|10/4/2023	|1	|1	|

**After Executing Workflow without filteredMetricsDiscriminatorColumn**

The three rows where the distinct user count of either impressions, clicks, or both is less than 2 are omitted from the output.

|impressions (metric) (NONE)	|clicks (metric) NONE	|event_date (dimension) (LOW)	|
|---	|---	|---	|
|10	|2	|10/2/2023	|

**After Executing Workflow with filteredMetricsDiscriminatorColumn of "filtered"**

The three rows where the distinct user count of either impressions, clicks, or both is less than 2 are included in the output.
For these rows, columns with an aggregation threshold of less than 2 have values of NULL and the filteredMetricsDiscriminatorColumn, "filtered" is set to TRUE.
The first, third and fourth rows of the original table are re-aggregated into a single row after the filtering.

|impressions (metric) (NONE)	|clicks (metric) NONE	|event_date (dimension) (LOW)	|Filtered (dimension) NONE	|
|---	|---	|---	|---	|
|10	|2	|10/2/2023	|FALSE	|
|14	|4	|NULL	|TRUE	|

#### Defining the time window

The timeWindow parameter of the workflow execution controls the date range over which you wish to query data. The timeWindow object has the following parameters:

- **`type`**: The time window type. See [Time Window Type](#time-window-type).
- **`timezone`**: The timezone for the time window. Default is UTC. The timezone should be in the timezone identifier form, for example `America/Los_Angeles`. For a full list of supported timezones see [List of timezones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)
- **`start`**: The start date time for the timewindow. Only use this when using a timeWindow type of `EXPLICIT`.
- **`end`**: The end date time for the timewindow. Only use this when using a timeWindow type of `EXPLICIT`.

##### Time Window Type

The type of time window to use to for specifying input data for the workflow execution. If not provided, the time window type of `MOST_RECENT_DAY` will be used.

- **`EXPLICIT`**: The start and end of the time window must be explicitly provided in the request.
- **`MOST_RECENT_HOUR`**: The time window will be the most recent hour for which the system has data.
- **`MOST_RECENT_24_HOURS`**: The time window will be the most recent 24 hour period for which the system has data.
- **`MOST_RECENT_DAY`**: The time window will be the most recent 1-day window for which the instance is likely to have data, aligned to day boundaries.
- **`MOST_RECENT_WEEK`**: The time window will be the most recent 1-week window for which the instance is likely to have data, aligned to day boundaries.
- **`CURRENT_MONTH`**: The time window will be the start of the current month up to the most recent time for which the instance is likely to have data.
- **`PREVIOUS_MONTH`**: The time window will be the entire previous month.

**Sample Request** 

In this sample request, we will use the same workflow definition as we did previously, but now we will include a timeWindow parameter to allow us to control the date range of the query.

```json
    "workflow": {
        "filteredMetricsDiscriminatorColumn": "filtered",
        "sqlQuery": "SELECT 
            campaign,
            sum(impressions) as impressions, 
            sum(clicks) as clicks, 
            sum(viewable_impressions) as viewable_impressions 
            FROM sponsored_ads_traffic
            GROUP BY campaign",
        "workflowId": "my-first-workflow"
    },
    "timeWindow": {
        "type": "EXPLICIT",
        "start": "2023-10-01T00:00:00",
        "end": "2023-10-31T00:00:00",
        "timezone": "America/Los_Angeles"
    }
```

After you run the workflow execution, the traffic events API returns a workflow execution ID, (workflowExecutionId) that you will need to retrieve from the results.

**Sample response**

```json
{
    "createTime": "2023-09-18T22:01:56Z",
    "dataGaps": [],
    "disableAggregationControls": false,
    "executionCategory": "AD_HOC",
    "expireTime": "2023-11-19T22:01:56Z",
    "ignoreDataGaps": true,
    "lastUpdatedTime": "2023-11-18T22:01:56Z",
    "parameterValues": {},
    "status": "PENDING",
    "timeWindowEnd": "2022-10-31T04:00:00Z",
    "timeWindowEndOriginal": "2022-10-31T00:00:00",
    "timeWindowStart": "2022-10-01T04:00:00Z",
    "timeWindowStartOriginal": "2022-10-01T00:00:00",
    "timeWindowTimeZoneOriginal": "America/Los_Angeles",
    "workflowExecutionId": "2b04762b-cbdb-47c7-be39-42a585ecd40e",
    "workflowExecutionTimeoutSeconds": 21600,
   "workflowId": " my-first-workflow "
}
```

##### Query execution limit

The traffic events API has a limit of 5 active query executions (that is, query executions with the status of running or pending). Any requests to create an execution over this limit will result in an HTTP 422 Service Limit Exceeded Error. For example, if there are 5 active executions, a request to create the 6th execution will throw an HTTP 422 status code with the following error message:

```
You have reached your execution limit of 5 workflowExecutions that are in PENDING or RUNNING status. Try again after one or some of your execution completes.
```

### View a workflow execution

#### Get status information about the requested workflow execution.

```
GET /avc/workflowexecutions/{workflowExecutionId}
```

**Possible workflow execution statuses**

|Status Name	|Description	|
|---	|---	|
|PENDING	|The workflow execution has not yet started running. It may be queueing to run, waiting for data to become available, or waiting until a specified time.	|
|RUNNING	|The workflow execution is running.	|
|SUCCEEDED	|The workflow execution has finished successfully.	|
|FAILED	|The workflow execution failed.	|

For details on the response parameters, see response of [POST operation of the workflowExecutions](#create-workflow-execution)

### Download your report

To retrieve a pre-signed URL for downloading workflow execution results from S3, copy the workflowExecutionId from the response returned in the POST operation of the workflowExecutions resource.

In our example, the workflowExecutionId is: `2b04762b-cbdb-47c7-be39-42a585ecd40e`.
Now, pass the instance identifier and the workflowexecution identifier you retrieved as path parameters in the following request:

```
GET /avc/workflowexecutions/{workflowExecutionId}/downloadUrls
```

This request will return a URL where your report can be directly downloaded. Copy the URL and enter it into a browser to download the resulting CSV file.
It may take some time for the report to generate. Wait until the workflow execution is in a 'SUCCEEDED' status to download your report. If your report is not ready, you will receive a HTTP 404 error stating that the download url is not found.

### View workflow executions

Returns workflow execution details for a specific workflow execution identifier.

```
GET /avc/workflowexecutions/6207c879-1bfd-492f-bfb5-41300324a40c
```

### View all workflow executions

In addition to being able to query a single workflow execution to retreive its status, you can list your previously created workflow executions using the ListWorkflowExecutions API.

```
POST /avc/workflowexecutions/6207c879-1bfd-492f-bfb5-41300324a40c
```

The body of the List Workflow Executions call must contain either the `workflowId` or the `minCreationTime` in order to filter the resulting list. When using a `workflowId` this API will return all workflow executions where the workflowId matches your input. The `minCreationTime`, which is a date filter based on the time that the workflow execution was created. It is in the yyyy-MM-dd'T'HH:mm:ss format. When the `minCreationTime` is specified, the response returns all workflow executions created after the specified time window.
