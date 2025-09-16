---
title: AMC migration hub
description: How to migrate
type: guide
interface: api
---
# Migration overview

The launch of AMC APIs on the Amazon Ads API represents the next generation API for programmatic access to Amazon features for AMC. All new development is occurring in the Amazon Ads API, including enhanced data protection, security features, usability, and performance. This change ensures developers are using the latest technology and features to utilize, manage, and build with these APIs for the benefit of both partners and customers.

> [WARNING] **The instance-level APIs have been officially retired as on August 01, 2024.**  Use of the instance-level APIs may result in 410 errors. To ensure uninterrupted operations, migrate to the AMC APIs on the Amazon Ads API as outlined in this Migration guide. To continue receiving query results in your connected S3 bucket, ensure that your **[connected AWS S3 bucket policy](guides/amazon-marketing-cloud/amc-migration-hub/migration-guide#updating-s3-bucket-policy-to-receive-query-results)** is updated. 

## What's changed?

- **Base URL**: Previously, API requests used the AMC Instance API URL, a unique URL for each instance. Now, API requests use a common [URL prefix](guides/get-started/first-call#url-prefixes) depending on the region.
- **Authorization**: Authorization is now based on OAuth2 instead of AWS IAM based permissions. An AWS Account is not a mandatory requirement to access AMC via APIs.
- **API endpoints**: API endpoints that are provided on a per-AMC-instance basis will be replaced by an instanceId parameter included in the Ads.
- **Request headers**: The request header now requires the following mandatory parameters: `Amazon-Advertising-API-AdvertiserId` (the AMC Account ID) and `Amazon-Advertising-API-MarketplaceId` in addition to the headers for accessing other Amazon Ads APIs.
- **Instance-level actions**: Previous instance-level APIs contained only instance level actions.

To learn more about Amazon Ads API, visit the [Amazon Ads - Advanced tools center](index).

## What's the same?

All AMC API actions will be supported on the Amazon Ads API and will remain the same. For example, if you were making API calls to the AMC reporting (workflows) API, an equivalent API is available on the Amazon Ads API, as listed in the [Endpoint equivalencies](#endpoint-equivalencies) section.

## AMC API categories

AMC APIs are now grouped by functional categories, as listed below:

| Category               | API                           | Description                                                           |
| ---------------------- | ----------------------------- | --------------------------------------------------------------------- |
| Administration         | Account / instance management | Manage AMC instances                                                  |
| Reporting              | Workflow execution            | Query data sources and generate reporting on demand                   |
| Reporting              | Workflow scheduling           | Query data sources and generate reporting per schedule                |
| Audience management    | Rule-based audiences          | Create ruled-based audiences ready for Amazon DSP campaign activation |
| Audience management    | Lookalike audiences           | Create lookalike audiences ready for Amazon DSP campaign activation   |
| Audience management    | Advertiser audiences          | Stream pseudonymized audience records into Amazon Ads                 |
| Advertiser data upload | Advertiser data upload        | Upload pseudonymized advertiser first-party signals to Amazon Ads     |

### Endpoint equivalencies

| Category                         | Resource            | AMC instance-level APIs endpoints                                               | AMC APIs on Amazon Ads endpoints                                                               |
| -------------------------------- | ------------------- | ------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| **Reporting**              | Workflow Executions | POST {unique-amc-url}/prod/workflowExecutions                                   | POST {api_url}/amc/reporting/{instanceId}/workflowExecutions                                   |
|                                  |                     | PUT {unique-amc-url}/prod/workflowExecutions/{workflowExecutionId}              | PUT {api_url}/amc/reporting/{instanceId}/workflowExecutions/{workflowExecutionId}              |
|                                  |                     | GET {unique-amc-url}/prod/workflowExecutions/{workflowExecutionId}/downloadUrls | GET {api_url}/amc/reporting/{instanceId}/workflowExecutions/{workflowExecutionId}/downloadUrls |
|                                  |                     | GET {unique-amc-url}/prod/workflowExecutions                                    | GET {api_url}/amc/reporting/{instanceId}/workflowExecutions                                    |
|                                  |                     | GET {unique-amc-url}/prod/workflowExecutions/{workflowExecutionId}              | GET {api_url}/amc/reporting/{instanceId}/workflowExecutions/{workflowExecutionId}              |
|                                  |                     | DELETE {unique-amc-url}/prod/workflowExecutions/{workflowExecutionId}           | DELETE {api_url}/amc/reporting/{instanceId}/workflowExecutions/{workflowExecutionId}           |
| **Reporting**              | Workflows           | GET {unique-amc-url}/prod/workflows/{workflowId}                                | GET {api_url}/amc/reporting/{instanceId}/workflows/{workflowId}                                |
|                                  |                     | DELETE {unique-amc-url}/prod/workflows/{workflowId}                             | DELETE {api_url} /amc/reporting/{instanceId}/workflows/{workflowId}                            |
|                                  |                     | PUT {unique-amc-url}/prod/workflows/{workflowId}                                | PUT {api_url}/amc/reporting/{instanceId}/workflows/{workflowId}                                |
|                                  |                     | POST {unique-amc-url}/prod/workflows                                            | POST {api_url}/amc/reporting/{instanceId}/workflows                                            |
|                                  |                     | GET {unique-amc-url}/prod/workflows                                             | GET {api_url}/amc/reporting/{instanceId}/workflows                                             |
| **Reporting**              | Schedules           | POST {unique-amc-url}/prod/schedules                                            | POST {api_url}/amc/reporting/{instanceId}/schedules                                            |
|                                  |                     | GET {unique-amc-url}/prod/schedules                                             | GET {api_url}/amc/reporting/{instanceId}/schedules                                             |
|                                  |                     | GET {unique-amc-url}/prod/schedules{scheduleId}                                 | GET {api_url}/amc/reporting/{instanceId}/schedules/{scheduleId}                                |
|                                  |                     | DELETE {unique-amc-url}/prod/schedules{scheduleId}                              | DELETE {api_url}/amc/reporting/{instanceId}/schedules/{scheduleId}                             |
|                                  |                     | PUT {unique-amc-url}/prod/schedules{scheduleId}                                 | PUT {api_url}/amc/reporting/{instanceId}/schedules/{scheduleId}                                |
| **Reporting**              | Datasources         | GET {unique-amc-url}/prod/{instance_id}/dataSources                             | GET {api_url}/amc/reporting/{instance_id}/dataSources                                          |
| **Audiences**              | Rule Based Audience | GET {rba\_api\_url}/amc/audiences/query                                         | GET {api_url}/amc/audiences/query                                                              |
|                                  |                     | GET {rba\_api\_url}/amc/audiences/query                                         | POST {api_url}/amc/audiences/query                                                             |
|                                  |                     | GET {rba\_api\_url}/amc/audiences/query                                         | GET {api_url} /amc/audiences/query/{audienceExecutionId}                                       |
| **Advertiser data upload** | Data sets           | POST {unique-amc-url}/dataSets                                                  | POST {api_url}/amc/advertiserData/{instanceId}/dataSets                                        |
|                                  |                     | PUT {unique-amc-url}/dataSets/{datasetID}                                       | PUT {api_url}/amc/advertiserData/{instanceId}/dataSets/{dataSetId}                             |
|                                  |                     | GET {unique-amc-url}/dataSets                                                   | POST {api_url}/amc/advertiserData/{instanceId}/dataSets/list                                   |
|                                  |                     | GET {unique-amc-url}/dataSets/{datasetID}                                       | GET {api_url}/amc/advertiserData/{instanceId}/dataSets/{dataSetId}                             |
|                                  | Uploads             | POST {unique-amc-url}/data/{datasetID}/uploads                                  | POST {api_url}/amc/advertiserData/{instanceId}/uploads/list                                    |
|                                  |                     | POST {unique-amc-url}/data/{datasetID}/uploads                                  | POST {api_url}/amc/advertiserData/{instanceId}/uploads/{dataSetId}                             |
|                                  |                     | GET {unique-amc-url}/data/{dataSetId}/uploads/{uploadId}                        | GET {api_url}/amc/advertiserData/{instanceId}/uploads/{dataSetId}/{uploadId}                   |

### New endpoints

| Category                         | Resource                              | Endpoints                                                              |
| :------------------------------- | :------------------------------------ | :--------------------------------------------------------------------- |
| **Administration**         | Accounts                              | GET {api_url}/amc/accounts                                             |
|                                  | Instances                             | POST {api_url}/amc/instances/{instanceId}/updateCustomerAwsAccount     |
|                                  |                                       | GET {api_url}/amc/instances                                            |
|                                  |                                       | GET {api_url}/amc/instances/{instanceId}/advertisers                   |
|                                  |                                       | GET {api_url}/amc/instances/{instanceId}                               |
| **Audiences**              | Advertiser audiences - connection     | POST {api_url} /amc/audiences/connections                              |
|                                  |                                       | GET {api_url}/amc/audiences/connections                                |
|                                  |                                       | DELETE {api_url}/amc/audiences/connections?connectionId={connectionId} |
|                                  | Advertiser audience creation metadata | POST {api_url}/amc/audiences/metadata/                                 |
|                                  |                                       | PUT {api_url}/amc/audiences/metadata/{audienceId}                      |
|                                  |                                       | GET {api_url}/amc/audiences/metadata/{audienceId}                      |
|                                  | Advertiser audience creation          | POST {api_url}/amc/audiences/records                                   |
|                                  | AMC terms                             | GET {api_url} /amc/audiences/connections/terms                         |
|                                  |                                       | PATCH {api_url} /amc/audiences/connections/terms                       |
| **Audiences**              | Rule-based lookalike audiences        | POST {api_url} /amc/audiences/lookalike                                |
| **Advertiser data upload** | Data sets                             | DELETE {api_url}/amc/advertiserData/{instanceId}/dataSets/{dataSetId}  |
|                                  | Data set column                       | POST {api_url}/amc/advertiserData/{instanceId}/dataSets/{dataSetId}    |
|                                  |                                       | PUT {api_url}/amc/advertiserData/{instanceId}/dataSets/{dataSetId}     |
|                                  |                                       | DELETE {api_url}/amc/advertiserData/{instanceId}/dataSets/{dataSetId}  |

### Deprecated Endpoint

| Category               | AMC v1 endpoint                                    | Description   |
| :--------------------- | :------------------------------------------------- | :------------ |
| Advertiser data upload | POST {unique-amc-url}/data/{datsetId}/batchUploads | Batch uploads |

### New or updated parameters for workflow management services

The following two new parameters have been included in the schedules and workflowExecutions of Sandbox instances.

- **requireSyntheticData**
- **disableAggregationControls**

For details, refer to [AMC sandbox](guides/amazon-marketing-cloud/amc-sandbox).

### New or updated parameters for advertiser data upload resources

**Update strategy**

This is a new parameter that allows you to set how new data will be merged with existing data in the instance.

**Dataset period**

Dataset period options have changed to:

- P1D (daily partition)
- PT1H (hourly partition)
- P1M (monthly partition)

> [NOTE] There is no longer the option to partition per-minute or per-week. Also, uploaded files do not have to be partitioned before uploading, though you can choose to partition files and upload individually if desired.

**S3 bucket for advertiser data upload**

The S3 bucket can be within the **Connected AWS Account** associated to the AMC instance you will be uploading data to, or within a different AWS account. If you are using an S3 bucket within the **Connected AWS Account**, we recommend using a bucket separate from one created for AMC API report storage.

For more information, refer to [Set up the S3 bucket](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-s3-bucket).

## Next steps

Start your [onboarding journey to the Ads API here](guides/onboarding/overview) or review the steps to help you [migrate from the AMC instance-level APIs to AMC Amazon Ads API](guides/amazon-marketing-cloud/amc-migration-hub/migration-guide).
