---
title: AMC migration hub
description: How to migrate
type: guide
interface: api
---
# FAQs on migrating from instance-level AMC APIs to AMC APIs on Amazon Ads

**What are instance-level APIs?**

The previous version of the AMC APIs were configured to be accessed at the advertiser’s instance-level. Each AMC instance has its own unique endpoint as detailed in the ‘Instance info’ page of the AMC instance. The instance-level APIs were not completely aligned with regular Amazon Ads API practices.

**How does the migration impact me? What actions should I take?**

We recommend users currently on the instance-level APIs migrate to the upgraded APIs as soon as possible to take advantage of the enhanced security, usability, and performance of new APIs.

> [WARNING] Instance-level APIs have retired as on **August 01, 2024.** Calling any of the instance-level endpoints will return a 410 error. Migrate to the new APIs to prevent disruptions to your existing business operations.

**Can I still use AMC instance-level APIs?**

No, instance-level APIs are completely retired. Calling the instance-level endpoints will result in a 410 error. To avoid disruptions to your business operations, we recommend you migrate to new APIs as soon as possible.

**How do I know whether a particular API I’m using is instance-level API or the updated version?**

If your base URL contain any of the following,

- [https://advertising-api.amazon.com](https://advertising-api.amazon.com/)
- [https://advertising-api-eu.amazon.com](https://advertising-api-eu.amazon.com/)
- [https://advertising-api-fe.amazon.com](https://advertising-api-fe.amazon.com/)

it’s the updated version. Otherwise, you are using the instance-level version.

**How do I migrate to the new AMC APIs?**

While detailed steps are outlined in the [migration guide](guides/amazon-marketing-cloud/amc-migration-hub/migration-guide), we outline the major steps below for your easy comprehension:

| Steps to onboard and migrate AMC APIs to Amazon Ads                                                                                                                                                                                                                                                                                    | Access to Instance-level AMC API only | Access to other Ad products on Amazon Ads APIs |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-----------------------------------: | :--------------------------------------------: |
| [Register with Amazon Developer](guides/onboarding/create-lwa-app#register-with-amazon-developer) and create Login with Amazon (LwA)                                                                                                                                                                                                      |                   X                   |                                                |
| Create an[LwA security profile](guides/onboarding/apply-for-access) and retrieve LwA Client Id and Client secret                                                                                                                                                                                                                          |                   X                   |                                                |
| [Apply for API Access](guides/onboarding/apply-for-access) as a [Partner](guides/onboarding/apply-for-access#to-apply-for-api-access-as-a-partner) or as an [advertiser](guides/onboarding/apply-for-access#to-apply-for-api-access-as-a-direct-advertiser).                                                                                    |                                      |                                                |
| Remember to select the API scopes required for your business. Note that it usually take around 1 business day for the team to process the approval request.                                                                                                                                                                            |                   X                   |                                                |
| On approval,[Assign API access to your LwA application](guides/onboarding/assign-api-access)                                                                                                                                                                                                                                              |                   X                   |                                                |
| [Create an authorization grant](guides/get-started/create-authorization-grant) (for first time access)                                                                                                                                                                                                                                    |                   X                   |                                                |
| [Use the code generated from the auth grant URL to generate access and refresh tokens](guides/get-started/retrieve-access-token)                                                                                                                                                                                                          |                   X                   |                                                |
| Set up[AMC API header parameters](guides/amazon-marketing-cloud/get-started/get-started#header-parameters)                                                                                                                                                                                                                                |                   X                   |                       X                       |
| For each endpoint, replace base URL with the new URLs[(AMC endpoints)](guides/amazon-marketing-cloud/amc-migration-hub/migration#endpoint-equivalencies). Where indicated, pass the instance id as path parameter. See [AMC API references](guides/amazon-marketing-cloud/get-started/get-started#header-parameters) and AMC migration guide |                   X                   |                       X                       |
| If you use S3 bucket to access AMC query results today, you need to update IAM policy associated with your output bucket                                                                                                                                                                                                               |                   X                   |                       X                       |

> [WARNING] As a policy, Amazon Ads enables API access for **one client Id per company**. There is no need for multiple client IDs, as an unlimited number of users can use the same client ID within a company. Additional user permissions can be managed later in the Amazon Developer console. However, **only the original creator** of the Amazon Developer account may complete the initial onboarding sequence.

**After I migrate to the new AMC APIs, can I continue to access the query results from my S3 bucket?**

Your query execution results (`POST /amc/reporting/{instanceId}/workflowExecutions`) can either be [downloaded by retrieving a pre-signed URL](guides/amazon-marketing-cloud/reporting/workflow-management-service#download-your-report) or accessed through an S3 bucket that you configured when your instance was created. After migrating to the Amazon Ads API, to continue to accessing your query execution results from your S3 bucket, you will need to update the Identity Access Management (IAM) policy associated with your output bucket to allow your S3 bucket to receive the results.
To update your IAM policy, perform the following steps:

1. Log in to your AWS Account. Access your S3 Console.
2. Access your bucket through your S3 account.
3. Navigate to the Permissions tab and scroll to the Bucket policy section.
4. Click Edit.
5. Update the bucket policy to grant 811639200769 `s3:PutObject` permissions. Depicted below is how the updated policy for a sample instance named `amc-instance01` will look.

```
{
    "Version": "2012-10-17",
    "Id": "AggregationDeliveryPolicy",
    "Statement": [
        {
            "Sid": "AggregationDelivery",
            "Effect": "Allow",
            "Principal": {
                "AWS": [
                    "arn:aws:iam::811639200769:root"
                ]
            },
            "Action": [
                "s3:PutObject",
                "s3:PutObjectAcl"
            ],
            "Resource": "arn:aws:s3:::amc-instance01/*"
        },
    ]
}
```

> [NOTE] If the required permissions to the new AWS account (through the policy update) is not provided by May 01, 2024, query results will not be delivered to the your S3 bucket configured to the connected AWS account ID. **S3 buckets configured for instances after September 01, 2023 should have no required changes.**

**What should I do if I'm already on the closed beta version of some APIs?**

If you are already using the closed beta version of AMC APIs, ensure that 1) you’ve migrated all instances you manage 2) you migrate any other instance-level APIs you are using to Amazon Ads API.

**Will the "AMC Insights on AWS" and "AMC  Uploader from AWS" solutions be impacted by this launch?**

These solutions have been updated to support the latest Amazon Ads APIs to provide you an uninterrupted experience.

**How is  this the new version of advertiser data upload API (ADU 2.0) different from the instance-level data upload API (ADU 1.0) in the past?**

While the process of performing data upload is still mostly the same as before, ADU API 2.0 includes number of improvements, as described below:

1. AMC ADU 2.0 can now handle partitioning data into time chunks required to run 1P data analysis. In ADU 1.0 partitioning the data was a manual process that advertisers needed to execute themselves. For example, in the past, if advertisers want to upload 5 days worth of data, they needed to manually separate out this data into 5 files and perform the upload; with ADU 2.0, advertisers can just share one file and let us know how to read and partition the data into an acceptable format.
2. AMC ADU 2.0 now has dedicated API actions for updating data tables in AMC. These include actions such as updating data, deleting data, and updating columns.

Note that ADU API 2.0 is only available as part of Amazon Ads API, and is not available as instance-level API.

**What will happen to  the existing advertiser first-party data previously uploaded via the instance-level ADU 1.0? Will I be able to access and utilize those datasets using ADU 2.0?**

Data that was uploaded using ADU 1.0 can no longer be queried. To continue querying datasets that you had uploaded in ADU 1.0, delete the previously uploaded data, and re-upload using updated gateway version (ADU 2.0).

**Are we expecting more changes after this?**

We don’t foresee the current set of APIs undergoing any significant or breaking changes. Note that in the future, we expect the AMC API headers to be standardized to align with the rest of the Amazon Ads APIs. The standardized headers will be implemented in a manner that will cause minimal disruption to your current deployments.  In case there are other major updates, we’ll notify our developers ahead of time.

**What are the key benefits of using the new API for advertiser audiences?**

The new advertiser audiences API has the following benefits:

- Follows industry conventions for creating first-party audiences, similar to the method used by other publishers.
- Allows user the option to send hashed records to both AMC and Amazon DSP.
- Allows ingressing signals from any destination, without having to go through S3 buckets.
- Supports incremental refresh (add/move members) instead of a full refresh; this helps to reduce processing latency.

**Who should / when should I choose to use advertiser audiences API?**

Advertiser audience API works best for SaaS providers like CDPs, as well as agencies, or consultancies that would like to build "always on" advertiser data upload integrations. Direct advertisers who are willing to build and maintain an API integration with Amazon Ads can use this API as well.

However, if you expect to bring in first-party signals on sales, conversions, or other area that is not related to audiences, or only plan upload data on an ad hoc basis, we recommend that you use advertiser data upload API instead.

### Advertiser data upload API

**How is  this new version of advertiser data upload API (ADU 2.0) different from the instance-level data upload API (ADU 1.0)?**

While the process of uploading data to AMC is still the same as before, ADU API 2.0 includes a number of improvements:

- AMC ADU 2.0 can now handle partitioning data into time chunks required to run 1P data analysis, which was a manual process advertisers need to execute themselves.
  For example, in the past, if advertisers wanted to upload 5 days worth of data, they needed to manually separate out this data into 5 files and perform the upload; with ADU 2.0, advertisers can just share one file and let AMC know how to read and partition the data into an acceptable format.
- AMC ADU 2.0 now has dedicated API actions for updating data tables in AMC. These include actions such as updating, deleting, and updating columns.

> [NOTE] ADU 2.0 features listed above are available only as part of Amazon Ads API, and not in the ADU 1.0 APIs.

**What will happen to the existing advertiser first-party data previously uploaded via the instance-level  APIs? Will I be able to access and utilize those datasets using the new advertiser data upload API?**

Data that was uploaded using ADU 1.0 can no longer be queried. To continue querying datasets that you had uploaded in ADU 1.0, delete the previously uploaded data, and re-upload using ADU 2.0

> [WARNING] We recommend advertisers using ADU 1.0 to delete the previously uploaded data, and re-upload using ADU 2.0 as soon as possible to reduce confusion and mitigate operational issues.

### Advertiser audiences

**How is the advertiser audiences API different from the advertiser data upload API?**

Compared to the existing advertiser data upload API, the advertiser audiences API provides pre-defined audience schemas tailored to audience retargeting or audience insight use cases.
