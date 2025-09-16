---
title: Sponsored Display locations guides
description: Understand how to add locations to a Sponsored Display campaign.
type: guide
interface: api 
tags:
    - Sponsored Display
---

# Sponsored Display locations

>[WARNING] Sponsored Display locations are only available for advertisers that do not sell on Amazon.

>[TIP] Try it in Postman. You can test out the locations APIs in our Postman collection for advertisers that don't sell on Amazon. Visit [GitHub]() to dowload the collection and environment file. 

Locations allow non-Amazon sellers to serve ads to users in a certain region. Location targeting is optional.

## Before you begin

Follow the [steps](guides/sponsored-display/get-started) to create a Sponsored Display campaign for advertisers that don't sell on Amazon.

## Exploring available locations

Advertisers can use the [POST /locations/list API](locations) to search for available locations and retrieve a location ID.

### Example: Filtering by state

Request:

```bash
curl --location 'https://advertising-api.amazon.com/locations/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxx' \
--data '{
 "filters": [
        {
            "field": "name",
            "values": [
                "New Yor"
            ]
        },
        {
            "field": "category",
            "values": [
                "STATE"
            ]
        }
    ]
}'
```
Response:

```json
{
  "locations": [
    {
      "id": "amzn1.ad-geo.G23XLKJV9KV95N",
      "name": "New York, US",
      "category": "STATE"
    }
  ],
  "nextToken": null
}
```


### Example: Filtering by ZIP code

Request:

```
curl --location 'https://advertising-api.amazon.com/locations/list' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Content-Type: application/json'  \
--data '{
    "filters": [
        {
            "field": "name",
            "values": [
                "9800"
            ]
        },
        {
            "field": "category",
            "values": [
                "POSTAL_CODE"
            ]
        }
    ]
}'
```

Response:

```json
{
  "locations": [
    {
      "id": "amzn1.ad-geo.GP-US-98001",
      "name": "Auburn, Washington, US - 98001",
      "category": "POSTAL_CODE"
    },
    {
      "id": "amzn1.ad-geo.GP-US-98002",
      "name": "Auburn, Washington, US - 98002",
      "category": "POSTAL_CODE"
    },
    {
      "id": "amzn1.ad-geo.GP-US-98003",
      "name": "Federal Way, Washington, US - 98003",
      "category": "POSTAL_CODE"
    },
    {
      "id": "amzn1.ad-geo.GP-US-98004",
      "name": "Bellevue, Washington, US - 98004",
      "category": "POSTAL_CODE"
    },
    {
      "id": "amzn1.ad-geo.GP-US-98005",
      "name": "Bellevue, Washington, US - 98005",
      "category": "POSTAL_CODE"
    }
  ],
  "nextToken": "INnwcQ/tkhkcsFSN1WgHB0I38apf56/URcJPfW723r1DzbXh9amcK836J8K3jxqu4S4="
}
```

Take note of the `locations.id` for use when adding the location to your campaign. 

## Adding locations to an ad group 

Use [POST /sd/locations](sponsored-display/3-0/openapi#tag/Locations/operation/createLocations) to add locations to your campaign. The type of expression should be set to `location` while the `value` should be the location ID. 

```bash
curl --location 'https://advertising-api.amazon.com/sd/locations' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Content-Type: application/json' \
--data '[
    {
        "adGroupId": 413803530857698,
        "expressionType": "manual",
        "state": "enabled",
        "expression": [
            {
                "type": "location",
                "value": "amzn1.ad-geo.GP-US-10005"
            }
        ]
    }
]'
```

```json
[
    {
        "locationExpressionId": 201992653281089,
        "adGroupId": 413803530857698,
        "state": "enabled",
        "expression": [
            {
                "type": "location",
                "value": "amzn1.ad-geo.GP-US-10005"
            }
        ],
        "resolvedExpression": [
            {
                "type": "location",
                "value": "New York, New York, US - 10005"
            }
        ]        
    }
]
```

## Retrieving locations

To see the locations associated with a specific campaign or ad group, you can call [GET/sd/locations](sponsored-display/3-0/openapi#tag/Locations/operation/listLocations) using the `campaignIdFilter` or `adGroupIdFilter`. 

```GET /sd/locations?campaignIdFilter=1234567890```

or

```GET /sd/locations?adGroupIdFilter=59841916872307```

A sample response with two location expressions associated might resemble:

```json
[
    {
        "locationExpressionId": 29590100316373,
        "adGroupId": 59841916872307,
        "state": "enabled",
        "expression": [
            {
                "type": "location",
                "value": "amzn1.ad-geo.GP-US-10005"
            }
        ],
        "resolvedExpression": [
            {
                "type": "location",
                "value": "New York, New York, US - 10005"
            }
        ]        
    },
    {
        "locationExpressionId": 18214502085837,
        "adGroupId": 59841916872307,
        "state": "enabled",
        "expression": [
            {
                "type": "location",
                "value": "amzn1.ad-geo.GP-US-10006"
            }
        ],
        "resolvedExpression": [
            {
                "type": "location",
                "value": "New York, New York, US - 10006"
            }
        ]        
    }    
]
```

## Deleting locations

To delete locations associated with a specific campaign or ad group, you can call the [POST/sd/locations/delete](sponsored-display/3-0/openapi#tag/Locations-(beta\)/operation/archiveLocations) using the `locationExpressionIdFilter`. 

```bash
curl --location 'https://advertising-api.amazon.com/sd/locations/delete' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Content-Type: application/json' \
--data '[
    {
        "locationExpressionIdFilter": {
            "include": [
                301324301169379,
                301324301169380
      ]
    }
]'
```

A sample response to archive the above two location expression ids might resemble:

```json
[
   {
      "code": "SUCCESS",
      "locationExpressionId": 301324301169379
   },
   {
      "code": "SUCCESS",
      "locationExpressionId": 301324301169380
   }
]
```