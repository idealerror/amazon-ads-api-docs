---
title: Discover targetable entities for Sponsored Display
description: Programmatically query for segments or entities to target based on a given a set of keywords. 
type: guide
interface: api 
tags:
    - Sponsored Display
keywords:
    - targets
---

# Getting started with discovering targetable entities

The [targetable entities search API](targetable-entities) enables you to programmatically query for segments or entities to target based on a given a set of keywords. This API provides semantic search, spelling correction, and prefix completion capabilities.

## Types of targetable entities supported

| Sponsored Display target type | Targetable entities API target type | Description |
|-------|-------|------|
| Contextual targeting | PRODUCT\_CATEGORY | Helps promote your product among audiences who are actively browsing your product or similar products and categories with ads that appear on relevant product detail pages. |
| Audience targeting | AUDIENCE | Gives advertisers access to a large catalog of audience segments that are easy-to-use and ready-to-go. Amazon audiences is a way to engage new customers as well as to help advertisers better learn about their brand on Amazon. |
| View and purchase remarketing | PRODUCT\_CATEGORY\_AUDIENCE | Based on view-to-purchase data (shoppers viewing or purchasing these products are likely to purchase your advertised product). |
| Entertainment targeting | CONTENT\_CATEGORY | Helps advertisers reach engaged audience during their entertainment journey |

## Scenario 1: Search all targetable entities by keyword

