---
title: Amazon Ads API targets model overview
description: Describes the target model and associated fields for the Amazon Ads API.
type: guide
interface: api
tags:
    - Operations
---

# Target

**Targets** allow advertisers to influence when ads will or will not appear, as well as specify how much to bid for an ad when these conditions are met. Targeting conditions can can be “inclusions” (`negative` = false) or “exclusions” (`negative` = true, also known as negative targets). 

## Targeting types

Amazon Ads supports a variety of targeting types depending on the ad product. Some targeting types also have the option to support negative targeting (exclusions). Targets are typically applied at the ad group level, however some types of negative targets are also available at the campaign level. 

| Targeting type | Supports negative | Supports campaign-level | Description | Sponsored Products | Sponsored Brands | Sponsored Display |
|------|-----|------|------|--|----|----|
| AUTO | No | No | Targets chosen by Amazon based on a high-level match type or criteria. | x | x | x |
| AUDIENCE | No |  No |Targets based on a selected chosen Amazon audience. |  | | x |
| KEYWORD | Yes | Yes; negative Sponsored Products targets only | Targets based on customer search terms. | x | x | |
| PRODUCT\_CATEGORY | Yes | Yes; negative Sponsored Products targets only | Targets based on a specified Amazon product category with optional refinements like brand, price, and rating. | x | x | x |
| PRODUCT | Yes | Yes; negative Sponsored Products targets only | Targets based on a specified Amazon product ASIN. | x | x | x |
| PRODUCT\_CATEGORY<br />\_AUDIENCE | No | No | Targets based on customer views or purchases on an Amazon product category (with optional refinements), within a specified lookback window. | | | x |
| PRODUCT\_AUDIENCE | No | No | Targets based on customer views or purchases on an Amazon product ASIN, within a specified lookback window. | | | x |
| THEME | No | No | Targets keywords related to a theme. | x | | |

## Schema

Targets contains the following fields. **Read-only** indicates that the field is part of the model, but cannot be modified by advertisers. **Required** indicates that a field will always appear in the model. 

