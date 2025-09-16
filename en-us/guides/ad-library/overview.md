---
title: Ad Library overview
description: Overview on how to use the Ad Library API
type: guide
interface: api
tags:
    - Ad library
keywords:
    - overview
    - ad library
---

# Ad Library overview

The Ad Library API provides users with the ability to query data related to advertisements and affiliate marketing content displayed on Amazon's European Union (EU) stores. This API enables users to search and retrieve information about ads and affiliate marketing content shown across various Amazon EU marketplaces.

The Ad Library API is globally accessible through the `http://advertising-api-eu.amazon.com` endpoint. It contains data for advertisements and affiliate marketing content displayed on Amazon's EU stores in the following countries: Germany, Spain, France, Italy, Netherlands, Poland, Sweden, and Belgium.

## FAQs

1. **Is there a cost associated with using the Ad Library API?** <br />
    The Ad Library API is available at no cost.
2. **Who can access the Ad Library API?** <br />
    The Ad Library API is accessible to anyone who visits Amazon EU stores. Access to the Amazon Ad Library API requires a developer account and a Login with Amazon (LwA) security profile associated with the developer account. If you do not have an existing developer account, please visit https://www.developer.amazon.com/ to create an account.
3. **What types of ads can I search via the Ad Library API?** <br />
    All ads and affiliate marketing content displayed on the EU stores are accessible. Affiliate marketing content is content created by affiliate marketers within the Amazon Associates program and include Shoppable Photos, Idea Lists, and Articles.
4. **How are dates displayed on the API?** <br />
    The date string is specified in ISO format (YYYY-MM-DD) in the UTC timezone. 
5. **Is there a limit to the number of results I can obtain?** <br />
    The Ad Library provides up to 1,000 results for each request. A pagination token (nextToken) can be used to retrieve the next set of results.

## Learn more
* [API reference](ad-library)
* [Get started with Ad Library API](guides/ad-library/get-started)