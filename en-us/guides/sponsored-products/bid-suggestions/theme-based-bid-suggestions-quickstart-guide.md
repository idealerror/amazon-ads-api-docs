---
title: Theme-based bid suggestions - Quick-start guide
description: A guide to using theme-based bid suggestions for Sponsored Products in the Amazon Ads API
type: guide
interface: api 
tags:
    - Sponsored Products
keywords:
    - bid suggestions
    - impact metrics
---

# Theme-based bid suggestions - Quick-start guide

## Overview

Theme-based bid suggestions delivers suggestions and impact metrics for multiple themes, allowing you to choose a bid from a theme that is best aligned with your campaign. 

The first theme released offers bid suggestions to generate sales, and the impact metrics are weekly clicks and purchase orders received by similar products. The bid suggestions are calculated from a group of recent winning bids for similar products, updated periodically to reflect the marketplace changes. Bid suggestions will use the local currency of the marketplace that the API endpoint is associated with. 

>[NOTE]The API currently supports bid suggestions for automatic targeting and keyword targeting. Support for product targeting will be added in the future. 

## Impact metrics

With these new impact metrics, you can look at the increase in clicks and purchase orders with bid values to choose the right bid for your targets. 

To receive the metrics for keyword targeting, the API must be called with a minimum number of five keywords per ad group. The response includes three suggested bids for each keyword. In addition, the response includes three sets of impact metrics aggregated at the ad group level associated with each of the low, median, and high bids, respectively. 

The request and response for automatic targeting works similarly, but there is no minimum requirement on the number of match types selected. The impact metrics are based on historic performance of similar products to provide guidance on choosing a bid, and are not estimates of expected performance. They are also computed assuming your campaign does not run out of budget. 

## Frequently asked questions

#### How is theme-based bid suggestions different from the current bid suggestions? 

The current bid suggestion service provides bids with no supporting impact metrics. Theme-based bid suggestions provides bids along with historic impact metrics of weekly clicks and purchase orders to provide guidance on which bid to choose. The suggested bids are returned at the targeting clause level, and the impact metrics are aggregated at the ad group level.

#### Can the current bid suggestion service continue to be used?

Theme-based bid suggestions is being released in open beta now. The current service will be deprecated in the future, but we will provide months of advance notice before this happens. During this period, both the current service and the new theme-based bid suggestions will be active and supported. However, we will no longer release any feature improvements in the current bid suggestion service. We encourage you to try out the new theme-based bid suggestions and benefit from the improved functionality.

#### What happens to the campaigns that previously adopted a bid suggestion?

Theme-based bid suggestions supports both new campaigns and existing campaigns. Bids that are already set in campaigns will not be impacted by the new suggestions. You would need to adopt the bids from theme-based bid suggestions to impact campaigns.

#### Are there new reports that need to be used with this feature?

No, there are no new reports that need to be used with this feature. You can use the Search Term Report, Campaign Performance Report, and Impression Rank report to get the regular advertising metrics you access today.

## Code Samples

`SP campaigns: Name - ``ThemeBasedBidRecommendation``, URL - /sp/targets/bid/recommendations, Method – POST`

### Scenario 1: You want to receive bid suggestions and impact metrics for a new automatic targeting campaign.

#### Request body

```
{
  "recommendationType": "BIDS_FOR_NEW_AD_GROUP",
  "asins": [
    "asin1",
    "asin2"
  ],
  "targetingExpressions": [
    {
      "type": "CLOSE_MATCH"
    },
    {
      "type": "LOOSE_MATCH"
    },
    {
      "type": "SUBSTITUTES"
    },
    {
      "type": "COMPLEMENTS"
    }
  ],
  "bidding": {
    "strategy": "AUTO_FOR_SALES",
    "adjustments": null
  }
}
```


#### Scenario 1 Response

```
{
  "bidRecommendations": [
    {
      "theme": "CONVERSION_OPPORTUNITIES",
      "bidRecommendationsForTargetingExpressions": [
        {
          "targetingExpression": {
            "type": "CLOSE_MATCH",
            "value": null
          },
          "bidValues": [
            {
              "suggestedBid": 0.14
            },
            {
              "suggestedBid": 0.44
            },
            {
              "suggestedBid": 0.78
            }
          ]
        },
        {
          "targetingExpression": {
            "type": "COMPLEMENTS",
            "value": null
          },
          "bidValues": [
            {
              "suggestedBid": 0.33
            },
            {
              "suggestedBid": 0.61
            },
            {
              "suggestedBid": 0.96
            }
          ]
        },
        {
          "targetingExpression": {
            "type": "LOOSE_MATCH",
            "value": null
          },
          "bidValues": [
            {
              "suggestedBid": 0.13
            },
            {
              "suggestedBid": 0.14
            },
            {
              "suggestedBid": 0.54
            }
          ]
        },
        {
          "targetingExpression": {
            "type": "SUBSTITUTES",
            "value": null
          },
          "bidValues": [
            {
              "suggestedBid": 0.14
            },
            {
              "suggestedBid": 0.31
            },
            {
              "suggestedBid": 0.52
            }
          ]
        }
      ],
      "impactMetrics": {
        "clicks": {
          "values": [
            {
              "lower": 30,
              "upper": 70
            },
            {
              "lower": 80,
              "upper": 90
            },
            {
              "lower": 95,
              "upper": 120
            }
          ]
        },
        "orders": {
          "values": [
            {
              "lower": null,
              "upper": null
            },
            {
              "lower": 7,
              "upper": 10
            },
            {
              "lower": 11,
              "upper": 15
            }
          ]
        }
      }
    }
  ]
} 
```



