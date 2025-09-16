---
title: Data sets
description: How to use data sets in AMC
type: guide
interface: api
tags:
  - amc
  - adu
keywords: 
  - AMC
  - ADU
  - Advertiser Data Upload
  - data upload API 
  - data upload process 
  - Prepare data for upload 
  - Set up S3 bucket for   data uploads 
  - dataset creation and management 
  - data upload prerequisites
---
# Advertiser data upload

Advertiser data upload (ADU) allows advertisers to upload customizable datasets to an AMC instance. Advertiser uploaded datasets can include any combination of columns defined by the advertiser. ADU APIs on Amazon Ads are also referred to as ADU 2.0.

| [TILES] WARNING                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Data that was uploaded using the previous version of ADU (ADU 1.0) can no longer be queried.  To continue querying datasets that you had uploaded in ADU 1.0, delete the previously uploaded data, and re-upload using ADU 2.0. |

Advertiser uploaded datasets can include any combination of columns defined by the advertiser. Examples include (but are not limited to):

- Off-Amazon conversion events
- Store visits and in-store purchases
- Additional product metadata
- CRM Audience Segments
- Advertiser mapping tables (e.g., DSP Advertiser to Sponsored Ads Advertiser)

Datasets containing supported identifiers can be joined with AMC data to attribute off-Amazon events (purchases, sign-ups, store visits, etc.) to Amazon advertising events such as impressions and clicks.

> [NOTE] This guide serves as a reference for the high-level upload process, API endpoints, and features.  When uploading customer files, do not include customer identifiers, such as email address or phone number, in uploaded data besides the supported pre-hashed fields. **If your datasets contain raw customer identifiers, the upload will be rejected.** See [Standardization and hashing](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-prepare-data#standardization-and-hashing).

## Prerequisites

1. [Amazon Marketing Cloud Terms](https://advertising.amazon.com/marketing-cloud/terms/AMC_Terms.html) must be accepted (in the UI) prior to using the data upload feature. If Terms and Conditions are not accepted, all API calls will respond with a 401 error code and error message.
2. Access to the AMC API on Amazon Ads API. AMC Data upload is an API-only feature. Note that you need the "Campaign Management" and "Data Provider" scopes associated with your developer credentials (Client ID). For more information on getting started with Amazon Ads API, see [onboarding](guides/onboarding/overview).
3. An AWS account and permissions to [create an S3 bucket](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-s3-bucket) to store upload files.

## Data upload process overview

The high-level process to upload data to an AMC instance is described below.

### Step 1 – Prepare your data

Prepare the data to be uploaded to the AMC instance. For more information, refer to [Prepare data](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-prepare-data).

### Step 2 – Set up the S3 bucket

Create an S3 bucket, if not already created, and configure it for storing data files to be uploaded. For more information, refer to [Set up the S3 bucket](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-s3-bucket).

### Step 3 – Create datasets

Create a new dataset using the **Dataset API**. For more information, refer to [Data sets](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-sets).

### Step 4 - Upload data to the datasets

Upload data to the new dataset by using the **Upload API**. For more information, refer to [Data uploads](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-upload).

### Step 5 - Optionally, modify columns

If needed, use the **Data Set Column API** to modify columns in a dataset. For more information, refer to [Data set columns](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-column).
