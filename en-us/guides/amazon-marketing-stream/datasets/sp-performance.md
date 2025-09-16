---
title: Sponsored Products performance datasets
description: Details and schema for the Sponsored Products conversion (sp-conversion) and traffic (sp-traffic) datasets.
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Campaign management
    - Sponsored Products
---

# Sponsored Products performance datasets

The Sponsored Products performance datasets provide details about traffic and conversions for Sponsored Products campaigns. 

>[NOTE] To subscribe or manage subscriptions to these datasets, use the [/streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription) endpoints.

## Sponsored Products traffic dataset

The sp-traffic dataset contains click, impression, and cost data related to Sponsored Products campaigns. 

### Dataset ID

To subscribe to this dataset, use the ID `sp-traffic`.

### Schema

This dataset supports the following fields and metrics:

| Field name | Type | Description|
|------------|------|------------|
| idempotency_id | String | An identifier than can be used to de-duplicate records. |
| dataset_id | String | An identifier used to identify the dataset (in this case, `sp-traffic`). |
| marketplace_id | String | The [marketplace identifier](https://developer-docs.amazon.com/sp-api/docs/marketplace-ids) associated with the account. |
| currency | String | The currency used for all monetary values for entities under the profile. |
| advertiser\_id | String | ID associated with the advertiser. You can retrieve this ID using the [GET v2/profiles](reference/2/profiles#tag/Profiles/operation/listProfiles) endpoint. This value is **not** unique and may be the same across marketplaces. Note: For non-seller accounts, this advertiser_id is set to the entity ID. |
| campaign_id | String | Unique numerical ID for a campaign. |
| ad\_group\_id | String | Unique numerical ID for an ad group. |
| ad_id | String | Unique numerical ID for the ad. |
| keyword_id | String | ID of the keyword used for a bid. |
| keyword_text | String | Text of the keyword or phrase used for a bid. |
| match_type | String | Type of matching for the keyword or phrase used in a bid. |
| placement | String | The page location where an ad appeared.  |
| time\_window\_start | String | The start of the hour to which the performance data is attributed (ISO 8601 date time). Time zone for each advertiser is indicated on the advertiser [profile](reference/2/profiles#/Profiles/listProfiles).  |
| clicks | Long | Total ad clicks. |
| impressions | Long | Total ad impressions. |
| cost | Double | Total cost of all clicks. Expressed in local currency. |

### Resource-based IAM policy

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
          "aws:SourceArn": "arn:aws:sns:us-east-1:906013806264:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:668473351658:*"
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:074266271188:*"
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

### Data freshness

Initial click data is available within 12 hours of the ad click. As part of the traffic validation process, clicks can be invalidated during the following 72 hours post the initial report. Amazon Marketing Stream will automatically deliver any adjustments to your queue. Data will be delivered to your queue in a burst every hour. It is not necessarily at the top of the hour, but it is once per hour


## Sponsored Products conversions dataset

The sp-conversion dataset contains conversion data attributed to Sponsored Products campaigns. 

### Dataset ID

To subscribe to this dataset, use the ID `sp-conversion`.

### Schema

| Field name | Field type | Description	|
|-----------|-----------|------------|
| idempotency_id | String | An identifier than can be used to de-duplicate records.	|
| dataset_id |String | An identifier used to identify the dataset (in this case, `sp-conversion`).	|
| marketplace_id | String | The [marketplace identifier](https://developer-docs.amazon.com/sp-api/docs/marketplace-ids) associated with the account.	|
| currency | String	| The currency used for all monetary values for entities under the profile.	|
| advertiser\_id | String | ID associated with the advertiser. You can retrieve this ID using the [GET v2/profiles](reference/2/profiles#tag/Profiles/operation/listProfiles) endpoint. This value is **not** unique and may be the same across marketplaces. Note: For non-seller accounts, this advertiser_id is set to the entity ID.	|
| campaign_id | String	| Unique numerical ID for a campaign.	|
| ad\_group\_id | String	| Unique numerical ID for an ad group.	|
| ad_id	| String | Unique numerical ID for the ad.	|
| keyword_id | String | ID of the keyword used for a bid.	|
| placement	| String | The page location where an ad appeared. |
| time\_window\_start	| String |The start of the hour to which the performance data is attributed (ISO 8601 date time). Time zone for each advertiser is indicated on the advertiser [profile](reference/2/profiles#/Profiles/listProfiles). 	|
| attributed\_conversions\_1d	| Long	| Number of attributed conversion events occurring within 24 hours of ad click.	|
| attributed\_conversions\_7d	| Long	| Number of attributed conversion events occurring within 7 days of an ad click.	|
| attributed\_conversions\_14d | Long	| Number of attributed conversion events occurring within 14 days of an ad click.	|
| attributed\_conversions\_30d | Long	| Number of attributed conversion events occurring within 30 days of an ad click.	|
| attributed\_conversions\_1d\_same\_sku | Long	| Number of attributed conversion events occurring within 24 hours of ad click where the purchased SKU was the same as the SKU advertised.	|
| attributed\_conversions\_7d\_same\_sku | Long	| Number of attributed conversion events occurring within 7 days of ad click where the purchased SKU was the same as the SKU advertised.	|
| attributed\_conversions\_14d\_same\_sku | Long	| Number of attributed conversion events occurring within 14 days of ad click where the purchased SKU was the same as the SKU advertised.	|
| attributed\_conversions\_30d\_same\_sku | Long	| Number of attributed conversion events occurring within 30 days of ad click where the purchased SKU was the same as the SKU advertised.	|
| attributed\_sales\_1d | Double	| Total value of sales occurring within 24 hours of an ad click.	|
| attributed\_sales\_7d | Double	| Total value of sales occurring within 7 days of an ad click.	|
| attributed\_sales\_14d | Double	| Total value of sales occurring within 14 days of an ad click.	|
| attributed\_sales\_30d | Double	| Total value of sales occurring within 30 days of an ad click.	|
| attributed\_sales\_1d\_same\_sku | Double	| Total value of sales occurring within 24 hours of ad click where the purchased SKU was the same as the SKU advertised.	|
| attributed\_sales\_7d\_same\_sku | Double	| Total value of sales occurring within 7 days of ad click where the purchased SKU was the same as the SKU advertised.	|
| attributed\_sales\_14d\_same\_sku	| Double	| Total value of sales occurring within 14 days of ad click where the purchased SKU was the same as the SKU advertised.	|
| attributed\_sales\_30d\_same\_sku	| Double	| Total value of sales occurring within 30 days of ad click where the purchased SKU was the same as the SKU advertised.	|
| attributed\_units\_ordered\_1d | Long	| Total number of units ordered within 24 hours of an ad click.	|
| attributed\_units\_ordered\_7d | Long	| Total number of units ordered within 7 days of an ad click.	|
| attributed\_units\_ordered\_14d | Long	| Total number of units ordered within 14 days of an ad click.	|
| attributed\_units\_ordered\_30d | Long	| Total number of units ordered within 30 days of an ad click.	|
| attributed\_units\_ordered\_1d\_same\_sku	| Long	|Total number of units ordered within 24 hours of ad click where the purchased SKU was the same as the SKU advertised.	|
| attributed\_units\_ordered\_7d\_same\_sku	| Long	|Total number of units ordered within 7 days of ad click where the purchased SKU was the same as the SKU advertised.	|
| attributed\_units\_ordered\_14d\_same\_sku	| Long	|Total number of units ordered within 14 days of ad click where the purchased SKU was the same as the SKU advertised.	|
| attributed\_units\_ordered\_30d\_same\_sku	| Long	|Total number of units ordered within 30 days of ad click where the purchased SKU was the same as the SKU advertised.	|

### Resource-based IAM policy

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
          "aws:SourceArn": "arn:aws:sns:us-east-1:802324068763:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:562877083794:*"
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:622939981599:*"
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

### Data freshness

Conversions for Sponsored Products campaigns are reported based on the hour when the click they are attributed to occurred. Conversion data is reported daily, weekly, and monthly, and you should expect to receive revisions to conversion data up to sixty days after the initial click. Data will be delivered to your queue in a burst every hour. It is not necessarily at the top of the hour, but it is once per hour.

