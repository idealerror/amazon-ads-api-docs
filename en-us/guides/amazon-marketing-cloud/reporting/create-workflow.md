---
title: Workflow management service
description: How to create and manage workflows in AMC
type: guide
interface: api
---


# Manage a workflow

Workflows are queries with associated metadata that take previously defined data sources as input and use them to generate a given output (aka "report"). 
The Workflow resource is essentially a SQL query that is mapped to a unique identifier (`workflowId`) that you provide when defining the workflow. When you define your `workflowId`, know that the id must comprise only alphanumeric characters, numbers, with periods, dashes, and underscores being the only special characters allowed (e.g. 'Streaming_T.V_Performance-Report2023')

You can reference the workflow using the `workflowId` for reuse, or for execution on-demand, or schedule it to be run at a specified time.



## Create a workflow

The [AMC Workflows API](amc-reporting#tag/Workflows) helps you create and manage workflows programmatically. Once defined and saved, a workflow on its own will not produce any output data; you will have to either execute it on-demand or schedule it to be run at a specified time. To run at a specific time, use the [Schedules](amc-reporting#tag/Schedules) resources to run the query via the AMC built-in scheduler or on-demand through the [Workflow Execution](amc-reporting#tag/Workflow-executions) resources.


> [WARNING] When defining a workflow, it is mandatory to adhere to specific aggregation requirements to preserve our retail customers\' privacy. See [aggregation thresholds](guides/amazon-marketing-cloud/aggregation-threshold) and [Append aggregation threshold columns](#append-aggregation-threshold-columns) for more details.


For this tutorial, to create a workflow, use the POST operation of the workflows resource with the `instanceId` as the path parameter.

```
POST /amc/reporting/{instanceId}/workflows
```

Workflows are created with the following mandatory request body parameters:

* **`workflowId`**: This is an alphanumeric string that you will specify. Workflow identifiers can only contain alphanumeric characters, periods, dashes, and underscores, without any spaces. This identifier will be used to identify the workflow when scheduling or executing on-demand.
* **`sqlQuery`**: The SQL script to use to construct and output your report. The query must be a single-line query and stripped of tabs and new-line delimiters before adding to the JSON object. Semicolons are not required at the end of the query.

Let's assume, you want to create a workflow with a basic query and save it with a unique name that you will refer to when scheduling or executing it.

We'll construct the workflow with the following SQL query that we will return total purchases with a 9-day conversion window.

```
SELECT
campaign,
SUM(total_purchases) AS total_orders_9d
FROM
amazon_attributed_events_by_traffic_time
WHERE
SECONDS_BETWEEN(traffic_event_dt_utc, conversion_event_dt_utc) \<= 60 \*
60 \* 24 \* 9
GROUP BY
Campaign
```

To pass the above query in the `sqlQuery` parameter, remove all line breaks so that the query is a single line query that JSON will be able to support.

We will name this workflow `my-first-workflow` and pass this name as the unique `workflowId`.

In our example, we will [append aggregation threshold columns](#append-aggregation-threshold-columns) as additional request body parameters.

**Sample request**

```
curl --location
'https://advertising-api.amazon.com/amc/reporting/{instanceId}/workflows'\

--header 'Authorization: Bearer XXXXX' \
--header 'Amazon-Advertising-API-ClientID:amzn1.application-oa2-client.XXXXX' \
--header 'Amazon-Advertising-API-AdvertiserId: XXXXX' \
--header 'Amazon-Advertising-API-MarketplaceId: ATVPDKIKX0DER' \
--header 'Content-Type: application/json' \
--data '{
"distinctUserCountColumn": "du_count",
"filteredMetricsDiscriminatorColumn": "filtered",
"filteredReasonColumn": "true",
"sqlQuery": "SELECT campaign, SUM(total_purchases) AS total_orders_9d
FROM amazon_attributed_events_by_traffic_time WHERE
SECONDS_BETWEEN(traffic_event_dt_utc, conversion_event_dt_utc) <= 60 *
60 * 24 * 9 GROUP BY campaign",
workflowId": "my-first-workflow"
},'
```

A successful call returns an HTTP status code: `200 OK`. You can use the `workflowId `to schedule an execution or retrieve information on the workflow.

You can go ahead and schedule the workflow for a timed execution, or call the POST operation of the `workflowExecutions` resource to execute the workflow.

The result of running this workflow will output the columns `campaign`, and `total_orders_9d`, listing the names of the campaigns with a total purchases with a 9-day conversion window.

> [NOTE] For more use cases and SQL queries, visit the Instructional queries tab in the AMC console,  

|[TILES] Did you know?|
|--|
|The Instructional queries tab in the AMC console is home to the instructional query library (IQL), where you can explore through Amazon-authored SQL queries, also known as instructional queries (IQs), designed by our experts, to help you leverage common measurement and analytics use cases. To learn more, visit [Instructional query library (IQL)](https://advertising.amazon.com/marketing-cloud/instructional-queries) (link only available for AMC UI users). |


### Include custom parameters

The AMC workflows API supports the ability to create [custom parameters (link only available for AMC UI users)](https://advertising.amazon.com/marketing-cloud/instructional-queries/658a85661c3f46d7ea56d9e25c79ac484ca62ceba2991b8c4cf94337995f0785), then allows you to pass values to these parameters at execution. This provides the ability to streamline query execution by reducing the values that need to be hard-coded in the query, and by providing you the ability to inject metadata.

**Supported simple data types**

- BINARY
- BOOLEAN
- BYTE
- DATE
- DOUBLE
- FLOAT
- INTEGER
- LONG
- SHORT
- STRING
- TIMESTAMP

**Supported complex data types**

- ARRAY of one of the supported simple data types above

> [NOTE] Never add any information about an individual person, such as an email address or phone number, to the description or provide as the value to a parameter.


#### Step 1: Creating custom parameters in workflows

In the workflow, you define what parameters can be referenced in the query, and you can optionally supply a default value (`defaultValue`) for the parameter. If you do not specify the `defaultValue`, you must set the value later in the workflow executions, as described in **Step 2: Invoking custom parameters in workflow executions**.

**Sample request**

The following example defines three custom parameters on the workflow, each of a different data type:

```
{
    "workflowId": "workflowWithCustomParams",
    "sqlQuery": "Insert SQL query",
    "inputParameters": 
    [
        {
            "name": "myCustomParameter1",
            "dataType": "INTEGER",
            "columnType": "DIMENSION",
            "defaultValue": 20,
            "description": "Enter description for parameter"
        },
      
        {
            "name": "myCustomParameter2",
            "dataType": "STRING",
            "columnType": "DIMENSION",
            "defaultValue": "4",
            "description": "Enter description for parameter"
        },

        {
            "name": "myCustomParameter3",
            "dataType": {
                "elementDataType": "STRING",
                "type": "ARRAY"
                },
            "column_type": "DIMENSION",
            "description": "Enter description for parameter",
            "defaultValue": [
                "11111111111111111",
                "22222222222222222",
                "33333333333333333"
                ]
        }
    ]
}
```

#### Step 2: Invoking custom parameters in workflow executions

One defined in a workflow, the custom parameters can then be invoked in workflow executions.

When you run the workflow execution, you can optionally override that `defaultValue` with a different value (for example, if you want to run the same query but with a different parameter). In the workflow execution, you can change the values using `parameterValues`, which is a map from the custom parameter's `name` to the new value. For example, you could define the following in the `/workflowExecutions` request:

```
{
  "workflowId": "workflowWithCustomParams",
  ...
  "parameterValues": {
    "myCustomParameter1": 9001,
    "myCustomParameter2": "A different string than the default value"
  },
  ...
}
```

Then, when the execution runs, you reference the custom parameter using:

```
CUSTOM_PARAMETER('myCustomParameter1')
```

During the workflow execution, the custom parameter will use the `parameterValues` definition first (for example, myCustomParameter1 will use 9001). If not specified, it will use the `defaultValue` from the workflow definition (for example, the array myCustomParameter3 will use the default values defined in the workflow).


### Append aggregation threshold columns

Different fields (columns) in AMC have different requirements in terms of minimum user granularity counts container in the aggregated query results.

After you define your basic SQL query, the workflows resource also accepts parameters to further qualify the output of your query. Column outputs are classified into categories based on the combination of the [aggregation threshold level](guides/amazon-marketing-cloud/aggregation-threshold) and the filtering restrictions, which we refer to as [**aggregation threshold**](#append-aggregation-threshold-columns). When defining your workflow, you can append these aggregation threshold columns with the help of the following parameters:

- [filteredMetricsDiscriminatorColumn](#filtered-output)
- [distinctUserCountColumn](#distinct-user-count)
- [filteredReasonColumn](#filtered-reason)

##### Filtered output

By default, rows that do not meet the minimum distinct user count requirements will be completely filtered out of the workflow output.

If the **`filteredMetricsDiscriminatorColumn`** parameter is included when defining workflow, then rows that not meet the minimum distinct user count requirements will have values removed and then the output will be re-aggregated with AggregationType.SUM.

This value you provide for the `filteredMetricsDiscriminatorColumn` parameter becomes the heading of the Boolean column that will be added to the workflow output. This Boolean column identifies rows with and without values in columns removed. Unchanged rows will have a value of FALSE for this column, while rows with fields removed will have a value of TRUE.

This field is entered in the workflow definition as a top-level field:

```
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

**Example: Filtering behavior with and without `filteredMetricsDiscriminatorColumn`**

**Example Raw Data**

| impressions (metric) (NONE) 	| clicks  (metric)  NONE 	| device_type (dimension) (LOW) 	| postal_code (dimension) (HIGH) 	| impressions distinct user count 	| clicks  distinct user count 	|
|-----------------------------	|------------------------	|-------------------------------	|--------------------------------	|---------------------------------	|-----------------------------	|
|     1100                    	|     300                	|     iPhone                    	|     11111                      	|     50                          	|     13                      	|
|     200                     	|     100                	|     Android                   	|     22222                      	|     30                          	|     8                       	|
|     1300                    	|     500                	|     Android                   	|     33333                      	|     133                         	|     87                      	|
|     1700                    	|     700                	|     Android                   	|     44444                      	|     108                         	|     359                     	|

**After executing workflow without `filteredMetricsDiscriminatorColumn`**

The three rows where the distinct user count of either impressions, clicks, or both is less than 100 are omitted from the output.

| impressions (metric) (NONE)|  clicks  (metric)  NONE | device_type (dimension) (LOW) | postal_code (dimension) (HIGH) |
| :--------------------------|  :--------------------- | :---------------------------- | :----------------------------- |
| 1700                       |  700                    | Android                       | 44444                          |



**After executing workflow with `filteredMetricsDiscriminatorColumn` of "filtered"**

The three rows where the distinct user count of either impressions, clicks, or both is less than 100 are included in the output.

For these rows, columns with an aggregation threshold of 100 have values of NULL and the filteredMetricsDiscriminatorColumn, "filtered" is set to TRUE.

The second and third rows of the original table are re-aggregated into a single row because their postal_code values match after the filtering.

| impressions (metric) (NONE) 	| clicks  (metric)  NONE | device_type (dimension) (LOW) 	| postal_code (dimension) (HIGH) 	| Filtered (dimension) NONE |
|-----------------------------|------------------------	|-------------------------------	|--------------------------------	|---------------------------	|
|     1100                    |     300                	|     iPhone                    	|     NULL                       	|     TRUE                  	|
|     1500                    	|     600                	|     Android                   	|     NULL                       	|     TRUE                  	|
|     1700                    	|     700                	|     Android                   	|     44444                      	|     FALSE                 	|


##### Distinct user count

In addition to the **filteredMetricsDiscriminatorColumn,** a **distinctUserCountColumn** can be added to receive the count of distinct users considered by AMC's privacy mechanisms.

This value you provide for the **distinctUserCountColumn** parameter becomes the heading of the column that will be added to the workflow output. This values in the rows help you understand how far off the user count is from the column's aggregation threshold. The value in this column may differ from the simple sum(distinct user_id) calculation, as AMCs calculation considers other factors.

At the workflow level:

```
{
    "workflowId": "DisplayImpressionsByAd",
    "distinctUserCountColumn": "du_count",
    "sqlQuery": 
    {
    ...
  
    }
}
```

The new column is added to the query output:

| impressions (metric) (NONE) 	| clicks  (metric)  NONE 	| device_type (dimension) (LOW) 	| postal_code (dimension) (HIGH) 	| du_count (dimension) NONE 	|
|-----------------------------	|------------------------	|-------------------------------	|--------------------------------	|---------------------------	|
|     1100                    	|     300                	|     iPhone                    	|     NULL                       	|     13                    	|
|     1500                    	|     600                	|     Android                   	|     NULL                       	|     22                    	|
|     1700                    	|     700                	|     Android                   	|     44444                      	|     157                   	|

##### Filtered Reason

If the **filteredReasonColumn** is included in the workflow definition, an additional column with this name will be included in the workflow output, that will contain a description of the reason why that data was filtered out off the row for privacy reasons, and null if no data was filtered out off the row.

This column is intended to provide the following information in one place:

- The fact that the row was filtered out.
- The type of distinct user count that was the reason for filtering (standard, effective, or equivalent).

  - An effective user count is defined as the count of users that contribute a non-zero value to each metric column with a non-zero total for the row.
  - An equivalent user count is defined as the row\'s total value for a given metric column divided by the highest value from any one user.
- Minimum required user count (if there are multiple columns with separate requirements, maximum of the minimum-required value across these columns will be displayed).
- Actual user count (the minimum, if there are multiple user count columns).
- The data source responsible for the filter-out to happen.
- (In scenarios where the query used the same data source multiple times) - the path responsible for the data filter-out to happen, and the total number of paths present in the query.

The column output will be structured as:

```
<actual user count> '<data source ID>' (path <path index> of <total number of paths for this data source>) <type of user count> < <required user count> required.
```

Examples:

```
"97 'dsp_impressions' users < 100 required"
"6 'sponsored_ads_traffic' (path 2 of 3) effective users < 10 required"
```

> [NOTE] Values for distinct user counts will be capped at either 2 or 100 depending on the highest count required for any output column. Values for effective distinct user counts will be capped at 100. Values for equivalent distinct user counts will be capped at 2 and truncated to an integer. In scenarios where privacy controls are not applicable (e.g. the workflow doesn't query user-owned data sets), the value will be NULL for all the rows. If multiple reasons trigger the filter to happen, the most standard and least restrictive reason will be returned in this column.

## View all workflows

Once you haveÂ created a workflow, you can list the details of all the workflows created within the instance using the GET operation of workflows resource.
If you have defined many workflows in your instance, know that you could limit the number of results that show up in each page by defining the `nextToken` and `limit` parameters as part of the [request body](amc-reporting#tag/Workflows/operation/listWorkflows). 

```
GET/amc/reporting/{instanceId}/workflows
```

**Sample request**

```
curl --location 'https://advertising-api.amazon.com/amc/reporting/{instanceId}/workflows' \
--header 'Authorization: Bearer XXXXXX \
--header 'Amazon-Advertising-API-ClientID: amzn1.application-oa2-client.XXXXX' \
--header 'Amazon-Advertising-API-AdvertiserId: XXXXXX' \
--header 'Amazon-Advertising-API-MarketplaceId: ATVPDKIKX0DER' \
```

**Sample response**

```
{
    "workflows": 
    [
        {
            "filteredMetricsDiscriminatorColumn": "filtered",
            "sqlQuery": "select advertiser, campaign, sum(clicks) as clicks, sum(conversions) as conversions, sum(product_sales) as product_sales, sum(total_product_sales) as brand_sales from amazon_attributed_events_by_conversion_time group by advertiser, campaign",
            "workflowId": "test_workflow1"
        },
        {
            "filteredMetricsDiscriminatorColumn": "filtered",
            "sqlQuery": "/* Test Query for uploaded data */ select product_id, product_name , BUILT_IN_PARAMETER('TIME_WINDOW_START') AS time_window_start, BUILT_IN_PARAMETER('TIME_WINDOW_END') AS time_window_end from tutorial_off_amazon_conversions_hashed_DUS",
            "workflowId": " Test_workflow"
        }
    ]
}

```

## View a specific workflow

To view the details of a specific workflow within your instance, pass the workflowID of the workflow in the GET operation of workflows resource.

The following is the endpoint to view a specific workflow with the identifier 'my-first-workflow', under your instance.

```
GET /amc/reporting/{instanceId}/workflows/my-first-workflow
```

## Update a workflow


The PUT operation of the workflows resource updates the values in a specific workflow. It uses the same structure as the create workflow request, except that it requires the `workflowId` field.

To update a workflow:

1. Use the **GET operation of the workflows resource** to retrieve the specific workflow you want to update. 
2. Provide the updated values for the individual parameters you want to modify as part of the body of the request. 
3. Use the PUT operation (of the workflows resource) with the `workflowId` as a path parameter to update the workflow. For example:

```
PUT /amc/reporting/{instanceId}/workflows/my-first-workflow
```

## Delete a workflow

The DELETE operation of the workflows resource deletes a single workflow. For example, the endpoint to delete a workflow with the identifier 'my-first-workflow' will be:

```
DELETE /amc/reporting/{instanceId}/workflows/my-first-workflow
```

> [NOTE] AMC will not allow you to delete a workflow if the workflow is referenced by a schedule.

## Next steps

- [Manage a workflow schedule](guides/amazon-marketing-cloud/reporting/schedule-workflow) 
or, 
- [Manage workflow executions](guides/amazon-marketing-cloud/reporting/execute-workflow)
- [Get your workflow results](guides/amazon-marketing-cloud/reporting/get-your-results)


