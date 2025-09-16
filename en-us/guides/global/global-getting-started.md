---
title: Getting started with the Global Campaign Management API
description: A guide for using the Global Campaign Management API
type: guide
interface: api
tags: 
    - Global
keywords:
    - guide
---

# Getting started with the Global Campaign Management API

## Overview

Use the global campaign management APIs to create and manage Sponsored Products campaigns that target multiple countries at once (e.g. a single global campaign can target US, UK and Germany). The global campaign management API provides the right balance of efficiency and flexibility, allowing you to set common global defaults while still customizing parameters specific to each country. For example, you may set the campaign name along with some keywords at the global level while adjusting the bids and budgets for each country. Global campaigns for Sponsored Products are available for vendors, sellers, and Kindle Direct Publishing authors in all countries where Amazon Ads is available. 

## Getting Started

### Global Campaigns structure

Creating a global campaign requires making multiple requests to create the entities included in your campaign hierarchy. Both manual and auto campaigns require you to create — at minimum:

* campaigns
* ad groups
* product ads

However, for manual campaigns, you have complete control over your [keywords](guides/sponsored-products/keywords/overview) and product targeting expressions. You can create both keyword targets or product targeting expressions using a single targeting API. For auto campaigns, Amazon suggests relevant targeting expressions and keywords based on the products you are advertising. 

For each entity in the campaign hierarchy, you are required to specify the countries the entity will target. Some attributes, such as the campaign name, start and end dates, targeting type, and bidding strategy, are set at the global level. The global values are then automatically inherited by each target country. These attributes cannot be set at a country level. Other attributes, like keywords, *can* be set at global level, but you can also choose to override the value within each country. Attributes that do not have global values, like bids and budgets, must be set at the country level.

Once a global campaign is created, you can extend it to new countries by adding new country codes to the campaign entity. You will be required to either extend existing sub-entities to the new countries or create additional sub-entities that target new countries.   

#### Auto campaigns

For **auto** campaigns, you only need to create a **campaign**, **ad group**, and **product ad**. The API then automatically assigns relevant targeting expressions and keywords to your campaign.

The following diagram shows an example of an auto global campaign that targets the US, UK and Germany. The ad group has three product ads: 

1. one product ad targeting US, UK and Germany
2. one product ad targeting the US and UK
3. one product ad that only targets Germany

The ad group contains one auto-targeting expression and one negative keyword targeting all three countries.

![](/_images/global/campaign-hierarchy-at.jpg)


#### Manual campaigns

For **manual** campaigns, you also need to create a **campaign**, **ad group**, and **product ad**, but they must also include at least one **product targeting expression** or **keyword** in each ad group. The diagram below contains a manual global campaign with keyword targeting

![keyword targeting only](/_images/global/campaign-hierarchy-mt-keyword.jpg)

This second diagram shows an example campaign with product and category targeting:

![product and category targeting](/_images/global/campaign-hierarchy-mt-product-targeting.jpg)

### Get started with auto campaigns

#### Steps

