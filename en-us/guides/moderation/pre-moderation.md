---
title: Get started with unified pre-moderation
description: Learn how developers can get started with pre-moderation using the Amazon Ads API.  
type: guide
interface: api
---

# Unified pre-moderation

For advertisers launching sponsored ads campaigns, the unified pre-moderation API offers real-time policy guidance based on the technical, creative, and policy components of an ad. The unified pre-moderation API ensures ad compliance before moderation, enabling alignment with sponsored ads policy guidelines.

This document includes common reasons why a campaign might be rejected during moderation review, example requests to the unified pre-moderation API, and links to relevant ad policies for additional explanation of the guidelines.

## Pre-moderation rejections

The most common moderation rejections result from incorrect capitulation or grammar or the use of unsupported languages within a creative headline. However, there are other reasons why a component of an ad can be rejected. The below tables show the possible rejection statuses from the pre-moderation API for each sponsored ads type.

### Sponsored Brands error codes

|Error code	|Message	|
|---	|---	|
|END_DATE	|Campaigns with deals or savings must have an end date on or before the date of the earliest-expiring deal or saving.	|
|SEASONAL\_EVENTS\_WORD	|Campaigns with holiday or seasonal event messaging must include an end date.	|
|PUNCTUATIONS	|Incorrect or excessive punctuation isn't allowed.	|
|ELLIPSIS	|We do not permit sentences that begin with an ellipsis or sentences that contain more than 3 sets of ellipses.	|
|GRAMMAR |Check for grammatical or typographical errors.	|
|SENTENCE_STRUCTURE	|Check for grammatical or typographical errors.	|
|LISTING_PRICES	|Numeric pricing, deals, or savings such as "$4.99" or "Save 50%" aren't allowed.	|
|CAP_WORDS	|All CAPS, CaMeL case, and random capitalization aren't allowed.	|
|PROFANE_WORDS	|Profane or vulgar content isn't allowed.	|
|SPECIAL_CHARACTERS	|Special characters such as "#@%" aren't permitted.	|
|AMAZON\_SPECIFIC\_TERM	|Amazon products or services can't be referenced.	|
|ADULT_WORDS	|Profanity, obscene language, or inappropriate double meanings aren't allowed.	|
|TYPO	|Please check for grammatical errors, misspellings, or typos.	|
|SPONSOR\_NAME\_MISMATCH	|Brand name must match your registered brand name, or the name of a brand that you own or can legitimately represent.	|
|UNSUBSTANTIATED\_CLAIM\_VIOLATION	|All claims in ads must be accurate and substantiated on the product packaging or the product detail page.	|
|CUSTOM\_IMAGE\_TEXT	|Additional text within a custom image such as overlayed or background text isn't allowed.	|
|INVALID\_CUSTOM\_IMAGE	|Custom image may not meet our policy.	|
|INVALID\_BRAND\_LOGO	|Logo does not meet our policy.	|
|LANGUAGE_UNSUPPORTED	|Headline must be in the language of the Amazon site where your ad will display.	|
|FATAL\_ADULT\_WORD	|Adult products aren't allowed.	|
|FATAL\_PROFANE\_WORD	|Profane or vulgar content is not allowed.	|
|END_DATE	|Campaigns with deals or savings must have an end date on or before the date of the earliest-expiring deal or saving.	|
|CAP_WORDS	|All CAPS, CaMeL case, and random capitalization aren't allowed.	|
|FATAL\_CRITICAL\_EVENTS\_WORD	|Content related to political groups or policies, that criticize political figures or that encourage civil or political unrest is not allowed.	|
|FATAL_WORD	|Drug paraphernalia or products related to illicit drugs aren't allowed.	| 

### Sponsored Brands Video error codes

