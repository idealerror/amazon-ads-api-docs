---
title: introduction
description: string
type: guide # (one of: guide)
interface: general # (one of: api, bulk-operations, amazon-marketing-stream)

---
## Introduction

The Amazon Ads Well-Architected Framework (Amazon Ads WAF) provides guidance on making informed architecture decisions when building advertising solutions that leverage the Amazon Ads API. Centered around six framework components — fundamental, account management, campaign management, reporting and analytics, insights and Rrecommendations, and integration with Amazon's Selling Partner API. This framework defines architectural best practices to help design and operate reliable, secure, efficient, cost-effective, and sustainable solutions that integrate with Amazon Ads API. This document focuses specifically on effectively using the Amazon Ads API. For general architectural guidance, please refer to the [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/welcome.html). Some of the best practices from AWS Well-Architected Framework are re-presented in this document to make additional recommendations specific to the Amazon Ads API.

In some cases, these best practices are repeated due to their high importance. By adopting this framework, technology professionals such as chief technology officers (CTOs), architect, product, and engineering team members can unlock the full potential of the Amazon Ads platform and ensure the reliable performance and optimization of the advertising infrastructure.

## Framework components

The Amazon Ads API offers a suite of features and capabilities to help advertisers manage, measure, and optimize their advertising efforts. The framework divides Amazon Ads API into six components, each addressing a distinct aspect of Amazon Ads. This document provides comprehensive architecture guidance and best practices across all these components for integrating with Amazon Ads.

* The **[fundamental](guides/amazon-ads-well-architected-framework/fundamental-component)** component establishes core design principles that form the foundation of any Amazon Ads solution. These principles apply across all framework components and guide critical technical decisions.
* The **[reporting and analytics](guides/amazon-ads-well-architected-framework/reporting-and-analytics-component)** component enables monitoring of Amazon Ads campaign performance and facilitate data-driven optimizations. This document presents design principles and best practices for building effective Amazon Ads reporting and analytics systems, guiding you in selecting appropriate solutions for your specific needs.
* The **[campaign management](guides/amazon-ads-well-architected-framework/campaign-management-component)** component focuses on the best practices for creating and managing campaigns across the diverse range of Amazon Ads products. This includes guidance on campaign setup, activation, and optimization to help advertisers achieve their advertising goals.
* The **account management** component outlines the best practices for creating and maintaining various Amazon Ads accounts, including test accounts. This ensures that advertisers have a well-structured and organized approach to their account setup, enabling them to efficiently manage their advertising activities.
* The **insights and recommendations** component examines the best practices for utilizing the insights and recommendations provided by Amazon Ads. This leverages Amazon Ads' machine learning and artificial intelligence expertise to help you achieve better campaign outcomes.
* The **Amazon's Selling Partner API** component explores the integration of the Amazon Selling Partner API (SP-API) with the Amazon Ads API. This allows you to create more targeted campaigns by accessing specific Amazon retail information: inventory levels, order data, product, and pricing details.

>Account management, insights and recommendations, and selling partner API integrations components will be available at a later date.

