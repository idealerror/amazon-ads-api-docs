---
title: Sponsored Brands theme-based targeting
description: Steps to create a theme-based targeting
type: guide
interface: api
tags: 
    - Sponsored Brands
    - Campaign management
keywords:
    - Theme-based targeting
    - Sponsored Brands campaign 
---

# Sponsored Brands theme-based targeting

Theme-based targeting is a curated set of keywords that enable brands to simplify the campaign creation process and improve campaign performance through well-performing targets. `KEYWORDS_RELATED_TO_YOUR_BRAND` gathers a set of keywords that are often use to search for brands and promote brand impressions. `KEYWORDS_RELATED_TO_YOUR_LANDING_PAGES` provides a set of relevant keywords that drive traffic to brand store page or product detail pages. 

>[NOTE] If you create a campaign with the `smartDefault`  parameter with `[“TARGETING”]` value, the theme-based targeting is automatically created based on the `goal` type chosen during ad group creation. For more information, see [step one of getting started with campaigns](guides/sponsored-brands/campaigns/get-started-with-campaigns#step-1-create-a-campaign).

>[NOTE] The bid is only mutable when the keyword's corresponding campaign does not have any enabled [optimization rule](sponsored-brands/3-0/openapi/prod#tag/Optimization-Rules-(beta)).

## Before you begin

You must have the following before creating a keyword:

* [A campaign Id](guides/sponsored-brands/campaigns/get-started-with-campaigns#Create-a-campaign)
* [An ad group Id](guides/sponsored-brands/campaigns/get-started-with-campaigns#Create-ad-group)

## Creating theme-based targeting

**Endpoint**

[POST /sb/themes](sponsored-brands/3-0/openapi#tag/Theme-targeting/operation/sbCreateThemes)

**Parameters**

|Name	|Optional	|Type	|Description	|
|---	|---	|---	|---	|
|adGroupId	|No	|string	|The identifier of the ad group to which the target is associated.	|
|campaignId	|No	|string	|The identifier of the campaign to which the target is associated.	|
|themeType	|No	|string	| `KEYWORDS_RELATED_TO_YOUR_BRAND` - keywords related to brands. <br /> `KEYWORDS_RELATED_TO_YOUR_LANDING_PAGES` - keywords related to your landing pages. <br />Note: Additional theme types may be added in the future.	|
|bid	|no	|number	|The associated bid. Note that this value must be less than the budget associated with the advertiser account	|

**Example**

```
curl --location 'https://advertising-api.amazon.com/sb/themes' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Accept: application/vnd.sbthemescreateresponse.v3+json' \
--header 'Content-Type: application/json' \
--data '{
  "themes": [
    {
      "adGroupId": "{{adGroupId}}",
      "campaignId": "{{campaignId}}",
      "themeType": "KEYWORDS_RELATED_TO_YOUR_LANDING_PAGES",
      "bid": 20.0
    }
  ]
}'
```

**Response**

```
{
    "success": [
        {
            "themeId": "9172714830852",
            "index": 0
        }
    ],
    "error": []
}
```

>[NOTE] To get metrics for theme-based targets, see [keyword](guides/reporting/v2/report-types#keyword-reports) and [search term](guides/reporting/v2/report-types#search-term-reports) reports.

## Updating theme target

**Endpoint**

[PUT /sb/themes](sponsored-brands/3-0/openapi#tag/Theme-targeting/operation/sbUpdateThemes)

Parameters

|Name	|Optional	|Type	|Description	|
|---	|---	|---	|---	|
|adGroupId	|No	|string	|The identifier of the ad group to which the target is associated.	|
|campaignId	|No	|string	|The identifier of the campaign to which the target is associated.	|
|themeId	|No	|string	|The identifier of the theme target.	|
|bid	|No	|number	|The associated bid. Note that this value must be less than the budget associated with the Advertiser account	|
|state	|No	|string	|The state of the theme target. Can be set to `enabled`, `paused`, or `archived`.	|

**Example**

```
curl --location --request PUT 'https://advertising-api.amazon.com/sb/themes' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
--header 'Accept: application/vnd.sbthemesupdateresponse.v3+json' \
--header 'Content-Type: application/json' \
--data '{
  "themes": [
    {
      "themeId": "{{themeId}}",
      "adGroupId": "{{adGroupId}}",
      "campaignId": "{{campaignId}}",
      "state": "enabled",
      "bid": 10.0
    }
  ]
}'
```
**Response**

```
{
    "success": [
        {
            "themeId": "9172714830852",
            "index": 0
        }
    ],
    "error": []
}
```

## Get a list of theme targets

**Endpoint**

[POST /sb/themes/list](sponsored-brands/3-0/openapi#tag/Theme-targeting/operation/sbListThemes)

|Name	|Optional	|Type	|Description	|
|---	|---	|---	|---	|
|nextToken	|Yes	|string	|Operations that return paginated results include a pagination token in this field. To retrieve the next page of results, call the same operation and specify this token in the request. If the `NextToken` field is empty, there are no further results.	|
|maxResults	|Yes	|string	|Sets a limit on the number of results returned by an operation.	|
|themeIdFilter	|Yes	|string	|A list of theme targets identifiers. List limit is 100 items.	|
|adGroupIdFilter	|Yes	|string	|A list of ad group identifiers. List limit is 100 items.	|
|campaignIdFilter	|Yes	|string	|A list of campaign identifiers. List limit is 100 items.	|
|stateFilter	|Yes	|string	|A list of theme target states. State enums are `paused`, `enabled`, or `archived`.	|
|themeTypeFilter	|Yes	|string	|A list of theme target type. Types include: `KEYWORDS_RELATED_TO_YOUR_BRAND` `KEYWORDS_RELATED_TO_YOUR_LANDING_PAGES`	|

**Example**

```
curl --location 'https://advertising-api.amazon.com/sb/themes/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Accept: application/vnd.sbthemeslistresponse.v3+json' \
--header 'Content-Type: application/json' \
--data '{
  "maxResults": 1,
  "themeIdFilter": {
    "include": [
      {{themeId}}
    ]
  },
  "adGroupIdFilter": {
    "include": [
      {{adGroupId}}
    ]
  },
  "campaignIdFilter": {
    "include": [
      {{campaignId}}
    ]
  },
  "stateFilter": {
    "include": [
      "enabled"
    ]
  },
  "themeTypeFilter": {
    "include": [
      "KEYWORDS_RELATED_TO_YOUR_BRAND",
      "KEYWORDS_RELATED_TO_YOUR_LANDING_PAGES"
    ]
  }
}'
```
**Response**

```
{
    "themes": [
        {
            "themeId": "9172714830852",
            "adGroupId": "201448861867105",
            "campaignId": "95364022256034",
            "themeType": "KEYWORDS_RELATED_TO_YOUR_BRAND",
            "state": "enabled",
            "bid": 1.0
        }
    ]
}
```