1. Create a campaign using the [POST global/campaigns](global-cm#tag/Campaigns/operation/CreateCampaignsOperation) endpoint. To create an auto campaign, make sure `targetingSetting` is set to `AUTO`

2. Create at least one ad group using the [POST global/adGroups](global-cm#tag/AdGroups/operation/CreateAdGroupsOperation) endpoint. Each ad group should contain one or more products that share similar characteristics (e.g., price, category, genre)

>[NOTE] In this release only one Ad Group can be created under a global campaign.

3. Create at least one product ad per ad group using the [POST global/productAds](global-cm#tag/Product%20Ads/operation/CreateProductAdsOperation) endpoint. 


>[NOTE] Please note that in this release, we are supporting only single country Product Ads. Currently one global Product Ad can only target one country at a time. In order to create a complete auto-campaign, create one Product Ad for each target country under the campaign.

4. (Optional) Set up auto targeting using the [PUT global/targets](global-cm#tag/Targets/operation/UpdateTargetsOperation) endpoint. Auto targeting allows you to set bids for targeting groups based on the relationship to the products you are advertising. You can set different bids for search terms that are close or loose matches, or product detail pages that are substitutes or complements to your products. If you choose to use auto targeting expressions, the bids you set for each expression replace the default bid for the ad group.

5. (Optional) Set up negative targeting. You can use negative keyword targeting or negative product targeting to stop ads from showing on certain search results, brands, or product detail pages. Both types of negative targeting are available at the ad group created using [POST global/targets](global-cm#tag/Targets/operation/CreateTargetsOperation) endpoint.

Once you have created at least one active campaign, ad group, and product ad, your campaign is considered complete and ready to run. 

### Get started with manual campaigns

Manual campaigns provides you with full control over targeting conditions, including targeting expressions and keywords. 

#### Steps

1. Create a campaign using the [POST global/campaigns](global-cm#tag/Campaigns/operation/CreateCampaignsOperation) endpoint. To create an auto campaign, make sure `targetingSetting` is set to manual.

>[NOTE] In this release only one Ad Group can be created under a global campaign.

2. Create at least one ad group using the [POST global/adGroups](global-cm#tag/AdGroups/operation/CreateAdGroupsOperation) endpoint. Each ad group should contain one or more products that share similar characteristics (e.g., price, category, genre)

3. Create at least one product ad per ad group using the [POST global/productAds](global-cm#tag/Product%20Ads/operation/CreateProductAdsOperation) endpoint. 

>[NOTE] Please note that in this release, we only support single country product ads. Currently one global Product Ad can only target one country at a time. In order to create a complete auto-campaign, create one Product Ad for each target country under the campaign.

4. Add targeting to the ad group. Ad groups for manual campaigns must have at least one associated targeting expression (product or category) or keyword. You can create a product targeting or one or more category targeting expressions or a keyword using the [POST global/targets](global-cm#tag/Targets/operation/CreateTargetsOperation) endpoint. 


>[NOTE] Please note that in this release, we only support single country targets. Currently one global Target can only target one country at a time. In order to create a complete auto-campaign, create one Target for each target country under the campaign. Read more information on [keyword](#keyword-targeting) and [product and category targeting](#product-and-category-targeting) expressions.


5. (Optional) Set up negative targeting. You can use negative keyword targeting or negative product targeting to stop ads from showing on certain search results, brands, or product detail pages. Both types of negative targeting are available at the ad group created using [POST global/targets](global-cm#tag/Targets/operation/CreateTargetsOperation) endpoint.


>[NOTE] See [more information on negative keywords](#negative-keywords) and [more information on negative product targeting](#negative-product-and-brand-targeting)


Once you have created at least one active campaign, ad group, product ad, and targeting expression or keyword, your campaign is considered complete and ready to run. 

### Campaigns

Campaigns are the highest level of the global campaign entity hierarchy. Budget, targeting type, start date, and end date are all defined at the campaign level.

#### Request

**Endpoint**

[POST global/campaigns](global-cm#tag/Campaigns/operation/CreateCampaignsOperation)

**Parameters**

|Name	|Description	|
|---	|---	|
|globalContext	|Option are either GLOBAL or SINGLE_COUNTRY. globalContext indicates whether the campaign is a global campaign or a single country campaigns. Currently, this API only supports global campaigns.	|
|countryCodes	|Countries targeted by your global campaign	|
|adProduct	|Currently the only allowed value is SPONSORED_PRODUCTS	|
|name	|A name for the campaign	|
|budgetCaps.countryMonetaryBudgetSetting	|Budget amount for each target country. You are required to provide budget amount for every country targeted by your global campaign	|
|startDate	|The start date for the campaign in the form YYYY-MM-DD.. This paramter is a global value but campaign will start in each target country according to default timezone of that country. 	|
|endDate	|The end date for the campaign. If not specified, the campaign will not have an end date. This paramter is a global value but campaign will end target country according to default timezone of that county	|
|targetingSetting	|Options are either `MANUAL` or `AUTO`	|
|state.value	|Options are ENABLED, PAUSED or ARCHIVED. If you don't want your campaign to serve in any of the target countries right away, set state to `PAUSED`. Once your archive a campaign, you cannot update it.	|
|optimizations.bidStrategy	|Bidding strategy for the campaign. Possible values are `MANUAL`, `SALES_UP_AND_DOWN`, `SALES_DOWN_ONLY`, `RULE_BASED`, `NEW_TO_BRAND_UP_AND_DOWN`, `SPEND_BUDGET_UP_AND_DOWN`, `PERFORMANCE_UP_AND_DOWN`	|
|optimizations.bidAdjustments.bidAdjustmentPercentage	|The percentage you want to adjust bids for the specified placement	|
|optimizations.bidAdjustments.bidAdjustmentType	|The Amazon.com placement you want to adjust the bid for. Possible values are `PLACEMENT_TOP, PLACEMENT_PRODUCT_PAGE`, `PLACEMENT_REST_OF_SEARCH`, `MOBILE_BID_ADJUSTMEN`T	|

**Example requests**

**Auto campaign**

```
curl --location 'https://advertising-api.amazon.com/global/campaigns' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxxx' \
--header 'Amazon-Ads-AccountId: amzn1.ads-account.g.xxxxxxxxxxxx' \
--header 'Content-Type: application/vnd.globalcampaign.v1+json' \
--header 'Accept: application/vnd.globalcampaign.v1+json' \
--data '{
    "campaigns" : [
        {
            "targetingSetting": "AUTO",
            "name": "`Global Campaign for Shoes - Auto Targeting`",
            "startDate": "2024-04-22",
            "endDate": "2025-04-12",
            "optimizations": {
                "bidStrategy": "MANUAL",
                "bidAdjustments": [
                    {
                        "bidAdjustmentType": "PLACEMENT_TOP",
                        "bidAdjustmentPercentage": 25
                    }
                ],
                "isPremiumBidEnabled": false
            },
            "tags": [
                {
                    "name": "product",
                    "value": "shoes"
                }
            ],
            "globalContext": "GLOBAL",
            "countryCodes": ["US", "UK", "JP"],
            "state": {
                "value": "ENABLED"
            },
            "budgetCaps": {
                "countryMonetaryBudgetSetting": {
                    "US" : {
                        "value": 30
                    },
                    "UK": {
                        "value": 45
                    },
                    "JP": {
                        "value": 120
                    }
                }
            }
        }
    ]
}'
```



**Manual campaign**

```
curl --location 'https://advertising-api.amazon.com/global/campaigns' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxxx' \
--header 'Amazon-Ads-AccountId: amzn1.ads-account.g.xxxxxxxxxxxx' \
--header 'Content-Type: application/vnd.globalcampaign.v1+json' \
--header 'Accept: application/vnd.globalcampaign.v1+json' \
--data '{
    "campaigns" : [
        {
            "targetingSetting": "MANUAL",
            "name": "Global Campaign for Shoes - Manual Targeting",
            "startDate": "2024-04-22",
            "endDate": "2025-04-12",
            "optimizations": {
                "bidStrategy": "MANUAL",
                "bidAdjustments": [
                    {
                        "bidAdjustmentType": "PLACEMENT_TOP",
                        "bidAdjustmentPercentage": 25
                    }
                ],
                "isPremiumBidEnabled": false
            },
            "tags": [
                {
                    "name": "product",
                    "value": "shoes"
                }
            ],
            "globalContext": "GLOBAL",
            "countryCodes": ["US", "UK", "JP"],
            "state": {
                "value": "ENABLED"
            },
            "budgetCaps": {
                "countryMonetaryBudgetSetting": {
                    "US" : {
                        "value": 30
                    },
                    "UK": {
                        "value": 45
                    },
                    "JP": {
                        "value": 120
                    }
                }
            }
        }
    ]
}'
```

#### Response

```
{
  "resultStatus": "SUCCESS",
  "campaignWriteResponses": [
    {
      "errors": [],
      "campaign": {
        "id": "5029975192109995498",
        "name": "Global Campaign for Shoes - Manual Targeting",
        "adProduct": "SPONSORED_PRODUCTS",
        "startDate": "2024-04-22",
        "endDate": "2025-04-12",
        "budgetCaps": {
          "countryMonetaryBudgetSetting": {
            "US": {
              "value": `30`
            },
            "UK": {
              "value": 45
            },
            "DE": {
              "value": 125
            }
          }
        },
        "optimizations": {
          "bidStrategy": "MANUAL",
          "bidAdjustments": [
            {
              "bidAdjustmentType": "PLACEMENT_TOP",
              "bidAdjustmentPercentage": 25
            }
          ],
          "isPremiumBidEnabled": true
        },
        "tags": [
          {
            "name": "product",
            "value": "shoes"
          }
        ],
        "countryCodes": [
          "US",
          "UK",
          "DE"
        ],
        "state": {
          "value": "ENABLED"
        },
        "globalContext": "GLOBAL",
        "targetingSetting": "MANUAL"
      },
      "resultStatus": "SUCCESS"
    }
  ]
}
```



### Ad Groups

#### Creating ad groups

Ad groups provide the ability to organize, manage, and track performance of the products within your campaign. Products placed together in an ad group share the same bids and targeting (keyword or product). Each campaign consists of one or more ad groups

#### Before you begin

In order to create an ad group, you must first create a campaign and save your `campaignId`. For more information on creating global campaigns, read the [global campaign creation](#campaigns) section.


#### **Request**

**Endpoint**
[POST global/adGroups](global-cm#tag/AdGroups/operation/CreateAdGroupsOperation)

**Parameters**

|Name	|Description	|
|---	|---	|
|globalContext	|Option are either GLOBAL or SINGLE_COUNTRY. globalContext indicates whether the campaign is a global campaign or a single country campaigns. 	|
|countryCodes	|Countries targeted by your global ad group	|
|adProduct	|Currently the only allowed value is SPONSORED_PRODUCTS	|
|name	|A name for the ad group	|
|campaignId	|The campaign to associate with the ad group	|
|bidValue.countryDefaultBidSetting	|Bid amount for each target country. You are required to provide bid amount for every target country. The default bid amount applies to all child entities (keywords, targeting expressions, etc.) of the ad group if you don't define bids for the child entities explicitly. For instance, if a keyword child entity doesn't include a defined bid, the keyword assumes the default bid for the ad group.	|
|state.value	|Options are ENABLED, PAUSED or ARCHIVED. If you don't want your campaign to serve in any of the target countries right away, set state to `PAUSED`.	|

**Example request**

```
curl --location 'https://advertising-api.amazon.com/global/adGroups' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxxx' \
--header 'Amazon-Ads-AccountId: amzn1.ads-account.g.xxxxxxxxxxxx' \
--header 'Content-Type: application/vnd.globaladgroup.v1+json' \
--header 'Accept: application/vnd.globaladgroup.v1+json' \
--data '{
  "adGroups": [
    {
      "name": "`NikeAirJordan2024EditionAdGroup-1`",
      "bidValue": {
        "countryDefaultBidSetting": {
                  "US": {
                     "value": 0.98
                  },
                  "UK": {
                     "value": 0.57
                  },
                  "DE": {
                     "value": 0.47
                  }
               }
      },
      "countryCodes": ["US", "UK", "JP"],
      "state": {
        "value": "ENABLED"
      },
      "globalContext": "GLOBAL",
      "campaignId": "5029975192109995498"
    }
  ]
}'
```

#### Response

```
{
   "resultStatus": "SUCCESS",
   "adGroupWriteResponses": [
      {
         "errors": [],
         "adGroup": {
            "id": "5030047411409185865",
            "name": "`NikeAirJordan2024EditionAdGroup-1`",
            "adProduct": "SPONSORED_PRODUCTS",
            "bidValue": {
               "countryDefaultBidSetting": {
                  "US": {
                     "value": 0.98
                  },
                  "UK": {
                     "value": 0.57
                  },
                  "DE": {
                     "value": 0.47
                  }
               }
            },
            "campaignId": "5029975192109995498",
            "countryCodes": [
               "US",
               "UK",
               "DE"
            ],
            "state": {
               "value": "ENABLED"
            },
            "globalContext": "GLOBAL"
         },
         "resultStatus": "SUCCESS"
      }
   ]
}
```

### Product Ads

Product ads are the smallest unit of your campaign and represent the individual products that you want to advertise. Each product ad is associated to a single product, either by SKU (sellers) or ASIN (vendors and authors). Each product ad is associated to an ad group 

#### Before you begin

* [Create a campaign](#campaigns)
* [Create an ad group](#ad-groups)

#### Request

**Endpoint**
[POST global/productAds](global-cm#tag/Product%20Ads/operation/CreateProductAdsOperation)

**Parameters**

|Name	|Description	|
|---	|---	|
|globalContext	|Option are either GLOBAL or SINGLE_COUNTRY. globalContext indicates whether the campaign is a global campaign or a single country campaigns. 	|
|countryCodes	|Countries targeted by your global product ad	|
|adProduct	|Currently the only allowed value is SPONSORED_PRODUCTS	|
|name	|A name for the ad	|
|campaignId	|The campaign to associate with the product ad	|
|adGroupId	|The ad group to associate with the product ad	|
|adSubject.adSubjectType	|Possible values are SELLER or VENDOR	|
|adSubject.adSubjectValue.vendorAdSubject.countryVendorAdSubjectSettings or adSubject.adSubjectValue.vendorAdSubject.countrySellerAdSubjectSettings	|For each country provide the value of ASIN or SKU 	|
|state.value	|Options are ENABLED, PAUSED or ARCHIVED. If you don't want your campaign to serve in any of the target countries right away, set state to `PAUSED`.	|

**Example request**


>[NOTE] Please note that in this release, we are supporting only single country Product Ads. Currently one global Product Ad can only target one country at a time. Below example shows create request for Product Ads targeting JP. In order to create Ads in US and UK, call the below API for each country with country specific values.

```
curl --location 'https://advertising-api.amazon.com/global/productAds' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxxx' \
--header 'Amazon-Ads-AccountId: amzn1.ads-account.g.xxxxxxxxxxxx' \
--header 'Content-Type: application/vnd.globalproductad.v1+json' \
--header 'Accept: application/vnd.globalproductad.v1+json' \
--data '{
  "productAds": [
    {
      "name": "`NikeAirJordanAd-1`",
      "adProduct": "SPONSORED_PRODUCTS",
      "countryCodes": [
        "JP"
      ],
      "state": {
        "value": "ENABLED"
      },
      "globalContext": "GLOBAL",
      "campaignId": "5029975192109995498",
      "adGroupId": "5030047411409185865",
      "adSubject": {
        "adSubjectType": "VENDOR",
        "adSubjectValue": {
          "vendorAdSubject": {
            "countryVendorAdSubjectSettings": {
              "JP": {
                "asin": "B07PG156V6"
              }
            }
          }
        }
      }
    }
  ]
}'
```

#### Response

```
{
   "resultStatus": "SUCCESS",
   "productAdWriteResponses": [
      {
         "errors": [],
         "productAd": {
            "id": "5040067323809178921",
            "name": "NikeAirJordanAd-1",
            "adGroupId": "5030047411409185865",
            "adProduct": "SPONSORED_PRODUCTS",
            "adSubject": {
               "adSubjectType": "VENDOR",
               "adSubjectValue": {
                  "vendorAdSubject": {
                     "countryVendorAdSubjectSettings": {
                        "JP": {
                           "asin": "B07PG156V6"
                        }
                     }
                  }
               }
            },
            "campaignId": "5029975192109995498",
            "countryCodes": [
               "JP"
            ],
            "globalContext": "GLOBAL",
            "state": {
               "value": "ENABLED"
            }
         },
         "resultStatus": "SUCCESS"
      }
   ]
}
```

### Keyword targeting

Keyword targeting lets you have more control over your targeting and spend, identifying keywords for which you want your ads to appear and optimizing bids for each. Targeting is assigned at the ad group level and all products in the ad group share the keyword targeting created. 

#### Request

**Endpoint**
[POST global/targets](global-cm#tag/Targets/operation/CreateTargetsOperation)

>[NOTE] Please note that both types of targets - Keywords and Product can be created in this API

**Parameters**

|Name	|Description	|
|---	|---	|
|globalContext	|Option are either GLOBAL or SINGLE_COUNTRY. globalContext indicates whether the campaign is a global campaign or a single country campaigns. 	|
|countryCodes	|Countries targeted by your global keyword	|
|adProduct	|Currently the only allowed value is SPONSORED_PRODUCTS	|
|name	|A name for the keyword	|
|campaignId	|The campaign to associate with the keyword	|
|adGroupId	|The ad group to associate with the keyword	|
|criteria.criteriaType	| To create a keyword, provide the value KEYWORD. Possible values are `KEYWORD,` `EXPRESSION and` `AUTO.`	|
|criteria.keywordCriteria.keywordSettings.keyword	|Global value of the keyword. Currently, we are supporting only single country targets.|
|criteria.keywordCriteria.nativeLanguageLocale	|The locale preference of the advertiser. For example, if the advertiser’s preferred language is Simplified Chinese, set the locale to zh_CN. Supported locales include: Simplified Chinese (locale: zh_CN) for US, UK and CA. English (locale: en_GB) for DE, FR, IT and ES.	|
|criteria.keywordCriteria.nativeLanguageKeywordSettings	|The unlocalized keyword text in the preferred locale of the advertiser	|
|criteria.keywordCriteria.matchType	|The possible values are "EXACT", "BROAD", "PHRASE"	|
|consideration	|The possible values are “INCLUSIVE" and "EXCLUSIVE". For keyword targeting use "INCLUSLIVE" and for negative keyword targeting use "EXCLUSIVE"	|
|bidValue.countryDefaultBidSetting	|Bids associated with the keyword for each country	|
|state.value	|Options are ENABLED, PAUSED or ARCHIVED. If you don't want your campaign to serve in any of the target countries right away, set state to `PAUSED`.	|

**Example request**


>[NOTE] Please note that in this release, we are supporting only single country Targets. Currently one global Target can only target one country at a time. Below example shows create request for Product Ads targeting JP. In order to create Ads in US and UK, call the below API for each country with country specific values.

```
curl --location 'https://advertising-api.amazon.com/global/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxxx' \
--header 'Amazon-Ads-AccountId: amzn1.ads-account.g.xxxxxxxxxxxx' \
--header 'Content-Type: application/vnd.globaltarget.v1+json' \
--header 'Accept: application/vnd.globaltarget.v1+json' \
--data '{
  "targets": [
    {
      "name": "`NikeAirJordanKeyword-1`",
      "bidValue": {
        "countryDefaultBidSetting": {
          "JP": {
            "value": 0.45
          }
        }
      },
      "consideration": "INCLUSIVE",
      "criteria": {
        "criteriaType": "KEYWORD",
        "criteria": {
          "keywordCriteria": {
            "matchType": "EXACT",
            "keywordSettings": {
              "keyword": "shoe",
              "countryToKeywordSettingsMap": {
                "JP": {
                  "value": "靴"
                }
              }
            },
          }
        }
      },
      "countryCodes": [
        "JP"
      ],
      "state": {
        "value": "ENABLED"
      },
      "globalContext": "GLOBAL",
      "campaignId": "5029975192109995560",
      "adGroupId": "5030047411409185975"
    }
  ]
}'
```

#### Response

```
{
   "resultStatus": "SUCCESS",
   "targetWriteResponses": [
      {
         "errors": [],
         "resultStatus": "SUCCESS",
         "target": {
            "id": "5027875192109995899",
            "adGroupId": "5030047411409185975",
            "adProduct": "SPONSORED_PRODUCTS",
            "bidValue": {
               "countryDefaultBidSetting": {
                  "JP": {
                     "value": 0.45
                  }
               }
            },
            "campaignId": "5029975192109995560",
            "consideration": "INCLUSIVE",
            "countryCodes": [
               "JP"
            ],
            "criteria": {
               "criteria": {
                  "keywordCriteria": {
                     "keywordSettings": {
                       "keyword": "靴"
                        }
                     },
                     "matchType": "BROAD"
                  }
               },
               "criteriaType": "KEYWORD"
            },
            "globalContext": "GLOBAL",
            "state": {
               "value": "ENABLED"
            },
            "name": "NikeAirJordanKeyword-1"
         }
      }
   ]
}
```

### Product and category targeting

You can also target individual products or product categories. When you target individual products, you can choose to a specific ASIN exactly, or target a specific ASIN and other similar ASINs, including substitutes, complements, and other related products. 

#### Request

**Endpoint**
[POST global/targets](global-cm#tag/Targets/operation/CreateTargetsOperation)

>[NOTE] Please note that both types of targets - Keywords and Product can be created using this API

**Parameters:**

|Name	|Description	|
|---	|---	|
|globalContext	|Option are either GLOBAL or SINGLE_COUNTRY. globalContext indicates whether the campaign is a global campaign or a single country campaigns. 	|
|countryCodes	|Countries targeted by your global keyword	|
|adProduct	|Currently the only allowed value is SPONSORED_PRODUCTS	|
|name	|A name for the keyword	|
|campaignId	|The campaign to associate with the keyword	|
|adGroupId	|The ad group to associate with the keyword	|
|criteria.criteriaType	| To create a product target, provide the value EXPRESSION. Possible values are `KEYWORD,` `EXPRESSION and` `AUTO.`	|
|criteria.expressionCriteria.expression.type	|See the supported expression types table below. At minimum, you must include `ASIN_CATEGORY_SAME_AS`. See the **Supported expression types** table below for more details.	|
|criteria.expressionCriteria.expression.countryToExpressionSettingsMap	|The value of targeting expression for each country within global campaign. The value differs based on the expression type. See the table below for more information. 
|consideration	|The possible values are “INCLUSIVE" and "EXCLUSIVE". For keyword targeting use "INCLUSLIVE" and for negative keyword targeting use "EXCLUSIVE"	|
|bidValue.countryDefaultBidSetting	|Bids associated with the keyword for each country	|
|state.value	|Options are ENABLED, PAUSED or ARCHIVED. If you don't want your campaign to serve in any of the target countries right away, set state to `PAUSED`.	|

>[NOTE] Currently, we are supporting only single country targets. 

#### Supported expression types

|**Predicate**	|Description	|**Example**	|
|---	|---	|---	|
|`ASIN_CATEGORY_SAME_AS`	|Target the category that is the same as the category. Value is a category ID.	|11056341	|
|`ASIN_BRAND_SAME_AS`	|Target the brand that is the same as the bran. Value is a brand ID. 	|7048034011	|
|`ASIN_PRICE_LESS_THAN`	|Target a price that is less than the price expressed. Value is a price in account currency.	|20.5	|
|`ASIN_PRICE_BETWEEN`	|Target a price that is between the prices expressed. Value is two prices in account currency separated by a hypen.	|10.5-12.5	|
|`ASIN_PRICE_GREATER_THAN`	|Target a price that is greater than the price expressed. Value is a price in account currency.	|10.75	|
|`ASIN_REVIEW_RATING_LESS_THAN`	|Target a review rating less than the review rating that is expressed. Value is a number less than 5.	|4	|
|`ASIN_REVIEW_RATING_BETWEEN`	|Target a review rating that is between the review ratings expressed.	|4-5	|
|`ASIN_REVIEW_RATING_GREATER_THAN`	|Target a review rating that is greater than the review rating expressed. Value is a number greater than 1.	|3	|
|`ASIN_SAME_AS`	|Target an ASIN that is the same as the ASIN expressed	|B07B421VFF	|
|`ASIN_IS_PRIME_SHIPPING_ELIGIBLE`	|Target products that are Prime Shipping Eligible. This refinement can be applied at a category or brand level only. Value is a boolean.	|TRUE	|
|`ASIN_AGE_RANGE_SAME_AS`	|Target an age range that is in the expressed range. This refinement can be applied for toys and games categories only. Value is an age range ID.	|165890011	|
|`ASIN_GENRE_SAME_AS`	|Target products related to the expressed genre. This refinement can be applied for Books and eBooks categories only.	|1235687	|
|`ASIN_EXPANDED_FROM`	|Target products similar in performance to the ASIN expressed.	|B07B421VFF	|

**Example request**


>[NOTE] Please note that in this release, we are supporting only single country Targets. Currently one global Target can only target one country at a time. Below example shows create request for Product Ads targeting JP. In order to create Ads in US and UK, call the below API for each country with country specific values.

```
curl --location 'https://advertising-api.amazon.com/global/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxxx' \
--header 'Amazon-Ads-AccountId: amzn1.ads-account.g.xxxxxxxxxxxx' \
--header 'Content-Type: application/vnd.globaltarget.v1+json' \
--header 'Accept: application/vnd.globaltarget.v1+json' \
 --data '{
  "targets": [
    {
      "name": "NikeAirJordanProductTarget-1",
      "bidValue": {
        "countryDefaultBidSetting": {
          "JP": {
            "value": 32.4159
          }
        }
      },
      "consideration": "INCLUSIVE",
      "criteria": {
        "criteriaType": "EXPRESSION",
        "criteria": {
          "expressionCriteria": {
            "expression": [
              {
                "type": "ASIN_CATEGORY_SAME_AS",
                "countryToExpressionSettingsMap": {
                  "JP": {
                    "value": "354613566"
                  }
                }
              },
             {
                "type": "ASIN_PRICE_LESS_THAN",
                "countryToExpressionSettingsMap": {
                  "JP": {
                    "value": "253361"
                  }
                }
              }
            ]
          }
        }
      },
      "countryCodes": [
        "JP"
      ],
      "state": {
        "value": "ENABLED"
      },
      "globalContext": "GLOBAL",
      "campaignId": "5029975192109995560",
      "adGroupId": "5030047411409185975"
    }
  ]
}'
```

#### Response

```
{
   "resultStatus": "SUCCESS",
   "targetWriteResponses": [
      {
         "errors": [],
         "resultStatus": "SUCCESS",
         "target": {
            "id": "5047975192109995599",
            "adGroupId": "5030047411409186789",
            "bidValue": {
               "countryDefaultBidSetting": {
                  "JP": {
                     "value": 0.47
                  }
               }
            },
            "campaignId": "5029975192109995999",
            "consideration": "INCLUSIVE",
            "countryCodes": [
               "JP"
            ],
            "criteria": {
               "criteriaType": "EXPRESSION",
               "criteria": {
                  "expressionCriteria": {
                     "expression": [
                        {
                           "type": "ASIN_CATEGORY_SAME_AS",
                           "countryToExpressionSettingsMap": {
                              "JP": {
                                 "value": "354613566"
                              }
                           }
                        },
                    {
                        "type": "ASIN_PRICE_LESS_THAN",
                        "countryToExpressionSettingsMap": {
                            "JP": {
                                "value": "253361"
                            }
                         }
                      } 
                    ]
                  }
               }
            },
            "globalContext": "GLOBAL",
            "adProduct": "SPONSORED_PRODUCTS",
            "state": {
               "value": "ENABLED"
            },
            "name": "NikeAirJordanProductTarget-1"
         }
      }
   ]
}
```

### Auto targeting

Auto targeting expressions let you can choose different bids for groups of search terms and products within an [auto targeting campaign](#auto-campaigns).

If you don’t set up any auto targeting clauses, your ad group uses the default bid you assigned during ad group creation for all targeting. However, you can use auto targeting expressions to specify different bids for different types of targeting expressions.

Auto targeting supports four types of targeting based on the ASINs that are assigned to an ad group as a product ad. 

#### Supported expression types

|Expression	|**Console Equivalent**	|Description	|
|---	|---	|---	|
|`QUERY_BROAD_REL_MATCHES`	|loose match	|Search terms loosely correlated to your product.	|
|`QUERY_HIGH_REL_MATCHES`	|close match	|Search terms closely related to your product.	|
|`ASIN_ACCESSORY_RELATED`	|complements	|Detail pages for products that complement your product.	|
|`ASIN_SUBSTITUTE_RELATED`	|substitutes	|Detail pages for products that are substitutes for your product.	|



### Negative targeting

#### Negative keywords

Negative keywords help prevent ads from appearing on shopping results pages that don’t meet your campaign objectives. Negative keyword targeting can be used at either the campaign level or ad group level. Negative keywords can be created using the [targets API](#auto-targeting) and by providing the value “EXCLUSIVE”  for the parameter “consideration”. You can provide a global value for negative keyword and/or provide country specific values for each target country of the global campaign**.**


>[NOTE] Please note that bid value is not set for negative keywords

**Example request**

```
curl --location 'https://advertising-api.amazon.com/global/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxxx' \
--header 'Amazon-Ads-AccountId: amzn1.ads-account.g.xxxxxxxxxxxx' \
--header 'Content-Type: application/vnd.globaltarget.v1+json' \
--header 'Accept: application/vnd.globaltarget.v1+json' \
--data '{
  "targets": [
    {
      "name": "`NikeAirJordanKeyword-1`",
      "consideration": "EXCLUSIVE",
      "criteria": {
        "criteriaType": "KEYWORD",
        "criteria": {
          "keywordCriteria": {
            "matchType": "EXACT",
            "keywordSettings": {
              "keyword": "shoe",
              "countryToKeywordSettingsMap": {
                "JP": {
                  "value": "靴"
                }
              }
            },
          }
        }
      },
      "countryCodes": [
        "JP"
      ],
      "state": {
        "value": "ENABLED"
      },
      "globalContext": "GLOBAL",
      "campaignId": "5029975192109995560",
      "adGroupId": "5030047411409185975"
    }
  ]
}'
```

**Response**

```
{
   "resultStatus": "SUCCESS",
   "targetWriteResponses": [
      {
         "errors": [],
         "resultStatus": "SUCCESS",
         "target": {
            "id": "5027875192109995899",
            "adGroupId": "5030047411409185975",
            "adProduct": "SPONSORED_PRODUCTS",
            "campaignId": "5029975192109995560",
            "consideration": "EXCLUSIVE",
            "countryCodes": [
               "JP"
            ],
            "criteria": {
               "criteria": {
                  "keywordCriteria": {
                     "keywordSettings": {
                       "keyword": "靴"
                        }
                     },
                     "matchType": "BROAD"
                  }
               },
               "criteriaType": "KEYWORD"
            },
            "globalContext": "GLOBAL",
            "state": {
               "value": "ENABLED"
            },
            "name": "NikeAirJordanKeyword-1"
         }
      }
   ]
}
```



#### Negative product and brand targeting

Both auto and manual Sponsored Products campaigns can use negative product targeting. Negative product targeting allows you to exclude certain products and brands from your targeting. Negative product targets can be created using the [targets API](#auto-targeting) and by providing the value `EXCLUSIVE` for the parameter `consideration`. You can provide the expression value for each target country.

>[NOTE] Please note that bid value is not set for negative targets.

**Example request**

```
curl --location 'https://advertising-api.amazon.com/global/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxxx' \
--header 'Amazon-Ads-AccountId: amzn1.ads-account.g.xxxxxxxxxxxx' \
--header 'Content-Type: application/vnd.globaltarget.v1+json' \
--header 'Accept: application/vnd.globaltarget.v1+json' \
 --data '{
  "targets": [
    {
      "name": "NikeAirJordanProductTarget-1",
      "consideration": "EXCLUSIVE",
      "criteria": {
        "criteriaType": "EXPRESSION",
        "criteria": {
          "expressionCriteria": {
            "expression": [
              {
                "type": "ASIN_CATEGORY_SAME_AS",
                "countryToExpressionSettingsMap": {
                  "JP": {
                    "value": "354613566"
                  }
                }
              },
             {
                "type": "ASIN_PRICE_LESS_THAN",
                "countryToExpressionSettingsMap": {
                  "JP": {
                    "value": "253361"
                  }
                }
              }
            ]
          }
        }
      },
      "countryCodes": [
        "JP"
      ],
      "state": {
        "value": "ENABLED"
      },
      "globalContext": "GLOBAL",
      "campaignId": "5029975192109995560",
      "adGroupId": "5030047411409185975"
    }
  ]
}'
```

#### Response

```
{
   "resultStatus": "SUCCESS",
   "targetWriteResponses": [
      {
         "errors": [],
         "resultStatus": "SUCCESS",
         "target": {
            "id": "5047975192109995599",
            "adGroupId": "5030047411409186789",
            "campaignId": "5029975192109995999",
            "consideration": "INCLUSIVE",
            "countryCodes": [
               "JP"
            ],
            "criteria": {
               "criteriaType": "EXPRESSION",
               "criteria": {
                  "expressionCriteria": {
                     "expression": [
                        {
                           "type": "ASIN_CATEGORY_SAME_AS",
                           "countryToExpressionSettingsMap": {
                              "JP": {
                                 "value": "354613566"
                              }
                           }
                        },
                    {
                        "type": "ASIN_PRICE_LESS_THAN",
                        "countryToExpressionSettingsMap": {
                            "JP": {
                                "value": "253361"
                            }
                         }
                      } 
                    ]
                  }
               }
            },
            "globalContext": "GLOBAL",
            "adProduct": "SPONSORED_PRODUCTS",
            "state": {
               "value": "ENABLED"
            },
            "name": "NikeAirJordanProductTarget-1"
         }
      }
   ]
}
```