|Error code	|Message	|
|---	|---	|
|PILLAR_BOX	|Video can’t contain colored bars framing video content on any side.	|
|LETTER_BOX	|Video can’t contain colored bars framing video content on any side.	|
|GRAMMAR	|Check for grammatical or typographical errors.	|
|SENTENCE_STRUCTURE	|Check for grammatical or typographical errors.	|
|END_DATE	|Campaigns with deals or savings must have an end date on or before the date of the earliest-expiring deal or saving.	|
|SEASONAL\_EVENTS\_WORD	|Campaigns with holiday or seasonal event messaging must include an end date.	|
|ADULT_WORDS	|Profanity, obscene language, or inappropriate double meanings aren't allowed.	|
|PROFANE_WORDS	|Profane or vulgar content isn't allowed.	|
|AMAZON_BRANDING	|Amazon branded elements aren’t allowed.	|
|AMAZON\_SPECIFIC\_TERM	|Amazon products or services can't be referenced.	|
|WINDOW_FRAME	|Video can’t contain colored bars framing video content on any side.	|
|TYPO	|Typos and spelling errors need to be corrected.	|
|LANGUAGE_UNSUPPORTED	|Audio or subtitles must be in the language of the Amazon site where your ad will display.	|
|LISTING_PRICES	|Numeric pricing, deals, or savings such as "$4.99" or "Save 50%" aren't allowed.	|
|CAP_WORDS	|All CAPS, CaMeL case, and random capitalization aren't allowed.	|
|PUNCTUATIONS	|Incorrect or excessive punctuation isn't allowed.	|
|SPECIAL_CHARACTERS	|Special characters such as "#@%" aren't permitted.	|
|SPONSOR\_NAME\_MISMATCH	|Brand name must match your registered brand name, or the name of a brand that you own or can legitimately represent.	|
|UNSUBSTANTIATED\_CLAIM\_VIOLATION	|All claims in ads must be accurate and substantiated on the product packaging or the product detail page.	|
|INVALID\_BRAND\_LOGO	|Logo does not meet our policy.	|

### Sponsored Display error codes

|Error code	|Message	|
|---	|---	|
|END_DATE	|Campaigns with deals or savings must have an end date on or before the date of the earliest-expiring deal or saving.	|
|TIME\_SENSITIVE\_EVENT	|Time-sensitive promotions are prohibited unless communicated dynamically.	|
|SENTENCE_STRUCTURE	|Please check for grammatical errors, misspellings, or typos.	|
|CAP_WORDS	|Inconsistent casing isn't allowed.	|
|PUNCTUATIONS	|Incorrect or excessive punctuation isn't allowed.	|
|ELLIPSIS	|Headlines that start with an ellipsis ("..."), or that contain more than 3 instances of ellipses, aren't allowed.	|
|ADULT_WORDS	|Profanity, obscene language, or inappropriate double meanings aren't allowed.	|
|PROFANE_WORDS	|Profane or vulgar content isn't allowed.	|
|SPECIAL_CHARACTERS	|Special characters (unless part of the brand name or slogan) aren't allowed.	|
|LISTING_PRICES	|Numeric pricing, deals, or savings such as "$4.99" or "Save 50%" aren't allowed.	|
|AMAZON\_SPECIFIC\_TERM	|Amazon products or services can't be referenced.	|
|SPELL_ERROR	|Please check for misspellings or typos.	|
|UNSUBSTANTIATED\_CLAIM\_VIOLATION	|All claims in ads must be accurate and substantiated on the product packaging or the product detail page.	|
|GRAMMAR	|Please check for grammatical errors.	|
|LANGUAGE_UNSUPPORTED	|Headline must be in the language of the Amazon site where your ad will display.	|

### Stores error codes

|Error code	|Message	|
|---	|---	|
|GRAMMAR	|Please check for grammatical errors, misspellings, or typos.	|
|SENTENCE_STRUCTURE	|Please check for grammatical errors, misspellings, or typos.	|
|ELLIPSIS	|The sentence starts with an ellipses ("..."), or if it contain more than 3 instances of an ellipses aren't allowed	|
|PUNCTUATIONS	|Incorrect or excessive punctuation isn't allowed	|
|PROFANE_WORDS	|Profane or vulgar content isn't allowed	|
|SPELL_ERROR	|Please check for grammatical errors, misspellings, or typos.	|
|CAP_WORDS, CamelCaseAnnotator, CapitalizationAnnotator	|All CAPS, CaMeL case and random capitalisation are not allowed	|
|ADULT_WORDS	|Adult products aren't permitted	|

