---
title: Sponsored ads campaign management
description: Details and schema for the campaigns, ad groups, ads, and targets datasets.
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Campaign management
    - Sponsored Display
---

# Sponsored ads campaign management

These datasets help advertisers understand changes that have been made to sponsored ads campaigns, ad groups, ads, and targets.

>[NOTE] To subscribe or manage subscriptions to these datasets, use the [/streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription) endpoints.

## Campaigns dataset (beta)

The campaigns dataset provides a snapshot of all your campaign objects--including Sponsored Products, Sponsored Brands, and Sponsored Display. 

### Dataset ID

To subscribe to this dataset, use the ID `campaigns`.

### Schema

|Field	|Type	|Description	|
|-----|----|-------
|dataset_id	|string	|An identifier used to identify the dataset (in this case, `campaigns`).	|
|advertiser_id	|string	|Unique identifier of the advertiser (standard in Stream datasets).	|
|marketplace_id	|string	|The marketplace of the advertiser.	|
|campaignId	|string	|Unique identifier of campaign.	|
|accountId	|string	|Unique identifier of advertiser.	Also referred to as `profileId` in the API. |
|portfolioId	|string	|Unique identifier of portfolio, if the campaign belongs to one.	|
|adProduct	|string	|Product/program type of campaign. One of: SPONSORED\_PRODUCTS, SPONSORED\_BRANDS, SPONSORED\_DISPLAY.	|
|productLocation	|string	|The product location of the campaign. One of: SOLD\_ON\_AMAZON (For products sold on Amazon websites), NOT\_SOLD\_ON\_AMAZON (For products not sold on Amazon websites), SOLD\_ON\_DTC (Deprecated - For products sold on DTC websites).	|
|version | long | This numeric value is incremented when changes are made to the object, representing a chronological history of changes. For example, the creation event would be the lowest version, while a subsequent update would be greater. | 
|name	|string	|Name of the campaign (must be unique for an account).	|
|startDateTime	|datetime	|Effective from date time of campaign.	|
|endDateTime	|datetime	|Effective through date time of campaign.	|
|state	|string	|The advertiser defined state falling into one of these values: <br> Sponsored Products: ENABLED, PAUSED, ARCHIVED, USER_DELETED, ENABLING, OTHER<br><br> Sponsored Brands/Sponsored Display: ENABLED, PAUSED, ARCHIVED	|
|tags	|map<string, string>	|User defined key value pairs.	|
|targetingSettings	|string	|Targeting type of the campaign, today is used to indicate auto or manual.	|
|budget.budgetCap.monetaryBudget.amount	|double	|The budget in amount allocated to this budgetCap.	|
|budget.budgetCap.monetaryBudget.currencyCode	|string	|ISO 4217 currency code.	|
|budget.budgetCap.recurrence.recurrenceType	|string	|Whether the budget renews weekly, monthly, or lifetime e.g., DAILY, LIFETIME.	|
|bidSetting.bidStrategy	|string	|Bid strategy options. For Sponsored Products, one of: LEGACY\_FOR\_SALES, AUTO\_FOR\_SALES, MANUAL, RULE\_BASED	|
|bidSetting.placementBidAdjustment |array |The bid adjustments for placements.  |
|bidSetting.placementBidAdjustment.placement	|string	|Bid strategy placement modifier<br><br> Sponsored Brands: HOME, DETAIL\_PAGE, OTHER<br>Sponsored Products: PLACEMENT\_TOP, PLACEMENT\_PRODUCT\_PAGE, PLACEMENT\_REST\_OF\_SEARCH, SITE\_AMAZON\_BUSINESS	|
|bidSetting.placementBidAdjustment.percentage	|double	|Percentage of bid adjustment.  |
|bidSetting.shopperCohortBidAdjustment |array |The bid adjustments for shopper cohorts.  |
|bidSetting.shopperCohortBidAdjustment.percentage |double	|The selection of the percentage change associated with a given shopper cohort and bid adjustment settings.	|
|bidSetting.shopperCohortBidAdjustment.shopperCohortType |string | The shopper cohort type required to specify the type of shopper cohort used to apply bid adjustments. Example, an Audience Segment. AUDIENCE\_SEGMENT|
|bidSetting.shopperCohortBidAdjustment.audienceSegments |array  |The audience segments associated with a bid adjustment for a given shopper cohort. |
|bidSetting.shopperCohortBidAdjustment.audienceSegments.audienceSegmentId |string	|This is an Audience identifier vended by Audience Discovery. |
|bidSetting.shopperCohortBidAdjustment.audienceSegments.audienceSegmentType |string	|The AudienceSegmentType field is optional as the type is automatically determined from the Audience path using the ListTargetableEntities API. Audience IDs are guaranteed to be unique across all audience types, enabling this inference. Supported types: DSP\_AMC (Deprecated), SPONSORED\_ADS\_AMC (AMC Audiences), BEHAVIOR\_DYNAMIC (Amazon Built Audiences for SP and SB) |
|audit.creationDateTime	|datetime	|Creation time stamp in ISO8601 format.	|
|audit.lastUpdatedDateTime	|datetime	|Last time record was updated timestamp in ISO8601 format.	|


