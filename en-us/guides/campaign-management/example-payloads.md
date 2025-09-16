---
title: Example request payloads for campaign management in the Amazon Ads API
description: Example payloads for campaign management requests in the Amazon Ads API, sorted by ad product and resource
type: guide
interface: api
tags:
    - Campaign management
keywords:
    - request
    - payload
    - unified
---

# Example request payloads for campaign management

This document includes example request payloads for campaign management operations in the Amazon Ads API. [Learn more about the campaign management API.](guides/campaign-management/overview)

The examples are arranged by ad product, then by resource, with an example payload for each available operation on each resource.

### Technical notes

- IDs included in these examples should be replaced with appropriate IDs for your account's resources.
- Additional optional fields may be available for some requests. For complete information, see:
    - [Campaign management API OpenAPI specification](amazon-ads/1-0/openapi)
    - [Campaign management entity guides](guides/campaign-management/entities/overview)
- For complete information on request paths and header values, see the [OpenAPI specification](amazon-ads/1-0/openapi).

### Contents

| Ad product | Resources | <span> </span> | <span> </span> | <span> </span> |
| --- | --- | --- | --- | --- |
| [**Sponsored Products**](#sponsored-products) | [Campaign](#campaign) | [Ad group](#ad-group) | [Ad](#ad) | [Target](#target) |
| [**Sponsored Brands**](#sponsored-brands) | [Campaign](#campaign-2) | [Ad group](#ad-group-2) | [Ad](#ad-2) | [Target](#target-2) |
| [**Amazon DSP**](#amazon-dsp) | [Campaign](#campaign-3) | [Ad group](#ad-group-3) | [Ad](#ad-3) | [Target](#target-3) |

## Sponsored Products 

### Campaign

#### Create Campaign

```json
{
    "campaigns": [
        {
            "adProduct": "SPONSORED_PRODUCTS",
            "name": "My Test Campaign",
            "state": "ENABLED",
            "marketplaceScope": "SINGLE_MARKETPLACE",
            "marketplaces": [
                "US"
            ],
            "startDateTime": "2025-11-01T00:00:00.000-07:00",
            "budgets": [
                {
                    "budgetType": "MONETARY",
                    "budgetValue": {
                        "monetaryBudgetValue": {
                            "monetaryBudget": {
                                "value": 1000.0
                            }
                        }
                    },
                    "recurrenceTimePeriod": "DAILY"
                }
            ],
            "autoCreationSettings": {
                "autoCreateTargets": false
            }
        }
    ]
}
```

#### Update Campaign

```json
{
    "campaigns": [
        {
            "campaignId": "339874654410161",
            "state": "PAUSED"
        }
    ]
}
```

#### Query Campaign

```json
{
    "campaignIdFilter": {
        "include": [
            "339874654410161"
        ]
    },
    "adProductFilter": {
        "include": [
            "SPONSORED_PRODUCTS"
        ]
    }
}
```

#### Delete Campaign

```json
{
  "campaignIds": [
    "339874654410161"
  ]
}
```

### Ad Group

#### Create Ad Group

```json
{
    "adGroups": [
        {
            "adProduct": "SPONSORED_PRODUCTS",
            "bid": {
                "defaultBid": 1.00
            },
            "campaignId": "449479039293829",
            "name": "My Test Ad Group",
            "state": "ENABLED"
        }
    ]
}
```

#### Update Ad Group

```json
{
    "adGroups": [
        {
            "adGroupId": "420175408832647",
            "bid": {
                "defaultBid": 2.00
            }
        }
    ]
}
```

#### Query Ad Group

```json
{
    "campaignIdFilter": {
        "include": [
            "449479039293829"
        ]
    },
    "adProductFilter": {
        "include": [
            "SPONSORED_PRODUCTS"
        ]
    }
}
```

#### Delete Ad Group

```json
{
    "adGroupIds": [
        "420175408832647"
    ]
}
```

### Ad

#### Create Ad

```json
{
    "ads": [
        {
            "adType": "PRODUCT_AD",
            "adProduct": "SPONSORED_PRODUCTS",
            "state": "ENABLED",
            "adGroupId": "420175408832647",
            "creative": {
                "productCreative": {
                    "productCreativeSettings": {
                        "advertisedProduct": {
                            "productId": "65e844ca-b9f2",
                            "productIdType": "SKU"
                        }
                    }
                }
            }
        }
    ]
}
```

#### Update Ad

```json
{
    "ads": [
        {
            "adId": "544973450853432",
            "state": "PAUSED"
        }
    ]
}
```

#### Query Ad

```json
{
    "adProductFilter": {
        "include": [
            "SPONSORED_PRODUCTS"
        ]
    },
    "adGroupIdFilter": {
        "include": [
            "420175408832647"
        ]
    },
    "maxResults": 1000
}
```

#### Delete Ad

```json
{
    "adIds": [
        "544973450853432"
    ]
}
```

### Target

#### Create Target

```json
{
    "targets": [{
        "adGroupId": "420175408832647",
        "adProduct": "SPONSORED_PRODUCTS",
        "negative": false,
        "state": "ENABLED",
        "targetType": "KEYWORD",
        "targetDetails": {
            "keywordTarget": {
                "keyword": "Test keyword",
                "matchType": "EXACT"
            }
        }
    }]
}
```

#### Update Target

```json
{
    "targets": [
        {
            "targetId": "401920758869247",
            "bid": {
                "bid": 10
            }
        }
    ]
}
```

#### Query Target

```json
{
    "adProductFilter": {
        "include": [
            "SPONSORED_PRODUCTS"
        ]
    },
    "targetTypeFilter": {
        "include": [
            "KEYWORD"
        ]
    },
    "campaignIdFilter": {
        "include": [
            "449479039293829"
        ]
    }
}
```

#### Delete Target 

```json
{
    "targetIds": [
        "401920758869247"
    ]
}
```

## Sponsored Brands

### Campaign

#### Create Campaign

```json
{
    "campaigns": [{
        "adProduct": "SPONSORED_BRANDS",
        "marketplaceScope": "SINGLE_MARKETPLACE",
        "name": "tomTests15",
        "state": "PAUSED",
        "marketplaces": ["US"],
        "startDateTime": "2027-03-18T00:00:08Z",
        "costType": "CPC",
        "budgets": [{
            "budgetType": "MONETARY",
            "budgetValue": {
                "monetaryBudgetValue": {
                    "marketplaceSettings": [],
                    "monetaryBudget": {
                        "value": 1000.0
                    }
                }
            },
            "recurrenceTimePeriod": "DAILY"
        }],
        "optimizations": {
            "goalSettings": {
                "kpi": "CLICKS"
            }
        }
    }]
}
```

#### Update Campaign

```json
{
    "campaigns": [{
        "campaignId": "475081133189954",
        "state": "ENABLED"
    }]
}
```

#### Query Campaign

```json
{
    "campaignIdFilter": {
        "include": ["475081133189954"]
    },
    "adProductFilter": {
        "include": ["SPONSORED_BRANDS"]
    }
}
```

#### Delete Campaign

```json
{
  "campaignIds": [
    "520852808634526"
  ]
}
```

### Ad Group

#### Create Ad Group

```json
{
    "adGroups": [{
        "adProduct": "SPONSORED_BRANDS",
        "name": "tomTests18",
        "state": "PAUSED",
        "campaignId": "475081133189954"
    }]
}
```

#### Update Ad Group

```json
{
    "adGroups": [{
        "adGroupId": "377556810682175",
        "state": "ENABLED"
    }]
}
```

#### Query Ad Group

```json
{
    "adGroupIdFilter": {
        "include": ["377556810682175"]
    },
    "adProductFilter": {
        "include": ["SPONSORED_BRANDS"]
    }
}
```

#### Delete Ad Group

```json
{
  "adGroupIds": [
    "475077903840424"
  ]
}
```

### Ad

#### Create Ad

```json
{
    "ads": [{
        "adProduct": "SPONSORED_BRANDS",
        "name": "tomTests19",
        "state": "PAUSED",
        "adGroupId": "377556810682175",
        "adType": "COMPONENT",
        "creative": {
                "componentCreative": {
                    "productVideoSettings": {
                        "products": [
                            {
                                "productId": "B003UKM9CO",
                                "productIdType": "ASIN"
                            }
                        ],
                        "videos": [
                            {
                                "assetId": "amzn1.assetlibrary.asset1.ae70d570b6209af0cb52f1bce8799e7c",
                                "assetVersion": "version_v1"
                            }
                        ]
                    }
                }
            }
    }]
}
```

#### Update Ad

```json
{
    "ads": [{
        "adId": "464024095499842",
        "state": "ENABLED"
    }]
}
```

#### Query Ad

```json
{
    "adIdFilter": {
        "include": ["464024095499842"]
    },
    "adProductFilter": {
        "include": ["SPONSORED_BRANDS"]
    }
}
```

#### Delete Ad

```json
{
  "adIds": [
    "334613205706295"
  ]
}
```

### Target

#### Create Target

```json
{
  "targets": [
    {
      "adProduct": "SPONSORED_BRANDS",
      "negative": false,
      "state": "PAUSED",
      "targetDetails": {
        "themeTarget": {
            "matchType": "KEYWORDS_RELATED_TO_YOUR_BRAND"
        }
      },
      "bid" : {
        "bid": 10
      },
      "targetType": "THEME",
      "adGroupId": "377556810682175"
    }
  ]
}
```

#### Update Target

```json
{
  "targets": [{
      "targetId": "456072297638168",
      "state": "ENABLED"
    }]
}
```

#### Query Target

```json
{
    "targetIdFilter": {
        "include": ["456072297638168"]
    },
    "adProductFilter": {
        "include": ["SPONSORED_BRANDS"]
    }
}
```

#### Delete Target

```json
{
  "targetIds": [
    "456072297638168"
  ]
}
```

## Amazon DSP

### Campaign

#### Create Campaign

```json
{
    "campaigns": [
        {
            "adProduct": "AMAZON_DSP",
            "name": "Test DSP Campaign",
            "state": "PAUSED",
            "marketplaces": [
                "US"
            ],
            "flights": [
                {
                    "budget": {
                        "budgetType": "MONETARY",
                        "budgetValue": {
                            "monetaryBudgetValue": {
                                "monetaryBudget": {
                                    "value": 45.0
                                }
                            }
                        }
                    },
                    "endDateTime": "2025-08-10T23:59:59Z",
                    "startDateTime": "2025-08-04T04:00:00Z"
                }
            ],
            "optimizations": {
                "bidSettings": {
                    "bidStrategy": "SPEND_BUDGET_IN_FULL"
                },
                "budgetSettings": {
                    "budgetAllocation": "MANUAL"
                },
                "goalSettings": {
                    "kpi": "REACH"
                }
            }
        }
    ]
}
```

#### Update Campaign

```json
{
    "campaigns": [
        {
            "campaignId": "583135750850324001",
            "flights": [
                {
                    "budget": {
                        "budgetType": "MONETARY",
                        "budgetValue": {
                            "monetaryBudgetValue": {
                                "monetaryBudget": {
                                    "value": 40.0
                                }
                            }
                        }
                    },
                    "endDateTime": "2025-08-10T23:59:59Z",
                    "startDateTime": "2025-08-04T04:00:00Z"
                }
            ]
        }
    ]
}
```

#### Query Campaign

```json
{
    "campaignIdFilter": {
        "include": [
            "583135750850324001"
        ]
    },
    "adProductFilter": {
        "include": [
            "AMAZON_DSP"
        ]
    }
}
```

### Ad Group

#### Create Ad Group

```json
{
    "adGroups": [
        {
            "adProduct": "AMAZON_DSP",
            "advertisedProductCategoryIds": [
                "324102335054453737"
            ],
            "bid": {
                "baseBid": 18.45
            },
            "campaignId": "583135750850324001",
            "creativeRotationType": "RANDOM",
            "endDateTime": "2025-08-09T23:59:59Z",
            "inventoryType": "STREAMING_TV",
            "name": "test Dsp Ad Group Create",
            "optimization": {
                "bidStrategy": "USE_CAMPAIGN_STRATEGY",
                "budgetSettings": {
                    "budgetAllocation": "MANUAL"
                }
            },
            "pacing": {
                "deliveryProfile": "PACE_AHEAD"
            },
            "budgets": [
                {
                    "budgetType": "MONETARY",
                    "budgetValue": {
                        "monetaryBudgetValue": {
                            "monetaryBudget": {
                                "currencyCode": "USD",
                                "value": 20.7
                            }
                        }
                    },
                    "recurrenceTimePeriod": "LIFETIME"
                }
            ],
            "startDateTime": "2025-08-04T04:00:00Z",
            "state": "PAUSED",
            "tags": [],
            "targetingSettings": {
                "amazonViewability": {
                    "includeUnmeasurableImpressions": false,
                    "viewabilityTier": "ALL_TIERS"
                },
                "defaultAudienceTargetingMatchType": "EXACT",
                "enableLanguageTargeting": false,
                "timeZoneType": "VIEWER",
                "userLocationSignal": "MULTIPLE_SIGNALS",
                "videoCompletionTier": "ALL_TIERS"
            }
        }
    ]
}
```

#### Update Ad Group

```json
{
    "adGroups": [{
        "adGroupId": "591846449645415195",
        "state": "ENABLED"
    }]
}
```

#### Query Ad Group

```json
{
    "adGroupIdFilter": {
        "include": ["591846449645415195"]
    },
    "adProductFilter": {
        "include": ["AMAZON_DSP"]
    }
}
```

### Ad

#### Create Ad

```json
{
  "ads": [
    {
      "adProduct": "AMAZON_DSP",
      "adType": "COMPONENT",
      "creative": {
        "componentCreative": {
          "brandStoreSettings": {
            "bodyText": [
              "BODY_TEXT_1"
            ],
            "brand": "TEST_DSP_BRAND_NAME",
            "callToActions": {
              "brandStoreCallToActionSettings": {
                "callToActionType": [
                  "SHOP_NOW"
                ],
                "url": "https://www.amazon.com/stores/page/4EBB17C3-A9F1-4F22-B988-45B760A21BDA"
              }
            },
            "clickTrackingUrls": [
              {
                "url": "https://www.amazon.com/2"
              }
            ],
            "headlines": [
              "HEADLINE_1"
            ],
            "inventoryTypes": [
              "NATIVE"
            ],
            "language": "en_US",
            "optimizationGoalKpi": "PURCHASE_RATE",
            "responsiveSizingBehavior": "DISABLED",
            "squareImages": [
              {
                "assetId": "amzn1.assetlibrary.asset1.7b13b1c596f946ff8417a62136e0e936",
                "assetVersion": "version_v1",
                "formatProperties": [
                  {
                    "height": 100,
                    "left": 10,
                    "top": 10,
                    "width": 100
                  }
                ]
              }
            ],
            "tallImages": [
              {
                "assetId": "amzn1.assetlibrary.asset1.7b13b1c596f946ff8417a62136e0e936",
                "assetVersion": "version_v1",
                "formatProperties": [
                  {
                    "height": 177,
                    "left": 10,
                    "top": 10,
                    "width": 100
                  }
                ]
              }
            ],
            "wideImages": [
              {
                "assetId": "amzn1.assetlibrary.asset1.7b13b1c596f946ff8417a62136e0e936",
                "assetVersion": "version_v1",
                "formatProperties": [
                  {
                    "height": 100,
                    "left": 10,
                    "top": 10,
                    "width": 191
                  }
                ]
              }
            ]
          }
        }
      },
      "marketplaces": [
        "US"
      ],
      "name": "Public API ADSP Test AD",
      "state": "ENABLED"
    }
  ]
}
```

#### Update Ad

```json
{
    "ads": [{
        "adId": "583817600028957485",
        "creative": {
            "componentCreative": {
                "brandStoreSettings": {
                    "bodyText": [
                        "BODY_TEXT_1"
                    ],
                    "brand": "TEST_DSP_BRAND_NAME",
                    "callToActions": {
                        "brandStoreCallToActionSettings": {
                            "callToActionType": [
                                "SHOP_NOW"
                            ],
                            "url": "https://www.amazon.com/stores/page/4EBB17C3-A9F1-4F22-B988-45B760A21BDA"
                        }
                    },
                    "clickTrackingUrls": [{
                        "url": "https://www.amazon.com/2"
                    }],
                    "headlines": [
                        "HEADLINE_1_updated3"
                    ],
                    "impressionTrackingUrls": [],
                    "inventoryTypes": [
                        "NATIVE"
                    ],
                    "language": "en_US",
                    "optimizationGoalKpi": "PURCHASE_RATE",
                    "responsiveSizingBehavior": "ENABLED",
                    "squareImages": [{
                        "assetId": "amzn1.assetlibrary.asset1.7b13b1c596f946ff8417a62136e0e936",
                        "assetVersion": "version_v1",
                        "formatProperties": [{
                            "height": 100,
                            "left": 10,
                            "top": 10,
                            "width": 100
                        }]
                    }],
                    "tallImages": [{
                        "assetId": "amzn1.assetlibrary.asset1.7b13b1c596f946ff8417a62136e0e936",
                        "assetVersion": "version_v1",
                        "formatProperties": [{
                            "height": 177,
                            "left": 10,
                            "top": 10,
                            "width": 100
                        }]
                    }],
                    "wideImages": [{
                        "assetId": "amzn1.assetlibrary.asset1.7b13b1c596f946ff8417a62136e0e936",
                        "assetVersion": "version_v1",
                        "formatProperties": [{
                            "height": 100,
                            "left": 10,
                            "top": 10,
                            "width": 191
                        }]
                    }]
                }
            }
        },
        "name": "Public API ADSP Test AD",
        "state": "ENABLED"
    }]
}
```

#### Query Ad

```json
{
    "adIdFilter": {
        "include": ["583817600028957485"]
    },
    "adProductFilter": {
        "include": ["AMAZON_DSP"]
    }
}
```

### Target

#### Create Target

```json
{
  "targets": [
    {
      "adGroupId": "591846449645415195",
      "adProduct": "AMAZON_DSP",
      "negative": false,
      "state": "PAUSED",
      "targetDetails": {
        "keywordTarget": {
          "keyword": "test_DSP_keyword",
          "matchType": "BROAD"
        }
      },
      "targetType": "KEYWORD"
    }
  ]
}
```

#### Query Target

```json
 {
  "adProductFilter": {
    "include": [
      "AMAZON_DSP"
    ]
  },
  "targetIdFilter": {
    "include": [
      "1299189929492847106"
    ]
  }
}
```

#### Delete Target

```json
{
  "targetIds": [
    "1299189929492847106"
  ]
}
```




