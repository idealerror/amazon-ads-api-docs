---
title: Sponsored Display performance datasets
description: Details and schema for the Sponsored Display traffic and conversion datasets.
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Campaign management
    - Sponsored Display
---

# Sponsored Display performance datasets

These datasets contain click and conversion data related to Sponsored Display campaigns.

>[NOTE] To subscribe or manage subscriptions to these datasets, use the [/streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription) endpoints.

## Sponsored Display traffic dataset

The sd-traffic dataset contains click, impression, and cost data related to Sponsored Display campaigns. 

>[NOTE] Some Sponsored Display campaigns created before 8/1/2021 are not included in the sd-traffic dataset.

### Dataset ID

To subscribe to this dataset, use the ID `sd-traffic`.

### Schema

|Field name	|Type	|Description	|
|---	|---	|---	|
|idempotency_id	|String	|An identifier than can be used to de-duplicate records.	|
|dataset_id	|String	|An identifier used to identify the dataset (in this case, sd-traffic).	|
|marketplace_id	|String	|[The marketplace identifier associated with the account.](https://developer-docs.amazon.com/sp-api/docs/marketplace-ids)	|
|currency	|String	|The currency used for all monetary values for entities under the profile.	|
|advertiser\_id	|String	| ID associated with the advertiser. You can retrieve this ID using the [GET v2/profiles](reference/2/profiles#tag/Profiles/operation/listProfiles) endpoint. This value is **not** unique and may be the same across marketplaces. Note: For non-seller accounts, this advertiser_id is set to the entity ID.	|
|campaign_id	|String	|Unique numerical ID for a campaign.	|
|ad\_group\_id	|String	|Unique numerical ID for an ad group.	|
|ad_id	|String	|Unique numerical ID for the ad.	|
|target_id	|String	|The identifier of the targeting expression.	|
|targeting_text	|String	|The text used in the targeting expression.	|
|time\_window\_start	|String	|The start of the hour to which the performance data is attributed (ISO 8601 date time). Time zone for each advertiser is indicated on the advertiser [profile](reference/2/profiles#/Profiles/listProfiles).	|
|clicks	|Long	|Total ad clicks.	|
|impressions	|Long	|Total ad impressions.	|
|view_impressions	|Long	|Total ad viewable impressions (for vCPM campaigns only).	|
|cost	|Double	|Total cost of all clicks (CPC) or click or view (vCPM). Expressed in local currency.	|

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
          "aws:SourceArn": "arn:aws:sns:us-east-1:370941301809:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:947153514089:*"
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:310605068565:*"
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

Initial click data is available within 12 hours of the ad click. As part of the traffic validation process, clicks can be invalidated during the following 72 hours post the initial report. Amazon Marketing Stream will automatically deliver any adjustments to your queue.

## Sponsored Display conversions dataset

The sd-conversion dataset contains conversion data attributed to Sponsored Display campaigns. 

### Dataset ID

To subscribe to this dataset, use the `sd-conversion` ID.

### Schema

|Field name	|Type	|Description	|
|---	|---	|---	|
|idempotency_id	|String	|An identifier than can be used to de-duplicate records.	|
|dataset_id	|String	|An identifier used to identify the dataset (in this case, sd-conversion).	|
|marketplace_id	|String	|[The marketplace identifier associated with the account.](https://developer-docs.amazon.com/sp-api/docs/marketplace-ids)	|
|currency	|String	|The currency used for all monetary values for entities under the profile.	|
|advertiser\_id	|String	| ID associated with the advertiser. You can retrieve this ID using the [GET v2/profiles](reference/2/profiles#tag/Profiles/operation/listProfiles) endpoint. This value is **not** unique and may be the same across marketplaces. Note: For non-seller accounts, this advertiser_id is set to the entity ID.	|
|campaign_id	|String	|Unique numerical ID for a campaign.	|
|ad\_group\_id	|String	|Unique numerical ID for an ad group.	|
|ad_id	|String	|Unique numerical ID for the ad.	|
|target_id	|String	|The identifier of the targeting expression.	|
|time\_window\_start	|String	|The start of the hour to which the performance data is attributed (ISO 8601 date time). Time zone for each advertiser is indicated on the advertiser [profile](reference/2/profiles#/Profiles/listProfiles).	|
|attributed\_conversions\_1d	|Long	|Number of attributed conversion events occurring within 24 hours of ad click.	|
|attributed\_conversions\_7d	|Long	|Number of attributed conversion events occurring within 7 days of an ad click.	|
|attributed\_conversions\_14d	|Long	|Number of attributed conversion events occurring within 14 days of an ad click.	|
|view\_attributed\_conversions\_14d	|Long	|Number of attributed conversion events occurring within 14 days of an ad click or view (for vCPM campaigns only).	|
|attributed\_conversions\_30d	|Long	|Number of attributed conversion events occurring within 30 days of an ad click.	|
|attributed\_conversions\_1d\_same\_sku	|Long	|Number of attributed conversion events occurring within 24 hours of ad click where the purchased SKU was the same as the SKU advertised.	|
|attributed\_conversions\_7d\_same\_sku	|Long	|Number of attributed conversion events occurring within 7 days of ad click where the purchased SKU was the same as the SKU advertised.	|
|attributed\_conversions\_14d\_same\_sku	|Long	|Number of attributed conversion events occurring within 14 days of ad click where the purchased SKU was the same as the SKU advertised.	|
|attributed\_conversions\_30d\_same\_sku	|Long	|Number of attributed conversion events occurring within 30 days of ad click where the purchased SKU was the same as the SKU advertised.	|
|attributed\_sales\_1d	|Double	|Total value of sales occurring within 24 hours of an ad click.	|
|attributed\_sales\_7d	|Double	|Total value of sales occurring within 7 days of an ad click.	|
|attributed\_sales\_14d	|Double	|Total value of sales occurring within 14 days of an ad click.	|
|view\_attributed\_sales\_14d	|Double	|Total value of sales occurring within 14 days of an ad click or view (for vCPM campaigns only).	|
|attributed\_sales\_30d	|Double	|Total value of sales occurring within 30 days of an ad click.	|
|attributed\_sales\_1d\_same\_sku	|Double	|Total value of sales occurring within 24 hours of ad click where the purchased SKU was the same as the SKU advertised.	|
|attributed\_sales\_7d\_same\_sku	|Double	|Total value of sales occurring within 7 days of ad click where the purchased SKU was the same as the SKU advertised.	|
|attributed\_sales\_14d\_same\_sku	|Double	|Total value of sales occurring within 14 days of ad click where the purchased SKU was the same as the SKU advertised.	|
|attributed\_sales\_30d\_same\_sku	|Double	|Total value of sales occurring within 30 days of ad click where the purchased SKU was the same as the SKU advertised.	|
|attributed\_units\_ordered\_1d	|Long	|Total number of units ordered within 24 hours of an ad click.	|
|attributed\_units\_ordered\_7d	|Long	|Total number of units ordered within 7 days of an ad click.	|
|attributed\_units\_ordered\_14d	|Long	|Total number of units ordered within 14 days of an ad click.	|
|view\_attributed\_units\_ordered\_14d	|Long	|Total number of units ordered within 14 days of an ad click or view (for vCPM campaigns only).	|
|attributed\_units\_ordered\_30d	|Long	|Total number of units ordered within 30 days of an ad click.	|
|attributed\_orders\_new\_to\_brand\_14d	|Long	|Total number of orders within 14 days of an ad click.	|
|attributed\_sales\_new\_to\_brand\_14d	|Double	|Total value of new to brand sales within 14 days of an ad click.	|
|attributed\_units\_ordered\_new\_to\_brand\_14d	|Long	|Total number of new to brand units ordered within 14 days of an ad click.	|
|view\_attributed\_orders\_new\_to\_brand\_14d	|Long	|Total number of orders within 14 days of an ad click or view (for vCPM campaigns only).	|
|view\_attributed\_sales\_new\_to\_brand\_14d	|Double	|Total value of new to brand sales within 14 days of an ad click or view (for vCPM campaigns only).	|
|view\_attributed\_units\_ordered\_new\_to\_brand\_14d	|Long	|Total number of new to brand units ordered within 14 days of an ad click or view (for vCPM campaigns only).	|

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
          "aws:SourceArn": "arn:aws:sns:us-east-1:877712924581:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:664093967423:*"
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:818973306977:*"
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

Conversions for Sponsored Display campaigns are reported based on the hour when the click they are attributed to occurred. Conversion data is reported daily, weekly, and monthly, and you should expect to receive revisions to conversion data up to sixty days after the initial click. 