### DSP error codes

|Error code	|Message	|
|---	|---	|
|PROFANE_WORDS	|All CAPS, CaMeL case, and random capitalization aren't allowed.	|
|ADULT_WORDS	|Profanity, obscene language, or inappropriate double meanings aren't allowed.	|
|SPELL_ERROR	|Please check for grammatical errors, misspellings, or typos.	|
|GRAMMAR	|Please check for grammatical errors, misspellings, or typos.	|
|SENTENCE_STRUCTURE	|Please check for grammatical errors, misspellings, or typos.	|
|UNSUBSTANTIATED\_CLAIM\_VIOLATION	|All claims in ads must be accurate and substantiated on the product packaging or the product detail page.	|
|ELLIPSIS	|Headlines that start with an ellipsis ("..."), or that contain more than 3 instances of ellipses, aren't allowed.	|
|LISTING_PRICES	|Numerical promotions, pricing, deals, or savings such as "Save 50%" aren't permitted.	|
|CAP_WORDS	|Numeric pricing, deals, or savings such as "$4.99" or "Save 50%" aren't allowed.	|
|PUNCTUATIONS	|Profane or vulgar content isn't allowed.	|
|SPECIAL_CHARACTERS	|Special characters such as "#@%" aren't permitted.	|
|TIME\_SENSITIVE\_EVENT	|Time-sensitive promotions are prohibited.	|
|AMAZON\_SPECIFIC\_TERM	|Amazon products or services can't be referenced.	|
|END_DATE	|Campaigns with deals or savings must have an end date on or before the date of the earliest-expiring deal or saving.	|

### DSP image error codes

|Error code	|Message	|
|---	|---	|
|FATAL\_ADULT\_WORD	|This product can’t be advertised because it is prohibited.	|
|FATAL\_POLITICAL\_WORD	| This product can’t be advertised because it is prohibited.	|
|FATAL\_ILLICIT\_DRUGS\_WORD	| This product can’t be advertised because it is prohibited.	|
|FATAL\_PROFANE\_WORD	| This product can’t be advertised because it is prohibited.	|
|FATAL\_TOBACCO\_WORD	| This product can’t be advertised because it is prohibited.	|
|FATAL\_PROHIBITED\_WORD	| This product can’t be advertised because it is prohibited.	|
|FATAL\_COMPETITIVE\_PRODUCTS\_WORD	|The product advertised is restricted and has required targeting and placement restrictions.	|
|FATAL\_MEDICAL\_AND\_WEIGHT\_LOSS	| The product advertised is restricted and has required targeting and placement restrictions.	|
|FATAL\_QUESTIONABLE\_AND\_DANGEROUS\_ACTIVITY\_WORD	| The product advertised is restricted and has required targeting and placement restrictions.	|
|RESTRICTED_WORD	| The product advertised is restricted and has required targeting and placement restrictions.	|
|FONT_ILLEGIBLE	|Image must meet font requirements.	|
|IMAGE\_FONT\_ILLEGIBLE	| Image must meet font requirements.	|
|FATAL\_ALCOHOL\_WORD	|Advertising alcohol requires day-parting on the line items.	|
|UNSUITABLE\_FOR\_GENERAL\_AUDIENCE\_VIOLATION	|The product being advertised requires targeting restrictions.	|
|ANIMATION\_MORE\_THAN\_15\_SECONDS	|Animation length may not follow guidelines.	|
|ANIMATION\_MORE\_THAN\_30\_SECONDS	|Animation length may not follow guidelines.	|
|LP\_AD\_PRODUCTS\_MISMATCH	|Product in image doesn't match product on the landing page.	|
|CTA\_IN\_ALL\_CAPS	|Call-to-action shouldn't use all caps.	|
|AD\_TEXT\_OR\_DISCLAIMER\_ILLEGIBLE	|Text or disclaimer must be legible.	|
|MISSING\_OR\_AMBIGUOUS\_CTA	|Call-to-action may be ambiguous or missing.	|
|BILLBOARD\_CREATIVES\_CANNOT\_HAVE\_CTA\_IN\_BUTTON\_FORM	|Call-to-action can't be a button for billboard size.	|
|AMBIGUOUS_CTA	|Call-to-action can't be ambiguous.	|
|AMAZON\_LOGO\_BLURRY	|The Amazon logo is blurry.	|
|POOR\_IMAGE\_OR\_VIDEO\_QUALITY	|Low image or video quality.	|
|LOOPS\_MORE\_THAN\_3\_TIMES	|Animation repeats more than 3 times.	|
|IMPROPER\_BRAND\_ELEMENTS	|The Amazon logo is used incorrectly.	|
|GENERAL\_DEAL\_DATES	|The dates of the deal stated in the creative should be aligned with the line item flight dates. Please ensure the creative is only live during the time of the deal.	|
|SINGLE\_DAY\_DEAL\_DATES	|The event you’re promoting qualifies as a Single-Day Event, which means your ad can be flighted up to 8 weeks prior to the start date and must end within 24 hours after the event.	|
|MULTI\_DAY\_DEAL\_DATES	|The event you’re promoting qualifies as a Multi-Day Event, which means your ad can be flighted up to 8 weeks prior to the start date and must end no more than 7 days after the event.	|
|PRICE\_OR\_SAVINGS\_LINKING\_TO\_PDP	|A specific price or savings claim has been detected within this creative. Please remove the claim or use a responsive ecommerce creative (REC) to promote this deal	|

