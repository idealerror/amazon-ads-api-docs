---
title: Get started with Amazon Data Firehose
description: Get started with Firehose
type: guide
interface: amazon-marketing-stream
tags:
    - Amazon Data Firehose
keywords:
    - Amazon Marketing Stream
    - Amazon Data Firehose
    - getting started guide
---

# Amazon Data Firehose getting started guide


## Before you begin

To complete this tutorial, you will need:

* [Access to the Amazon Ads API](guides/onboarding/overview)
* [A valid Ads API access token](guides/get-started/retrieve-access-token)
* [Your Ads API profile ID](guides/get-started/retrieve-profiles)
* [An AWS account](https://aws.amazon.com/free/) with appropriate access. Learn more about [prerequisites for subscribing to the Firehose stream](https://docs.aws.amazon.com/sns/latest/dg/prereqs-kinesis-data-firehose.html)
* An understanding of your company's cloud infrastructure and the [final data output](guides/amazon-marketing-stream/onboarding/firehose/overview) for Stream data
* The ability to make basic cURL requests using a tool like Terminal or [Postman](https://www.postman.com/downloads/)
* An understanding of the [Amazon Marketing Stream datasets](guides/amazon-marketing-stream/data-guide)

## Get started with Amazon Data Firehose

Note that you must be logged into AWS in the correct region before you can get started. Use the table to select the correct region based on the location of the advertiser you are creating a subscription for:

|Advertiser region	|AWS region	|
|---	|---	|
|NA	|us-east-1	|
|EU	|eu-west-1	|
|FE	|us-west-2	|

For full details on the countries that have access to Amazon Marketing Stream, see [geographic availability](guides/amazon-marketing-stream/overview#geographic-availability).

### Manual set up instructions

The manual set up instructions walk through creating a Firehose stream and IAM roles manually in the AWS console. If you'd prefer to review the steps manually or using a data output other than S3, see [Manual steps](guides/amazon-marketing-stream/onboarding/firehose/manual-steps). Otherwise, we recommend using the CloudFormation template for faster setup. 

### CloudFormation template set up instructions

>[NOTE] This CloudFormation template only supports S3 as a data output. If you want to configure another data output (see [Overview](guides/amazon-marketing-stream/onboarding/firehose/overview) for all supported destinations), you will need to follow [the manual process](guides/amazon-marketing-stream/onboarding/firehose/manual-steps). 

**Important!** Use of the CloudFormation template will automatically provision the following AWS resources:

1. An S3 bucket
2. An Amazon Data Firehose stream
3. Three (3) IAM roles 

#### Steps

The following steps assume that you are logged in to your AWS account in the appropriate region as described above. 

1. Go to [GitHub](https://github.com/amzn/ads-advanced-tools-docs/tree/main/amazon_marketing_stream) and download a copy of `Stream-Firehose-S3-CF-Template.yaml`. 
2. From the [AWS console](https://aws.amazon.com/console/), search for “CloudFormation”.
3. From the CloudFormation page, go to **Create Stack** > **With new resources (standard)**.
4. Under **Prerequisite - prepare template**, make sure **Choose an existing template** is selected.
5. Under **Specify template**, select **Upload a template** file, then choose the CloudFormation template you downloaded in step 1. Click **Next**.
6. Enter a name for your stack. 
7. Under **S3Storage**, enter a name for the S3 bucket to be created.
8. Choose a Stream dataset ID that you want to be associated with this Firehose.
9. Enter a name for the **StreamDestinationFirehose**.
10. Under **StreamRealm**, choose a region associated to the subscription. Make sure this is the same as the AWS region you are signed in to. 
11. After you’ve completed all fields, click **Next**. An example of the fields to select if subscribing to the sp-traffic dataset in North America is shown below:

![Firehose CloudFormation config](/_images/amazon-marketing-stream/firehose-cf-template.png)

12. Review the **Configure stack options** page, then click **Next**.
13. On the **Review and create** page, review your inputs. At the bottom, check the box that says “I acknowledge that AWS CloudFormation might create IAM resources with custom names,” then click **Submit**.
14. You are taken to a status page where you can review the progress of your stack. 
15. Once your stack is complete, you can move on to the next step: [Subscribing to Stream datasets](guides/amazon-marketing-stream/onboarding/firehose/subscribing-to-datasets).

### Subscribing to Stream datasets

After creating your AWS resources, you need to subscribe to Amazon Marketing Stream datasets using the Amazon Ads API. For more information, see [Subscribing to datasets](guides/amazon-marketing-stream/onboarding/firehose/subscribing-to-datasets).

## Video demo

The video embedded below provides a comprehensive walkthrough and demo of Amazon Data Firehose, including how to set up IAM roles and create subscriptions. High-level timestamps are below in case you want to skip to a specific topic:

|Timestamp	|Topic	|
|---	|---	|
|0:00	|Intro and refresher on Amazon Marketing Stream	|
|2:12	|Painpoints when using only SQS	|
|3:25	|Difference between SQS and Firehose	|
|7:45	|Benefits of Firehose	|
|9:15	|Firehose demo: Data ingestion and analysis	|
|9:45	|Firehose demo: Setting up the delivery stream and IAM roles	|
|17:55	|Firehose demo: Creating subscriptions	|
|27:05	|Resources for getting started	|

<iframe sandbox="allow-scripts allow-same-origin" width="560" height="315" src="https://www.youtube.com/embed/ccw0WIIAMvk?si=V7xugxTp2aT3PbYM" title="YouTube video player" frameborder="0" allowfullscreen></iframe>

