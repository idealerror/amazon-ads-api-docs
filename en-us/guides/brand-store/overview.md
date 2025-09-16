---
title: Brand Store overview
description: Learn how to create a Brand Store and how to obtain brand registry ID. 
type: guide
interface: api 
tags:
    - Brand Stores
keywords:
    - brand
    - brand store
    - brand store metrics
---

# Brand Stores overview

Brand Stores are customizable storefronts that allow brands to showcase their products and tell their brand story in a more curated and branded environment.


## Before you begin

* Must have your [brand registered](https://brandservices.amazon.com/brandregistry)
* Must have an existing [Store](https://advertising.amazon.com/solutions/products/stores?ref_=a20m_us_faq_st)



## Retrieve Brand Store information

This endpoint will retrieve Brand Store information for all registered stores under an advertiser. The `brandEntityId` is required in order to use the Brand stores metrics APIs. 

Endpoint: `/v2/stores` 

Request:


```
curl --location --request GET 'https://advertising-api.amazon.com/v2/stores' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Amazon-Advertising-API-Scope: xxxxxx' \
```

Response: 



```
[{
    "code": "SUCCESS",
    "entityId": "ENTITY6SICSOL71XVX",
    "storeName": "Test 1",
    "brandEntityId": "ENTITY6SICSOL71XVX",
    "storePageInfo": [{
            "storePageId": "75233FD4-DC27-4D39-A3AE-2DDBEE144AD2",
            "storePageUrl": "https://www.amazon.com/stores/page/75233FD4-DC27-4D39-A3AE-2DDBEE144AD2",
            "storePageName": "Home"
        },
        {
            "storePageId ": "75233FD4-DC27-4D39-A3AE-2DDBEE144AD3",
            "storePageUrl ": " https://www.amazon.com/stores/page/75233FD4-DC27-4D39-A3AE-2DDBEE144AD3",
            "storePageName ": "Subpage title"
        }
    ]
}]
```



## Brand Stores metrics

Metrics like sales, visits, page views, and traffic sources help you better understand how to best serve your shoppers. 

Learn more:

* [Brand Stores insights metrics](guides/brand-store/insight-metrics)

