---
title: Amazon DSP campaign management
description: Details and schema for the DSP campaigns, campaign flights, ad groups, and ad group targets datasets.
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Campaign management
---

# Amazon DSP campaign management datasets

These datasets contain information about Amazon DSP campaigns, flights, ad groups, and targets.

>[NOTE] To subscribe or manage subscriptions to the DSP campaign management datasets, use the [/dsp/streams/subscriptions](amazon-marketing-stream/openapi#tag/DSP-Stream-Subscription) endpoints.

>[TIP:Note about marketplace\_id] `marketplace_id` should only be paired with `advertiser_id` to uniquely identify an Amazon DSP advertiser. Please do not use it to determine locale information.

## Amazon DSP campaigns (beta)

The Amazon DSP campaigns dataset provides a snapshot of all your campaign objects.

### Dataset ID

For this dataset, the ID is `adsp-campaigns`. 

### Schema

|Field	|Type	|Description	|
|---	|---	|---	|
|dataset\_id	|string	|An identifier used to identify the dataset (in this case, `adsp-campaigns`.)	|
|advertiser\_id	|string	|Unique identifier of the advertiser (standard in Stream datasets).	|
|marketplace\_id	|string	|The marketplace of the advertiser.	|
|dspEntityId	|string	|DSP EntityId of the advertiser.	|
|campaignId	|string	|Unique identifier of the campaign.	|
|name	|string	|The name for the campaign.	|
|version | long | This numeric value is incremented when changes are made to the object, representing a chronological history of changes. For example, the creation event would be the lowest version, while a subsequent update would be greater. | 
|creationDateTime	|string	|The campaign creation date.	|
|lastUpdatedDateTime	|string	|The campaign's last updated date.	|
|currencyCode	|enum	|ISO 4217 standard currency code for all currency-related campaign attributes (bid, budget)	|
|state	|enum	|The user-defined activation status, falling into one of the following two values: [ACTIVE, INACTIVE]	|
|optimization.bidStrategy	|enum	|A bid strategy specifies how we determine bids on your behalf. Examples include: [ SPEND\_BUDGET\_IN\_FULL, MAXIMIZE\_PERFORMANCE, OTHER ]	|
|optimization.bidOptimization	|boolean	|TRUE will automatically adjust bids for impressions that are more likely to improve your selected goal KPI.	|
|optimization.budgetOptimization	|boolean	|TRUE will automatically adjust budgets towards better performing ad groups based on your selected goal KPI.	|
|optimization.goalSettings.goalType	|enum	|This is the primary goal that you're trying to achieve with campaign and ad groups. Examples include: [ AWARENESS, ENGAGEMENT\_WITH\_MY\_AD, CONSIDERATIONS\_ON\_AMAZON, CONVERSIONS\_OFF\_AMAZON, PURCHASES\_ON\_AMAZON, PURCHASES\_ON\_OFF\_AMAZON, MOBILE\_APP\_INSTALLS, OTHER ]	|
|optimization.goalSettings.goalKpi	|enum	|The goal KPI is the key performance metric that'll be used to measure success of your campaign. Examples include: [ VIDEO\_COMPLETION\_RATE, CLICK\_THROUGH\_RATE, COST\_PER\_CLICK, COST\_PER\_ACTION, COST\_PER\_DOWNLOAD, COST\_PER\_INSTALL, DETAIL\_PAGE\_VIEW\_RATE, COST\_PER\_DETAIL\_PAGE\_VIEW, RETURN\_ON\_AD\_SPEND, TOTAL\_RETURN\_ON\_AD\_SPEND, COMBINED\_RETURN\_ON\_AD\_SPEND, COST\_PER\_VIDEO\_COMPLETION, NONE, OTHER, REACH ]	|
|optimization.goalSettings.targetKpi	|double	|The target value of the goalKpi. Depending on the goalKpi selected, this will be either currency based, percentage based, or an integer (e.g. REACH).	|
|optimization.goalSettings.currencyCode	|enum	|The campaign's currency code.	|
|fees.agencyFeePercentage	|double	|A service fee that is subtracted from budget.	|
|frequencies	|array	|The maximum number of times this campaign serves an impression to a user.	|
|frequencies.type	|enum	|The type of advertising frequency. If UNCAPPED, no other fields are used. Example values [ UNCAPPED, CUSTOM ]	|
|frequencies.timeUnit	|enum	|The time unit. Example values: [ DAYS, HOURS, OTHER ]	|
|frequencies.timeCount	|integer	|The count of time units.	|
|frequencies.maxImpressions	|integer	|The maximum number of times an impression is served per user.	|
|budgetCaps	|array	|A limit on how much gets spent per day and/or month to more carefully control budget. This refers to calendar day or month, so applying this mid-day or month will assume the cap is for the rest of the day or month. Setting budget caps will affect pacing and may cause delivery spikes depending on how and when they're set. 	|
|budgetCaps.monetaryBudget.amount	|double	| The budget cap amount. |
|budgetCaps.monetaryBudget.currencyCode	|enum	| The currency code of the budget cap. |
|budgetCaps.recurrenceTimePeriod	|enum	| Examples of recurrenceTimePeriod: [ DAILY, MONTHLY, OTHER ]. |
|tags.comments	|string	|Comments are only visible inside of the tool. Use this field to track variations in targeting, optimization notes, etc.	|
|tags.purchaseOrderNumber	|string	|External purchase order ID, set by advertiser. It will be available in all dashboards and downloadable reports.	|

### Sample payload

```json
{
  "dataset_id": "adsp-campaigns",
  "advertiser_id": "",
  "marketplace_id": "",
  "dspEntityId": "",
  "campaignId": "",
  "name": "",
  "version": 0,
  "creationDateTime": "",
  "lastUpdatedDateTime": "",
  "currencyCode": "",
  "state": "",
  "optimization": {
    "bidStrategy": "",
    "bidOptimization": false,
    "budgetOptimization": false,
    "goalSettings": {
      "goalType": "",
      "goalKpi": "",
      "targetKpi": 0.0,
      "currencyCode": ""
    }
  },
  "fees": {
    "agencyFeePercentage": 0.0
  },
  "frequencies": [
    {
      "type": "",
      "timeUnit": "",
      "timeCount": 0,
      "maxImpressions": 0
    }
  ],
  "budgetCaps": [
    {
      "monetaryBudget": {
        "amount": 0.0,
        "currencyCode": ""
      },
      "recurrenceTimePeriod": ""
    }
  ],
  "tags": {
    "comments": "",
    "purchaseOrderNumber": ""
  }
}
```


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
          "aws:SourceArn": "arn:aws:sns:us-east-1:153247821255:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:599052634802:*"
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:216875695489:*"
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

You will receive a notification in near real time for every change in the configuration of your campaign.

## Amazon DSP campaign flights (beta)

The Amazon DSP campaigns dataset provides a snapshot of all your campaign flight objects.

### Dataset ID

For this dataset, the ID is `adsp-campaign-flights`. 

### Schema

|Field	|Type	|Description	|
|---	|---	|---	|
|dataset\_id	|string	|An identifier used to identify the dataset (in this case, `adsp-campaign-flights`.)	|
|advertiser\_id	|string	|Unique identifier of the advertiser (standard in Stream datasets).	|
|marketplace\_id	|string	|The marketplace of the advertiser.	|
|version | long | This numeric value is incremented when changes are made to the object, representing a chronological history of changes. For example, the creation event would be the lowest version, while a subsequent update would be greater. | 
|dspEntityId	|string	|DSP EntityId of the advertiser.	|
|campaignId	|string	|Unique identifier of the campaign this flight applies to.	|
|flightId	|string	|Unique identifier of this campaign-flight.	|
|creationDateTime	|string	|The campaign-flight creation date.	|
|lastUpdatedDateTime	|string	|The campaign-flight last updated date.	|
|startDateTime	|string	|Start date of flight.	|
|endDateTime	|string	|End date of flight.	|
|budgetAmount	|double	|The numerical value of the budget.	|

### Sample payload

```json
{
  "dataset_id": "adsp-campaign-flights",
  "advertiser_id": "",
  "marketplace_id": "",
  "version": 0,
  "dspEntityId": "",
  "campaignId": "",
  "flightId": "",
  "creationDateTime": "",
  "lastUpdatedDateTime": "",
  "startDateTime": "",
  "endDateTime": "",
  "budgetAmount": 0.0
}
```

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
          "aws:SourceArn": "arn:aws:sns:us-east-1:700228448367:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:633559263003:*"
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:451213518288:*"
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

You will receive a notification in near real time for every change in the configuration of your campaign flight.

## Amazon DSP ad groups (beta)

The Amazon DSP campaigns dataset provides a snapshot of all your ad group objects.

### Dataset ID

For this dataset, the ID is `adsp-adgroups`. 

### Schema

|Field	|Type	|Description	|
|---	|---	|---	|
|dataset\_id	|string	|An identifier used to identify the dataset (in this case, `adsp-campaign-flights`.)	|
|advertiser\_id	|string	|Unique identifier of the advertiser (standard in Stream datasets).	|
|marketplace\_id	|string	|The marketplace of the advertiser.	|
|dspEntityId	|string	|DSP EntityId of the advertiser.	|
|campaignId	|string	|Unique identifier of the campaign this flight applies to.	|
|adgroupId	|string	|Unique identified of this ad group. 	|
|name	|string	|The ad group's name.	|
|version | long | This numeric value is incremented when changes are made to the object, representing a chronological history of changes. For example, the creation event would be the lowest version, while a subsequent update would be greater. | 
|startDateTime	|string	|Start date of this adGroup, in ISO format.	|
|endDateTime	|string	|End date of this adGroup, in ISO format.	|
|state	|enum	|The user-defined activation status, falling into one of the following two values: [ACTIVE, INACTIVE]	|
|inventoryType	|string	|The ad group type.	|
|creativeRotationType	|enum	|The creative rotation type. Examples include: [ RANDOM, WEIGHTED, OTHER ]	|
|tags.comments	|string	|Comments are only visible inside of the tool. Use this field to track variations in targeting, optimization notes, etc.	|
|tags.purchaseOrderNumber	|string	|External purchase order ID, set by advertiser. It will be available in all dashboards and downloadable reports.	|
|frequencies	|array	|A list of frequency information about this ad group.	|
|frequencies.type	|enum	|The type of advertising frequency. If UNCAPPED, no other fields are used. Example values: [ UNCAPPED, CUSTOM ]	|
|frequencies.timeUnit	|enum	|The time unit. Example values: [ DAYS, HOURS, OTHER ]	|
|frequencies.timeCount	|integer	|The count of time units.	|
|frequencies.maxImpressions	|integer	|The maximum number of times an impression is served per user.	|
|advertisedProductCategoryIds	|string array	|The array of identifiers of product categories associated with the ad group.	|
|budgetCaps	|array	|The per day or per month spending limit.	|
|budgetCaps.recurrenceTimePeriod	|enum	|The type of recurrence for the spending limit. Examples include: [ DAILY, MONTHLY, OTHER ]	|
|budgetCaps.monetaryBudget.amount	|double	|The spending limit expressed in the currency of the ad group.	|
|budgetCaps.monetaryBudget.currencyCode	|enum	|The currency set on the ad group level.	|
|pacing.catchupPercentage	|double	|It is the numerical value of the boost percentage, which specifies additional delivery that can be enforced to catch up when under-pacing. A catch up boost offsets any drops that may occur while ad groups are delivering during weekends, holidays, or other busy time periods.	|
|pacing.deliveryProfile	|enum	|Determines how the system will spend budget and deliver impressions over the ad group’s flight. The following options are available: EVENLY: Distributes the delivery evenly across the duration of the ad groups. For even delivery, we recommend using a catch-up boost to prepare for expected dips from weekends and holidays. FRONT\_LOADED: Increases delivery during the first half of the campaign (up to 25%), then up to 5% during the second half. This has a catch-up boost of 40% ASAP: Makes your entire budget available to spend immediately. This is ideal for ad groups with limited inventory or when there's no requirement to spend throughout the length of the campaign. Warning: Selecting ASAP may result in your entire budget being spent immediately. OTHER: Returned by GET operations as a substitute for values that are incompatible with the requested GET API version.	|
|bid.currencyCode	|enum	|The currency set on the ad group level.	|
|bid.baseBid	|double	|This is the ideal amount you'd like to pay (per thousand impressions) for your ad inventory expressed in the currency of the campaign. This price isn't guaranteed, but we optimize the bidding strategy to try to meet this goal.	|
|bid.maxAverageCpm	|double	|The maximum cost-per-thousand impressions bid for media supply. Expressed in dollars.	|
|optimization.budgetOptimization	|boolean	|Set to true to enable budget optimization for the ad group.	|
|optimization.bidStrategy	|enum	|A bid strategy specifies how we determine bids on your behalf for each opportunity to serve an impression. SPEND\_BUDGET\_IN\_FULL: Our algorithm will focus on spending the full budget. Performance will be the second priority. MAXIMIZE\_PERFORMANCE: Our algorithm will focus on performance. For the best performance and budget delivery, you will need to monitor and adjust your bid amounts. USE\_CAMPAIGN\_STRATEGY: The ad group will inherit the campaigns bid strategy. 	|
|targetingSettings.siteLanguage	|array	|The site language targeting type, in ISO 639-1 language code. format. Language Targeting enables targeting supply content language. When enabled, the ad group will automatically target supply language based on the creative language selected. If this is set, the ad group will not bid on placements with unrecognized language.  Only applicable and required when `inventoryType` is STANDARD\_DISPLAY or VIDEO.	|
|targetingSettings.amazonViewability.viewablitiyTier	|enum	|The targeted viewability tier. These are predictions based on machine learning and aren’t guaranteed. Selecting a higher percentage limits overall reach. Examples: [ ALL\_TIERS, GREATER\_THAN\_70\_PERCENT, GREATER\_THAN\_60\_PERCENT, GREATER\_THAN\_50\_PERCENT, GREATER\_THAN\_40\_PERCENT, LESS\_THAN\_40\_PERCENT, OTHER ]	|
|targetingSettings.amazonViewability.includeUnmeasurableImpressions	|boolean	|If set to true to include impressions where impressions can't be measured.	|
|targetingSettings.timeZoneType	|enum	|The time zone associated with ad group delivery when daypart targeting is applied. VIEWER is the time zone of the person viewing the ad. ADVERTISER\_REGION refers to one of these three options based on the region of the advertiser: - US/Eastern: The time zone of the US East Coast which can be EST (UTC-5) or EDT (UTC-4). - Europe/London: The time zone of London which can be GMT (UTC) or BST (UTC+1). - Asia/Japan: Japan Standard Time (UTC+9).	|
|targetingSettings.userLocation	|enum	|The geographical location type of users targeted. Examples include: [ EVERYWHERE, NON\_US, AE, AU, BR, CA, DE, ES, FR, IN, IT, JP, MX, NL, PL, SA, SE, SG, TR, UK, US, OTHER ]	|
|targetingSettings.userLocationSignal	|enum	|The type of signal used to determine a users location when geo location targeting is applied. CURRENT: Includes people who are currently in this location. MULTIPLE\_SIGNALS: Multiple location signals are used, including current and home location. Examples include: [ CURRENT, MULTIPLE\_SIGNALS, OTHER ]	|
|creationDateTime	|string	|The adGroup creation date.	|
|lastUpdatedDateTime	|string	|The adGroup last updated date.	|

### Sample payload

```json
{
  "dataset_id": "adsp-adgroups",
  "advertiser_id": "",
  "marketplace_id": "",
  "version": 0,
  "inventoryType": "",
  "lastUpdatedDateTime": "",
  "campaignId": "",
  "creativeRotationType": "",
  "dspEntityId": "",
  "endDateTime": "",
  "advertisedProductCategoryIds": "[,]",
  "budgetCaps": [
    {
      "monetaryBudget": {
        "amount": 0.0,
        "currencyCode": ""
      },
      "recurrenceTimePeriod": ""
    }
  ],
  "startDateTime": "",
  "pacing": {
    "catchupPercentage": 0.0,
    "deliveryProfile": ""
  },
  "optimization": {
    "bidStrategy": "",
    "budgetOptimization": false
  },
  "targetingSettings": {
    "amazonViewability": {
      "viewablitiyTier": "",
      "includeUnmeasurableImpressions": false
    },
    "siteLanguage": [
      "",
      ""
    ],
    "timeZoneType": "",
    "userLocation": "",
    "userLocationSignal": "",
  },
  "adGroupId": "",
  "name": "",
  "state": "",
  "bid": {
    "maxAverageCpm": 0.0,
    "baseBid": 0.0,
    "currencyCode": ""
  },
  "frequencies": [
    {
      "timeCount": 0,
      "maxImpressions": 0,
      "timeUnit": "",
      "type": ""
    }
  ],
  "creationDateTime": "",
  "tags": {
    "comments": "",
    "purchaseOrderNumber": ""
  }
}

```

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
          "aws:SourceArn": "arn:aws:sns:us-east-1:222778752755:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:682324742468:*"
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:360850786875:*"
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

You will receive a notification in near real time for every change in the configuration of your ad group.

## Amazon DSP ad group targets (beta)

The Amazon DSP campaigns dataset provides a snapshot of all your target objects. 

### Dataset ID

For this dataset, the ID is `adsp-adgroup-targets`. 

### Schema

The ad group targets dataset currently contains the following target types: audience, geolocation, day parting, devices, application, and third party, all of which have both common and unique fields. The first object in the list below contains the core fields that will be included in every adsp-adgroup-target document, which will be extended for each type. The objects listed after are the fields unique to each type of the target types.

>[NOTE] There are three types of targets (audience, geolocation, and app) that can have documents that are too large to be written through Amazon Marketing Stream via SQS. This is due to the limitation of SQS message size to 256 KB, and does not impact targets in advertising systems in any way. We are working on an alternate solution to resolve the issue. This limitation impacts a small percentage of overall records, and in this version of our API we still send these records, but we will instead denote the fields have been truncated as `PAYLOAD_TRUNCATED`. If you would like to retrieve this information in the interim, please call the [geotargeting API](dsp-ad-group-targeting-geo), [audiences targeting API](dsp-ad-group-targeting-audiences), or the [app targeting API](dsp-ad-group-targeting-apps) to receive the documents in full.

### Common fields

|Field	|Type	|Description	|
|---	|---	|---	|
|dataset\_id	|string	|An identifier used to identify the dataset (in this case, `adsp-campaign-flights`.)	|
|advertiser\_id	|string	|Unique identifier of the advertiser (standard in Stream datasets).	|
|marketplace\_id	|string	|The marketplace of the advertiser.	|
|dspEntityId	|string	|DSP EntityId of the advertiser.	|
|campaignId	|string	|Unique identifier of the campaign this target applies to.	|
|adgroupId	|string	|Unique identified of the ad group this target applies to.	|
|version | long | This numeric value is incremented when changes are made to the object, representing a chronological history of changes. For example, the creation event would be the lowest version, while a subsequent update would be greater. | 
|creationDateTime	|string	|Creation time of the ad group the target belongs to, in ISO format.	|
|lastUpdatedDateTime	|string	|Last modified time of the ad group the target belongs to, in ISO format.	|

### Geolocation targets

Geolocation targeting entries contain the following fields.

|Field	|Type	|Description	|
|---	|---	|---	|
|geoLocationTargets	|array	|Contains the list of GeoLocationTargets for this AdGroup	|
|geoLocationTargets.negative	|boolean	|Whether to include (false) or exclude (true) users who match the given target	|
|geoLocationTargets.geoLocationId	|string	|The unique identifier for the location	|

#### Sample payload

```json
{
  "dataset_id": "adsp-adgroup-targets",
  "advertiser_id": "",
  "marketplace_id": "",
  "version": 0,
  "dspEntityId": "",
  "campaignId": "",
  "adgroupId": "",
  "creationDateTime": "",
  "lastUpdatedDateTime": "",
  "geoLocationTargets": [
    {
      "negative": false,
      "geoLocationId": ""
    },
    {
      "negative": true,
      "geoLocationId": ""
    }
  ]
}

```

### Daypart targets

Daypart targets contain the following fields.

|Field	|Type	|Description	|
|---	|---	|---	|
|daypartTargets	|array	|Contains the list of DaypartTargets for this AdGroup	|
|daypartTargets.dayOfWeek	|string	|Day of the week for this target. ex: "**SUNDAY**"	|
|daypartTargets.hourSlot	|integer	|Hour of day for this target. ex: **0** for midnight, **15** for 3pm, etc	|

#### Sample payload

```json
{
  "dataset_id": "adsp-adgroup-targets",
  "advertiser_id": "",
  "marketplace_id": "",
  "version": 0,
  "dspEntityId": "",
  "campaignId": "",
  "adgroupId": "",
  "creationDateTime": "",
  "lastUpdatedDateTime": "",
  "daypartTargets": [
    {
      "dayOfWeek": "SUNDAY",
      "hourSlot": 0
    },
    {
      "dayOfWeek": "SUNDAY",
      "hourSlot": 1
    },
    {
      "dayOfWeek": "FRIDAY",
      "hourSlot": 15
    },
    {
      "dayOfWeek": "FRIDAY",
      "hourSlot": 16
    }
  ]
}
```


### Device targets

Device targets contain the following fields.

|Field	|Type	|Description	|
|---	|---	|---	|
|deviceTargets	|array	|Contains the list of DeviceTargets for this AdGroup	|
|deviceTargets.deviceType	|enum	|Type of device being targeted. Must be one of [DESKTOP, MOBILE]	|
|deviceTargets.mobileEnvironment	|enum	|This field is only set if deviceType is `MOBILE`.  If set, must be one of [WEB, APP]	|
|deviceTargets.mobileDevice	|enum	|This field is only set if mobileEnvironment is `APP`. If set, must be one of [IPHONE, IPAD, ANDROID, KINDLE\_FIRE, KINDLE\_FIRE\_HD]	|
|deviceTargets.deviceOrientation	|enum	|This field is only set if mobileEnvironment is `APP`. If set, must be one of [PORTRAIT, LANDSCAPE]	|
|deviceTargets.mobileOS	|enum	|This field is only set if mobileEnvironment is `WEB`. If set, must be one of [IOS, ANDROID]	|

#### Sample payload

```json
{
  "dataset_id": "adsp-adgroup-targets",
  "advertiser_id": "",
  "marketplace_id": "",
  "version": 0,
  "dspEntityId": "",
  "campaignId": "",
  "adGroupId": "",
  "creationDateTime": "",
  "lastUpdatedDateTime": "",
  "deviceTargets": [
    {
      "deviceType": "MOBILE",
      "mobileEnvironment": "APP",
      "mobileOS": null,
      "mobileDevice": null,
      "deviceOrientation": "PORTRAIT"
    },
    {
      "deviceType": "MOBILE",
      "mobileEnvironment": "APP",
      "mobileOS": null,
      "mobileDevice": null,
      "deviceOrientation": "LANDSCAPE"
    }
  ]
}
```

### Audience targets

Audience targets contain the following fields.

|Field	|Type	|Description	|
|---	|---	|---	|
|audienceTargets	|array	|Contains the list of DeviceTargets for this AdGroup	|
|audienceTargets.negative	|boolean	|Whether to include (false) or exclude (true) users who match the given target when serving an ad.	|
|audienceTargets.inGroupOperator	|enum	|The interoperator used among audiences within the same audience group. This is a legacy field that is no longer supported, and will only return a non-null value on legacy ad groups. Values: [ ANY, ALL ]	|
|audienceTargets.acrossGroupOperator	|enum	|The intraoperator used among audiences between audience groups. This is a legacy field that is no longer supported, and will only return a non-null value on legacy ad groups. Values: [ ANY, ALL ]	|
|audienceTargets.groupId	|string	|A unique ID for the audience group.	|
|audienceTargets.audienceId	|string	|The unique identifier for the audience. Use the [audiences discovery resource](audiences)to look up audience identifiers.	|

#### Sample payload

```json
{
  "dataset_id": "adsp-adgroup-targets",
  "advertiser_id": "",
  "marketplace_id": "",
  "version": 0,
  "dspEntityId": "",
  "campaignId": "",
  "adGroupId": "",
  "creationDateTime": "",
  "lastUpdatedDateTime": "",
  "audienceTargets": [
    {
      "negative": true,
      "inGroupOperator": "string",
      "acrossGroupOperator": "string",
      "groupId": "string",
      "audienceId": "string"
    }
  ]
}
```

### App targets

App targets contain the following fields.

|Field	|Type	|Description	|
|---	|---	|---	|
|appTargets	|array	|Contains the list of App Targets for this AdGroup	|
|appTargets.negative	|boolean	|Whether to include (false) or exclude (true) apps. All apps within the same ad group must be either included or excluded. If appType is STV, this is set to to true.	|
|appTargets.appType	|enum	|The type of app being targeted. Only one type can be used per ad group. Values: [MOBILE, STV]	|
|appTargets.appId	|string	|The unique identifier for the app.	|

#### Sample payload

```json
{
  "dataset_id": "adsp-adgroup-targets",
  "advertiser_id": "",
  "marketplace_id": "",
  "version": 0,
  "dspEntityId": "",
  "campaignId": "",
  "adGroupId": "",
  "creationDateTime": "",
  "lastUpdatedDateTime": "",
  "appTargets": [
    {
      "negative": true,
      "appType": "MOBILE",
      "appId": "string"
    }
  ]
}
```

### Third-party targets

|Field	|Type	|Description	|
|---	|---	|---	|
|thirdPartyTargets	|array	|List of third-party targeting settings applied on given ad group id	|
|thirdPartyTargets.doubleVerifyFraudInvalidTraffic	|struct	|List of third-party targeting settings applied on given ad group id. |
|thirdPartyTargets.doubleVerifyFraudInvalidTraffic.excludeImpressions	|boolean	|If set to true, will exclude impressions delivered to devices identified to be fraudulent or invalid.	|
|thirdPartyTargets.doubleVerifyFraudInvalidTraffic.excludeAppsAndSites	|enum	|Supported [ ALLOW\_ALL, FRAUD\_TRAFFIC\_LEVEL\_GTE\_100, FRAUD\_TRAFFIC\_LEVEL\_GTE\_50, FRAUD\_TRAFFIC\_LEVEL\_GTE\_25, FRAUD\_TRAFFIC\_LEVEL\_GTE\_10, FRAUD\_TRAFFIC\_LEVEL\_GTE\_08, FRAUD\_TRAFFIC\_LEVEL\_GTE\_06, FRAUD\_TRAFFIC\_LEVEL\_GTE\_04, FRAUD\_TRAFFIC\_LEVEL\_GTE\_02 ]	|
|thirdPartyTargets.doubleVerifyFraudInvalidTraffic.blockAppAndSites	|boolean	|True will block applications and sites with insufficient historical fraud and invalid traffic statistics. This will not be applicable if ALLOW\_ALL is chosen.	|
|thirdPartyTargets.integralAdScienceFraudInvalidTraffic	|struct	|Supported InventoryTypes['STANDARD\_DISPLAY', 'MOBILE\_DISPLAY', AAP\_MOBILE\_APP', 'VIDEO'].	|
|thirdPartyTargets.integralAdScienceFraudInvalidTraffic.targetSetting	|enum	|The type of fraud invalid traffic. [ ALLOW\_ALL, FRAUD\_INVALID\_TRAFFIC\_EXCLUDE\_HIGH\_RISK, FRAUD\_INVALID\_TRAFFIC\_EXCLUDE\_HIGH\_MODERATE\_RISK]	|
|thirdPartyTargets.oracleDataCloudFraudInvalidTraffic	|struct	|Supported InventoryTypes['STANDARD\_DISPLAY', 'AAP\_MOBILE\_APP', 'VIDEO'].	|
|thirdPartyTargets.oracleDataCloudFraudInvalidTraffic.targetSetting	|enum	|The fraud invalid traffic type. [ ALLOW\_ALL, FRAUD\_INVALID\_TRAFFIC\_ESSENTIAL\_PROTECTION, FRAUD\_INVALID\_TRAFFIC\_MAXIMUM\_PROTECTION ]	|
|thirdPartyTargets.pixalateFraudInvalidTraffic	|struct	|Supported InventoryTypes['STANDARD\_DISPLAY', 'MOBILE\_DISPLAY', AAP\_MOBILE\_APP', 'VIDEO'].	|
|thirdPartyTargets.pixalateFraudInvalidTraffic.excludeIpAddressAndUserAgents	|boolean	|If set to true, exclude traffic from IPV4 and IPV6 addresses and user agents identified to be fraudulent or invalid.	|
|thirdPartyTargets.pixalateFraudInvalidTraffic.excludeOttAndMobileDevices	|boolean	|If set to true, exclude traffic from OTT and Mobile devices identified to be fraudulent or invalid.	|
|thirdPartyTargets.pixalateFraudInvalidTraffic.excludeRemovedAppsFromAppStores	|boolean	|If set to true, exlude traffic from Apps that have been removed from the google play and apple app stores in the last 6 months.	|
|thirdPartyTargets.pixalateFraudInvalidTraffic.excludeAppsAndDomains	|boolean	|If set to true, exclude traffic from Apps and Domains identified to be fraudulent or invalid.	|
|thirdPartyTargets.doubleVerifyStandardDisplayBrandSafety	|struct	|Supported InventoryTypes['STANDARD\_DISPLAY'].	|
|thirdPartyTargets.doubleVerifyStandardDisplayBrandSafety.contentCategories	|array	|A list of content categories to exclude from targeting. [ AD\_SERVER, CELEBRITY\_GOSSIP, CULTS\_SURVIVALISM, GAMBLING, INCENTIVIZED\_MALWARE\_CLUTTER, INFLAMMATORY\_POLITICS\_NEWS, NEGATIVE\_NEWS\_FINANCIAL, NEGATIVE\_NEWS\_PHARMACEUTICAL, NON\_STANDARD\_CONTENT\_NON\_ENGLISH, NON\_STANDARD\_CONTENT\_PARKING\_PAGE, OCCULT, PIRACY\_COPYRIGHT\_INFRINGEMENT, UNMODERATED\_UGC\_FORUMS\_IMAGES\_VIDEO, EXTREME\_GRAPHIC ]	|
|thirdPartyTargets.doubleVerifyStandardDisplayBrandSafety.contentCategoriesWithRisk	|map	|The Double Verify brand suitability risk level. [ ALLOW\_ALL, HIGH, HIGH\_MEDIUM, HIGH\_MEDIUM\_LOW ]	|
|thirdPartyTargets.doubleVerifyStandardDisplayBrandSafety.unknownContent	|boolean	|If set to true, exclude unknown content.	|
|thirdPartyTargets.doubleVerifyBrandSafety	|struct	|Supported InventoryTypes['AAP\_MOBILE\_APP', 'VIDEO'].	|
|thirdPartyTargets.doubleVerifyBrandSafety.appStarRating	|enum	|App star rating to be used for excluding apps. [ ALLOW\_ALL, APP\_STAR\_RATING\_LT\_1\_POINT\_5\_STARS, APP\_STAR\_RATING\_LT\_2\_STARS, APP\_STAR\_RATING\_LT\_2\_POINT\_5\_STARS, APP\_STAR\_RATING\_LT\_3\_STARS, APP\_STAR\_RATING\_LT\_3\_POINT\_5\_STARS, APP\_STAR\_RATING\_LT\_4\_STARS, APP\_STAR\_RATING\_LT\_4\_POINT\_5\_STARS ]	|
|thirdPartyTargets.doubleVerifyBrandSafety.contentCategories	|array	|A list of content categories to exclude from targeting. [ AD\_SERVER, CELEBRITY\_GOSSIP, CULTS\_SURVIVALISM, GAMBLING, INCENTIVIZED\_MALWARE\_CLUTTER, INFLAMMATORY\_POLITICS\_NEWS, NEGATIVE\_NEWS\_FINANCIAL, NEGATIVE\_NEWS\_PHARMACEUTICAL, NON\_STANDARD\_CONTENT\_NON\_ENGLISH, NON\_STANDARD\_CONTENT\_PARKING\_PAGE, OCCULT, PIRACY\_COPYRIGHT\_INFRINGEMENT, UNMODERATED\_UGC\_FORUMS\_IMAGES\_VIDEO, EXTREME\_GRAPHIC ]	|
|thirdPartyTargets.doubleVerifyBrandSafety.appAgeRating	|array	|A list of app age ratings to be used for excluding apps. For example, TEENS\_12\_PLUS will only exclude apps with content rated for everyone ages 12 and over. UNKNOWN will exclude apps with content unrated or unknown to Double Verify. [ EVERYONE\_4\_PLUS, TWEENS\_9\_PLUS, TEENS\_12\_PLUS, MATURE\_17\_PLUS, ADULTS\_ONLY\_18\_PLUS, UNKNOWN ]	|
|thirdPartyTargets.doubleVerifyBrandSafety.excludeAppsWithInsufficientRating	|boolean	|If set to true, exclude unofficial apps or apps with insufficient user ratings (<100 lifetime).	|
|thirdPartyTargets.doubleVerifyBrandSafety.contentCategoriesWithRisk	|map	|A map from content categories to risk level to exclude from targeting. Available keys are: [ADULT\_CONTENT, ALCOHOL, CRIME, DEATH\_INJURIES, DISASTER\_AVIATION, DISASTER\_MAN\_MADE, DISASTER\_NATURAL, DISASTER\_TERRORIST\_EVENTS, DISASTER\_VEHICLE, HATE\_SPEECH, PROFANITY, SUBSTANCE\_ABUSE, TOBACCO\_ECIGARETTES, VIOLENCE\_EXTREME\_GRAPHIC].|
|thirdPartyTargets.doubleVerifyBrandSafety.unknownContent	|boolean	|Set to true to exclude unknown content.	|
|thirdPartyTargets.integralAdScienceBrandSafety	|struct	|Supported InventoryTypes['STANDARD\_DISPLAY', 'AAP\_MOBILE\_APP', 'VIDEO']	|
|thirdPartyTargets.integralAdScienceBrandSafety.iasBrandSafetyOffensiveLanguage	|enum	|The IAS brand safety risk level. [ ALLOW\_ALL, BRAND\_SAFETY\_EXCLUDE\_HIGH\_RISK, BRAND\_SAFETY\_EXCLUDE\_HIGH\_AND\_MODERATE\_RISK ]	|
|thirdPartyTargets.integralAdScienceBrandSafety.excludeContent	|boolean	|If set to true, exclude content that Integral Ad Science is not able to rate.	|
|thirdPartyTargets.integralAdScienceBrandSafety.iasBrandSafetyAlcohol	|enum	|The IAS brand safety risk level. [ ALLOW\_ALL, BRAND\_SAFETY\_EXCLUDE\_HIGH\_RISK, BRAND\_SAFETY\_EXCLUDE\_HIGH\_AND\_MODERATE\_RISK ]	|
|thirdPartyTargets.integralAdScienceBrandSafety.iasBrandSafetyIllegalDownloads	|enum	|The IAS brand safety risk level. [ ALLOW\_ALL, BRAND\_SAFETY\_EXCLUDE\_HIGH\_RISK, BRAND\_SAFETY\_EXCLUDE\_HIGH\_AND\_MODERATE\_RISK ]	|
|thirdPartyTargets.integralAdScienceBrandSafety.iasBrandSafetyHateSpeech	|enum	|The IAS brand safety risk level. [ ALLOW\_ALL, BRAND\_SAFETY\_EXCLUDE\_HIGH\_RISK, BRAND\_SAFETY\_EXCLUDE\_HIGH\_AND\_MODERATE\_RISK ]	|
|thirdPartyTargets.integralAdScienceBrandSafety.iasBrandSafetyIllegalDrugs	|enum	|The IAS brand safety risk level. [ ALLOW\_ALL, BRAND\_SAFETY\_EXCLUDE\_HIGH\_RISK, BRAND\_SAFETY\_EXCLUDE\_HIGH\_AND\_MODERATE\_RISK ]	|
|thirdPartyTargets.integralAdScienceBrandSafety.iasBrandSafetyAdult	|enum	|The IAS brand safety risk level. [ ALLOW\_ALL, BRAND\_SAFETY\_EXCLUDE\_HIGH\_RISK, BRAND\_SAFETY\_EXCLUDE\_HIGH\_AND\_MODERATE\_RISK ]	|
|thirdPartyTargets.integralAdScienceBrandSafety.iasBrandSafetyViolence	|enum	|The IAS brand safety risk level. [ ALLOW\_ALL, BRAND\_SAFETY\_EXCLUDE\_HIGH\_RISK, BRAND\_SAFETY\_EXCLUDE\_HIGH\_AND\_MODERATE\_RISK ]	|
|thirdPartyTargets.integralAdScienceBrandSafety.iasBrandSafetyGambling	|enum	|The IAS brand safety risk level. [ ALLOW\_ALL, BRAND\_SAFETY\_EXCLUDE\_HIGH\_RISK, BRAND\_SAFETY\_EXCLUDE\_HIGH\_AND\_MODERATE\_RISK ]	|
|thirdPartyTargets.oracleDataCloudBrandSafety	|struct	|Supported InventoryTypes['STANDARD\_DISPLAY', 'AAP\_MOBILE\_APP', 'VIDEO']. The oracle data cloud brand safety.	|
|thirdPartyTargets.oracleDataCloudBrandSafety.essentialProtection	|enum	|[ ADULT, ARMS, CRIME, INJURY, PIRACY, DRUGS, HATE\_SPEECH, MILITARY, OBSCENITY, TERRORISM, TOBACCO ]	|
|thirdPartyTargets.oracleDataCloudBrandSafety.targetingOption	|enum	|[ NO\_BRAND\_SAFETY, MAXIMUM\_PROTECTION, ESSENTIAL\_PROTECTION ]	|
|thirdPartyTargets.doubleVerifyViewability	|struct	|Supported InventoryTypes['STANDARD\_DISPLAY', 'AAP\_MOBILE\_APP', 'VIDEO'].	|
|thirdPartyTargets.doubleVerifyViewability.averageCompletionAndFullyViewableRateTargeting	|enum	|The type of average completion and fully viewable rate targeting. [ ALLOW\_ALL, AVG\_COMPLETION\_FULLY\_VIEWABLE\_GTE\_10, AVG\_COMPLETION\_FULLY\_VIEWABLE\_GTE\_20, AVG\_COMPLETION\_FULLY\_VIEWABLE\_GTE\_25, AVG\_COMPLETION\_FULLY\_VIEWABLE\_GTE\_30, AVG\_COMPLETION\_FULLY\_VIEWABLE\_GTE\_35, AVG\_COMPLETION\_FULLY\_VIEWABLE\_GTE\_40 ]	|
|thirdPartyTargets.doubleVerifyViewability.brandExposureViewabilityTargeting	|enum	|The type of brand exposure viewability targeting. [ ALLOW\_ALL, BRAND\_EXPOSURE\_VIEWABILITY\_GTE\_15\_SEC\_AVG\_DURATION, BRAND\_EXPOSURE\_VIEWABILITY\_GTE\_10\_SEC\_AVG\_DURATION, BRAND\_EXPOSURE\_VIEWABILITY\_GTE\_5\_SEC\_AVG\_DURATION ]	|
|thirdPartyTargets.doubleVerifyViewability.mrcViewabilityTargeting	|enum	|The type of MRC viewability targeting. [ ALLOW\_ALL, MRC\_VIEWABILITY\_GTE\_80, MRC\_VIEWABILITY\_GTE\_75, MRC\_VIEWABILITY\_GTE\_70, MRC\_VIEWABILITY\_GTE\_65, MRC\_VIEWABILITY\_GTE\_60, MRC\_VIEWABILITY\_GTE\_55, MRC\_VIEWABILITY\_GTE\_50, MRC\_VIEWABILITY\_GTE\_40, MRC\_VIEWABILITY\_GTE\_30 ]	|
|thirdPartyTargets.doubleVerifyViewability.includeUnmeasurableImpressions	|boolean	|If set to true, include impressions where impressions can't be measured.	|
|thirdPartyTargets.integralAdScienceViewability	|struct	|Supported InventoryTypes['STANDARD\_DISPLAY', 'VIDEO']. The IAS viewability standard.	|
|thirdPartyTargets.integralAdScienceViewability.standard	|enum	|The viewability standard. [ NONE, MRC, GROUPM, PUBLICIS ]	|
|thirdPartyTargets.integralAdScienceViewability.viewabilityTargeting	|enum	|The type of viewability tier. [ ALLOW\_ALL, VIEWABILITY\_TIER\_GT\_70, VIEWABILITY\_TIER\_GT\_60, VIEWABILITY\_TIER\_GT\_50, VIEWABILITY\_TIER\_GT\_40, VIEWABILITY\_TIER\_LT\_40 ]	|
|thirdPartyTargets.oracleDataCloudViewability	|struct	|Supported InventoryTypes['STANDARD\_DISPLAY', 'AAP\_MOBILE\_APP', 'VIDEO']. The ODC viewability standard.	|
|thirdPartyTargets.oracleDataCloudViewability.standard	|enum	|The viewability standard. [ NONE, MRC ]	|
|thirdPartyTargets.oracleDataCloudViewability.viewabilityTargeting	|enum	|The type of ODC MRC viewability tier. [ VIEWABILITY\_TIER\_GT\_80, VIEWABILITY\_TIER\_GT\_70, VIEWABILITY\_TIER\_GT\_60, VIEWABILITY\_TIER\_GT\_50, VIEWABILITY\_TIER\_GT\_40, VIEWABILITY\_TIER\_GT\_30, VIEWABILITY\_TIER\_GT\_20 ]	|
|thirdPartyTargets.doubleVerifyAuthenticBrandSafety	|struct	|Supported InventoryTypes['STANDARD\_DISPLAY', 'AAP\_MOBILE\_APP', 'VIDEO'].	|
|thirdPartyTargets.doubleVerifyAuthenticBrandSafety.doubleVerifySegmentId	|string	|The segment identifier. *Pattern: `1^51[0-9]{6}$*1`	|
|thirdPartyTargets.doubleVerifyCustomContextualSegmentId	|struct	|Supported InventoryTypes['STANDARD\_DISPLAY', 'AAP\_MOBILE\_APP', 'VIDEO'].	|
|thirdPartyTargets.doubleVerifyCustomContextualSegmentId.segmentId	|string	|The custom segment identifier. *Pattern: `1^52[0-9]{6}$*1`	|
|thirdPartyTargets.oracleDataCloudCustomSegmentId	|struct	|Supported InventoryTypes['STANDARD\_DISPLAY', 'AAP\_MOBILE\_APP', 'VIDEO'].	|
|thirdPartyTargets.oracleDataCloudCustomSegmentId.segmentId	|string	|The custom segment identifier. *Pattern:* `^(255257)[0-9]{5}$`	|
|thirdPartyTargets.oracleDataCloudContextualPredictsSegmentId	|struct	|Supported InventoryTypes['STANDARD\_DISPLAY', 'AAP\_MOBILE\_APP', 'VIDEO'].	|
|thirdPartyTargets.oracleDataCloudContextualPredictsSegmentId.segmentId	|string	|The custom segment predict identifier. *Pattern: `^259[0-9]{5}$*`|
|thirdPartyTargets.oracleDataCloudStandardPredictsSegmentIds	|struct	|Supported InventoryTypes['STANDARD\_DISPLAY', 'AAP\_MOBILE\_APP', 'VIDEO'].	|
|thirdPartyTargets.oracleDataCloudStandardPredictsSegmentIds.segmentIds	|array	|The standard predict segment identifiers. 	|

#### Sample payload

```json
{
  "dataset_id": "adsp-adgroup-targets",
  "advertiser_id": "",
  "marketplace_id": "",
  "version": 0,
  "dspEntityId": "",
  "campaignId": "",
  "adgroupId": "",
  "creationDateTime": "",
  "lastUpdatedDateTime": "",
  "thirdPartyTargets": [
    {
      "doubleVerifyFraudInvalidTraffic": {
        "excludeImpressions": true,
        "excludeAppsAndSites": "ALLOW_ALL",
        "blockAppAndSites": true
      }
    },
    {
      "integralAdScienceFraudInvalidTraffic": {
        "targetSetting": "ALLOW_ALL"
      }
    },
    {
      "oracleDataCloudFraudInvalidTraffic": {
        "targetSetting": "ALLOW_ALL"
      }
    },
    {
      "pixalateFraudInvalidTraffic": {
        "excludeIpAddressAndUserAgents": true,
        "excludeOttAndMobileDevices": true,
        "excludeRemovedAppsFromAppStores": true,
        "excludeAppsAndDomains": true
      }
    },
    {
      "doubleVerifyStandardDisplayBrandSafety": {
        "contentCategories": ["AD_SERVER"],
        "contentCategoriesWithRisk": {
          "additionalProp1": "ALLOW_ALL",
          "additionalProp2": "ALLOW_ALL",
          "additionalProp3": "ALLOW_ALL"
        },
        "unknownContent": true
      }
    },
    {
      "doubleVerifyBrandSafety": {
        "appStarRating": "ALLOW_ALL",
        "contentCategories": ["AD_SERVER"],
        "appAgeRating": ["EVERYONE_4_PLUS"],
        "excludeAppsWithInsufficientRating": true,
        "contentCategoriesWithRisk": {
          "additionalProp1": "ALLOW_ALL",
          "additionalProp2": "ALLOW_ALL",
          "additionalProp3": "ALLOW_ALL"
        },
        "unknownContent": true
      }
    },
    {
      "integralAdScienceBrandSafety": {
        "iasBrandSafetyOffensiveLanguage": "ALLOW_ALL",
        "excludeContent": true,
        "iasBrandSafetyAlcohol": "ALLOW_ALL",
        "iasBrandSafetyIllegalDownloads": "ALLOW_ALL",
        "iasBrandSafetyHateSpeech": "ALLOW_ALL",
        "iasBrandSafetyIllegalDrugs": "ALLOW_ALL",
        "iasBrandSafetyAdult": "ALLOW_ALL",
        "iasBrandSafetyViolence": "ALLOW_ALL",
        "iasBrandSafetyGambling": "ALLOW_ALL"
      }
    },
    {
      "oracleDataCloudBrandSafety": {
        "essentialProtection": "ADULT",
        "targetingOption": "NO_BRAND_SAFETY"
      }
    },
    {
      "doubleVerifyViewability": {
        "averageCompletionAndFullyViewableRateTargeting": "ALLOW_ALL",
        "brandExposureViewabilityTargeting": "ALLOW_ALL",
        "mrcViewabilityTargeting": "ALLOW_ALL",
        "includeUnmeasurableImpressions": true
      }
    },
    {
      "integralAdScienceViewability": {
        "standard": "NONE",
        "viewabilityTargeting": "ALLOW_ALL"
      }
    },
    {
      "oracleDataCloudViewability": {
        "standard": "NONE",
        "viewabilityTargeting": "VIEWABILITY_TIER_GT_80"
      }
    },
    {
      "doubleVerifyAuthenticBrandSafety": {
        "doubleVerifySegmentId": "string"
      }
    },
    {
      "doubleVerifyCustomContextualSegmentId": {
        "segmentId": "string"
      }
    },
    {
      "oracleDataCloudCustomSegmentId": {
        "segmentId": "string"
      }
    },
    {
      "oracleDataCloudContextualPredictsSegmentId": {
        "segmentId": "string"
      }
    },
    {
      "oracleDataCloudStandardPredictsSegmentIds": {
        "segmentIds": ["string"]
      }
    }
  ]
}
```

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
          "aws:SourceArn": "arn:aws:sns:us-east-1:419834811630:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:764057072099:*"
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:178122609971:*"
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

You will receive a notification in near real time for every change in the configuration of your target.
