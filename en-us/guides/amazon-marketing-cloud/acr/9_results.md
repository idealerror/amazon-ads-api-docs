---
title: Amazon Marketing Cloud - AWS Clean Rooms
description: AMC - AWS Clean Rooms
type: guide
interface: api
---
# View query results

The results of your workflow executions are redirected to the Amazon S3 bucket associated with your collaboration instance.

To access your results, after you successfully execute your workflow using the POST method of the [/amc/reporting/{instanceid}/workflowExecutions/](amc-reporting#tag/Workflow-executions/operation/createWorkflowExecution) resource, perform the following steps:

1. Use the GET method of the [amc/reporting/{instanceId}/workflowExecutions/{workflowExecutionId}](amc-reporting#tag/Workflow-executions/operation/getWorkflowExecution) operation, with the workflowExecutionId as the path parameter.
2. Check if the status of the `workflowExecutionId` you queried for, returned as  'SUCCEEDED', which means the workflow executed completed successfully and would have generated results that are ready to download.
3. The results of your queries will live in AWS. Use the AWS APIs to retrieve the results from your connected Amazon S3 bucket.

> [NOTE] You can also use [GetProtectedQuery](https://docs.aws.amazon.com/clean-rooms/latest/apireference/API_GetProtectedQuery.html) with your `membershipIdentifier`as the path parameter to retrieve a URL to access or download your query results. Then, access your Amazon S3 bucket to retrieve your results.
