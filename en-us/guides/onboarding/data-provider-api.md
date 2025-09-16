---
title: Additional setup steps for the data provider API
description: Describes additional setup steps for the data provider API, including access to billing reports.
type: guide
interface: api
tags:
    - Onboarding
    - Data provider
keywords:
    - setup
    - billing report
    - AWS 
    - S3 
    - IAM
---

# Additional setup steps for the data provider API

This document describes the additional setup steps required to onboard an application to use the data provider API. 

## Prerequisites

Before moving on to the additional setup steps for the data provider API, verify that you have completed all the [account setup steps for all APIs](guides/onboarding/overview).

As part of the API onboarding process, you were invited via email to use Jira for technical support. You must have followed the steps in the invitation and created a Jira account before you continue with setup for the data provider API. 

## Submit an onboarding request in Jira

To gain access to the data provider API, submit a support ticket via Jira stating that you'd like to be onboarded to use the data provider API. Select **/dp Data Provider** as the **Symptom Path** when submitting the ticket. 

You will receive a response through Jira.

## Additional billing report setup steps (optional)

Amazon Ads provides data provider monthly audience usage delivered in billing reports. Integrators who create segments with positive CPM will need to set up billing reports during the onboarding process. 

Billing reports are typically available on the 11th day of each month. These reports are delivered to an [Amazon S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html) to which your authorized [AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/) users or roles have access. 

### Prerequisites

Before you begin the additional billing report setup steps, you must have an [AWS account](https://aws.amazon.com/account/). If you do not have an existing AWS Account, [create a new AWS account](https://portal.aws.amazon.com/billing/signup#/start) now.

A billing account is associated with one or more AWS Identity Access Management (IAM) users or roles. If you are not familiar with IAM, we recommend that you read and understand the topics in the [What is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) documentation. After you've read this documentation, you'll understand the differences between users and roles and will be able to decide if you should manage access to your billing reports using a [role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html), a [user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html), or both. 

If you've decided to [create one or more users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html), you must complete this step before continuing with billing setup. If you've decided to [create one or more roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html), you must complete this step before continuing with billing setup.

### Billing report setup

Make note of the [IAM Amazon Resource Name (ARN)](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html) that you created in the prerequisites.

Next, create a Jira support ticket with the title **[Access Request - Billing] Request Access to Billing Reports**. In the body of the support ticket, enter the following:

>I have set up as a Developer User of the Data Provider API (link: https://advertising.amazon.com/API/docs/en-us/guides/onboarding/data-provider-api). I am requesting access to my Billing Reports. <br/><br/>Please grant access to my Billing Reports to these authorized AWS IAM Roles/Users, identified by ARN:<br/><br/>\<\< paste IAM role/user ARNs here \>\>

Once your Jira ticket has been processed, you will receive a confirmation email that includes billing report access instructions.

### Steps to be able to raise invoices (optional)

If you expect to charge audience fees, you will need to onboard to Payee Central to be able to raise invoices. You need to onboard to Payee Central for every country you want to do business in. Please follow the following steps:

1. [Payee Central eLearning Portal](https://amazonexteu.qualtrics.com/jfe/form/SV_cOL2E5ISvOTVcjQ) is available to guide you through the onboarding process.
1. If you need additional assistance, you can sign up for our [Payee Central Office Hours](https://amazonexteu.qualtrics.com/jfe/form/SV_cOL2E5ISvOTVcjQ).
1. For assistance with Payee Central and any related onboarding/ongoing tasks, reach out using the Contact Us tab in Payee Central.

## Next steps

Now that you have onboarded to the data provider API, you can make API calls. To begin, learn how to [create API authorization tokens](guides/get-started/overview).
