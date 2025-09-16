---
title: Getting started with the traffic events API
description: Walkthrough of a typical request and response to the traffic events API.
type: guide
interface: api
tags:
    - Reporting
keywords:
    - SQL query
    - clean room
    - Postman example
    - Curl example
---

# Get started

## Prerequisites

To work with traffic events APIs, you need to have:

* An existing Amazon Ads account.
* A Login with Amazon account through which you have onboarded the Amazon Ads API.

For information about onboarding to the Amazon Ads API, see [Onboarding](guides/onboarding/overview). For information about generating access tokens, see the general [guide to getting started with the Amazon Ads API](guides/get-started/overview).

## Header parameters

In addition to the individual API parameters, the following header parameters are required for most of your calls.

|Header	|Description	|
|---	|---	|
|Amazon-Advertising-API-ClientId	|The identifier of a client associated with an Amazon Developer account.	|
|Amazon-Advertising-API-MarketplaceId	|The marketplace identifier for the marketplace in the request. Marketplaces are tied to the country.	|
|Amazon-Advertising-Advertiser-Id	|The traffic events API account identifier. Include this header when querying Amazon DSP advertisers. This value is the advertiser id found in the Amazon Ads DSP console, or from the `/dsp/adverisers` API. Do not include this header when querying Sponsored Ads advertisers.	|
|Amazon-Advertising-API-EntityId	|The traffic events API entity identifier. Include this header when querying Amazon Sponsored Ad advertisers. This value is the entity id found in the Amazon Ads console, or from the `/v2/profiles` API. Do not include this header when querying DSP advertisers.	|
|Authorization	|Login with Amazon token in the form of Bearer {{token}}	|
|Content-type	|application/json	|

>[TIP]The **Amazon-Advertising-API-MarketplaceId** parameter is exclusive to traffic events APIs and is not required for other Amazon Ads APIs. Make sure you specify the marketplace of your account when calling traffic events APIs.

>[WARNING]The **Amazon-Advertising-Advertiser-Id** is a mandatory header parameter when querying DSP advertisers. It must _not_ be included when querying Sponsored Ads entities.<br/><br/>Similarly, **Amazon-Advertising-API-EntityId** is a mandatory header parameter when querying Sponsored Ads entities. It must _not_ be included when querying DSP advertisers.

### Marketplaces

The marketplace identifiers called in requests through the Amazon-Advertising-API-MarketplaceId header parameter are tied to a country, as listed in the tables below.

#### Europe

|Country	|Marketplace ID	|Country code	|
|---	|---	|---	|
|Germany	|A1PA6795UKMFR9	|DE	|
|Spain	|A1RKKUPIHCS9HS	|ES	|
|France	|A13V1IB3VIYZZH	|FR	|
|Italy	|APJ6JRA9NG5V4	|IT	|
|Netherlands	|A1805IZSGTT6HS	|NL	|
|Sweden	|A2NODRKZP88ZB9	|SE	|
|Turkey	|A33AVAJ2PDY3EV	|TR	|
|United Kingdom	|A1F83G8C2ARO7P	|UK	|
|Saudi Arabia	|A17E79C6D8DWNP	|SA	|
|United Arab Emirates	|A2VIGQ35RCS4UG	|AE	|

#### North America

|Country	|Marketplace ID	|Country code	|
|---	|---	|---	|
|Brazil	|A2Q3Y263D00KWC	|BR	|
|Canada	|A2EUQ1WTGCTBG2	|CA	|
|Mexico	|A1AM78C64UM0Y8	|MX	|
|United States of America	|ATVPDKIKX0DER	|US	|

#### APAC

|Country	|Marketplace ID	|Country code	|
|---	|---	|---	|
|Australia	|A39IBJ37TRP1C6	|AU	|
|Japan	|A1VC38T7YXB528	|JP	|

## Make your first call

The tutorials in this section walk you through the steps to create, read, and update resources using the traffic events API. 

>[NOTE]When completing the Login With Amazon flow, ensure that the Amazon user account providing the authorization grant has access to the advertiser acount ID you wish to query. If you do not have access to the advertiser account ID you specified, you will receive an unauthorized response when attempting to call the traffic events APIs.

To test your first call, we will use the POST method of the `/avc/workflowexecutions` endpoint to create a workflow execution.

### Workflow Execution

A workflow allows you to create and execute a query on demand. You can then download the output of the query.

