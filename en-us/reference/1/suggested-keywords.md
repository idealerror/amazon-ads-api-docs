---
title: Suggested keywords
description: Suggested keywords endpoints in version 1 of the Amazon Ads API.
type: guide
interface: api
---
# Suggested keywords

**Note: See the complete list of valid keyword characters and constraints in the [Limits](reference/concepts/limits#keyword-character-constraints) section.**

Used to request suggested keywords at the ad group and ASIN levels.
Suggested keywords are ordered by most effective to least.

| Method | URL | Use Case  |
|--------|-----|-----------|
| GET    | `/adGroups/{adGroupId}/suggested/keywords` | Request suggested keywords for specified ad group |
| GET    | `/adGroups/{adGroupId}/suggested/keywords/extended` | Provides suggested keywords for Ad Group. Returns additional fields: `campaignId`, `adGroupId`, and `state` (of the ad) with each suggested keyword |
| GET    | `/asins/{asinValue}/suggested/keywords` | Provides suggested keywords for specified ASIN |
| POST   | `/asins/suggested/keywords` | Provides suggested keywords for specified list of ASINs |

## Operations

### getAdGroupSuggestedKeywords

```shell
GET /adGroups/{adGroupId}/suggested/keywords
```

Retrieve suggested keyword data for the specified `adGroupId`

#### Parameters

| Parameter Name    | Type   | Specified In | Description |
|-------------------|--------|--------------|-------------|
| `adGroupId`         | number | URL path     | The ID of the requested ad group |
| `maxNumSuggestions` | number | URL query    | Maximum number of returned suggested keywords. Default is 100, maximum is 1000 |
| `adStateFilter`     | string | URL query    | Ad state filter (values are comma separated), to filter out the Ads to get suggested keywords for their ASINs. Valid values are: `enabled`, `paused`, and `archived`. Default values are `enabled` and `paused` |

#### Response

| Status Code   | Response object                               |
|---------------|-----------------------------------------------|
| `200 - Success` | Suggested keywords for specified `adGroupId`    |
| `400 – Error`   | No Ad Group was found for specified `adGroupId` |
| `404 – Error`   | One or more incorrect URL query parameters      |

### getAdGroupSuggestedKeywordsEx

```shell
GET /adGroups/{adGroupId}/suggested/keywords/extended
```

Retrieve extended suggested keyword data for the specified `adGroupId`

#### Parameters

| Parameter Name    | Type   | Specified In | Description |
|-------------------|--------|--------------|-------------|
| `adGroupId`         | number | URL path     | The ID of the requested ad group |
| `maxNumSuggestions` | number | URL query    | Maximum expected number of suggested keywords. Default is 100, maximum is 1000 |
| `suggestBids`       | string | URL query    | Valid values are yes and no. Default value is no. If yes, any suggested keywords can contain the extra bid field, where bid will be populated by calculated suggested bid.  **Note:** The bid field will be missing if there are no suggested bids for the keyword |
| `adStateFilter`     | string | URL query    | Ad state filter (values are comma separated) to filter out the ads to get suggested keywords for their ASINs. Valid values are: `enabled`, `paused`, and `archived`. Default values are `enabled` and `paused` |

#### Response

| Status Code     | Response object                                     |
|-----------------|-----------------------------------------------------|
| `200 - Success`   | Extended suggested keywords for specified `adGroupId` |
| `404 – Not Found` | No Ad Group was found for specified `adGroupId`       |
| `400 – Error`     | One or more incorrect URL query parameters            |

### getAsinSuggestedKeywords

```shell
GET /asins/{asinValue}/suggested/keywords
```

Provides suggested keywords for specified ASIN. Suggested keywords are ordered by most effective to least effective.

#### Parameters

| Parameter Name    | Type   | Specified In | Description                                                                    |
|-------------------|--------|--------------|--------------------------------------------------------------------------------|
| `asinValue`         | string | URL path     | ASIN                                                                           |
| `maxNumSuggestions` | number | URL query    | Maximum number of returned suggested keywords. Default is 100, maximum is 1000 |

#### Response

| Status Code   | Response object                                          |
|---------------|----------------------------------------------------------|
| `200 – Success` | Suggested keywords for specified `asinValue`               |
| `400 – Error`   | One or more incorrect URL query parameters or incorrect ASIN |

### bulkGetAsinSuggestedKeywords

```shell
POST /asins/suggested/keywords
```

Provides keyword suggestions for specified list of ASINs. Suggested keywords are ordered by most effective to least effective.

#### Parameters

| Parameter name    | Type            | Specified in | Description                                                                    |
|-------------------|-----------------|--------------|--------------------------------------------------------------------------------|
| `asins`            | List of strings | body         | List of ASINs for which keywords are suggested                                 |
| `maxNumSuggestions` | Number          | body         | Maximum number of returned suggested keywords. Default is 100, maximum is 1000 |

#### Response

| Status Code   | Response object                                          |
|---------------|----------------------------------------------------------|
| `200 – Success` | Suggested keywords for specified list of ASINs           |
| `400 – Error`   | One or more incorrect URL query parameters or incorrect ASIN |

## Resource representations

### AdGroupSuggestedKeywordsResponse

```JSON
{
    "title": "AdGroupSuggestedKeywordsResponse",
    "type": "object",
    "properties": {
        "adGroupId": {
            "description": "The ID of the requested ad group.",
            "type": "number"
        },
        "suggestedKeywords": {
            "description": "List of suggested keywords.",
            "type": "array"
        }
    }
}
```

### GetAdGroupSuggestedKeywordsExResponse

```JSON
{
    "title": "GetAdGroupSuggestedKeywordsExResponse",
    "type": "array",
    "properties": {
        "adGroupId": {
            "description": "The ID of the requested ad group",
            "type": "number"
        },
        "campaignId": {
            "description": "The campaign ID in which the ad group belongs to",
            "type": "number"
        },
        "keywordText": {
            "description": "The suggested keyword",
            "type": "string"
        },
        "matchType": {
            "description": "Match type of the suggested keyword",
            "type": "string"
        },
        "state": {
            "description": "The state of the ad for which the keyword is suggested. Should be either enabled or paused",
            "type": "string"
        },
        "bid": {
            "description": "The keyword bid suggestion. Will only be shown if suggestBid is 'yes' and the keyword has a bid suggestion",
            "type": "number"
        }
    }
}
```

### GetAsinSuggestedKeywordsResponse

```JSON
{
    "title": "GetAsinSuggestedKeywordsResponse",
    "type": "object",
    "properties": {
        "asin": {
            "description": "The ASIN for which a keyword suggestion is requested",
            "type": "string"
        },
        "suggestedKeywords": {
            "description": "List of suggested keywords",
            "type": "array",
            "properties": {
                "keywordText": {
                    "description": "The suggested keyword",
                    "type": "string"
                },
                "matchType": {
                    "description": "Match type of the suggested keyword",
                    "type": "string"
                }
            }
        }
    }
}
```

### BulkGetAsinSuggestedKeywordsResponse

```JSON
{
    "title": "BulkGetAsinSuggestedKeywordsResponse",
    "type": "array",
    "properties": {
        "keywordText": {
            "description": "The suggested keyword",
            "type": "string"
        },
        "matchType": {
            "description": "Match type of the suggested keyword",
            "type": "string"
        }
    }
}
```

### Error

See [error object return](reference/concepts/developer-notes) format as described in Developer notes.
