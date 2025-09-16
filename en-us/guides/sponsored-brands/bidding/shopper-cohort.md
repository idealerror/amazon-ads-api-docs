---
title: Shopper cohort bidding
description: Increase your bids on AMC audiences in your Sponsored Brands campaigns
type: guide
interface: api 
tags:
    - Sponsored Brands
keywords:
    - Shopper cohort bidding
---
# Shopper Cohort bidding

## Before you begin

* Complete the Amazon Ads API [onboarding](onboarding/overview) and [getting started](getting-started/overview) processes to obtain your access token and profile ID. You will need these to make all the calls referenced in the tutorial.
* Understand the structure of [Sponsored Brands campaigns](sponsored-brands/campaigns/structure).

> [NOTE] If you want to test creating Sponsored Brands campaigns without worrying about ad spend, create a [test account](https://advertising.amazon.com/API/docs/en-us/test-accounts/overview) and use it to complete this tutorial.

## Overview

Sponsored Brands new bidding control allows you to adjust bids and engage with audiences created in Amazon Marketing Cloud (AMC), a privacy-safe cloud-based clean room solution. For example, using the bidding controls, you can single out shoppers who clicked a certain product but did not purchase it, and then selectively modify the bids for these shoppers within your campaigns. Afterwards, you can view the campaign performance for these shoppers using our reporting tools.

## Check the availability of AMC audience groups

For more guidance on creating an AMC audience group, read our [audience management service guide](guides/amazon-marketing-cloud/audiences/audience-management-service).

You can grab a list of targetable AMC audiences through the [Audience Discovery API](targetable-entities):

#### Sample request: POST targetableEntities/list

```
{
    "adProduct": "SPONSORED_BRANDS"
}
```

#### Sample response

```
{
    "targetableEntities": [
        {
            "audienceId": "407242961697740599",
            "audienceResolved": "AMC Lookalike from HVA - MostBroadLookalike",
            "audienceTooltip": "No description provided.",
            "path": [
                "Audience Category",
                "Custom-built",
                "AMC"
            ],
            "targetType": "AUDIENCE"
        },
        {
            "audienceId": "370624265845962462",
            "audienceResolved": "AMC - Air Fryer - Added to wishlist, no purchase P183 Days",
            "audienceTooltip": "Shoppers who added an Air Fryer ASIN to their wish list in the past 183 days, but have not purchased.",
            "path": [
                "Audience Category",
                "Custom-built",
                "AMC"
            ],
            "targetType": "AUDIENCE"
        }
    ],
    "totalResults": 2
}
```

>[TIP:Amazon-built audiences]Advertisers can also use audiences built by Amazon for bidding. Amazon-built audiences can be retrieved from the audience discovery API using a `pathsFilter` in the request, with the value `[["Audience Category","Custom-built","Product"]]`.

## Using sponsored cohort bidding

After finding a targetable audience, reference it when [creating a campaign](sponsored-brands/campaigns/get-started-with-campaigns):

#### Sample request: creating a campaign

```
{
    "campaigns": [
        {
            "budgetType": "DAILY",
            "budget": 20,
            "name": "TEST",
            "state": "ENABLED",
            "bidding": {
                "bidOptimization": false,
                "shopperCohortBidAdjustments": [
                    {
                        "percentage": 50,
                        "shopperCohortType": "AUDIENCE_SEGMENT",
                        "audienceSegments": [
                            {
                                "audienceSegmentType": "SPONSORED_ADS_AMC",
                                "audienceId": "401111161697740599"
                            }
                        ]
                    }
                ]
            }
        }
    ]
}
```

## FAQs

#### 1. Can an advertiser add a new audience group to the campaign?

We only allow one audience group per one campaign. If you want to adjust the bid for a new segment, you either need to create a new campaign for this new segment or replace the existing segment in the existing campaign with the new segment.

#### 2. How can advertisers compare the performance of their AMC audience bidding groups to general shoppers on Amazon?

Access the Amazon Marketing Cloud tab in the measurement and reporting section. Then, select the instance and advertiser to run a query with. Finally, download the report from the AMC reporting tab.

Sample query:

```
SELECT
    matched_behavior_segment_ids, ad_product_type,campaign, sum(impressions) as impressions ,
    COUNT(1) AS segment_count
FROM
    sponsored_ads_traffic
WHERE
    matched_behavior_segment_ids IS NULL
GROUP BY
    matched_behavior_segment_ids,ad_product_type, campaign
LIMIT 50;
```

#### 3. Can advertisers update the bid adjustment percentage?

You can update the segment and bid adjustment percentage through the API.

#### Sample request: update the bid adjustment percentage in an existing campaign

```
{
    "campaigns": [
        {
            "campaignId": "50123123123",
            "bidding": {
                "bidOptimization": false,
                "shopperCohortBidAdjustments": [
                    {
                        "audienceSegments": [
                            {
                                "audienceId": "40711212312312399",
                                "audienceSegmentType": "SPONSORED_ADS_AMC"
                            }
                        ],
                        "percentage": 500,
                        "shopperCohortType": "AUDIENCE_SEGMENT"
                    }
                ]
            }
        }
    ]
}
```

#### 4. How does AMC audience bidding work with cost control in Sponsored Brands?

You can use both cost control and shopper cohort bidding in Sponsored Brands to optimize the cost while engaging with a specific shopper cohort. If you choose to adjust bids for an AMC shopper segment while cost control is enabled, the expected cost target of this AMC segment is adjusted accordingly. This allows you to value certain shopper cohorts while still controlling your campaign cost. For example, if you set the cost control for a campaign at $2.0, and then increase the bid for an AMC segment by 50%, the target cost KPI for this segment will ultimately be adjusted to $2.0 x (1+50%), or $3.0.

#### 5. How does AMC audience bidding work with placement level bidding controls in Sponsored Brands?

You can use both placement level bidding and shopper cohort bidding in Sponsored Brands to optimize the placement performance for specific shopper cohorts. If you choose to adjust bids for an AMC shopper segment while placement level bidding is in place, the expected bid target of your AMC segment will be adjusted accordingly. For example, if you set the placement level bidding for a campaigns at $2.0 and then increase the bid for a `DETAIL_PAGE` placement by 40, the API will increase the bid to $2.0 x (1+40%) = $2.80. If you then adjust the bid for your AMC segment by 50%, the target bid for this segment will ultimately increase to $2.8 x (1+50%) = $4.20.