Use [POST /targetableEntities/list](targetable-entities#tag/TargetableEntities/operation/ListTargetableEntities) to search for targetable entities of all types that relate to keyword `auto`.

>[NOTE] An empty list of targetType will also search all targetable entities.

**Sample request**

```bash
curl --location 'https://advertising-api.amazon.com/targetableEntities/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
--header 'Content-Type: application/json' \
--data '{
    "adProduct": "SPONSORED_DISPLAY",
    "searchQueryFilter": "auto care",
    "targetTypeFilter": [
        "PRODUCT_CATEGORY",
        "PRODUCT_CATEGORY_AUDIENCE",
        "AUDIENCE",
        "CONTENT_CATEGORY"
    ],
    "pathsFilter": [
        ["Audience Category"],
        ["Product Category"],
        ["Product Category Audience"],
        ["Content Category"]
    ],
    "maxResults": 2
}'
```

**Sample response**

```json
{
    "targetableEntities": [
        {
            "path": ["Product Category", "Automotive", "Car Care"],
            "targetType": "PRODUCT_CATEGORY",
            "id": "15718271",
            "resolved": "Car Care"
        },
        {
            "path": ["Audience Category", "In-market", "Vehicles & Automotive", "Automotive Parts & Accessories", "Automotive Care", "Exterior Care"],
            "targetType": "AUDIENCE",
            "id": "403097064506351917",
            "resolved": "IM - Automotive Exterior Care"
        },
        {
            "path": ["Product Category Audience", "Automotive", "Car Care", "Tire & Wheel Care Products"],
            "targetType": "PRODUCT_CATEGORY_AUDIENCE",
            "id": "2687787011",
            "resolved": "Automotive Wheel Care"
        },
        {
            "path": ["Content Category", "Movies & TV"],
            "targetType": "CONTENT_CATEGORY",
            "id": "amzn1.iab-content.156",
            "resolved": "Musicals"
        }
    ],
    "nextToken": "INnwcQ/tkhkcsFSN1WgHB0I38apf56/URcJPfW723r1DzbXh9amcK836J8K3jxqu4S4=",
    "totalResults": 5
}
```

## Scenario 2: Search all targetable entities by keyword

Use [POST /targetableEntities/list](targetable-entities#tag/TargetableEntities/operation/ListTargetableEntities) to search for targetable entities of all types that relate to keyword `auto`.

>[NOTE] An empty list of  pathsFilter will return a curated list of target entities (excludes in-market audiences only for SPONSORED_DISPLAY AdProduct).

**Sample request**

```bash
curl --location 'https://advertising-api.amazon.com/targetableEntities/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
--header 'Content-Type: application/json' \
--data '{
    "adProduct": "SPONSORED_DISPLAY",
    "searchQueryFilter": "auto care",
    "targetTypeFilter": [
        "PRODUCT_CATEGORY",
        "PRODUCT_CATEGORY_AUDIENCE",
        "AUDIENCE",
        "CONTENT_CATEGORY"
    ],
    "pathsFilter": [],
    "maxResults": 2
}'
```

**Sample response**

```json
{
    "targetableEntities": [
        {
            "path": ["Product Category", "Automotive", "Car Care"],
            "targetType": "PRODUCT_CATEGORY",
            "id": "15718271",
            "resolved": "Car Care"
        },
        {
            "path": ["Audience Category", "In-market", "Vehicles & Automotive", "Automotive Parts & Accessories", "Automotive Care", "Exterior Care"],
            "targetType": "AUDIENCE",
            "id": "403097064506351917",
            "resolved": "IM - Automotive Exterior Care"
        },
        {
            "path": ["Product Category Audience", "Automotive", "Car Care", "Tire & Wheel Care Products"],
            "targetType": "PRODUCT_CATEGORY_AUDIENCE",
            "id": "2687787011",
            "resolved": "Automotive Wheel Care"
        }
    ],
    "nextToken": "INnwcQ/tkhkcsFSN1WgHB0I38apf56/URcJPfW723r1DzbXh9amcK836J8K3jxqu4S4=",
    "totalResults": 5
}
```

## Scenario 3: Search only contextual targets using a keyword

Use [POST /targetableEntities/list](targetable-entities#tag/TargetableEntities/operation/ListTargetableEntities) to search for contextual targets that relate to keyword `auto`.

**Sample request**

```bash
curl --location 'https://advertising-api.amazon.com/targetableEntities/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxx' \
--header 'Content-Type: application/json' \
--data '{
    "adProduct": "SPONSORED_DISPLAY",
    "searchQueryFilter": "auto care",
    "targetTypeFilter": [
        "PRODUCT_CATEGORY"
    ],
    "pathsFilter": [],
    "maxResults": 2
}'
```

**Sample response**

```json
{
    "targetableEntities": [
        {
            "path": ["Product Category", "Automotive", "Car Care"],
            "targetType": "PRODUCT_CATEGORY",
            "id": "15718271",
            "resolved": "Car Care"
        },
        {
            "path": ["Product Category", "Automotive", "Car Care", "Tire & Wheel Care Products", "Automotive Wheel Care"],
            "targetType": "PRODUCT_CATEGORY",
            "id": "2687787011",
            "resolved": "Automotive Wheel Care"
        }
    ],
    "nextToken": "INnwcQ/tkhkcsFSN1WgHB0I38apf56/URcJPfW723r1DzbXh9amcK836J8K3jxqu4S4=",
    "totalResults": 5
}
```

## Scenario 4: Search only view/purchase remarketing targets using a keyword

Use [POST /targetableEntities/list](targetable-entities#tag/TargetableEntities/operation/ListTargetableEntities) to search for view/purchase targets that relate to keyword `auto`.

**Sample request**

```bash
curl --location 'https://advertising-api.amazon.com/targetableEntities/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxx' \
--header 'Content-Type: application/json' \
--data '{
    "adProduct": "SPONSORED_DISPLAY",
    "searchQueryFilter": "auto care",
    "targetTypeFilter": [
        "PRODUCT_CATEGORY_AUDIENCE"
    ],
    "pathFilter": [],
    "maxResults": 2
}'
```

**Sample response**

```json
{
    "targetableEntities": [
        {
            "path": ["Product Category Audience", "Automotive", "Car Care"],
            "targetType": "PRODUCT_CATEGORY_AUDIENCE",
            "id": "15718271",
            "resolved": "Car Care"
        },
        {
            "path": ["Product Category Audience", "Electronics", "Vehicle Electronics", "Car Electronics Accessories", "Car Audio & Video Accessories"],
            "targetType": "PRODUCT_CATEGORY_AUDIENCE",
            "id": "10981681",
            "resolved": "Car Audio & Video Accessories"
        },    
    ],
    "nextToken": "INnwcQ/tkhkcsFSN1WgHB0I38apf56/URcJPfW723r1DzbXh9amcK836J8K3jxqu4S4=",
    "totalResults": 5
}
```

# Discover Targetable Entity Paths

Use [POST /targetableEntities/paths/list](targetable-entities#tag/TargetableEntityPaths/operation/ListTargetableEntityPaths) to get all entity paths.

## Scenario 1: Identify direct children of a path 

**Sample request**

```bash
curl --location 'https://advertising-api.amazon.com/targetableEntities/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxx' \
--header 'Content-Type: application/json' \
--data '{
    "pathsFilter": [
        [
            "Audience Category",
            "In-market",
            "Baby Products"
        ]
    ],
    "adProduct": "SPONSORED_DISPLAY"
}'
```

**Sample response**

```json
{
    "paths": [
        [
            "Audience Category",
            "In-market",
            "Baby Products",
            "Pacifiers, Teethers & Teething Relief"
        ],
        [
            "Audience Category",
            "In-market",
            "Baby Products",
            "Handmade Baby Products"
        ],
        [
            "Audience Category",
            "In-market",
            "Baby Products",
            "Nursing & Feeding"
        ],
        ...
        ...
    ],
    "totalResults": 11
}
```

## Scenario 2: Identify top level paths for Contextual Targeting

```bash
curl --location 'https://advertising-api.amazon.com/targetableEntities/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxx' \
--header 'Content-Type: application/json' \
--data '{
    "pathsFilter": [
        ["Product Category"]
    ],
    "adProduct": "SPONSORED_DISPLAY"
}'
```

```json
{
    "paths": [
        [
            "Product Category",
            "Gift Cards"
        ],
        [
            "Product Category",
            "Collectibles & Fine Art"
        ],
        [
            "Product Category",
            "Clothing, Shoes & Jewelry"
        ],
        [
            "Product Category",
            "Amazon Devices & Accessories"
        ],
        ...
        ...
    ],
    "totalResults": 36
}
```