### Sample payload

```json
{
  "dataset_id": "campaigns",
  "advertiser_id": "",
  "marketplace_id": "",
  "version": 0,
  "campaignId": "",
  "advertiser_id": "",
  "portfolioId": "",
  "adProduct": "",
  "name": "",
  "startDateTime": "",
  "endDateTime": "",
  "state": "",
  "tags": {
    "": ""
  },
  "targetingSettings": "",
  "audit": {
    "creationDateTime": "",
    "lastUpdatedDateTime": ""
  },
  "budget": {
    "budgetCap": {
      "monetaryBudget": {
        "amount": 0,
        "currencyCode": ""
      },
      "recurrence": {
        "recurrenceType": ""
      }
    },
    "bidSetting": {
      "bidStrategy": "",
      "placementBidAdjustment": [{
        "placement": "",
        "percentage": 0
      }],
      "shopperCohortBidAdjustment": [{
        "shopperCohortType": "",
        "percentage": 0,
        "audienceSegments": [{
          "audienceSegmentId": "",
          "audienceSegmentType": ""
        }]
      }]
    }
  }
}

```

### Resource-based IAM policy

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
          "aws:SourceArn": "arn:aws:sns:us-east-1:570159413969:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:834862128520:*"
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
FE campaigns
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:527383333093:*"
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

### Data freshness

You will receive a notification in near real time for every change in the configuration of your campaign.

## Ad groups dataset (beta)

The ad groups dataset provides a snapshot of all your ad group objects--including those related to Sponsored Products, Sponsored Brands, and Sponsored Display campaigns. 

### Dataset ID 

To subscribe to this dataset, use the ID `adgroups`.

### Schema

|Field	|Type	|Description	|
|----|----|----|
|dataset_id	|string	|An identifier used to identify the dataset (in this case, `adgroups`.)	|
|advertiser_id	|string	|Unique identifier of the advertiser (standard in Stream datasets).	|
|marketplace_id	|string	|The marketplace of the advertiser.	|
|adGroupId	|string	|Unique identifier of ad group.	|
|name	|string	|Name of the ad group.	|
|version | long | This numeric value is incremented when changes are made to the object, representing a chronological history of changes. For example, the creation event would be the lowest version, while a subsequent update would be greater. | 
|accountId	|string	|Unique identifier of advertiser.	Also referred to as `profileId` in the API. |
|campaignId	|string	|Unique identifier of parent campaign object.	|
|adProduct	|string	|Product/program type of campaign. One of: SPONSORED\_PRODUCTS, SPONSORED\_BRANDS, SPONSORED\_DISPLAY	|
|bidValue.defaultBid.value	|double	|Value of the bid in the currency specified.	|
|bidValue.defaultBid.currencyCode	|string	|ISO4217 currency code.	|
|state	|string	|The advertiser defined state falling into one of these values: <br><br>Sponsored Products: ENABLED, PAUSED, ARCHIVED, USER\_DELETED, ENABLING, OTHER<br>Sponsored Brands/Sponsored Display: ENABLED, PAUSED, ARCHIVED	|
|audit.creationDateTime	|datetime	|Creation time stamp in ISO8601 format.	|
|audit.lastUpdatedDateTime	|datetime	|Last time record was updated timestamp in ISO8601 format.	|