Before you test out the API by sending your first request, it is important to understand that if any personal information (such as an individual's email address or phone number) is added to a request, the query will not output desired results.

We will use the POST method with the `/workflowexecutions` endpoint path, providing a request body containing a JSON object with the following parameters:

* **sqlQuery**: The SQL query must be a single-line query and stripped of tabs and new-line delimiters before added to the JSON object. Semicolons are not required at the end of the query.
* **workflowId**: Friendly name to identify the query when you search for it later.

For this example, we'll be running a basic SQL query that aggregates total cost, and impressions at the advertiser and campaign level. The query is below:

```
SELECT
advertiser,
campaign,
sum(total_cost) as spend,
sum(impressions) as impressions
FROM dsp_impressions
GROUP BY
advertiser,
campaign
```

>[NOTE]The above example contains newline characters. The traffic events API does not support newline characters in the SQL query in the JSON object. You will need to remove all newlines from the query before you submit the API request.

Here is the query converted to a single line:

```
SELECT advertiser, campaign, sum(total_cost) as spend, sum(impressions) as impressions FROM dsp_impressions GROUP BY advertiser, campaign
```

### Request payload

We will use the single-line query above as the value to pass for the `sqlQuery` parameter. The following is an example of the message body:

```json
{
 "workflow": 
    {
     "sqlQuery": "SELECT advertiser, campaign, sum(total_cost) as spend, sum(impressions) as impressions FROM dsp_impressions GROUP BY advertiser, campaign",
     "workflowId": "TEST_WORKFLOW"
    },
 "timeWindow":   
    {
     "type": "MOST_RECENT_WEEK"
    }
}
```

When executed, this value will run the query for the last 7 days. There are also predefined values for most recent day, current month, previous month, and custom (explicit) time windows.

We can call the POST workflow execution endpoint using one of the following approaches:

* Using cURL
* Using Postman

### Using cURL

You can use cURL to create an ad hoc workflow using the North America URL prefix.

#### Sample request

```json
curl --location 'https://advertising-api.amazon.com/avc/workflowexecutions' \
    --header 'Authorization: Bearer {{Bearer Token}}' \
    --header 'Amazon-Advertising-API-ClientID: {{Login with Amazon Client ID}}' \ 
    --header 'Amazon-Advertising-Advertiser-Id: {{DSP Advertiser ID}}' \
    --header 'Amazon-Advertising-API-MarketplaceId: {{Marketplace ID}}' \
    --data '{
        "workflow": {
            "sqlQuery": "SELECT advertiser, campaign, sum(total_cost) as spend, sum(impressions) as impressions FROM dsp_impressions GROUP BY advertiser, campaign",
            "workflowId": "TEST_WORKFLOW"
        },
        "timeWindow": {"type": "MOST_RECENT_WEEK"}
    }'
```

#### Sample response

```json
{
 "workflowExecution":
    {
        "timeWindow": {
            "startOriginal": "2023-08-17T04:12:53Z",
            "start": "2023-08-17T04:12:53Z",
            "end": "2023-08-17T04:12:53Z",
            "timeZoneOriginal": "UTC",
            "endOriginal": "2023-08-16T04:12:53Z"
        },
         "expireTime": "2023-08-17T04:12:53Z",
         "workflowExecutionTimeoutSeconds": 21600,
         "sqlQuery": "SELECT advertiser, campaign, sum(total_cost) as spend, sum(impressions) as impressions FROM dsp_impressions GROUP BY advertiser, campaign",
         "createTime": "2023-08-16T04:12:53Z",
         "lastUpdatedTime": "2023-08-16T04:12:53Z",
         "waitUntil": "2023-08-16T03:12:53Z",
         "workflowExecutionId": "a629552c-027f-4624-b4fb-55d159f9c7d2",
         "workflowId": "TEST_WORKFLOW",
         "status": "PENDING",
         "advertiserId": "123456",
    }
}
```

Note the `workflowExecutionId` from the response. This identifier will be used to download a CSV file containing query results.

### Using Postman

In this example, we'll be using Postman, an API platform that has an easy-to-use graphical user interface (GUI).

>[NOTE]Postman is not an Amazon-affiliated tool. You can use any API tool for this tutorial (such as the cURL command-line tool), but we chose Postman for its intuitive GUI.

#### Setup

1. Download and [install Postman](https://www.postman.com/downloads/).
2. [Complete the setup for the Amazon Ads API Postman collection](guides/get-started/using-postman-collection). Ensure that the user account you use to generate the authorization code has access to the advertising account you want to query.
3. Click **Add request** within the collection to create a new request. This will inherit the authorization parameters of the collection.
4. Name your request as desired (for example, Execute ad hoc workflow). By default, Postman creates a GET request. Make sure the HTTP method is set to POST.
5. In the request URL field, enter the base traffic events API URL and append the endpoint path for the workflow executions resource (/workflowExecutions). Using the North America URL prefix, your request will look something like the example below, with your unique instance being called as a variable:

    `https://advertising-api.amazon.com/avc/workflowexecutions`

#### Headers and payload

1. On the Headers tab, add the **mandatory** header parameters noted above, with the appropriate values for each.
2. Next, enter the JSON object in the request body as described above. In Postman, on the **Body** tab, select **Raw** and enter the JSON object that defines the SQL query (sqlQuery) and the timeWindowType as `MOST_RECENT_WEEK.`
3. Click **Send**. If successful, you will receive a response with the HTTP status code 200 OK and the following response parameters:

```json
{
 "workflowExecution":
    {
    "timeWindow": 
        {
            "startOriginal": "2023-08-17T04:12:53Z"
"start": "2023-08-17T04:12:53Z"
"end": "2023-08-17T04:12:53Z"
"timeZoneOriginal": "UTC"
"endOriginal": "2023-08-16T04:12:53Z"
        }
         "expireTime": "2023-08-17T04:12:53Z",
         "workflowExecutionTimeoutSeconds": 21600,
         "sqlQuery": "SELECT advertiser, campaign, sum(total_cost) as spend, sum(impressions) as impressions FROM dsp_impressions GROUP BY advertiser, campaign",
         "createTime": "2023-08-16T04:12:53Z",
         "lastUpdatedTime": "2023-08-16T04:12:53Z",
         "waitUntil": "2023-08-16T03:12:53Z",
         "workflowExecutionId": "a629552c-027f-4624-b4fb-55d159f9c7d2",
         "workflowId": "TEST_WORKFLOW",
         "status": "PENDING",
         "advertiserId": "123456",
    }
}
```

If you receive a response code other than 200, check that your credentials are authenticated and that your token has not expired.

>[TIP]Note the `workflowExecutionId` from the response, because we will need it to retrieve the query results.

## Get your query results

In the previous section, we ran an ad hoc workflow which executed a query that aggregates total cost, impressions, and reach at the advertiser and campaign level. Next, we will access the CSV file containing the results of the query. You can retrieve your results by generating a download URL.

You will need to copy the `workflowExecutionId` from the response of the ad hoc execution that we previously ran. We will use that with the `/workflowexecutions/{workflowExecutionId}/downloadUrls` endpoint to retrieve a link for the results.

>[NOTE]Workflow executions are run asynchronously, and often take at least 15 minutes to execute.

### Generate the download URL

#### Using Curl

Make a GET request, substituting your `workflowExecutionUd` into the location below, along with the appropriate values for each header.

```bash
curl --location 'https://advertising-api.amazon.com/avc/workflowexecutions/{workflowExecutionId}/downloadUrls' \
    --header 'Authorization: Bearer {{Bearer Token}}' \
    --header 'Amazon-Advertising-API-ClientID: {{Login with Amazon Client ID}}' \ 
    --header 'Amazon-Advertising-Advertiser-Id: {{DSP Advertiser ID}} \
    --header 'Amazon-Advertising-API-MarketplaceId: {{Marketplace ID}}
```

#### Using Postman

Let's create a new request to download the results of our query:

1. Copy the workflowExecutionId from the response returned in the previous section. In the above response example, the workflowExecutionId is: `a629552c-027f-4624-b4fb-55d159f9c7d2`
2. Create a new request in Postman and select the GET method.

Enter the request URL, inserting the workflowExecutionId from the previous step into the following URL: 

```
https://advertising-api.amazon.com/avc/workflowexecutions/{workflowExecutionId}/downloadUrls
```

### Download the CSV file

This request will return a URL where your report can be directly downloaded. Copy the URL and enter it into a browser to download the resulting CSV file.

It may take some time for the report to generate. Wait until the workflow execution is in a 'SUCCEEDED' status to download your report. If your report is not ready, you will receive a HTTP 404 error stating that the download URL is not found.

If you receive that error, wait until the workflow execution is in SUCCEEDED state. You can check the state of the Workflow Execution by calling the Get Workflow Execution API.

We've just run our first ad hoc query by creating a workflow execution. Then, we created a request to generate a URL to download the CSV file containing the query results.

## Next steps

Now that you've made your first call, learn more about:

- [Data sources for the traffic events API](guides/traffic-events/data-sources)
- [Querying data in the traffic events API](guides/traffic-events/querying-data)
- [Data aggregation thresholds](guides/traffic-events/data-aggregation-thresholds)
