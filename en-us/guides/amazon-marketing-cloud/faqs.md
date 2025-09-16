---
title: FAQs
description: Frequently asked questions
type: guide
interface: api
---
# AMC Ads API FAQs

## AMC APIs overview

**What are AMC APIs?**

Amazon Marketing Cloud APIs (AMC APIs) are a suite of REST interfaces for developers to programmatically access AMC features. AMC APIs are often used to automate or schedule query workflows, create custom audiences, facilitate scaled operations, upload first-party signals, and enable integrations between AMC and other systems.

**What were AMC APIs like in the past?**

The previous version of the AMC APIs were configured to be accessed at the advertiser’s instance-level. With these instance-level APIs, each AMC instance has its own unique endpoint as detailed in the **Instance info** page of the AMC UI. The instance-level APIs were not completely aligned with regular Amazon Ads API practices.

**What are Amazon Ads APIs?**

Amazon Ads APIs are a suite of REST interfaces for developers to programmatically access Amazon Ads features. Major Amazon Ads solutions are available via Amazon Ads APIs, including sponsored ads, Amazon DSP, Amazon Marketing Cloud (AMC), Amazon Marketing Stream, etc.

**What are the benefits?**

The AMC APIs align with the standards that the rest of the Amazon Ads APIs follow. **The AMC APIs offer further enhanced security, usability, and performance, allowing developers to easily utilize, manage, and build with these APIs.**

Specifically:

* **Authorization:** Developers can authorize and authenticate AMC APIs the same way as the rest of the Amazon Ads APIs; which is via client applications administered by Login with Amazon.
  * This **streamlines and simplifies** the set up and provides seamless access to relevant AMC accounts and instances.
* **Multi-client access:** Multiple users and partner organizations can now access one AMC instance at the same time.
  * This enables more **scalable AMC usage and operations**, is especially beneficial for partners handling multiple advertiser instances.
* **Standard URL pathway:** Developers can now find AMC APIs grouped by functional categories, and following a standard endpoint naming convention composed of a base URL, the API path, and resources.
  * This helps developers to **easily comprehend and identify the correct APIs to use**, and reduces the chance of human errors while executing the APIs.
* **Easy access:** Developers can access detailed descriptions and instructions in the Advanced tools center
  * This helps developers to find comprehensive API documentation along with developer guides easily at the same place where other Amazon Ads APIs documentation is housed.

**What AMC APIs are available today as part of Amazon Ads API?  For what purpose should each be used?**

|Category	|API	|Description	|
|---	|---	|---	|---	|---	|
|Administration	|Account / instance management 	|Manage AMC instances	|
|Reporting	|Workflow execution	|Query data sources and generate reporting on demand	|
|Reporting	|Workflow scheduling	|Query data sources and generate reporting per schedule	|
|Audience management	|Rule-based audiences	|Create ruled-based audiences ready for Amazon DSP campaign activation 	|
|Audience management	|Lookalike audiences	|Create lookalike audiences ready for Amazon DSP campaign activation	|
|Audience management	|Advertiser audiences	|Stream pseudonymized audience records into Amazon Ads 	|
|Advertiser data upload	|Advertiser data upload	|Upload pseudonymized advertiser first-party signals to Amazon Ads	|

**Do AMC APIs support versioning?**

API versioning is the practice of managing changes to an API and ensuring that these changes are made without disrupting customers. We support versioning for AMC APIs. If we introduce any breaking changes in the future, these changes will be rolled out through new versions.

**Do AMC APIs have a rate limit or throttling?**

Rate limits, or throttling, are restrictions placed on the number of API requests a user or an application can make within a specified time frame. Such restrictions help prevent abuse and ensure fair usage of the APIs. AMC employs rate limiting for APIs.

