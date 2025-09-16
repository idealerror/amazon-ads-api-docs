---
title: Subscribing to Amazon Marketing Stream datasets
description: Instructions for subscribing to Amazon Marketing Stream datasets, using Firehose.
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

# Subscribing to datasets 

Once you have [created your AWS Firehose stream and IAM roles](guides/amazon-marketing-stream/onboarding/firehose/get-started), you can subscribe to datasets using the Amazon Ads API. If you are subscribing to data for multiple profiles, you must make a separate call for each. Learn more about [profiles](guides/account-management/authorization/profiles).

Note that DSP and sponsored ads datasets use different APIs for dataset subscription. Refer to the [Datasets overview](guides/amazon-marketing-stream/data-guide) to see the endpoint for each dataset. 

## Endpoints

| Ad product | Endpoint |
|-----|-----|
| Sponsored ads datasets |[POST /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/CreateStreamSubscription)  |
| Amazon DSP datasets | [POST dsp/streams/subscriptions endpoint](amazon-marketing-stream/openapi#tag/DSP-Stream-Subscription/operation/CreateDspStreamSubscription) | 

## Headers

### Sponsored ads

For sponsored ads datasets, use the following headers in your request to POST /streams/subscriptions:

|Parameter	|Description	|
|---	|---	|
|Amazon-Advertising-API-ClientId	|The [client ID](https://advertising.amazon.com/API/docs/en-us/guides/onboarding/create-lwa-app) associated with your Login with Amazon application.	|
|Amazon-Advertising-API-Scope	|Your [profile ID](https://advertising.amazon.com/API/docs/en-us/guides/account-management/authorization/profiles).	|
|Authorization	|Your [access token](https://advertising.amazon.com/API/docs/en-us/guides/account-management/authorization/access-tokens).	|
|Content-Type	|`application/vnd.MarketingStreamSubscriptions.StreamSubscriptionResource.v1.0+json`	|

### Amazon DSP

For Amazon DSP datasets, use the following headers in your request to POST dsp/streams/subscriptions:


| Parameter | Description |
|-----------|-------------|
| Amazon-Advertising-API-ClientId | The [client ID](guides/onboarding/create-lwa-app) associated with your Login with Amazon application. |
| Amazon-Advertising-API-Account-ID | The Amazon DSP advertiser ID that you want to receive data for via Stream. You can view the advertiser IDs associated with you profile using [GET /dsp/advertisers](dsp-advertiser/#tag/Advertiser/paths/~1dsp~1advertisers/get). |
| Authorization | Your [access token](guides/account-management/authorization/access-tokens). |
| Content-Type | `application/vnd.MarketingStreamSubscriptions.DspStreamSubscriptionResource.v1.0+json` |

## Parameters

Both Amazon DSP and sponsored ads APIs take the same parameters:

|Parameter	|x	|	x|Required	|Description	|
|---	|---	|---	|---	|---	|
|`dataSetId`	|	|	|Yes	|To view available datasets, see the [data guide](https://advertising.amazon.com/API/docs/en-us/guides/amazon-marketing-stream/data-guide).	|
|`clientRequestToken`	|	|	|Yes	|Unique value supplied by the caller used to track identical API requests. Should request be re-tried, the caller should supply the same value. We recommend using GUID.	|
|`notes`	|	|	|No	|Additional details which can be used to identity the destination.	|
|`destination`	|`firehoseDestination`	|`deliveryStreamArn`	|Yes	|The ARN of the Amazon Data Firehose where you want to receive data. This arn value is found in the main account where you created the initial Firehose stream. In the [AWS console](console.aws.amazon.com), go to **Amazon Data Firehose > Firehose streams > [YOUR_STREAM]** to locate it.	|
|`destination`	|`firehoseDestination`	|`subscriptionRoleArn`	|Yes	|The ARN of the **FIREHOSE\_SUBSCRIPTION\_ROLE**. In the [AWS console](console.aws.amazon.com), go to **IAM > Roles** and locate the subscription role from the list. Click into the role to find the arn. ****	|
|`destination`	|`firehoseDestination`	|`subscriberRoleArn`	|Yes	|The ARN of **FIREHOSE\_SUBSCRIBER\_ROLE**. In the [AWS console](console.aws.amazon.com), go to **IAM > Roles** and locate the subscriber role from the list. Click into the role to find the arn. ****	|

### Retrieving ARNs

To subscribe to datasets with a destination of Amazon Data Firehose, you will need to retrieve the ARNs associated with Firehose and IAM roles.

#### deliveryStreamArn

1. In the AWS console search, enter “Firehose” and select **Amazon Data Firehose**.
2. Select the Firehose stream created by CloudFormation.
3. Copy the ARN. This is your `deliveryStreamArn`.

#### subscriberRoleArn and subscriptionRoleArn

1. In the AWS console search, enter “IAM” and select **IAM**.
2. From the IAM dashboard, select **Roles**. You should see three newly created Roles related to Firehose. 
3. Select the role with the `-FirehoseSubscriberRole` suffix and copy the ARN. This is your `subscriberRoleArn`.
4. Return to the Roles dashboard and select the Role with the `-FirehoseSubscriptionRole
    ` suffix. This is your `subscriptionRoleArn`.

## Example requests

### Sponsored ads sp-traffic dataset

#### Request

Make sure to replace `Amazon-Advertising-API-ClientId`, `Amazon-Advertising-API-Scope`, `Authorization`, and `firehoseDestination` with the values specific to your account. The `clientRequestToken` can be any idempotency token that could be used to identify the request if needed.


```bash
 curl --location --request POST 'https://advertising-api.amazon.com/streams/subscriptions' \
  --header 'Amazon-Advertising-API-ClientId: xxxxxx' \ 
  --header 'Amazon-Advertising-API-Scope: xxxxxxx' \
  --header 'Content-Type: application/vnd.MarketingStreamSubscriptions.StreamSubscriptionResource.v1.0+json' \
  --header 'Authorization: Bearer xxxxxxxxx' \
  --data-raw '{ 
     "clientRequestToken": "123456789xyz1234567",
     "dataSetId": "sp-traffic", 
     "notes": "Advertiser 1 sp-traffic subscription", 
     "destination": "{
        "firehoseDestination": {
            "deliveryStreamArn": "{YOUR_FIREHOSE_DELIVERY_STREAM_ARN}",
            "subscriptionRoleArn": "{FIREHOSE_SUBSCRIPTION_ROLE_ARN}",
            "subscriberRoleArn": "{FIREHOSE_SUBSCRIBER_ROLE_ARN}"
            }
        }
     }'
```

#### Response

A successful request results in a response that includes a `subscriptionId`.

```json
{ 
    "subscriptionId": "xxxxxxxxxxxxxx", 
    "clientRequestToken": "123456789xyz1234567",
 }
```

### Amazon DSP adsp-campaigns dataset

#### Request

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
        "destination": "{
            "firehoseDestination": {
                "deliveryStreamArn": "{YOUR_FIREHOSE_DELIVERY_STREAM_ARN}",
                "subscriptionRoleArn": "{FIREHOSE_SUBSCRIPTION_ROLE_ARN}",
                "subscriberRoleArn": "{FIREHOSE_SUBSCRIBER_ROLE_ARN}"
                }
            }
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

## Checking subscription status

You can check the status of your subscription using `GET /streams/subscriptions/{subscriptionId}`. For a code sample and a descriptions for each possible status, see [Managing subscriptions](guides/amazon-marketing-stream/managing-subscriptions#check-subscription-status).
 
