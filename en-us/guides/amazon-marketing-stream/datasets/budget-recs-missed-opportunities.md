---
title: Sponsored Products budget recommendations and missed opportunities
description: Details and schema for the Sponsored Products budget recommendations and missed opportunities dataset.
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Campaign management
    - Sponsored Products
---

# Sponsored Products budget recommendations and missed opportunities

This dataset provides an unmodified view of the Sponsored Products budget recommendations and missed opportunities insights from campaigns that have been recently out of budget. Missed opportunities of an out-of-budget campaign are the additional clicks, impressions, and sales between the start date and end date that the campaign could have received had it stayed in budget for the hours it was out of budget.

>[NOTE] To subscribe or manage subscriptions to this dataset, use the [/streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription) endpoints.

## Dataset ID

To subscribe to this dataset, use the ID `sp-budget-recommendations`.

## Schema

This dataset supports the following fields and metrics:

|Field	|Type	|Description	|
|-----|-----|------|
|dataset\_id	|String	|An identifier used to identify the dataset (in this case, `sp-budget-recommendations`).	|
|marketplace\_id	|String	|The [marketplace identifier](https://developer-docs.amazon.com/sp-api/docs/marketplace-ids) associated with the account.	|
|advertiser\_id	|String	|The unique identifier of the advertiser.	|
|recommendation\_time	|String (ISO Format)	|The date and time of the recommendation.	|
|campaign\_id	|String	|The unique numerical ID for a campaign.	|
|suggested\_budget	|Double	|Estimated daily budget needed to keep the campaign in budget for the full 24-hour period in a day. Consider adopting this daily budget to minimize your campaign's chances of running out of budget. Expressed in local currency.	|
|reason\_for\_suggestion | String | The reason for the budget recommendation. |
|estimated\_missed\_sales\_lower	|Double	|This is the estimated lower bound of additional sales the campaign might have generated between the start and end date had it been in budget 100% of the time. These are estimates based on historical traffic and the campaign's past performance (e.g., impressions & clicks generated) but not guaranteed.	|
|estimated\_missed\_sales\_upper	|Double	|This is the estimated upper bound of additional sales the campaign might have generated between the start and end date had it been in budget 100% of the time. These are estimates based on historical traffic and the campaign's past performance (e.g., impressions & clicks generated) but not guaranteed.	|
|estimated\_missed\_clicks\_lower	|Double	|This is the estimated lower bound of additional clicks the campaign might have generated between the start and end date had it been in budget 100% of the time. These are estimates based on historical traffic and the campaign's past performance (e.g., impressions & clicks generated) but not guaranteed.	|
|estimated\_missed\_clicks\_upper	|Double	|This is the estimated upper bound of additional clicks the campaign might have generated between the start and end date had it been in budget 100% of the time. These are estimates based on historical traffic and the campaign's past performance (e.g., impressions & clicks generated) but not guaranteed.	|
|estimated\_missed\_impressions\_lower	|Double	|This is the estimated lower bound of additional impressions the campaign might have generated between the start and end date had it been in budget 100% of the time. These are estimates based on historical traffic and the campaign's past performance (e.g., impressions & clicks generated) but not guaranteed.	|
|estimated\_missed\_impressions\_upper	|Double	|This is the estimated upper bound of additional impressions the campaign might have generated between the start and end date had it been in budget 100% of the time. These are estimates based on historical traffic and the campaign's past performance (e.g., impressions & clicks generated) but not guaranteed.	|
|start\_date	|String	|Start day of missed opportunity estimation range. (end\_date - start\_date = 7)	|
|end\_date	|String	|End day of missed opportunity estimation range. (end\_date - start\_date = 7)	|
|percent\_time\_in\_budget	|Double	|Actual average percentage of time the campaign was in budget between the start and end date. Note: value -1 means we don’t have enough information to compute the campaign’s percent time in budget. 1.0 means it's in budget all the time.	|


## Resource-based IAM policy

To subscribe to this dataset, ensure the correct IAM policy is added to the SQS queue based on the marketplace.

**NA**

```json
{
  "Version": "2008-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "sns.amazonaws.com"
      },
      "Action": "SQS:SendMessage",
      "Resource": "*",
      "Condition": {
        "ArnEquals": {
          "aws:SourceArn": "arn:aws:sns:us-east-1:678715897637:*"
        }
      }
    },
    {
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
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "sns.amazonaws.com"
      },
      "Action": "SQS:SendMessage",
      "Resource": "*",
      "Condition": {
        "ArnEquals": {
          "aws:SourceArn": "arn:aws:sns:eu-west-1:158915609581:*"
        }
      }
    },
    {
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
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "sns.amazonaws.com"
      },
      "Action": "SQS:SendMessage",
      "Resource": "*",
      "Condition": {
        "ArnEquals": {
          "aws:SourceArn": "arn:aws:sns:us-west-2:007292432803:*"
        }
      }
    },
    {
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

Amazon Marketing Stream pushes budget recommendations every hour. 