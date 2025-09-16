---
title: fundamental component
description: string
type: guide # (one of: guide)
interface: general # (one of: api, bulk-operations, amazon-marketing-stream)

---

## Fundamental component

The Fundamental component serves as the backbone of solution architecture, outlining best practices that span across all or multiple framework components. It ensures consistency and efficiency throughout the entire system. This component covers key areas such as architectural principles, security standards, performance optimization, and data management. By establishing these overarching guidelines, the Fundamental component promotes scalability, modularity, and interoperability.

### Definitions

The definitions presented here are tailored to the context of Amazon Ads, and may have a narrower scope compared to more general definitions of these concepts.

* **Amazon Ads partner**: Amazon Ads partners are companies available to support the needs you may have for a variety of business tasks and strategies. They have a range of expertise across Amazon Ads products and services, and they can act as a resource to help businesses achieve their goals in the Amazon store or beyond.
* **Amazon advertiser**: Advertisers that use Amazon Ads for their advertising activities.
* **Amazon seller**: Sellers sell products directly to Amazon customers.
* **Amazon vendor**: Vendors sell their items directly to Amazon, who then sells them to customers.
* **Regions**: The Amazon Ads API operates in three regions: North America (NA), Europe (EU), and Asia (FE), each with its own endpoint. Data for each region is exclusively accessible through its corresponding endpoint. This structure ensures data locality, optimizes performance, and maintains data sovereignty. Advertisers and developers should connect to the appropriate regional endpoint to access or manipulate their campaign data.
* **Transactions per second (TPS):** TPS measures the rate at which Amazon Ads API endpoints can process requests or transactions.
* **TPS limit:** A Transactions Per Second (TPS) limit is set at the regional level for each API endpoint to manage the load and ensure fair usage across clients. This limit represents the maximum number of transactions that can be performed within a one-second timeframe, with exceeding the limit resulting in request throttling to maintain the overall system performance.

### Fundamental design principles

* **Customers first:** Customers (advertisers) come first, always. The Amazon Ads WAF is an effort to streamline solutions developed by third-parties, ensuring these experiences continue to delight our customers.
* **Modular design**: The system should be designed with a modular/loosely-coupled architecture, where different components encapsulate distinct responsibilities and interactions are well-defined. This modular design promotes maintainability, testability, and flexibility.
* **Scalability and performance**: The solution should be designed to scale efficiently, handling increased workloads and data without significant performance degradation. For example, solution must be able to handle increased volume during the key shopping events like Amazon Prime Day.
* **Consider evolutionary architectures**: As a business and its context continue to evolve, these initial decisions might hinder the system's ability to deliver changing business requirements. Build architectures that can evolve with business needs and review your architecture periodically to make sure that it meets the business requirements.
* **Drive architectures using data**: Collecting comprehensive data on the target system, whether by studying a legacy system or using a similar existing system as a proxy, is crucial for defining the system architecture.
* **Regulatory compliance in advertiser data management:** Ensure strict adherence to country-specific data handling regulations when managing advertiser signals to maintain compliance and foster trust in your data governance practices.
* **Maintain security and controlled access:** Encrypt all advertising data, both at rest and in transit, to ensure confidentiality and maintain data integrity throughout your AdTech infrastructure. Implement strong access controls and authentication mechanisms.
* **Ensure resilience in API interactions:** Implement error handling and retry mechanisms to manage API failures and throttling events effectively. Design your systems to handle service disruptions, network issues, and rate limiting.
* **Infrastructure as Code (IaC):** Implement Infrastructure as Code (IaC) to manage multiple AdTech environments consistently. Codify common infrastructure components and configurations across development, testing, production or any other environment using AWS CloudFormation or similar tools. Maintain version control for all codified artifacts to track changes and enable rollbacks.
* **Architect for mechanical sympathy:** Understand Amazon Ads APIs' characteristics, including rate limits, throughputs, and data freshness requirements. Align your system architecture with these characteristics to optimize performance and enhance user experience.
* **Batch operations when supported by the API:** Use batch operations in Amazon Ads API to improve performance and minimize calls.

### Fundamental best practices