### Scenario 2:  You want to receive bid suggestions and impact metrics for a new keyword targeting campaign.

#### Request body

```
{
  "recommendationType": "BIDS_FOR_NEW_AD_GROUP",
  "asins": [
    "asin1",
    "asin2"
  ],
  "targetingExpressions": [
    {
      "type": "KEYWORD_BROAD_MATCH",
      "value": "sandals"
    },
    {
      "type": "KEYWORD_BROAD_MATCH",
      "value": "shoe"
    },
    {
      "type": "KEYWORD_EXACT_MATCH",
      "value": "basketball"
    },
    {
      "type": "KEYWORD_PHRASE_MATCH",
      "value": "net"
    },
    {
      "type": "KEYWORD_PHRASE_MATCH",
      "value": "phone"
    }    
  ],
  "bidding": {
    "strategy": "AUTO_FOR_SALES",
    "adjustment": null
  }
}
```


#### Scenario 2 response

```
{
  "bidRecommendations": [
    {
      "theme": "CONVERSION_OPPORTUNITIES",
      "bidRecommendationsForTargetingExpressions": [
        {
          "targetingExpression": {
            "type": "KEYWORD_BROAD_MATCH",
            "value": "sandals"
          },
          "bidValues": [
            {
              "suggestedBid": 0.15
            },
            {
              "suggestedBid": 0.45
            },
            {
              "suggestedBid": 0.77
            }
          ]
        },
        {
          "targetingExpression": {
            "type": "KEYWORD_BROAD_MATCH",
            "value": "shoe"
          },
          "bidValues": [
            {
              "suggestedBid": 0.14
            },
            {
              "suggestedBid": 0.44
            },
            {
              "suggestedBid": 0.78
            }
          ]
        },
        {
          "targetingExpression": {
            "type": "KEYWORD_EXACT_MATCH",
            "value": "basketball"
          },
          "bidValues": [
            {
              "suggestedBid": 0.33
            },
            {
              "suggestedBid": 0.61
            },
            {
              "suggestedBid": 0.96
            }
          ]
        },
        {
          "targetingExpression": {
            "type": "KEYWORD_PHRASE_MATCH",
            "value": "net"
          },
          "bidValues": [
            {
              "suggestedBid": 0.28
            },
            {
              "suggestedBid": 0.55
            },
            {
              "suggestedBid": 0.87
            }
          ]
        },
        {
          "targetingExpression": {
            "type": "KEYWORD_PHRASE_MATCH",
            "value": "phone"
          },
          "bidValues": [
            {
              "suggestedBid": 0.13
            },
            {
              "suggestedBid": 0.14
            },
            {
              "suggestedBid": 0.54
            }
          ]
        }
      ],
      "impactMetrics": {
        "clicks": {
          "values": [
            {
              "lower": 30,
              "upper": 70
            },
            {
              "lower": 80,
              "upper": 90
            },
            {
              "lower": 95,
              "upper": 120
            }
          ]
        },
        "orders": {
          "values": [
            {
              "lower": null,
              "upper": null
            },
            {
              "lower": 7,
              "upper": 10
            },
            {
              "lower": 11,
              "upper": 15
            }
          ]
        }
      }
    }
  ]
}
```

### Scenario 3: You want to receive bid suggestions and impact metrics for an existing automatic targeting ad-group.

#### Request body

```
{
  "recommendationType": "BIDS_FOR_EXISTING_AD_GROUP",
  "targetingExpressions": [
    {
      "type": "CLOSE_MATCH"
    },
    {
      "type": "COMPLEMENTS"
    },
    {
      "type": "LOOSE_MATCH"
    },
    {
      "type": "SUBSTITUTES"
    }
  ],
  "campaignId": "12345678XYZ",
  "adGroupId": "987654321XYZ"
}
```

#### Scenario 3 response

Same as [scenario 1 response](#scenario-1-response).

### Scenario 4: You want to receive bid suggestions and impact metrics for an existing keyword targeting ad group.

#### Request body

```
{
  "recommendationType": "BIDS_FOR_EXISTING_AD_GROUP",
  "targetingExpressions": [
    {
      "type": "KEYWORD_BROAD_MATCH",
      "value": "sandals"
    },
    {
      "type": "KEYWORD_BROAD_MATCH",
      "value": "shoe"
    },
    {
      "type": "KEYWORD_EXACT_MATCH",
      "value": "basketball"
    },
    {
      "type": "KEYWORD_PHRASE_MATCH",
      "value": "net"
    },
    {
      "type": "KEYWORD_PHRASE_MATCH",
      "value": "phone"
    }
  ],
  "campaignId": "12345678XYZ",
  "adGroupId": "987654321XYZ"
}
```

#### Scenario 4 response

Same as [scenario 2 response](#scenario-2-response).
