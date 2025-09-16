---
title: Advertiser audiences
description: Advertiser audiences using advertiser uploaded data
type: guide
interface: api
---

# Create advertiser audiences with Amazon ADU APIs

The Advertiser Data Upload (ADU) APIs enable you to create first-party audience segments in Amazon Marketing Cloud (AMC) using advertiser audiences. The audiences created in AMC are activated in the Amazon Demand-Side Platform (Amazon DSP) from where you can use them for your your targeting needs. 


## Before you begin

- Ensure you have access to AMC and have completed [Amazon Ads API onboarding](guides/amazon-marketing-cloud/get-started/get-started#onboard-the-amazon-ads-api).
- Familiarize yourself with [ADU APIs](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-overview) and [set up an S3 bucket](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-s3-bucket). 
- Gain a thorough understanding of [advertiser audiences](guides/amazon-marketing-cloud/audiences/audience-management-service) and its requirements.


### Rule-based audience with Advertiser audiences

This tutorial helps you with the creation of advertiser audiences using your proprietary first-party customer data uploaded through the Advertiser Data Upload (ADU) APIs. 
 
**Choosing between advertiser audiences APIs and S3 Upload APIs**

The choice between advertiser audiences APIs and S3 Upload (via the AMC Advertiser Data Upload API) APIs depends largely on your organization's role, technical capabilities, and data management needs.

The advertiser audiences API is particularly well-suited for SaaS providers, especially Customer Data Platforms (CDPs), along with agencies and consultancies that need to build permanent, "always-on" data integrations with Amazon Ads. This API is also appropriate for direct advertisers who have the technical resources and capability to build and maintain an ongoing API integration. Organizations choosing this route should be prepared for continuous management and have dedicated technical teams to support the integration. The Advertiser Audience APIs can create an audience in Amazon DSP by default, with no further alterations by the user in AMC.

However, the S3 Upload APIs are often a better choice if your primary need is to upload first-party signals related to sales, conversions, or other non-audience metrics and do further filtering or transformation. This API is also more appropriate if you plan to manage your data uploads on an ad hoc or periodic basis rather than requiring continuous synchronization. It offers more flexibility for data types and requires less technical overhead to maintain if performing infrequent or ad-hoc uploads.

| Factor              | Advertiser audiences API | S3 Upload API 
|---------------------|--------------------------|----------------------------|               
| Integration type    | Permanent                | Flexible                   |
| Upload frequency    | Continuous               | As needed                  |
| Use case            | Audience management      | Open-ended data types      |

### Process overview
1. [Create a dataset definition](#step-1-create-dataset-definition)
2. [Upload hashed customer data](#step-2-upload-hashed-customer-data)
3. [Set the country code](#step-3-set-the-country-code)
4. [Create a rule-based audience](#step-4-create-rule-based-audience)

#### Detailed steps

#### Step 1: Create dataset definition

Creating a dataset defines the schema that you will be uploading data to. Each time data is uploaded through the ADU APIs, it is validated against the schema you define. If any rows or columns in the file do not conform to the schema, the upload will be rejected. You can use the `/amc/advertiserData/{instanceId}/dataSets` API endpoint to create a new dataset.

#### Step 2: Upload hashed customer data

After you create a dataset definition, you can upload hashed first-party customer data to AMC. To do this, you'll need to [prepare the data](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-prepare-data) following required guidelines. Once you have prepared the data, you can use the steps described to [upload the prepared data](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-upload). After the upload, AMC will match the hashed users to Amazon users and populate the `user_id` column accordingly. In addition, AMC will remove all other data from a row once a `user_id` has been assigned to that row.

#### Step 3: Set the country code

When creating a dataset, you can specify the country code either at the row-level or dataset-level. You also have the option of setting the country code on the upload level. To learn more about setting the country code at the upload-level, refer to [Setting the country code](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-sets#setting-the-country-code). 

>[WARNING]  While the country code is an optional field, it is highly recommended that you specify a country code when uploading data to improve the match rate with Amazon users. The row-level country code value will override the value set at the upload-level or dataset-level, if they are in conflict. 

#### Step 4: Create rule-based audience

After you create the dataset and upload the customer data, you can [create a rule-based audience](guides/amazon-marketing-cloud/audiences/rule-based-audiences) and push it to Amazon DSP. 