### DSP rec error codes

|Error code	|Message	|
|---	|---	|
|FATAL\_ADULT\_WORD	|Product is prohibited.	|
|FATAL\_POLITICAL\_WORD	| Product is prohibited.	|
|FATAL\_HATE\_AND\_INTOLERANCE\_WORD	| Product is prohibited.	|
|FATAL\_ILLICIT\_DRUGS\_WORD	| Product is prohibited.	|
|FATAL\_PROFANE\_WORD	| Product is prohibited.	|
|FATAL\_TOBACCO\_WORD	| Product is prohibited.	|
|FATAL\_PROHIBITED\_WORD	| Product is prohibited.	|
|FATAL\_COMPETITIVE\_PRODUCTS\_WORD	|The product advertised is restricted and has required targeting and placement restrictions.	|
|FATAL\_MEDICAL\_AND\_WEIGHT\_LOSS	| The product advertised is restricted and has required targeting and placement restrictions.	|
|FATAL\_QUESTIONABLE\_AND\_DANGEROUS\_ACTIVITY\_WORD	| The product advertised is restricted and has required targeting and placement restrictions.	|
|RESTRICTED_WORD	| The product advertised is restricted and has required targeting and placement restrictions.	|
|FONT_ILLEGIBLE	|Image must meet font requirements.	|
|IMAGE\_FONT\_ILLEGIBLE	| Image must meet font requirements.	|
|AMBIGUOUS_CTA	|Call-to-action can't be ambiguous.	|
|CTA\_IN\_ALL\_CAPS	| Call-to-action can't be ambiguous.	|
|GRAMMAR	|There may be grammar issues.	|
|TYPO	|There may be spelling issues.	|
|PUNCTUATIONS	|Custom headline shouldn't use multiple punctuation marks in a row.	|
|CAP_WORDS	|Custom headline should only have the first letter capitalized except brand names.	|
|UNSUITABLE\_FOR\_GENERAL\_AUDIENCE\_VIOLATION	|The product being advertised requires targeting restrictions.	|
|FATAL\_ALCOHOL\_WORD	|Advertising alcohol requires day parting on the line items.	|
|AD\_TEXT\_OR\_DISCLAIMER\_ILLEGIBLE	|Text or disclaimer must be legible.	|
|MISSING\_OR\_AMBIGUOUS\_CTA	|Call-to-action may be ambiguous or missing.	|
|BILLBOARD\_CREATIVES\_CANNOT\_HAVE\_CTA\_IN\_BUTTON\_FORM	|Call-to-action can't be a button for billboard size.	|
|AMAZON\_LOGO\_BLURRY	|The Amazon logo is blurry.	|
|POOR\_IMAGE\_OR\_VIDEO\_QUALITY	|Low image or video quality.	|

## Examples for using preModeration API 

