---
title: Get your results
description: How to get results of AMC queries
type: guide
interface: api
---
# Get your workflow results

In the previous section, we executed workflows, which in turn processed SQL queries to output results as a Comma Separated Value (.csv) file.

You can retrieve your results by generating a download URL or if you have [set up an Amazon S3 bucket](guides/amazon-marketing-cloud/get-started/get-started#set-up-s3-buckets-optional) for your instance, you can retrieve reports via S3.

> [NOTE] If you have not set up an S3 bucket, reports can only be retrieved via a download URL.

## Download your query results

To retrieve a pre-signed URL for downloading workflow execution results from S3, copy the workflowExecutionId from the response returned in the [POST operation of the workflowExecutions](amc-reporting#tag/Workflow-executions/operation/createWorkflowExecution) resource.

In our example, the workflowExecutionId is: `2b04762b-cbdb-47c7-be39-42a585ecd40e.`

Now, pass the instance identifier and the workflowexecution identifier you retrieved as path parameters in the following request:

```
GET /amc/reporting/{instanceId}/workflowExecutions/{workflowExecutionId}/downloadUrls
```

This request will return a URL where your report can be directly downloaded. Copy the URL and enter it into a browser to download the resulting CSV file.

It may take some time for the report to generate. If your report is not ready, you will receive the following response:

`Execution with ID 2b04762b-cbdb-47c7-be39-42a585ecd40e is unavailable for download.`

We will now navigate to our S3 bucket where the output of the query is saved.

## View your workflow output in the Amazon S3 bucket

If you've [defined an S3 bucket](guides/amazon-marketing-cloud/get-started/get-started#set-up-s3-buckets-optional) to store the results of your query execution, your workflow executions generate CSV files and direct those to your S3 bucket.

Let's access the S3 bucket and locate the CSV file for the workflow execution that we just ran in the previous section.

> [NOTE] You can also use the AMC API to [retrieve a download link](#download-your-query-results) to view the output for a specific workflow execution.

To access the Amazon S3 bucket:

1. Log in to the AMC UI and navigate to the **Instance info** page.
2. Locate the S3 bucket name. This will be the name of the S3 bucket created in the connected AWS account.
3. Log in to the connected AWS account and navigate to the S3 Buckets page. For more details, see  [Methods for accessing a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-bucket-intro.html).
4. Choose your AMC S3 bucket. Each folder maps 1:1 to a workflow. In other words, there is a folder for each individual SQL query.
5. Search for the folder with with the `workflowId` that was generated or named (in our example, my-first-workflow).
6. Within the folder, you will see sub-folders based on any schedules or workflow executions that have been created. For more details, see [Understand the S3 Folder Path.](#understand-the-s3-folder-path)

The following depicts a workflow that was run weekly as well as ad hoc:

> [NOTE] If you run an ad hoc workflow (that is, a workflow execution without first saving the workflow), you'll see folders with randomly generated names prefixed with "transient"

7. Open one of the workflow folders. Within the folder is a CSV file and another folder containing metadata. You can download the CSV file and open it to view the results of the query. You can also download the metadata to view additional information about the workflow that was run, including the SQL query used.

### Understand the S3 folder path

AMC creates unique folder paths within the connected S3 bucket to organize different AMC reports. The folder path for a report written to S3 looks like this:

`amc-myinstance-2ofoqzvk/workflow=my_report_daily/schedule=daily/2023-06-03-my_report_daily-ver1.csv`

The first part of the path (`amc-myinstance-2ofoqzvk`) contains the `amc-` prefix, your AMC instance name, and a hash value. The hash (in this example `2ofoqzvk`) ensures that the full S3 bucket name `amc-myinstance-2ofoqzvk` is globally unique.

The second part of the path (`workflow=my_report_daily`) provides the name of the **workflow** that generated the report file.

The third part (`schedule=daily`) is the schedule, either "daily", weekly", or "adhoc". Scheduled reports are sent to the "daily" or "weekly" paths based on the value for aggregationPeriod set within the schedule.

Finally, the file name (`2023-06-03-my_report_daily-ver1.csv`) is based off the date the report was generated, the workflow that generated it and a version number.
