---
title: Sponsored Products keyword recommendations overview
description: Overview of sponsored products keyword recommendations
type: guide
interface: api 
tags:
    - Sponsored Products
keywords:
    - keyword recommendations
    - keyword suggestions
---

# Sponsored Products keyword recommendations

Sponsored Products keyword recommendations allow you to retrieve bid suggestions for keyword targets along with impact metrics for a given list of ASINs or a specified campaign identifier and associated ad group identifier. In the response, recommended keywords are ranked using your chosen sort dimension while providing bid suggestions for the keywords from different themes.

There are currently three versions of the keywords recommendation resource: version 3, version 4, and version 5. If you have currently integrated with either version 3 or version 4, we recommend that you integrate with version 5 as soon as possible.

## Version 3

Version 3 introduced keyword recommendations and allowed the retrieval of recommendations either for a given list of ASINs or a specified campaign identifier and associated ad group identifier. The response supported sorting by either the `clicks`, `conversions`, or `default` dimensions.

## Version 4

Version 4 includes all the features of version 3, but adds search term metrics. Search term impression share indicates the percentage share of all ad-attributed impressions you received on that keyword in the last 30 days. This metric helps advertisers identify potential opportunities based on their share on relevant keywords. Search term impression rank indicates your ranking among all advertisers for the keyword by ad impressions in a marketplace. It tells an advertiser how many advertisers had higher share of ad impressions. Search term information is only available for keywords the advertiser targeted with ad impressions.

## Version 5

Version 5 includes all the features of version 4 and adds theme-based bid recommendations that return improved bid recommendations for each keyword. Theme-based bid recommendations provide *themes* and *impact metrics* along with each bid suggestion to help you choose the right bid for your keyword target. Version 5 also add *impact metrics* that include insights on the increase in clicks and purchase orders with bid values, helping you choose the right bid for your targets. The response includes three suggested bids for each keyword. The resource includes three sets of impact metrics aggregated at the ad group level associated with each of the `low`, `median`, and `high` bids, respectively. 
 
Note that impact metrics are based on historic performance of similar products to provide guidance on choosing a bid, and are not estimates of expected performance. Also, impact metrics are computed on the assumption that a campaign has not run out of budget. Version 5 provides bids along with historic impact metrics of weekly clicks and purchase orders to provide guidance on which bid to choose. The suggested bids are returned at the keyword level, and the impact metrics are aggregated at the ad group level.

The impact metrics reflect performance aggregated across all the recommended keywords in the response. If you like to see the impact metrics only for a select subset of the keywords, you can use keyword recommendations service with this subset only to receive bids and impact metrics for this set. Alternately, you can also request to receive top N keywords when ordered by the dimensions (conversions and clicks) which will return metrics corresponding to only this set of N keywords. 

## Getting started with Sponsored Products keyword recommendations

