---
title: Data uploads
description: How to use data uploads in AMC
type: guide
interface: api
---
# Upload data to datasets

The Upload API is used to manage the transfer of data to the dataset.

> [NOTE] When uploading, we recommend that individual files do not exceed 1GB.

Before you begin creating uploading data, thoroughly read through [Prepare data](guides/amazon-marketing-cloud/advertiser-data-upload/prepare-data) to understand guidelines and requirements for the files and data you will upload.

| [TILES] WARNING                                                                                                                                                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Data that was uploaded using ADU 1.0 can no longer be queried.  To continue querying datasets that you had uploaded in ADU 1.0, delete the previously uploaded data, and re-upload using ADU 2.0. |

### Supported Methods

| Method                                                            | Endpoint                                                                                                                  | Description                                          |
| ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| [POST](amc-advertiser-data-upload#tag/Upload/operation/CreateUpload) | [/amc/advertiserData/{instanceId}/uploads/{dataSetId}](amc-advertiser-data-upload#tag/Upload/operation/CreateUpload)         | Create new upload                                    |
| [POST](amc-advertiser-data-upload#tag/Upload/operation/ListUploads)  | [/amc/advertiserData/{instanceId}/uploads/list](amc-advertiser-data-upload#tag/Upload/operation/ListUploads)                 | Retrieve a paginated list of uploads for an instance |
| [GET](amc-advertiser-data-upload#tag/Upload/operation/GetUpload)     | [/amc/advertiserData/{instanceId}/uploads/{dataSetId}/{uploadId}](amc-advertiser-data-upload#tag/Upload/operation/GetUpload) | View status and other details of an upload operation |

### Creating an upload

After you have [created a dataset](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-sets), upload data to the dataset using the following endpoint with the `dataSetId` in the path, as well as the request body.

```
POST /amc/advertiserData/{instanceId}/uploads/{dataSetId}
```

**Sample request body**

```
{
  "updateStrategy": "ADDITIVE",
  "countryCode": "US",
  "compressionFormat": "GZIP",
  "dataSource": {
    "sourceS3Bucket": "S3Bucket",
    "sourceFileS3Key": "S3Key"
  },
  "fileFormat": {
    "csvDataFormat": {
      "fieldDelimiter": ","
    }
  }
}
```

A successful call will return the status of the upload and an `uploadId`.

**Sample response**

```
{
  "status": "PENDING",
  "uploadId": "5f384873-1234-1234-1234-3738f7eb74ae"
}

```

#### S3 Bucket source

This Create Upload API call requires you to specify where the data is stored to be uploaded.

1. First, you will specify the **sourceS3Bucket**. This is the S3 bucket name.
2. Then you will specify which files or objects to upload. Your options include:

    - **sourceFileS3Key**: Used to specify a single file (for example, `"sourceFileS3Key" : "uploadFile.csv.gz"`) in the  **sourceS3Bucket** to upload to AMC.
    - **sourceS3Prefix**: Used to specify multiple S3 objects rooted at a single S3 prefix. Learn more about [S3 prefixes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-prefixes.html).
    - **sourceManifestS3Key**: Used to specify a manifest file, which is a file that points to multiple, distinct S3 objects. For example, you could specify the manifest file `"sourceManifestS3Key" : "manifestForAMCUpload.txt"` which points to other objects. See below for an example manifest file format.

**Manifest file format**

The following is the format of a manifest file.

```
s3://<s3bucketname1>/<pathKey1>/<fileName1>
s3://<s3bucketname2>/<pathKey2>/<fileName2>
s3://<s3bucketname3>/<pathKey3>/<fileName3>
s3://<s3bucketname4>/<pathKey4>/<fileName4>
```

> [NOTE] Know that the file names of files you are uploading in the `sourceS3Bucket`, irrespective of the key type provided (`sourceFileS3Key`, `sourcePrefixKey`, or `sourceManifestS3Key`), cannot start with a . (period character) or _ (underscore character).

#### Update strategies

The `updateStrategy` parameter allows you to specify how the new data will be merged with existing data in the dataset. The valid options are:

- ADDITIVE
- FULL_REPLACE
- OVERLAP_REPLACE
- OVERLAP_KEEP



| [IMAGE]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |                                                                                       |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| **ADDITIVE**<br />**For fact datasets**: This strategy adds new records to records that may already be present in each time-based partition of the table. If the table already contains records, then the (new) record set being uploaded may overlap with the existing records. These overlaps are partition overlaps. <br /><br />For example, if the table contains time-based data that is partitioned on month, and the table already holds data for the months January, February, March, then when a new upload containing data for March, April, May is performed with the ADDITIVE strategy, then records that will fall within the March partition will overlap with a partition where data already exists. | ![Additive strategy for fact](/_images/amazon-marketing-cloud/StrategyAdditiveFact.png) |


<br/><br/>

| [IMAGE]                                                                                                                                                                                                                   |                                                                                            |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| **ADDITIVE**<br />**For dimension datasets**: Dimension datasets are a single partition of records. This strategy, when used with a dimension dataset, will add the uploaded records to the existing records. | ![Additive strategy for fact](/_images/amazon-marketing-cloud/StrategyAdditiveDimension.png) |

<br/><br/>

| [IMAGE]                                                                                                                         |                                                                                              |
| ------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| **FULL_REPLACE**<br />**For fact datasets**: The uploaded data replaces all records previously saved in the tables. | ![Full replace strategy for fact](/_images/amazon-marketing-cloud/StrategyFullReplaceFact.png) |

<br/><br/>

| [IMAGE]                                                                                                                                                                                                                 |                                                                                                   |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| **FULL_REPLACE**<br />**For dimension datasets**: Dimension datasets have a single partition of records. This strategy will replace all existing records in that partition with the records to be uploaded. | ![Full replace strategy for fact](/_images/amazon-marketing-cloud/StrategyFullReplaceDimension.png) |

<br/><br/>

| [IMAGE]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |                                                                                                    |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| **OVERLAP_REPLACE**<br />**This strategy applies for uploads to fact datasets only.** When a data upload is performed with OVERLAP_REPLACE as the strategy, then new data will be added to table partitions. Any overlapping partitions will be removed and **replaced** with new content. <br /><br />For example, previously uploaded monthly data contains records in the February, March, and April partitions. Data being uploaded should fall into April and May. With this option, any prior events in April will be **removed**, and replaced with the data for April from the new upload, and May will be added directly, as there is no overlap with existing data. | ![Overlap replace strategy for fact](/_images/amazon-marketing-cloud/StrategyOverlapReplaceFact.png) |

<br/><br/>

| [IMAGE]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                                                                                 |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| **OVERLAP_KEEP**<br />**This strategy applies for uploads to fact datasets only.** All new data for non-overlapping partitions will be added to the table. Any overlapping partitions will retain their original data. When the upload overlaps with any partitions that already have data, the original data of the overlapping partition(s) is RETAINED, and those in the upload are ignored. For example, if the dataset has previously uploaded monthly data, in the February, March, and April partitions. A new upload containing data for April and May is performed. When uploaded with the OVERLAP_KEEP as the upload strategy, any existing data for April will be retained, and those corresponding to April in the new upload will be **ignored**. Data for May will be added directly, as there is no overlap with the existing data. | ![Overlap replace strategy for fact](/_images/amazon-marketing-cloud/StrategyOverlapKeepFact.png) |

<br/><br/>

#### Setting the country code at upload-level

You can specify the country code for an upload.

> [NOTE] You can also set the country code on the row-level or dataset-level. Refer to [Setting the country code (row-level or dataset-level)](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-sets#setting-the-country-code-row-level-or-dataset-level).

Add the following attribute to the upload creation:

```
"countryCode" : "US"
```

> [NOTE] This is an example of setting the country code to `US`. Replace US with the appropriate 2-character string in the ISO 3166 format.

```
{
  "updateStrategy": "ADDITIVE",
  "countryCode": "US",
  "compressionFormat": "BZIP",
  "dataSource": {
    ...
  },
  "fileFormat": {
    ...
  }
}
```

### View all uploads

Returns a list of all uploads, as well as the upload details.

```
POST /amc/advertiserData/{instanceId}/uploads/list
```

**Sample response**

```
{
  "uploads": [
    {
      "createdAt": "2023-09-29T17:36:32Z",
      "metrics": {
        "OutputRowCount": 1782332
      },
      "status": "COMPLETED",
      "updatedAt": "2023-09-29T17:51:30Z",
      "uploadId": "2dd474b2-1234-1234-1234-eafe9ccc216c"
    },
    {
      "createdAt": "2023-09-29T17:58:45Z",
      "message": "No such file or directory 's3://myS3bucket'",
      "metrics": {},
      "status": "FAILED",
      "updatedAt": "2023-09-29T18:04:26Z",
      "uploadId": "c83cd17b-1234-1234-1234-db6424aab1c3"
    }
  ]
} 

```

### View a specific upload

To get details about a specific upload, use the following operation:

```
GET /amc/advertiserData/{instanceId}/uploads/{dataSetId}/{uploadId}
```

**Sample response**

```
{
  "upload": {
    "createdAt": "2023-09-29T17:36:32Z",
    "metrics": {
      "OutputRowCount": 1782332
    },
    "status": "COMPLETED",
    "updatedAt": "2023-09-29T17:51:30Z",
    "uploadId": "2dd474b2-1234-1234-1234-eafe9ccc216c"
  }
}

```

## Viewing and using the uploaded dataset in the AMC UI

| [IMAGE]                                                                                                                                                                                                                                                                                                                                                                                |                                                                                                 |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Once the upload status is returned as COMPLETED, you will be able to see the uploaded data in the Advertiser uploaded section of the Schema explorer.<br /><br />You will also be able to run SQL queries using the dataset both in the Query Editor in the AMC UI and using the [Workflow management service APIs](guides/amazon-marketing-cloud/reporting/workflow-management-service). | ![Advertiser uploaded data AMC UI](/_images/amazon-marketing-cloud/amcAdvertiserUploadedData.png) |

## Next steps

If you need to modify columns in the dataset, refer to [Data set columns API](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-column).
