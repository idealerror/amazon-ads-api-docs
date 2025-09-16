---
title: Sponsored Product keyword groups targeting
description: Overview of keyword group targeting
type: guide
interface: api 
tags:
    - Sponsored Products
keywords:
    - Keyword groups
    - groups targeting
---

# Keyword Groups overview

Keyword Groups targeting automatically identifies and targets highly relevant keyword targets for ads. Keyword Groups targeting is based on specified product ASINs and continuously refreshes them over time to maximize ad performance.

>[NOTE] Keyword Groups is currently in beta and only available for US marketplace.


## Retrieve list of recommended keyword groups

The Keyword Groups recommendations API uses the product ASINs as input and returns suggested Keyword Groups to target.

>[NOTE] You can adopt both keyword and Keyword Groups targets for a given ad group. However, you cannot mix keyword group targets with product targets.

Endpoint: [/sp/targeting/recommendations/keywordGroups](sponsored-products/3-0/openapi/prod#tag/Keyword-Group-Targeting-Recommendations)


### Parameters

|Name	|Optional	|Type	|Location	|Description	|
|---	|---	|---	|---	|---	|
|asins	|No	|List	|Body	|A list of Strings representing the ASINs for which you desire to get Keyword Groups recommendations.	|
|locale	|Yes	|String	|Header	|User specified locale. If nothing is passed the default for the marketplace will be used. The value should confirm to the IETF BCP 47 standard, using language tags composed of language- and optionally region specific sub-tags (e.g., 'en-us' for American English and 'fr-CA' for Canadian French).	|



### Sample request

```
curl --location --request POST 'https://advertising-api.amazon.com/sp/targeting/recommendations/keywordGroups' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{
    "asins": [
        "PBTKGAT1",
        "B07QBVG5V9",
        "B0CJCKQT3B"
    ]
}'
```

### Response

```
{
    "keywordGroups": [{
        "id": "branded",
        "sampleKeywords": [
            "arteza artists-markers",
            "arteza twinmarkers",
            "arteza+everblend+markers+12",
            "arteza marker sets",
            "arteza market set"
        ],
        "text": "Keywords related to your brand"
    }]
}
```

