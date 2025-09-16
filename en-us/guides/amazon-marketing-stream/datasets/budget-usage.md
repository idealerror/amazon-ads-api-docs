---
title: Sponsored ads budget usage dataset
description: Details and schema for the sponsored ads budget usage (budget-usage) dataset.
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Campaign management
    - Sponsored Products
---

# Sponsored ads budget usage dataset

The budget-usage dataset provides the budget status (spend compared to budget) for sponsored ads (Sponsored Products, Sponsored Display, or Sponsored Brands) campaigns and portfolios. Advertisers receive notifications at every 5% interval of budget consumption. 

>[NOTE] To subscribe or manage subscriptions to this dataset, use the [/streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription) endpoints.

## Dataset ID

To subscribe to this dataset, use the ID `budget-usage`.

## Schema

This dataset supports the following fields and metrics:

| Field name	| Field type | Field description |
|---	|---	|---	|
|advertiser\_id	| String	| ID associated with the advertiser. You can retrieve this ID using the [GET v2/profiles](reference/2/profiles#tag/Profiles/operation/listProfiles) endpoint. This value is **not** unique and may be the same across marketplaces. Note: For non-seller accounts, this advertiser_id is set to the entity ID.	|
|marketplace_id	|String	|The identifier of the marketplace to which the account is associated.	|
|dataset_id	|String	|An identifier used to uniquely identify a specific dataset.	|
|budget\_scope\_id	|String	|The unique identifier of campaign or portfolio for which budget usage percentage is provided.	|
|budget\_scope\_type	|String	|The type of budget scope. Currently supported values: `CAMPAIGN` or `PORTFOLIO`.	|
|advertising\_product\_type	|String	|Advertising product type `sp` (Sponsored Products), `sb` (Sponsored Brands), or `sd` (Sponsored Display).	|
|budget	|Double	|Value of budget for budget\_scope\_id. It will be rule-based budget if any budget rule is winning. Otherwise it will be the advertiser's set budget.	|
|budget\_usage\_percentage	|Double	|Budget usage percentage (Spend divided by budget).	|
|usage\_updated\_timestamp	|String	|Last evaluation time for budget usage.	|

## Resource-based IAM policy

To subscribe to this dataset, ensure the correct IAM policy is added to the SQS queue based on the marketplace.

**NA**

```json
{
  "Version": "2008-10-17",
  "Id": "{REPLACE_WITH_ANY_VALUE}",
  "Statement": [
    {
      "Sid": "{REPLACE_WITH_ANY_VALUE}",
      "Effect": "Allow",
      "Principal": {
        "Service": "sns.amazonaws.com"
      },
      "Action": "SQS:SendMessage",
      "Resource": "{REPLACE WITH ARN Of SQS DESTINATION QUEUE}",
      "Condition": {
        "ArnEquals": {
          "aws:SourceArn": "arn:aws:sns:us-east-1:055588217351:*"
        }
      }
    },
    {
      "Sid": "{REPLACE_WITH_ANY_VALUE}",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::926844853897:role/ReviewerRole"
      },
      "Action": "SQS:GetQueueAttributes",
      "Resource": "*"
    }
  ]
}
```

**EU**

```json
{
  "Version": "2008-10-17",
  "Id": "{REPLACE_WITH_ANY_VALUE}",
  "Statement": [
    {
      "Sid": "{REPLACE_WITH_ANY_VALUE}",
      "Effect": "Allow",
      "Principal": {
        "Service": "sns.amazonaws.com"
      },
      "Action": "SQS:SendMessage",
      "Resource": "{REPLACE WITH ARN Of SQS DESTINATION QUEUE}",
      "Condition": {
        "ArnEquals": {
          "aws:SourceArn": "arn:aws:sns:eu-west-1:675750596317:*"
        }
      }
    },
    {
      "Sid": "{REPLACE_WITH_ANY_VALUE}",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::926844853897:role/ReviewerRole"
      },
      "Action": "SQS:GetQueueAttributes",
      "Resource": "*"
    }
  ]
}
```

**FE**

```json
{
  "Version": "2008-10-17",
  "Id": "{REPLACE_WITH_ANY_VALUE}",
  "Statement": [
    {
      "Sid": "{REPLACE_WITH_ANY_VALUE}",
      "Effect": "Allow",
      "Principal": {
        "Service": "sns.amazonaws.com"
      },
      "Action": "SQS:SendMessage",
      "Resource": "{REPLACE WITH ARN Of SQS DESTINATION QUEUE}",
      "Condition": {
        "ArnEquals": {
          "aws:SourceArn": "arn:aws:sns:us-west-2:100899330244:*"
        }
      }
    },
    {
      "Sid": "{REPLACE_WITH_ANY_VALUE}",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::926844853897:role/ReviewerRole"
      },
      "Action": "SQS:GetQueueAttributes",
      "Resource": "*"
    }
  ]
}
```

## Data freshness

Amazon Marketing Stream pushes budget usage updates in near real-time. You should receive a notification soon after your budget usage increases 5%. Due to Amazon’s traffic validation process, it is possible that you may receive the same notification twice. For example, if a click is invalidated, that may reduce your budget usage under the 5% threshold. 