Rate limits per API differ and are subject to change. Surpassing the rate limit will return a 429 error. These rate limits employ token bucket and should be handled by industry standard methods such as token bucket algorithm, exponential back off, and jitter.  There are resources online to help, including information [here](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/#Retries_and_backoff).  AMC recommends employing these industry-standard methods and not coding to specific rate limits.

**How do I deepen my AMC knowledge?**

You can explore the [AMC education resources](https://learningconsole.amazonadvertising.com/student/path/37211-amazon-marketing-cloud-education?sid=da9d26fa-93a7-4e15-8fbb-20c519d58d8b&sid_i=3) available on Amazon Ads learning console.


### Reporting APIs

**Where can I find the results of the reporting workflow execution?**

There are two ways for you to access workflow results

1. You can find the results in your “output” S3 bucket. The details of your S3 bucket can be located in the **Instance info** page of the AMC UI. Note that after migration, you must modify your S3 bucket’s IAM policy to continue receiving query execution results.
2. Alternatively, you can download reports through a pre-signed URL, which is generated by calling `GET /workflowExecutions/{workflowExecutionId}/downloadUrls`. Detailed instructions are [here](guides/amazon-marketing-cloud/reporting/workflow-management-service#download-your-report.).

**Are there any quotas on workflow executions?**

AMC has limits on the concurrency of workflow executions. Currently, the following limits are applied:

* 10 workflow executions can run in parallel at the instance level for API
* 10 workflow executions can run in parallel at the instance level for UI
* 200 workflow executions can run in parallel at the account level for API
* 50 workflow executions can run in parallel at the account level for UI

Note that, at the **account level**, there is no limit to the number of workflow executions queued. For example, at any given point, an account can run a maximum a maximum of 200 workflow executions running concurrently from the multiple instances associated with it, and have then any number of workflow executions from instances queued for processing.

> [NOTE] In case you reach the limit for execution, you can choose to either wait for running or queued workflow executions to complete, or to use the `DELETE /workflowExecutions/{workflowExecutionId}` endpoint to clear their queue.



**How do I handle "data gaps" in my query date ranges?**

AMC has introduced a new Data Availability Service Level Agreement (SLA) of 48 hours. This SLA guarantees that for all AMC instances, data will be available within 48 hours of its creation or update in the origin system, making the previous "Ignore data gaps" option redundant.

### Audiences APIs

**What is advertiser audiences API?**

The advertiser audiences API allows users to stream advertiser first-party audience lists to AMC and Amazon DSP.  Using rule-based audiences and lookalike audiences, you can create custom audiences using signals in AMC.

**How is the advertiser audience API different from the advertiser data upload API?**

Compared to the existing advertiser data upload API, this new API provides [pre-defined audience schema](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-prepare-data) tailored to audience retargeting or audience insight use cases.

Key benefits of using the new API for audiences:

* Follows industry-standard conventions for creating first-party audiences, similar to the method used by other publishers.
* Allows users the option to send hashed records to both AMC and Amazon DSP.
* Allows ingression of signals from any destination, without having to go through S3 buckets.
* Supports incremental refresh (add / move members) instead of full refresh; this helps to reduce processing latency as a result.

**Who should / when should I choose to use the advertiser audience API?**

Advertiser audience API is best for SaaS providers like CDPs, as well as agencies, or consultancies that would like to build “always on” advertiser data upload integrations. Direct advertisers who are willing to build and maintain an API integration with Amazon Ads can use this API as well.

However, if you expect to bring in first-party signals on sales, conversions, or other topic not related to audience, or only plan to upload data on an ad hoc basis, we recommend that you use advertiser data upload API instead.

### Advertiser data upload API

**How is advertiser data upload API different from other data onboarding mechanisms of Amazon ad tech?**

Detailed comparison across different onboarding mechanisms listed in the table below. The “Best for” row summarizes the scenario each onboarding option is suitable for.

![comparison of different onboarding mechanisms](/_images/amazon-marketing-cloud/1Ponboardingmethods.png)

**How is advertiser provided data handled in AMC?**

AMC is Amazon Ads’ clean room product. Each AMC user is provided with one or more AMC instances within an account for them to access. Each instance has isolated data from one another, meaning dataSets within each instance are independent and may have different rows from another instance. This applies both to the Advertiser Data loaded through AMC’s Advertiser Data Upload (ADU) APIs, as well as the standard dataSets made available in your AMC instance from Amazon Ads.

AMC supports AWS Key Management Service (KMS) encryption to further secure the data you would like to upload. The benefit of using a customer generated encryption key you the ability to revoke AMC’s access to previously uploaded data at any point. In addition, customers can monitor encryption key access via AWS CloudTrail event logs. Details on how to create encryption key with AWS KMS can be found [here](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-s3-bucket#create-encryption-key-with-aws-key-management-service-kms).

**How does AMC identity resolution work?**

AMC support three types of identifiers for resolution:

1. Hashed PII (with 8 fields for resolution)
2. Mobile Ad IDs (IDFA and AAID)
3. External Ids provided by our identity onboarding partners.

We do not support cookie-based or IP address-based resolution. When using Hashed PII or Mobile Ad Id based resolution, we perform a deterministic resolution process. This means that we only create a record with a resolved `user_id` when we find an exact pairing between the advertiser data provided and Amazon’s identity data. Once resolved, we drop all hashed PII or MAID values from the advertiser uploaded data and retain only a reference to the resolved Amazon rows.

**How do I improve my match rates?**

Amazon Ads uses a waterfall match which resolves uploaded records to Amazon Ads users by evaluating against most precise rules to least precise. Including as many supported matching keys (Hashed PII fields, MAID, or ExternalId) in a given upload request will result in higher match rates (Individual APIs may support fewer IDs than our overall supported IDs).

Some best practices include:

* Provide multiple supported match key types / fields.
* If one match types have multiple values, load them as one array vs. individual records.
* Make sure you normalize and hash the key in correct format as recommended by Amazon.
* If you use any 3rd party identity resolution services such as Acxiom and LiveRamp, use these services to augment the match.

> [WARNING] To comply with upcoming regulation, Amazon Ads requires advertisers to provide country information in the form of a two-character ISO 3166 Country Code. This code indicates from which country the data was collected and can be added when advertisers upload first-party data to Amazon Marketing Cloud (via Advertiser Data Upload API or Advertiser Audience API).

Specifically:

* Advertisers using instance-level ADU API (ADU 1.0) need to specify countryCode at upload-level.
* Advertisers using updated gateway ADU API (ADU 2.0) need to specify countryCode at row-level, upload-level, or dataset-level.
* Advertisers using Advertiser Audience API need to specify countryCode at row-level or dataset-level.

Starting 2/29, for data uploaded to AMC without countryCode info specified, the countryCode field will be left blank by default, signaling the system to not utilize the records during identity resolution. This will negatively impact record resolution rate, and lower the usability of the data you uploaded. The countryCode specification requirements will not impact data uploaded prior to 2/29. We strongly encourage all advertisers using ADU or Advertiser Audience API to start specifying countryCode as soon as possible in all uploads from now on to prevent experiencing negative outcomes described above.

**How do I delete data based on my compliance or business needs?**

AMC supports the ability to delete dataSets in their entirety, or the ability to overwrite a specific partition which will remove data from your AMC instance. Deleting a dataSet or overwriting a specific set of rows should only be used for functional deletion. We provide specific APIs for compliance specific use cases. Please see `user Deletion Request` APIs (also known as `dataDeletions` depending on the version of the API docs you are using.)
