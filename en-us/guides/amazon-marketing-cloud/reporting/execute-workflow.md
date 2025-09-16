---
title: Workflow management service
description: How to create and manage workflows in AMC
type: guide
interface: api
---
# Manage workflow executions

The [WorkflowExecutions](amc-reporting#tag/Workflow-executions) resource allows you to run a previously saved workflow. The resource also allows you to create a workflow and execute it on demand ([ad hoc workflow execution](guides/amazon-marketing-cloud/reporting/ad-hoc-workflow)).

When executing an ad hoc workflow, define the SQL query in the body of the request and include the additional parameters to output your results.

For both types of workflows, use the POST operation of the `WorkflowExecutions` resource.

```
POST /amc/reporting/{instanceId}/workflowExecutions
```

The response object contains the created workflow execution which represents the status of the execution.

| [TILES] Did you know ?                                                                                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Before a workflow execution runs, AMC checks the query against previously run executions. AMC will not return results if you have run multiple, similar queries (those with slightly different filtering expressions) on non-sensitive fields to obtain information associated with a sensitive value. |

 If too many executions have already been performed against overlapping input records, the execution is rejected with the following message:

`Execution "X" of workflow "Y" could not be run because too many workflows have already been run against the same input data sources in the same or overlapping time windows.`

This is done to protect the identity of the audience information within the datasets you query. 

## Execute a saved workflow

To execute a previously saved workflow, you will need to pass the instance identifier as a mandatory path parameter. You will include the workflow id as part of the body of the request. You can also include some optional parameters as part of the body of the request to further customize the report your query/workflow outputs and define dates for the query to consider as the time window. Then use the following endpoint to execute your saved workflow:

```
POST /amc/reporting/{instanceId}/workflowExecutions
```

Before you execute your workflow, understand the following optional parameters you can pass in your workflow execution request that will shape your output.

- [timeWindowType](#time-window-type)
- [Data freshness](#data-freshness)

### Time window type

The type of time window to use to for specifying input data for the workflow execution. If not provided, the time window type of `MOST_RECENT_DAY` will be used. Valid options are:

- **`EXPLICIT`**: The start and end of the time window must be explicitly provided in the request.
- **`MOST_RECENT_DAY`**: The time window will be the most recent 1-day window for which the instance is likely to have data, aligned to day boundaries.
- **`MOST_RECENT_WEEK`**: The time window will be the most recent 1-week window for which the instance is likely to have data, aligned to day boundaries.
- **`CURRENT_MONTH`**: The time window will be the start of the current month up to the most recent time for which the instance is likely to have data.
- **`PREVIOUS_MONTH`**: The time window will be the entire previous month.

### Data freshness

AMC does not ingest data at real-time and propagates your ad activity data after a time-lag. As a best practice, AMC recommends that you select a date range of **48-hours prior to the current date**. This is because, the result of your query may be incomplete as the data can take up to 48 hours to process and be available in your instances.

**Sample request**

In our sample request, we will execute the `my-first-workflow` workflow we created when we discussed how to [create a workflow](guides/amazon-marketing-cloud/reporting/create-workflow). Notice how we reference the workflow using the `workflowID` and set the `timeWindowType` to `EXPLICIT`. Since the `timeWindowType` is set to `EXPLICIT`, we provide the `timeWindowStart` and `timeWindowEnd` values.

```
{
    "workflowId":"my-first-workflow",
    "timeWindowStart": "2023-05-20T00:00:00",
    "timeWindowEnd": "2023-08-03T00:00:00",
    "timeWindowType": "EXPLICIT",
    "timeWindowTimeZone": "America/New_York",
  
}
```

After you run the workflow, AMC returns a workflow execution Id, (`WorkflowExecutionId`) that you will need to retrieve from the results.

**Sample response**

```
{
    "createTime": "2023-09-18T22:01:56Z",
    "dataGaps": [],
    "disableAggregationControls": false,
    "executionCategory": "AD_HOC",
    "expireTime": "2023-09-19T22:01:56Z",
    "lastUpdatedTime": "2023-09-18T22:01:56Z",
    "parameterValues": {},
    "status": "PENDING",
    "timeWindowEnd": "2022-09-30T04:00:00Z",
    "timeWindowEndOriginal": "2022-09-30T00:00:00",
    "timeWindowStart": "2022-09-20T04:00:00Z",
    "timeWindowStartOriginal": "2022-09-20T00:00:00",
    "timeWindowTimeZoneOriginal": "America/New_York",
    "waitUntil": "2023-09-18T21:01:56Z",
    "workflowExecutionId": "2b04762b-cbdb-47c7-be39-42a585ecd40e",
    "workflowExecutionTimeoutSeconds": 21600,
    "workflowId": "my-first-workflow"
}

```

You can [append aggregation threshold columns](guides/amazon-marketing-cloud/reporting/create-workflow#append-aggregation-threshold-columns) when defining the workflow execution.

To understand each of the response parameters, see the [Workflow Execution API reference.](amc-reporting#tag/Workflow-executions)

### Query execution limit

An AMC account has no limit on the active workflow executions (that is, workflow executions with the status of running or pending ).

However, at an instance level, the following limits are applied:

* 10 workflow executions can run in parallel at the instance level for API
* 200 workflow executions can run in parallel at the account level for API

Note that, at the **account level**, there is no limit to the number of workflow executions queued. For example, at any given point, an account can run a maximum a maximum of 200 workflow executions running concurrently from the multiple instances associated with it, and have then any number of workflow executions from instances queued for processing.

> [NOTE] In case you reach the limit for execution, you can choose to either wait for running or queued workflow executions to complete, or to use the `DELETE /workflowExecutions/{workflowExecutionId}` endpoint to clear their queue.

## View a workflow execution

Gets status information about the requested workflow execution.

```
GET/amc/reporting/{instanceId}/workflowExecutions/{workflowExecutionId}
```

**Possible workflow execution statuses**

| Status Name | Description                                                                                                                                             |
| :---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| PENDING     | The workflow execution has not yet started running. It may be queueing to run, waiting for data to become available, or waiting until a specified time. |
| RUNNING     | The workflow execution is running.                                                                                                                      |
| SUCCEEDED   | The workflow execution has finished successfully.                                                                                                       |
| FAILED      | The workflow execution failed.                                                                                                                          |
| CANCELLED   | The workflow execution was cancelled before succeeding or failing.                                                                                      |

For details on the response parameters, see the response of [POST operation of the workflowExecutions.](amc-reporting#tag/Workflow-executions/operation/createWorkflowExecution)

## View workflow executions

Returns workflow execution details for a specific workflow identifier.

```
GET /amc/reporting/{instanceId}/workflowExecutions/
```

The other parameter you can use to list workflow executions is the `minCreationTime`, which is a date filter based on the time that the workflow execution was created. It is in the yyyy-MM-dd'T'HH:mm:ss format. When the `minCreationTime` is specified, the response returns all workflow executions created after the specified time window.

```
GET /amc/reporting/{instanceId}/workflowExecutions?minCreationTime=2023-10-01T00:00:00
```

## Update a workflow execution

The PUT operation of the workflowExecutions resource updates the values in a workflow execution definition. It uses the same structure as the POST workflowExecutions request.

To update a workflow execution definition, retrieve the specific workflow execution identifier. Next, provide the updated values for the parameters you want to modify. Then, use the PUT operation of the workflowExecutions resource to save the changes.

```
PUT /amc/reporting/{instanceId}/workflowExecutions/{workflowExecutionId}
```

Updates the requested workflow execution.

## Delete a workflow execution definition

To delete a workflow execution definition within an instance, use the DELETE operation of the workflowExecutions resources. Pass the instance identifier and workflow executions identifier as path parameters.

```
DELETE/amc/reporting/{instanceId}/workflowExecutions/{workflowExecutionId}
```

Deletes the requested workflow execution.

## Next steps

* [Get your workflow results](guides/amazon-marketing-cloud/reporting/get-your-results)
