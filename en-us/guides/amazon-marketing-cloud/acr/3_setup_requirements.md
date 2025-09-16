---
title: Amazon Marketing Cloud - AWS Clean Rooms
description: AMC - AWS Clean Rooms
type: guide
interface: api
---
# Requirements for AMC-AWS Clean Rooms set up

> [WARNING] You may need to work with your security, legal or IT teams for use of AWS or first-party (1P) signals.

To work with AMC–AWS Clean Rooms, you must have:

- [Access to AMC](#access-to-amc)
- [Access to AWS](#access-to-aws)

## Access to AMC

You must be eligible to onboard AMC or are a current AMC customer.

The requirements to be eligible to onboard AMC are listed below:

- An executed Amazon Demand Side Platform (Amazon DSP) Master Service Agreement (MSA), which in the future will be a standalone AMC agreement.
- Planned campaigns, or campaigns that were live in the last 28 days.
- DSP advertiser IDs and Sponsored Ads IDs (if applicable) to be included in your AMC instance.
- SQL-capable resource to pull an insight and work with supported SQL commands as listed in [AWS Clean Rooms SQL Reference](https://docs.aws.amazon.com/clean-rooms/latest/sql-reference/index.html).

You can also leverage AMC-AWS Clean Rooms if you are an existing AMC customer, with:

- 1P data uploaded into AMC,
- or, with available 1P data to upload into AMC,
- or, with available 1P identity data that want to map with your AMC data.

  We recommend a [certified AMC Data Analyst](https://learningconsole.amazonadvertising.com/student/collection/8532/path/77501) to help you with advanced insights planning.

To create id mapping tables for identity data, and to access AMC–AWS Clean Rooms programmatically (through APIs), you will need authenticated access to the Amazon Ads API. For details, see [Amazon Ads API onboarding overview](guides/onboarding/overview).

For programmatic access to the Amazon Ads API, you will need to perform the following steps:

1. Create a **[Login with Amazon application](guides/onboarding/create-lwa-app)**.
2. **[Apply for Amazon Ads API access](guides/onboarding/apply-for-access#next-steps).**

   When applying, ensure that you access and review the information in the following:

   - [Amazon Ads API License Agreement](https://advertising.amazon.com/API/docs/license-agreement)
   - [Data Protection Policy](https://advertising.amazon.com/API/docs/policy/en_US)
   - [Amazon Marketing Cloud Terms](https://advertising.amazon.com/marketing-cloud/terms)
3. [Assign API access to a Login with Amazon application](guides/onboarding/assign-api-access)

   - [Create an authorization grant](guides/get-started/create-authorization-grant)
   - [Retrieve access and refresh tokens](guides/get-started/retrieve-access-token) to authenticate and authorize your client id for using Amazon Ads API.

## Access to AWS

Since AWS Clean Rooms is an AWS service, when applying for AMC–AWS Clean Rooms, you must:

- [Sign up for AWS](https://signin.aws.amazon.com/signup?request_type=register) to create an AWS account with administrative permissions.
- [Create an administrator user](https://docs.aws.amazon.com/clean-rooms/latest/userguide/setting-up-roles.html#setting-up-create-iam-user) for AWS Clean Rooms.
  An administrator user determines which permissions should be assigned to different users accessing your collaboration instance.
- [Create IAM roles for collaboration members](https://docs.aws.amazon.com/clean-rooms/latest/userguide/setting-up-roles.html#create-role-DP)
  A collaboration member is a participant in the collaboration. The administrator user creates this role using AWS managed policies, depending on your use case (View only, Query, Query and receive results, or Manage collaboration resources but do not query).
- [Set up service roles for AWS Clean Rooms](https://docs.aws.amazon.com/clean-rooms/latest/userguide/setting-up-roles.html#create-role-DP)

  - [Create a service role to read data](https://docs.aws.amazon.com/clean-rooms/latest/userguide/setting-up-roles.html#create-service-role-procedure)

    A service role to read data is for any collaboration member who brings their datasets to a collaboration. The administrator creates this role for you, or, if you already have the necessary IAM permissions (`iam:CreateRole`, `iam:CreatePolicy`, and `iam:AttachRolePolicy`), you use the AWS Clean Rooms console to create a service role.
  - [Create a service role to receive results](https://docs.aws.amazon.com/clean-rooms/latest/userguide/setting-up-roles.html#create-role-write-results)

    A service role to receive results is only for the collaboration member who is receiving the results of the query. The administrator creates this role for you, or, if you already have the necessary IAM permissions (`iam:CreateRole`, `iam:CreatePolicy`, and `iam:AttachRolePolicy`), you use the AWS Clean Rooms console to create a service role.

  You will be using the following services on AWS:

  - [Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) to upload/store/retrieve data.

  > [WARNING] When defining your S3 buckets, know that the bucket for receiving query results **cannot** be the same as the bucket that stores your 1P data brought into the collaboration.
  >

  - [AWS Clean Rooms](https://docs.aws.amazon.com/clean-rooms/latest/userguide/what-is.html) to set up collaborations.
  - [AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html) for data cataloging.
  - [AWS Entity Resolution](https://docs.aws.amazon.com/entityresolution/latest/userguide/what-is-service.html) for rule-based matching of identity signals.
  - [AWS Identity and Access Management (IAM)](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) to manage secure access to AWS resources.
  - [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) (CLI) to interact with AWS Entity Resolution and AWS Clean Rooms APIs.

## Create an AMC collaboration instance

If you meet the above prerequisites, the eligibility and usage for AMC, you can programmatically create your own  collaboration instance in AMC for AWS Clean Rooms.

After your collaboration instance is created in AMC, you will need to join a collaboration in AWS Clean Rooms (or through the AMC console) to begin working with AMC–AWS Clean Rooms and query your data. For more details, see [Set up a collaboration instance](guides/amazon-marketing-cloud/acr/4_setup_collab).
