---
title: Persona builder
description: Overview of the Persona builder API for DSP
type: guide
interface: api
tags:
    - DSP
keywords:
    - audiences
---

# Persona builder

The [Persona Builder API](persona-builder) helps brands get insights on custom, composite audience expressions. The API provides the following functionality: 

- [Get a banded size -- an upper and lower bound estimate -- of the number of unique customers within your audience](persona-builder#tag/Persona-Builder-API/operation/BandedSize)
- [Get demographic insights](persona-builder#tag/Persona-Builder-API/operation/Demographics)
- [Discover the top retail categories purchased by customers in your audience](persona-builder#tag/Persona-Builder-API/operation/TopCategoriesPurchased)
- [Find overlapping audiences](persona-builder#tag/Persona-Builder-API/operation/TopOverlappingAudiences)
- [Review Prime Video insights for your audience](persona-builder#tag/Persona-Builder-API/operation/Demographics)

To use the API, first define your audience using `audienceTargetingExpression`. You can use any custom-built, demographic, in-market, interest, life event, lifestyle, advertiser, or lookalike audience returned from the [Audience discovery API](audiences#tag/Discovery/operation/listAudiences). You can either use these audiences themselves or combine them together to define an `audienceTargetingExpression`. For example, this expression defines a custome audience consisting of both Amazon “interested in technology” and “in market for headphones” audiences:

```bash
{
  "audienceTargetingExpression": {
    "audiences": [
      {
        "negative": "false",
        "groupId": "interested_in_technology",
        "audienceId": "0000000000"
      },
      {
        "negative": "false",
        "groupId": "in_market_for_headphones",
        "audienceId": "0000000000"
      }
    ]
  }
}
```

## Banded size estimates

To get the banded size for your audience expression, call the following API endpoint with the audience expression:

[POST /insights/bandedSize](persona-builder#tag/Persona-Builder-API/operation/BandedSize)

The API will return the estimated size, from lower to upper bound, of the audience in this expression.

#### Sample request

```bash
curl --location 'https://advertising-api.amazon.com/insights/bandedSize?advertiserId=xxxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-xxxxxxxxx' \
    --header 'Authorization: Bearer Atza|xxxxxxx' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
    --data '{
        "audienceTargetingExpression": {
            "audiences": [
                {
                "negative": "false",
                "groupId": "Group 1",
                "audienceId": "4193226111111"
                },
                {
                "negative": "false",
                "groupId": "Group 1",
                "audienceId": "37638381111111"
                }
            ]
        }
    }'
```

#### Sample response

```bash
{
    "estimatedSize": {
        "max": 000000000,
        "min": 000000000
    }
}
```

## Demographic insights

To get demographic insights for your audience expression, call the following API endpoint: 

[POST /insights/demographics](persona-builder#tag/Persona-Builder-API/operation/Demographics)

The API will return a list of demographic insights, such as age and gender. Affinity, overlap percentage, and segment ID of the corresponding demographic audience are provided for each insight. 

#### Sample request

```bash
curl --location 'https://advertising-api.amazon.com/insights/demographics?advertiserId=xxxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-xxxxxxxxx' \
    --header 'Authorization: Bearer Atza|xxxxxxx' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
    --data '{
        "audienceTargetingExpression": {
            "audiences": [
                {
                "negative": "false",
                "groupId": "Group 1",
                "audienceId": "4193111111"
                },
                {
                "negative": "false",
                "groupId": "Group 1",
                "audienceId": "3763111111"
                }
            ]
        }
    }'
```

#### Sample response

```bash
{
    "demographics": {
        "age": [
            {
                "insightMetrics": {
                    "affinity": 00.00,
                    "overlapPercentage": 00.00
                },
                "range": {
                    "max": 00,
                    "min":00
                },
                "segmentId": "0000000000"
            },
            ...
        ],
        "gender": [
            {
                "attribute": "Example 1",
                "insightMetrics": {
                    "affinity": 00.00,
                    "overlapPercentage": 00.00
                },
                "segmentId": "0000000000"
            },
            {
                "attribute": "Example 2",
                "insightMetrics": {
                    "affinity": 00.00,
                    "overlapPercentage": 00.00
                },
                "segmentId": "0000000000"
            }
        ],
}
```

## Top categories purchased

To get insights on top retail categories purchased, call the following API endpoint: 

[POST /insights/topCategoriesPurchased](persona-builder#tag/Persona-Builder-API/operation/TopCategoriesPurchased)

The API will return a list of top retail categories purchased, including the browse node name and path. Results are paginated.

#### Sample request

```bash
curl --location 'https://advertising-api.amazon.com/insights/topCategoriesPurchased?advertiserId=xxxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-xxxxxxxxx' \
    --header 'Authorization: Bearer Atza|xxxxxxx' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
    --data '{
        "categoryFilter": ["INTEREST"],
        "audienceTargetingExpression": {
            "audiences": [
                {
                "negative": "false",
                "groupId": "Group 1",
                "audienceId": "4193226111111"
                },
                {
                "negative": "false",
                "groupId": "Group 1",
                "audienceId": "37638381111111"
                }
            ]
        }
    }'
```

#### Sample response

```bash
"retailCategories": [
        {
            "id": "0000000000",
            "insightMetrics": {
                "affinity": 00.00,
                "overlapPercentage": 00.00
            },
            "name": "Example Category 1",
            "path": [
                {
                    "browseNodeName": "Example Browse Node 1"
                },
                {
                    "browseNodeName": "Example Browse Node 2"
                }
            ]
        },
        {
            "id": "0000000000",
            "insightMetrics": {
                "affinity": 00.00,
                "overlapPercentage": 00.00
            },
            "name": "Example Category 2",
            "path": [
                {
                    "browseNodeName": "Example Browse Node 3"
                },
                {
                    "browseNodeName": "Example Browse Node 4"
                }
            ]
        },
        ...
      ],
    "nextToken": "TRUNCATED_TOKEN"
}
```

## Overlapping audiences

To get insights on top overlapping audiences, call the following API endpoint: 

[POST /insights/topOverlappingAudiences](persona-builder#tag/Persona-Builder-API/operation/TopOverlappingAudiences)

The API will return a list of top overlapping audiences for this audience expression, including audience category, forecast daily impressions, and forecast daily reach for each overlapping audience. Results are paginated. You can also filter the results to a specific set of audience categories only.

#### Sample request

```bash
curl --location 'https://advertising-api.amazon.com/insights/topOverlappingAudiences?advertiserId=xxxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-xxxxxxxxx' \
    --header 'Authorization: Bearer Atza|xxxxxxx' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
    --data '{
        "categoryFilter": ["INTEREST"],
        "audienceTargetingExpression": {
            "audiences": [
                {
                "negative": "false",
                "groupId": "Group 1",
                "audienceId": "4193111111"
                },
                {
                "negative": "false",
                "groupId": "Group 1",
                "audienceId": "3763111111"
                }
            ]
        }
    }'
```

#### Sample response

```bash
{
    "audiences": [
        {
            "category": "Example Category 1",
            "forecast": {
                "dailyImpressions": {
                    "lowerBound": 00000000
                },
                "dailyReach": {
                    "lowerBound": 00000000,
                    "upperBound": 00000000
                }
            },
            "id": "0000000000",
            "insightMetrics": {
                "affinity": 00.00,
                "overlapPercentage": 00.00
            },
            "name": "Example 1"
        },
        {
            "category": "Example Category 2",
            "forecast": {
                "dailyImpressions": {
                    "lowerBound": 00000000
                },
                "dailyReach": {
                    "lowerBound": 00000000,
                    "upperBound": 00000000
                }
            },
            "id": "0000000000",
            "insightMetrics": {
                "affinity": 00.00,
                "overlapPercentage": 00.00
            },
            "name": "Example 2"
        },
    "nextToken": "TRUNCATED_TOKEN"
}
```

## Prime Video

To get insights on top Prime Video content and artists streamed, call the following API endpoint: 

`POST /insights/primeVideo`

The API will return a list of top Prime Video genres, actors, directors, series, and movies and a dateRange value that specifies the corresponding time period. You can filter these to get results for a specific insight type, for example, for top movies only.

#### Sample request

```bash
curl --location 'https://advertising-api.amazon.com/insights/primeVideo?advertiserId=xxxxxxx' \
    --header 'Amazon-Advertising-API-ClientId: amzn1.application-xxxxxxxxx' \
    --header 'Authorization: Bearer Atza|xxxxxxx' \
    --header 'Accept: application/json' \
    --header 'Content-Type: application/json' \
    --header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
    --data '{
        "categoryFilter": ["DIRECTORS"],
        "audienceTargetingExpression": {
            "audiences": [
                {
                "negative": "false",
                "groupId": "Group 1",
                "audienceId": "4193226111111"
                },
                {
                "negative": "false",
                "groupId": "Group 1",
                "audienceId": "37638381111111"
                }
            ]
        }
    }'
```

#### Sample response

```bash
{
    "primeVideoInsights": {
        "actors": [
            {
                "actor": "Example 1",
                "insight": {
                    "affinity": 00.00,
                    "overlapPercentage": 00.00
                }
            },
            ...
         ],
        "directors": [
            {
                "director": "Example 2",
                "insight": {
                    "affinity": 00.00,
                    "overlapPercentage": 00.00
                }
            },
            ...
         ],
        "genres": [
            {
                "genre": "Example 3",
                "insight": {
                    "affinity": 00.00,
                    "overlapPercentage": 00.00
                }
            },
            ...
         ],
        "movies": [
            {
                "movie": "Example 4",
                "insight": {
                    "affinity": 00.00,
                    "overlapPercentage": 00.00
                },
            },
            ...
         ],
     "series": [
            {
                "series": "Example 5"
                "insight": {
                    "affinity": 00.00,
                    "overlapPercentage": 00.00
                },
            },
            ...
         ],
      "dateRange": {
            "endDate": "0000-00-00T00:00:00Z",
            "startDate": "0000-00-00T00:00:00Z"
        }
}
```

## Save your audience expression to Amazon DSP using the combined audience API

This audience expression can be saved to Amazon DSP as a combined audience using the [combined audience API](dsp-combined-audiences). To do this, use the following API call:

[POST /dsp/audiences/combinedAudiences](dsp-combined-audiences#tag/Combined-Audience-API/operation/CreateCombinedAudience)

#### Sample response

```bash
{
    "idempotencyKey": "0000000-0000-0000-0000-000000000000",
    "name": "Example Combined Audience",
    "description": "Combined audience description",
    "inputExpression": {
      "audienceTargetingExpression": {
            {
                "negative": "false",
                "groupId": "intrested_in_technology",
                "audienceId": "0000000000"
            },
            {
                "negative": "false",
                "groupId": "in_market_for_headphones",
                "audienceId": "0000000000"
            }
        ]
      }
    }
  }
```

The API will create the combined audience and will return its audience ID.

```bash
{
    "audienceId": "0000000000"
}
```

## View an existing combined audience

To view an existing combined audience, use the following API call with the audience ID:

[GET dsp/audiences/combinedAudiences/{audienceID}](dsp-combined-audiences#tag/Combined-Audience-API/operation/GetCombinedAudienceDetails)

The API will return the following information about this audience:

#### Sample response

```bash
{
    "audienceId": "0000000000",
    "audienceName": "Example Combined Audience",
    "category": "Combined Audience",
    "createDate": "0000-000-00T00:00:00.000Z",
    "description": "Combined audience description",
    "expression": {
        "audienceTargetingExpression": {
            "audiences": [
                {
                    "negative": "false",
                    "groupId": "interested_in_technology",
                    "audienceId": "0000000000"
                },
                {
                    "negative": "false",
                    "groupId": "in_market_for_headphones",
                    "audienceId": "0000000000"
                }
            ]
        }
    },
    "status": "Active",
    "updateDate": "0000-000-00T00:00:00.000Z"
}
```