The POST [/sp/targets/keywords/recommendations](sponsored-products/3-0/openapi/prod#tag/Keyword-Recommendations/operation/getRankedKeywordRecommendation) resource retrieves recommended keyword targets given either a list of ad ASINs or a specified campaign identifier and associated ad group identifier. In the request, specify this choice using the `recommendationType` field.

In the response, keyword recommendations are ranked in descending order of clicks or impressions, depending on the value that the `sortDimension` field is set to in the request. This may be one of `clicks`, `conversions`, or `default`.  

Note that you can include your own keyword targets to be ranked alongside the keyword recommendations by using the `targets` array in your request body.

Finally, use the `locale` field to localize keywords to the specified locale. Supported marketplace to locale mappings can be retrieved using the [/keywords/localize](localization/#/Keyword%20Localization/getLocalizedKeywords) resource. 

## Examples

To get keyword recommendations for a new ad group, sort the keywords by clicks, and use “Dynamic Bidding - Up and Down.”

<!-- >[NOTE] Refer to the information here for keyword recommendations [https://advertising.amazon.com/API/docs/en-us/sponsored-products/3-0/openapi/prod#tag/Keyword-Recommendations/operation/getRankedKeywordRecommendation]. -->

[NOTE] For more information on keyword recommendations, see the [technical specifications for Sponsored Products](sponsored-products/3-0/openapi/prod#tag/Keyword-Recommendations/operation/getRankedKeywordRecommendation).

Request body:

```JSON
{
  "recommendationType": "KEYWORDS_FOR_ASINS",
  "biddingStrategy": "AUTO_FOR_SALES",
  "sortDimension": "CLICKS",
  "asins": [
    "asin1",
    "asin2",
    "asin3" 
  ],
  "bidsEnabled": true
}
```

Response body:

```JSON
{  
  "keywordTargetList": [    
    {      
      "keyword": "keyword1",      
      "bidInfo": [        
        {          
          "matchType": "BROAD",          
          "theme": "CONVERSION_OPPORTUNITIES",          
          "rank": 1,          
          "bid": 75,          
          "suggestedBid": {            
            "rangeStart": 50,            
            "suggested": 75,            
            "rangeEnd": 100          
          }
        },
        {          
          "matchType": "PHRASE",          
          "theme": "CONVERSION_OPPORTUNITIES",          
          "rank": 1,          
          "bid": 79,          
          "suggestedBid": {            
            "rangeStart": 54,            
            "suggested": 79,            
            "rangeEnd": 105          
          }
        },
        {          
          "matchType": "EXACT",          
          "theme": "CONVERSION_OPPORTUNITIES",          
          "rank": 1,          
          "bid": 76,          
          "suggestedBid": {            
            "rangeStart": 60,            
            "suggested": 76,            
            "rangeEnd": 98          
          }
        }
      ],            
      "userSelectedKeyword": false,      
      "searchTermImpressionRank": 0,      
      "searchTermImpressionShare": 0,      
      "recId": "12345ABC"    
    },
    {      
      "keyword": "keyword2",      
      "bidInfo": [        
        {          
          "matchType": "BROAD",          
          "theme": "CONVERSION_OPPORTUNITIES",          
          "rank": 2,          
          "bid": 47,          
          "suggestedBid": {            
            "rangeStart": 34,            
            "suggested": 47,            
            "rangeEnd": 67          
          }
        },
        {          
          "matchType": "PHRASE",          
          "theme": "CONVERSION_OPPORTUNITIES",          
          "rank": 2,          
          "bid": 57,          
          "suggestedBid": {            
            "rangeStart": 43,            
            "suggested": 57,            
            "rangeEnd": 70          
          }
        },
        {          
          "matchType": "EXACT",          
          "theme": "CONVERSION_OPPORTUNITIES",          
          "rank": 2,          
          "bid": 58,          
          "suggestedBid": {            
            "rangeStart": 44,            
            "suggested": 58,            
            "rangeEnd": 70          
          }
        }
      ],            
      "userSelectedKeyword": false,      
      "searchTermImpressionRank": 0,      
      "searchTermImpressionShare": 0,      
      "recId": "12345XYZ"    
    },
  ],  
  "impactMetrics": [    
    {      
      "theme": "CONVERSION_OPPORTUNITIES",      
      "clicks": {        
        "lowRange": {          
          "lower": 10,          
          "upper": 18        
        },        
        "middleRange": {          
          "lower": 22,          
          "upper": 31        
        },        
        "highRange": {          
          "lower": 26,          
          "upper": 34        
        }
      },      
      "orders": {        
        "lowRange": {          
          "lower": 2,          
          "upper": 5        
        },        
        "middleRange": {          
          "lower": 4,        
          "upper": 8        
        },        
        "highRange": {          
          "lower": 5,          
          "upper": 9        
        }
      }
    }
  ]
}
```

To get keyword recommendations for a new ad group, sort the keywords by conversions, use fixed bidding, and see how your own keywords perform against our suggested keywords.

Request body:

```JSON
{
  "recommendationType": "KEYWORD_FOR_ASINS",
  "biddingStrategy": "MANUAL",
  "sortDimension": "CONVERSIONS",
  "asins": [
    "asin1",
    "asin2",
    "asin3"
  ],
  "targets": [
    {
      "keyword": "your own keyword1",
      "matchType": "EXACT"
    },
    {
      "keyword": "your own keyword2"
    }
  ],
  "bidsEnabled": true
}
```

Response:

```JSON
{  
  "keywordTargetList": [    
    {      
      "keyword": "keyword1",      
      "bidInfo": [        
        {          
          "matchType": "BROAD",          
          "theme": "CONVERSION_OPPORTUNITIES",          
          "rank": 1,          
          "bid": 75,          
          "suggestedBid": {            
            "rangeStart": 50,            
            "suggested": 75,            
            "rangeEnd": 100          
          }
        },
        {          
          "matchType": "PHRASE",          
          "theme": "CONVERSION_OPPORTUNITIES",          
          "rank": 1,          
          "bid": 79,          
          "suggestedBid": {            
            "rangeStart": 54,            
            "suggested": 79,            
            "rangeEnd": 105          
          }
        },
        {          
          "matchType": "EXACT",          
          "theme": "CONVERSION_OPPORTUNITIES",          
          "rank": 1,          
          "bid": 76,          
          "suggestedBid": {            
            "rangeStart": 60,            
            "suggested": 76,            
            "rangeEnd": 98          
          }
        }
      ],            
      "userSelectedKeyword": false,      
      "searchTermImpressionRank": 0,      
      "searchTermImpressionShare": 0,      
      "recId": "12345ABC"    
    },
    {      
      "keyword": "your own keyword1",      
      "bidInfo": [        
        {          
          "matchType": "EXACT",          
          "theme": "CONVERSION_OPPORTUNITIES",          
          "rank": 2,          
          "bid": 29,          
          "suggestedBid": {            
            "rangeStart": 27,            
            "suggested": 29,            
            "rangeEnd": 34          
          }
        }
      ],            
      "userSelectedKeyword": true,      
      "searchTermImpressionRank": 0,      
      "searchTermImpressionShare": 0,      
      "recId": "54321DEF"    
    },
    {      
      "keyword": "keyword2",      
      "bidInfo": [        
        {          
          "matchType": "BROAD",          
          "theme": "CONVERSION_OPPORTUNITIES",          
          "rank": 3,          
          "bid": 47,          
          "suggestedBid": {            
            "rangeStart": 34,            
            "suggested": 47,            
            "rangeEnd": 67          
          }
        },
        {          
          "matchType": "PHRASE",          
          "theme": "CONVERSION_OPPORTUNITIES",          
          "rank": 3,          
          "bid": 57,          
          "suggestedBid": {            
            "rangeStart": 43,            
            "suggested": 57,            
            "rangeEnd": 70          
          }
        },
        {          
          "matchType": "EXACT",          
          "theme": "CONVERSION_OPPORTUNITIES",          
          "rank": 3,          
          "bid": 58,          
          "suggestedBid": {            
            "rangeStart": 44,            
            "suggested": 58,            
            "rangeEnd": 70          
          }
        }
      ],            
      "userSelectedKeyword": false,      
      "searchTermImpressionRank": 0,      
      "searchTermImpressionShare": 0,      
      "recId": "12345XYZ"    
    },
    {      
      "keyword": "your own keyword2",      
      "bidInfo": [        
        {          
          "matchType": "BROAD",          
          "theme": "CONVERSION_OPPORTUNITIES",          
          "rank": 4,          
          "bid": 146,          
          "suggestedBid": {            
            "rangeStart": 86,            
            "suggested": 146,            
            "rangeEnd": 226          
          }
        }
      ],            
      "userSelectedKeyword": true,      
      "searchTermImpressionRank": 0,      
      "searchTermImpressionShare": 0,      
      "recId": "56789HIJ"    
    },
  ],  
  "impactMetrics": [    
    {      
      "theme": "CONVERSION_OPPORTUNITIES",      
      "clicks": {        
        "lowRange": {          
          "lower": 10,          
          "upper": 18        
        },        
        "middleRange": {          
          "lower": 22,          
          "upper": 31        
        },        
        "highRange": {          
          "lower": 26,          
          "upper": 34        
        }
      },      
      "orders": {        
        "lowRange": {          
          "lower": 2,          
          "upper": 5        
        },        
        "middleRange": {          
          "lower": 4,        
          "upper": 8        
        },        
        "highRange": {          
          "lower": 5,          
          "upper": 9        
        }
      }
    }
  ]
}
```

To get two keyword recommendations for an existing ad group and sort the keywords using the default ranking heuristic, but do not want bid suggestions.

Request body:

```JSON
{
  "recommendationType": "KEYWORD_FOR_ASINS",
  "maxRecommendations": 2,
  "campaignId": "A12345678",
  "adGroupId": "B12345678",
  "bidsEnabled": false
}
```

Response:

```JSON
{  
  "keywordTargetList": [    
    {      
      "keyword": "keyword1",      
      "bidInfo": [        
      ],            
      "userSelectedKeyword": false,      
      "searchTermImpressionRank": 0,      
      "searchTermImpressionShare": 0,      
      "recId": "12345ABC"    
    },
    {      
      "keyword": "keyword2",      
      "bidInfo": [        
      ],            
      "userSelectedKeyword": false,      
      "searchTermImpressionRank": 0,      
      "searchTermImpressionShare": 0,      
      "recId": "12345XYZ"    
    },
  ],  
  "impactMetrics":null
}
```
