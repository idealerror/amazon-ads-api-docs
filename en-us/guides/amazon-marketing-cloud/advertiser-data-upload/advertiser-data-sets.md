---
title: Datasets
description: How to use data sets in AMC
type: guide
interface: api
tags:
 - amc
keywords:
 - Data sets
 - Data schema
 - create table
 - isMainUserId
 - isMainUserIdType
 - DataSetId guidelines
 - dataset naming conventions
 - Country code setting
 - Country code setting row-level
 - Country code setting upload-level
 - Country code setting dataset-level
 - Consent management
 - TCF
 - GPP
 - ACS
 - Consent columns
 - tcf_string
 - gpp_string
 - amzn_user_data
 - amzn_ad_storage
 - Uploads API
---
# Create dataset

Creating a dataset defines the schema you will be uploading data to. Whenever data is uploaded, it is validated against the schema; if any rows/columns in the file do not conform to the schema, then the upload is rejected.

## Supported Methods

| Method                                                                 | Endpoint                                                                                                              | Description                                                                                                                                                                  |
| ---------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [POST](amc-advertiser-data-upload#tag/Data-Set/operation/CreateDataSet)   | [/amc/advertiserData/{instanceId}/dataSets](amc-advertiser-data-upload#tag/Data-Set/operation/CreateDataSet)             | Create new dataset                                                                                                                                                           |
| [GET](amc-advertiser-data-upload#tag/Data-Set/operation/GetDataSet)       | [/amc/advertiserData/{instanceId}/dataSets/{dataSetId}](amc-advertiser-data-upload#tag/Data-Set/operation/GetDataSet)    | Retrieve a specific dataset                                                                                                                                                  |
| [DELETE](amc-advertiser-data-upload#tag/Data-Set/operation/DeleteDataSet) | [/amc/advertiserData/{instanceId}/dataSets/{dataSetId}](amc-advertiser-data-upload#tag/Data-Set/operation/DeleteDataSet) | Deletes the specified data set. WARNING: This is an irreversible action. All data associated with the data set will be deleted and will no longer be available for querying. |
| [PUT](amc-advertiser-data-upload#tag/Data-Set/operation/UpdateDataSet)    | [/amc/advertiserData/{instanceId}/dataSets/{dataSetId}](amc-advertiser-data-upload#tag/Data-Set/operation/UpdateDataSet) | Update the description of a dataset                                                                                                                                          |
| [POST](amc-advertiser-data-upload#tag/Data-Set/operation/ListDataSets)    | [/amc/advertiserData/{instanceId}/dataSets/list](amc-advertiser-data-upload#tag/Data-Set/operation/ListDataSets)         | Retrieve a list of datasets                                                                                                                                                  |

### Create new dataset

Use `POST /amc/advertiserData/{instanceId}/dataSets` to create a data set with columns in it.

If created successfully, the response will return the `dataSetId` which you can use to later retrieve the dataset.

> [WARNING] Each instance has a maximum capacity of 5000 datasets.

#### Column guidelines

- Specify a column as DIMENSION if you expect to perform GROUP BY operations on the column.
- Specify a column as METRIC if you expect to perform SUM or AVERAGE operations on the column.
- Columns marked as `isMainUserId` and `isMainUserIdType` cannot be defined as a METRIC type.
- You may define columns flagged as `mainUserId` and `mainUserIdType` in any dataset containing external identity. If these columns are not defined, they will be added on your behalf with the names `user_id` and `user_id_type`. These columns will be populated by AMC when an identity match is found. Do not upload data that contains values for these two columns, as the values will be overwritten as identity matching is performed.

#### Guidelines for dataSetId

When selecting a `dataSetId`:

- The `dataSetId` will not accept spaces or uppercase characters; and must comprise only lowercase characters, numbers, with the underscore being the only special character allowed.
- Ensure all datasets created have unique names. Duplicate dataset names between sources will result in an error.
- You must use a unique dataSetID for when creating the new datasets. For example, if the datasets created using v1 had "dataSetId": "myfirstdataset", you must create the new dataset with a different name (for example, "dataSetId": "myfirstdatasetv2"). Refer to the [Migration guide](guides/amazon-marketing-cloud/migration#migrating-from-advertiser-data-upload-v1-to-advertiser-data-upload-apis-on-amazon-ads) for more information.
- In case of naming conflicts between advertiser and Amazon-owned datasets, resolution for customer queries are handled in the following order:

  - Advertiser-owned datasets
  - Amazon-owned datasets

#### Setting the country code

When creating a dataset, you can specify the country code at either:

* The row-level
* The upload-level
* The dataset-level

> [WARNING] The row-level country code value will override the value set at the upload-level or dataset-level, if they are in conflict. For more about priority orders and considerations for setting the country code, refer to [Setting country codes](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-prepare-data#setting-country-codes).

##### At row-level

You can set the country code in each row of a dataset. To do this, define a single column in the dataset with the isCountryCode = true setting.

> [NOTE] When you specify a `isCountryCode` column in the dataset, this will flag the column as the one that will contain the country codes for the records uploaded, but it does not set the values of the country codes. You will do that in the files you upload by including the country code column in your files and setting each row to the appropriate 2-character string in the ISO 3166 format.

```
...
"columns":[
  {
    "columnType":"DIMENSION",
    "isCountryCode":true,
    "nullable":true,
    "dataType":"STRING",
    "name":"country_code",
    "description":"country code for each row"
  }
]
...
```

###### At upload-level

To learn more about setting the country code at the upload-level, refer to [Setting the country code at upload-level](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-upload#setting-the-country-code-at-upload-level).

###### At dataset-level

You can specify the country code for a dataset, which will apply the country code to all records uploaded to a dataset (unless there are row-level or upload-level country codes that are in conflict). For more information about country code conflicts and the priority order, refer to [Setting country codes](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-prepare-data#setting-country-codes).

You can add a new country code column to an existing dataset, or define the column when creating a new dataset. You can not update an existing column in an existing dataset to be the country code column.

To add the country code at the dataset-level, add the following attribute to the dataset:

```
"countryCode" : "US"
```

> [NOTE] This is an example of setting the country code to US for United States. Replace US with the appropriate 2-character string in the ISO 3166 format.

```
{
  "dataSet":{
      "period":"P1M",
      "dataSetId":"datasetid",
      "columns":[
          ...
        ],
    "countryCode":"US",
    "customerEncryptionKeyArn":"...",
    "description":"datasetdescription"
  }
}
```

#### Specifying user privacy choices

AMC has begun accepting consent choices from advertisers (and third-party)  uploading or sharing their customer data with Amazon Ads. For more details, see [Consent Management in AMC.](guides/amazon-marketing-cloud/amc_consent_management)

For records belonging to the EEA and UK regions, uploaded datasets, must include at least one column defined with a `consentType`.
The `consentType` can be one of three options, each requiring your payload structure to include columns as listed below:

| consentType | Number of columns to add | Value                                     |
| ----------- | ------------------------ | ----------------------------------------- |
| TCF         | 1                        | tcf\_string                               |
| GPP\_String | 1                        | gpp\_string                               |
| ACS         | 1 or 2                   | amzn\_user\_data and\or amzn\_ad\_storage |

> [NOTE] For ACS, both `consentType` parameters (`amzn_user_data`  and `amzn_ad_storage`) expect values: “GRANTED”  or “DENIED”. For ACS `consentType`, you may define one of the two columns\. If you provide data for only one column, AMC will apply defaults to the non\-existent column\.

**Example dataset request**

> [NOTE] To provide multiple `consentType` columns your payload must include columns for each of the above types.

The following sample request payload creates a dataset with Id “my-dataset-with-consent” with `consentType` = TCF.

**Sample request payload**

```
{
    "dataSet": {
        "period": "P1D",
        "dataSetId": " my-dataset-with-consent",
        "columns": [
            {
                "columnType": "DIMENSION",
                "dataType": "STRING",
                "externalUserIdType": {
                    "hashedPii": "EMAIL"
                },
                "name": "email"
            },
            {
                "columnType": "DIMENSION",
                "dataType": "DATE",
                "isMainEventTime": true,
                "name": "record_date"
            },
            {
                        "columnType": "DIMENSION",
                        "dataType": "STRING",
                        "name": "tcf_string",
                        "consentType": "TCF"
                  },
            ]
        "countryCode": "DE",
        "customerEncryptionKeyArn": "---",
        "description": "Dataset containing 3P consent information."
    }
}
```

**Example dataset request**

The above example creates a dataset with `dataSetId` as "my-dataset-with-consent". In our example, the columns: `email`, `record_date`, and `tcf_string` will be the columns that form the table. The corresponding input .csv file is depicted below:

| email               | record\_date | tcf\_string                                                                                                                                                                                                                                                                                             |
| ------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| example1@gmail\.com | 6/15/2024    | `CQB4ySFQB4ySFBlAAAENCZCAAAAAAAAAAAAAAAAAAAAA\.I7Nd\_X\_\_bX9n\-\_7\_6ft0eY1f9\_r37uQzDhfNs\-8F3L\_W\_LwX32E7NF36tq4KmR4ku1bBIQNtHMnUDUmxaolVrzHsak2cpyNKJ\_JkknsZe2dYGF9Pn9lD\-YKZ7\_5\_9\_f52T\_9\_9\_\-39z3\_9f\_\_\_dv\_\-\_\_\-vjf\_599n\_v9fV\_78\_Kf9\_\_\_\_\_\_\-\_\_\_\_\_\_\_\_\_\_\_\_8A` |

> [NOTE] You only need to provide one of the three consent formats. When multiple consent formats are shared, Amazon evaluates them in order of priority, with TCF having the highest priority, followed by GPP, and then ACS.

**Sample construct for GPP format**

```
{
    "columnType": "DIMENSION",
    "dataType": "STRING",
    "name": "gpp_string",
    "consentType": "GPP"
}
```

**Sample construct for ACS format**

```
{
        "columnType": "DIMENSION",
        "dataType": "STRING",
        "name": "AMAZON_AD_STORAGE",
        "consentType": "amzn_ad_storage"
    },
    {
        "columnType": "DIMENSION",
        "dataType": "STRING",
        "name": "AMAZON_USER_DATA",
        "consentType": "amzn_user_data"
    }
```

> [NOTE] If you are uploading data that does not involve cookies then the `amzn_ad_storage` value can be left as NULL.

After the dataset is created, you can upload data to the new dataset using the Uploads API, specifically [POST /amc/advertiserData/{instanceId}/uploads/list](amc-advertiser-data-upload#tag/Upload/operation/CreateUpload).

### Retrieve a specific dataset

To view the details of a specific dataset, use the following endpoint passing the `dataSetId` as the path parameter.

```
GET /amc/advertiserData/{instanceId}/dataSets/{dataSetId} 
```

**Sample response**

For the example dataset created above, the response would return the following:

```
{
  "dataSet": {
    "columns": [
      {
        "name": "product_asin",
        "columnType": "DIMENSION",
        "description": "ASIN of the product",
        "dataType": "STRING"
      },
      {
        "name": "product_sku",
        "columnType": "METRIC",
        "description": "SKU of the product",
        "dataType": "STRING"
      }
    ],
    "dataSetId": "myfirstdataset",
    "description": "My dataset"
  }
}
```

### Delete a specific dataset

To delete a specific dataset, use the following endpoint with the dataSetId as the path parameter.

```
DELETE /amc/advertiserData/{instanceId}/dataSets/{dataSetId}
```

A successful response will return the `dataSetId` of the dataset that was deleted along with `instanceId` that contained the dataset. Once deleted, you will not be able to query the dataset.

**Sample response**

```
{
  "dataSetId": "myfirstdataset",
  "instanceId": "amca0a00aaa"
}
```

> [WARNING] The delete operation is permanent. You cannot recover uploaded data once the dataset is deleted. Perform a delete operation with **extreme caution**.

### Update the description of a dataset

After a dataset is created, you can only modify the description of the dataset. Pass the `instanceId` and the `dataSetId` as path parameters and specify the updated values as the request body payload.

```
PUT /amc/advertiserData/{instanceId}/dataSets/{dataSetId} 

```

**Sample request body**

```
{
  "description": "An updated description for my first dataset"
}
```

A successful response returns the details of the updated dataset with the updated description.

**Sample response**

```
{
  "dataSet": {
    "columns": [
      {
        "columnType": "METRIC",
        "dataType": "STRING",
        "description": "ASIN of the product",
        "isMainEventTime": false,
        "isMainUserId": false,
        "isMainUserIdType": false,
        "name": "product_asin",
        "nullable": true,
        "requiresOneWayHashing": false
      },
      {
        "columnType": "METRIC",
        "dataType": "STRING",
        "description": "SKU of the product",
        "isMainEventTime": false,
        "isMainUserId": false,
        "isMainUserIdType": false,
        "name": "product_sku",
        "nullable": true,
        "requiresOneWayHashing": false
      }
    ],
    "dataSetId": "myfirstdataset",
    "description": "An updated description for my first dataset"
  }
}
```

### Retrieve a list of datasets

To list all the datasets defined in an instance, use the following endpoint, with the `instanceId` as a path parameter.

```
POST /amc/advertiserData/{instanceId}/dataSets/list
```

#### Sample response

```
{
  "nextToken": "{token}",
  "dataSets": [
    {
      "columns": [
        {
          "columnType": "METRIC",
          "dataType": "STRING",
          "description": "ASIN of the product",
          "isMainEventTime": false,
          "isMainUserId": false,
          "isMainUserIdType": false,
          "name": "product_asin",
          "nullable": true,
          "requiresOneWayHashing": false
        },
        {
          "columnType": "METRIC",
          "dataType": "STRING",
          "description": "SKU of the product",
          "isMainEventTime": false,
          "isMainUserId": false,
          "isMainUserIdType": false,
          "name": "product_sku",
          "nullable": true,
          "requiresOneWayHashing": false
        }
      ],
      "dataSetId": "myfirstdataset",
      "description": "An updated description for my first dataset"
    },
    {
      ...
    }
  ]
}
```

## Next steps

After the dataset is created, you can upload data to the new dataset using the [Uploads API](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-upload).
