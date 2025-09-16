---
title: How to build Sponsored Display entertainment targets 
description: Learn how to build and use Sponsored Display entertainment targets
type: guide
interface: api 
tags:
    - Sponsored Display
keywords:
    - targets
---

# Get started with Sponsored Display entertainment targeting

Sponsored Display entertainment targeting provides all advertisers an easier and streamlined way of reaching audiences in entertainment mindset (i.e., people who are either actively engaged in entertainment or have shown interest in it). Advertisers can now reach and influence audiences with genres of entertainment - customers listening to classical music, playing racing games, watching a horror movie, streaming esports, or reading reviews on their favorite blogging sites, now or in the past. Self-service advertisers can now use Amazon Ads to reach audiences in their entertainment journey across Twitch, IMDb, Amazon, and third-party publishers. 

In order to facilitate entertainment targeting in Sponsored Display, the [/sd/targets](sponsored-display/3-0/openapi#tag/Targeting) resource has been extended with the ability to create entertainment targeting clauses.

## Before you begin

Make sure you have created a Sponsored Display campaign, ad group, and product ad. Note that you can create entertainment targets for either contextual (T00020) or audience (T00030) campaigns.

## Creating entertainment targets

You can use [POST /sd/targets](sponsored-display/3-0/openapi#tag/Targeting/operation/createTargetingClauses) to create an entertainment target.

**Sample request**

```bash
curl --location 'https://advertising-api.amazon.com/sd/targets' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx' \
--header 'Content-Type: application/json' \
--data '[
    {
        "adGroupId": 1111111111,
        "expressionType": "manual",
        "state": "paused",
        "bid": 3.50,
        "expression": [
            {
                "type": "contentCategorySameAs",
                "value": "amzn1.iab-content.156"
            }
        ]
    }
]'
```

**Sample response**

```json
[
    {
        "code": "SUCCESS",
        "targetId": 11111222222
    }
]
```

## Retrieving entertainment targets

To retrieve entertainment targets, you can use the [GET /sd/targets/{targetId}](sponsored-display/3-0/openapi#tag/Targeting/operation/getTargets) endpoint.

**Sample request**

```bash
curl --location 'https://advertising-api.amazon.com/sd/targets/11111222222' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxxxxxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxxxx'
```

**Sample response**

```json
{
    "adGroupId": 1111111111,
    "bid": 3.5,
    "expression": [
        {
            "type": "contentCategorySameAs",
            "value": "amzn1.iab-content.156"
        }
    ],
    "expressionType": "manual",
    "resolvedExpression": [
        {
            "type": "contentCategorySameAs",
            "value": "Musicals"
        }
    ],
    "state": "paused",
    "targetId": 11111222222,
    "campaignId": 3333333333
}
```

## Discovering entertainment target categories

>[TIP] You can also use the [targetable entities API](guides/sponsored-display/targetable-entities) to discover available content categories to target.

For entertainment targeting, a new predicate has been added called contentCategorySameAs which takes in an IAB category as the value. The complete list of allowed IAB categories can be found in the table below.

|Category	|Subcategory	|Value	|
|-------|--------|--------|
|Movies and Television	|All Movies and Television	|amzn1.iab-content.SPSHQ5	|
|Movies and Television	|Action or Adventure	|amzn1.iab-content.325	|
|Movies and Television	|Animation or Anime	|amzn1.iab-content.641	|
|Movies and Television	|Biographies	|amzn1.iab-content.44	|
|Movies and Television	|Comedy	|amzn1.iab-content.646	|
|Movies and Television	|Documentary	|amzn1.iab-content.332	|
|Movies and Television	|Drama	|amzn1.iab-content.647	|
|Movies and Television	|Factual	|amzn1.iab-content.648	|
|Movies and Television	|Family	|amzn1.iab-content.645	|
|Movies and Television	|Fantasy	|amzn1.iab-content.335	|
|Movies and Television	|History	|amzn1.iab-content.EZWB7V	|
|Movies and Television	|Holiday	|amzn1.iab-content.649	|
|Movies and Television	|Horror	|amzn1.iab-content.336	|
|Movies and Television	|Lifestyle	|amzn1.iab-content.TIFQA5	|
|Movies and Television	|Music Video	|amzn1.iab-content.650	|
|Movies and Television	|Musicals	|amzn1.iab-content.156	|
|Movies and Television	|Mystery	|amzn1.iab-content.331	|
|Movies and Television	|Reality TV	|amzn1.iab-content.651	|
|Movies and Television	|Romance	|amzn1.iab-content.326	|
|Movies and Television	|Science Fiction	|amzn1.iab-content.652	|
|Movies and Television	|Soap Opera	|amzn1.iab-content.642	|
|Movies and Television	|Special Interest (Indie or Art House)	|amzn1.iab-content.643	|
|Movies and Television	|Sports Radio	|amzn1.iab-content.370	|
|Movies and Television	|Talk Show	|amzn1.iab-content.A0AH3G	|
|Movies and Television	|True Crime	|amzn1.iab-content.KHPC5A	|
|Movies and Television	|Western	|amzn1.iab-content.KHPC6A	|
|Music and Radio	|All Music and Radio	|amzn1.iab-content.338	|
|Music and Radio	|Blues	|amzn1.iab-content.360	|
|Music and Radio	|Classical Music	|amzn1.iab-content.346	|
|Music and Radio	|Comedy (Music and Audio)	|amzn1.iab-content.348	|
|Music and Radio	|Pop, Contemporary Hits, or Top 40 Music	|amzn1.iab-content.349	|
|Music and Radio	|Country Music	|amzn1.iab-content.350	|
|Music and Radio	|Dance and Electronic Music	|amzn1.iab-content.351	|
|Music and Radio	|Hip Hop Music	|amzn1.iab-content.355	|
|Music and Radio	|Inspirational or New Age Music	|amzn1.iab-content.356	|
|Music and Radio	|Jazz	|amzn1.iab-content.357	|
|Music and Radio	|Oldies or Adult Standards	|amzn1.iab-content.358	|
|Music and Radio	|R&B, Soul or Funk Music	|amzn1.iab-content.362	|
|Music and Radio	|Reggae	|amzn1.iab-content.359	|
|Music and Radio	|Rock Music	|amzn1.iab-content.363	|
|Music and Radio	|Songwriters or Folk	|amzn1.iab-content.353	|
|Music and Radio	|World or International Music	|amzn1.iab-content.352	|
|Video Games	|All Video Games	|amzn1.iab-content.680	|
|Video Games	|Action-Adventure Games	|amzn1.iab-content.691	|
|Video Games	|Casual Games	|amzn1.iab-content.693	|
|Video Games	|Puzzle Video Games	|amzn1.iab-content.698	|
|Video Games	|Racing Video Games	|amzn1.iab-content.VK7KD0	|
|Video Games	|Role-Playing Video Games	|amzn1.iab-content.687	|
|Video Games	|Simulation Video Games	|amzn1.iab-content.688	|
|Video Games	|Sports Video Games	|amzn1.iab-content.689	|
|Video Games	|Strategy Video Games	|amzn1.iab-content.690	|
|Video Games	|PC Games	|amzn1.iab-content.684	|
|Video Games	|Mobile Games	|amzn1.iab-content.683	|
|Video Games	|Console Games	|amzn1.iab-content.681	|
|Video Games	|eSports	|amzn1.iab-content.682	|

## FAQ

### What are the benefits of using entertainment targeting?

Using entertainment audiences may help with reaching an engaged audience in their entertainment journey as they are streaming or playing media or researching their favorite games and movies on blog sites or leaving reviews on their experiences. This helps you to expand your reach beyond shopping audiences. Additionally, you can ensure your brand’s ads show up next to content category/genre that aligns with your ads messaging. Note that we always strive to ensure brand safety, avoiding content that is considered to be inappropriate for advertising.  

### How are these entertainment audiences different from current entertainment segments in Amazon Audiences?

Entertainment audiences have a wider reach allowing you to connect to a larger set of customers. Current entertainment segments allow you to reach audiences who have viewed such content in the past. The new entertainment audiences allow you to additionally reach audiences currently viewing entertainment content. These new audiences also reach a broader audiences across all Amazon premiere entertainment properties like Twitch, IMDB, and Prime Video as against the current entertainment segments that are specific to past content views on individual properties. 

### If I use entertainment targeting, where will my ads show up?

Your ads could appear on Amazon’s owned and operated entertainment publishers, such as Twitch & IMDb, Amazon.com, and reputable third-party publishers including those in the entertainment space that are available through Amazon Publisher Services (APS) and third-party exchanges.  

### Why are sites like Twitch so important for advertisers?

For more information, see our complete guide on [Sponsored Display on Twitch](https://advertising.amazon.com/library/guides/guide-to-sponsored-display-on-twitch).