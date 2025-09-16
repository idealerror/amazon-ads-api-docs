---
title: Sponsored ads campaign recommendations
description: Details and schema for the sponsored ads campaign recommendations dataset.
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Campaign management
    - Sponsored Display
---

# Sponsored ads campaign recommendations (beta)

This dataset contains recommendations to optimize existing ad campaigns or create new ad campaigns to improve performance of your sponsored ads portfolio. The types of recommendations delivered by the dataset can be found in the table below.

>[NOTE] To subscribe or manage subscriptions to this dataset, use the [/streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription) endpoints.

## Dataset ID

To subscribe to this dataset, use the ID `sponsored-ads-campaign-diagnostics-recommendations`.

## Recommendation types

The following table details the types of recommendations available by ad type as part of this dataset. 

|Ad product	|Description	|
|---	|---	|
|Sponsored Brands, Sponsored Products	|Campaign with over 50% budget utilization, which will be impacted by an upcoming special event. Budget rules help prevent campaigns from going out of budget during peak traffic periods. Apply the recommended budget.	|
|Sponsored Brands	|Low-performing Sponsored Brands campaign with at least one impression in the last 14 days and ROAS less than or equal to 3. Apply the recommended bid to improve performance.	|
|Sponsored Display	|Low-performing Sponsored Display campaign with spend in the top 50% of the advertiser's campaigns. Apply the recommended bid to improve performance.	|
|Sponsored Display	|Advertiser is not actively using Sponsored Display audiences. Create a Sponsored Display campaign to reengage audiences and help secure missed sales opportunities or further cultivate brand loyalty.	|
|Sponsored Display	|The advertiser is active on Sponsored Display CPC campaigns, but not using reach bid-optimization, which can increase product awareness on and off Amazon. Create a Sponsored Display campaign optimized for reach to increase awareness.	|
|Sponsored Display	|Advertiser is not actively using Sponsored Display contextual targeting. Create a Sponsored Display campaign to target specific products and categories of detail pages on Amazon to help drive consideration and sales.	|
|Sponsored Display	|These ASINs are forecasted to have high sales in next 90 days, but are not advertised. Create Sponsored Display contextual campaign for high performing ASINs.	|
|Sponsored Display	|Enabled Sponsored Display campaigns have zero ad spend or clicks. Campaigns that do not generate clicks, also do not generate attributed orders. Recommend adding new ad groups in these campaigns with suggested ASINs, targets, and bids to improve performance.	|
|Sponsored Products	|Campaigns have ROAS greater than 5. Recommend advertiser to optimize bids to drive impressions and sales.	|
|Sponsored Products	|High-performing campaign with ROAS greater than 3 and last week's budget utilization greater than 80%. We've estimated this is a missed opportunity in sales and clicks. Apply the recommended budget.	|
|Sponsored Products	|The advertiser has ad-ready ASINs that they're not advertising. Ad-ready ASINS are 16 times more likely to be clicked on if advertised. Create campaigns with ad-ready ASINS to increase clicks.	|
|Sponsored Products	|The advertiser has unadvertised ASINs with deals starting in the next 7-14 days. Advertise ASINs with deals to increase impressions and clicks.	|
|Sponsored Products	|These ASINs are forecasted to have high sales in next 90 days, but are not advertised. Create a Sponsored Product manual campaign to advertise the high performing ASINs.	|
|Sponsored Products	|Enabled Sponsored Products auto-targeting campaigns with zero ad spend. Campaigns that do not generate clicks do not generate attributed orders. Recommend creating new ad groups in these campaigns with suggested ASINs, targets, and bids to improve performance.	|
|Sponsored Products	|Enabled Sponsored Products manual-targeting campaigns have zero ad spend or clicks. Campaigns that do not generate clicks, do not generate attributed orders. Recommend adding new ad groups in these campaigns with suggested ASINs, targets, and bids to improve performance.	|
|Sponsored Products	|Low-performing Sponsored Products campaign with at least one impression in the last 14 days and ROAS less than or equal to 3. Apply the recommended bid to improve performance.	|
| Sponsored Products | Ad groups either did not receive any impressions or clicks, or had 50% decline in impressions or clicks compared in last 14 days. Consider adding new keywords. |
| Sponsored Brands | Campaigns have less than 200 impressions or zero clicks in last 14 days. Add new keywords to help improve performance. |
| Sponsored Display | Advertiser's top performing cost per click Sponsored Dsiplay campaigns with spend in the bottom 50%. Add new targets to drive sales. |
| Sponsored Display | These ASINs have upcoming deals in 7-14 days. Promoting deal ASINs in detail pages of relevant products and categories can increase reach and sales. |
| Sponsored Products | Campaigns have lower click through and conversion compared to other similar campaigns. Recommend adding new keywords to drive performance. |
| Sponsored Brands | Sponsored Brands campaigns with ROAS above 3 that have used more than 80% of their budget. Running out of budget could result in missed impressions and clicks. Recommend the advertiser increase budget. |
| Sponsored Products | Newly launched ASINs that are yet to be advertised. Creating new campaigns for these ASINs could help drive incremental clicks and conversions. |

