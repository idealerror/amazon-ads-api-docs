---
title: Sponsored TV creative preview
description: Sponsored TV getting started with how to create a creative preview.
type: guide
interface: api
keywords:
    - Sponsored TV
    - Creative preview
---

# Sponsored TV creative preview

The [creative preview](sponsored-tv-open-beta#tag/CreativesPreview/operation/PreviewSponsoredTvCreative) endpoint provides a preview as to how the video will look in the ad itself. In the request body, you can either get the preview by using the `assetProperties` object or the `creativeId`. Once you specify if you want to use creativeId or assetProperties, you must also specify the `previewConfiguration`. 

>[NOTE] `assetProperties` cannot be used in conjunction with `creativeId`.


## Before you begin

* `creativeId` - if using the creativeId parameter.  See [creative overview](guides/sponsored-tv/creatives/overview#example) for more details.
* `assetId` and `assetVersion` - if using the assetProperties object. See [creating assets](guides/creative-asset/creating-assets) to obtain these values.



## Preview configuration

The `previewConfiguration` object contains the configuration settings for the preview. 

>[NOTE] You must use either the `asin` or `sku` parameter. Both parameters cannot be used together. 


### Parameters

|Parameter	|Type	|Description	|Optional   |
|---	|---	|---	|
|safeZoneEnabled	|boolean	|If set to true, any interactive elements added by Sponsored TV to your ad creative are in the left-hand side of your ad. For more information, see [Sponsored TV interactive experiences section](https://advertising.amazon.com/resources/ad-specs/sponsoredtv).	|Yes |
|callToActionPosition	|string	|The location of the CallToAction prompt. Can choose either `LEFT` or `RIGHT.`	| Yes |
|asin	|string	|Amazon Standard Identification Number: The code that identifies the product being advertised. Vendors should use the `asin` parameter. |No |
|sku	|string	|Stock Keeping Unit: The unique identifier for the product being advertised. Sellers should use the `sku` parameter.	| No | 
|callToAction	|string	|Prompt to encourage the customer to take a desired action. When `callToAction` is not provided, we will provide preview for all supported `callToAction`. We currently only support `SHOP_NOW` `callToAction` and base video preview. The `SHOP_NOW` prompt takes the customer to the product's landing page. 	|Yes| 

### Vendor request

Endpoint: POST [/st/creatives/preview](sponsored-tv-open-beta#tag/CreativesPreview/operation/PreviewSponsoredTvCreative)

```
curl --location 'https://advertising-api.amazon.com/st/creatives/preview' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data '{
    "assetProperties": {
        "assetId": "amzn1.assetlibrary.asset1.xxxxxx",
        "assetVersion": "version_v1"
    },
    "previewConfiguration": {
        "callToAction": "SHOP_NOW",
        "asin": "B001PYUTII",
        "callToActionPosition": "RIGHT"
    }
}'
```

### Seller request

Endpoint: POST [/st/creatives/preview](sponsored-tv-open-beta#tag/CreativesPreview/operation/PreviewSponsoredTvCreative)

```
curl --location 'https://advertising-api.amazon.com/st/creatives/preview' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data '{
    "assetProperties": {
        "assetId": "amzn1.assetlibrary.asset1.xxxxxx",
        "assetVersion": "version_v1"
    },
    "previewConfiguration": {
        "callToAction": "SHOP_NOW",
        "sku": "elc_EJMwj",
        "callToActionPosition": "LEFT"
    }
}'
```

## Getting preview using creative ID


Endpoint: POST [/st/creatives/preview](sponsored-tv-open-beta#tag/CreativesPreview/operation/PreviewSponsoredTvCreative)


```
curl --location 'https://advertising-api.amazon.com/st/creatives/preview' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data '{
    "creativeId": "869860752003455077",
    "previewConfiguration": {
        "callToAction": "SHOP_NOW",
        "asin": "B001PYUTII"
    }
}'
```



## Getting preview using the asset properties

To use the `assetProperties` object, you must have the `assetId` and `assetVersion`. 

Endpoint: POST [/st/creatives/preview](sponsored-tv-open-beta#tag/CreativesPreview/operation/PreviewSponsoredTvCreative)


```
curl --location 'https://advertising-api.amazon.com/st/creatives/preview' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data '{
    "assetProperties": {
        "assetId": "amzn1.assetlibrary.asset1.xxxxxx",
        "assetVersion": "version_v1"
    },
    "previewConfiguration": {
        "callToAction": "SHOP_NOW",
        "asin": "B001PYUTII"
    }
}'
```

## Response preview

A successful response will include an escaped HTML. You need to unescape the HTML and save it as a separate HTML file.

```
{
    "previewHtmls": [
        {
            "experience": {
                "callToAction": "SHOP_NOW",
                "interactivity": "BASE"
            },
            "html": "<!doctype html><html><video controls><source src=https://m.media-amazon.com/images/S/al-na-184b9306-7f8a/e9079512-4c5d-4fcf-bf35-758ae7ef2e90.mp4 type=\"video/mp4\"/> </video> </html>"
        }
    ]
}
```

The iframe has the same size as the default video size of 1920x1080. You can then open the iframe HTML in your browser to preview the video. 

Below is a sample iframe file code with a size of 480x 270.

```
<html>
<body>
<iframe sandbox="allow-scripts allow-same-origin" src="./previewCreative.html" style="border:none; position:absolute;"  width="480px" height="270px" />
</body>
</html>
```
