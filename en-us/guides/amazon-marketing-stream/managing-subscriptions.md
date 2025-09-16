---
title: Managing Amazon Marketing Stream Subscriptions
description: Instructions for managing Amazon Marketing Stream subscriptions, including how to check subscription status and archive a subscription 
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Display
keywords:
    - stream 
    - subscribe to stream data
    - subscription status
    - archive subscription
---

# Managing Amazon Marketing Stream Subscriptions

Once you have completed the [initial Amazon Marketing Stream setup](guides/amazon-marketing-stream/onboarding), you can use the following operations to manage your view, add, update, and archive subscriptions. 

>[NOTE] To manage subscriptions any any [DSP datasets](guides/amazon-marketing-stream/datasets/dsp-campaign-management), you need to use the Amazon DSP-specific [API endpoints](amazon-marketing-stream/openapi#tag/DSP-Stream-Subscription).

>[TIP] Use our [Postman collection](guides/postman) to test the Amazon Marketing Stream APIs. Once you've set up the collection and environment, check out the **Amazon Marketing Stream** folder to call the API. 

>[TIP] Try the CLI. A CLI to handle all operations in this document is available as part of our Amazon Marketing Stream [reference implementation on GitHub](https://github.com/amzn/amazon-marketing-stream-examples). 

## View all subscriptions

To see all dataset subscriptions associated with an advertising profile, use the [GET `/streams/subscriptions`](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/GetStreamSubscription) endpoint.

>[NOTE] When making API calls, '/streams/subscriptions' endpoint can accept either a profile ID (as [Amazon-Advertising-API-Scope]) or a DSP advertiser ID (as [Amazon-Ads-AccountID]), whereas the [dsp/streams/subscriptions] endpoint will only accept Amazon-Ads-Account-ID.

### Request

This resource returns subscriptions created for the [profile ID](guides/account-management/authorization/profiles) passed in `Amazon-Advertising-API-Scope`. You can control the number of results returned per page using the `maxResults` query parameter. 

#### Sample

```bash
curl --location --request GET 'https://advertising-api.amazon.com/streams/subscriptions?maxResults=10' \
--header 'Amazon-Advertising-API-ClientId: xxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
--header 'Authorization: Bearer xxxxxxxxx'
```

### Response

#### Sample

```json
{
    "subscriptions": [
        {
            "createdDate": "2024-05-01T14:43:19.136Z",
            "dataSetId": "sp-traffic",
            "destination": {
                "firehoseDestination": {
                    "deliveryStreamArn": "arn:aws:firehose:us-east-1:xxxxxxxx:deliverystream/NA-sp-traffic-sp-traffic-na",
                    "subscriberRoleArn": "arn:aws:iam::xxxxxxxx:role/NA-sp-traffic-FirehoseSubscriberRole",
                    "subscriptionRoleArn": "arn:aws:iam::xxxxxxxx:role/NA-sp-traffic-FirehoseSubscriptionRole"
                }
            },
            "notes": "",
            "status": "ACTIVE",
            "subscriptionId": "amzn1.fead.cs1.xxxxxxxxx",
            "updatedDate": "2024-05-01T14:43:23.476Z"
        },
        {
            "createdDate": "2024-05-01T14:46:26.643Z",
            "dataSetId": "sp-traffic",
            "destination": {
                "sqsDestination": {
                    "queueArn": "arn:aws:sqs:us-east-1:xxxxxxxx:StreamDestinationQueue"
                }
            },
            "destinationArn": "arn:aws:sqs:us-east-1:xxxxxx:StreamDestinationQueue",
            "notes": "string",
            "status": "PENDING_CONFIRMATION",
            "subscriptionId": "amzn1.fead.cs1.xxxxxxx",
            "updatedDate": "2024-05-01T14:46:30.02Z"
        }
    ]
}
```

If there are more pages of results, you can pass the `nextToken` value in the `startingToken` query parameter of your next call.

## Add a subscription

If you need to subscribe to a new dataset or advertiser, use the [POST `/streams/subscriptions`](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription) endpoint.

Make sure that you have [added the relevant IAM policy](guides/amazon-marketing-stream/onboarding#step-2-add-a-resource-based-iam-policy) for the dataset to your SQS queue before adding the subscription. In order to complete your subscription and start receiving data, you will need to [confirm your subscription in SQS](guides/amazon-marketing-stream/onboarding#step-4-confirm-your-subscription-in-sqs).

You can see what datasets are currently supported in the [Data guide](guides/amazon-marketing-stream/data-guide). 

>[WARNING] Make sure you are using a single Login With Amazon client ID to manage subscriptions. If you want to begin using a new Login With Amazon client ID, you should first subscribe to your datasets using the new client ID and a new destination queue. Once you have confirmed that you are receiving data in the new queues, you should archive all existing subscriptions using the old client ID. 

### Request

For `destinationArn` make sure to include an SQS ARN that has an IAM policy applied matching the `dataSetId` you are requesting.

#### Sample: SQS queue 

```bash
curl --location --request POST 'https://advertising-api.amazon.com/streams/subscriptions' \
--header 'Amazon-Advertising-API-ClientId: xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Content-Type: application/vnd.MarketingStreamSubscriptions.StreamSubscriptionResource.v1.0+json' \
--header 'Authorization: Bearer xxxxxxxxx' \
--data-raw '{
  "notes": "string",
  "clientRequestToken": "xxxxxxxxxxxxxxxxxxxxxxxxxxx",
  "dataSetId": "sp-traffic",
  "destination": {
    "sqsDestination": {
      "queueArn": "arn:aws:sqs:us-east-1:xxxxxxxx:StreamDestinationQueue"
    }
  }
}‘
```

### Response

A successful call returns a response indicating the `subscriptionID` that you can use to [check the subscription status](guides/amazon-marketing-stream/managing-subscriptions#check-subscription-status).

#### Sample

```json
{
   "subscriptionId": "xxxxxxxxxxxxxxxxxxxxxxxxxxx",
   "clientRequestToken": "123456xyz789101112",
 }
```

### Troubleshooting

When subscripting to endpoints, you may encounter the following errors. 

#### Cannot have more than one subscription per advertiser, dataSetId, & destination combination

A single Login with Amazon client application cannot support more than one subscription per advertiser (profile), dataSetId, and destination ARN combination. A single destination ARN cannot be used to receive data for the same advertiser or profile ID.

#### Cannot have more than two subscriptions per application, advertiser, & dataSetId combination

A single Login With Amazon client application cannot support more than two subscriptions for the same advertiser and dataset ID. 

#### Dataset subscription limits exceeded

By default, each Login With Amazon client application can support up to 25000 subscriptions per dataset. If you would like to increase your subscription limit, contact our [support team](support/overview).

#### Application’s pending confirmation limit of 1500 exceeded

Each Login With Amazon client application cannot have more than 1500 subscriptions in PENDING_CONFIRMATION state. Make sure to confirm all pending subscriptions. 

## Check subscription status

Once you have [created and confirmed](guides/amazon-marketing-stream/onboarding#step-3-subscribe-to-datasets) a subscription, you can see status information for the subscription using the [GET `/streams/subscriptions/{subscriptionId}`](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/GetStreamSubscription) endpoint.

>[TIP] Regularly check subscription status to ensure all subscriptions are active. A subscription can be suspended if the user who’s token was used in the subscription loses access to the advertising entity. 

### Possible statuses

|Status	|Description	|
|---	|---	|
|PROVISIONING	|Subscription has been created and is being provisioned on the Amazon side. Once finished provisioning, the subscription will move into PENDING_CONFIRMATION status.	|
|PENDING_CONFIRMATION	|Subscription was created and provisioned, and is awaiting [confirmation in the SQS queue](guides/amazon-marketing-stream/onboarding#step-4-confirm-your-subscription-in-sqs).	|
|FAILED_CONFIRMATION	|The subscription was not confirmed in SQS within 3 days. If this occurs, please create a new subscription and confirm the subscription within the 3-day expiration window.	|
|ACTIVE	|The subscription is active and delivering data.	|
|ARCHIVED	|The subscription has been archive. 	|
|SUSPENDED	|The subscription has been suspended. This happens if the account that created the subscription loses access to the advertiser in the advertising console.	|

>[WARNING] `SUSPENDED` subscriptions cannot be set to `ACTIVE`. Once a subscription has been suspended, you must repeat the [subscription process](guides/amazon-marketing-stream/onboarding#step-3-subscribe-to-amazon-marketing-stream-datasets) with an account that has access to the data. 

### Request

#### Sample

```bash
curl --location -g --request GET 'https://advertising-api.amazon.com/streams/subscriptions/xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Authorization: Bearer Atza| xxxxxxxx'
```

### Response

#### Sample

```json
{
    "subscription": {
        "createdDate": "2024-05-01T14:46:26.643Z",
        "dataSetId": "sp-traffic",
        "destination": {
            "sqsDestination": {
                "queueArn": "arn:aws:sqs:us-east-1:379073720614:StreamDestinationQueue"
            }
        },
        "destinationArn": "arn:aws:sqs:us-east-1:379073720614:StreamDestinationQueue",
        "notes": "string",
        "status": "PENDING_CONFIRMATION",
        "subscriptionId": "amzn1.fead.cs1.nQHVYtEaQMEqCWGBX31DHQ",
        "updatedDate": "2024-05-01T14:46:30.02Z"
    }
}
```

## Archive a subscription

Currently there is no way to delete Amazon Marketing Stream subscriptions. Instead you can archive a subscription using the [PUT `/streams/subscriptions/{subscriptionId}`](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/UpdateStreamSubscription) endpoint and setting the `status` to `ARCHIVED`. Setting the status as archived stops all data delivery for the advertiser account and dataset associated with the subscription.


### Request

#### Sample

```bash
curl --location -g --request PUT 'https://advertising-api.amazon.com/streams/subscriptions/xxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxx' \
--data-raw '{
 "status": "ARCHIVED"
}'

```

### Response

A successful response returns a 200 response. 


 




