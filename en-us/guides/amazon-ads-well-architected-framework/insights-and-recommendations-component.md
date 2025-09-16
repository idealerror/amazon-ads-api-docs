---
title: Insights and recommendations component
description: string
type: guide # (one of: guide)
interface: general # (one of: api, bulk-operations, amazon-marketing-stream)

---

# Insights and recommendations component

The insights and recommendations component provides guidance on leveraging Amazon Ads insights and recommendations capabilities to inform decision-making, optimize campaigns, and discover new growth opportunities.

## Definitions

* **Insights:** Insights are data-driven observations and analyses that help advertisers understand their [audiences](https://advertising.amazon.com/API/docs/en-us/reference/migration-guides/adsp-campaign-management#audience-targeting), [campaign performance](https://advertising.amazon.com/API/docs/en-us/offline-report-prod-3p), and [retail trends](https://advertising.amazon.com/measurement-analytics/rapid-retail-analytics).
* **Recommendations:** Recommendations deliver specific, actionable optimization suggestions including bid adjustments, budget allocations, targeting improvements, and creative enhancements to help advertisers improve campaign performance.
* **Budget recommendations:** Suggests daily budgets and estimated missed opportunity metrics, including impressions, clicks, and sales for all sponsored ad products.
* **Bid recommendations:** Provides advertisers with suggested bid values for keywords, products, categories, or audiences. These recommendations are tailored to maximize performance metrics such as impressions, clicks, or conversions.
* **Targeting recommendations:** Suggests targeting strategy, including keywords, products, theme-based, category targeting, or audiences to reach more relevant shoppers and improve campaign efficiency.
* **Creative recommendations:** Suggests improvements to ad creatives, such as headlines, to increase engagement and conversion rates.
* **Consolidated recommendations:** Get the top [consolidated recommendations](https://advertising.amazon.com/API/docs/en-us/sponsored-products/3-0/openapi/prod#tag/Consolidated-Recommendations/operation/getCampaignRecommendations) across bid, budget, targeting for Sponsored Products campaigns. The recommendations are refreshed every day.
* **[Partner opportunities](https://advertising.amazon.com/API/docs/en-us/partner-opportunities):** A distribution channel for partners to download actionable and relevant Insights and recommendations for advertisers they support. These opportunities are available only for partners registered to the Amazon Ads Partner Network (AAPN).
* **[Audience Insights](https://advertising.amazon.com/API/docs/en-us/overlapping-audiences#/Audience%20insights/insightsGetAudiencesOverlappingAudiences):** Helps advertisers explore similar audiences, based on audiences you are already using.
* **[Brand Metrics](https://advertising.amazon.com/API/docs/en-us/guides/reporting/brand-metrics/getting-started):** Brand Metrics provides a new measurement solution that quantifies opportunities for your brand at each stage of the customer journey on Amazon, and helps brands understand the value of different shopping engagements that impact stages of that journey.
* **[Brand Benchmarks](https://advertising.amazon.com/API/docs/en-us/brand-benchmarks):** Serves various inquiries regarding advertiser and brand metrics and surfaces Amazon Brand View Pro reports for 3P callers.
* **Media planning:** Media planning services address a range of challenges faced by media planning teams - from understanding your potential audience and forecasting reach, to optimizing your media mix and measuring campaign performance. By tapping into Amazon's unique data and signals, you can make more informed, strategic decisions to drive better outcomes for your business and your clients. These include the [Historic Reach API](https://advertising.amazon.com/API/docs/en-us/guides/media-planning/historic-reach/overview), [Reach Forecasting API](https://advertising.amazon.com/API/docs/en-us/guides/media-planning/reach-forecasting/overview), [Audiences discovery API](https://advertising.amazon.com/API/docs/en-us/audiences#tag/Discovery), and [Persona builder API](https://advertising.amazon.com/API/docs/en-us/persona-builder).

## Design Principles

* **Select purpose-built insights or recommendations services:** Amazon Ads provides purpose-built insights and recommendation services designed to support strategic planning, performance optimization, and growth objectives. Select services that align with your specific goals and campaign lifecycle stage.
* **Implement holistic campaign optimization:** Consider all aspects of campaign performance when generating recommendations such as audience targeting, budget allocation, keyword selection, and creative optimization across all your portfolio.To avoid recommendation overlap, start with services that directly support your primary goals, then expand based on proven results and team capacity.
* **Enable independent scaling:** Separate recommendation ingestion from recommendation implementation for independent scaling. This principle aligns with the fundamental design principles of ‘modularity’, ‘scalability'  and 'performance'.
* **Facilitate progressive implementation:** Design systems that allow for incremental adoption of recommendation capabilities, starting with high-impact, low-complexity features. This principle aligns with the fundamental design principles of 'Consider evolutionary architectures'.
* **Define recommendation lifecycle workflow:** Consider implementing stages in recommendation workflows such as discover, validate, approve, implement, and measure. These stages provide a logical flow that can be customized and implemented as needed. Since some recommendations come with expiration times, implement expiration tracking to measure effectiveness across various recommendation types.
* **Implement observability:** Embed observability into every stage of recommendation processing to proactively detect failures, conflicts, and data staleness.

## Best Practices

* **Select purpose-built insights and recommendations services:** Select purpose-built insights and recommendations that align with your business goals. Your approach should reflect factors like campaign maturity, current performance, budget limitations, and business priorities. New campaigns or seasonal businesses may need different combinations of recommendations compared to established or year-round operations.
Examples include:
    * **Strategic planning:** Explore services such as  Media Planning and Partner Opportunities.
    * **Performance optimization:** Consider bid, budget, and targeting recommendations etc.
    * **Growth and expansion**: Leverage services such as Audience discovery and Partner Opportunities.
* **Unify ad insights and actions:** Amazon Ads provides various signals for diverse needs, ranging from bid and budget management to targeting, creative optimization, partner opportunities, audience insights, brand analytics, and consolidated recommendations. Unify these ad insights and actions to prevent contradictory optimizations that could destabilize campaigns. Examples of such contradictions include simultaneous budget cuts and bid increases, budget decreases coupled with creative expansion, and conflicting advice from consolidated recommendations and partner opportunity optimizations.
* **Adopt time-driven processing:** Design different processing approaches based on the insights and recommendation’s time-sensitivity and complexity. For example, Amazon Marketing Stream provides real-time signals that require near real-time action. The [`budget-usage`](https://advertising.amazon.com/API/docs/en-us/guides/amazon-marketing-stream/datasets/budget-usage) dataset monitors campaigns nearing their budget limits, while [`sp-budget-recommendations`](https://advertising.amazon.com/API/docs/en-us/guides/amazon-marketing-stream/datasets/budget-recs-missed-opportunities) dataset uncovers missed opportunities. These signals provide actionable insights and recommendations and do require near real-time implementation. On the other hand, strategic insights and recommendations like Partner Opportunities, media planning,  product targeting, and keyword suggestions contribute to your overarching advertising strategy. While valuable, These insights allow flexible implementation at a pace that works best for your process.
* **Validate with data-driven insights:** Use performance data from the services covered in detail in the [reporting and analytics component](https://advertising.amazon.com/API/docs/en-us/guides/amazon-ads-well-architected-framework/reporting-and-analytics-component) to validate recommendation effectiveness. Implement feedback loops that measure recommendation impact and improve future recommendation selection.
* **Monitor recommendation relevance:** Monitor recommendation relevance by implementing freshness checks for different recommendation types. For example, Partner Opportunities refresh every 24 hours and may include expiration timestamps. Act only on timely information that aligns with your business goals.
* **Centralize recommendations with Partner Opportunities API:** Prioritize Partner Opportunities API as the primary entry point for obtaining recommendations across all advertisers managed within your partner network account.
* **Automate easy win opportunities:** Partner Opportunities provides **confidence** and **impact score** metrics. Use these metrics to automate low-risk/high-reward actions and improve efficiency while maintaining control. For example, filter and auto-apply recommendations where `confidence > 0.8 AND impactScore > 7`. Other APIs, like [**Brand Metrics**](https://advertising.amazon.com/API/docs/en-us/guides/reporting/brand-metrics/how-to), provide metrics such as **newToBrandCustomerRate** and **customerConversionRate**, and [Audience Insights API](https://advertising.amazon.com/API/docs/en-us/guides/dsp/overlapping-audiences) provides **affinity** scores, which can be used in a similar way.

 ## Implementation Guidance

* **Partner Opportunities API implementation:** Partners use the Partner Opportunities API to efficiently discover, download, and apply recommendations at scale, automate campaign actions, track results, and drive better outcomes for all advertisers they manage, without needing to log into each account individually or manually aggregate insights.
    * **Discover:** The partner makes a single API call to `GET /partnerOpportunities` with their Partner Network account headers. The response includes a list of opportunities, each tagged with metadata (e.g., advertiser, product, objective type, and a data file URL). The partner checks `dataMetadata.rowCount` for each opportunity. If greater than zero, actionable data is available.
    * **Download:** For each opportunity with data, the partner downloads the CSV file using the `GET /partnerOpportunities/{partnerOpportunityId}/file` endpoint.
    * **Implement:** The partner programmatically applies these recommendations by posting to `/partnerOpportunities/{partnerOpportunityId}/apply` with the required payload
    * **Monitor:** The partner uses `/partnerOpportunities/{partnerOpportunityId}/applicationStatus` to check the status of applied recommendations.

Explore these workflows in the Partner Opportunities Integration flow diagram below:

![Partner Opportunities Integration Flow](/_images/amazon-ads-well-architected-framework/insights.png)
