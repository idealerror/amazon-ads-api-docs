---
title: FAQ related to getting started with Amazon Data Firehose
description: FAQ related to getting started with Amazon Data Firehose
type: guide
interface: amazon-marketing-stream
tags:
    - Amazon Data Firehose
keywords:
    - Amazon Marketing Stream
    - Amazon Data Firehose
    - getting started guide
---

# Amazon Marketing Stream and Data Firehose FAQ

## SQS vs. Firehose

### Does Amazon Marketing Stream delivery to Firehose differ from SQS? 

Data is still delivered at least once, with potential duplicates. There is no guarantee of ordering in either Firehose or SQS.

### Is the format of Amazon Marketing Stream data different in Firehose versus SQS? 

There is no change in data format. Both Firehose and SQS receive JSON from Amazon Marketing Stream.

However, Firehose supports parquet/orc conversion out-of-the-box when you write your data to Amazon S3. Firehose also integrates with Lambda, so you can write your own transformation code. Additionally, Firehose has built-in support for extracting the key data fields from records that are in JSON format. Firehose also supports the JQ parsing language to enable transformations on those partition keys. 

To learn more, see the [AWS Firehose developer guide](https://docs.aws.amazon.com/firehose/latest/dev/data-transformation.html).

### What is the behavior of `idempotency_id` if I'm using both Firehose and SQS?

The data from SQS subscriptions is identical to that from Firehose. Firehose serves as another destination option for added flexibility. Each message still carries a unique `idempotency_id`. If you create a new subscription to Firehose in addition to SQS, the unique `idempotency_id` will remain identical across both platforms.

### We are currently using SQS to receive data. What is the recommended strategy for migrating subscriptions to Firehose?

>[WARNING] Important! We recommend this migration be completed by an experienced AWS administrator.

A potential approach could be:

1. Run both SQS and Data Firehose together temporarily. Use an "idempotency_id" in your application to deduplicate messages.
2. (Optional) For a controlled switch, migrate new subscriptions to Firehose first, then existing ones.
3. Monitor both services and verify data consistency throughout.
4. Once confident, stop processing messages from SQS. Consider keeping SQS as a backup for a short while before removing it.

### What is the cost difference between Firehose and SQS?

As every integration and volume of messages is different, we recommend using the AWS cost calculator to get an estimate based on your business.

[AWS Cost Calculator](https://calculator.aws/#/)

### Is there any plan to deprecate the SQS integration?

No, we are not moving from SQS. We're adding a new destination to provide more flexibility for customers. Firehose's built-in support for data transformation and integration with various tools like S3, Redshift, Snowflake, and Elasticsearch empowers customersrs to streamline their data pipelines.

## Onboarding

### Should I use a CloudFormation template to set up Firehose, or should I configure my Firehose stream manually?

If you plan to use S3 as your data output, we strongly recommend [using the CloudFormation template](guides/amazon-marketing-stream/onboarding/firehose/get-started#cloudformation-template-set-up-instructions). This method avoids the manual set-up, and helps ensure that your region and subscription details are done correctly. 

### Where can I find the Amazon Data Firehose ARN values needed for subscriptions?

To subscribe to a dataset and deliver to Data Firehose, you will need three ARN values, as shown below. All ARN values can be found in the [AWS console](https://aws.amazon.com). 

1. Delivery Firehose stream ARN: This ARN value is found in the main account where you created the initial Firehose stream. **Go to Amazon Data Firehose** > **Firehose streams** > [YOUR_STREAM] to locate it.
2. Subscription role ARN: Go to **IAM** > **Roles** and locate the subscription role from the list. Click into the role to find the ARN.  
3. Subscriber role ARN: Go to **IAM** > **Roles** and locate the subscriber role from the list. Click into the role to find the ARN. 

### Does Firehose support backfilling?

Backfilling of data is not currently supported in Amazon Marketing Stream. 

## Firehose management

### How do I unsubscribe from a dataset delivering to Firehose?

Get a `subscriptionId` of the subscription you'd like to delete using [GET /streams/subscriptions](/amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/ListStreamSubscriptions) (sponsored ads datasets) or [GET /dsp/streams/subscriptions](amazon-marketing-stream/openapi#tag/DSP-Stream-Subscription/operation/ListDspStreamSubscriptions) (Amazon DSP datasets).

Subscriptions delivering to Firehose will contain a `firehoseDestination` object. 

Once you have the subscriptionId, use PUT /streams/subscriptions/{subscriptionId} (sponsored ads datasets)  or PUT /dsp/streams/subscriptions/{subscriptionId} (Amazon DSP datasets) or set the status of the subscription to `ARCHIVED`.

For more information, see [Managing subscriptions](guides/amazon-marketing-stream/managing-subscriptions).

### We noticed that Firehose delivers data to S3 with a file extension that cannot be configured. Can we change this format without using a Lambda?

You can set a file extension for objects delivered to the Amazon S3 destination bucket by enabling the S3 file extension format feature. 

In the AWS console, go to Firehose. Under **Buffer hints, compression, file extension and encryption** settings, set the file extension to .txt, enter “.txt” (including the period) in the S3 file extension format field. Firehose will override the default file extensions appended by Data Format Conversion or S3 compression features. [Learn more](https://docs.aws.amazon.com/firehose/latest/dev/create-destination.html#create-destination-s3).

### I've noticed Firehose adds a timestamp as a prefix to my file fame in S3. Can I remove this?

Yes, you can configure Data Firehose to not add a timestamp prefix to files when storing them in Amazon S3. To achieve this, you need to customize the S3 prefix in the delivery stream configuration.

1. Open the Firehose console.
2. Select your delivery stream.
3. In the **S3 destination** section, click **Edit**.
4. In the **S3 prefix** field, remove the default timestamp prefix or modify it according to your requirements.
5. Click **Save changes** to apply the modifications.

For more detailed instructions, you can refer to the [official Firehose documentation](https://docs.aws.amazon.com/firehose/latest/dev/create-destination.html#create-destination-s3).
