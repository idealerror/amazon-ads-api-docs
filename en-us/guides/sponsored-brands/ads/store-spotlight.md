---
title: Store spotlight ad
description: Overview for a store spotlight ad type
type: guide
interface: api
tags: 
    - Sponsored Brands
keywords:
    - Store spotlight
---


# Store spotlight

Store spotlight can help drive traffic to your Store. This ad format features your brand logo, a custom headline, and your Store on Amazon with at least three subpages. When Amazon shoppers click the ad tile, it will direct them to a specified subpage within the Store. Clicking the brand logo or the headline will direct shoppers to your main Store page. Learn more about [Stores](https://advertising.amazon.com/help?ref_=AAC_unav_support_center#GEQXGAH9CZWU2K76).

See the [API reference](sponsored-brands/3-0/openapi/prod#tag/Ads/operation/CreateSponsoredBrandStoreSpotlightAds) for more details.


## Before you begin

You must have the following before creating a store spotlight ad:

* [A campaign ID](guides/sponsored-brands/campaigns/get-started-with-campaigns#step-1-create-a-campaign)
* [An ad group ID](guides/sponsored-brands/campaigns/get-started-with-campaigns#step-2-create-ad-group)
* An existing Store homepage URL
* Three subpages with the following information for each subpage:
    * Page title
    * ASIN present on the subpage
    * URL
* An `assetId` for the brand image using the [asset library API](guides/creative-asset/asset-library-overview)

## Examples

### Create a Store spotlight ad

For the `landingPage.url`, make sure to include the Store home page URL. Ensure that for each subpage, the ASIN specified in `asin` key is present in the subpage specified in `url` key .

```json
curl --location 'https://advertising-api.amazon.com/sb/v4/ads/storeSpotlight' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Accept: application/vnd.sbadresource.v4+json' \
--header 'Content-Type: application/json' \
--data '{
"ads": [{
    "landingPage": {
        "url": "https://www.amazon.com/stores/page/xxxxxx"
    },
    "name": "Name of store spotlight ad",
    "state": "ENABLED",
    "adGroupId": "{{adGroupId}}",
    "creative": {
        "brandName": "Name of the brand",
        "subpages": [{
                "pageTitle": "Savannah",
                "asin": "B020000000",
                "url": "https://www.amazon.com/stores/page/xxxxxx"
            },
            {
                "pageTitle": "Miami",
                "asin": "B001000000",
                "url": "https://www.amazon.com/stores/page/xxxxxxx"
            },
            {
                "pageTitle": "San Fran",
                "asin": "B000000000",
                "url": "https://www.amazon.com/stores/page/xxxxxx"
            }
        ],
        "brandLogoAssetID": "amzn1.assetlibrary.asset1.xxxxxx",
        "headline": "The headline text of products in ad"
    }
}]
}
'
```

### Change to a new Store Spotlight landing page from existing Store Spotlight landing page

The `POST sb/ads/creatives/storeSpotlight` endpoint changes an existing Store Spotlight ad landing page to a new Store Spotlight landing page. The `adId` parameter represents the id of the existing Store Spotlight landing page. The creative object contains the details of the updated creative for the new landing page.

Endpoint : [/sb/ads/creatives/storeSpotlight](sponsored-brands/3-0/openapi/prod#tag/Ad-creatives/operation/CreateStoreSpotlightCreative)

```json
curl --location 'https://advertising-api.amazon.com/sb/ads/creatives/storeSpotlight' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Accept: application/vnd.sbadresource.v4+json' \
--header 'Content-Type: application/json' \
--data '{
    "adId": "{{adId}}",
    "creative": {
        "brandName": "name of brand",
        "headline": "Headline text",
        "subpages": [{
                "pageTitle": "LEGO 90th Anniversary",
                "asin": "B09JHT6XWS",
                "url": "https://www.amazon.com/stores/page/6C008738-C2FF-4BEA-94CA-98B9C7387CDA"
            },
            {
                "pageTitle": "New Releases",
                "asin": "B0BSRF4FSK",
                "url": "https://www.amazon.com/stores/page/18B5FDA3-412C-494E-B791-9CBBC62AADDB"
            },
            {
                "pageTitle": "Bestsellers",
                "asin": "B0BBY56VZ7",
                "url": "https://www.amazon.com/stores/page/9608963C-8BD1-4CD8-B9ED-06926DAA233E"
            }
        ],
        "brandLogoAssetId": "amzn1.assetlibrary.asset1.xxxxxx:version_v1",
        "brandLogoCrop": {
            "top": 0,
            "left": 0,
            "width": 500,
            "height": 500
        },
        "landingPage": {
            "type": "STORE",
            "url": "https://www.amazon.com/stores/page/017EF856-965D-4B56-A171-EA61CAFF45DD"
        },
        "consentToTranslate": null
    }
}
```

