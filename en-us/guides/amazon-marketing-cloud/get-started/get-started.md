---
title: Get started with Amazon Marketing Cloud
description: How to get started with AMC
type: guide
interface: api
---
# Get started

To work with AMC APIs, you need to have:

- An AMC instance with the basic requirements to start querying your data.
- A [Login with Amazon account](guides/onboarding/create-lwa-app) through which you have onboarded the Amazon Ads API.
- An S3 bucket to store the output of your queries (optional).

> [WARNING] **Instance-level APIs retired on August 01, 2024. All instance-level APIs must migrate to AMC APIs on Amazon Ads API.** For more details, go to the [Migration hub](guides/amazon-marketing-cloud/amc-migration-hub/migration).

## Requirements for an AMC instance

To work with AMC programmatically, you must have an instance that fulfills the following basic requirements for utilizing AMC:

- Planned campaigns, or campaigns that were live in the last 28 days.
- Working knowledge of [AMC SQL](guides/amazon-marketing-cloud/amc-sql/overview).
- DSP advertiser IDs and/or sponsored ads IDs to be included in your AMC instance.

## Onboard the Amazon Ads API

After you have access to an AMC account, and possess your instance identifier, you will have to set up your Amazon Ads API developer account. Onboarding the Amazon Ads APIs requires you to perform the following steps:

1. **Create a [Login with Amazon application](guides/onboarding/create-lwa-app)**.
   Requests to the Amazon Ads API are made by a client application administered by Login with Amazon.
2. **Apply for permission to access the API.** The application form includes questions about how your business intends to use the Amazon Ads API, as well as information about the [Amazon Ads API License Agreement](https://advertising.amazon.com/API/docs/license-agreement), the [Data Protection Policy](https://advertising.amazon.com/API/docs/policy/en_US), and
   [Amazon Marketing Cloud Terms](https://advertising.amazon.com/marketing-cloud/terms/AMC_Terms.html).
   If you are a partner, visit [the Amazon Ads Partner page](https://advertising.amazon.com/partner-network/register-api?ref_=a20m_us_api_drctad) for information on how to apply for access to the API.
3. **Create an authorization grant.** To generate the necessary access tokens to successfully perform an API request, you first need to [create an authorization grant](guides/get-started/create-authorization-grant) and then [retrieve access and refresh tokens](guides/get-started/retrieve-access-token).

## Set up S3 buckets (optional)

AMC offers the flexibility of viewing the outputs of your workflows either through:

- a download URL using the `GET/amc/reporting/{instanceId}/workflowExecutions/{workflowExecutionId}/downloadUrls` endpoint,
- or by sending your query execution results to an S3 bucket that is linked to your instance.

Once your instance is created, [update your instance](guides/amazon-marketing-cloud/admin/instance-management#update-instance-information) to include your connected AWS account and S3 bucket name.  You will then use the CloudFormation link generated to create an S3 bucket within your account, and grant the AMC account to save your query results to into that S3 bucket.   

Since the bucket is created within your account, it is therefore a bucket that is owned by your AWS account.

> [NOTE] Possessing an AWS account and an S3 bucket is not a requirement for AMC APIs. The setup process simply allows AMC to  direct your reports into it, and AMC will not be able to read any of your output files from that or any other bucket that you own without permission.

The S3 bucket for AMC is used to store the results of both ad-hoc workflows and scheduled workflows. CloudFormation is used to set up the S3 bucket and link it to the AMC instance. For more information on CloudFormation, refer to [What is CloudFormation?](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) in AWS documentation.

### Create S3 bucket using CloudFormation

To create the S3 bucket using CloudFormation:

1. Sign in to your AWS account as the admin user.
2. Click the CloudFormation URL. This is the URL that is displayed on the Instance Info page in the AMC UI.
3. The creation of the S3 bucket will begin. On the Stacks page, you will see the Status. Wait for this to reach `CREATE_COMPLETE`.
4. Once the Status field displays `CREATE_COMPLETE`, select the AMC stack and navigate to the **Resources** tab. From the Physical ID column, click the link to the S3 bucket.

> [NOTE] You can also find the link to the S3 bucket by navigating to the S3 homepage within the AWS console.

5. The S3 bucket is displayed. When the S3 bucket is first created, it is empty. Once you begin to execute queries and/or schedule them, you will see folders for each workflow. Here is what it will look like after you execute queries, or after scheduled queries are run.

![S3Bucket](/_images/amazon-marketing-cloud/amc_get_started_s3bucket.png)

The S3 bucket is used to store the results of both ad-hoc workflows and scheduled workflows. Each folder maps 1:1 with a query or "workflow".

AMC output data is accessed through three primary resources:

- **workflows** - A workflow maps 1:1 with a SQL query.
- **workflowExecutions** - A workflowExecution runs a workflow on an ad-hoc basis.
- **workflow schedules** - A workflow schedule runs a defined workflow on a predefined cadence and interval.

Each workflow will have a folder automatically created in S3 once it has been either scheduled or executed. For more details on the S3 folder path, see [Understand S3 Folder Path](guides/amazon-marketing-cloud/get-started/get-your-results).

## AMC rate limits

AMC enforces API rate limits to ensure all clients can use AMC effectively. API rate limits are defined as a maximum number of calls a client can make to each platform endpoint within a time period. If your app makes a lot of API requests in a short period of time then it may receive a 429 error response. The limits are subject to change.

AMC has limits on the concurrency of workflow executions at both the account and instance level. For more details, see [Query execution limits](guides/amazon-marketing-cloud/reporting/execute-workflow#query-execution-limit)

## Next steps

* If you are a sponsored ads advertiser, you can use AMC instant access to start using AMC within minutes. Refer to [Instant self-service access to AMC](guides/amazon-marketing-cloud/get-started/instant-access-to-amc).
* Otherwise, you can refer to [Make your first call](guides/amazon-marketing-cloud/get-started/make-your-first-call) for an in-depth tutorial for all advertisers.