Components that get an `APPROVED` status in pre-moderation are not guaranteed to be approved in moderation. Different component objects that can be specified in the request body depending on what needs to be pre-moderated. For more details on the specifics of each component, see [the technical specifications for the /preModeration endpoint.](pre-moderation#tag/Pre-Moderation/operation/preModeration).

>[NOTE] The id parameter value is the `assetId` obtained after [registering an asset](guides/creative-asset/creating-assets).

### Sponsored Brands Video

Request:

```json
curl --location --request POST 'https://advertising-api.amazon.com/preModeration' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--data-raw '{
    "adProgram": "SPONSORED_BRANDS_VIDEO",
    "locale": "es-US",
    "videoComponents": [
        {
            "componentType": "SPONSORED_BRANDS_VIDEO",
            "landingPage": {
                "url": "https://www.amazon.com/dp/B07R7V6KS8"
            },
            "id": "amzn1.assetlibrary.asset1.xxxxxx",
            "url": "https://www.amazon.com/dp/B07R7V6KS8"
        }
    ]
}'
```

Response:


```json
{
    "preModerationId": "a94dbd145a714b9c9ef6d464d912a096",
    "adProgram": "SPONSORED_BRANDS_VIDEO",
    "locale": "es-US",
    "videoComponents": [{
        "preModerationStatus": "APPROVED",
        "componentType": "SPONSORED_BRANDS_VIDEO",
        "policyViolations": [],
        "id": "amzn1.assetlibrary.asset1.xxxxxx",
        "url": "https://www.amazon.com/dp/B07R7V6KS8",
        "landingPage": {
            "url": "https://www.amazon.com/dp/B07R7V6KS8"
        }
    }]
}
```

### Sponsored Brands Spotlight

Request:

```json
curl --location --request POST 'https://advertising-api.amazon.com/preModeration' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--data-raw '{
    "adProgram": "SPONSORED_BRANDS_SPOTLIGHT",
    "locale": "es-US",
    "imageComponents": [
        {
            "componentType": "BRAND_LOGO",
            "landingPage": {
                "url": "https://www.amazon.com/stores/page/0064AC34-18F6-4CD3-849F-531E7C5AE8D6"
            },
            "id": "amzn1.assetlibrary.asset1.xxxxxx",
            "url": "https://www.amazon.com/dp/B07R7V6KS8"
        }
    ]
}'
```

Response:


```json
{
    "preModerationId": "9139465e3bcd4b97aca02655c8af1039",
    "adProgram": "SPONSORED_BRANDS_SPOTLIGHT",
    "locale": "es-US",
    "imageComponents": [
        {
            "preModerationStatus": "APPROVED",
            "componentType": "BRAND_LOGO",
            "policyViolations": [],
            "id": "amzn1.assetlibrary.asset1.xxxxxx",
            "url": "https://www.amazon.com/dp/B07R7V6KS8",
            "landingPage": {
                "url": "https://www.amazon.com/stores/page/0064AC34-18F6-4CD3-849F-531E7C5AE8D6"
            },
            "specViolations": []
        }
    ]
}
```

### Sponsored Brands

Request:


```json
curl --location --request POST 'https://advertising-api.amazon.com/preModeration' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{
  "asinComponents": [
    {
      "componentType": "LANDING_ASIN",
      "asin": "B07R7V6KS8",
      "id": "string"
    }
  ],
  "adProgram": "SPONSORED_BRANDS",
  "locale": "ar-AE"
}'
```

Response:


```json
{
    "preModerationId": "69ffa1d3632d4987b9b6add3f392b8af",
    "adProgram": "SPONSORED_BRANDS",
    "locale": "ar-AE",
    "asinComponents": [
        {
            "preModerationStatus": "APPROVED",
            "componentType": "LANDING_ASIN",
            "policyViolations": [],
            "asin": "B07R7V6KS8",
            "id": "string"
        }
    ]
}
```

### Sponsored Display

Request:

```json
curl --location --request POST 'https://advertising-api.amazon.com/preModeration' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--data-raw '{
  "asinComponents": [
    {
      "componentType": "LANDING_ASIN",
      "asin": "B07R7V6KS8",
      "id": "string"
    }
  ],
  "adProgram": "SPONSORED_DISPLAY",
  "locale": "ar-AE"
}'
```

Response:


```json
{
    "preModerationId": "b494c3c4c0ce4d9181bac0be883d5c13",
    "adProgram": "SPONSORED_DISPLAY",
    "locale": "ar-AE",
    "asinComponents": [
        {
            "preModerationStatus": "APPROVED",
            "componentType": "LANDING_ASIN",
            "policyViolations": [],
            "asin": "B07R7V6KS8",
            "id": "string"
        }
    ]
}
```

### Stores

Request:

```json
curl --location --request POST 'https://advertising-api.amazon.com/preModeration' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--data-raw '{
    "adProgram": "STORES",
    "locale": "es-US",
    "imageComponents": [
        {
            "componentType": "BRAND_LOGO",
            "landingPage": {
                "url": "https://www.amazon.com/dp/B07R7V6KS8"
            },
            "id": "amzn1.assetlibrary.asset1.xxxxxx",
            "url": "https://www.amazon.com/dp/B07R7V6KS8"
        }
    ]
}'
```

Response:


```json
{
    "preModerationId": "1a0627a5ef9248589ec1fd024f9a32ed",
    "adProgram": "STORES",
    "locale": "es-US",
    "imageComponents": [
        {
            "preModerationStatus": "APPROVED",
            "componentType": "BRAND_LOGO",
            "policyViolations": [],
            "id": "amzn1.assetlibrary.asset1.xxxxxx",
            "url": "https://www.amazon.com/dp/B07R7V6KS8",
            "landingPage": {
                "url": "https://www.amazon.com/dp/B07R7V6KS8"
            },
            "specViolations": []
        }
    ]
}
```

### DSP

Request:

```json
curl --location --request POST 'https://advertising-api.amazon.com/preModeration' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--data-raw '{
    "adProgram": "DSP",
    "locale": "es-US",
    "imageComponents": [
        {
            "componentType": "BRAND_LOGO",
            "landingPage": {
                "url": "https://www.amazon.com/dp/B07R7V6KS8"
            },
            "id": "amzn1.assetlibrary.asset1.xxxxxx",
            "url": "https://www.amazon.com/dp/B07R7V6KS8"
        }
    ]
}'
```

Response:

```json
{
    "preModerationId": "762ded936e794103a4de0f658cd0bfeb",
    "adProgram": "DSP",
    "locale": "es-US",
    "imageComponents": [
        {
            "preModerationStatus": "APPROVED",
            "componentType": "BRAND_LOGO",
            "policyViolations": [],
            "id": "amzn1.assetlibrary.asset1.xxxxxx",
            "url": "https://www.amazon.com/dp/B07R7V6KS8",
            "landingPage": {
                "url": "https://www.amazon.com/dp/B07R7V6KS8"
            },
            "specViolations": []
        }
    ]
}
```

## Ad component specifications and moderation guidelines

A creative must meet certain requirements before it can be approved. Below are specifications for both a brand logo and a custom image.

### Brand Logo specifications

|Specification	|Details	|
|---	|---	|
|Image size	|400 x 400 px or larger	|
|File size	|1MB or smaller	|
|File format	|PNG or JPG	|
|Content	|Logo must fill the image or have a white or transparent background. For more information, visit [Brand Logo Creative Acceptance Policy](https://advertising.amazon.com/help?ref_=AAC_unav_support_center#GYUVDSUJJQV4NGQB).	|

### Custom image specifications

|Specification	|Details	|
|---	|---	|
|Image size	|1200 x 628 px or larger	|
|File size	|5MB or smaller	|
|File format	|PNG or JPG	|
|Content	|No text, graphics, or logos added to the image. For more information, visit [Custom Image Creative Acceptance Policy](https://advertising.amazon.com/help?ref_=AAC_unav_support_center#G76M96SHBLDBRSQT).	|

For general information on moderation rules, see [Amazon Ads Guidelines and Acceptance Policies.](https://advertising.amazon.com/resources/ad-policy/creative-acceptance?ref_=a20m_us_spcs_cap).
