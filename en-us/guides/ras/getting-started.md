---
title: Getting started with the Retail Ad Service API
description: Learn to use the Retail Ad Service API
type: guide
interface: api
keywords:
   - get started
   - request example
   - response example
---

# Get started with Amazon Retail Ad Service

## Overview

Amazon Retail Ad Service (RAS) allows advertisers to build campaigns through Amazon Ads that run on other retailer’s websites and apps. The API follows a similar pattern to Amazon Ads Sponsored Products campaigns. 

### 1.1 Onboarding to to the Amazon Ads API

If this is your first time calling the API, please follow our [Onboarding](guides/onboarding/overview) and [Getting started](guides/get-started/overview) guides to request access to the API and set up authentication.

### 1.2 Headers

Each request described in this guide requires these header parameter: 

|Parameter	|Description	|
|---	|---	|
|`Amazon-Advertising-API-ClientId`	|The client ID associated with your [Login with Amazon application](guides/onboarding/create-lwa-app). Used for authentication.	|
|`Authorization`	|Your [access token](guides/account-management/authorization/access-tokens). Used for authentication.	|
|`Amazon-Advertising-API-Scope`	|The [profile ID](guides/account-management/authorization/profiles) associated with an advertising account in a specific marketplace. Make sure you are using the [correct base URL](reference/api-overview#api-endpoints) for the marketplace associated to the profile.	|

### 1.3 Auto vs. manual campaigns

There are two types of RAS campaigns: auto targeting and manual targeting. Both manual and auto campaigns require you to create, at minimum, campaigns, ad groups, and product ads. However, for manual campaigns, the advertiser also has complete control over targets. For auto campaigns, Amazon suggests relevant targeting expressions and keywords based on the products you are advertising. Both auto and manual campaigns also support negative targeting.

Amazon sets initial bids for the created auto targets, but advertisers can update the bids if they choose.


### 1.4 API URLs

In Amazon Ads the base URL you must use with your API call depends on the region of the profile associated to the advertiser you are making a call on behalf of. At the moment, the RAS API only supports calls to the North America region for Advertisers in United States (US) marketplace.

Use the table below to understand which base URL you should use:

|URL	|Region	|Marketplaces	|
|---	|---	|---	|
|[https://advertising-api.amazon.com](https://advertising-api.amazon.com/)	|North America (NA)	|United States (US)	|

## 2. Creating campaigns

Creating an active campaign requires a sequence of API calls to create campaign, ad group, product ad, and targeting resources.

### 2.1 Create a campaign

#### Resource

[POST ras/v1/campaigns](retail-ad-service#tag/Campaigns/operation/RASv1CreateCampaigns)

#### Request: Auto campaign

```
{
  "campaigns": [
    {
      "name": "My New Campaign Name",
      "budget": {
        "budget": 5.0,
        "budgetType": "DAILY"
      },
      "targetingType": "AUTO",
      "state": "ENABLED",
      "startDate": "2024-04-24",
      "endDate": "2024-05-24",
      "dynamicBidding": {
        "strategy": "MANUAL",
        "placementBidding": [
          {
            "placement": "PLACEMENT_TOP",
            "percentage": 900
          }
        ]
      },
      "tags": [
        {
          "key": "string",
          "value": "string"
        }
      ]
    }
  ]
}
```

#### Request: Manual campaign

```
{
  "campaigns": [
    {
      "name": "My New Campaign Name",
      "budget": {
        "budget": 5.0,
        "budgetType": "DAILY"
      },
      "targetingType": "MANUAL",
      "state": "ENABLED",
      "startDate": "2024-04-24",
      "endDate": "2024-05-24",
      "dynamicBidding": {
        "strategy": "MANUAL",
        "placementBidding": [
          {
            "placement": "PLACEMENT_TOP",
            "percentage": 900
          }
        ]
      },
      "tags": [
        {
          "key": "string",
          "value": "string"
        }
      ]
    }
  ]
}
```

#### Response

Response from the API varies based on the presence of “Prefer” header in the Request. When “Prefer” header is present and its value is “return=representation” then full entity details will be returned in the response. Otherwise only entity identifier will be returned. A successful call results in a 207 response with individual response codes for each entity object included in the request.

```
{
  "campaigns": {
    "success": [
      {
        "index": 0,
        "campaignId": "string",
        "campaign": {
          "campaignId": "string",
          "name": "string",
          "budget": {
            "budget": 5.0,
            "effectiveBudget": 5.0,
            "budgetType": "DAILY"
          },
          "targetingType": "AUTO",
          "state": "ENABLED",
          "startDate": "2024-04-24",
          "endDate": "2024-05-24",
          "dynamicBidding": {
            "strategy": "MANUAL",
            "placementBidding": [
              {
                "placement": "PLACEMENT_TOP",
                "percentage": 900
              }
            ]
          },
          "tags": [
            {
              "key": "string",
              "value": "string"
            }
          ]
        }
      }
    ]
  }
}
```

Make note of your `campaignId` for use in the next step.

### 2.2 Create an ad group

Both auto and manual campaigns require at least one ad group. Creation of an ad group requires a `campaignId` (see step 2.1) and a retailerId. The `retailerId` represents the product catalog that includes the products you want to advertise.

#### 2.2.1 Retrieving retailerIds

**Resource**

[POST ras/v1/retailers/list](retail-ad-service/retailer-identity#tag/Retailers/RASv1ListRetailers)

**Request**

You can configure the number of results returned using the `maxResults` field, which is optional and has a default value of 10. If the number of retailers exceeds `maxResults`, a `nextToken` string is returned in the response which can be provided on another API request to fetch the next page of results.

```
{
  "maxResults": "10",
  "nextToken": "string"
}
```

**Response**

```
{
  "retailers": [
    {
      "domain": "string",
      "name": "string",
      "retailerId": "string",
      "status": "ACTIVE"
    }
  ],
  "totalResults": 1,
  "nextToken": "string"
}
```

#### 2.2.2 Creating an ad group

**Resource**

[POST ras/v1/adGroups](retail-ad-service#tag/AdGroups/operation/RASv1CreateAdGroups)

**Request**

```
{
  "adGroups": [
    {
      "name": "string",
      "campaignId": "string",
      "defaultBid": 5.0,
      "state": "ENABLED",
      "retailerId": "string"
    }
  ]
}
```

**Response**

Response from the API varies based on the presence of “Prefer” header in the Request. When “Prefer” header is present and its value is “return=representation” then full entity details will be returned in the response. Otherwise only entity identifier will be returned. A successful call results in a 207 response with individual response codes for each entity object included in the request.

```
{
  "adGroups": {
    "success": [
      {
        "index": 0,
        "adGroupId": "string",
        "adGroup": {
          "adGroupId": "string",
          "name": "string",
          "campaignId": "string",
          "defaultBid": 5.0,
          "state": "ENABLED",
          "retailerId": "string"
        }
      }
    ]
  }
}
```

### 2.3 Create a product ad

To create a product ad, you will need a `campaignId` (step 2.1), `adGroupId` (step 2.2), `retailerId` (step 2.2), and `retailerOfferId`. The `retailerOfferId` represents the product or service that you are advertising. Amazon will use offer details like image, name, and rating to build a responsive creative for use in your campaign. You can discover available products per retailer using product metadata API (see section [3\. Product Metadata](#3-product-metadata))

#### Resource

[POST ras/v1/productAds](retail-ad-service#tag/ProductAds/operation/RASv1CreateProductAds)

#### Request

```
{
  "productAds": [
    {
      "campaignId": "string",
      "adGroupId": "string",
      "state": "ENABLED",
      "retailerId": "string",
      "retailerOfferId": "string"
    }
  ]
}
```

#### Response

Response from the API varies based on the presence of “Prefer” header in the Request. When “Prefer” header is present and its value is “return=representation” then full entity details will be returned in the response. Otherwise only entity identifier will be returned. A successful call results in a 207 response with individual response codes for each entity object included in the request.

```
{
  "productAds": {
    "success": [
      {
        "index": 0,
        "productAdId": "string",
        "productAd": {
          "productAdId": "string",
          "campaignId": "string",
          "adGroupId": "string",
          "state": "ENABLED",
          "retailerId": "string",
          "retailerOfferId": "string"
        }
      }
    ]
  }
}
```

### 2.4 Create  keyword targets for manual campaigns

For manual campaigns, advertisers must create their own targets. Each manual campaign must have at least one non-negative target. Currently RAS only supports keyword targets, which are based on customer search terms. 

To create a keyword, you will need a `campaignId` (step 2.1) and `adGroupId` (step 2.2), as well as the keyword you want to target. 

#### Resource

[POST ras/v1/targets](retail-ad-service#tag/Targets/operation/RASv1CreateTargets)

#### Request - Keyword target

* Make sure `negative` is set to `false` to create an inclusive keyword target.
* See [Keyword match types](reference/common-models/enums#keyword-target-match-types) to learn more about the meaning of each `matchType` .

```
{
  "targets": [
    {
      "campaignId": "string",
      "adGroupId": "string",
      "state": "ENABLED",
      "negative": false,
      "targetType": "KEYWORD",
      "targetDetails": {
        "keywordTarget": {
          "matchType": "EXACT",
          "keyword": "string"
        }
      },
      "bid": {
        "bid": 0
      }
    }
  ]
}
```

>[NOTE] Minimum and maximum bids for each marketplace are the same as in [Sponsored Products](reference/concepts/limits#bid-constraints-by-marketplace).

#### Response

Response from the API varies based on the presence of “Prefer” header in the Request. When “Prefer” header is present and its value is “return=representation” then full entity details will be returned in the response. Otherwise only entity identifier will be returned. A successful call results in a 207 response with individual response codes for each entity object included in the request.

```
{
  "targets": {
    "success": [
      {
        "index": 0,
        "targetId": "string",
        "target": {
          "targetId": "string",
          "campaignId": "string",
          "adGroupId": "string",
          "state": "ENABLED",
          "negative": false,
          "targetLevel": "AD_GROUP",
          "targetType": "KEYWORD",
          "targetDetails": {
            "keywordTarget": {
              "matchType": "EXACT",
              "keyword": "string"
            }
          },
          "bid": {
            "bid": 0,
            "currencyCode": "USD"
          }
        }
      }
    ]
  }
}
```

### 2.5 Update targets for auto campaigns (optional)

For auto campaigns, advertisers do not create their own targets. Amazon automatically creates targets after you have created an ad group. Amazon will set bids automatically for these automatic targets, but advertisers can manually update the bids if they want. 

#### 2.5.1 Retrieve targetIds

**Resource**

[POST ras/v1/targets/list](retail-ad-service#tag/Targets/operation/RASv1ListTargets)

**Request**

You can use the `targetTypeFilter` to only return targets of type `AUTO`.

```
{
  "targetTypeFilter": [
    "AUTO"
  ]
}
```

**Response**

A successful call results in a 200 response with all targeting clauses matching the filters in the request. You should see four auto targeting clauses that have been automatically created for you with matchTypes of `PRODUCT_SUBSTITUTES`, `SEARCH_LOOSE_MATCH`, `SEARCH_CLOSE_MATCH`, `PRODUCT_COMPLEMENTS`. [Learn more about match types for auto targets](reference/common-models/enums#auto-target-match-types).

```
{
  "totalResults": 3,
  "targets": [
    {
      "targetId": "string",
      "campaignId": "string",
      "adGroupId": "string",
      "state": "ENABLED",
      "negative": false,
      "targetLevel": "AD_GROUP",
      "targetType": "AUTO",
      "targetDetails": {
        "autoTarget": {
          "matchType": "PRODUCT_SUBSTITUTES"
        }
      },
      "bid": {
        "bid": 0,
        "currencyCode": "USD"
      },
      "extendedData": {
        "servingStatus": "TARGETING_CLAUSE_STATUS_LIVE",
        "servingStatusDetails": [
          {
            "name": "TARGETING_CLAUSE_STATUS_LIVE_DETAIL",
            "message": "string",
            "helpUrl": "string"
          }
        ],
        "creationDateTime": "2019-08-24T14:15:22Z",
        "lastUpdateDateTime": "2019-08-24T14:15:22Z"
      }
    },
    {
      "targetId": "string",
      "campaignId": "string",
      "adGroupId": "string",
      "state": "ENABLED",
      "negative": false,
      "targetLevel": "AD_GROUP",
      "targetType": "AUTO",
      "targetDetails": {
        "autoTarget": {
          "matchType": "KEYWORDS_LOOSE_MATCH"
        }
      },
      "bid": {
        "bid": 0,
        "currencyCode": "USD"
      },
      "extendedData": {
        "servingStatus": "TARGETING_CLAUSE_STATUS_LIVE",
        "servingStatusDetails": [
          {
            "name": "TARGETING_CLAUSE_STATUS_LIVE_DETAIL",
            "message": "string",
            "helpUrl": "string"
          }
        ],
        "creationDateTime": "2019-08-24T14:15:22Z",
        "lastUpdateDateTime": "2019-08-24T14:15:22Z"
      }
    },
    {
      "targetId": "string",
      "campaignId": "string",
      "adGroupId": "string",
      "state": "ENABLED",
      "negative": false,
      "targetLevel": "AD_GROUP",
      "targetType": "AUTO",
      "targetDetails": {
        "autoTarget": {
          "matchType": "KEYWORDS_CLOSE_MATCH"
        }
      },
      "bid": {
        "bid": 0,
        "currencyCode": "USD"
      },
      "extendedData": {
        "servingStatus": "TARGETING_CLAUSE_STATUS_LIVE",
        "servingStatusDetails": [
          {
            "name": "TARGETING_CLAUSE_STATUS_LIVE_DETAIL",
            "message": "string",
            "helpUrl": "string"
          }
        ],
        "creationDateTime": "2019-08-24T14:15:22Z",
        "lastUpdateDateTime": "2019-08-24T14:15:22Z"
      }
    }
  ]
}
```

Record the `targetIds` for use in the next step.

#### 2.5.2 Update target bids

**Resource**

[PUT ras/v1/targets](retail-ad-service#tag/Targets/operation/RASv1UpdateTargets)

**Request**

Use the target IDs retrieved in step 2.5.1 in the PUT request.

```
{
  "targets": [
    {
      "targetId": "string",
      "state": "ENABLED",
      "bid": {
        "bid": 0
      }
    }
  ]
}
```


**Response**

A successful PUT request returns a 207 response. You should see the updated bid in the response.

```
{
  "targets": {
    "success": [
      {
        "index": 0,
        "targetId": "string",
        "target": {
          "targetId": "string",
          "campaignId": "string",
          "adGroupId": "string",
          "state": "ENABLED",
          "negative": true,
          "targetLevel": "AD_GROUP",
          "targetType": "AUTO",
          "targetDetails": {
            "autoTarget": {
              "matchType": "KEYWORDS_LOOSE_MATCH"
            }
          },
          "bid": {
            "bid": 0,
            "currencyCode": "USD"
          },
          "extendedData": {
            "servingStatus": "TARGETING_CLAUSE_STATUS_LIVE",
            "creationDateTime": "2019-08-24T14:15:22Z",
            "lastUpdateDateTime": "2019-08-24T14:15:22Z"
          }
        }
      }
    ]
  }
}
```

### 2.6 Create negative targets (optional)

Negative targets help prevent ads from appearing in contexts that don’t meet campaign objectives. Both auto and manual targeting campaigns support negative targets. Negative targets can be at either the ad group or campaign level. Campaign level negative targets will impact all ads and ad groups within the campaign. Negative targets are created using the same resource as positive targets. 

#### Resource

[POST ras/v1/targets](retail-ad-service#tag/Targets/operation/RASv1CreateTargets)

#### Request - Ad group negative keyword target

```
{
  "targets": [
    {
      "campaignId": "string",
      "adGroupId": "string",
      "state": "ENABLED",
      "negative": true,
      "targetType": "KEYWORD",
      "targetDetails": {
        "keywordTarget": {
          "matchType": "EXACT",
          "keyword": "string"
        }
      }
    }
  ]
}
```

#### Response

Response from the API varies based on the presence of “Prefer” header in the Request. When “Prefer” header is present and its value is “return=representation” then full entity details will be returned in the response. Otherwise only entity identifier will be returned. A successful call results in a 207 response with individual response codes for each entity object included in the request.

```
{
  "targets": {
    "success": [
      {
        "index": 0,
        "targetId": "string",
        "target": {
          "targetId": "string",
          "campaignId": "string",
          "adGroupId": "string",
          "state": "ENABLED",
          "negative": true,
          "targetLevel": "AD_GROUP",
          "targetType": "KEYWORD",
          "targetDetails": {
            "keywordTarget": {
              "matchType": "EXACT",
              "keyword": "string"
            }
          }
        }
      }
    ]
  }
}
```

#### Request - Campaign negative keyword target

To create a campaign-level negative target, do not input an adGroupId in the request.

```
{
  "targets": [
    {
      "campaignId": "string",
      "state": "ENABLED",
      "negative": true,
      "targetType": "KEYWORD",
      "targetDetails": {
        "keywordTarget": {
          "matchType": "EXACT",
          "keyword": "string"
        }
      }
    }
  ]
}
```

#### Response

Response from the API varies based on the presence of “Prefer” header in the Request. When “Prefer” header is present and its value is “return=representation” then full entity details will be returned in the response. Otherwise only entity identifier will be returned. A successful call results in a 207 response with individual response codes for each entity object included in the request.

```
{
  "targets": {
    "success": [
      {
        "index": 0,
        "targetId": "string",
        "target": {
          "targetId": "string",
          "campaignId": "string",
          "state": "ENABLED",
          "negative": true,
          "targetLevel": "CAMPAIGN",
          "targetType": "KEYWORD",
          "targetDetails": {
            "keywordTarget": {
              "matchType": "EXACT",
              "keyword": "string"
            }
          }
        }
      }
    ]
  }
}
```

## 3. Product Metadata

To retrieve metadata and details for products, you can use the existing `/product/metadata` API endpoint and include a new `retailerInfo` element, which contains the `retailerType` as `"RMS"` and the `retailerId`, for which products are requested:
product-metadata/

#### Resource

[POST product/metadata](product-metadata#tag/Product-Selector/operation/ProductMetadata)

#### Request - Retrieve products based on a specific offer ID:

```
{ 
    "pageIndex": 0, 
    "pageSize": 30, 
    "checkEligibility": true, 
    "adType": "SP", 
    "checkItemDetails": true,
    "retailerInfo": { 
        "retailerType": "RMS", 
        "retailerId": string
    }, 
    "retailerOfferIds": [ string, string ] 
}
```

#### Request - Retrieve products based on a search string/query:

```
{ 
    "pageIndex": 0, 
    "pageSize": 300, 
    "checkEligibility": true, 
    "adType": "SP", 
    "checkItemDetails": true,
    "sortOrder": "DESC", 
    "retailerInfo": { 
        "retailerType": "RMS", 
        "retailerId": string
    }, 
    "searchStr": "shoes"
}
```

#### Response

A successful call results in a 200 response with a list of products for the retailer ID included in the request (and matching the offer IDs or search string if any were provided in the request).

```
{
  "ProductMetadataList": [
    {
      "availability": string,
      "createdDate": date,
      "eligibilityStatus": string,
      "imageUrl": string,
      "priceToPay": {
        "amount": float,
        "currency": string
      },
      "basisPrice": {
        "amount": float,
        "currency": string
      },
      "title": string,
      "category": string,
      "brand": string,
      "retailerOfferId": string
    }
  ],
  "retailerInfo": {
    "retailerType": "RMS",
    "retailerId": string
  }
}
```

## 4. Reporting

To get performance data for RAS campaigns, you can use the existing Sponsored Products report types in [reporting version 3](guides/reporting/v3/overview).

Available report types:

* [spCampaigns](guides/reporting/v3/report-types/campaign)
* [spTargeting](guides/reporting/v3/report-types/targeting)
* [spSearchTerm](guides/reporting/v3/report-types/search-term)
* [spGrossAndInvalids](guides/reporting/v3/report-types/gross-and-invalid-traffic)
* [spAdvertisedProduct](guides/reporting/v3/report-types/advertised-product)

To request these reports, follow the [Getting started guide](guides/reporting/v3/get-started).