>[NOTE] Some fields are only available for certain ad products. For details, see the [Ad product mapping table](#ad-product-mapping).

|Common field	|Type	|Required	|Read only	|Description	|
|-----|------|-----|-----|-----|
|targetId	|string	|Required	|Read Only	|The unique identifier of the target.	|
|adGroupId	|string	|	|	|The unique identifier of the ad group that the target belongs to.	|
|campaignId | string | | | The campaignId associated to the target (for campaign-level targets only).|
|adProduct	|[Enum](reference/common-models/enums#adproduct)	|Required	|	|The ad product that the target belongs to.	|
|state	|[Enum](reference/common-models/enums#state)	|Required	|	|The user set state of the target.	|
|negative	|boolean	|Required	|	|Whether to target (false) or exclude (true) the given target.	|
|deliveryStatus	|[Enum](reference/common-models/enums#deliverystatus)	|Required	|Read Only	|This is an overall status if the target is delivering or not.	|
|deliveryReasons	|[Enum](reference/common-models/enums#deliveryreasons)|	|Read Only	|This is a list of reasons why the target is not delivering and the reasons behind the delivery status.	|
|creationDateTime	|datetime	|Required	|Read Only	|The date time that the target was created.	|
|lastUpdatedDateTime	|datetime	|Required	|Read Only	|The date time that the target was last updated.	|
|bid.bid	|double	|	|	|The bid applied to the target.	|
|bid.<br />currencyCode	|[Enum](reference/common-models/enums#currencyCode)	|	|	|The currency code of the bid applied to the target.	|
|targetType	|[Enum](reference/common-models/enums#targetType)	|Required	|	|The type of targe.	|
|targetDetails.<br />matchType	|[Enum](reference/common-models/enums#matchType)	|	|	|The match type associated with the target. Differs based on the targetType. 	|
|targetDetails.<br />keyword	|string	|	|	|The keyword text to target.	|
|targetDetails.<br />nativeLanguageKeyword	|string	|	|	|The unlocalized keyword text in the preferred locale of the advertiser.	|
|targetDetails.<br />nativeLanguageLocale	|string	|	|	|The locale preference of the advertiser. For example, if the advertiser’s preferred language is Simplified Chinese, set the locale to zh_CN. Supported locales include: Simplified Chinese (locale: zh_CN) for US, UK and CA. English (locale: en\_GB) for DE, FR, IT and ES.	|
|targetDetails.<br />productCategoryId	|string	|	|	|The product category to target.	|
|targetDetails.<br />productCategoryResolved	| string	|	|Read Only	|The resolved human readable name of the category.	|
|targetDetails.<br />productBrand	|string	|	|	|Refinement to target a specific brand within the product category.	|
|targetDetails.<br />productBrandResolved	| string	|	|Read Only	|The resolved human readable name of the brand.	|
|targetDetails.<br />productGenre	|string	|	|	|Refinement to target a specific product genre within the product category.	|
|targetDetails.<br />productPriceLessThan	|string	|	|	|Refinement to target products with a price less than the value within the product category.	|
|targetDetails.<br />productPriceGreaterThan	|string	|	|	|Refinement to target products with a price greater than the value within the product category.	|
|targetDetails.<br />productRatingLessThan	|string	|	|	|Refinement to target products with a rating less than the value within the product category.	|
|targetDetails.<br />productRatingGreaterThan	|string	|	|	|Refinement to target products with a rating greater than the value within the product category.	|
|targetDetails.<br />productAgeRange	|string	|	|	|Refinement to target products for a specific age range (given as an ID) within the product category.	|
|targetDetails.<br />productAgeRangeResolved	|string	|	|	| The resolved product age range to target.	|
|targetDetails.<br />productPrimeShippingEligible	|boolean	|	|	|Refinement to target products that are prime shipping eligible within the product category.	|
|targetDetails.<br />asin	|string	|	|	|The product asin to target.	|
|targetDetails.<br />event	|[Enum](reference/common-models/enums#event)	|	|	|The product based event to target the audience.	|
|targetDetails.<br />lookback	|integer	|	|	|The lookback period in days to target the audience for the specified product event.	|
|targetDetails.<br />audienceId	|string	|	|	| An audience identifier retrieved from the audiences/list resource.	|

## Ad product mapping

The mapping table shows how current versions of different ad products map to the common target model. Over time, we will move to standardize the fields in each individual API to the common target model.

| Ad product | Latest version |
|-----|-------|
| Sponsored Products | [Version 3](sponsored-products/3-0) |
| Sponsored Brands | [Version 3](sponsored-brands/3-0) |
| Sponsored Display | [Version 1](sponsored-display/3-0) |

For Amazon Marketing Stream, the table below references the [targets](guides/amazon-marketing-stream/datasets/sponsored-ads-campaign-management#tag/Reports/operation/downloadReport) dataset.

**Legend**

**x**: The ad product uses the common field name in its current version.

**N/A**:  The ad product contains the field in the common model schema, but not in its current version schema. 

**Empty cell**: The field is not represented for the ad product.

|Common field	|Sponsored Products	|Sponsored Brands	|Sponsored Display	|Amazon Marketing Stream	|
|----|------|------|------|-----|
|targetId	|keywordId, targetId, negativeKeywordId	|keywordId, targetId	|x	|x	|
|adGroupId	|x	|x	|x	|x	|
|campaignId	|x	|	|	|x	|
|adProduct	|N/A	|N/A	|N/A	|x	|
|state	|x	|x	|x	|x	|
|negative	|N/A	|N/A	|N/A	|x	|
|deliveryStatus	|N/A	|N/A	|N/A	|	|
|deliveryReasons	|extendedData.<br />servingStatus	|N/A	|N/A	|	|
|creationDateTime	|extendedData.<br />creationDateTime	|N/A	|creationDate	|audit.<br />creationDateTime	|
|lastUpdatedDateTime	|extendedData.<br />lastUpdateDateTime	|N/A	|lastUpdateDate	|audit.<br />lastUpdatedDateTime	|
|bid.<br />bid	|bid	|bid	|bid	|bid	|
|bid.<br />currencyCode	|N/A	|N/A	|N/A	|currencyCode	|
|targetType	|N/A	|N/A	|N/A	|x	|
|targetDetails.<br />matchType	|expression.type (target), matchType (keyword)	|expression.type (target), matchType (keyword), themes.<br />themeType (theme)	|expression.type	|{targetType}.<br />matchType	|
|targetDetails.<br />keyword	|keywordText	|keywordText	|	|keywordTarget.<br />keyword	|
|targetDetails.<br />nativeLanguageKeyword	|nativeLanguageKeyword	|nativeLanguageKeyword	|	|keywordTarget.<br /> nativeLanguageKeyword	|
|targetDetails.<br />nativeLanguageLocale	|nativeLanguageLocale	|N/A	|	|keywordTarget.<br />nativeLanguageLocale	|
|targetDetails.<br />productCategoryId	|expression.value where "type": "ASIN\_CATEGORY\_SAME\_AS"	|expressions.<br />value where "type": "asinCategorySameAs"	|expression.value where "type": "asinCategorySameAs"	|{targetType}.<br />targetingClause	|
|targetDetails.<br />productCategoryResolved	|resolvedExpression.value where "type": "ASIN\_CATEGORY\_SAME\_AS"	|resolvedExpressions.<br />value where "type": "asinCategorySameAs"	|resolvedExpression.value where "type": "asinCategorySameAs"	|{targetType}.<br />targetingClause	|
|targetDetails.<br />productBrand	|expression.value where "type": "ASIN\_BRAND\_SAME\_AS"	|expressions.<br />value where "type":"asinBrandSameAs"	|expression.value where "type":"asinBrandSameAs"	|{targetType}.<br />targetingClause	|
|targetDetails.<br />productBrandResolved	|resolvedExpression.value where "type": "ASIN\_BRAND\_SAME\_AS"	|resolvedExpressions.<br /> value where "type":"asinBrandSameAs"	|resolvedExpression.value where "type":"asinBrandSameAs"	|{targetType}.<br />targetingClause	|
|targetDetails.<br />productGenre	|expression.value where "type": "ASIN\_GENRE\_SAME\_AS"	|	|expression.value where "type":"asinGenreSameAs"	|{targetType}.<br />targetingClause	|
|targetDetails.<br />productGenreResolved	|resolvedExpression.value where "type": "ASIN\_GENRE\_SAME\_AS"	|	|resolvedExpression.value where "type":"asinGenreSameAs"	|{targetType}.<br />targetingClause	|
|targetDetails.<br />productPriceLessThan	|expression.value where "type": "ASIN\_PRICE\_LESS\_THAN" or "type": "ASIN\_PRICE\_BETWEEN" (bottom of range)	|expressions.<br />value where "type":"asinPriceLessThan" or "type": "asinPriceBetween" (bottom of range)	|expression.value where "type":"asinPriceLessThan" or "type": "asinPriceBetween" (bottom of range)	|{targetType}.<br />targetingClause	|
|targetDetails.<br />productPriceGreaterThan	|expression.value where "type": "ASIN\_PRICE\_GREATER\_THAN" or or "type": "ASIN\_PRICE\_BETWEEN" (top of range)	|expressions.<br />value where "type":"asinPriceGreaterThan" or "type": "asinPriceBetween" (top of range)	|expression.value where "type":"asinPriceGreaterThan" or "type": "asinPriceBetween" (top of range)	|{targetType}.<br />targetingClause	|
|targetDetails.<br />productRatingLessThan	|expression.value where "type": "ASIN\_REVIEW\_RATING\_LESS\_THAN" or "type": "ASIN\_REVIEW\_RATING\_BETWEEN" (bottom of range)	|expressions.<br />value where "type":"asinReviewRatingLessThan" or "type": "asinReviewRatingBetween" (bottom of range)	|expression.value where "type":"asinReviewRatingLessThan" or "type": "asinReviewRatingBetween" (bottom of range)	|{targetType}.<br />targetingClause	|
|targetDetails.<br />productRatingGreaterThan	|expression.value where "type": "ASIN\_REVIEW\_RATING\_GREATER\_THAN" or "type": "ASIN\_REVIEW\_RATING\_BETWEEN" (top of range)	|expressions.<br />value where "type":"asinReviewRatingGreaterThan" or "type": "asinReviewRatingBetween" (top of range)	|expression.value where "type":"asinReviewRatingGreaterThan" or "type": "asinReviewRatingBetween" (top of range)	|{targetType}.<br />targetingClause	|
|targetDetails.<br />productAgeRange	|expression.value where "type": "ASIN\_AGE\_RANGE\_SAME\_AS"	|	|expression.value where "type": "asinAgeRangeSameAs"	|{targetType}.<br />targetingClause	|
|targetDetails.<br />productAgeRangeResolved	|resolvedExpression.value where "type": "ASIN\_AGE\_RANGE\_SAME\_AS"	|	|resolvedExpression.value where "type": "asinAgeRangeSameAs"	|{targetType}.<br />targetingClause	|
|targetDetails.<br />productPrimeShippingEligible	|expression.value where "type": "ASIN\_IS\_PRIME\_SHIPPING\_ELIGIBLE"	|	|expression.value where "type": "asinIsPrimeShippingEligible"	|{targetType}.<br />targetingClause	|
|targetDetails.<br />asin	|expression.value where "type": "ASIN\_EXPANDED\_FROM" or "type:" "ASIN\_SAME\_AS"	|expression.value where "type:"asinSameAs"	|expression.value where "type:"asinSameAs"	|{targetType}.<br />targetingClause	|
|targetDetails.<br />event	|	|	|expression.type	|{targetType}.<br />targetingClause	|
|targetDetails.<br />lookback	|	|	|expression.value.<br />value where "expression.value.<br />type": "views" or "expression.value.<br />type": "purchases"	|{targetType}.<br />targetingClause	|
|targetDetails.<br />audienceId	|	|	|expression.<br />value.<br />type.<br />value where "expression.<br />value.<br />type": "audienceSameAs"	|{targetType}.targetingClause	|

## Representations

The following table shows the different areas where targets are surfaced in the Amazon Ads API.

| Feature | Operations | User guides |
|------|------|---------|
| [sp/targets](sponsored-products/3-0/openapi/prod#tag/TargetingClauses) | POST /sp/targets <br /> POST /sp/targets/list <br /> PUT /sp/targets <br /> POST /sp/targets/delete | [SP targeting overview](guides/sponsored-products/product-targeting/overview) |
| [sp/negativeTargets](sponsored-products/3-0/openapi/prod#tag/NegativeTargetingClauses) <br />[sp/campaignNegativeTargets](sponsored-products/3-0/openapi/prod#tag/campaignNegativeTargetingClauses) | POST /sp/negativeTargets <br /> POST /sp/negativeTargets/list <br /> PUT /sp/negativeTargets <br /> POST /sp/negativeTargets/delete <br /> POST /sp/campaignNegativeTargets <br /> POST /sp/campaignNegativeTargets/list <br /> PUT /sp/campaignNegativeTargets <br /> POST /sp/campaignNegativeTargets/delete | [SP negative targeting overview](guides/sponsored-products/negative-targeting/product-brand) |
| [sp/keywords](sponsored-products/3-0/openapi/prod#tag/Keywords) | POST /sp/keywords <br /> POST /sp/keywords/list <br /> PUT /sp/keywords <br /> POST /sp/keywords/delete | [SP campaign model diagram](guides/sponsored-products/get-started/campaign-structure) <br /> [SP keywords overview](guides/sponsored-products/keywords/overview) |
| [sp/negativeKeywords](sponsored-products/3-0/openapi/prod#tag/NegativeKeywords) <br /> [sp/campaignNegativeKeywords](sponsored-products/3-0/openapi/prod#tag/NegativeKeywords)| POST /sp/negativeKeywords <br /> POST /sp/negativeKeywords/list <br /> PUT /sp/negativeKeywords <br /> POST /sp/negativeKeywords/delete <br />POST  /sp/campaignNegativeKeywords <br /> POST /sp/campaignNegativeKeywords/list <br /> PUT /sp/campaignNegativeKeywords <br /> POST /sp/campaignNegativeKeywords/delete| [SP negative keywords overview](guides/sponsored-products/negative-targeting/keywords) |
| [sb/targets](sponsored-brands/3-0/openapi#tag/Product-targeting) | POST /sb/targets <br /> POST /sb/targets/list <br /> PUT /sb/targets <br /> DELETE /sb/targets/{targetId} | [SB targets overview](guides/sponsored-brands/targeting/product-targeting) |
| [sb/negativeTargets](sponsored-brands/3-0/openapi#tag/Negative-product-targeting) | POST /sb/negativeTargets <br /> POST /sb/negativeTargets/list <br /> PUT /sb/negativeTargets <br /> DELETE /sb/negativeTargets/{targetId} | [SB negative targets overview](guides/sponsored-brands/targeting/negative-product-targeting) |
| [sb/keywords](sponsored-brands/3-0/openapi#tag/Keywords) | POST /sb/keywords <br /> GET /sb/keywords <br /> PUT /sb/keywords <br /> DELETE /sb/keywords/{keywordId} | [SB keywords overview](guides/sponsored-brands/targeting/keyword-targeting) |
| [sb/negativeKeywords](sponsored-brands/3-0/openapi#tag/Negative-keywords) | POST /sb/negativeKeywords <br /> GET /sb/negativeKeywords <br /> PUT /sb/negativeKeywords <br /> DELETE /sb/negativeKeywords/{keywordId} | [SB negative keywords overview](guides/sponsored-brands/targeting/negative-keyword-targeting) |
| [sb/themes](sponsored-brands/3-0/openapi#tag/Theme-targeting) | POST /sb/themes/list <br /> PUT /sb/themes <br /> POST /sb/themes | [Theme targeting overview](guides/sponsored-brands/theme-based-targeting) |
| [sd/targeting](sponsored-display/3-0/openapi#tag/Targeting/operation/listTargetingClauses) | POST /sd/targets <br /> GET sd/targets <br /> GET sd/targets/extended <br /> PUT /sd/targets <br /> DELETE /sd/targets/{targetId} | [SD audiences overview](guides/sponsored-display/audience-targeting) <br /> [SD contextual targeting overview](guides/sponsored-display/contextual-targeting)|
| [sd/negativeTargeting](sponsored-display/3-0/openapi#tag/Negative-Targeting) | POST /sd/negativeTargets <br /> GET sd/negativeTargets <br /> GET sd/negativeTargets/extended <br /> PUT /sd/negativeTargets <br /> DELETE /sd/negativeTargets/{targetId} | |
| [Amazon Marketing Stream](guides/amazon-marketing-stream/overview) | N/A | [Targets dataset](guides/amazon-marketing-stream/datasets/sponsored-ads-campaign-management#targets-dataset-beta) |
| [targets/exports](exports) | POST /targets/exports | [Exports overview](guides/exports/overview) |

## JSON examples

Below you can find examples of how each ad product represents a target. 

### Generic

The generic sample includes a JSON representation of all possible fields in the common schema.

```json
{
    "campaignId": "string",
    "targetId": "string",
    "adGroupId": "string",
    "adProduct": "SPONSORED_DISPLAY",
    "state": "ENABLED",
    "negative": true,
     "bid": {
        "currencyCode": "USD",
        "bid": 0.0
    },
    "targetDetails": {
        "matchType": "EXACT",
        "asin": "string",
        "keyword": "string",
        "productCategoryId": "string",
        "productCategoryResolved": "Women's Fashion",
        "productBrand": "string",
        "productBrandResolved": "string",
        "productGenre": "string",
        "productGenreResolved": "string",
        "productPriceLessThan": 0.0,
        "productPriceGreaterThan": 0.0,
        "productRatingLessThan": 0.0,
        "productRatingGreaterThan": 0.0,
        "productAgeRange": "string",
        "productAgeRangeResolved": "string",
        "productPrimeShippingEligible": false,
        "audienceId": "string",
        "lookback": 0,
        "event": "VIEW"
    },
    "targetType": "PRODUCT",
    "deliveryStatus": "NOT_DELIVERING",
    "deliveryReasons": [
        "CAMPAIGN_PENDING_START_DATE"
    ],
    "creationDateTime": "2023-10-26T14:39:13.241Z",
    "lastUpdatedDateTime": "2023-10-26T14:39:13.254Z"
}
```

### Sponsored Products

This example includes all possible fields that could be present in a Sponsored Products target, regardless of targetType.

```json
{
    "campaignId": "string",
    "targetId": "string",
    "adGroupId": "string",
    "adProduct": "SPONSORED_PRODUCTS",
    "state": "ENABLED",
    "negative": true,
    "bid": {
        "currencyCode": "USD",
        "bid": 0.0
    },
    "targetDetails": {
        "matchType": "EXACT",
        "asin": "string",
        "keyword": "string",
        "productCategoryId": "string",
        "productCategoryResolved": "string",
        "productBrand": "string",
        "productBrandResolved": "string",
        "productGenre": "string",
        "productGenreResolved": "string",
        "productPriceLessThan": 0.0,
        "productPriceGreaterThan": 0.0,
        "productRatingLessThan": 0.0,
        "productRatingGreaterThan": 0.0,
        "productAgeRange": "string",
        "productAgeRangeResolved": "string",
        "productPrimeShippingEligible": false
    },
    "targetType": "KEYWORD",
    "deliveryStatus": "NOT_DELIVERING",
    "deliveryReasons": [
        "CAMPAIGN_PENDING_START_DATE"
    ],
    "creationDateTime": "2023-10-26T14:39:13.241Z",
    "lastUpdatedDateTime": "2023-10-26T14:39:13.254Z"
}
```

### Sponsored Brands

This example includes all possible fields that could be present in a Sponsored Brands target, regardless of targetType.


```json
{
    "targetId": "string",
    "adGroupId": "string",
    "adProduct": "SPONSORED_BRANDS",
    "state": "ENABLED",
    "negative": true,
    "bid": {
        "currencyCode": "USD",
        "bid": 0.0
    },
    "targetDetails": {
        "matchType": "EXACT",
        "asin": "string",
        "keyword": "string",
        "productCategoryId": "string",
        "productCategoryResolved": "string",
        "productBrand": "string",
        "productBrandResolved": "string",
        "productGenre": "string",
        "productGenreResolved": "string",
        "productPriceLessThan": 0.0,
        "productPriceGreaterThan": 0.0,
        "productRatingLessThan": 0.0,
        "productRatingGreaterThan": 0.0,
        "productAgeRange": "string",
        "productAgeRangeResolved": "string",
        "productPrimeShippingEligible": false
    },
    "targetType": "KEYWORD",
    "deliveryStatus": "NOT_DELIVERING",
    "deliveryReasons": [
        "CAMPAIGN_PENDING_START_DATE"
    ],
    "creationDateTime": "2023-10-26T14:39:13.241Z",
    "lastUpdatedDateTime": "2023-10-26T14:39:13.254Z"
}
```

### Sponsored Display

This example includes all possible fields that could be present in a Sponsored Display target, regardless of targetType.

```json
{
    "targetId": "string",
    "adGroupId": "string",
    "adProduct": "SPONSORED_DISPLAY",
    "state": "ENABLED",
    "negative": true,
    "bid": {
        "currencyCode": "USD",
        "bid": 0.0
    },
    "targetDetails": {
        "matchType": "EXACT",
        "asin": "string",
        "productCategoryId": "string",
        "productCategoryResolved": "string",
        "productBrand": "string",
        "productBrandResolved": "string",
        "productGenre": "string",
        "productGenreResolved": "string",
        "productPriceLessThan": 0.0,
        "productPriceGreaterThan": 0.0,
        "productRatingLessThan": 0.0,
        "productRatingGreaterThan": 0.0,
        "productAgeRange": "string",
        "productAgeRangeResolved": "string",
        "productPrimeShippingEligible": false,
        "audienceId": "string",
        "lookback": 0,
        "event": "VIEW",
    },
    "targetType": "PRODUCT",
    "deliveryStatus": "NOT_DELIVERING",
    "deliveryReasons": [
        "CAMPAIGN_PENDING_START_DATE"
    ],
    "creationDateTime": "2023-10-26T14:39:13.241Z",
    "lastUpdatedDateTime": "2023-10-26T14:39:13.254Z"
}
```

### Auto target

```json
{
        "adGroupId": "string",
        "targetId": "string",
        "adProduct": "SPONSORED_BRANDS",
        "state": "ENABLED",
        "bid": {
            "currencyCode": "USD",
            "bid": 0.0
        },
        "negative": false,
        "targetDetails": {
            "matchType": "SEARCH_RELATED_TO_YOUR_LANDING_PAGES"
        },
        "targetType": "AUTO",
        "deliveryStatus": "DELIVERING",
        "creationDateTime": "string",
        "lastUpdatedDateTime": "string"
    }
```

### Keyword target

#### Positive keyword target

```json
{
        "adGroupId": "string",
        "targetId": "string",
        "adProduct": "SPONSORED_PRODUCTS",
        "state": "ENABLED",
        "bid": {
            "currencyCode": "USD",
            "bid": 0.0
        },
        "negative": false,
        "targetDetails": {
            "matchType": "EXACT",
            "keyword": "string",
            "nativeLanguageKeyword": "string",
            "nativeLanguageLocale": "string"
        },
        "targetType": "KEYWORD",
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "CAMPAIGN_PENDING_START_DATE"
        ],
        "creationDateTime": "string",
        "lastUpdatedDateTime": "string"
}
```

#### Negative keyword target (ad group level)

```json
{
        "adGroupId": "string",
        "targetId": "string",
        "adProduct": "SPONSORED_PRODUCTS",
        "state": "ENABLED",
        "negative": true,
        "targetDetails": {
            "matchType": "EXACT",
            "keyword": "string",
            "nativeLanguageKeyword": "string",
            "nativeLanguageLocale": "string"
        },
        "targetType": "KEYWORD",
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "CAMPAIGN_PENDING_START_DATE"
        ],
        "creationDateTime": "string",
        "lastUpdatedDateTime": "string"
}
```

#### Negative keyword target (campaign level)

```json
{
        "campaignId": "string",
        "targetId": "string",
        "adProduct": "SPONSORED_PRODUCTS",
        "state": "ENABLED",
        "negative": true,
        "targetDetails": {
            "matchType": "EXACT",
            "keyword": "string"
        },
        "targetType": "KEYWORD",
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "CAMPAIGN_PENDING_START_DATE"
        ],
        "creationDateTime": "string",
        "lastUpdatedDateTime": "string"
}
```

### Product category target

#### Positive product category target

```json
{
        "adGroupId": "string",
        "targetId": "string",
        "adProduct": "SPONSORED_DISPLAY",
        "state": "ENABLED",
        "bid": {
            "currencyCode": "USD",
            "bid": 0.0
        },
        "negative": false,
        "targetDetails": {
            "productCategoryId": "string",
            "productCategoryResolved": "string",
            "productBrand": "string",
            "productBrandResolved": "string",
            "productGenre": "string",
            "productGenreResolved": "string",
            "productPriceLessThan": 0.0,
            "productPriceGreaterThan": 0.0,
            "productRatingLessThan": 0.0,
            "productRatingGreaterThan": 0.0,
            "productAgeRange": "string",
            "productAgeRangeResolved": "string",
            "productPrimeShippingEligible": false,
        },
        "targetType": "PRODUCT_CATEGORY",
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "OTHER"
        ],
        "creationDateTime": "string",
        "lastUpdatedDateTime": "string"
    
}
```

#### Negative product category target (ad group level)

```json
{
        "adGroupId": "string",
        "targetId": "string",
        "adProduct": "SPONSORED_DISPLAY",
        "state": "ENABLED",
        "negative": true,
        "targetDetails": {
            "productBrand": "string",
            "productBrandResolved": "string"
        },
        "targetType": "PRODUCT_CATEGORY",
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "OTHER"
        ],
        "creationDateTime": "string",
        "lastUpdatedDateTime": "string"
}
```

#### Negative product category target (campaign level)

```json
{
        "campaignId": "string",
        "targetId": "string",
        "adProduct": "SPONSORED_DISPLAY",
        "state": "ENABLED",
        "negative": true,
        "targetDetails": {
            "productBrand": "string",
            "productBrandResolved": "string"
        },
        "targetType": "PRODUCT_CATEGORY",
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "OTHER"
        ],
        "creationDateTime": "string",
        "lastUpdatedDateTime": "string"
}
```

### Product target

#### Positive product target

```json
{
        "adGroupId": "string",
        "targetId": "string",
        "adProduct": "SPONSORED_PRODUCTS",
        "state": "ENABLED",
        "bid": {
            "currencyCode": "USD",
            "bid": 0.0
        },
        "negative": false,
        "targetDetails": {
            "matchType": "PRODUCT_EXACT",
            "asin": "string"
        },
        "targetType": "PRODUCT",
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "CAMPAIGN_ENDED"
        ],
        "creationDateTime": "string",
        "lastUpdatedDateTime": "string"
    }
```

#### Negative product target (ad group level)

```json
{
        "adGroupId": "string",
        "targetId": "string",
        "adProduct": "SPONSORED_PRODUCTS",
        "state": "ENABLED",
        "negative": true,
        "targetDetails": {
            "matchType": "PRODUCT_EXACT",
            "asin": "string"
        },
        "targetType": "PRODUCT",
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "CAMPAIGN_ENDED"
        ],
        "creationDateTime": "string",
        "lastUpdatedDateTime": "string"
    }
```

#### Negative product target (campaign level)

```json
{
        "campaignId": "string",
        "targetId": "string",
        "adProduct": "SPONSORED_PRODUCTS",
        "state": "ENABLED",
        "negative": true,
        "targetDetails": {
            "matchType": "PRODUCT_EXACT",
            "asin": "string"
        },
        "targetType": "PRODUCT",
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "CAMPAIGN_ENDED"
        ],
        "creationDateTime": "string",
        "lastUpdatedDateTime": "string"
    }
```

### Product category audience target

```json
{
        "adGroupId": "string",
        "targetId": "string",
        "adProduct": "SPONSORED_DISPLAY",
        "state": "ENABLED",
        "bid": {
            "currencyCode": "USD",
            "bid": 0.0
        },
        "negative": false,
        "targetDetails": {
            "productCategoryId": "string",
            "productCategoryResolved": "string",
            "productBrand": "string",
            "productBrandResolved": "string",
            "productGenre": "string",
            "productGenreResolved": "string",
            "productPriceLessThan": 0.0,
            "productPriceGreaterThan": 0.0,
            "productRatingLessThan": 0.0,
            "productRatingGreaterThan": 0.0,
            "productAgeRange": "string",
            "productAgeRangeResolved": "string",
            "productPrimeShippingEligible": false,
            "event": "VIEWS",
            "lookback": "string"
        },
        "targetType": "PRODUCT_CATEGORY_AUDIENCE",
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "OTHER"
        ],
        "creationDateTime": "string",
        "lastUpdatedDateTime": "string"
    }

```

### Product audience target

```json
{
        "adGroupId": "string",
        "targetId": "string",
        "adProduct": "SPONSORED_DISPLAY",
        "state": "ENABLED",
        "bid": {
            "currencyCode": "USD",
            "bid": 0.0
        },
        "negative": false,
        "targetDetails": {
            "matchType": "PRODUCT_EXACT", 
            "event": "VIEWS",
            "lookback": "string"
        },
        "targetType": "PRODUCT_AUDIENCE",
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "OTHER"
        ],
        "creationDateTime": "string",
        "lastUpdatedDateTime": "string"
    }

```

### Audience target

```json
{
        "adGroupId": "string",
        "targetId": "string",
        "adProduct": "SPONSORED_DISPLAY",
        "state": "ENABLED",
        "bid": {
            "currencyCode": "USD",
            "bid": 0.0
        },
        "negative": false,
        "targetDetails": {
            "audienceId": "string"
        },
        "targetType": "AUDIENCE",
        "deliveryStatus": "NOT_DELIVERING",
        "deliveryReasons": [
            "CAMPAIGN_PAUSED"
        ],
        "creationDateTime": "string",
        "lastUpdatedDateTime": "string"
    }

```