### Sample payload

```json
{
  "dataset_id": "adgroups",
  "advertiser_id": "",
  "version": 0,
  "marketplace_id": "",
  "adGroupId": "",
  "name": "",
  "accountId": "",
  "campaignId": "",
  "adProduct": "",
  "bidValue": {
    "defaultBid": {
      "value": 0.0,
      "currencyCode": ""
    }
  },
  "state": "",
  "audit": {
    "creationDateTime": "",
    "lastUpdatedDateTime": ""
  }
}
```

### Resource-based IAM policy

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
          "aws:SourceArn": "arn:aws:sns:us-east-1:118846437111:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:130948361130:*"
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:668585072850:*"
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

### Data freshness

You will receive a notification in near real time for every change in the configuration of an ad group.

## Ads dataset (beta)

The ads dataset provides a snapshot of all your ad objects--including those related to Sponsored Products, Sponsored Brands, and Sponsored Display campaigns. 

### Dataset ID

To subscribe to this dataset, use `ads` as the ID.

### Schema

|Field	|Type	|Description	|
|----|----|----|
|dataset_id	|string	|An identifier used to identify the dataset (in this case, `ads`).	|
|advertiser_id	|string	|Unique identifier of the advertiser (standard in Stream datasets).	|
|marketplace_id	|string	|The marketplace of the advertiser.	|
|adId	|string	|Unique identifier of ad.	|
|name	|string	|Name of ad, advertiser description.	|
|version | long | This numeric value is incremented when changes are made to the object, representing a chronological history of changes. For example, the creation event would be the lowest version, while a subsequent update would be greater. | 
|accountId	|string	|Unique identifier of advertiser.	Also referred to as `profileId` in the API. |
|adGroupId	|string	|Unique identifier of parent record (ad group ID).	|
|campaignId	|string	|Unique identifier of campaign.	|
|adProduct	|string	|Product/program type of campaign. One of: SPONSORED\_PRODUCTS, SPONSORED\_BRANDS, SPONSORED\_DISPLAY	|
|customText	|string	|Custom text for KDP accounts. Sponsored Products only.	|
|asin	|string	|The ASIN of the product advertised by the product ad. Relevant for Sponsored Products and Sponsored Display vendors and KDP. 	|
|sku	|string	|The SKU of the product advertised by the product ad. Relevant only for sellers using Sponsored Products and Sponsored Display.	|
|landingPage.url	|string	|Landing page URL for Sponsored Brands and Sponsored Display campaigns.	|
|landingPage.type	|string	|Type of landing page. <br><br>Sponsored Brands: PRODUCT\_LIST, STORE, CUSTOM\_URL, DETAIL\_PAGE<br>Sponsored Display: STORE, MOMENT, OFF\_AMAZON\_LINK	|
|state	|string	|The advertiser defined state.<br>Sponsored Products: ENABLED, PAUSED, ARCHIVED, USER\_DELETED, ENABLING, OTHER<br>Sponsored Brands/Sponsored Display: ENABLED, PAUSED, ARCHIVED	|
|audit.creationDateTime	|datetime	|Creation time stamp in ISO8601 format.	|
|audit.lastUpdatedDateTime	|datetime	|Last time record was updated timestamp in ISO8601 format.	|


### Sample payload

```json
{
  "dataset_id": "ads",
  "advertiser_id": "",
  "marketplace_id": "",
  "adId": "",
  "name": "",
  "version": 0,
  "accountId": "",
  "adGroupId": "",
  "campaignId": "",
  "adProduct": "",
  "customText": "",
  "asin": "",
  "sku": "",
  "landingPage": {
    "url": "",
    "type": ""
  },
  "state": "",
  "audit": {
    "creationDateTime": "",
    "lastUpdatedDateTime": ""
  }
}
```

### Resource-based IAM policy

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
          "aws:SourceArn": "arn:aws:sns:us-east-1:305370293182:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:648558082147:*"
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:802070757281:*"
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

### Data freshness

You will receive a notification in near real time for every change in the configuration of an ad.

## Targets dataset (beta)

The targets dataset provides a snapshot of all your targeting and keyword objects--including those related to Sponsored Products, Sponsored Brands, and Sponsored Display campaigns. 

