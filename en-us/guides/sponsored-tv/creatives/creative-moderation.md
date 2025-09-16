---
title: Sponsored TV creative moderation
description: Details on how to check creative moderation.
type: guide
interface: api
keywords:
    - Sponsored TV
    - Creative moderation
---

# Sponsored TV creative moderation

Your video asset will go through a moderation process, which includes technical specification and video quality checks. This document explains how to use the creative moderation endpoints to check moderation status and get more information if a video asset is rejected. 


## Check moderation status

To check if your asset has been `REJECTED` for moderation, you can use the [/st/creatives/moderations/list](sponsored-tv-open-beta#tag/CreativesModerations/operation/ListSponsoredTvCreativesModerations) endpoint.

```
curl --location 'https://advertising-api.amazon.com/st/creatives/moderations/list' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data '{
    "creativeIdFilter": {
        "include": [
            "12345678901234567890"
        ]
    }
}'

```

In the response, you can see the moderation status as `REJECTED` and the video URL source.


```
{
    "creativesModerations": [
        {
            "creativeId": "12345678901234567890",
            "etaForModeration": "",
            "moderationStatus": "REJECTED",
            "reviewedVideoUrl": "https://m.media-amazon.com/images/S/al-na-9d5791cf-2345/972f3cc3-3fb1-4993-8781-565b124b9850.mp4"
        }
    ]
}
```

## Get more information regarding rejection status

To get more information as to why your asset is `REJECTED`, you can use the [/st/creatives/moderations/policyViolations/list](sponsored-tv-open-beta#tag/CreativesModerations/operation/ListSponsoredTvCreativesModerationsPolicyViolations) endpoint. This endpoint returns a policy description citing what violations the creative asset has and what needs to be modified. 

```json
POST /st/creatives/moderations/policyViolations/list HTTP /1.1

curl --location 'https://advertising-api.amazon.com/st/creatives/moderations/policyViolations/list' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxxxx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxxxx' \
--data '{
    "creativeIdFilter": {
        "include": [
            "12345678901234567890"
        ]
    }
}'
```

```json
{
    "policyViolations": [
        {
            "policyDescription": "Your ad does not meet required specifications or comply with policies. This may include poor audio quality, such as: static, crackling, faint messages or low tone voices, fast paced, inaudible, unclear, or unrecognizable sounds. Please update your video.",
            "policyLinkUrl": "https://advertising.amazon.com/en-us/resources/ad-policy/sponsored-ads-policies#prohibitedcontent",
            "videoEvidences": [
                {
                    "end": 30,
                    "start": 0
                }
            ]
        }
    ]
}
```