* ****Gather business requirements and define solutions** **before integration****: Engage key stakeholders, including business, product and technology teams, to discuss goals, needs, and priorities of customers. Early engagement of stakeholders will help you define an optimal solution and optimal usage of Amazon Ads API.
* ****Estimate the API call volume accurately****: Calculating the expected API call volume based on your business requirements is a crucial step before integrating with any API. This upfront analysis allows you to properly size and provision the necessary infrastructure, optimize costs, manage performance, design for scalability, and set meaningful monitoring thresholds.
* ****Optimize API call payload to reduce the call volume****: Optimizing API payloads is essential for minimizing the overall API call volume required  by an application. Lowering the API call volume reduces the strain on the API's infrastructure and your application.
* **Monitor and manage API call volume**: Implement monitoring systems to track API call volumes and set up alerts for approaching thresholds. Regularly review usage patterns and proactively manage quotas to stay ahead of potential issues.
* **Protect all sensitive data at rest and in transit**: Enforce the use of encryption for data at rest and in transit. Encryption maintains the confidentiality of sensitive data in the event of unauthorized access or accidental disclosure.
* [**Define recovery objectives for downtime and data loss**](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/rel_planning_for_recovery_objective_defined_recovery.html)**:** Recovery Time Objective (RTO) is the maximum acceptable delay between the interruption of service and restoration of service. Recovery Point Objective (RPO) is the maximum acceptable time after the last data recovery point. Define RTO and RPO based on your application’s business requirements.
* **[Use defined recovery strategies to meet the recovery objectives](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/rel_planning_for_recovery_disaster_recovery.html)**: Define a disaster recovery (DR) strategy that meets your application’s recovery objectives. Choose a strategy such as backup and restore, standby (active/passive), or active/active.
* **Automate your backups:** Regular backups to a durable storage service ensure that you have the ability to recover from administrative, logical, or physical error scenarios. Configure backups to be taken automatically based on a periodic schedule, or by changes in the dataset.
* **Leverage asynchronous processing for** **long-running** **or complex operations:** Implement asynchronous workflows for bulk campaign creation or updates, using polling or web hook mechanisms to handle completion of these tasks.

### Implementation guidance

* **Gather business requirements and define solutions before integration:** Gathering business requirements and defining solutions before integration is a critical step in ensuring successful implementation of Amazon Ads APIs. This process helps align the technical integration with business goals and user/advertiser needs, reducing the risk of costly rework and improving overall project outcomes.

  Begin by engaging key stakeholders and creating a Product Requirement Document (PRD) before initiating system design and planning. This critical first step ensures that the project is aligned with business objectives and user needs from the outset.

* **Choose purpose-built API:** Amazon Ads provides a comprehensive suite of APIs to help you build advertising solutions, including Campaign Management, Reporting, Recommendations, and Creative APIs. For example, if you need to retrieve metadata in bulk, use the [Exports API](exports). Exports and GET operations return similar campaign metadata: use Exports (async) for bulk campaigns, use GET (sync) for single or subset of campaigns.
* **Estimate the API call volume accurately:** Calculating the expected API call volume is a crucial step before integrating with any API. This upfront analysis allows you to properly size and provision the necessary infrastructure, optimize costs, manage performance, design for scalability, and set meaningful monitoring thresholds. To accomplish this, start by mapping out your business processes that will interact with the API, estimating the frequency of these interactions, and considering factors like peak usage times and potential growth.

  You should use a framework for assessing API usage and optimization that consists of three stages. First, review existing data by analyzing current call patterns and TPS needs for existing advertisers, or using lookalike models based on factors like the number of entities (campaigns, ad groups, etc.) supported and types of API calls. Next, conduct a deep review of your solution, examining the functional design and discussing potential call patterns. Finally, identify optimization opportunities, leveraging an understanding of your solution and calling patterns to suggest improvements. If gaps remain between your TPS needs and assigned capacity after optimization, explore mitigation options. This structured approach ensures efficient API usage and helps partners maximize the value they derive from Amazon Ads APIs.

  With this information, you can make informed decisions about infrastructure needs, set appropriate rate limiting in your code, and establish alerts for when usage approaches thresholds. This proactive approach helps ensure smooth integration and operation, minimizing the risk of performance issues or unexpected implementation costs as you leverage the Amazon Ads APIs.

* **Optimize API request and response payloads:** Optimizing API payloads is a critical strategy for enhancing the efficiency and performance of applications that integrate with Amazon Ads APIs. This approach focuses on minimizing the overall API call volume, which in turn reduces the strain on both the API's infrastructure and the application itself.

  The process begins with a careful analysis of the current API usage patterns. Developers should examine the frequency and content of their API calls, identifying opportunities to consolidate requests or eliminate redundant data retrieval. This might involve implementing more sophisticated caching mechanisms to store frequently accessed data locally, reducing the need for repeated API calls.

  Another key aspect of payload optimization is the judicious use of query parameters and filters. By crafting more precise API requests, applications can retrieve only the necessary data, reducing the size of responses and the processing load on both ends. This targeted approach not only improves response times but also helps in staying within API rate limits.

  Batching is another powerful technique for payload optimization. Instead of making multiple individual API calls, developers can group related requests into a single, larger payload. This approach significantly reduces the overhead associated with establishing multiple connections and can lead to substantial improvements in overall performance.