### Dataset ID

To subscribe to this dataset, use the `targets` ID.

### Schema

|Field	|Type	|Description	|
|---|---|---|
|dataset_id	|string	|An identifier used to identify the dataset (in this case `targets`).	|
|advertiser_id	|string	|Unique identifier of the advertiser (standard in Stream datasets).	|
|marketplace_id	|string	|The marketplace of the advertiser	|
|targetId	|string	|Unique identifier of target.	|
|accountId	|string	|Unique identifier of advertiser.	Also referred to as `profileId` in the API.  |
|adGroupId	|string	|Unique identifier of the ad group.	|
|campaignId	|string	|Unique identifier of the campaign.	|
|version | long | This numeric value is incremented when changes are made to the object, representing a chronological history of changes. For example, the creation event would be the lowest version, while a subsequent update would be greater. | 
|adProduct	|string	|Product/program type of campaign. One of: SPONSORED\_PRODUCTS, SPONSORED\_BRANDS, SPONSORED\_DISPLAY	|
|state	|string	|The advertiser defined state falling into one of these values:<br>Sponsored Products: ENABLED, PAUSED, ARCHIVED, USER\_DELETED, ENABLING, OTHER<br>Sponsored Brands/Sponsored Display: ENABLED, PAUSED, ARCHIVED	|
|targetType	|string	|The type of targeting e.g. KEYWORD, AUDIENCE, EXPRESSION, AUTO (EXPRESSION\_PREDEFINED), WEBSITE, APP, ASIN_EXPAND	|
|negative	|boolean	|Negative or positive targeting on the expression.	|
|bid	|double	|The bid used for auction. A null bid is valid, and means the target is inheriting from the ad group default bid.	|
|currencyCode	|string	|ISO4217 currency code.	|
|{targetType}.matchType	|string	|Matching type depending on target type.<br>For keywords: EXACT, PHRASE, BROAD (positive only)<br>For expressions: ASIN, CATEGORY, BRAND (negative only), DYNAMIC\_SEGMENTS (Sponsored Display only).<br>For audiences: VIEWS\_REMARKETING, PURCHASE\_REMARKETING, AMAZON\_AUDIENCES	|
|{targetType}.targetingClause	|string	|Targeting expression string.	|
|keywordTarget.keyword	|string	|Keyword value to be used for targeting.	|
|keywordTarget.nativeLanguageKeyword	|string	|Keyword value language.	|
|keywordTarget.nativeLanguageLocale	|string	|Locale of language for keyword.	|
|audit.creationDateTime	|datetime	|Creation time stamp in ISO8601 format.	|
|audit.lastUpdatedDateTime	|datetime	|Last time record was updated timestamp in ISO8601 format.	|


### Sample payload

```json
{
  "dataset_id": "targets",
  "advertiser_id": "",
  "marketplace_id": "",
  "targetId": "",
  "accountId": "",
  "version": 0,
  "adGroupId": "",
  "campaignId": "",
  "adProduct": "",
  "targetType": "",
  "negative": true,
  "bid": 0.0,
  "currencyCode": "",
  "keywordTarget": {
    "matchType": "",
    "keyword": "",
    "nativeLanguageKeyword": "",
    "nativeLanguageLocale": ""
  },
  "productTarget": {
    "matchType": "",
    "targetingClause": ""
  },
  "productAudienceTarget": {
    "matchType": "",
    "targetingClause": ""
  },
  "productCategoryTarget": {
    "matchType": "",
    "targetingClause": ""
  },
  "productCategoryAudienceTarget": {
    "matchType": "",
    "targetingClause": ""
  },
  "audienceTarget": {
    "matchType": "",
    "targetingClause": ""
  },
  "autoTarget": {
    "matchType": "",
    "targetingClause": ""
  },
  "state": "",
  "audit": {
    "creationDateTime": "",
    "lastUpdatedDateTime": ""
  }
}
```

### Resource-based IAM policy

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
          "aws:SourceArn": "arn:aws:sns:us-east-1:644124924521:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:503759481754:*"
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:248074939493:*"
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

### Data freshness

You will receive a notification in near real time for every change in the configuration of a keyword or 