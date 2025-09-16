---
title: Creating keywords
description: Tutorial for creating Sponsored Products keywords using the Amazon Ads API
type: guide
interface: api 
tags:
    - Sponsored Products
    - Campaign management
keywords:
    - auto campaigns
    - create keyword
---

# Keyword targeting overview


[Keyword targeting](https://advertising.amazon.com/help?&ref_=AAC_unav_support_center#GCLXZBQSTR4PCB66) lets you have more control over your targeting and spend, identifying keywords for which you want your ads to appear and optimizing bids for each. Targeting is assigned at the ad group level and all products in the ad group share the keyword targeting created.

Learn more:

* [Getting started with keyword recommendations](guides/sponsored-products/keywords/suggestions)
* [Get started with keyword bid recommendations](guides/sponsored-products/bid-suggestions/theme-based-bid-suggestions-quickstart-guide)

## Create a keyword

Once you have the recommended keywords, you can create keywords to associate to a campaign and ad group.

>[TIP] To make sure your keywords are valid and in the correct format, see [keyword character constraints.](reference/concepts/limits#keyword-character-constraints)

### Endpoint 

[POST /sp/keywords](sponsored-products/2-0/openapi#/prod#tag/Keywords/operation/CreateSponsoredProductsKeywords)

### Parameters

|Name	|Optional	|Type	|Description	|
|---	|---	|---	|---	|
|campaignId	|No	|Number	|The campaign to associate the keyword with.	|
|adGroupId	|No	|String	|The ad group to associate the keyword with.	|
|state	|No	|Enum	|The current resource state.	|
|keywordText	|No	|String	|The keyword text.	|
|nativeLanguageKeyword	|Yes	|String	|The unlocalized keyword text in the preferred locale of the advertiser.	|
|nativeLanguageLocale	|Yes	|String	|The locale preference of the advertiser. For example, if the advertiser’s preferred language is Simplified Chinese, set the locale to `zh_CN`. Supported locales include: Simplified Chinese (locale: zh_CN) for US, UK and CA. English (locale: en_GB) for DE, FR, IT and ES.	|
|matchType	|No	|String	|The type of match. For more information, see match types in the [advertising console support center](https://advertising.amazon.com/help?#GHTRFDZRJPW6764R).	|
|bid	|No	|Number	|Bid associated with this keyword. Applicable to biddable [match types](https://advertising.amazon.com/help?entityId=ENTITY20JRZCUO3OE1U#GHTRFDZRJPW6764R) only. You can also get [bid recommendations for keywords](guides/sponsored-products/bid-suggestions/theme-based-bid-suggestions-quickstart-guide)	|

### Examples

**Standard** 

Most advertisers should not use `nativeLanguageLocale` and should use the following request body structure:


```
curl --location --request POST 'https://advertising-api.amazon.com/sp/keywords' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Authorization: Bearer xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Prefer: return=representation' \
--header 'Accept: application/vnd.spKeyword.v3+json' \
--header 'Content-Type: application/vnd.spKeyword.v3+json' \
--data-raw '{
    "keywords": [
        {
            "campaignId": "153839444046652",
            "matchType": "EXACT",
            "state": "ENABLED",
            "bid": 0.02,
            "adGroupId": "208676741301789",
            "keywordText": "soap bar"
        }
    ]
}'
```

```
{
    "keywords": {
        "error": [],
        "success": [
            {
                "index": 0,
                "keyword": {
                    "adGroupId": "80908659241419",
                    "bid": 0.02,
                    "campaignId": "152865535357884",
                    "keywordId": "104465559608285",
                    "keywordText": "soap bar",
                    "matchType": "EXACT",
                    "state": "ENABLED"
                },
                "keywordId": "104465559608285"
            }
        ]
    }
}
```



**Using nativeLanguageLocale** 


Advertisers who are advertising in other regions can use the `nativeLanguageLocale` and `nativeLanguageKeyword` parameters to specify the equivalent `keywordText` in their preferred language for reporting purposes.


```
curl --location --request POST 'https://advertising-api.amazon.com/sp/keywords' \
--header 'Authorization: Bearer xxxxxxxx ' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: xxxxxxxx' \
--header 'Accept: application/vnd.spKeyword.v3+json' \
--header 'Prefer: return=representation' \
--header 'Content-Type: application/vnd.spKeyword.v3+json' \
--data-raw '{
    "keywords": [
        {
            "campaignId": "153839444046652",
            "matchType": "EXACT",
            "state": "ENABLED",
            "bid": 0.02,
            "adGroupId": "208676741301789",
            "keywordText": "soap bar",
            "nativeLanguageLocale" : "zh_CN",
            "nativeLanguageKeyword" : "肥皂"
        }
    ]
}'

```

```
{
    "keywords": {
        "error": [],
        "success": [
            {
                "index": 0,
                "keyword": {
                    "adGroupId": "80908659241419",
                    "bid": 0.02,
                    "campaignId": "153839444046652",
                    "keywordId": "104465559608285",
                    "keywordText": "soap bar",
                    "matchType": "EXACT",
                    "nativeLanguageKeyword": "肥皂",
                    "nativeLanguageLocale": "zh_CN",
                    "state": "ENABLED"
                },
                "keywordId": "104465559608285"
            }
        ]
    }
}
```


