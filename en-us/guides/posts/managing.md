---
title: Get started with Posts
description: Learn how developers can get started with Posts using the Amazon Ads API.  
type: guide
interface: api
---

# Get started with Posts

>[WARNING]The Posts API is discontinued as of June 3, 2025. New users will no longer be able to make requests to the Posts API; existing Posts users should expect new Posts creation to be discontinued after June 16, 2025, and a full shut-off date of July 31, 2025.

## Overview

Amazon Posts enable advertisers to nurture new customer connections, keep their brand top of mind, and grow its following through the use of engaging product imagery and effective content.

[Learn more about Posts](https://advertising.amazon.com/solutions/products/posts).

A Post consists of following details:

- Image asset
- Caption 
- Up to five products

## Before you begin

Complete the Amazon Ads API [Onboarding](guides/onboarding/overview) and [Getting started](guides/get-started/overview) processes to obtain your access token and profile ID. You will need these to make all the calls referenced in the tutorial.

## Creating Posts

### Step 1: Identify the brand profile ID

To create a Post, you need to identify the brand profile ID using [GET /bp/v2/profiles](posts#tag/Posts/operation/ListProfiles) endpoint.

**Sample request**

```bash
curl "https://advertising-api.amazon.com/bp/v2/profiles" \
-X GET \
-H "Authorization: Bearer Atza|xxxxxxxxxx" \
-H "Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx" \
-H "Amazon-Advertising-API-Scope: xxxxxxxxxx"
```

**Sample response**

```json
{
  "profiles": [
    {
      "postProfileId": "5728881f-9d91-499a-a39a-xxxxxxxxxx",
      "name": "My brand",
      "logoUrl": "https://images-na.ssl-images-amazon.com/images/S/mms-media-storage-prod/final/BrandPosts/brandPosts/xxxxxxxxxxx.jpeg",
      "status": "ACTIVE",
      "browseNodeId": "1111111111",
      "isAuthorized": true
    }
  ]
}
```

You will use the `postProfileId` in Step 3.

### Step 2: Create an asset for use in Posts

Posts require all assets to be uploaded using the [Creative asset library API](creative-asset-library).

The general steps to upload an asset are:

1. Generate an asset upload URL.
2. Upload your asset to the provided URL. 
3. Register your asset.

For a full tutorial on uploading assets, see [Get started with asset library](guides/creative-asset/creating-assets).

### Step 3: Create a Post

Use the [POST /bp/v2/posts](posts#tag/Posts/operation/CreatePost) endpoint to create a Post.

For the request, use the `postProfileId` retrieved in step 1 as the `profileId` in the request body. You can get `mediaUrl` and `mediaId` from asset library (step 2).

**Sample request**

```bash
curl "https://advertising-api.amazon.com/bp/v2/posts" 
    -X POST -H "Content-Type: application/json;charset=utf-8" 
    -H "Authorization: Bearer Atza|xxxxxxxxxx" 
    -H "Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx" 
    -H "Amazon-Advertising-API-Scope: xxxxxxxxxx" 
    -H "Content-Length: 314" 
    -d "{\"medias\":[{\"mediaUrl\":\"https://m.media-amazon.com/images/S/al-na-184b9306-7f8a/1e0e4406-9103-4def-8bae-9a4f8287bb74._SL853_FMjpeg_.jpg\",\"mediaId\":\"amzn1.assetlibrary.asset1.ce408acef4a32b8e24060e83e87d0379\"}],\"profileId\":\"5728881f-9d91-499a-a39a-41ef85f54acc\",\"caption\":\"Test Caption..\",\"products\":[\"B0C3D79V1C\"]}"
```

**Sample response**

```json
{
  "post": {
    "id": "d9da33ba-dfb7-4db1-85d8-xxxxxxxxx",
    "profileId": "5728881f-9d91-499a-a39a-xxxxxxxx",
    "caption": "Test Caption..",
    "medias": [
      {
        "mediaId": "amzn1.assetlibrary.asset1.xxxxxxxxx",
        "mediaUrl": "https://m.media-amazon.com/images/S/alxxxxxxxxxx_.jpg"
      }
    ],
    "products": [
      "B0111111"
    ],
    "version": 1,
    "status": "DRAFT",
    "isFlaggedForQuality": false,
    "createdDate": "2023-09-20T17:01:23.170Z",
    "lastModified": "2023-09-20T17:01:23.201Z"
  }
}
```

### Step 4: Publish a Post

Use the [PUT /bp/v2/posts/{postId}/submitForReview](posts#tag/Posts/operation/SubmitPostForReview) endpoint to submit post for moderation.

**Sample request**

```bash
curl "https://advertising-api.amazon.com/bp/v2/posts/d9da33ba-dfb7-4db1-85d8-15a7dbe34405/submitForReview" \
-X PUT \
-H "Content-Type: application/json;charset=utf-8" \ 
-H "Authorization: Bearer Atza|xxxxxxxxxx" \
-H "Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx" \
-H "Amazon-Advertising-API-Scope: xxxxxxxxxx" \
-H "Content-Length: 13" -d "{\"version\":1}" \
```

**Sample response**

```json
{
  "post": {
    "id": "d9da33ba-dfb7-4db1-85d8-xxxxxxxxx",
    "profileId": "5728881f-9d91-499a-a39a-xxxxxxxx",
    "caption": "Test Caption..",
    "medias": [
      {
        "mediaId": "amzn1.assetlibrary.asset1.xxxxxxxxx",
        "mediaUrl": "https://m.media-amazon.com/images/S/alxxxxxxxxxx_.jpg"
      }
    ],
    "products": [
      "B0111111"
    ],
    "version": 1,
    "status": "REVIEW",
    "isFlaggedForQuality": false,
    "createdDate": "2023-09-20T17:01:23.170Z",
    "lastModified": "2023-09-20T17:01:23.201Z"
  }
}
```

You will see the `status` change to `REVIEW`. When the Post has been approved, the status will change to `LIVE`. 

## Withdrawing Posts

Once a Post is live, you can take it down using the [PUT /bp/v2/posts/{postId}/unpublish](posts#tag/Posts/operation/WithdrawPost) endpoint.  

**Sample request**

```bash
curl "https://advertising-api.amazon.com/bp/v2/posts/d9da33ba-dfb7-4db1-85d8-xxxxxxx/unpublish" \
-X PUT \
-H "Content-Type: application/json;charset=utf-8" \
-H "Authorization: Bearer Atza|xxxxxxxxxx" \
-H "Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx" \
-H "Amazon-Advertising-API-Scope: xxxxxxxxx" \
-H "Content-Length: 13" \ 
-d "{\"version\":3}"
```

**Sample response**

```json
{
  "post": {
    "id": "d9da33ba-dfb7-4db1-85d8-xxxxxxxxx",
    "profileId": "5728881f-9d91-499a-a39a-xxxxxxxx",
    "caption": "Test Caption..",
    "medias": [
      {
        "mediaId": "amzn1.assetlibrary.asset1.xxxxxxxxx",
        "mediaUrl": "https://m.media-amazon.com/images/S/alxxxxxxxxxx_.jpg"
      }
    ],
    "products": [
      "B0111111"
    ],
    "version": 1,
    "status": "WITHDRAWN",
    "isFlaggedForQuality": false,
    "createdDate": "2023-09-20T17:01:23.170Z",
    "lastModified": "2023-09-20T17:01:23.201Z"
  }
}
```

## Retrieving Posts

You can get details for a specific Post using [GET /bp/v2/posts/{postId}](posts#tag/Posts/operation/GetPost).

**Sample request**

```bash
curl "https://advertising-api.amazon.com/bp/v2/posts/586c4dd9-f350-45ca-b483-xxxxxxxxx" \
-X GET \
-H "Authorization: Bearer Atza|xxxxxxxxxx" \
-H "Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxx" \
-H "Amazon-Advertising-API-Scope: xxxxxxxxxx"
```

**Sample response**

```json
{
  "post": {
    "id": "d9da33ba-dfb7-4db1-85d8-xxxxxxxxx",
    "profileId": "5728881f-9d91-499a-a39a-xxxxxxxx",
    "caption": "Test Caption..",
    "medias": [
      {
        "mediaId": "amzn1.assetlibrary.asset1.xxxxxxxxx",
        "mediaUrl": "https://m.media-amazon.com/images/S/alxxxxxxxxxx_.jpg"
      }
    ],
    "products": [
      "B0111111"
    ],
    "version": 1,
    "status": "REVIEW",
    "isFlaggedForQuality": false,
    "createdDate": "2023-09-20T17:01:23.170Z",
    "lastModified": "2023-09-20T17:01:23.201Z"
  }
}
```

## Learn more

- [Reporting on Posts](guides/posts/reporting)