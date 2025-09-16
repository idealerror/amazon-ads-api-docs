---
title: Amazon Marketing Stream onboarding guide
description: Instructions for setting up Amazon Marketing Stream
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Display
    - Onboarding
keywords:
    - stream 
    - get started with stream
    - subscribe to stream datasets
    - AWS 
    - stream datasets 
    - create SQS queue 
    - CloudFormation
    - SQS 
---
# Amazon Marketing Stream onboarding guide

Follow the steps in this tutorial to connect your AWS application to Amazon Marketing Stream datasets. At the end of the tutorial, you should start receiving performance and campaign data in your SQS queue. 

## Before you begin 

To complete this tutorial, you will need:

* [Access to the Amazon Ads API](guides/onboarding/overview)
* [A valid Ads API access token](guides/get-started/retrieve-access-token)
* [Your Ads API profile ID](guides/get-started/retrieve-profiles) 
* [An AWS account](https://aws.amazon.com/free/)
* The ability to make basic cURL requests using a tool like Terminal or [Postman](https://www.postman.com/downloads/)
* An understanding of the [Amazon Marketing Stream datasets](guides/amazon-marketing-stream/data-guide)

>[NOTE] During the Amazon Ads API onboarding process, you will need to create an Amazon Login With Amazon account. If you're creating an application associated with a business, we recommend you use an email address that is managed by multiple individuals in your organization. This will ensure that you don't lose access to your account if an individual leaves your organization. 

>[TIP] Onboarding to Stream using our reference application. Our python-based reference application used the AWS CDK to help you programmatically complete the steps listed in the document. [Learn more](guides/amazon-marketing-stream/reference-implementation). 

## Step 1: Create an SQS queue

To receive data using Amazon Marketing Stream, you must first set up a queue using AWS Simple Queue Service (SQS). *We recommend your AWS set up be completed by someone familiar with AWS.*

You can create a queue by logging in to your AWS account and using the [SQS console](https://console.aws.amazon.com/sqs/) or by using an AWS CloudFormation template. 

When using AWS (both SQS and CloudFormation), its important to make sure that you are logged in to the correct region. Use this table to select the correct region based on the location of the advertiser you are creating a subscription for.

| Advertiser region | AWS region |
|---------|----------|
| NA | us-east-1 |
| EU  | eu-west-1 |
| FE | us-west-2 |

You can select a region using the dropdown in the top right corner of the AWS console.

![AWS console region selector](/_images/amazon-marketing-stream/aws-console-region.png)

For full details on what countries are supported, see the [Geographic availability](guides/amazon-marketing-stream/overview#geographic-availability). 

### CloudFormation

>[TIP] To avoid errors during onboarding, we recommend using the CloudFormation template instead of manually setting up the queue in the SQS Console. 

To use a CloudFormation template, follow the instructions in [Onboarding with CloudFormation](guides/amazon-marketing-stream/cloud-formation). 

Our CloudFront template automatically adds a valid resource-based IAM policy, so if you are using CloudFormation, you can skip step 2 in this guide and move directly to [step 3](#step-3-subscribe-to-amazon-marketing-stream-datasets).

### SQS Console

You can also create SQS queues manually using the [SQS console](https://console.aws.amazon.com/sqs/). Follow the instructions in the [AWS documentation](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-configure-create-queue.html). 

>[NOTE] You must create a **Standard** queue; FIFO queues are not currently supported. 

## Step 2: Add a resource-based IAM policy

>[WARNING] You must add a valid IAM policy to your queue in order to receive Amazon Marketing Stream data.

Each combination of dataset and region has a distinct IAM policy. For example, the sp-traffic dataset has different IAM policies for NA, EU, and FE. Make sure you are applying the correct IAM policy based on the dataset and region that you want to subscribe to. To view resourced-based IAM policies for all available datasets and regions, see the [Amazon Marketing Stream data guide](guides/amazon-marketing-stream/data-guide). 

You can add an access policy during queue creation, or [edit an existing queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-configure-add-permissions.html).  

>[NOTE] When inserting an IAM policy, make sure `Resource` points to your own queue's Amazon Resource Name (ARN).

## Step 3: Subscribe to Amazon Marketing Stream datasets

>[WARNING] Before sending a subscription request, make sure you have already [applied the IAM policy](#step-2-add-a-resource-based-iam-policy) associated with the dataset to your SQS queue. 

After setting up a queue and adding an IAM policy, you can subscribe to advertiser data using the Amazon Marketing Stream [subscription APIs](amazon-marketing-stream/openapi). You need to make a separate request for each dataset you want to subscribe to.

>[WARNING] There are two sets of subscription APIs, one for sponsored ads datasets and one for Amazon DSP datasets. For the relevant subscription endpoint for each dataset, see [Datasets overview](guides/amazon-marketing-stream/datasets/dsp-campaign-management).

>[TIP] Use our [Postman collection](guides/postman) to test the Amazon Marketing Stream APIs. Once you've set up the collection and environment, check out the **Amazon Marketing Stream** folder to call the API. 

### Subscribing to sponsored ads datasets

To subscribe to sponsored ads datasets, use the [POST /streams/subscriptions endpoint](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription).

If you are subscribing to data for multiple [profiles](guides/account-management/authorization/profiles), you must make a separate call for each. 

**Headers**

| Parameter | Description |
|-----------|-------------|
| Amazon-Advertising-API-ClientId | The [client ID](guides/onboarding/create-lwa-app) associated with your Login with Amazon application. |
| Amazon-Advertising-API-Scope | Your [profile ID](guides/account-management/authorization/profiles). |
| Authorization | Your [access token](guides/account-management/authorization/access-tokens). |
| Content-Type | `application/vnd.MarketingStreamSubscriptions.StreamSubscriptionResource.v1.0+json` |

**Parameters**

| Parameter | Required | Description |
|-----------|-------------|
| `dataSetId` | Yes | To view available datasets, see the [data guide](guides/amazon-marketing-stream/data-guide).  |
| `destinationArn` | Yes | The ARN of the SQS queue where you want to receive data. |
| `clientRequestToken` | Yes | Unique value supplied by the caller used to track identical API requests. Should request be re-tried, the caller should supply the same value. We recommend using GUID. |
| `notes` | No | Additional details which can be used to identity the destination. |

**Example**

Make sure to replace `Amazon-Advertising-API-ClientId`, `Amazon-Advertising-API-Scope`, `Authorization`, and `destinationArn` with the values specific to your account. The `clientRequestToken` can be any idempotency token that could be used to identify the request if needed. 

```bash
    curl --location --request POST 'https://advertising-api.amazon.com/streams/subscriptions' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Scope: xxxxxxx' \
    --header 'Content-Type: application/vnd.MarketingStreamSubscriptions.StreamSubscriptionResource.v1.0+json' \
    --header 'Authorization: Bearer xxxxxxxxx' \
    --data-raw '{
    "clientRequestToken": "123456789xyz1234567",
    "dataSetId": "sp-conversion",
    "notes": "Advertiser 1 sp-conversion subscription",
    "destinationArn": "QUEUE_ARN"
    }‘
```

**Response**

A successful request results in a response that includes a `subscriptionId`. 

```json
{
    "subscriptionId": "xxxxxxxxxxxxxx",
    "clientRequestToken": "123456789xyz1234567",
}
```

### Subscribing to Amazon DSP datasets

To subscribe to Amazon DSP datasets, use the [POST dsp/streams/subscriptions endpoint](amazon-marketing-stream/openapi#tag/DSP-Stream-Subscription/operation/CreateDspStreamSubscription).

If you are subscribing to data for multiple Amazon DSP advertiser IDs, you must make a separate call for each. 

**Headers**

| Parameter | Description |
|-----------|-------------|
| Amazon-Advertising-API-ClientId | The [client ID](guides/onboarding/create-lwa-app) associated with your Login with Amazon application. |
| Amazon-Advertising-API-Account-ID | The Amazon DSP advertiser ID that you want to receive data for via Stream. You can view the advertiser IDs associated with you profile using [GET /dsp/advertisers](dsp-advertiser/#tag/Advertiser/paths/~1dsp~1advertisers/get). |
| Authorization | Your [access token](guides/account-management/authorization/access-tokens). |
| Content-Type | `application/vnd.MarketingStreamSubscriptions.DspStreamSubscriptionResource.v1.0+json` |

**Parameters**

| Parameter | Required | Description |
|-----------|-------------|
| `dataSetId` | Yes | To view available datasets, see the [data guide](guides/amazon-marketing-stream/data-guide).  |
| `destinationArn` | Yes | The ARN of the SQS queue where you want to receive data. |
| `clientRequestToken` | Yes | Unique value supplied by the caller used to track identical API requests. Should request be re-tried, the caller should supply the same value. We recommend using GUID. |
| `notes` | No | Additional details which can be used to identity the destination. |

**Example**

Make sure to replace `Amazon-Advertising-API-Account-ID`, `Amazon-Advertising-API-ClientId`, `Authorization`, and `destinationArn` with the values specific to your account. The `clientRequestToken` can be any idempotency token that could be used to identify the request if needed. 

```bash
    curl --location --request POST 'https://advertising-api.amazon.com/dsp/streams/subscriptions' \
    --header 'Amazon-Advertising-API-ClientId: xxxxxx' \
    --header 'Amazon-Advertising-API-Account-ID: xxxxxxx' \
    --header 'Content-Type: application/vnd.MarketingStreamSubscriptions.DspStreamSubscriptionResource.v1.0+json' \
    --header 'Authorization: Bearer xxxxxxxxx' \
    --data-raw '{
    "clientRequestToken": "123456789xyz1234567",
    "dataSetId": "adsp-campaigns",
    "notes": "Advertiser 1 adsp-campaigns subscription",
    "destinationArn": "QUEUE_ARN"
    }‘
```

**Response**

A successful request results in a response that includes a `subscriptionId`. 

```json
{
    "subscriptionId": "xxxxxxxxxxxxxx",
    "clientRequestToken": "123456789xyz1234567",
}
```

## Step 4: Confirm your subscription in SQS

Once you have successfully subscribed to Amazon Marketing Stream using the API, you need to confirm the subscription in SQS to start receiving data. 

You should see a message in your SQS queue resembling:

```json
{
    "Type" : "SubscriptionConfirmation",
    "MessageId" : "165545c9-2a5c-472c-8df2-7ff2be2b3b1b",
    "Token" : "2336412f37...",
    "TopicArn" : "arn:aws:sns:us-west-2:123456789012:MyTopic",
    "Message" : "You have chosen to subscribe to the topic arn:aws:sns:us-west-2:123456789012:MyTopic.To confirm the subscription, visit the SubscribeURL included in this message.",
    "SubscribeURL" : "https://sns.us-west-2.amazonaws.com/?Action=ConfirmSubscription&TopicArn=arn:aws:sns:us-west-2:123456789012:MyTopic&Token=2336412f37...",
    "Timestamp" : "2012-04-26T20:45:04.751Z",
    "SignatureVersion" : "1",
    "Signature" : "EXAMPLEpH+DcEwjAPg8O9mY8dReBSwksfg2S7WKQcikcNKWLQjwu6A4VbeS0QHVCkhRS7fUQvi2egU3N858fiTDN6bkkOxYDVrY0Ad8L10Hs3zH81mtnPk5uvvolIC1CXGu43obcgFxeL3khZl8IKvO61GWB6jI9b5+gLPoBc1Q=",
    "SigningCertURL" : "https://sns.us-west-2.amazonaws.com/SimpleNotificationService-f3ecfb7224c7233fe7bb5f59f96de52f.pem"
}
```

To start receiving data in your queue, you must confirm the subscription in one of the following ways: 

1. Make a GET request to the `SubscribeURL` value returned in the SQS queue confirmation message. This call should return a message saying that subscription has been confirmed and the queue should start receiving messages.
    
    **Example confirmation message**

    ```
    <ConfirmSubscriptionResponse>
        <ConfirmSubscriptionResult>
            <SubscriptionArn>arn:aws:sns:us-east-2:123456789012:MyTopic:1234a567-bc89-012d-3e45-6fg7h890123i</SubscriptionArn>
        </ConfirmSubscriptionResult>
        <ResponseMetadata>
            <RequestId>abcd1efg-23hi-jkl4-m5no-p67q8rstuvw9</RequestId>
        </ResponseMetadata>
    </ConfirmSubscriptionResponse>
    ```
2. Use the [AWS SDK](https://docs.aws.amazon.com/sns/latest/api/API_ConfirmSubscription.html) to call `ConfirmSubscription` using the `Token` and `TopicArn` values returned in the SQS queue confirmation message.

 >[NOTE] You must confirm the subscription for each advertiser and dataset you are subscribing to. 

 If you are receiving data for multiple advertisers or datasets in the same queue, you should consider using option 2 above to create an application that programmatically confirms subscriptions. The application should listen to the queue and distinguish confirmation messages from data events. 

You can identify subscription messages versus data events using logic similar to:

```
    if "Type" in content and content['Type'] == 'SubscriptionConfirmation':

    def confirm_subscription(content):
        token = content['Token']
        topicArn = content['TopicArn']
        print(f"Confirming subscription for {topicArn}")
        sns.confirm_subscription(TopicArn=topicArn, Token=token)
```

## Subscription status flow

You can view the status flow for Stream subscriptions in the following diagram.

![Stream status flow diagram](/_images/amazon-marketing-stream/state-diagram.png)

Learn more about the possible statuses for subscriptions in [Managing subscriptions](guides/amazon-marketing-stream/managing-subscriptions#possible-statuses).

## Next steps

Once you've confirmed your subscriptions and started receiving data, you can move on to:

- [Aggregating data](guides/amazon-marketing-stream/aggregating-data)
- [Querying data](guides/amazon-marketing-stream/querying-data)
- [Managing subscriptions](guides/amazon-marketing-stream/managing-subscriptions)
- [AWS best practices](guides/amazon-marketing-stream/aws-best-practices)
- [AWS serverless application repository](https://serverlessrepo.aws.amazon.com/applications)
- [SQS poller](https://serverlessrepo.aws.amazon.com/applications/us-east-1/077246666028/sqs-poller)

## AWS Workshop
 
This Workshop provides an example of a serverless data analytics and managed machine learning platform built using Amazon Marketing Stream and AWS technology. Developers, data analysts, and architects can use the Workshop to get a practical idea of how Stream might be implemented to help your business. 
 
[View the Workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/73553fea-93ea-4091-a871-89ba40dc2dec/en-US).

## Troubleshooting FAQ

### Why am I seeing negative values for the first couple of days of data?

The negatives are corrections that are happening retroactively based on Amazon’s click and conversion validation process. You may have negative aggregated values for the first day or two that we enabled the connection as you didn’t receive original traffic on those days, just corrections. The recommendation is to ignore negative values in the first one to two days. 

After the first two days, you should start receiving positive values; if you do not see aggregate positive values after two days, contact support. 

You will continue to see negative deltas as corrections happen due to click invalidation, but they should always aggregate to positive values.

### Why are I receiving null records?

You can disregard these rows. This is expected and these these rows are not an issue and will cause no issues with data as they aggregate to 0.

### I have created queue and while I am trying to subscribe using  POST /streams/subscriptions I am getting error code 400 that my region is not supported.

In the current state, Amazon Marketing Stream supports only three regions: us-east-1 (NA), eu-west-1 (EU), and us-west-2 (FE). When creating your SQS queue, make sure you are only using a supported region. To avoid this error, we recommend using the provided [CloudFormation template](guides/amazon-marketing-stream/cloud-formation) to create your queue.

### My subscription status is `PENDING_CONFIRMATION`, but I never received a confirmation message in SQS.

You may have applied the wrong IAM policy to your queue. Make sure that the IAM policy you are using in your queue matches the region of the advertiser profile and the dataset that you are trying to subscribe to. 

If you applied the wrong IAM policy to your queue, you should create a new queue, apply the correct policy, and subscribe to the dataset again using the new queue ARN.  

>[TIP] We suggest using the provided CloudFormation template to create your queues to reduce the risk of manual errors.

If you are sure that you applied the correct IAM policy, but have been stuck in `PENDING_CONFIRMATION`, [submit a ticket](https://amzn-clicks.atlassian.net/servicedesk/customer/portal/2/group/2) and our support team can manually resend the confirmation message.  

### My subscription is stuck in `PENDING_CONFIRMATION` and won’t move to `FAILED_CONFIRMATION`.

If you don’t confirm a subscription within three days, the status should automatically move to `FAILED_CONFIRMATION`. Once the status has moved to `FAILED_CONFIRMATION`, you can send another request to subscribe to the dataset. In rare cases, the status may not switch to `FAILED_CONFIRMATION` automatically, and you should [submit a ticket](https://amzn-clicks.atlassian.net/servicedesk/customer/portal/2/group/2) to our support team.

Alternatively, you can create a new queue, apply the relevant IAM policy, and try to subscribe to the dataset again using the new queue ARN. 

### I have successfully confirmed my subscription in SQS, but I not receiving any messages in my SQS destination queue.

Since Amazon Marketing Stream delivers data in near real time, an event related to the advertiser and their campaign in the subscribed dataset and marketplace must occur for anything to be sent. In the absence of these events, there will no data. In other, words, data is only sent if there is performance for that campaign within the hour.

### It looks like I am receiving duplicate data.

If it looks like you are receiving duplicate records, be sure to inspect your data first. There are a couple of scenarios where the data you receiving might resemble duplicates, but is actually valid.

First, it’s possible that your data for two different hours could be very similar. For example, consider a keyword that receives ten impressions and three clicks on a given date in hour one. In hour two, they again receive ten impressions and three clicks. Stream would send similar records for that keyword in both hours, differentiated only by the hour the data is related to. For keywords without many impressions, it's likely you might see only one impression and no clicks for many hours throughout the day.      

Second, Amazon Marketing Stream sends an hourly delta for your campaigns, so restatements can result in records that appear almost identical. This could lead to two records with the same click and impression data for the same keyword and same hour, however one is the original delta and the second is a restatement. This is not a duplicate and can be checked by examining the `idempotency_id` of the two records. If the idempotency IDs are different, than they are not duplicates and are two distinct records for that keyword and hour.

### How do I cancel a subscription? 

There is not currently a way to delete or cancel a subscription. To stop receiving data for a subscription, you should set the `subscriptionStatus` to `ARCHIVED`. For more information, see [Managing subscriptions](guides/amazon-marketing-stream/managing-subscriptions).

### What is the time zone associated with Amazon Marketing Stream data?

For hourly data, datasets contain a `time_window_start` field that represents the start timestamp of the activity within the advertiser’s time zone. You can check the time zone associated with an advertiser profile by calling [GET /v2/profiles](reference/2/profiles#tag/Profiles/operation/listProfiles).

### How do I ensure that I am receiving all Amazon Marketing Stream data?

[AWS CloudWatch](https://aws.amazon.com/cloudwatch/) provides a variety of metrics for SQS queues including the number of messages received, the number of messages deleted, and the number of messages in the queue. Partners can monitor these metrics to identify any discrepancies. For example, if the number of messages received is greater than the number of messages deleted plus the number of messages in the queue, then some messages may be missing.


