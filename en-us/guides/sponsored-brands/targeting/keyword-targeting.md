---
title: Creating keyword targeting
description: A tutorial for keyword targets for Sponsored Brands, with example requests and responses.
type: guide
interface: api
tags: 
    - Sponsored Brands
    - Campaign management
keywords:
    - keyword targeting
---

# Sponsored Brands keyword targeting

With keyword targeting, Amazon matches your keywords to a customer's search terms and displays the branded headline and advertised products in your campaign. Keyword targeting in Sponsored Brands has semantic matching. Instead of having a word-to-word matching only, it uses a meaning-based approach. For instance, if a customer searches for "running shoes," campaigns that target semantically equivalent keywords such as "jogging shoes" and "shoes for running" are eligible to bid on that search.

In addition, for Sponsored Brands campaigns, you can choose words that must be present in the shopping query in order for your ad to run. Broad match modifiers can be added by adding the plus symbol "+" in front of the keyword. For example, if you use the keyword “+kids shoes” with a broad match modifier, then the ad will match only the searches that contain the word “kids.” The ad may match to “kids sneakers” or “running shoes for kids” but they won’t match any shopping query that doesn’t contain the word “kids,” such as “sneakers” or “running shoes.”


## Before you begin

You must have the following before creating a keyword:

* [A campaign Id](guides/sponsored-brands/campaigns/get-started-with-campaigns#Create-a-campaign)
* [An ad group Id](guides/sponsored-brands/campaigns/get-started-with-campaigns#Create-ad-group)


If you don't know what keywords to target, try using [keyword recommendations](sponsored-brands/3-0/openapi/prod#tag/Keyword-Recommendations/operation/getKeywordRecommendations) resource. If you need Amazon's suggested bids for keywords, use the [bid recommendations](sponsored-brands/3-0/openapi#tag/Bid-recommendations/operation/getBidsRecommendations) resource. 

>[NOTE] The bid is only mutable when the keyword's corresponding campaign does not have any enabled [optimization rule](sponsored-brands/3-0/openapi/prod#tag/Optimization-Rules-(beta)).

## Creating keywords

**Endpoint** 
[POST sb/keywords](sponsored-brands/3-0/openapi#tag/Keywords/operation/createKeywords)

**Parameters**


|Name	|Optional	|Type	|Description	|
|---	|---	|---	|---	|
|adGroupId	|No	|string	|The ad group to associated the campaign with	|
|campaignId	|No	|string	|The campaign to associated the target	|
|keywordText	|No	|string	|The keyword text	|
|`nativeLanguageKeyword`	|Yes	|string	|The unlocalized keyword text in the preferred locale of the advertiser.	|
|nativeLanguageLocale	|Yes	|string	|The locale preference of the advertiser. For example, if the advertiser’s preferred language is Simplified Chinese, set the locale to `zh_CN`. Supported locales include: Simplified Chinese (locale: zh*CN) for US, UK and CA. English (locale: en*GB) for DE, FR, IT and ES.	|
|matchType	|No	|string	|The type of match. For more information, see match types in the [advertising console support center](https://advertising.amazon.com/help?#GHTRFDZRJPW6764R).	|
|bid	|No	|number	|Bid associated with this keyword. 	|

>[NOTE] In Sponsored Brands and Sponsored Display campaigns, if we find an opportunity where your ad is more likely to lead to your desired goal, we may increase your bid for that auction up to 300%. Similarly, if we find an opportunity that looks less likely to lead to your campaign goal, we might lower your bid for that auction. For example, Amazon can adjust a bid of $1 up to $4 when there are opportunities for higher expected result. For Sponsored Brands, these bid adjustments are available only on campaigns where you select "Drive page visits" as your campaign goal. For more information related to bidding, please see our updated [help page](https://advertising.amazon.com/help/GPDRGYSKVB7H4BUR). 

>[TIP] To make sure your keywords are valid and in the correct format, see [keyword character constraints.](reference/concepts/limits#keyword-character-constraints)

The max number of keywords that can be created per request is one hundred. For most advertisers, the following request body structure is recommended:


```
curl --location 'https://advertising-api.amazon.com/sb/keywords' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.`xxxxxxxx`' \
--header 'Authorization: Bearer `xxxxxxxx`' \
--header 'Amazon-Advertising-API-Scope: `xxxxxxxx`' \
--header 'Content-Type: application/json' \
  {
    "adGroupId": {{adGroupId}},
    "campaignId": {{campaignId}},
    "keywordText": "shoes",
    "matchType": "exact",
    "bid": 10
  }
]'
```

**Using nativeLanguageLocale**
Advertisers who are advertising in other regions can use the `nativeLanguageLocale` and `nativeLanguageKeyword` parameters to specify the equivalent `keywordText` in their preferred language for reporting purposes.


```
curl --location 'https://advertising-api.amazon.com/sb/keywords' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.`xxxxxxxx`' \
--header 'Authorization: Bearer `xxxxxxxx`' \
--header 'Amazon-Advertising-API-Scope: `xxxxxxxx`' \
--header 'Content-Type: application/json' \
--data '[
  {
    "adGroupId": {{adGroupId}},
    "campaignId": {{campaignId}},
    "keywordText": "soap bar",
    "matchType": "exact",
    "nativeLanguageKeyword": "肥皂",
    "nativeLanguageLocale" : "zh_CN",
    "bid": 10
  }
]'
```

## **Get keyword recommendations** 

The [POST /sb/recommendations/keyword](sponsored-brands/3-0/openapi/prod#tag/Keyword-Recommendations/operation/getKeywordRecommendations) resource retrieves recommended keyword given either a list of ASINS that will be on the landing page for the ad or a specific Stores page URL. Vendors can also provide the URL of a custom landing page. If inputting a URL, the ASINs featured on that landing page are used to generate keyword recommendations. 

>[TIP] While only `asins` or `url` are required in the request, we also recommend including both the `creativeType` and `creativeAsins` parameters. Since landing pages can have up to 100 ASINs, including these parameters helps focus your keyword recommendations on the products you display as part of your ad.



### Getting keyword recommendations based on creative asins and types.


If a list of ASINs is provided in the `creativeAsins` parameter, Amazon will rank the keywords recommendations higher for that list of ASINs than the ASINs provided either in the `asins` object or the landing page. If a `creativeType` is provided, Amazon will rank keyword recommendations based on either a static image or a video. Table below shows where recommendations are generated from depending on the identifier chosen.  Please note that both these parameters are optional and not required to obtain a keyword recommendation. 


|Identifier	|Description	|Recommendations generated from	|
|---	|---	|---	|
|`PRODUCT_COLLECTION`	|`Showcase multiple products within a branded shopping experience`	|Static image	|
|`AUTHOR_COLLECTION`	|`Showcase books under your name that direct to your Book Brand landing page`	|Static image	|
|`STORE_SPOTLIGHT`	|`Showcase your brand logo, headline, and up to 3 product categories or sub-pages`	|Static image	|
|`VIDEO`   |`Display a video ad promoting a product that redirects to its landing page`	|Video	|
|`BRAND_VIDEO`	| `Display a branded video ad that redirects shoppers to your Brand Store landing page`	|Video	|


#### List of ASIN

**Endpoint**
[POST sb/recommendations/keyword](sponsored-brands/3-0/openapi/prod#/Keyword%20Recommendations/getKeywordRecommendations)

**Scenario 1**: three ASINs are passed in the request body with the creative parameters.


```
curl --location 'https://advertising-api.amazon.com/sb/recommendations/keyword' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.`xxxxxxxx`' \
--header 'Authorization: Bearer `xxxxxxxx`' \
--header 'Amazon-Advertising-API-Scope: `xxxxxxxx`' \
--header 'Content-Type: application/json' \
--data '{
{
   "asins":[
      "B00J000000",
      "B0787FTYUO",
      "B004000000"
   ],
   "maxNumSuggestions":"3",
   "creativeType":"PRODUCT_COLLECTION",
   "creativeAsins":[
      "B00JVTNWB0",
      "B0787FVB5F",
      "B004QF0TFQ"
   ],
   "locale":"zh_CN"
}
}'
```

**Response**

In this response, the field `translation` returned `null` because a `locale` was not specified in the request body. The recommendationId field is just a unique ID for each recommended keyword. Match type `PHRASE` is the only option for keyword suggestions. 

The `searchTermImpressionShare` indicates the percentage share of all ad-attributed impressions you received on that keyword in the last 7 days. This metric helps advertisers identify potential opportunities based on their share on relevant keywords. The `searchTermImpressionRank` indicates your ranking among all advertisers for the keyword by ad impressions in a marketplace. It tells an advertiser how many advertisers had higher share of ad impressions. Search term information is only available for keywords the advertiser targeted with ad impressions. If either field is null, it means there is not enough data for show a ranking.The `addKeyword`  value for the `type` parameter means that Amazon recommends adding the returned keyword recommendation to your ad group. 

```
[
    {
        "recommendationId": "bff847042aec2aeab666bef537733f49",
        "type": "addKeyword",
        "value": "socks for men",
        "translation": null,
        "matchType": "PHRASE",
        "searchTermImpressionShare": null,
        "searchTermImpressionRank": null
    },
    {
        "recommendationId": "522f7a270e925a43343e348558e80a8a",
        "type": "addKeyword",
        "value": "socks for women",
        "translation": null,
        "matchType": "PHRASE",
        "searchTermImpressionShare": null,
        "searchTermImpressionRank": null
    },
    {
        "recommendationId": "1c8044ca430081b78f10db3346f41729",
        "type": "addKeyword",
        "value": "womens socks",
        "translation": null,
        "matchType": "PHRASE",
        "searchTermImpressionShare": null,
        "searchTermImpressionRank": null
    }
]
```

**Scenario 2**: three ASINs are passed in the request body with `locale` provided.


```
curl --location 'https://advertising-api.amazon.com/sb/recommendations/keyword' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Authorization: Bearer xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Content-Type: application/json' \
--data '{
   "asins":[
      "B00JVTNWB0",
      "B0787FVB5F",
      "B004QF0TFQ"
   ],
   "maxNumSuggestions":"3",
   "locale":"zh_CN"
}'
```

**Response**

In this response, you can see the suggested keyword translation to Simplified Chinese under the `translation` field.

```
[
    {
        "recommendationId": "d5e9f874357f0ebc691d6bb4538ba999",
        "type": "addKeyword",
        "value": "socks for men",
        "translation": "男士袜子",
        "matchType": "PHRASE",
        "searchTermImpressionShare": null,
        "searchTermImpressionRank": null
    },
    {
        "recommendationId": "64f62372dec36c981c45c8b7570309d9",
        "type": "addKeyword",
        "value": "socks for women",
        "translation": "女式袜子",
        "matchType": "PHRASE",
        "searchTermImpressionShare": null,
        "searchTermImpressionRank": null
    },
    {
        "recommendationId": "80e9accc0a3a5c68aa19831abd02c320",
        "type": "addKeyword",
        "value": "womens socks",
        "translation": "女士袜子",
        "matchType": "PHRASE",
        "searchTermImpressionShare": null,
        "searchTermImpressionRank": null
    }
]
```

### URL

**Endpoint**
[POST sb/recommendations/keyword](sponsored-brands/3-0/openapi/prod#tag/Keyword-Recommendations/operation/getKeywordRecommendations)

**Scenario 1:** URL passed in the request body with creative parameters


```
curl --location 'https://advertising-api.amazon.com/sb/recommendations/keyword' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Authorization: Bearer xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Content-Type: application/json' \
--data '{
   "url":"https://www.amazon.com/stores/page/xxxxxxx",
   "maxNumSuggestions":"3",
   "creativeType":"PRODUCT_COLLECTION",
   "creativeAsins":[
      "B00JVTNWB0",
      "B0787FVB5F",
      "B004QF0TFQ"
   ],
   "locale":"zh_CN"
}'
```

**Response**

In this response, you can see the keyword recommendations returned are based on the creative parameters passed in the request. If the creative parameters were not included in the request, the keyword recommendations would be based off the ASINs located within the provided `url` , which can be a store URL or a custom URL.


```
[
    {
        "recommendationId": "615d92d1bcc8a91eac8af72d3809c18b",
        "type": "addKeyword",
        "value": "socks for men",
        "translation": null,
        "matchType": "PHRASE",
        "searchTermImpressionShare": null,
        "searchTermImpressionRank": null
    },
    {
        "recommendationId": "bd7eb870bc57e1f8f5312a7404d870ec",
        "type": "addKeyword",
        "value": "socks for women",
        "translation": null,
        "matchType": "PHRASE",
        "searchTermImpressionShare": null,
        "searchTermImpressionRank": null
    },
    {
        "recommendationId": "37d87e3a0e27ffee77d1ab0aa651d925",
        "type": "addKeyword",
        "value": "womens socks",
        "translation": null,
        "matchType": "PHRASE",
        "searchTermImpressionShare": null,
        "searchTermImpressionRank": null
    }
]
```



## Get bid recommendations for keywords

The `/sb/recommendations/bids` resource provides bid recommendations for up to one hundred keywords per request.

>[TIP] While only `campaignId`, `matchType` and `keywordText` are required in the request, we also recommend including the `adFormat` parameter. This parameter helps focus your bid recommendations based on what ad format you are using. If you are using a product collection or store spotlight ad format, use the `productCollection `value. If you are using video or brand video, use the `video` value. 

**Endpoint**
[POST /sb/recommendations/bids](sponsored-brands/3-0/openapi#tag/Bid-recommendations/operation/getBidsRecommendations)

**Request**


```
curl --location 'https://advertising-api.amazon.com/sb/recommendations/bids' \
--header 'Authorization: Bearer xxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Content-Type: application/json' \
--data '{
  "campaignId": {{campaignId}},
  "keywords": [
    {
      "matchType": "broad",
      "keywordText": "socks for men"
    }
  ],
  "adFormat": "productCollection"
}'
```


**Response**

In the response, you can see the `recommendedBid` object having a recommended bid range as well as the recommended bid from that range. 

```
{
    "keywordsBidsRecommendationSuccessResults": [
        {
            "keyword": {
                "keywordText": "socks for men",
                "matchType": "broad"
            },
            "keywordIndex": 0,
            "recommendationId": "2f46761fa2f602ff152fa56d2247494d",
            "recommendedBid": {
                "rangeStart": 1.67,
                "rangeEnd": 5.78,
                "recommended": 2.73
            }
        }
    ],
    "keywordsBidsRecommendationErrorResults": [],
    "targetsBidsRecommendationSuccessResults": [],
    "targetsBidsRecommendationErrorResults": [],
    "themesRecommendationSuccessResults": [],
    "themesRecommendationErrorResults": []
}
```

