---
title: Sponsored TV managing campaign
description: Sponsored TV getting started with details on how manage campaigns.
type: guide
interface: api
keywords:
    - Sponsored TV
    - Manage campaigns
---

# Managing Sponsored TV campaigns

Jump to: [Campaigns](#campaigns) | [Ad groups](#ad-groups) | [Ads](#ads) | [Creatives](#creatives)

## Campaigns

Listed below are the update, read, and delete options for campaigns.

### Listing campaigns

To get a list of Sponsored TV campaigns, use the POST [/st/campaigns/list](sponsored-tv-open-beta#tag/Campaigns/operation/ListSponsoredTvCampaigns) endpoint. 

>[NOTE] For the `nameFilter` object, only one entry is allowed in the `include` array if `queryTermMatchType` is set to `BROAD_MATCH`.

Sample request: 

```json
curl --location --request POST 'https://advertising-api.amazon.com/st/campaigns/list' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data-raw '{
  "campaignIdFilter": {
    "include": [
      "{{campaignId}}"
    ]
  },
  "stateFilter": {
    "include": [
      "ENABLED"
    ]
  },
  "maxResults": 1,
  "nameFilter": {
    "queryTermMatchType": "BROAD_MATCH",
    "include": [
      "Campaign TV"
    ]
  }
}'
```

Sample response:

```json
{
  "campaigns": [
    {
      "budgetSettings": {
        "budget": {
          "recurrenceType": "DAILY",
          "budgetValue": {
            "amount": 0.1,
            "budgetCurrencyCode": "USD"
          }
        }
      },
      "endDate": "2019-08-24T14:15:22Z",
      "campaignId": "string",
      "costType": "string",
      "name": "string",
      "state": "ENABLED",
      "startDate": "2019-08-24T14:15:22Z",
      "extendedData": {
        "servingStatus": "ADVERTISER_STATUS_ENABLED",
        "lastUpdateDate": "2019-08-24T14:15:22Z",
        "creationDate": "2019-08-24T14:15:22Z"
      },
      "tags": {
        "property1": "string",
        "property2": "string"
      }
    }
  ],
  "nextToken": "string",
  "totalCount": 0
}

```

### Updating campaigns

To update a campaign, use the PUT [/st/campaigns/](sponsored-tv-open-beta#tag/Campaigns/operation/UpdateSponsoredTvCampaigns) endpoint.

Sample request: 

```json
curl --location --request PUT 'https://advertising-api.amazon.com/st/campaigns' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data-raw '{
    "campaigns": [{
     "campaignId": "campaignId",
     "budgetSettings": {
         "budget": {
             "recurrenceType": "DAILY",
             "budgetValue": {
                 "amount": 500.00,
                 "budgetCurrencyCode": "USD"
             }
         }
     },
     "startDate": "2023-08-23T14:20:36.555Z",
     "endDate": "2023-08-23T14:20:36.555Z",
     "name": "campaign name",
     "state": "ENABLED",
     "tags": {
         "additionalProp1": "value1",
         "additionalProp2": "value2",
         "additionalProp3": "value3"
     }
 }]
}'
```

Sample response: 

```json
{
    "campaigns": {
        "error": [],
        "success": [{
            "campaignId": "string",
            "index": 0,
            "campaign": {
                "budgetSettings": {
                    "budget": {
                        "recurrenceType": "DAILY",
                        "budgetValue": {
                            "amount": 500.00,
                            "budgetCurrencyCode": "USD"
                        }
                    }
                },
                "endDate": "2023-08-23T14:20:36.555Z",
                "campaignId": "string",
                "costType": "CPM",
                "name": "string",
                "state": "ENABLED",
                "startDate": "2023-08-23T14:20:36.555Z",
                "extendedData": {
                    "servingStatus": "ADVERTISER_STATUS_ENABLED",
                    "lastUpdateDate": "2023-08-23T14:20:36.555Z",
                    "creationDate": "2023-08-23T14:20:36.555Z"
                },
                "tags": {
                    "additionalProp1": "value1",
                    "additionalProp2": "value2",
                    "additionalProp3": "value3"
                }
            }
        }]
    }
}
```

### Deleting campaigns

To delete a campaign, use the POST [/st/campaigns/delete](sponsored-tv-open-beta#tag/Campaigns/operation/DeleteSponsoredTvCampaigns) endpoint.

Sample request: 

```json
curl --location --request POST 'https://advertising-api.amazon.com/st/campaigns/delete' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data-raw '{
    "campaignIdFilter": {
        "include": [
            "123456789123456"
        ]
    }
}'
```

Sample response:

```json
{
    "campaigns": {
        "error":[],
        "success": [{
            "campaignId": "string",
            "index": 99,
            "campaign": {
                "budgetSettings": {
                    "budget": {
                        "recurrenceType": "DAILY",
                        "budgetValue": {
                            "amount": 0.1,
                            "budgetCurrencyCode": "USD"
                        }
                    }
                },
                "endDate": "2019-08-24T14:15:22Z",
                "campaignId": "string",
                "costType": "string",
                "name": "string",
                "state": "ENABLED",
                "startDate": "2019-08-24T14:15:22Z",
                "extendedData": {
                    "servingStatus": "ADVERTISER_STATUS_ENABLED",
                    "lastUpdateDate": "2019-08-24T14:15:22Z",
                    "creationDate": "2019-08-24T14:15:22Z"
                },
                "tags": {
                    "property1": "string",
                    "property2": "string"
                }
            }
        }]
    }
}
```

## Ad groups

### Listing ad groups

To get a list of Sponsored TV ad groups, use the POST [/st/adGroups/list](sponsored-tv-open-beta#tag/Campaigns/operation/ListSponsoredTvAdGroups) endpoint. 

>[NOTE] For the `nameFilter` object, only one entry is allowed in the `include` array if `queryTermMatchType` is set to `BROAD_MATCH`.

Sample request: 

```json
curl --location --request POST 'https://advertising-api.amazon.com/st/adGroups/list' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data-raw '{
  "campaignIdFilter": {
    "include": [
      "{{campaignId}}"
    ]
  },
  "stateFilter": {
    "include": [
      "ENABLED"
    ]
  },
  "adGroupIdFilter": {
    "include": [
      "{{adGroupId}}"
    ]
  },
  "maxResults": 1,
  "nameFilter": {
    "queryTermMatchType": "BROAD_MATCH",
    "include": [
      "Ad group TV"
    ]
  }
}'
```

Sample response:

```json
{
    "adGroups": {
        "error": [],
        "success": [{
            "adGroup": {
                "campaignId": "string",
                "name": "string",
                "state": "ENABLED",
                "adGroupId": "string",
                "defaultBid": {
                    "bid": 0.1
                },
                "extendedData": {
                    "servingStatus": "ACCOUNT_OUT_OF_BUDGET",
                    "lastUpdateDate": "2019-08-24T14:15:22Z",
                    "creationDate": "2019-08-24T14:15:22Z"
                }
            },
            "index": 99,
            "adGroupId": "string"
        }]
    }
}

```

### Updating ad groups

To update an ad group, use the PUT [/st/adGroups/](sponsored-tv-open-beta#tag/AdGroups/operation/UpdateSponsoredTvAdGroups) endpoint.

Sample request:

```json
curl --location --request PUT 'https://advertising-api.amazon.com/st/adGroups' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data-raw '{
  "adGroups": [
    {
      "name": "Name of adGroup",
      "state": "ENABLED",
      "adGroupId": "string",
      "defaultBid": {
        "bid": 15
      }
    }
  ]
}'

```

Sample response:

```json
{
    "adGroups": {
        "error": [],
        "success": [{
            "adGroup": {
                "campaignId": "string",
                "name": "string",
                "state": "ENABLED",
                "adGroupId": "string",
                "defaultBid": {
                    "bid": 15
                },
                "extendedData": {
                    "servingStatus": "STATUS_UNAVAILABLE",
                    "lastUpdateDate": "2023-08-23T14:51:09.940Z",
                    "creationDate": "2023-08-23T14:51:09.940Z"
                }
            },
            "index": 0,
            "adGroupId": "string"
        }]
    }
}
```

### Deleting ad groups

To delete an ad group, use the POST [/st/adsGroups/delete](sponsored-tv-open-beta#tag/AdGroups/operation/DeleteSponsoredTvAdGroups) endpoint which will mark the ad group status as ‘ARCHIVED’.

Sample request:

```json
curl --location 'https://advertising-api.amazon.com/st/adGroups/delete' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data '{
    "adGroupIdFilter": {
        "include": [
             "123456789123456"
        ]
    }
}'
```
Sample response: 

```json
{
    "adGroups": {
        "error": [],
        "success": [
            {
                "adGroup": {
                    "adGroupId": "123456789123456",
                    "campaignId": "123456789123456",
                    "defaultBid": {
                        "bid": 10.0
                    },
                    "name": "string",
                    "state": "ARCHIVED"
                },
                "adGroupId": "123456789123456",
                "index": 0
            }
        ]
    }
}
```

## Ads

### Listing ads

To get a list of Sponsored TV ads, use the POST [/st/ads/list](sponsored-tv-open-beta#tag/Ads/operation/ListSponsoredTvAds) endpoint.

Sample request:

```json
curl --location --request POST 'https://advertising-api.amazon.com/st/ads/list' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data-raw '{
  "campaignIdFilter": {
    "include": [
      "{{campaignId}}"
    ]
  },
  "stateFilter": {
    "include": [
      "ENABLED"
    ]
  },
  "maxResults": 1,
  "adIdFilter": {
    "include": [
      "{{adId}}"
    ]
  },
}'
```

Sample response:

```json
{
  "ads": [
    {
      "adName": "string",
      "adId": "string",
      "landingPageValue": "string",
      "landingPageType": "string",
      "campaignId": "string",
      "state": "ENABLED",
      "adGroupId": "string",
      "extendedData": {
        "servingStatus": "ACCOUNT_OUT_OF_BUDGET",
        "lastUpdateDate": "2019-08-24T14:15:22Z",
        "creationDate": "2019-08-24T14:15:22Z"
      }
    }
  ],
  "nextToken": "string",
  "totalCount": 0
}
```

### Updating ads

To update ads, use the PUT [/st/ads](sponsored-tv-open-beta#tag/Ads/operation/UpdateSponsoredTvAds) endpoint.

Sample request:

```json
curl --location --request PUT 'https://advertising-api.amazon.com/st/ads' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data-raw '{
  "ads": [
    {
      "adName": "name of ad",
      "adId": "{{adId}}",
      "state": "ENABLED"
    }
  ]
}
```

Sample response:

```json
{
    "ads": {
        "error": [],
        "success": [{
            "adId": "string",
            "ad": {
                "adName": "string",
                "adId": "string",
                "landingPageValue": "string",
                "landingPageType": "string",
                "campaignId": "string",
                "state": "ENABLED",
                "adGroupId": "string",
                "extendedData": {
                    "servingStatus": "ACCOUNT_OUT_OF_BUDGET",
                    "lastUpdateDate": "2019-08-24T14:15:22Z",
                    "creationDate": "2019-08-24T14:15:22Z"
                }
            },
            "index": 99
        }]
    }
}
```

### Deleting ads

To delete an ad, use the POST [/st/ads/delete](sponsored-tv-open-beta#tag/Ads/operation/DeleteSponsoredTvAds)

Sample request:

```json
curl --location --request POST 'https://advertising-api.amazon.com/st/ads' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data-raw '{
  "adIdFilter": {
    "include": [
      "{{adId}}"
    ]
  }
}
```

Sample response:

```json
{
  "ads": {
    "error":[],  
    "success": [
      {
        "adId": "string",
        "ad": {
          "adName": "string",
          "adId": "string",
          "landingPageValue": "string",
          "landingPageType": "string",
          "campaignId": "string",
          "state": "ENABLED",
          "adGroupId": "string",
          "extendedData": {
            "servingStatus": "ACCOUNT_OUT_OF_BUDGET",
            "lastUpdateDate": "2019-08-24T14:15:22Z",
            "creationDate": "2019-08-24T14:15:22Z"
          }
        },
        "index": 99
      }
    ]
  }
}
```

## Creatives

### Update creative

To update a Creative, use the PUT [/st/creatives/](sponsored-tv-open-beta#tag/Creatives/operation/UpdateSponsoredTvCreatives) endpoint.

Sample request: 

```json
curl --location --request PUT 'https://advertising-api.amazon.com/st/creatives' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data-raw '{
  "creatives": [
    {
      "assetId": "string",
      "assetVersion": "string",
      "creativeId": "string"
    }
  ]
}'

```
Sample response:

```json
{
    "creatives": {
        "error": [],
        "success": [{
            "creative": {
                "creativeId": "string",
                "assetId": "string",
                "assetVersion": "string",
                "adGroupId": "string",
                "moderationStatus": "APPROVED"
            },
            "index": 0,
            "creativeId": "string"
        }]
    }
}
```