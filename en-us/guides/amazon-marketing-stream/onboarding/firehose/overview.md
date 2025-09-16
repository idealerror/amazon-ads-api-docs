---
title: Introduction to Amazon Data Firehose
description: Introduction to Amazon Data Firehose
type: guide
interface: amazon-marketing-stream
tags:
    - Amazon Data Firehose
keywords:
    - Amazon Marketing Stream
    - Amazon Data Firehose
    - SQS
    - AWS
    - create subscription
---

# Introduction to Amazon Data Firehose 

Amazon Data Firehose is a fully managed service for delivering real-time [streaming data](http://aws.amazon.com/streaming-data/) to destinations such as Amazon Simple Storage Service (Amazon S3), Amazon Redshift, Amazon OpenSearch Service, Amazon OpenSearch Serverless, Splunk, and any custom HTTP endpoint or HTTP endpoints owned by supported third-party service providers, including Datadog, Dynatrace, LogicMonitor, MongoDB, New Relic, Coralogix, and Elastic. With Amazon Data Firehose, you don't need to write applications or manage resources. You configure your data producers to send data to Amazon Data Firehose, and it automatically delivers the data to the destination that you specified. 

With Amazon Data Firehose, you can subscribe to Amazon Marketing Stream datasets and have them delivered to a location of your choice—including third-party data outputs. Without Firehose, your Stream datasets can only be delivered to Amazon SQS, so Firehose provides more flexibility if you prefer a different data output.

By integrating Amazon Data Firehose as a delivery option, customers can streamline their data ingestion process and minimize operational costs. As a fully managed service, it automatically scales to accommodate data volume, and eliminates the need for ongoing administration. Additionally, Firehose can batch, compress, and encrypt data before delivery—reducing storage consumption at the destination and enhancing security. To learn more about Amazon Data Firehose, refer to the official AWS [Amazon Data Firehose developer guide](https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html).

## Component architecture 

![Component architecture](/_images/amazon-marketing-stream/intro-firehose-architecture-diagram.png "Firehose component architecture") 
This component architecture depicts the data flow path of the Amazon Marketing Stream Data Firehose architecture. Let's break down the key components:

*  **Stream (Input):** This represents the source of data, which originates from the Amazon Marketing Stream itself.
*  **Stream SNS Topic (Ingestion):** Data is ingested into the customer’s system via a Simple Notification Service (SNS) topic. SNS is a service that facilitates coordinated message delivery across multiple AWS services.
* **Amazon Data Firehose (Data delivery):** Firehose is a service designed for near real-time data delivery to various destinations. Firehose excels at efficient and reliable data delivery.
* **Optional data transformation (AWS Lambda):** This step allows you to optionally transform the data using AWS Lambda. Lambda is a serverless compute service that empowers you to run code without provisioning or managing servers.
* **Data Outputs:** Processed or raw data can be delivered to various applications based on your specific needs. Some popular choices include:
    * **Amazon S3:** Stores the data in an object storage service, ideal for raw data archiving or further processing. (An example is provided below.)
    * **Amazon Redshift:** Loads the data into a data warehouse for large-scale data analysis.
    * **Snowflake (or other data warehouses):** Delivers the data to a data warehouse of your choice, depending on your existing tech stack.
    * **Splunk:** Routes the data to a software platform for searching, analyzing, and visualizing machine-generated data.
    * **OpenSearch Service:** Delivers the data to a search and analytics engine for logging data analysis and visualization.
    * **6+ HTTP Endpoint Partners:** Delivers data to various partner destinations through HTTP endpoints.

## Choosing your data output

The chosen data output location depends on your specific needs and tech stack. You may want the data readily available in S3 for further processing, analyze it in a data warehouse like Redshift or Snowflake, or integrate it with other services like Splunk or OpenSearch.

## Next steps for getting started

To set up Amazon Data Firehose, view the [Getting started guide](guides/amazon-marketing-stream/onboarding/firehose/get-started), which includes a comprehensive [video walkthrough and demo of Amazon Data Firehose](guides/amazon-marketing-stream/onboarding/firehose/get-started#video-demo).
