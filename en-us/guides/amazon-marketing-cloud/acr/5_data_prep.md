---
title: Amazon Marketing Cloud - AWS Clean Rooms
description: AMC - AWS Clean Rooms
type: guide
interface: api
---
# Data upload essentials

To run measurement and analysis queries in your AMC collaboration instance, you must first bring your data from your source to the collaboration instance.

> [WARNING] Before you upload data, ensure you read and comply with the terms listed in the [Amazon Marketing Cloud Agreement](https://advertising.amazon.com/marketing-cloud/terms/AMC_Terms.html), [Amazon Ads API License Agreement](https://advertising.amazon.com/API/docs/license-agreement) and in the [Data Protection Policy](https://advertising.amazon.com/API/docs/policy/en_US). **Any GDPR un-consented event data should not be brought into the collaboration**.

You can bring data to your collaboration instance in one of the following ways:

- [Bring event data into the collaboration through AWS Clean Rooms](guides/amazon-marketing-cloud/acr/6_upload_event)
- [Bring identity data to perform rule-based matching for identity resolution](guides/amazon-marketing-cloud/acr/7_upload_identity)
- [Upload data using AMC Data Upload ](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-overview)

> [NOTE] While AMC Data Upload allows you to bring in data to your collaboration instance, we recommend that you use the first two options instead.

## Prerequisites for data preparation

Before bringing in data (event or identity), to associate with your collaboration instance, ensure your data sets meet the following prerequisites:

- All datasets created have unique names. Duplicate dataset names will result in an error.
- When naming datasets, do not include any spaces, special characters, or uppercase characters. Your dataset name must comprise only lowercase characters, and/or numbers.
- Your event tables must not contain PII data.
- 3P identifiers should not be hashed.

## Consent management essentials

 AMC - AWS Clean Rooms  have begun accepting consent choices from advertisers (and third-parties)  uploading or sharing their customer data with Amazon Ads. For more details, see [Consent Management in AMC.](guides/amazon-marketing-cloud/amc_consent_management)

For data brought into AMC - AWS Clean Rooms collaboration instances, advertisers must define the specific columns for consent data using column names as indicated here:

| Column name                | Description                                                                        | Data type |
| -------------------------- | ---------------------------------------------------------------------------------- | --------- |
| `country`                | The column containing consent information formatted as a TCF string\.              | String    |
| `tcf`                    | The column containing consent information formatted as a TCF string\.              | String    |
| `gpp`                    | The column containing consent information formatted as a GPP string\.              | String    |
| `amz_user_data_consent`  | The column containing the user data consent portion of ACS  consent information\.  | String    |
| `amz_ad_storage_consent` | The column containing the ad storage consent portion of ACS consent  information\. | String    |

> [NOTE] For the two ACS columns (`amz_user_data_consent` and `amz_ad_storage_consent`),  only the string "GRANTED " is considered positive consent.

AMC - AWS Clean Rooms will only recognize and process these special columns if they match the specified data type. For instance, a column named `tcf_consent` must be of string type to be considered valid and processed as consent data.

When processing datasets containing user IDs, the consent is evaluated for each row individually, and any rows that fail to meet the consent requirements will be excluded from the query.

> [WARNING] Starting November 6, 2024, AMC - AWS Clean Rooms has begun accepting consent signals. 

The consent signals will be processed as below:

- If no consent columns are  provided, each row will be considered consented.
- If one or more consent columns  are provided but all values in these columns are null for a given row, that  row will be considered consented.
- In all cases other than the two above  cases, consent will be processed as outlined in the next section (when  consent information is mandatory).

Consent will be calculated for each row in the following descending order of priority:

**Country  code**

- If a country code column is provided the rows with country codes that don't require consent checks are automatically considered consented.
- If no country code column is provided, the all rows are treated as if they are from countries that require consent.

**Consent  type**

- If no consent columns are defined at all, each row is  considered not consented.
- Consent is evaluated using the highest-priority consent  type with non-null values. The priority order will be considered in the  descending priority order as below:
  - TCF
  - GPP
  - ACS
    - For ACS, the `amz_ad_storage_consent` column and its values are considered optional.
    - When evaluating AMC consent, the `amz_user_data_consent` column must be provided and non-null. If `amz_ad_storage_consent` is missing or null, it is treated as GRANTED.

## Prerequisites to allow creating joins between event and identity

In addition to these [prerequisites](https://docs.aws.amazon.com/clean-rooms/latest/userguide/prepare-data), ensure that your data sets meet the following additional guidelines to enable joins between your event and identity data:

- The **event** tables must have a column titled `customer_user_id` of datatype string, which is the foreign key that will map with the unique key column in your **identity** dataset. In our [sample tables](#sample_tables), the unique key column in your identity dataset is titled `customerId`.

## Considerations

For queries that perform identity joins between your data and Amazon data, to work correctly in a collaboration instance, you should have **only one mapping table**.

When running queries, note these [limitations](guides/amazon-marketing-cloud/acr/limitations) in addition to considerations listed below.

- Only one mapping table is allowed.
- When your data is associated with your collaboration instance, the `customer_user_id` and an additional column name `user_id` will appear in your schema in the AMC UI.
- For performing identity joins between advertiser connected data and Amazon data, use the `user_id` column in your queries as indicated in the following example.

```
SELECT advertiser,
Count(1)
FROM dsp_impressions a
INNER JOIN sample_event_data c
ON a.user_id = c.user_id
GROUP  BY 1 
```

## Sample tables for reference

Example `sampleidentity` data for reference

| customerid | firstname | lastname | email | phone | address | city | state | zip | country |
| ---------- | --------- | -------- | ----- | ----- | ------- | ---- | ----- | --- | ------- |
| 1          | john      | …       | …    | …    | …      | …   | …    | …  | …      |
| 2          | sue       | …       | …    | …    | …      | …   | …    | …  | …      |
| 3          | bob       | …       | …    | …    | …      | …   | …    | …  | …      |
| 4          | nancy     | …       | …    | …    | …      | …   | …    | …  | …      |

> [WARNING] In the above identity table, the column customerId is the unique column in the table. If the table has more than 25 rows for the same customerId, only 25 rows will be considered during the mapping process. Any additional rows beyond this 25-row threshold for a given customerId will be excluded from the mapping operation.

Example `sampleevent` data for reference

| customer\_user\_id | eventtime | eventtype | productid | productquantity | totalcost |
| ------------------ | --------- | --------- | --------- | --------------- | --------- |
| 1                  | …        | purchase  | 111       | 1               | \$10.00   |
| 1                  | …        | purchase  | 222       | 3               | \$15.65   |
| 2                  | …        | purchase  | 333       | 5               | \$13.22   |
| 3                  | …        | addtocart | 333       | 7               | \$11.47   |
