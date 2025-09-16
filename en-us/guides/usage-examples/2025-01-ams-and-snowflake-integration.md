---
title: Streamline Amazon Marketing Stream Data Integration with Snowflake 
description: This guide describes how to get Amazon Marketing Stream data into Snowflake, regardless of where your Snowflake instance is hosted.
type: guide
interface: amazon-marketing-stream
keywords:
    - Amazon Marketing Stream
    - Snowflake
---
# Streamline Amazon Marketing Stream Data Integration with Snowflake 

Amazon Marketing Stream (AMS) helps drive success in digital advertising with near real-time insights from Amazon Ads reporting data.

This guide demonstrates how you can send AMS data directly to Snowflake, regardless of where you host your Snowflake instance (Amazon Web Services, Microsoft Azure, Google Cloud Platform, etc.).

## Overview

Snowflake AI Data Cloud platform provides data solutions from data warehousing and collaboration to data science and generative AI. Snowflake is an [AWS Partner](https://partners.amazonaws.com/partners/001E000000d8qQcIAI/Snowflake) with multiple AWS accreditations that include AWS ISV Competencies in Generative AI, Machine Learning, Data and Analytics, and Retail.

Previous Snowflake integrations were a multi-step process using Amazon Simple Queue Service (Amazon SQS), Amazon Kinesis Data Firehouse, and Amazon Simple Storage Service, introducing latencies and higher costs.

To simplify the process and increase processing speeds while decreasing costs, this guide will show you how to use the [Snowflake Snowpipe Streaming API](https://aws.amazon.com/about-aws/whats-new/2024/01/stream-data-snowflake-kinesis-data-firehose-snowpipe-streaming-preview/) to deliver records from Amazon Data Firehouse as soon as they are available, making data ready for querying in Snowflake within seconds. 

This guide describes how to use both the AWS Management Console configuration and the automated [AWS CloudFormation](https://aws.amazon.com/cloudformation/) deployment. The Infrastructure as Code (IaC) approach ensures consistent and scalable deployments while delivering a low-code, serverless, and cost-effective solution.

Benefits of this implementation include:

* Near real-time insights: Access up-to-date AMS data in Snowflake within mins
* Scalability: Handles increasing data volumes across multiple AMS datasets
* Maintainability: Easy updates through CloudFormation
* Simplified architecture: Direct streaming eliminates need for interim storage

## Requirements

This guide requires the following:

* **Amazon**  
    * [AWS account](https://portal.aws.amazon.com/billing/signup/iam?nc2=h_ct&redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation&src=header_signup#/support) and access to the following AWS services:
        * [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM)
        * Amazon Data Firehose
        * Amazon S3
        * Cloud Formation
    * Familiarity with the [AWS Management Console](http://aws.amazon.com/console)
    * An Amazon S3 bucket for error logging
* **Snowflake**  
    * [Snowflake account](https://signup.snowflake.com/)
        * If you are using PrivateLink, the Snowflake account should be the Business Critical Snowflake edition.
    * Snowflake user ID with `ACCOUNTADMIN` role
    * Key-pair authentication configured and a key pair assigned to the user. For more information, see the following articles in the Snowflake documentation:
        * [Generate the private key](https://docs.snowflake.com/en/user-guide/key-pair-auth#generate-the-private-key)
        * [Generate a public key](https://docs.snowflake.com/en/user-guide/key-pair-auth#generate-a-public-key)
        * [Store the private and public keys securely](https://docs.snowflake.com/en/user-guide/key-pair-auth#generate-a-public-key)
        * [Assign the public key to a Snowflake user](https://docs.snowflake.com/en/user-guide/key-pair-auth#generate-a-public-key)
        * [Verify the user’s public key fingerprint](https://docs.snowflake.com/en/user-guide/key-pair-auth#generate-a-public-key)

## Solution

1. [Create a Snowflake database, schema, and table](#step-1-create-a-snowflake-database-schema-and-table)
2. [Create a Firehose delivery stream](#step-2-create-a-firehose-delivery-stream)
3. [Configure IAM roles and policies for Firehose](#step-3-configure-iam-roles-and-policies-for-firehose)

![Amazon Marketing Insights Stream to Snowflake in Real Time](/_images/usage-examples/stream-to-snowflake-real-time.png)

### Step 1: Create a Snowflake database, schema, and table

Complete the following steps to set up your Snowflake database, schema, and table:

1. Log in to your Snowflake account and create the database:

```
create database db_name;
```

2. Create a schema in the new database:

```
create schema db_name.schema_name;
```

3. Create a table in the new schema:  

```
CREATE TABLE IF NOT EXISTS amazon_ads_traffic (
    idempotency_id VARCHAR NOT NULL,
    dataset_id VARCHAR NOT NULL,
    marketplace_id VARCHAR NOT NULL,
    currency VARCHAR(3),
    advertiser_id VARCHAR NOT NULL,
    campaign_id VARCHAR NOT NULL,
    ad_group_id VARCHAR NOT NULL,
    ad_id VARCHAR NOT NULL,
    keyword_id VARCHAR,
    keyword_text VARCHAR,
    match_type VARCHAR,
    placement VARCHAR,
    time_window_start VARCHAR,
    clicks NUMBER(38,0),
    impressions NUMBER(38,0),
    cost DOUBLE
);
```

### Step 2: Create a Firehose delivery stream

Complete the following steps to create a Firehose delivery stream with Kinesis Data Streams as the source and Snowflake as its destination:

1. In the Amazon Data Firehose console, select **Create Firehose** stream.
2. For **Source**, select **Direct PUT**.
3. For **Destination**, select **Snowflake**.
4. For **Firehose Stream Name**, enter a name or use the default generated name. ![Amazon Data Firehose Create Firehose](/_images/usage-examples/create-firehose.png)
5. In the **Destination settings > Connection Settings**, provide the following information to connect Amazon Data Firehose to Snowflake:
    * **Snowflake account URL**: Your Snowflake account URL. For example, `xy12345.us-east-1.aws.snowflakecomputing.com`. For more information, see [Snowflake: Connecting to your accounts](https://docs.snowflake.com/en/user-guide/organizations-connect).
    * **User**: Enter the Snowflake user ID.
    * **Private key**: Enter the private key generated in the prerequisites. Ensure the private key is in PKCS8 format. Do not include the PEM `header-BEGIN` prefix or `footer-END` suffix as part of the private key. If the key is split across multiple lines, remove the line breaks.
    * **Role**: Select the custom Snowflake role and enter the [custom Snowflake role](https://docs.snowflake.com/en/sql-reference/sql/create-role) that has access to write to the database table.
![Firehose destination settings](/_images/usage-examples/destination-settings.png)
![Firehose destination settings](/_images/usage-examples/destination-settings2.png)
![Firehose destination settings](/_images/usage-examples/destination-settings3.png)
    **Connection Access**  
    You can connect to Snowflake using public or private connectivity. If you don’t provide a VPC endpoint, the default connectivity mode is public. To allow list Firehose IPs in your Snowflake network policy, see [Choose Snowflake for Your Destination](https://docs.aws.amazon.com/firehose/latest/dev/create-destination.html#create-destination-snowflake). If you’re using a private link URL, provide the VPCE ID using [SYSTEM$GET\_PRIVATELINK\_CONFIG](https://docs.snowflake.com/en/sql-reference/functions/system_get_privatelink_config). For more information, see [Snowflake: AWS PrivateLink and Snowflake](https://docs.snowflake.com/en/user-guide/admin-security-privatelink).  
    The following function returns a JSON representation of the Snowflake account information necessary to facilitate the self-service configuration of private connectivity to the Snowflake service:
```
select SYSTEM$GET_PRIVATELINK_CONFIG();
```

6. In this guide we are not using private link, so we will keep **VPCE ID** empty.
7. Under **Database configuration settings**, enter your Snowflake database, schema, and table names.
8. In the **Backup settings > S3 backup bucket**, enter the bucket you created as part of the prerequisites.
9. Click **Create Firehose stream**.

### Step 3: Configure IAM roles and policies for Firehose

To set up Amazon Marketing Stream, create two essential IAM roles. Follow the links to create these roles and their associated policies, ensuring proper integration setup:

1. [Firehose subscription role](guides/amazon-marketing-stream/onboarding/firehose/manual-steps#step-2-create-firehose-subscription-role) - Allows SNS to deliver data to Firehose.
2. [Firehose subscriber role](guides/amazon-marketing-stream/onboarding/firehose/manual-steps#step-3-create-firehose-subscriber-role) - Enables Amazon Marketing Stream to create SNS subscriptions on behalf of you.

## Use AWS CloudFormation

Alternatively, you can use an AWS CloudFormation template to create the Firehose delivery stream with Snowflake as the destination rather than using the Amazon Data Firehose console:

1. Ensure you have the [requirements](#requirements) above.
2. Complete [Step 1:Create a Snowflake database, schema, and table](#step-1-create-a-snowflake-database-schema-and-table).
3. Download the AWS CloudFormation template. Go to [GitHub](https://github.com/amzn/ads-advanced-tools-docs/tree/main/amazon_marketing_stream) and download a copy of `Stream-Firehose-S3-CF-Template.yaml`.
4. Deploy the stack using AWS CloudFormation:
    1. Log in to AWS console.
    2. In the search bar, enter `CloudFormation`.
    3. From the CloudFormation screen, click **Create stack**.
    4. Under **Template Source**, select **Upload a template file** and upload the CloudFormation template.
    5. Fill in the required parameters, including Snowflake credentials and AMS dataset selection.
    6. Review and create the stack.
5. After the stack is created, use the [Amazon Ads API to subscribe](amazon-marketing-stream/openapi) to the relevant AMS topics and direct the data to your new Kinesis Data Firehose delivery stream.
6. Verify the data flow:
    * Check CloudWatch logs for any errors.
    * Query Snowflake to confirm data arrival for each AMS dataset.

```
select `*` `from` db_name`.schema_name``.table_name``;
```

## Summary

This solution empowers you with near real-time insights across multiple ad types and metrics, enabling near real-time data-driven decision-making and faster campaign updates.

Amazon Data Firehose provides a straightforward way to deliver data to Snowflake using the Snowflake Snowpipe Streaming API, letting you reduce costs and minimize latency to seconds. Additionally, by leveraging the Amazon CloudFormation template, you can quickly set up a robust, scalable, and efficient data pipeline for your Amazon Marketing Stream data.

## Next Steps

* Download the CloudFormation template from the Amazon AMS GitHub repository: [Customer.yml](https://github.com/amzn/ads-advanced-tools-docs/blob/main/amazon_marketing_stream/Stream_firehose_snowflake.yml) 
* Deploy the stack in your AWS account
* Configure your AMS subscriptions to use the new Kinesis Data Firehose stream
* Start exploring your near real-time advertising data in Snowflake

We're excited to see how you'll use this integration to drive your advertising strategies forward. If you have any questions or feedback, please don't hesitate to reach out to our support team.