* **Exponential backoff:** When a system encounters a failed or slow request, it can leverage an exponential backoff and retry strategy to handle the issue gracefully. This approach involves the system automatically waiting for an exponentially increasing amount of time before retrying the request. For example, the first retry might happen after 100 milliseconds, the second after 200 milliseconds, the fourth after 400 milliseconds, and so on, doubling the delay with each subsequent retry attempt. You can also add a random jitter value to the delay. The jitter value can be a random number between 0 and the current delay amount. You can find additional information on exponential backoff and retry [here](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/).
  * **Initial delay**: Start with an initial delay before the first retry attempt, e.g., 1 second. 
  * **Doubling delay**: For each subsequent retry attempt, double the delay. So, the delays would be: 1 second, 2 seconds, 4 seconds, 8 seconds, and so on. 
  * **Jitter**: After calculating the delay based on the doubling factor, add a random jitter value to the delay. The jitter value can be a random number between 0 and the current delay amount. 
  * **Maximum delay**: Optionally, you can set a maximum delay to avoid the delays becoming too long, e.g., 60 seconds.
  * **Limit retries**: You can also set a limit on the number of retry attempts, e.g., 5 or 10 attempts.
* **Monitor and manage API call volumes:** Most Amazon ads APIs set API rate limits to ensure all clients can use the API effectively. API rate limits are defined as a maximum number of calls a client can make to each endpoint in each region within a time period. If your app makes a lot of API requests in a short period of time then it may receive a throttling error (429) response which may lead to bad user experience. We recommend following the steps below to effectively manage to rate limits.
  * Understand the rate limits for your API.
  * Use exponential backoff and retry for failed API calls.
  * Log API calls so they call volume can be monitored.
  * Continuously monitor api call volume and alert appropriate teams (e.g. DevOps) if it reaches a pre-defined threshold.
  * Avoid throttling using mechanisms including distributing the API calls over longer periods of time, optimizing the API payload (batching) to reduce the number of calls, and use different API which is better suited for the task (e.g., use reporting tools for mass data retrieval instead of CM API). where possible.

  Some SDKs implement retries and exponential backoff by default. Use these built-in implementations where applicable in your workload. Implement similar logic in your workload when calling services that are idempotent and where retries improve your client availability. Decide what the timeouts are and when to stop retrying based on your use case. Build and exercise testing scenarios for those retry use cases.

* **Enforce encryption at rest**: Data at rest encompasses all persistent data stored within your workload. Implement encryption and appropriate access controls to protect this data across various storage types.
* **Enforce encryption in transit**: Data in transit refers to data moving between systems within your workload or to external services and users. Protect the confidentiality and integrity of this data by implementing appropriate security measures, such as encrypted protocols. Ensure compliance with security standards when handling API responses or customer data. For detailed guidance, consult the [data protection](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/data-protection.html) section of the AWS Well-Architected Framework security pillar.
* **Automate your backups:** Implement automated backups to protect against administrative, logical, and physical failures. Use appropriate backup tools to maintain data consistency. Validate your recovery procedures through regular testing, especially after significant environment changes. For detailed backup strategies, refer to the [backup and recovery approaches on AWS](https://docs.aws.amazon.com/prescriptive-guidance/latest/backup-recovery/welcome.html).
* **Use advertising test accounts:** [Test accounts](guides/account-management/test-accounts/overview) support the creation of advertising test account dedicated for testing. Test accounts do not serve ads or have active billing, allowing testing and learning of Amazon Ads API functionalities. Use Test account API to self-provision test accounts and maintain a separate environment for testing API features and roll-out changes. Leverage test accounts as well to create rollback procedures for campaign configurations and manage API version updates.
* **Implement Amazon Ads API versioning strategy:** Amazon Ads API can be versioned independently. Adhere to version changes, manage compatibility updates, and follow [Amazon ads deprecation policy](release-notes/deprecations).
* **Handle HTTP 207 Multi-Status responses for bulk operations:** When using Amazon Ads APIs that support bulk operations (multiple objects in a single request), expect HTTP 207 Multi-Status responses. These responses indicate that some operations may have succeeded while others failed within the same request. Applications must parse the response body to determine the status of each individual operation and handle successes and failures accordingly. Each object in the response will contain its own status code and message.

