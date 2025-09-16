---
title: Troubleshooting issues with the Brand Metrics API
description: Troubleshooting basic issues calling the brand metrics API
type: guide
interface: api
tags:
    - Brand Metrics
keywords:
    - troubleshooting
    - FAQ
    - frequently asked questions
---

# Troubleshooting issues with the Brand Metrics API

This troubleshooting guide is divided into three sections: Account authorization, reporting, and data issues. 

## Account authorization

In order to use the Brand Metrics API, your API developer account must have access to an advertiser’s advertising profile. Please refer to the [authorization grant](guides/get-started/create-authorization-grant) for authorization methods. 

**I called the profiles endpoint but I don’t see my advertiser’s profile in the response**

Your advertiser must grant you access to their profile, and then the profile should appear in the profiles endpoint response list within two hours. If you're using the API test domain, you should use the production domain instead. Please refer to Please refer to the [authorization grant](guides/get-started/create-authorization-grant) and verify you have requested authorization correctly.

**I get an HTTP 403 response when I call an Brand Metrics endpoint using my advertiser’s profile, even though I am authorized**

The advertiser profile passed in the request may not be eligible for Brand Metrics. See the [overview](guides/reporting/brand-metrics/overview) for eligibility requirements.

An HTTP 403 response may indicate a lack of permissions for the account to view data for the brand or brands. If this is the case, it is up to the advertiser to register brands using the advertising console.   

For Sellers - Their brand is not registered in Brand Registry. We rely on BrandRegistry to determine brand access permissions for sellers. Make sure that the seller account associated with your advertiser account has access to enhanced brand tools such as Vine, Brand Analytics, and the like. For more information about Brand Registry, see the [announcement](https://advertising.amazon.com/blog/brand-registry/?ref_=a20m_us_search_title).

For Vendors - The vendor does not have a verified brand relationship in BrandAid. We rely on BrandAid to determine brand access permissions for vendors. Other issues that may be preventing access is an [unverified status with Brand Aid](https://advertising.amazon.com/en-us/resources/whats-new/brand-management-self-service/?ref_=a20m_us_wn_gw). For more information about the Brand management self-service console launch, see the [announcement](https://advertising.amazon.com/en-us/resources/whats-new/brand-management-self-service/?ref_=a20m_us_wn_gw). 

If you are still experiencing issues, it may be possible that the brand is listed as a homonym. Please reach out to your account manager to investigate.  

## Reporting 

**Why are some of metrics not available?** 

Please note: Brand Metrics is in open beta and available to ~ 50% of brands. Brands that meet the eligibility criteria may not yet have access to Brand Metrics. We will continue to expand the open beta to eligible brands in 2022.

Additionally, metrics are not shown for a brand if there is insufficient data for a requested metric.