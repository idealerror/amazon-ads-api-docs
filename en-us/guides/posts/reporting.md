---
title: Reporting on Posts
description: Learn how developers can get started with Posts reporting using the Amazon Ads API.  
type: guide
interface: api
---
# Reporting on Posts

>[WARNING]The Posts API is discontinued as of June 3, 2025. New users will no longer be able to make requests to the Posts API; existing Posts users should expect new Posts creation to be discontinued after June 16, 2025, and a full shut-off date of July 31, 2025.

The Posts API reporting functionality provides a variety of reports to help you retrieve historical impressions, clicks, and views for your Posts.

To learn more about creating and managing Posts, see [Get started with Posts](guides/posts/managing).

## Before you begin

Complete the Amazon Ads API [Onboarding](guides/onboarding/overview) and [Getting started](guides/get-started/overview) processes to obtain your access token and profile ID. You will need these to make all the calls referenced in the tutorial.

## Brand-level reporting

Use the [POST /bp/v2/profiles/{profileId}/metrics](posts#tag/Posts/operation/GetProfileMetrics) endpoint to get analytics at the brand or post profile level. Use your `postProfileId` as the `profileId` in the path. 

**Sample request**

```bash
curl "https://advertising-api.amazon.com/bp/v2/profiles/5728881f-9d91-499a-a39a-xxxxxxxxx/metrics" \
-X POST \
-H "Content-Type: application/json;charset=utf-8" \
-H "Authorization: Bearer Atza|xxxxxxxxxx" \
-H "Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx" \
-H "Amazon-Advertising-API-Scope: xxxxxxxxxx" \
-H "Content-Length: 2" \
-d "{}"
```

**Sample response**

```json
{
  "aggregateType": "DAY",
  "metrics": [
    {
      "date": "2023-08-22T00:00:00Z",
      "reach": 0,
      "engagements": 0,
      "impressions": 0,
      "clicksToDetailPage": 0
    },
    {
      "date": "2023-08-23T00:00:00Z",
      "reach": 0,
      "engagements": 0,
      "impressions": 0,
      "clicksToDetailPage": 0
    },
    {
      "date": "2023-08-24T00:00:00Z",
      "reach": 0,
      "engagements": 0,
      "impressions": 0,
      "clicksToDetailPage": 0
    },
    {
      "date": "2023-08-25T00:00:00Z",
      "reach": 0,
      "engagements": 0,
      "impressions": 0,
      "clicksToDetailPage": 0
    }
  ],
  "aggregateMetrics": {
    "engagements": 0,
    "impressions": 0,
    "clicksToDetailPage": 0
  }
}
```

## Post-level reporting

Use the [POST /bp/v2/posts/list](posts#tag/Posts/operation/ListPosts) endpoint to get analytics at the individual post level. 

Note that `metricsStartDate` and `metricsEndDate` can’t be more than a year apart. 

**Sample request**

```bash
curl "https://advertising-api.amazon.com/bp/v2/posts/list" \
-X POST -H "Content-Type: application/json;charset=utf-8" \
-H "Authorization: Bearer Atza|xxxxxxxxxx" \
-H "Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx" \
-H "Amazon-Advertising-API-Scope: xxxxxxxxxx" \
-H "Content-Length: 224" \
-d "{\"selectedMetrics\":[\"impressions\"],\"maxResults\":50,\"metricEndDate\":\"2023-09-28\",\"profileId\":\"5728881f-9d91-499a-a39a-xxxxxxxxx\",\"sortCriterion\":{\"sortOrder\":\"ASC\",\"sortField\":\"createdDate\"},\"metricStartDate\":\"2022-10-28\"}"
```

**Sample response**

```json
{
  "nextToken": "50",
  "totalPosts": 76,
  "isNumPostsOverLimit": false,
  "posts": [
    {
      "id": "98687ba4-09fb-44d9-988f-xxxxxxx",
      "caption": "Test content 1. \n",
      "mediaUrl": "https://images-na.ssl-images-amazon.com/images/S/xxx.jpeg",
      "products": [
        "B1111111"
      ],
      "status": "WITHDRAWN",
      "isFlaggedForQuality": false,
      "metrics": {
        "impressions": 0
      },
      "createdDate": "2019-03-11T18:11:00.059Z"
    },
    {
      "id": "e24cb84a-d916-4be6-b130-xxxxxxx",
      "caption": "Test content 2.\n",
      "mediaUrl": "https://images-na.ssl-images-amazon.com/images/S/xxx.jpeg",
      "products": [
        "B00000000"
      ],
      "status": "LIVE",
      "isFlaggedForQuality": false,
      "metrics": {
        "impressions": 6
      },
      "createdDate": "2019-03-11T18:12:49.083Z"
    }
  ]
}
```