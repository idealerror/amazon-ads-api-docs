---
title: Workflow management service
description: How to create and manage workflows in AMC
type: guide
interface: api
---


# Create and execute an ad hoc workflow 

An ad hoc workflow allows you to create and execute a query on-demand. You can then download the output of the query or access your connected S3 bucket to check the output.

Before you test out the API by sending your request, it is important to understand that any if any personal information (such as an individual's email address or phone number) is added to a request, the query will not output desired results.

We will use the POST method with the `/workflowExecutions` endpoint path, as well as the Body of the request containing a JSON object with the following parameters:

- **sqlQuery**: The SQL query must be a single-line query and stripped of tabs and new-line delimiters before added to the JSON object. Semicolons are not required at the end of the query.
- **timeWindowType**: The type of time window to use to for specifying input data for the workflow execution. In our example, we will use `MOST_RECENT_WEEK`. The other predefined values for most recent day, current month, previous month, and custom (explicit) time windows.

For this example, we'll be running a basic SQL query that aggregates total cost, impressions, and reach at the advertiser and campaign level. The query is below:

```
SELECT
  advertiser,
  campaign,
  SUM(total_cost) AS spend,
  SUM(impressions) AS impressions,
  COUNT(DISTINCT user_id) AS reach
FROM
  dsp_impressions
GROUP BY
  advertiser,
  campaign
```

> [NOTE] The above example contains newline characters. The AMC API does not support newline characters in the SQL query in the JSON object. You will need to remove all newlines from the query before you submit the API request.

Note how we convert the SQL query to a single line.

```
SELECT advertiser, campaign, sum(total_cost) as spend, sum(impressions) as impressions, count(DISTINCT user_id) as reach FROM dsp_impressions GROUP BY advertiser, campaign
```

**Sample request payload**

We will use the single-line query as the value to pass for the sqlQuery parameter. The following is an example of the message body:

```
{
 "workflow": 
    {
     "sqlQuery": "SELECT advertiser, campaign, sum(total_cost) as spend, sum(impressions) as impressions, count(DISTINCT user_id) as reach FROM dsp_impressions GROUP BY advertiser, campaign"
    },
    "timeWindowType": "MOST_RECENT_WEEK"
}
```

When executed, this value will run the query for the last 7 days. There are also predefined values for most recent day, current month, previous month, and custom (explicit) time windows.

We can call the POST workflow execution endpoint using one of the following approaches:

- [Using cURL](#using-curl)

- [Using Postman](#using-postman)

### Using cURL

You can use cURL to create an ad hoc workflow using the North America URL prefix.

**Sample request**

```
curl --location 'https://advertising-api.amazon.com/amc/reporting/{instanceId}/workflowExecutions' \
--header 'Authorization: Bearer {token}' \
--header 'Amazon-Advertising-API-ClientID: {Login with Amazon Client ID}' \
--header 'Amazon-Advertising-API-AdvertiserId: {AMC entity ID}' \
--header 'Amazon-Advertising-API-MarketplaceId: {Marketplace ID}' \
--data '{
 "workflow": {
 "sqlQuery": "SELECT advertiser, campaign, sum(total_cost) as spend, sum(impressions) as impressions, count(DISTINCT user_id) as reach FROM dsp_impressions GROUP BY advertiser, campaign"
 },
 "timeWindowType": "MOST_RECENT_WEEK"
}'

```

**Sample response**

```
{
    "createTime": "2023-08-16T04:12:53Z",
    "dataGaps": [],
    "disableAggregationControls": false,
    "executionCategory": "AD_HOC",
    "expireTime": "2023-08-17T04:12:53Z",
    "ignoreDataGaps": false,
    "lastUpdatedTime": "2023-08-16T04:12:53Z",
    "parameterValues": {},
    "status": "PENDING",
    "timeWindowTimeZoneOriginal": "UTC",
    "waitUntil": "2023-08-16T03:12:53Z",
    "workflowExecutionId": "a111111c-111f-1111-b1f1-11d111f1c1d1",
    "workflowExecutionTimeoutSeconds": 21600,
    "workflowId": "transient_c3393afa-5996-4e0e-829e-5da4718eb9e3"
}
```


Note the `workflowExecutionId` from the response. This identifier will be used to [Download a CSV file containing query results.](guides/amazon-marketing-cloud/get-started/get-your-results#download-a-csv-file-containing-query-results)

### Using Postman

In this example, we'll be using Postman, an API platform that has an easy-to-use graphical user interface (GUI).

> [NOTE] Postman is not an Amazon-affiliated tool. You can use any API tool for this tutorial (such as the cURL command-line tool), but we chose Postman for its intuitive GUI.

1. Download and [install Postman](https://www.postman.com/downloads/).

2. Open Postman and click **Create new collection**. The collection will be used to group our requests and for inheritance of authorization parameters, saving us a lot of time and effort.

3. Name your collection (for example, Amazon Ads APIs).

4. Set up your environment and collection by following [these](guides/postman) instructions.

5. [Generate an authorization grant code](guides/get-started/using-postman-collection#generate-an-authorization-grant-code-) and [retrieve access and refresh tokens](guides/get-started/using-postman-collection#retrieve-access-and-refresh-tokens) to complete your authorization.

Your authorization credentials will expire after 24-hours. When the authorization credentials expire, you will receive an HTTP 403 forbidden response when calling the API. In order to update, you'll need to re-generate your authorization credentials and update the parameters in the collection's Authorization tab with the regenerated details. Refer to [Generate API access credentials](guides/get-started/retrieve-access-token#call-the-authorization-url-to-request-access-and-refresh-tokens) for more information about generating the tokens.

6. Click **Add request** within the newly created collection to create a new request. This will inherit the authorization parameters of the collection.

7. Name your request (for example, Execute ad hoc workflow).

By default, Postman creates a GET request. Make sure the HTTP method is set to POST.

8. In the request URL field, enter the base AMC API URL and append the endpoint path for the workflow executions resource (/workflowExecutions). Using the North America URL prefix, your request will look something like the example below, with your unique instance being called as a variable.

`<https://advertising-api.amazon.com/amc/reporting/{instanceId}/workflowExecutions>`

9. On the Headers tab, add the **mandatory [header parameters](guides/amazon-marketing-cloud/get-started/get-started#header-parameters)** with the appropriate values for each.

10. Next, we will enter the JSON object in the request body as described in the example below.

    In Postman, on the Body tab, select **Raw** and enter the JSON object that defines the SQL query (`sqlQuery`) and the timeWindowType as `MOST_RECENT_WEEK.`


11. Click **Send**. If successful, you will receive a response with the HTTP status code 200 OK and the following response parameters:

```
{
    "createTime": "2023-08-16T04:12:53Z",
    "dataGaps": [],
    "disableAggregationControls": false,
    "executionCategory": "AD_HOC",
    "expireTime": "2023-08-17T04:12:53Z",
    "ignoreDataGaps": false,
    "lastUpdatedTime": "2023-08-16T04:12:53Z",
    "parameterValues": {},
    "status": "PENDING",
    "timeWindowTimeZoneOriginal": "UTC",
    "waitUntil": "2023-08-16T03:12:53Z",
    "workflowExecutionId": "a111111c-111f-1111-b1f1-11d111f1c1d1",
    "workflowExecutionTimeoutSeconds": 21600,
    "workflowId": "transient_c3393afa-5996-4e0e-829e-5da4718eb9e3"
}
```

If you receive a response code other than 200, check if your credentials are authenticated and that your token has not expired.

12. Note the `workflowExecutionId` from the response, because we will need it in the next section.

## View your workflow results

If you have an S3 bucket configured to store the output of your workflow results, [access your S3 bucket](guides/amazon-marketing-cloud/reporting/get-your-results#view-your-workflow-output-in-the-amazon-s3-bucket) to view your report. Alternatively, refer to the steps below to generate a URL to download your workflow results in the CSV format.

To download your workflow results, you will need to copy the `workflowExecutionId` from the response of the ad hoc execution that we previously ran. We will use the `workflowExecutionId` with the `/workflowExecutions/{workflowExecutionId}/downloadUrls` endpoint to retrieve a link for the results.

Let's create a new request to download the results of our query:

1. Copy the `workflowExecutionId` from the response returned in the previous section. In our example, the workflowExecutionId is: `a111111c-111f-1111-b1f1-11d111f1c1d1`

2. Create a new request and select the GET method.

> [NOTE] Workflow executions are run asynchronously, and often take at least 15 minutes to execute.

3. Enter the request URL, inserting the `workflowExecutionId` from the previous step into the following URL:
`https://advertising-api.amazon.com/amc/reporting/{instanceId}/workflowExecutions/{workflowExecutionId}/downloadUrls`

 This request will return a URL where your report can be directly downloaded. 

4. Copy the URL and enter it into a browser to download the resulting CSV file.

It may take some time for the report to generate. If your report is not ready, you will receive the following response:
`"Execution with ID a111111c-111f-1111-b1f1-11d111f1c1d1 is unavailable for download".`

If you receive the above message, wait for 15 minutes and try the request again.

We've just run our ad hoc query by creating a workflow execution. Then, we created a request to generate a URL to download the CSV file containing the query results.

