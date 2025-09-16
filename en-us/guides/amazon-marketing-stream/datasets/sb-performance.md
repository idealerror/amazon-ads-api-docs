---
title: Sponsored Brands performance datasets
description: Details and schema for the Sponsored Brands performance datasets.
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Campaign management
    - Sponsored Brands
---

# Sponsored Brands performance datasets

These datasets contain click and conversion data related to Sponsored Brands campaigns.

>[NOTE] To subscribe or manage subscriptions to these datasets, use the [/streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription) endpoints.

## Sponsored Brands traffic dataset (beta)

The sb-traffic dataset contains click, impression, and cost data related to Sponsored Brands campaigns. 

>[NOTE] Amazon Marketing Stream only includes traffic deriving from Sponsored Brands version 4 (multi-ad group campaigns). Data related to any campaigns created using [POST /sb/campaigns](sponsored-brands/3-0/openapi#tag/Campaigns/operation/createCampaigns) before May 22, 2023 (in the US) or before August 10, 2023 (rest of the world) will not be sent. For more details,view the Sponsored Brands version 4 [migration guide](reference/migration-guides/sb-v3-v4). 

### Dataset ID

To subscribe to this dataset, use the ID `sb-traffic`.

### Schema

|Field name	|Type	|Description	|
|--------|----|--------|
|idempotency\_id	|String	|An identifier than can be used to de-duplicate records.	|
|dataset\_id	|String	|An identifier used to identify the dataset (in this case, sb-traffic).	|
|marketplace\_id	|String	|The [marketplace identifier](https://developer-docs.amazon.com/sp-api/docs/marketplace-ids) associated with the account.	|
|time\_window\_start	|String	|The start of the hour to which the performance data is attributed (ISO 8601 date time). Time zone for each advertiser is indicated on the advertiser [profile](reference/2/profiles#/Profiles/listProfiles).	|
|campaign\_id	|String	|Unique numerical ID for a campaign.	|
|ad\_id	|String	|Unique numerical ID for the ad.	|
|ad\_group\_id	|String	|Unique numerical ID for an ad group.	|
|advertiser\_id	|String	|ID associated with the advertiser. You can retrieve this ID using the [GET v2/profiles](reference/2/profiles#/Profiles/listProfiles) endpoint. This value is **not** unique and may be the same across marketplaces. Note: For non-seller accounts, this advertiser\_id is set to the entity ID.	|
|keyword\_id	|String	|ID of the keyword used for a bid.	|
|keyword\_text	|String	|Text of the keyword or phrase used for a bid.	|
|keyword\_type	|String	|The type of targeting used in the expression.|
|placement\_type	|String	| The page location where an ad appeared. Possible values: Detail Page on-Amazon, Other on-Amazon, Top of Search on-Amazon.	|
|currency	|String	|The currency used for all monetary values for entities under the profile.	|
|clicks	|Long	|Total ad clicks.	|
|cost	|Double	|Total cost of all clicks. Expressed in local currency.	|
|impressions	|Long	|Total ad impressions.	|
|viewable\_impressions	|Long	|Number of impressions that met the Media Ratings Council (MRC) viewability standard (50 percent of the ad viewable with two seconds playback completed)	|


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
          "aws:SourceArn": "arn:aws:sns:us-east-1:709476672186:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:623198756881:*"
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:485899199471:*"
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

## Sponsored Brands conversion dataset (beta)

The sb-conversion dataset contains conversion and new-to-brand data related to Sponsored Brands campaigns.

>[NOTE] Amazon Marketing Stream only includes traffic deriving from Sponsored Brands version 4 (multi-ad group campaigns). Data related to any campaigns created using [POST /sb/campaigns](sponsored-brands/3-0/openapi#tag/Campaigns/operation/createCampaigns) before May 22, 2023 (in the US) or before August 10, 2023 (rest of the world) will not be sent. For more details,view the Sponsored Brands version 4 [migration guide](reference/migration-guides/sb-v3-v4). 

### Dataset ID

To subscribe to this dataset, use the ID `sb-conversion`.

### Schema

|Field name	|Type	|Description	|
|-------|-----|-----------|
|idempotency\_id	|String	|An identifier than can be used to de-duplicate records.	|
|dataset\_id	|String	|An identifier used to identify the dataset (in this case, sb-conversion).	|
|marketplace\_id	|String	|The [marketplace identifier](https://developer-docs.amazon.com/sp-api/docs/marketplace-ids) associated with the account.	|
|time\_window\_start	|String	|The start of the hour to which the performance data is attributed (ISO 8601 date time). Time zone for each advertiser is indicated on the advertiser [profile](reference/2/profiles#/Profiles/listProfiles).	|
|campaign\_id	|String	|Unique numerical ID for a campaign.	|
|ad\_id	|String	|Unique numerical ID for the ad.	|
|ad\_group\_id	|String	|Unique numerical ID for an ad group.	|
|advertiser\_id	|String	|ID associated with the advertiser. You can retrieve this ID using the [GET v2/profiles](reference/2/profiles#/Profiles/listProfiles) endpoint. This value is **not** unique and may be the same across marketplaces. Note: For non-seller accounts, this advertiser\_id is set to the entity ID.	|
|keyword\_id	|String	|ID of the keyword used for a bid.	|
|keyword\_text	|String	|Text of the keyword or phrase used for a bid.	|
|keyword\_type	|String	|The type of targeting used in the expression.	|
|placement\_type	|String	| The page location where an ad appeared. Possible values: Detail Page on-Amazon, Other on-Amazon, Top of Search on-Amazon.	|
|currency	|String	|The currency used for all monetary values for entities under the profile.	|
|view\_attributed\_conversions_14d	|Long	|Number of attributed conversion events occurring within 14 days of an ad view or click.	|
|attributed\_conversions\_14d\_same\_sku	|Long	|Number of attributed conversion events occurring within 14 days of ad click where the purchased SKU was the same as the SKU advertised.	|
|view\_attributed\_orders\_new\_to\_brand\_14d	|Long	|Total number of orders within 14 days of an ad view or click.	|
|view\_attributed\_sales\_14d	|Double	|Total value of sales occurring within 14 days of an ad view or click.	|
|view\_attributed\_units\_ordered\_14d	|Long	|Total number of units ordered within 14 days of an ad view or click.	|
|attributed\_sales\_14d\_same\_sku	|Double	|Total value of sales occurring within 14 days of ad click where the purchased SKU was the same as the SKU advertised.	|
|view\_attributed\_sales\_new\_to\_brand\_14d	|Double	|Total value of new to brand sales within 14 days of an ad view or click.	|
|view\_attributed\_units\_ordered\_new\_to\_brand\_14d	|Long	|Total number of new to brand units ordered within 14 days of an ad view or click.	|
|attributed\_conversions\_14d	|Long	|Number of attributed conversion events occurring within 14 days of an ad click.	|
|attributed\_orders\_new\_to\_brand\_14d	|Long	|Total number of new to brand units ordered within 14 days of a click.	|
|attributed\_sales\_14d	|Double	|Total value of sales occurring within 14 days of an ad click.	|
|attributed\_sales\_new\_to\_brand\_14d	|Double	|Total value of new to brand sales within 14 days of an ad click.	|
|attributed\_units\_ordered\_new\_to\_brand\_14d	|Long	|Total number of new to brand units ordered within 14 days of an ad click.	|
|attributed\_units\_ordered\_14d	|Long	|Total number of units ordered within 14 days of an ad click.	|

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
          "aws:SourceArn": "arn:aws:sns:us-east-1:154357381721:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:195770945541:*"
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:112347756703:*"
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

Conversions for Sponsored Brands campaigns are reported based on the hour when the click they are attributed to occurred. Conversion data is reported daily, weekly, and monthly, and you should expect to receive revisions to conversion data up to sixty days after the initial click. 

## Sponsored Brands clickstream dataset (beta)

The sb-clickstream dataset contains detail page views and branded searches data related to Sponsored Brands campaigns.

>[NOTE] Amazon Marketing Stream only includes traffic deriving from Sponsored Brands version 4 (multi-ad group campaigns). Data related to any campaigns created using [POST /sb/campaigns](sponsored-brands/3-0/openapi#tag/Campaigns/operation/createCampaigns) before May 22, 2023 (in the US) or before August 10, 2023 (rest of the world) will not be sent. For more details,view the Sponsored Brands version 4 [migration guide](reference/migration-guides/sb-v3-v4). 

### Dataset ID

To subscribe to this dataset, use the `sb-clickstream` ID.

### Schema

|Field name	|Type	|Description	|
|------|-----|-------|
|idempotency\_id	|String	|An identifier than can be used to de-duplicate records.	|
|dataset\_id	|String	|An identifier used to identify the dataset (in this case, sb-clickstream).	|
|marketplace\_id	|String	|The [marketplace identifier](https://developer-docs.amazon.com/sp-api/docs/marketplace-ids) associated with the account.	|
|time\_window\_start	|String	|The start of the hour to which the performance data is attributed (ISO 8601 date time). Time zone for each advertiser is indicated on the advertiser [profile](reference/2/profiles#/Profiles/listProfiles).	|
|campaign\_id	|String	|Unique numerical ID for a campaign.	|
|ad\_id	|String	|Unique numerical ID for the ad.	|
|ad\_group\_id	|String	|Unique numerical ID for an ad group.	|
|advertiser\_id	|String	|ID associated with the advertiser. You can retrieve this ID using the [GET v2/profiles](reference/2/profiles#/Profiles/listProfiles) endpoint. This value is **not** unique and may be the same across marketplaces. Note: For non-seller accounts, this advertiser_id is set to the entity ID.	|
|keyword\_id	|String	|ID of the keyword used for a bid.	|
|keyword\_text	|String	|Text of the keyword or phrase used for a bid.	|
|keyword\_type	|String	|The type of targeting used in the expression.	|
|placement\_type	|String	| The page location where an ad appeared. Possible values: Detail Page on-Amazon, Other on-Amazon, Top of Search on-Amazon.	|
|view\_attributed\_branded\_searches\_14d	|Long	|Total number of branded searches within 14 days of an ad view or click.	|
|attributed\_branded\_searches\_14d	|Long	|Total number of branded searches within 14 days of an ad click.	|
|view\_attributed\_detail\_page\_views\_14d	|Long	|Total number of detail page views within 14 days of an ad view or click.	|
|attributed\_detail\_page\_views\_14d	|Long	|Total number of detail page views within 14 days of an ad click.	|

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
          "aws:SourceArn": "arn:aws:sns:us-east-1:091028706140:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:219513501272:*"
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:632322331982:*"
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

 Note that it could take up to 12 hours for performance data to populate after an event (detail page view, branded search) occurs. Performance data is reported hourly, and you should expect to receive revisions to performance data up to 60 days after the initial event. 

## Sponsored Brands rich media dataset (beta)

The sb-rich-media dataset contains video data related to Sponsored Brands campaigns.

>[NOTE] Amazon Marketing Stream only includes traffic deriving from Sponsored Brands version 4 (multi-ad group campaigns). Data related to any campaigns created using [POST /sb/campaigns](sponsored-brands/3-0/openapi#tag/Campaigns/operation/createCampaigns) before May 22, 2023 (in the US) or before August 10, 2023 (rest of the world) will not be sent. For more details,view the Sponsored Brands version 4 [migration guide](reference/migration-guides/sb-v3-v4).  

### Dataset ID

To subscribe to this dataset, use the `sb-rich-media` ID.

### Schema

|Field name	|Type	|Description	|
|------|-----|----------|
|idempotency\_id	|String	|An identifier than can be used to de-duplicate records.	|
|dataset\_id	|String	|An identifier used to identify the dataset (in this case, sb-rich-media).	|
|marketplace\_id	|String	|The [marketplace identifier](https://developer-docs.amazon.com/sp-api/docs/marketplace-ids) associated with the account.	|
|time\_window\_start	|String	|The start of the hour to which the performance data is attributed (ISO 8601 date time). Time zone for each advertiser is indicated on the advertiser [profile](reference/2/profiles#/Profiles/listProfiles).	|
|campaign\_id	|String	|Unique numerical ID for a campaign.	|
|ad\_id	|String	|Unique numerical ID for the ad.	|
|ad\_group\_id	|String	|Unique numerical ID for an ad group.	|
|advertiser\_id	|String	|ID associated with the advertiser. You can retrieve this ID using the [GET v2/profiles](https://advertising.amazon.com/API/docs/en-us/reference/2/profiles#/Profiles/listProfiles) endpoint. This value is **not** unique and may be the same across marketplaces. Note: For non-seller accounts, this advertiser_id is set to the entity ID.	|
|keyword\_id	|String	|ID of the keyword used for a bid.	|
|keyword\_text	|String	|Text of the keyword or phrase used for a bid.	|
|keyword\_type	|String	|The type of targeting used in the expression.|
|placement\_type	|String	| The page location where an ad appeared. Possible values: Detail Page on-Amazon, Other on-Amazon, Top of Search on-Amazon.	|
|video\_5\_second\_views	|Long	|The number of impressions where the customer watched the complete video or 5 seconds (whichever is shorter).	|
|video\_complete\_views	|Long	|The number of impressions where the video was viewed to 100%.	|
|video\_first\_quartile\_views	|Long	|The number of impressions where the video was viewed to 25%.	|
|video\_midpoint\_views	|Long	|The number of impressions where the video was viewed to 50%.	|
|video\_third\_quartile\_views	|Long	|The number of impressions where the video was viewed to 75%.	|
|video\_unmutes	|Long	|The number of impressions where a customer unmuted the video.	|

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
          "aws:SourceArn": "arn:aws:sns:us-east-1:010312603579:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:662188760626:*"
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:618223300352:*"
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

Note that it may take up to 12 hours for data to populate after a video impression or view event occurs. As part of the validation process, impressions or views can be invalidated during the following 72 hours post the initial report. Amazon Marketing Stream will automatically deliver any adjustments to your queue.