## Schema

>[NOTE] This dataset contains nested objects.

|Field name	| Type	| Description	|
|---	|---	|---	|
|recommendation\_id	|String	|A unique identifier for every recommendation in this dataset.	|
|advertiser\_id	|String	| ID associated with the advertiser. You can retrieve this ID using the [GET v2/profiles](reference/2/profiles#tag/Profiles/operation/listProfiles) endpoint. This value is **not** unique and may be the same across marketplaces. Note: For non-seller accounts, this advertiser_id is set to the entity ID.	|
|marketplace\_id	|String	|[The marketplace identifier associated with the account.](https://developer-docs.amazon.com/sp-api/docs/marketplace-ids)	|
|dataset\_id	|String	|An identifier used to identify the dataset (in this case, *sponsored-ads-campaign-diagnostics-recommendations*).	|
|group\_id	|String	|Identifier to suggest grouping of recommendations in case multiple recommendations are suggested against same advertiser\_id / marketplace\_id / campaign\_id combination.	|
|apply\_endpoint	|String	|API endpoint to apply this recommendation in one call. Users can call this API and provide the recommendation_id the API request body to adopt this recommendation. [API reference](recommendations#/Recommendations/ApplyRecommendations).	|
|type	|String	|Type of recommendation. Currently we support CAMPAIGN\_BUDGET, KEYWORD\_BID. NEW\_CAMPAIGN, CAMPAIGN\_BUDGET\_RULE, NEW\_CAMPAIGN\_BUDGET\_RULE, PRODUCT\_TARGETING\_BID, AUDIENCE\_TARGETING\_BID	|
|published\_date	|String	|Date when this recommendation was published.	|
|expiry\_date	|String	|Date after which this recommendation would expire.	|
|explanation	|Object	|Object containing the reasons for recommending this campaign change. See the [Explanation object](guides/amazon-marketing-stream/data-guide#explanation-object) for more details.	|
|campaign\_name	|String	|Name of the campaign for which recommendations are being made.	|
|campaign\_id	|String	|Unique numerical ID for a campaign.	|
|ad\_product	|String	|Ad product of the campaign. (One of: SP, SB, SD).	|
|current\_campaign\_settings	|Object	|The settings currently used for your campaign. See [CampaignSettings](guides/amazon-marketing-stream/data-guide#campaignsettings-object) object for details.	|
|recommended\_campaign\_settings	|Object	|The recommended settings for your campaign. See [CampaignSettings](guides/amazon-marketing-stream/data-guide#campaignsettings-object) object for details.	|

### Explanation object

|Field name	| Type	| Description	|
|---	|---	|---	|
|description	|String	|Provides the user readable reason for suggesting this change.	|
|missed\_opportunities	|Object	| Object containing the estimated additional impressions, clicks, and sales the campaign might have generated had it adopted the recommended budget. These are estimates based on previous website traffic and historical campaign performance, not a guarantee of actual impressions, clicks, and sales.	|
|missed\_opportunities.impressions	|Object	| Object containing the estimated additional impressions the campaign might have generated had it adopted the recommended budget. These are estimates based on previous website traffic and historical campaign performance, not a guarantee of actual impressions.	|
|missed\_opportunities.impressions.upper\_bound	|Integer	|Upper bound value for the missed opportunity impressions.	|
|missed\_opportunities.impressions.lower\_bound	|Integer	|Lower bound value for the missed opportunity impressions.	|
|missed\_opportunities.clicks	|Object	| Object containing the estimated additional clicks the campaign might have generated had it adopted the recommended budget. These are estimates based on previous website traffic and historical campaign performance, not a guarantee of actual clicks.	|
|missed\_opportunities.clicks.upper\_bound	|Integer	|Upper bound value for the missed opportunity clicks.	|
|missed\_opportunities.clicks.lower\_bound	|Integer	|Lower bound value for the missed opportunity clicks.	|
|missed\_opportunities.conversions	|Object	| Object containing the estimated additional sales the campaign might have generated had it adopted the recommended budget. These are estimates based on previous website traffic and historical campaign performance, not a guarantee of actual sales.	|
|missed\_opportunities.conversions.upper\_bound	|Integer	|Upper bound value for the missed opportunity conversions.	|
|missed\_opportunities.conversions.lower\_bound	|Integer	|Lower bound value for the missed opportunity conversions.	|
|missed\_opportunities.time\_period	|String	|Time period used for calculating *missed_opportunities*.	|

### CampaignSettings object

|Field name	| Type	| Description	|
|---	|---	|---	|
|campaign\_targeting	|String	|Targeting type for the campaign (One of: MANUAL, AUTO).	|
|bidding\_strategy	|String	|Bidding strategy for the campaign (One of: LEGACY\_FOR\_SALES, AUTO\_FOR\_SALES, MANUAL).	|
|start\_date	|String	|Start date of the campaign.	|
|end\_date	|String	|End date for the campaign.	|
|ad\_groups | Array | Array that contains the ad groups associated with a campaign. |
|ad\_groups.adgroup\_id | String | ID of the ad group. |
|ad\_groups.adgroup\_name | String | Name of the ad group. |
|ad\_groups.asins | Array | ASINs associated to the ad group. |
|ad\_groups.bid | Decimal | Bid associated to the ad group. |
|ad\_groups.bid\_optimization | String | Bid optimization strategy for the ad group. |
|ad\_groups.target\_type | String | Targeting type associated with the ad group. |
|ad\_groups.targets | Array | An array containing targets associated with the ad group. |
|ad\_groups.targets.bid | Decimal | Bid associated with a target. |
|ad\_groups.targets.text | String | The keyword text. |
|ad\_groups.targets.match\_type | String | The match type for a keyword target. |
|ad\_groups.targets.targeting | String | The targeting for the bid. |
|ad\_groups.targets.category | Object | Category associated with the targeting expression. |
|ad\_groups.targets.category.id | Integer | ID of the category. |
|ad\_groups.targets.category.name | String | Name of the category. |
|ad\_groups.targets.category.path | String | Path of the category. |
|ad\_groups.targets.expressions | Array | Array of targeting expressions. |
|ad\_groups.targets.expressions.type | String | The type of the targeting expressions. |
|ad\_groups.targets.expressions.value | String | The value of the targeting expressions. |
|ad\_groups.targets.audience\_target\_type | String | For audience targeting, the type of target. |
|budget | Object | Object that contains budget details for the campaign. |
|budget.budget\_rules | Array | An array of budget rules associated with the campaign. |
|budget.budget\_rules.rule\_name | String | Name associated with the rule. |
|budget.budget\_rules.type | String | Type of budget rule. |
|budget.budget\_rules.start\_date | String | Start date of budget rule. |
|budget.budget\_rules.end\_date | String | End date of budget rule. |
|budget.budget\_rules.event\_id | String | Event ID associated with the budget rule. |
|budget.budget\_rules.event\_name | String | Event name associated with the budget rule. |
|budget.budget\_rules.increase\_by | String | Amount to increase the budget by. |
|budget.budget\_rules.performance\_measure\_condition | Object | Condition where the budget rule should take effect. |
|budget.budget\_rules.performance\_measure\_condition.metric\_name | String | Metric used in the condition. |
|budget.budget\_rules.performance\_measure\_condition.comparison\_operator | String | Comparison operator for the condition. |
|budget.budget\_rules.performance\_measure\_condition.threshold | Decimal | Value to be used in the condition. |
|budget.budget\_rules.recurrence | Object | Recurrence settings for the budget rule. |
|budget.budget\_rules.recurrence.recurrence\_type | String | Type of recurrence. |
|budget.budget\_rules.recurrence.days\_of\_week | Array | Days of the week where the budget rule should be active. |

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
          "aws:SourceArn": "arn:aws:sns:us-east-1:084590724871:*"
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
          "aws:SourceArn": "arn:aws:sns:eu-west-1:059061853903:*"
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
          "aws:SourceArn": "arn:aws:sns:us-west-2:489995134625:*"
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

We diagnose the advertiser campaigns and suggest recommendations every week, though it is not guaranteed that every campaign will have an associated recommendation each week. Additionally, we have built feedback loops where we learn from the engagement of advertisers and course correct our future recommendations based on adoption and performance attribution of existing recommendations. 

## Applying recommendations

To take action on a recommendation received from Stream, you can use the tactical recommendations API. For more details, see the [User guide](guides/recommendations/tactical-recommendations/stream-user-guide).