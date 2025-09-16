---
title: Sponsored TV ads
description: Sponsored TV getting started with details on how to create an ad.
type: guide
interface: api
keywords:
    - Sponsored TV
    - Ads
---

# Sponsored TV ads


A Sponsored TV ad includes the details of the product from your brand. This product will be the landing page for your ad. In essence, this means that when a viewer interacts remotely with your ad and scans your ad QR code, they’ll link to the detail page of this product.


## Before you begin

* Have an [ad group ID](guides/sponsored-tv/ad-groups)


## Create an ad

The request to create an ad differs by whether you have a seller or vendor account. Seller accounts will set the entityType header to `SELLER_CENTRAL_ACCOUNT`, while vendors will set entityType to `VENDOR`. A successful response returns an `adID`.

>[NOTE] `landingPageType` for a seller is `SKU_DP`. For a vendor it is `ASIN_DP`. The `landingPageValue` parameter is the URL of the landing page. 

Endpoint: POST [/st/ads](sponsored-tv-open-beta/#tag/Ads/operation/CreateSponsoredTvAds)


### Request for seller account

```
curl --location --request POST 'https://advertising-api.amazon.com/st/ads' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'entityType: SELLER_CENTRAL_ENTITY' \
--data-raw '{
        "ads": [{
        "adGroupId": "123456789123456",
        "state": "ENABLED",
        "adName": "ad",
        "landingPageType": "SKU_DP",
        "landingPageValue": "elc_JzpnW"
    }]
}'
```

Response: 

```
{
    "ads": {
        "error": [],
        "success": [
            {
                "ad": {
                    "adGroupId": "123456789123456",
                    "adId": "123456789123456",
                    "campaignId": "123456789123456",
                    "landingPageType": "SKU_DP",
                    "landingPageValue": "elc_JzpnW"
                    "state": "ENABLED",
                    "adName": "ad"
                },
                "adId": "123456789123456",
                "index": 0
            }
        ]
    }
}
```

### Request for vendor account

```
curl --location --request POST 'https://advertising-api.amazon.com/st/ads' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'entityType: VENDOR' \
--data-raw '{
    "ads": [{
        "adGroupId": "123456789123456",
        "state": "ENABLED",
        "adName": "ad",
        "landingPageValue": "B0B42MH6YR",
        "landingPageType": "ASIN_DP"
    }]
}'
```

Response:

```
{
    "ads": {
        "error": [],
        "success": [
            {
                "ad": {
                    "adGroupId": "123456789123456",
                    "adId": "123456789123456",
                    "campaignId": "123456789123456",
                    "landingPageType": "ASIN_DP",
                    "landingPageValue": "B0B42MH6YR",
                    "state": "ENABLED",
                    "adName": "ad"
                },
                "adId": "123456789123456",
                "index": 0
            }
        ]
    }
}
```

