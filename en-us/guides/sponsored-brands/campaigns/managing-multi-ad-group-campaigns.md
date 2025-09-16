---
title: Managing Sponsored Brands campaigns
description: A tutorial for managing multi-ad group campaigns for Sponsored Brands, with example requests and responses.
type: guide
interface: api
tags: 
    - Sponsored Brands
    - Campaign management
keywords:
    - multiple ad groups
---

# Managing campaigns

>[NOTE] The advertising console will start using multi-ad group campaigns starting in September 2022, replacing the legacy campaign creation process. In order to retrieve full details for campaigns created in the ad console, you need to use the new POST /sb/v4/campaigns/list endpoint and `"creativeType": "all"` filter for reporting.

Jump to: [Campaigns](#campaigns) | [Ad groups](#ad-groups) | [Ads](#ads) | [Creatives](#creatives) | [Reports](#reports)

## Campaigns

Version 4 supports create, update, read, and delete options for campaigns. 

### Listing campaigns

See details for campaigns using the [POST /sb/v4/campaigns/list](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/ListSponsoredBrandsCampaigns) endpoint.

**Sample request**

```shell
curl --location --request POST 'https://advertising-api.amazon.com/sb/v4/campaigns/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
--header 'Accept: application/vnd.sbcampaignresource.v4+json' \
--header 'Content-Type: text/plain' \
--data-raw '{
  "includeExtendedDataFields": true,
  "stateFilter": {
    "include": []
  },
  "nameFilter": {
    "include": [],
    "queryTermMatchType": "BROAD_MATCH"
  },
  "campaignIdFilter": {
    "include": [
      "63665260422798"
    ]
  },
  "maxResults": 10
}'
```

**Sample response**

```json
{
  "campaigns": [
    {
      "bidding": {
        "bidOptimizationStrategy": "MAXIMIZE_IMMEDIATE_SALES"
      },
      "budget": 100.0,
      "budgetType": "DAILY",
      "campaignId": "63665260422798",
      "extendedData": {
        "creationDate": 1644332919536,
        "lastUpdateDate": 1644332919536,
        "servingStatus": "CAMPAIGN_INCOMPLETE"
      },
      "isMultiAdGroupsEnabled": true,
      "name": "create campaign test",
      "startDate": "2022-02-08",
      "state": "ENABLED"
    }
  ],
  "totalCount": 1
}
```

### Updating campaigns

Update a multi-ad group campaign using [PUT /sb/v4/campaigns](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/UpdateSponsoredBrandsCampaigns) endpoint. 

 
**Sample request**


```shell
curl --location --request PUT 'https://advertising-api.amazon.com/sb/v4/campaigns' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
--header 'Accept: application/vnd.sbcampaignresource.v4+json' \
--header 'Content-Type: text/plain' \
--data-raw '[
  {
    "campaignId": 63665260422798,
    "budget": 50
  }
]'
```

### Deleting campaigns

Delete multi-ad group campaigns, using the [POST /sb/v4/campaigns/delete](sponsored-brands/3-0/openapi/prod#tag/Campaigns/operation/DeleteSponsoredBrandsCampaigns) endpoint. 

**Sample request**

```shell
curl --location --request POST 'https://advertising-api.amazon.com/sb/v4/campaigns/delete' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
--header 'Accept: application/vnd.sbcampaignresource.v4+json' \
--header 'Content-Type: text/plain' \
--data-raw '[
  {
  "campaignIdFilter": {
    "include": [
      "1234567890"
    ]
  }
}

```

## Ad groups

### Listing ad groups

List ad groups that you want to update using the [POST /sb/v4/adGroups/list](sponsored-brands/3-0/openapi/prod#tag/Ad-Groups/operation/ListSponsoredBrandsAdGroups) endpoint.

**Sample request**

```shell
curl --location --request POST 'https://advertising-api.amazon.com/sb/v4/adGroups/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxxxx' \
--header 'Accept: application/vnd.sbadgroupresource.v4+json' \
--header 'Content-Type: text/plain' \
--data-raw '{
  "includeExtendedDataFields": true,
  "campaignIdFilter": {
    "include": []
  },
  "stateFilter": {
    "include": [
      "ENABLED"
    ]
  },
  "maxResults": 10,
  "adGroupIdFilter": {
    "include": [
    "35407526816553"
    ]
  },
  "nameFilter": {
    "queryTermMatchType": "EXACT_MATCH",
    "include": []
  }
}'
```

**Sample response**

```json
{
  "adGroups": [
    {
      "adGroupId": "35407526816553",
      "campaignId": "63665260422798",
      "extendedData": {
        "creationDate": 1644334666767,
        "lastUpdateDate": 1644334666767,
        "servingStatus": "CAMPAIGN_INCOMPLETE"
      },
      "name": "create ad group test 2",
      "state": "ENABLED"
    }
  ],
  "totalResults": 1
}
```

### Updating ad groups

Update ad group status using the [PUT /sb/v4/adGroups](sponsored-brands/3-0/openapi/prod#tag/Ad-Groups/operation/UpdateSponsoredBrandsAdGroups) endpoint.

**Sample request**

```shell
curl --location --request PUT 'https://advertising-api.amazon.com/sb/v4/adGroups' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Accept: application/vnd.sbadgroupresource.v4+json' \
--header 'Content-Type: text/plain' \
--data-raw '{
  "adGroups": [
    {
      "adGroupId": "35407526816553",
      "state": "PAUSED",
      "name": "New ad group name",
    }
  ]
}'
```

**Sample response**

```json
{
  "adGroups": {
    "error": [],
    "success": [
      {
        "adGroup": {
          "adGroupId": "35407526816553",
          "campaignId": "63665260422798",
          "extendedData": {
            "creationDate": 1644334666767,
            "lastUpdateDate": 1644336561287,
            "servingStatus": "CAMPAIGN_INCOMPLETE"
          },
          "name": "New ad group name",
          "state": "PAUSED"
        },
        "adGroupId": "35407526816553",
        "index": 0
      }
    ]
  }
}
```

### Deleting ad groups

Use the [POST /sb/v4/adGroups/delete](sponsored-brands/3-0/openapi/prod#tag/Ad-Groups/operation/DeleteSponsoredBrandsAdGroups) endpoint to archive an endpoint. 

**Sample request**

```shell
curl --location --request POST 'https://advertising-api.amazon.com/sb/v4/adGroups/delete' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
--header 'Accept: application/vnd.sbadgroupresource.v4+json' \
--header 'Content-Type: text/plain' \
--data-raw '{
  "adGroupIdFilter": {
    "include": [
      "35407526816553"
    ]
  }
}'
```

**Sample response**

```json
{
  "adGroups": {
    "error": [],
    "success": [
      {
        "adGroup": {
          "adGroupId": "35407526816553",
          "campaignId": "63665260422798",
          "extendedData": {
            "creationDate": 1644334666767,
            "lastUpdateDate": 1644336599743,
            "servingStatus": "CAMPAIGN_INCOMPLETE"
          },
          "name": "Test campaign",
          "state": "ARCHIVED"
        },
        "adGroupId": "35407526816553",
        "index": 0
      }
    ]
  }
}
```

## Ads

### Listing ads

Use [POST /sb/v4/ads/list](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/ListSponsoredBrandsAds) Endpoint to list the ads. The list endpoint returns ads created using both [version 3](sponsored-brands/3-0/openapi#/Campaigns) and [version 4](sponsored-brands/3-0/openapi/prod#/Ads). 

>[NOTE] Ads created using the version 3 POST sb/campaigns endpoint will not include an `adId` or a `name`.

>[WARNING] Some advertisers have experienced a known issue where older creatives are missing creative-level fields. To ensure you receive all creative-level fields, we suggest uploading a more recent version of the creative asset. 

**Sample request: all ads**

```shell
curl --location --request POST 'https://advertising-api.amazon.com/sb/v4/ads/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxxxx' \
--header 'Accept: application/vnd.sbadresource.v4+json' 
```

**Sample response**

In the following example response, the first returned ad was created as part of version 3 campaign, so it does not contain an `adId` or ad `name`. The second ad was created as part of a version 4/multi-ad group campaign, so it contains both an `adId` and an ad `name`.

```json
{
    "ads": [
        {
            "adGroupId": "144346139432941903",
            "campaignId": "144327786111035248",
            "creative": {
                "asins": [
                    "B07R7V6KS8",
                    "B07X2X4D5H",
                    "B003L1717K"
                ],
                "brandLogoAssetID": "amzn1.assetlibrary.asset1.xxxxxxxxxxxx1",
                "brandLogoUrl": "https://m.media-amazon.com/images/S/al-na-184b9306-7f8a/xxxxxxxxxxxxx.png",
                "brandName": "IM Brand",
                "headline": "AA AAA High-Capacity Rechargeable Batteries",
                "type": "PRODUCT_COLLECTION"
            },
            "extendedData": {
                "creationDate": 1635884791000,
                "lastUpdateDate": 1635884791000,
                "servingStatus": "UNKNOWN"
            },
            "landingPage": {
                "pageType": "PRODUCT_LIST",
                "url": "https://development.amazon.com/stores/page/xxxxxxxxxxxxxx"
            },
            "state": "ENABLED"
        },
        {
            "adGroupId": "238010898977121",
            "adId": "7278627757134",
            "campaignId": "177829948461538",
            "creative": {
                "asins": [
                    "B07R7V6KS8"
                ],
                "type": "VIDEO",
                "videoAssetIds": [
                    "amzn1.assetlibrary.asset1.xxxxxxxxxxxxxx"
                ]
            },
            "extendedData": {
                "creationDate": 1665435788770,
                "lastUpdateDate": 1665435791462,
                "servingStatus": "AD_POLICING_PENDING_REVIEW"
            },
            "landingPage": {
                "pageType": "DETAIL_PAGE",
                "url": "https://www.amazon.com/dp/B0000000000"
            },
            "name": "Test video ad",
            "state": "ENABLED"
        }
    ],
    "totalResults": 2
}
```

**Sample request: filter by `adId`**

You can use filters to return specific ads by `adId`. 

```shell
curl --location --request POST 'https://advertising-api.amazon.com/sb/v4/ads/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxxxx' \
--header 'Accept: application/vnd.sbadresource.v4+json' \
--data-raw '
{  
  "maxResults": 10,
  "adIdFilter": {
    "include": [
      "209123071503234", "277277580302345", "158961813536049"
    ]
  }
}'
```

**Sample response**

```json
{
    "ads": [
        {
            "adGroupId": "281221120502625",
            "adId": "209123071503234",
            "campaignId": "10391599853578",
            "creative": {
                "asins": [
                    "B00MCW8MGI",
                    "B07BWBQ66J",
                    "B07F6FSDHS"
                ],
                "brandLogoAssetID": "amzn1.assetlibrary.asset1.fcf9b0ad0ac4cfedcd647b53b0f9c14f:version_v1",
                "brandName": "test brand name",
                "headline": "SB v2 product collection ad",
                "type": "PRODUCT_COLLECTION"
            },
            "extendedData": {
                "creationDate": 0,
                "lastUpdateDate": 0
            },
            "landingPage": {
                "pageType": "PRODUCT_LIST",
                "url": "https://development.amazon.com/stores/page/C3769F1F-3A90-4357-8678-258CF10FB46A"
            },
            "name": "test product collection ad",
            "state": "ENABLED"
        },
        {
            "adGroupId": "73487879298399",
            "adId": "277277580302345",
            "campaignId": "10391599853578",
            "creative": {
                "asins": [
                    "B00DLDH1N2"
                ],
                "type": "VIDEO",
                "videoAssetIds": [
                    "amzn1.assetlibrary.asset1.f976557494bbce76a848044e2cf32890:version_v1"
                ]
            },
            "extendedData": {
                "creationDate": 0,
                "lastUpdateDate": 0
            },
            "landingPage": {
                "pageType": "DETAIL_PAGE",
                "url": "https://www.amazon.com/dp/B00DLDH1N2"
            },
            "name": "test video ad",
            "state": "ENABLED"
        },
        {
            "adGroupId": "281221120502625",
            "adId": "158961813536049",
            "campaignId": "10391599853578",
            "creative": {
                "brandLogoAssetID": "amzn1.assetlibrary.asset1.1db463836cdc85be5e5ad73c3b73557c:version_v1",
                "brandName": "Test",
                "headline": "Create new buildings with Lego",
                "subpages": [
                    {
                        "asin": "B005K5GQ9O",
                        "pageTitle": "cool lego buildings with permissions",
                        "url": "https://development.amazon.com/stores/page/DE343B73-427D-4600-B645-338AFA3D36AE"
                    },
                    {
                        "asin": "B00EQ7LZSY",
                        "pageTitle": "lego cars",
                        "url": "https://development.amazon.com/stores/page/EA9BE582-5996-45B6-BD7E-C2BBCEA5668D"
                    },
                    {
                        "asin": "B08F2S3QW5",
                        "pageTitle": "fantasy legos",
                        "url": "https://development.amazon.com/stores/page/B03C3B80-78DA-4978-AECC-3E7A5CE2CDA2"
                    }
                ],
                "type": "STORE_SPOTLIGHT"
            },
            "extendedData": {
                "creationDate": 0,
                "lastUpdateDate": 0
            },
            "landingPage": {
                "pageType": "STORE",
                "url": "https://development.amazon.com/stores/page/B63FC824-E7AF-4B24-B898-83B4B46B3F15"
            },
            "name": "test store spotlight ad",
            "state": "ENABLED"
        }
    ],
    "totalResults": 3
}
```

### Updating ads

Use the [PUT /sb/v4/ads](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/UpdateSponsoredBrandsAds) endpoint to update the status or name of an ad. 

**Sample request**

```shell
curl --location --request PUT 'https://advertising-api.amazon.com/sb/v4/ads' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Accept: application/vnd.sbadresource.v4+json' \
--header 'Content-Type: text/plain' \
--data-raw '{
  "ads": [
    {
      "adId": "277277580302345",
      "name": "New ad name",
      "state": "PAUSED"
    }
  ]
}

'
```

**Sample response**

```json
{
    "ads": {
        "error": [],
        "success": [
            {
                "ad": {
                    "adGroupId": "73487879298399",
                    "adId": "277277580302345",
                    "campaignId": "10391599853578",
                    "creative": {
                        "asins": [
                            "B00DLDH1N2"
                        ],
                        "type": "VIDEO",
                        "videoAssetIds": [
                            "amzn1.assetlibrary.asset1.f976557494bbce76a848044e2cf32890:version_v1"
                        ]
                    },
                    "extendedData": {
                        "creationDate": 0,
                        "lastUpdateDate": 0
                    },
                    "landingPage": {
                        "pageType": "DETAIL_PAGE",
                        "url": "https://www.amazon.com/dp/B00DLDH1N2"
                    },
                    "name": "New ad name",
                    "state": "PAUSED"
                },
                "adId": "277277580302345",
                "index": 0
            }
        ]
    }
}
```

### Deleting ads

Use the [POST /sb/v4/ads/delete](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/DeleteSponsoredBrandsAds) endpoint to delete an ad. 

**Sample request**

```shell
curl --location --request POST 'https://advertising-api.amazon.com/sb/v4/ads/delete' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxx' \
--header 'Accept: application/vnd.sbadresource.v4+json' \
--header 'Content-Type: text/plain' \
--data-raw '{
  "adIdFilter": {
    "include": [
      "158961813536049"
    ]
  }
}

'
```

**Sample response**

```json
{
    "ads": {
        "error": [],
        "success": [
            {
                "ad": {
                    "adGroupId": "281221120502625",
                    "adId": "158961813536049",
                    "campaignId": "10391599853578",
                    "creative": {
                        "brandLogoAssetID": "amzn1.assetlibrary.asset1.1db463836cdc85be5e5ad73c3b73557c:version_v1",
                        "brandName": "Test",
                        "headline": "Create new buildings with Lego",
                        "subpages": [
                            {
                                "asin": "B005K5GQ9O",
                                "pageTitle": "cool lego buildings with permissions",
                                "url": "https://development.amazon.com/stores/page/DE343B73-427D-4600-B645-338AFA3D36AE"
                            },
                            {
                                "asin": "B00EQ7LZSY",
                                "pageTitle": "lego cars",
                                "url": "https://development.amazon.com/stores/page/EA9BE582-5996-45B6-BD7E-C2BBCEA5668D"
                            },
                            {
                                "asin": "B08F2S3QW5",
                                "pageTitle": "fantasy legos",
                                "url": "https://development.amazon.com/stores/page/B03C3B80-78DA-4978-AECC-3E7A5CE2CDA2"
                            }
                        ],
                        "type": "STORE_SPOTLIGHT"
                    },
                    "extendedData": {
                        "creationDate": 0,
                        "lastUpdateDate": 0
                    },
                    "landingPage": {
                        "pageType": "STORE",
                        "url": "https://development.amazon.com/stores/page/B63FC824-E7AF-4B24-B898-83B4B46B3F15"
                    },
                    "name": "test store spotlight ad",
                    "state": "ARCHIVED"
                },
                "adId": "158961813536049",
                "index": 0
            }
        ]
    }
}
```

## Creatives

### Listing creatives

Gets an array of all Sponsored Brands creatives that qualify the given resource identifiers and filters. 

**Sample request**

```shell
curl --location --request POST 'https://advertising-api.amazon.com/sb/ads/creatives/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxx' \
--header 'Authorization: Bearer xxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxxxxxx' \
--header 'Accept: application/vnd.sbAdCreativeResource.v4+json' \
--header 'Content-Type: text/plain' \
--data-raw '{
    "adId":"6972682587490",
    "maxResults":1
}'
```

**Sample response**

```json
{
    "creatives": [{
        "adId": "1234567890",
        "creationTime": 1656026684.349,
        "creativeProperties": {
            "asins": [
                "B0000000001"
            ],
            "brandLogoAssetId": "amzn1.assetlibrary.asset1.abcdefghijklmn:version_v1",
            "brandLogoCrop": {
                "height": 400,
                "left": 0,
                "top": 0,
                "width": 400
            },
            "brandLogoUrl": "https://m.media-amazon.com/images/S/al-na-some0-image.jpeg",
            "brandName": "Test Brand",
            "headline": "Test Headline",
            "landingPage": {
                "type": "STORE",
                "value": "https://www.amazon.com/stores/page/1111111-222222-AAAA-BBBB-333333333333"
            },
          "videoAssetIds": ["amzn1.assetlibrary.asset1.abcdefghijklmn:version_v1"]
        },
        "creativeStatus": "PENDING_FOR_MODERATION",
        "creativeType": "BRAND_VIDEO",
        "creativeVersion": "2"
    }],
    "totalResults": 1
}
```

### Updating video creatives

Creates a new version of an existing video creative for given Sponsored Brands video ad by supplying new video creative content.

**Sample request**

```shell
curl --location --request POST 'https://advertising-api.amazon.com/sb/ads/creatives/video' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxx' \
--header 'Accept: application/vnd.sbAdCreativeResource.v4+json' \
--header 'Content-Type: text/plain' \
--data-raw '{
  "adId": "6972682587490",
  "creative": {
    "videoAssetIds": [
      "amzn1.assetlibrary.asset1.abcdefghijklmn:version_v1"
    ]
  }
}'
```

**Sample response**

```json
{
    "adId": "1234567890",
    "creativeVersion": "2"
}
```

### Updating brand video creatives

Creates a new version of an existing video creative for given Sponsored Brands brand video ad by supplying new video creative content.


**Sample request**

```shell
curl --location --request POST 'https://advertising-api.amazon.com/sb/ads/creatives/brandVideo' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza| xxxxxxxxxxxx' \
--header 'Accept: application/vnd.sbAdCreativeResource.v4+json' \
--header 'Content-Type: text/plain' \
--data-raw '{
  "adId": "1234567890",
  "creative": {
    "asins": [
      "B000000001"
    ],
    "brandName": "Test Brand Name",
    "videoAssetIds": [
      "amzn1.assetlibrary.asset1.abcdefghijklmn:version_v1"
    ],
    "brandLogoAssetId": "amzn1.assetlibrary.asset1.abcdefghijklmn:version_v1",
    "headline": "Test Headline"
  }
}'
```

**Sample response**

```json
{
    "adId": "1234567890",
    "creativeVersion": "2"
}
```

## Reports

Use the version 2 reporting endpoint: [POST /v2/hsa/{reportType}/report](sponsored-brands/3-0/openapi#/Reports).

**Sample request body**

```json
{
    "reportDate": "20220101",
    "creativeType": "all",
    "metrics": "clicks,campaignName,campaignId,adGroupName,adGroupId,adId,impressions,cost,dpv14d,attributedDetailPageViewsClicks14d,attributedOrdersNewToBrand14d,attributedOrdersNewToBrandPercentage14d,attributedOrderRateNewToBrand14d,attributedSalesNewToBrand14d,attributedSalesNewToBrandPercentage14d,attributedUnitsOrderedNewToBrand14d,attributedUnitsOrderedNewToBrandPercentage14d"
}
```

**Sample response**

```json
{
    "reportId": "amzn1.clicksAPI.v1.p1.620D5F4E.ba672e9e-0ceb-4982-8bb3-318d90b9a526",
    "recordType": "ad",
    "status": "IN_PROGRESS",
    "statusDetails": "Report is being generated."
}
```



