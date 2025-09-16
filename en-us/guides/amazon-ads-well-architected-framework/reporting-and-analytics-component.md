---
title: reporting and analytics component
description: string
type: guide # (one of: guide)
interface: general # (one of: api, bulk-operations, amazon-marketing-stream)

---

## Reporting and analytics component

Reporting and analytics systems enable monitoring of Amazon Ads campaign performance and facilitate data-driven optimizations. This document presents design principles and best practices for building effective Amazon Ads [reporting and analytics systems](https://advertising.amazon.com/API/docs/en-us), guiding you in selecting appropriate alternatives for your specific needs.

The document applies to all reporting and analytics systems built on Amazon Ads, providing product-specific best practices for [reporting API](offline-report-prod-3p), [Amazon Marketing Stream](guides/amazon-marketing-stream/overview), and [Amazon Marketing Cloud](guides/amazon-marketing-cloud/overview). We offer reference implementations, emphasizing the importance of tailoring these to your unique environment and requirements.
Access to these reporting and analytics offerings requires approved Amazon Ads API credentials. For Amazon Marketing Stream and Amazon Marketing Cloud implementations, experience with AWS services and SQL knowledge is beneficial.

### **Reporting and analytics definitions**

* **Attribution:** Attribution is the measurement of conversion events, or shoppers behaviours that occur following an ad interaction. These behaviors measure the success of an ad campaign based on advertiser goals.
* **Traffic:** Traffic is defined as events such as impressions and clicks, which are the user interactions associated with a particular ad campaign.
* **Conversion:** A conversion is an action that your shoppers take, and that number can be defined by you and your business needs. These actions could be clicks on your website, purchases or new customer registrations for email newsletters, for example.
* **Attribution restatements:** The process of running additional attribution calculations to provide the most accurate advertising performance data over time. As new information becomes available, the attribution metrics can be updated and refined.
* **Pseudonymized signals:** Advertising and shoppers signals in Amazon Marketing Cloud (AMC) that has been anonymized and de-identified to protect individual privacy.

### **Reporting and analytics design principles**

* **Select purpose-built products for distinct analytics use cases:** Amazon Ads offers purpose-built products for different analytics use cases. These products differ based on integration patterns, data granularity, metric aggregation, and customization options. Align your product selection with your specific reporting and analytics needs to maximize operational efficiency.
* **Ensure data integrity across all layers:** Implement data validation and cleansing for advertising data integrity. Establish quality checks at ingestion, validating key metrics. Develop governance policies with lineage tracking and error handling. Regularly audit data pipelines. Prioritize data quality to improve analytics, decision-making, and reporting trust.
* **Persist advertising data for enhanced insights:** Store campaign performance data and metadata in scalable storage solutions like [data lakes](https://aws.amazon.com/big-data/datalakes-and-analytics/datalakes/) or data warehouses. Leverage this centralized repository for comprehensive reporting and analysis. This approach enables historical trend analysis, faster query responses, and integration with other business metrics, ultimately enhancing the advertiser experience and driving data-driven decision making.

### Reporting API

The [Amazon Ads reporting API](offline-report-prod-3p) offers a solution for accessing comprehensive advertising performance data. This RESTful API enables integrators to efficiently retrieve daily campaign metrics, historical data, and custom reports. Unlike stream-based architectures, the Reporting API does not require continuous data ingestion or processing, making it an optimal choice for daily or scheduled data retrieval with minimal infrastructure overhead. This approach is particularly effective for integrators seeking in-depth advertising performance analysis on an as-needed basis, facilitating data-driven decision-making without the complexity of maintaining real-time data pipelines.

#### **Reporting API best practices**

* **Leverage batch data retrieval:** The reporting API is designed for batch data retrival, so frequent intraday requests does not provide significant additional value. For near real-time data needs, utilize Amazon Marketing Stream instead of constantly polling the reporting API.
* **Maximize API efficiency with date range requests:** Utilize the reporting API's date range capability to retrieve up to 31 days of data in a single request. This approach reduces API calls, minimizes overhead, and streamlines data collection for historical analysis.
* **Manage reporting life cycle efficiently:** Implement systematic tracking of report IDs after initiating requests. Use stored IDs to retrieve reports, avoiding duplicate creation and too early responses(425). Remove pending reports before resubmitting identical requests. This practice reduces API calls, optimizes performance, and ensures efficient data retrieval while adhering to API best practices.
* **Implement exponential backoff and re-try after:** Implement [retry logic with exponential backoff](reference/concepts/rate-limiting#use-retry-logic-with-exponential-backoff) for report creation, using the ['Retry-After'](reference/concepts/rate-limiting#use-retry-logic-with-exponential-backoff)header to adjust timing. Avoid duplicate requests to prevent 425 errors. If persistent throttling occurs, reevaluate your use case. The reporting API isn't designed for high-frequency, real-time data retrieval. For near real-time needs, consider Amazon Marketing Stream.

#### Implementation guidance

* **Automated reporting solution for Amazon sponsored Ads**: This server less architecture leverages AWS services to automate the end-to-end process of creating, retrieving, and storing Sponsored Ads reports. By utilizing AWS Lambda for API interactions, AWS Step Functions for workflow orchestration, and Amazon EventBridge for scheduling, the solution enables advertisers to configure custom reporting schedules and parameters. Reports are securely stored in Amazon S3, providing a scalable and cost-effective method for maintaining up-to-date performance data. This approach minimizes manual effort, enhances data consistency, and facilitates ongoing campaign optimization and analysis.


### **Amazon Marketing Stream**

[Amazon Marketing Stream](amazon-marketing-stream/openapi) is a data-streaming service providing near real-time advertising performance data. It enables timely campaign decisions without API polling, avoiding throttling issues. The platform suits use cases requiring real-time insights, like bid, budget, and target optimizations. It offers detailed cross-dimensional data, including granular, placement-level insights for specific keywords, supporting informed campaign optimization.


#### **Amazon Marketing Stream best practices**

* **Choose appropriate delivery mechanisms:** When using Amazon Marketing Stream, you can choose between Amazon SQS, a reliable queue-based destination, or Amazon Data Firehose, an ingest and transform service. The choice should be guided by the specific requirements of the workload, such as message durability, and integration with downstream analytics tools. For more details please follow the [documentation](https://advertising.amazon.com/cms/contents/becb071d-ce3b-4e32-acf4-20a1dc19ce45).
* **Implement deduplication:** Amazon Marketing Stream uses Amazon SNS for message delivery, which may result in duplicate notifications. Utilize the `idempotency_id` parameter provided by Amazon Marketing Stream to implement effective [deduplication](guides/amazon-marketing-stream/aggregating-data) logic in your data processing workflows.
* **Aggregate delta messages:** Given the near real-time nature of Amazon Marketing Stream, consider  [aggregating](guides/amazon-marketing-stream/aggregating-data) delta messages at a cadence that aligns with your reporting and analysis needs. This approach provides a balanced view of performance trends while optimizing data processing resources.
* **Manage time zone considerations:** When comparing Amazon Marketing Stream data with other reporting sources, consider potential time zone differences, as these can affect the interpretation and analysis of performance metrics.
* **Monitor operational metrics of the delivery mechanisms**: [Monitor operational metrics](guides/amazon-marketing-stream/best-practices/sqs-cloudwatch) of the SQS or Firehose, the availability of source data, and validate the data quality at destination before making decisions.
* **Optimize cost:** For cost optimization, focus on leveraging a range of cost reduction strategies, such as event source mapping, batching, buffering, partitioning, and data transformation, as detailed in the [Amazon Ads documentation](guides/amazon-marketing-stream/best-practices/sqs-lambda).
* **Ensure access token management:** The Amazon Marketing Stream subscription is tied to the customer ID. If the end user who approved the access token leaves, you will lose access to the subscription. Implement mechanisms to securely store the access tokens and gracefully handle any access token transfers or revocations.
* **Maintain data consistency across datasets:** When making decisions based on insights derived from multiple Amazon Marketing Stream datasets, ensure that all the relevant datasets are up-to-date and consistent. Design your processing pipelines to handle potential failures or data inconsistencies to maintain the reliability of your analytical outputs.

#### **Implementation guidance**

* **Infrastructure as Code (IaC):** [Implement Infrastructure as Code](https://github.com/amzn/ads-advanced-tools-docs/tree/main/amazon_marketing_stream) to provision and manage Amazon Marketing Stream resources consistently across datasets and regions. This approach automates the setup of SQS queues or Kinesis Data Firehose streams and associated permissions, ensuring scalability, reducing manual errors, and enabling efficient replication of stream infrastructure.
* **Real-time advertising data processing with Amazon Marketing Stream:** [This reference architecture](https://github.com/amzn/amazon-marketing-stream-examples) illustrates the processing of near real-time advertising data via Amazon Marketing Stream. The solution utilizes AWS services to efficiently ingest, process, and analyze streaming data, facilitating near real-time campaign performance monitoring and intraday optimization strategies.

### Amazon Marketing Cloud

Amazon Marketing Cloud (AMC) is a comprehensive, flexible analytics platform that provides unified signals and insights to drive omni-channel advertising strategies. With AMC, advertisers can leverage a suite of audience segmentation, campaign measurement, and custom data integration capabilities to unlock deeper customer insights and optimize their marketing efforts. The AMC APIs are interfaces for developers to programmatically access AMC features. AMC APIs are often used to automate or schedule query workflows, create custom audiences, facilitate scaled operations, upload first-party signals, and enable integrations between AMC and other systems.

#### **AMC best practices**

* **Use AMC sandbox for development activities**: Users can simulate AMC activities in a sandbox environment, without affecting any live AMC instances. AMC sandbox mimics the functionality of advertiser AMC instances, but contains only synthetic signals that are artificially and programmatically generated to reflect the structure and statistical properties of advertiser signals. Run queries with Aggregation Controls turned off to inspect non-aggregated reporting in detail and understand how to optimize queries. Learn more about AMC sandbox [here](guides/amazon-marketing-cloud/amc-sandbox).
* **Schedule routinely used analyses**: Workflows can be executed "on-demand" via the the [Workflow Execution](amc-reporting#tag/Workflow-executions) resources or they can be scheduled to run at a specific time via AMC's built-in [Schedules](amc-reporting#tag/Schedules) resources.
* **Leverage AMC for analytics instead of reporting**: AMC does not ingest data at real-time and propagates ad activity data after a time-lag. As a best practice, AMC recommends that you select a date range of 48-hours prior to the current date. This is because, the result of your query may be incomplete as the data can take up to 48 hours to process and be available in your instances. AMC has a Data Availability Service Level Agreement (SLA) of 48 hours. This SLA guarantees that for all AMC instances, data will be available within 48 hours of its creation or update in the origin system.
* **Adhere with AMC privacy safeguards and aggregation thresholds:** To protect the privacy of our shoppers exposed to ads, we have privacy safeguards in place that are apparent when you query the data. The privacy safeguards prevent AMC users from exporting data at a grain that would violate our privacy safeguards. The aggregation threshold of a column determines the restrictions that are enforced to ensure customer privacy safeguards are not violated if a column is used in a workflow. See [aggregation thresholds](guides/amazon-marketing-cloud/aggregation-threshold) and [Append aggregation threshold columns](guides/amazon-marketing-cloud/reporting/create-workflow#append-aggregation-threshold-columns) for more details. You can also follow[these steps](guides/amazon-marketing-cloud/aggregation-threshold#steps-to-follow-when-your-query-output-has-null-rows)for guidance on how to handle NULL rows. 

