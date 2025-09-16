---
title: Amazon DSP performance datasets
description: Details and schema for the Amazon DSP performance datasets.
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Campaign management
    - Amazon DSP
---

# Amazon DSP performance datasets

The Amazon DSP performance datasets provide details about traffic, conversions, clickstream, and rich media for Amazon DSP campaigns.

To subscribe or manage subscriptions to these datasets, use the [/dsp/streams/subscriptions](openapi#tag/Stream-Subscription) endpoints.

>[TIP:Note about data freshness] The Amazon Marketing Stream DSP performance dataset follows [the same SLA as Amazon DSP reports](https://advertising.amazon.com/help/G2QE6RKPFG6C4KBH). Also note that compared to the Amazon DSP reporting API, Amazon Marketing Stream delivers interaction data and processes adjustments more quickly. When comparing data between Stream and API reports, we suggest waiting **five days** after the event has occurred.

>[TIP:Note about marketplace\_id] `marketplace_id` should only be used in combination with `advertiser_id` to uniquely identify an Amazon DSP advertiser. Do not use `marketplace_id` to determine locale information. Instead, refer to dedicated locale fields provided in the dataset.

>[TIP:Note About Locale Indicator] There's currently **two** locale indicator fields: `advertiser_country` and `country`. 
`advertiser_country`: The country assigned to Amazon DSP advertiser. 
`country`: The country where Amazon DSP ads are served.

## Amazon DSP traffic dataset

The adsp-traffic dataset contains click, impression, and cost data related to Amazon DSP campaigns.

### Dataset ID

To subscribe to this dataset, use the ID `adsp-traffic`.

### Schema

This dataset supports the following fields and metrics:

|Field name	|API name	|Type	|Type	|Description	|
|---	|---	|---	|---	|---	|
|dataset_id	|	N/A|String	|Dimension	|An identifier used to identify the dataset	|
|idempotency_id	|N/A	|String	|Dimension	|An identifier that can be used to de-duplicate records	|
|time\_window\_start	|N/A	|String	|Dimension	|The start of the hour at which the performance data is attributed (ISO 8601 date time). Time zone for each advertiser is indicated in the [advertiser profile](reference/2/profiles#/Profiles/listProfiles).	|
|adgroup_id	|lineItemId	|String	|Dimension	|A unique identifier assigned to a line item or ad group	|
|advertiser_country	|advertiserCountry	|String	|Dimension	|The country assigned to the advertiser	<br/> **Note:** This field can be null if the advertiser is not tied to a specific country|
|advertiser_id	|advertiserId	|String	|Dimension	|The unique identifier for the advertiser	|
|advertiser_name	|advertiserName	|String	|Dimension	|The customer that has an advertising relationship with Amazon	|
|advertiser_timezone	|advertiserTimezone	|String	|Dimension	|The time zone the advertiser uses for reporting and ad serving purposes	|
|country	|country	|String	|Dimension	|Country or locale	|
|creative_id	|creativeId	|String	|Dimension	|A unique identifier assigned to a creative. When this is different from creativeAdId, creativeId cannot be used in our /dsp/creatives endpoints	|
|creative_language	|creativeLanguage	|String	|Dimension	|The primary language in the creative	|
|creative_name	|creativeName	|String	|Dimension	|Creative name	|
|creative_size	|creativeSize	|String	|Dimension	|The dimensions of the creative in pixels	|
|creative_type	|creativeType	|String	|Dimension	|The type of creative (for example static image, third party, or video)	|
|deal	|deal	|String	|Dimension	|The name of a deal	|
|deal_id	|dealID	|String	|Dimension	|The unique identifier for the deal	|
|deal_type	|dealType	|String	|Dimension	|The type of deal	|
|device	|device	|String	|Dimension	|The devices the customers used to view the ad	|
|dsp\_entity\_id	|entityId	|String	|Dimension	|Entity (seat) ID	|
|placement_name	|placementName	|String	|Dimension	|The name of the placement where the campaign ran	|
|proposal_id	|proposalId	|String	|Dimension	|A unique identifier for an order imported from the Amazon Order Management System (OMS)	|
|region	|region	|String	|Dimension	|State or region	|
|site	|siteName	|String	|Dimension	|The site or group of sites the campaign ran on	|
|supply\_source\_name	|supplySourceName	|String	|Dimension	|The inventory the campaign ran on, for example real-time bidding exchanges or Amazon-owned sites	|
|marketplace_id	|N/A	|String	|Dimension	|The marketplace of the advertiser|
|campaign_id	|orderId	|String	|Dimension	|The ID associated with a campaign	|
|flight_id	|N/A	|String	|Dimension	|Unique identifier of the campaign flight	|
|agency_fee	|agencyFee	|Currency	|Metric	|A percentage or flat fee removed from the total budget to compensate the agency that is managing the media buy	|
|amazon\_dsp\_audience_fee	|amazonAudienceFee	|Currency	|Metric	|CPM charge applied to impressions that leverage Amazon's behavioral targeting	|
|amazon\_dsp\_console\_fee	|amazonPlatformFee	|Currency	|Metric	|The technology fee applied to the media supply costs	|
|supply_cost	|supplyCost	|Currency	|Metric	|The total amount of money spent on media supply	|
|total_cost	|totalCost	|Currency	|Metric	|The total amount of money spent on running the campaign not including third-party fees paid by the agency	|
|gross_impressions	|grossImpressions	|Integer	|Metric	|The total number of times the ad was displayed. This includes valid and invalid impressions, such as potentially fraudulent, non-human, and other illegitimate impressions.	|
|impressions	|impressions	|Integer	|Metric	|Total number of ad impressions	|
|invalid_impressions	|invalidImpressions	|Integer	|Metric	|The number of impressions removed by a traffic quality filter. This includes potentially fraudulent, non-human, and other illegitimate traffic	|
|measurable_impressions	|measurableImpressions	|Integer	|Metric	|Number of impressions that were measured for viewability	|
|viewable_impressions	|viewableImpressions	|Integer	|Metric	|Number of impressions that met the Media Ratings Council's (MRC) viewability standard	|
|clicks	|clickThroughs	|Integer	|Metric	|Total number of clicks on an ad	|

### Sample Payload

```
{
    "dataset_id": "adsp-traffic",
    "idempotency_id": "string",
    "time_window_start": "string",
    "adgroup_id": "string",
    "advertiser_country": "string",
    "advertiser_id": "string",
    "advertiser_name": "string",
    "advertiser_timezone": "string",
    "country": "string",
    "creative_id": "string",
    "creative_language": "string",
    "creative_name": "string",
    "creative_size": "string",
    "creative_type": "string",
    "deal": "string",
    "deal_id": "string",
    "deal_type": "string",
    "device": "string",
    "dsp_entity_id": "string",
    "placement_name": "string",
    "proposal_id": "string",
    "region": "string",
    "site": "string",
    "supply_source_name": "string",
    "marketplace_id": "string",
    "campaign_id": "string",
    "flight_id": "string",
    "agency_fee": 0.0,
    "amazon_dsp_audience_fee": 0.0,
    "amazon_dsp_console_fee": 0.0,
    "supply_cost": 0.0,
    "total_cost": 0.0,
    "gross_impressions": 0,
    "impressions": 0,
    "invalid_impressions": 0,
    "measurable_impressions": 0,
    "viewable_impressions": 0,
    "clicks": 0
}
```

### Resource-based IAM policy

To subscribe to this dataset, ensure the correct IAM policy is added to the SQS queue based on the marketplace.

**NA**

```
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
          "aws:SourceArn": "arn:aws:sns:us-east-1:192826442293:*"
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

```
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:738976022360:*"
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

```
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:115810253223:*"
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

## Amazon DSP conversion dataset

The adsp-conversion dataset contains conversion data attributed to Amazon DSP campaigns.

### Dataset ID

To subscribe to this dataset, use the ID `adsp-conversion`.

### Schema

This dataset supports the following fields and metrics:

|Field name	|API name	|Type	|Type	|Description	|
|---	|---	|---	|---	|---	|
|dataset_id	|N/A	|String	|Dimension	|An identifier used to identify the dataset	|
|idempotency_id	|N/A	|String	|Dimension	|An identifier that can be used to de-duplicate records	|
|time\_window\_start	|N/A	|String	|Dimension	|The start of the hour at which the performance data is attributed (ISO 8601 date time). Time zone for each advertiser is indicated in the [advertiser profile](reference/2/profiles#/Profiles/listProfiles).	|
|adgroup_id	|lineItemId	|String	|Dimension	|A unique identifier assigned to a line item or ad group	|
|advertiser_country	|advertiserCountry	|String	|Dimension	|The country assigned to the advertiser	<br/> **Note:** This field can be null if the advertiser is not tied to a specific country|
|advertiser_id	|advertiserId	|String	|Dimension	|The unique identifier for the advertiser	|
|advertiser_name	|advertiserName	|String	|Dimension	|The customer that has an advertising relationship with Amazon	|
|advertiser_timezone	|advertiserTimezone	|String	|Dimension	|The time zone the advertiser uses for reporting and ad serving purposes	|
|country	|country	|String	|Dimension	|Country or locale	|
|creative_id	|creativeId	|String	|Dimension	|A unique identifier assigned to a creative. When this is different from creativeAdId, creativeId cannot be used in our /dsp/creatives endpoints.	|
|creative_language	|creativeLanguage	|String	|Dimension	|The primary language in the creative	|
|creative_name	|creativeName	|String	|Dimension	|Creative name	|
|creative_size	|creativeSize	|String	|Dimension	|The dimensions of the creative in pixels	|
|creative_type	|creativeType	|String	|Dimension	|The type of creative (for example static image, third party, or video)	|
|deal	|deal	|String	|Dimension	|The name of a deal	|
|deal_id	|dealID	|String	|Dimension	|The unique identifier for the deal	|
|deal_type	|dealType	|String	|Dimension	|The type of deal	|
|device	|device	|String	|Dimension	|The devices the customers used to view the ad	|
|dsp\_entity\_id	|entityId	|String	|Dimension	|Entity (seat) ID	|
|placement_name	|placementName	|String	|Dimension	|The name of the placement where the campaign ran	|
|proposal_id	|proposalId	|String	|Dimension	|A unique identifier for an order imported from the Amazon Order Management System (OMS)	|
|region	|region	|String	|Dimension	|State or region	|
|site	|siteName	|String	|Dimension	|The site or group of sites the campaign ran on	|
|supply\_source\_name	|supplySourceName	|String	|Dimension	|The inventory the campaign ran on--for example real-time bidding exchanges or Amazon-owned sites	|
|marketplace_id	|N/A	|String	|Dimension	|The marketplace of the advertiser.|
|campaign_id	| sorderId	|String	|Dimension	|The ID associated with a campaign	|
|units\_sold	|unitsSold	|Integer	|Metric	|Number of attributed units sold within 14 days of an ad click or view	|
|new\_to\_brand\_units\_sold	|newToBrandUnitsSold	|Integer	|Metric	|Total number of attributed units ordered as part of new-to-brand sales occurring within 14 days of an ad click or view. Not available for book vendors	|
|total\_units\_sold	|totalUnitsSold	|Integer	|Metric	|The total quantity of promoted products and products from the same brand as promoted products purchased by customers on Amazon after delivering an ad. A campaign can have multiple units sold in a single purchase event.	|
|total\_new\_to\_brand\_units\_sold	|totalNewToBrandUnitsSold	|Integer	|Metric	|The total quantity of promoted products and products from the same brand as promoted products purchased by customers on Amazon after delivering an ad. A campaign can have multiple units sold in a single purchase event.	|
|sales	|sales	|Currency	|Metric	|The total sales (in local currency) of promoted ASINs purchased by customers on Amazon after delivering an ad.	|
|new\_to\_brand\_product\_sales	|newToBrandProductSales	|Currency	|Metric	|Sales (in local currency) of products promoted to new-to-brand shoppers, attributed to an ad view or click. Use total_new_to_brand_product_sales to see all conversions for the brand's products.	|
|total\_sales	|totalSales	|Currency	|Metric	|Sales (in local currency) of the brand's products, attributed to an ad view or click.	|
|total\_new\_to\_brand\_product\_sales	|totalNewToBrandSales	|Currency	|Metric	|Sales (in local currency) of the brands' products purchased by new-to-brand shoppers, attributed to an ad view or click. Shoppers are "new to brand" if they have not purchased from the brand in the last 365 days.	|
|purchases	|purchases	|Integer	|Metric	|Number of attributed conversion events occurring within 14 days of an ad click or view.	|
|purchases\_views	|purchasesViews	|Integer	|Metric	|Number of attributed conversion events occurring within 14 days of an ad view.	|
|purchases\_clicks	|purchasesClicks	|Integer	|Metric	|Number of attributed conversion events occurring within 14 days of an ad click.	|
|new\_to\_brand\_purchases	|newToBrandPurchases	|Integer	|Metric	|The number of first-time orders for brand products over a one-year lookback window resulting from an ad click or view. Not available for book vendors.	|
|new\_to\_brand\_purchases\_views	|newToBrandPurchasesViews	|Integer	|Metric	|The number of new to branch purchases attributed to an ad view.	|
|new\_to\_brand\_purchases\_clicks	|newToBrandPurchasesClicks	|Integer	|Metric	|The number of first-time orders for brand products over a one-year lookback window resulting from an ad click. Not available for book vendors.	|
|total\_purchases	|totalPurchases	|Integer	|Metric	|Number of times any quantity of a brand product was included in a purchase event, attributed to an ad view or click. Purchase events include new Subscribe & Save subscriptions and video rentals.	|
|total\_purchases\_views	|totalPurchasesViews	|Integer	|Metric	|Number of times any quantity of a brand product was included in a purchase event, attributed to an ad view. Purchase events include new Subscribe & Save subscriptions and video rentals.	|
|total\_purchases\_clicks	|totalPurchasesClicks	|Integer	|Metric	|Number of times any quantity of a brand product was included in a purchase event, attributed to an ad click. Purchase events include new Subscribe & Save subscriptions and video rentals.	|
|total\_new\_to\_brand\_purchases	|totalNewToBrandPurchases	|Integer	|Metric	|Number of new-to-brand purchases for the brands' products, attributed to an ad view or click. Shoppers are "new to brand" if they have not purchased from the brand in the last 365 days.	|
|total\_new\_to\_brand\_purchases\_views	|totalNewToBrandPurchasesViews	|Integer	|Metric	|Number of new-to-brand purchases for the brands' products, attributed to an ad view. Shoppers are "new-to-brand" if they have not purchased from the brand in the last 365 days.	|
|total\_new\_to\_brand\_purchases\_clicks	|totalNewToBrandPurchasesClicks	|Integer	|Metric	|Number of new-to-brand purchases for the brands' products, attributed to an ad click. Shoppers are "new-to-brand" if they have not purchased from the brand in the last 365 days.	|
|app\_subscription\_sign\_up\_free\_trial\_clicks	|appSubscriptionSignUpFreeTrialClicks	|Integer	|Metric	|The number of free trial app subscriptions associated with a click on your ad.	|
|app\_subscription\_sign\_up\_free\_trial	|appSubscriptionSignUpFreeTrial	|Integer	|Metric	|The number of free trial app subscriptions associated with a click or view on your ad.	|
|app\_subscription\_sign\_up\_free\_trial\_views	|appSubscriptionSignUpFreeTrialViews	|Integer	|Metric	|The number of free trial app subscriptions associated with ad impressions.	|
|new\_to\_brand\_purchases\_brand\_halo\_clicks	|newToBrandPurchasesBrandHaloClicks	|Integer	|Metric	|Number of new-to-brand purchases for brand halo products, attributed to an ad click. Shoppers are "new to brand" if they have not purchased from the brand in the last 365 days. Use total_new_to_brand_purchases_clicks to see all conversions for the brands' products.	|
|new\_to\_brand\_purchases\_brand\_halo	|newToBrandPurchasesBrandHalo	|Integer	|Metric	|Number of new-to-brand purchases for brand halo products, attributed to an ad view or click. Shoppers are "new to brand" if they have not purchased from the brand in the last 365 days. Use total_new_to_brand_purchases to see all conversions for the brands' products.	|
|new\_to\_brand\_purchases\_brand\_halo\_views	|newToBrandPurchasesBrandHaloViews	|Integer	|Metric	|Number of new-to-brand purchases for brand halo products, attributed to an ad view. Shoppers are "new to brand" if they have not purchased from the brand in the last 365 days. Use total_new_to_brand_purchases_views to see all conversions for the brands' products.	|
|app\_subscription\_sign\_up\_paid\_clicks	|appSubscriptionSignUpPaidClicks	|Integer	|Metric	|The number of paid app subscriptions associated with a click on your ad.	|
|app\_subscription\_sign\_up\_paid	|appSubscriptionSignUpPaid	|Integer	|Metric	|The number of paid app subscriptions associated with a click or view on your ad.	|
|app\_subscription\_sign\_up\_paid\_views	|appSubscriptionSignUpPaidViews	|Integer	|Metric	|The number of paid app subscriptions associated with ad impressions.	|
|purchases\_brand\_halo\_clicks	|purchasesBrandHaloClicks	|Integer	|Metric	|Number of times any quantity of a brand halo product was included in a purchase event, attributed to an ad click. Purchase events include new Subscribe & Save subscriptions and video rentals. Use total_purchases_clicks to see all conversions for the brands' products.	|
|purchases\_brand\_halo	|purchasesBrandHalo	|Integer	|Metric	|Number of times any quantity of a brand halo product was included in a purchase event, attributed to an ad view or click. Purchase events include new Subscribe & Save subscriptions and video rentals. Use total_purchases to see all conversions for the brands' products.	|
|purchases\_brand\_halo\_views	|purchasesBrandHaloViews	|Integer	|Metric	|Number of times any quantity of a brand halo product was included in a purchase event, attributed to an ad view. Purchase events include new Subscribe & Save subscriptions and video rentals. Use total_purchases_views to see all conversions for the brands' products.	|
|app\_subscription\_sign\_up\_clicks	|appSubscriptionSignUpClicks	|Integer	|Metric	|The number of free trial and paid app subscriptions associated with a click on your ad.	|
|app\_subscription\_sign\_up	|appSubscriptionSignUp	|Integer	|Metric	|The number of free trial and paid app subscriptions associated with a click or view on your ad.	|
|app\_subscription\_sign\_up\_views	|appSubscriptionSignUpViews	|Integer	|Metric	|The number of free trial and paid app subscriptions associated with ad impressions.	|
|sales\_clicks	|salesClicks	|Currency	|Metric	|Total value of sales occurring within 14 days of an ad click.	|
|units\_sold\_clicks	|unitsSoldClicks	|Integer	|Metric	|Number of attributed units sold within 14 days of an ad click.	|
|units\_sold\_promoted\_clicks	|unitsSoldPromotedClicks	|Integer	|Metric	|Units of promoted products purchased, attributed to an ad click. A single click-attributed purchase event can include multiple sold units.	|

### Sample payload

```
{
    "dataset_id": "adsp-conversion",
    "idempotency_id": "string",
    "time_window_start": "string",
    "adgroup_id": "string",
    "advertiser_country": "string",
    "advertiser_id": "string",
    "advertiser_name": "string",
    "advertiser_timezone": "string",
    "country": "string",
    "creative_id": "string",
    "creative_language": "string",
    "creative_name": "string",
    "creative_size": "string",
    "creative_type": "string",
    "deal": "string",
    "deal_id": "string",
    "deal_type": "string",
    "device": "string",
    "dsp_entity_id": "string",
    "placement_name": "string",
    "proposal_id": "string",
    "region": "string",
    "site": "string",
    "supply_source_name": "string",
    "marketplace_id": "string",
    "campaign_id": "string",
    "units_sold": 0,
    "new_to_brand_units_sold": 0,
    "total_units_sold": 0,
    "total_new_to_brand_units_sold": 0,
    "sales": 0.0,
    "new_to_brand_product_sales": 0.0,
    "total_sales": 0.0,
    "total_new_to_brand_product_sales": 0.0,
    "purchases": 0,
    "purchases_views": 0,
    "purchases_clicks": 0,
    "new_to_brand_purchases": 0,
    "new_to_brand_purchases_views": 0,
    "new_to_brand_purchases_clicks": 0,
    "total_purchases": 0,
    "total_purchases_views": 0,
    "total_purchases_clicks": 0,
    "total_new_to_brand_purchases": 0,
    "total_new_to_brand_purchases_views": 0,
    "total_new_to_brand_purchases_clicks": 0,
    "app_subscription_sign_up_free_trial_clicks": 0,
    "app_subscription_sign_up_free_trial": 0,
    "app_subscription_sign_up_free_trial_views": 0,
    "new_to_brand_purchases_brand_halo_clicks": 0,
    "new_to_brand_purchases_brand_halo": 0,
    "new_to_brand_purchases_brand_halo_views": 0,
    "app_subscription_sign_up_paid_clicks": 0,
    "app_subscription_sign_up_paid": 0,
    "app_subscription_sign_up_paid_views": 0,
    "purchases_brand_halo_clicks": 0,
    "purchases_brand_halo": 0,
    "purchases_brand_halo_views": 0,
    "app_subscription_sign_up_clicks": 0,
    "app_subscription_sign_up": 0,
    "app_subscription_sign_up_views": 0,
    "sales_clicks": 0.0,
    "units_sold_clicks": 0,
    "units_sold_promoted_clicks": 0
}
```

### Resource-based IAM policy

To subscribe to this dataset, ensure the correct IAM policy is added to the SQS queue based on the marketplace.

**NA**

```
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
          "aws:SourceArn": "arn:aws:sns:us-east-1:561176707666:*"
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

```
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:238549931437:*"
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

```
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:063613587323:*"
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

## Amazon DSP clickstream dataset

The adsp-clickstream dataset contains detail page views and add-to-cart metrics related to Amazon DSP campaigns.

### Dataset ID

To subscribe to this dataset, use the ID `adsp-clickstream`.

### Schema

This dataset supports the following fields and metrics:

|Field name	|API name	|Type	|Type	|Description	|
|---	|---	|---	|---	|---	|
|dataset_id	|N/A	|String	|Dimension	|An identifier used to identify the dataset	|
|idempotency_id	|N/A	|String	|Dimension	|An identifier that can be used to de-duplicate records	|
|time\_window\_start	|	|String	|Dimension	|The start of the hour at which the performance data is attributed (ISO 8601 date time). Time zone for each advertiser is indicated in the [advertiser profile](reference/2/profiles#/Profiles/listProfiles)	|
|adgroup_id	|lineItemId	|String	|Dimension	|A unique identifier assigned to a line item or ad group	|
|advertiser_country	|advertiserCountry	|String	|Dimension	|The country assigned to the advertiser	<br/> **Note:** This field can be null if the advertiser is not tied to a specific country|
|advertiser_id	|advertiserId	|String	|Dimension	|The unique identifier for the advertiser	|
|advertiser_name	|advertiserName	|String	|Dimension	|The customer that has an advertising relationship with Amazon	|
|advertiser_timezone	|advertiserTimezone	|String	|Dimension	|The time zone the advertiser uses for reporting and ad serving purposes	|
|country	|country	|String	|Dimension	|Country or locale	|
|creative_id	|creativeId	|String	|Dimension	|A unique identifier assigned to a creative. When this is different from creativeAdId, creativeId cannot be used in our /dsp/creatives endpoints.	|
|creative_language	|creativeLanguage	|String	|Dimension	|The primary language in the creative	|
|creative_name	|creativeName	|String	|Dimension	|Creative name	|
|creative_size	|creativeSize	|String	|Dimension	|The dimensions of the creative in pixels	|
|creative_type	|creativeType	|String	|Dimension	|The type of creative (for example static image, third party, or video)	|
|deal	|deal	|String	|Dimension	|The name of a deal	|
|deal_id	|dealID	|String	|Dimension	|The unique identifier for the deal	|
|deal_type	|dealType	|String	|Dimension	|The type of deal	|
|device	|device	|String	|Dimension	|The devices the customers used to view the ad	|
|dsp\_entity\_id	|entityId	|String	|Dimension	|Entity (seat) ID	|
|placement_name	|placementName	|String	|Dimension	|The name of the placement where the campaign ran	|
|proposal_id	|proposalId	|String	|Dimension	|A unique identifier for an order imported from the Amazon Order Management System (OMS)	|
|region	|region	|String	|Dimension	|State or region	|
|site	|siteName	|String	|Dimension	|The site or group of sites the campaign ran on	|
|supply\_source\_name	|supplySourceName	|String	|Dimension	|The inventory the campaign ran on, for example real-time bidding exchanges or Amazon-owned sites	|
|marketplace_id	|N/A	|String	|Dimension	|The marketplace of the advertiser.|
|campaign_id	| orderId	|String	|Dimension	|The ID associated with a campaign	|
|add\_to\_cart	|addToCart	|Integer	|Metric	|Number of times shoppers added a brand's products to their cart, attributed to an ad view or click	|
|add\_to\_cart\_clicks	|addToCartClicks	|Integer	|Metric	|Number of times shoppers added a brand's products to their cart, attributed to an ad click	|
|add\_to\_cart\_views	|addToCartViews	|Integer	|Metric	|Number of times shoppers added the brand's products to their cart, attributed to an ad view	|
|add\_to\_list	|addToList	|Integer	|Metric	|Number of times shoppers added a promoted product to a wish list, gift list, or registry, attributed to an ad view or click. Use total_add_to_list to see all conversions for the brand's products.	|
|add\_to\_list\_clicks	|addToListClicks	|Integer	|Metric	|Number of times shoppers added a promoted product to a wish list, gift list, or registry, attributed to an ad click. Use total_add_to_list clicks to see all conversions for the brand's products.	|
|add\_to\_list\_views	|addToListViews	|Integer	|Metric	|Number of times shoppers added a promoted product to a wish list, gift list, or registry, attributed to an ad view. Use total_add_to_list views to see all conversions for the brand's products.	|
|detail\_page\_views	|detailPageViews	|Integer	|Metric	|Number of detail page views occurring within 14 days of an ad click or view	|
|detail\_page\_view\_clicks	|detailPageViewClicks	|Integer	|Metric	|Number of detail page views occurring within 14 days of an ad click	|
|detail\_page\_view\_views	|detailPageViewViews	|Integer	|Metric	|The number of times a product detail page was viewed attributed to an ad view	|
|subscribe\_and\_save	|newSubscribeAndSave	|Integer	|Metric	|Number of new Subscribe & Save subscriptions for all products, attributed to an ad view or click. This does not include replenishment subscription orders. Use total_subscribe_and_save to see all conversions for the brand's products.	|
|subscribe\_and\_save\_clicks	|newSubscribeAndSaveClicks	|Integer	|Metric	|Number of new Subscribe & Save subscriptions for all products, attributed to ad clicks. This does not include replenishment subscription orders. Use total_subscribe_and_save to see all conversions for the brand's products.	|
|subscribe\_and\_save\_views	|newSubscribeAndSaveViews	|Integer	|Metric	|Number of new Subscribe & Save subscriptions for all products, attributed to ad views. This does not include replenishment subscription orders. Use total_subscribe_and_save to see all conversions for the brand's products.	|
|total\_add\_to\_cart	|totalAddToCart	|Integer	|Metric	|Number of times shoppers added the brand's products to their cart, attributed to an ad view or click	|
|total\_add\_to\_cart\_clicks	|totalAddToCartClicks	|Integer	|Metric	|Number of times shoppers added the brand's products to their cart, attributed to an ad click	|
|total\_add\_to\_cart\_views	|totalAddToCartViews	|Integer	|Metric	|Number of times shoppers added the brand's products to their cart, attributed to an ad view	|
|total\_add\_to\_list	|totalAddToList	|Integer	|Metric	|Number of times shoppers added the brand's products to a wish list, gift list, or registry, attributed to an ad view or click	|
|total\_add\_to\_list\_clicks	|totalAddToListClicks	|Integer	|Metric	|Number of times shoppers added the brand's products to a wish list, gift list, or registry, attributed to an ad click	|
|total\_add\_to\_list\_views	|totalAddToListViews	|Integer	|Metric	|Number of times shoppers added the brand's products to a wish list, gift list or registry, attributed to an ad view	|
|total\_detail\_page\_view\_clicks	|totalDetailPageViewClicks	|Integer	|Metric	|Number of detail page views for all the brands and products attributed to an ad click	|
|total\_detail\_page\_views	|totalDetailPageView	|Integer	|Metric	|Number of detail page views for all the brands and products attributed to an ad view or click	|
|total\_detail\_page\_view\_views	|totalDetailPageViewViews	|Integer	|Metric	|Number of detail page views for all the brand's products, attributed to an ad view	|
|total\_subscribe\_and\_save\_clicks	|totalSubscribeAndSaveClicks	|Integer	|Metric	|Number of new Subscribe & Save subscriptions for the brands and products, attributed to an ad click. This does not include replenishment subscription orders.	|
|total\_subscribe\_and\_save	|totalSubscribeAndSave	|Integer	|Metric	|Number of new Subscribe & Save subscriptions for the brands and products, attributed to an ad view or click. This does not include replenishment subscription orders.	|
|total\_subscribe\_and\_save\_views	|totalSubscribeAndSaveViews	|Integer	|Metric	|Number of new Subscribe & Save subscriptions for the brands and products, attributed to an ad view. This does not include replenishment subscription orders.	|
|add\_to\_watchlist\_clicks	|addToWatchlistClicks	|Integer	|Metric	|The number of Add to Watchlist clicks attributed to ad click-throughs.	|
|add\_to\_watchlist	|addToWatchlist	|Integer	|Metric	|The number of times Add to Watchlist was clicked on a featured product.	|
|add\_to\_watchlist\_views	|addToWatchlistViews	|Integer	|Metric	|The number of Add to Watchlist clicks attributed to ad impressions.	|
|video\_downloads\_clicks	|videoDownloadsClicks	|Integer	|Metric	|The number of video downloads attributed to ad click-throughs.	|
|video\_downloads	|videoDownloads	|Integer	|Metric	|The number of times a video was downloaded for the featured product.	|
|video\_downloads\_views	|videoDownloadsViews	|Integer	|Metric	|The number of video downloads attributed to ad impressions.	|
|downloaded\_video\_plays\_clicks	|downloadedVideoPlaysClicks	|Integer	|Metric	|The number of downloaded video plays attributed to ad click-throughs.	|
|downloaded\_video\_plays	|downloadedVideoPlays	|Integer	|Metric	|The number of times a video was downloaded then played for the featured product.	|
|downloaded\_video\_plays\_views	|downloadedVideoPlaysViews	|Integer	|Metric	|The number of downloaded video plays attributed to ad impressions.	|
|play\_trailers\_clicks	|playTrailersClicks	|Integer	|Metric	|The number of video trailer plays attributed to ad click-throughs.	|
|play\_trailers	|playTrailers	|Integer	|Metric	|The number of times a video trailer was played for the featured product.	|
|play\_trailers\_views	|playTrailersViews	|Integer	|Metric	|The number of video trailer players attributed to ad impressions.	|
|video\_streams\_clicks	|videoStreamsClicks	|Integer	|Metric	|The number of video streams attributed to ad click-throughs.	|
|video\_streams	|videoStreams	|Integer	|Metric	|The number of times a video was streamed (played without downloading).	|
|video\_streams\_views	|videoStreamsViews	|Integer	|Metric	|The number of video streams attributed to ad impressions.	|
|rentals\_clicks	|rentalsClicks	|Integer	|Metric	|The number of video rentals attributed to ad click-throughs.	|
|rentals	|rentals	|Integer	|Metric	|The number of times a video was rented for the featured product.	|
|rentals\_views	|rentalsViews	|Integer	|Metric	|The number of video rentals attributed to ad impressions.	|
|skill\_invocation\_clicks	|skillInvocationClicks	|Integer	|Metric	|The number of times shoppers launched a promoted Alexa skill, attributed to an ad click. This can include tap, touch, and voice invocations.	|
|skill\_invocation	|skillInvocation	|Integer	|Metric	|The number of times shoppers launched a promoted Alexa skill, attributed to an ad view or click. This can include tap, touch, and voice invocations.	|
|skill\_invocation\_views	|skillInvocationViews	|Integer	|Metric	|The number of times shoppers launched a promoted Alexa skill, attributed to an ad view. This can include tap, touch, and voice invocations.	|
|total\_new\_to\_brand\_dpv\_clicks	|totalNewToBrandDPVClicks	|Integer	|Metric	|Number of new-to-brand detail page views for all the brands' products, attributed to an ad click.	|
|total\_new\_to\_brand\_dpvs	|totalNewToBrandDPVs	|Integer	|Metric	|Number of new-to-brand detail page views for all the brands' products, attributed to an ad view or click.	|
|total\_new\_to\_brand\_dpv\_views	|totalNewToBrandDPVViews	|Integer	|Metric	|Number of new-to-brand detail page views for all the brands' products, attributed to an ad view.	|
|total\_product\_review\_page\_visits\_clicks	|totalProductReviewPageVisitsClicks	|Integer	|Metric	|Number of times shoppers visited a product review page for the brands' products, attributed to an ad click.	|
|total\_product\_review\_page\_visits	|totalProductReviewPageVisits	|Integer	|Metric	|Number of times shoppers visited a product review page for the brands' products, attributed to an ad view or click.	|
|total\_product\_review\_page\_visits\_views	|totalProductReviewPageVisitsViews	|Integer	|Metric	|Number of times shoppers visited a product review page for the brands' products, attributed to an ad view.	|
|brand\_searches\_clicks	|brandSearchesClicks	|Integer	|Metric	|The number of branded search conversions attributed to ad click-throughs.	|
|brand\_searches	|brandSearches	|Integer	|Metric	|The number of times a branded keyword was searched on Amazon based on keywords generated from the featured ASINs in your campaign.	|
|brand\_searches\_views	|brandSearchesViews	|Integer	|Metric	|The number of branded search conversions attributed to ad impressions.	|
|click\_to\_apply\_clicks	|clickToApplyClicks	|Integer	|Metric	|The number of click to apply conversions attributed to ad click-throughs.	|
|click\_to\_apply	|clickToApply	|Integer	|Metric	|The number of time Click to apply was clicked for a featured product.	|
|click\_to\_apply\_views	|clickToApplyViews	|Integer	|Metric	|The number of Click to apply conversions attributed to ad impressions.	|
|off\_amazon\_other\_clicks	|otherClicks	|Integer	|Metric	|The number of Other conversions attributed to an ad click.	|
|off\_amazon\_other	|other	|Integer	|Metric	|The number of Other conversions.	|
|off\_amazon\_other\_views	|otherViews	|Integer	|Metric	|The number of Other conversions attributed to an ad view.	|
|off\_amazon\_page\_view\_clicks	|pageViewClicks	|Integer	|Metric	|The number of Page view conversions attributed to an ad click.	|
|off\_amazon\_page\_views	|pageViews	|Integer	|Metric	|The number of Page view conversions.	|
|off\_amazon\_page\_view\_views	|pageViewViews	|Integer	|Metric	|The number of Page view conversions attributed to an ad view.	|
|off\_amazon\_search\_clicks	|searchClicks	|Integer	|Metric	|The number of Search conversions attributed to an ad click.	|
|off\_amazon\_search	|search	|Integer	|Metric	|The number of Search conversions.	|
|off\_amazon\_search\_views	|searchViews	|Integer	|Metric	|The number of Search conversions attributed to an ad view.	|
|off\_amazon\_contact\_clicks	|contactClicks	|Integer	|Metric	|The number of Contact conversions attributed to an ad click.	|
|off\_amazon\_contact	|contact	|Integer	|Metric	|The number of Contact conversions.	|
|off\_amazon\_contact\_views	|contactViews	|Integer	|Metric	|The number of Contact conversions attributed to an ad view.	|
|off\_amazon\_checkout\_clicks	|checkoutClicks	|Integer	|Metric	|The number of Checkout conversions attributed to an ad click.	|
|off\_amazon\_checkout	|checkout	|Integer	|Metric	|The number of Checkout conversions.	|
|off\_amazon\_checkout\_views	|checkoutViews	|Integer	|Metric	|The number of Checkout conversions attributed to an ad view.	|
|off\_amazon\_lead\_clicks	|leadClicks	|Integer	|Metric	|The number of Lead conversions attributed to an ad click.	|
|off\_amazon\_lead	|lead	|Integer	|Metric	|The number of Lead conversions.	|
|off\_amazon\_lead\_views	|leadViews	|Integer	|Metric	|The number of Lead conversions attributed to an ad view.	|
|off\_amazon\_signup\_views	|signUpViews	|Integer	|Metric	|Number of Sign-up conversions occurring off Amazon, attributed to an ad view.	|
|off\_amazon\_mobile\_app\_first\_start\_clicks	|mobileAppFirstStartClicks	|Integer	|Metric	|The number of Mobile app first start conversions attributed to ad click-throughs.	|
|off\_amazon\_mobile\_app\_starts	|mobileAppFirstStarts	|Integer	|Metric	|The number of times an app for the featured product was first started.	|
|off\_amazon\_mobile\_app\_first\_start\_views	|mobileAppFirstStartViews	|Integer	|Metric	|The number of Mobile app first start conversions attributed to ad impressions.	|
|off\_amazon\_subscribe\_clicks	|subscribeClicks	|Integer	|Metric	|Number of Subscribe conversions occurring off Amazon, attributed to an ad click.	|
|off\_amazon\_subscribe	|subscribe	|Integer	|Metric	|Number of Subscribe conversions occurring off Amazon, attributed to an ad view or click.	|
|off\_amazon\_subscribe\_views	|subscribeViews	|Integer	|Metric	|Number of Subscribe conversions occurring off Amazon, attributed to an ad view.	|
|off\_amazon\_add\_to\_shopping\_cart\_clicks	|addToShoppingCartClicks	|Integer	|Metric	|The number of Add to shopping cart conversions attributed to an ad click.	|
|off\_amazon\_add\_to\_shopping\_cart	|addToShoppingCart	|Integer	|Metric	|The number of Add to shopping cart conversions.	|
|off\_amazon\_add\_to\_shopping\_cart\_views	|addToShoppingCartViews	|Integer	|Metric	|The number of Add to shopping cart conversions attributed to an ad view.	|
|off\_amazon\_purchases\_clicks	|offAmazonPurchasesClicks	|Integer	|Metric	|Number of off-Amazon purchase events attributed to an ad click.	|
|off\_amazon\_purchases	|offAmazonPurchases	|Integer	|Metric	|Off Amazon Purchases	|
|off\_amazon\_product\_sales	|offAmazonProductSales	|Currency	|Metric	|Sales (in local currency) of promoted products to off Amazon shoppers, attributed to an ad view or click.	|
|off\_amazon\_purchases\_views	|offAmazonPurchasesViews	|Integer	|Metric	|Number of off-Amazon purchase events attributed to an ad view.	|
|off\_amazon\_application\_clicks	|applicationClicks	|Integer	|Metric	|The number of Application conversions attributed to an ad click.	|
|off\_amazon\_application	|application	|Integer	|Metric	|The number of Application conversions.	|
|off\_amazon\_application\_views	|applicationViews	|Integer	|Metric	|The number of Application conversions attributed to an ad view.	|
|off\_amazon\_clicks	|offAmazonClicks	|Integer	|Metric	|Number of conversions that occured off Amazon attributed to an ad click. This includes all conversion types.	|
|off\_amazon\_views	|offAmazonViews	|Integer	|Metric	|Number of conversions that occured off Amazon attributed to an ad view. This includes all conversion types.	|
|product\_review\_page\_visits\_clicks	|productReviewPageVisitsClicks	|Integer	|Metric	|Number of times shoppers visited the product review page for a promoted product, attributed to an ad click. Use Total PRPV clicks to see all conversions for the brands' products.	|
|product\_review\_page\_visits	|productReviewPageVisits	|Integer	|Metric	|Number of times shoppers visited the product review page for a promoted product, attributed to an ad view or click. Use Total PRPV to see all conversions for the brands' products.	|
|product\_review\_page\_visits\_views	|productReviewPageVisitsViews	|Integer	|Metric	|Number of times shoppers visited the product review page for a promoted product, attributed to an ad view. Use Total PRPV views to see all conversions for the brands' products.	|
|new\_to\_brand\_detail\_page\_view\_clicks	|newToBrandDetailPageViewClicks	|Integer	|Metric	|Number of new-to-brand detail page views for all the brands' products, attributed to an ad click.	|
|new\_to\_brand\_detail\_page\_views	|newToBrandDetailPageViews	|Integer	|Metric	|The number of new detail page views from shoppers who have not previously viewed a detail page with an ASIN of the same brand in past 365 days and who either clicked or viewed an ad.	|
|new\_to\_brand\_detail\_page\_view\_views	|newToBrandDetailPageViewViews	|Integer	|Metric	|Number of new-to-brand detail page views for all the brands' products, attributed to an ad view	|

### Sample payload

```
{
    "dataset_id": "adsp-clickstream",
    "idempotency_id": "string",
    "time_window_start": "string",
    "adgroup_id": "string",
    "advertiser_country": "string",
    "advertiser_id": "string",
    "advertiser_name": "string",
    "advertiser_timezone": "string",
    "country": "string",
    "creative_id": "string",
    "creative_language": "string",
    "creative_name": "string",
    "creative_size": "string",
    "creative_type": "string",
    "deal": "string",
    "deal_id": "string",
    "deal_type": "string",
    "device": "string",
    "dsp_entity_id": "string",
    "placement_name": "string",
    "proposal_id": "string",
    "region": "string",
    "site": "string",
    "supply_source_name": "string",
    "marketplace_id": "string",
    "campaign_id": "string",
    "add_to_cart": 0,
    "add_to_cart_clicks": 0,
    "add_to_cart_views": 0,
    "add_to_list": 0,
    "add_to_list_clicks": 0,
    "add_to_list_views": 0,
    "detail_page_views": 0,
    "detail_page_view_clicks": 0,
    "detail_page_view_views": 0,
    "subscribe_and_save": 0,
    "subscribe_and_save_clicks": 0,
    "subscribe_and_save_views": 0,
    "total_add_to_cart": 0,
    "total_add_to_cart_clicks": 0,
    "total_add_to_cart_views": 0,
    "total_add_to_list": 0,
    "total_add_to_list_clicks": 0,
    "total_add_to_list_views": 0,
    "total_detail_page_view_clicks": 0,
    "total_detail_page_views": 0,
    "total_detail_page_view_views": 0,
    "total_subscribe_and_save_clicks": 0,
    "total_subscribe_and_save": 0,
    "total_subscribe_and_save_views": 0,
    "add_to_watchlist_clicks": 0,
    "add_to_watchlist": 0,
    "add_to_watchlist_views": 0,
    "video_downloads_clicks": 0,
    "video_downloads": 0,
    "video_downloads_views": 0,
    "downloaded_video_plays_clicks": 0,
    "downloaded_video_plays": 0,
    "downloaded_video_plays_views": 0,
    "play_trailers_clicks": 0,
    "play_trailers": 0,
    "play_trailers_views": 0,
    "video_streams_clicks": 0,
    "video_streams": 0,
    "video_streams_views": 0,
    "rentals_clicks": 0,
    "rentals": 0,
    "rentals_views": 0,
    "skill_invocation_clicks": 0,
    "skill_invocation": 0,
    "skill_invocation_views": 0,
    "total_new_to_brand_dpv_clicks": 0,
    "total_new_to_brand_dpvs": 0,
    "total_new_to_brand_dpv_views": 0,
    "total_product_review_page_visits_clicks": 0,
    "total_product_review_page_visits": 0,
    "total_product_review_page_visits_views": 0,
    "brand_searches_clicks": 0,
    "brand_searches": 0,
    "brand_searches_views": 0,
    "click_to_apply_clicks": 0,
    "click_to_apply": 0,
    "click_to_apply_views": 0,
    "off_amazon_other_clicks": 0,
    "off_amazon_other": 0,
    "off_amazon_other_views": 0,
    "off_amazon_page_view_clicks": 0,
    "off_amazon_page_views": 0,
    "off_amazon_page_view_views": 0,
    "off_amazon_search_clicks": 0,
    "off_amazon_search": 0,
    "off_amazon_search_views": 0,
    "off_amazon_contact_clicks": 0,
    "off_amazon_contact": 0,
    "off_amazon_contact_views": 0,
    "off_amazon_checkout_clicks": 0,
    "off_amazon_checkout": 0,
    "off_amazon_checkout_views": 0,
    "off_amazon_lead_clicks": 0,
    "off_amazon_lead": 0,
    "off_amazon_lead_views": 0,
    "off_amazon_signup_views": 0,
    "off_amazon_mobile_app_first_start_clicks": 0,
    "off_amazon_mobile_app_starts": 0,
    "off_amazon_mobile_app_first_start_views": 0,
    "off_amazon_subscribe_clicks": 0,
    "off_amazon_subscribe": 0,
    "off_amazon_subscribe_views": 0,
    "off_amazon_add_to_shopping_cart_clicks": 0,
    "off_am_amazon_add_to_shopping_cart": 0,
    "off_amazon_add_to_shopping_cart_views": 0,
    "off_amazon_purchases_clicks": 0,
    "off_amazon_purchases": 0,
    "off_amazon_product_sales": 0,
    "off_amazon_purchases_views": 0,
    "off_amazon_application_clicks": 0,
    "off_amazon_application": 0,
    "off_amazon_application_views": 0,
    "off_amazon_clicks": 0,
    "off_amazon_views": 0,
    "product_review_page_visits_clicks": 0,
    "product_review_page_visits": 0,
    "product_review_page_visits_views": 0,
    "new_to_brand_detail_page_view_clicks": 0,
    "new_to_brand_detail_page_views": 0,
    "new_to_brand_detail_page_view_views": 0
}
```

### IAM policy

To subscribe to this dataset, ensure the correct IAM policy is added to the SQS queue based on the marketplace.

**NA**

```
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
          "aws:SourceArn": "arn:aws:sns:us-east-1:740036912861:*"
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

```
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:475804811800:*"
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

```
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:201394813667:*"
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

## Amazon DSP rich media dataset

The adsp-rich-media dataset contains video data related to Amazon DSP campaigns.

### Dataset ID

To subscribe to this dataset, use the ID `adsp-rich-media`.

### Schema

This dataset supports the following fields and metrics:

|Field name	|API name	|Type	|Type	|Description	|
|---	|---	|---	|---	|---	|
|dataset_id	|N/A	|String	|Dimension	|An identifier used to identify the dataset	|
|idempotency_id	|N/A	|String	|Dimension	|An identifier that can be used to de-duplicate records	|
|time\_window\_start	|N/A	|String	|Dimension	|The start of the hour at which the performance data is attributed (ISO 8601 date time). Time zone for each advertiser is indicated in the [advertiser profile](reference/2/profiles#/Profiles/listProfiles).	|
|adgroup_id	|lineItemId	|String	|Dimension	|A unique identifier assigned to a line item or ad group	|
|advertiser_country	|advertiserCountry	|String	|Dimension	|The country assigned to the advertiser	<br/> **Note:** This field can be null if the advertiser is not tied to a specific country|
|advertiser_id	|advertiserId	|String	|Dimension	|The unique identifier for the advertiser	|
|advertiser_name	|advertiserName	|String	|Dimension	|The customer that has an advertising relationship with Amazon	|
|advertiser_timezone	|advertiserTimezone	|String	|Dimension	|The time zone the advertiser uses for reporting and ad serving purposes	|
|country	|country	|String	|Dimension	|Country or locale	|
|creative_id	|creativeId	|String	|Dimension	|A unique identifier assigned to a creative. When this is different from creativeAdId, creativeId cannot be used in our /dsp/creatives endpoints.	|
|creative_language	|creativeLanguage	|String	|Dimension	|The primary language in the creative	|
|creative_name	|creativeName	|String	|Dimension	|Creative name	|
|creative_size	|creativeSize	|String	|Dimension	|The dimensions of the creative in pixels	|
|creative_type	|creativeType	|String	|Dimension	|The type of creative (for example static image, third party, or video)	|
|deal	|deal	|String	|Dimension	|The name of a deal	|
|deal_id	|dealID	|String	|Dimension	|The unique identifier for the deal	|
|deal_type	|dealType	|String	|Dimension	|The type of deal	|
|device	|device	|String	|Dimension	|The devices the customers used to view the ad	|
|dsp\_entity\_id	|entityId	|String	|Dimension	|Entity (seat) ID	|
|placement_name	|placementName	|String	|Dimension	|The name of the placement where the campaign ran	|
|proposal_id	|proposalId	|String	|Dimension	|A unique identifier for an order imported from the Amazon Order Management System (OMS).	|
|region	|region	|String	|Dimension	|State or region.	|
|site	|siteName	|String	|Dimension	|The site or group of sites the campaign ran on.	|
|supply\_source\_name	|supplySourceName	|String	|Dimension	|The inventory the campaign ran on, for example real-time bidding exchanges or Amazon-owned sites.	|
|marketplace_id	|N/A	|String	|Dimension	|The marketplace of the advertiser.|
|campaign_id	|orderId	|String	|Dimension	|The ID associated with a campaign.	|
|audio\_ad\_companion\_banner_clicks	|audioAdCompanionBannerClicks	|Integer	|Metric	|Tracks the number of times that the audio ad companion banner was clicked.	|
|audio\_ad\_companion\_banner\_views	|audioAdCompanionBannerViews	|Integer	|Metric	|Tracks the number of times that the audio ad companion banner was displayed.	|
|audio\_ad\_completions	|audioAdCompletions	|Integer	|Metric	|Tracks the number of times the audio ad plays to the end.	|
|audio\_ad\_first\_quartile	|audioAdFirstQuartile	|Integer	|Metric	|Tracks the number of times the audio ad plays to 25% of its length.	|
|audio\_ad\_midpoint	|audioAdMidpoint	|Integer	|Metric	|Tracks the number of times the audio ad plays to 50% of its length	|
|audio\_ad\_mute	|audioAdMute	|Integer	|Metric	|Tracks the number of times a user mutes the audio ad.	|
|audio\_ad\_pause	|audioAdPause	|Integer	|Metric	|The number of times a user pauses the audio ad.	|
|audio\_ad\_progression	|audioAdProgression	|Integer	|Metric	|Tracks an optimal audio ad time marker agreed upon with the publisher	|
|audio\_ad\_resume	|audioAdResume	|Integer	|Metric	|Tracks the number of times a user resumes playback of the audio ad.	|
|audio\_ad\_rewind	|audioAdRewind	|Integer	|Metric	|Tracks the number of times a user rewinds the audio ad.	|
|audio\_ad\_skip	|audioAdSkip	|Integer	|Metric	|Tracks the number of times a user skips the audio ad.	|
|audio\_ad\_start	|audioAdStart	|Integer	|Metric	|Tracks the number of times audio ad starts playing.	|
|audio\_ad\_third\_quartile	|audioAdThirdQuartile	|Integer	|Metric	|Tracks the number of times the audio ad plays to 75% of its length.	|
|audio\_ad\_unmute	|audioAdUnmute	|Integer	|Metric	|Tracks the number of times a user skips the audio ad.	|
|audio\_ad\_views	|audioAdViews	|Integer	|Metric	|Tracks the number of times that some playback of the audio ad has occurred.	|
|video\_ad\_complete	|videoComplete	|Integer	|Metric	|The number of times a video ad played to completion. If rewind occurred, completion was calculated on the total percentage of unduplicated video viewed.	|
|video\_ad\_first\_quartile	|videoFirstQuartile	|Integer	|Metric	|The number of times at least 25% of a video ad played. If rewind occurred, percent complete was calculated on the total percentage of unduplicated video viewed.	|
|video\_ad\_midpoint	|videoMidpoint	|Integer	|Metric	|The number of times at least 50% of a video ad played. If rewind occurred, percent complete was calculated on the total percentage of unduplicated video viewed.	|
|video\_ad\_mute	|videoMute	|Integer	|Metric	|The number of times a user muted the video ad.	|
|video\_ad\_pause	|videoPause	|Integer	|Metric	|The number of times a user paused the video ad.	|
|video\_ad\_resume	|videoResume	|Integer	|Metric	|The number of times a user unpaused the video ad.	|
|video\_ad\_start	|videoStart	|Integer	|Metric	|The number of times a video ad was started.	|
|video\_ad\_third\_quartile	|videoThirdQuartile	|Integer	|Metric	|The number of times at least 75% of a video ad played. If rewind occurred, percent complete was calculated on the total percentage of unduplicated video viewed.	|
|video\_ad\_unmute	|videoUnmute	|Integer	|Metric	|The number of times a user unmuted the video ad.	|

### Sample payload

```
{
    "dataset_id": "adsp-rich-media",
    "idempotency_id": "string",
    "time_window_start": "string",
    "adgroup_id": "string",
    "advertiser_country": "string",
    "advertiser_id": "string",
    "advertiser_name": "string",
    "advertiser_timezone": "string",
    "country": "string",
    "creative_id": "string",
    "creative_language": "string",
    "creative_name": "string",
    "creative_size": "string",
    "creative_type": "string",
    "deal": "string",
    "deal_id": "string",
    "deal_type": "string",
    "device": "string",
    "dsp_entity_id": "string",
    "placement_name": "string",
    "proposal_id": "string",
    "region": "string",
    "site": "string",
    "supply_source_name": "string",
    "marketplace_id": "string",
    "campaign_id": "string",
    "audio_ad_companion_banner_clicks": 0,
    "audio_ad_companion_banner_views": 0,
    "audio_ad_completions": 0,
    "audio_ad_first_quartile": 0,
    "audio_ad_midpoint": 0,
    "audio_ad_mute": 0,
    "audio_ad_pause": 0,
    "audio_ad_progression": 0,
    "audio_ad_resume": 0,
    "audio_ad_rewind": 0,
    "audio_ad_skip": 0,
    "audio_ad_start": 0,
    "audio_ad_third_quartile": 0,
    "audio_ad_unmute": 0,
    "audio_ad_views": 0,
    "video_ad_complete": 0,
    "video_ad_first_quartile": 0,
    "video_ad_midpoint": 0,
    "video_ad_mute": 0,
    "video_ad_pause": 0,
    "video_ad_resume": 0,
    "video_ad_start": 0,
    "video_ad_third_quartile": 0,
    "video_ad_unmute": 0
}
```

### Resource-based IAM policy

To subscribe to this dataset, ensure the correct IAM policy is added to the SQS queue based on the marketplace.

**NA**

```
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
          "aws:SourceArn": "arn:aws:sns:us-east-1:718926279583:*"
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

```
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:047120534722:*"
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

```
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:886046350753:*"
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
