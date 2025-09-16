---
title: Advertiser Data Upload - examples
description: Advertiser Data Upload - examples
type: guide
interface: api
---

# Appendix

## Examples

### Create Time-based (Fact) Dataset with Hashed Identifiers (Email)

Below is an example to create a time-based, or fact, dataset using an EMAIL hashed identifier.

You can define the `user_id` and `user_id_type` columns in a dataset containing external identity (including hashed identifiers). Providing these columns is optional; they will be automatically added to the dataset by default if they are not provided. These columns will be populated by AMC when a match is found with a hashed record in the uploaded data. Do not include these columns in the uploaded data file(s). For more information, refer to [ID resolution and supported identifiers](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-process-overview#id-resolution-and-supported-identifiers).

Any combination of hashed identifier columns can be included on the datasets. For each hashed identifier column, the `hashedPii` should correspond with the type of identifier for the column. See [Hashed Identifier Column Options](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-process-overview#hashed-identifier-column-options) for all supported identifier types.


#### Create the dataset

The following example creates a dataset for the instance ID, amc12345678.

Some things to note about this dataset definition:

- The `hashed_email` column will contain the hashed PII (email) indicated by:

```
  "externalUserIdType": {
    "hashedPii": "EMAIL"
  }
```

- The `purchase_time` column will contain `"isMainEventTime": true`, which indicates that this is a fact dataset.

**API Endpoint**

```
POST /amc/advertiserData/amc12345678/dataSets
```

**Request body**

```
{
  "dataSetId": "hashed_identifier_dataset",
  "description": "Hashed data upload",
  "period": "P1D",
  "columns": [
    {
      "name": "user_id",
      "description": "The customer's resolved id",
      "columnType": "DIMENSION",
      "dataType": "STRING",
      "nullable": true,
      "isMainUserId": true
    },
    {
      "name": "user_type",
      "description": "The customer's resolved id type",
      "columnType": "DIMENSION",
      "dataType": "STRING",
      "nullable": true,
      "isMainUserIdType": true
    },
    {
      "name": "hashed_email",
      "description": "The user's hashed email address",
      "dataType": "STRING",
      "columnType": "DIMENSION",
      "nullable": true,
      "externalUserIdType": {
        "hashedPii": "EMAIL"
      }
    },
    {
      "name": "product_name",
      "description": "e.g., Product A, Product B.",
      "dataType": "STRING",
      "columnType": "DIMENSION"
    },
    {
      "name": "purchase_quantity",
      "description": "The number of products purchased by the user",
      "dataType": "LONG",
      "columnType": "METRIC"
    },
    {
      "name": "purchase_time",
      "description": "When the products were purchased by the customer",
      "dataType": "TIMESTAMP",
      "columnType": "METRIC",
      "isMainEventTime": true
    }
  ]
}
```

#### Start the upload

Next, create a new upload by specifying in the path the instance ID, amc12345678, and dataSetId to which you want to upload.

**API Endpoint**

```
POST /amc/advertiserData/amc12345678/uploads/hashed_identifier_dataset
```

**Request body**

```
{
  "updateStrategy": "ADDITIVE",
  "compressionFormat": "GZIP",
  "dataSource": {
    "sourceS3Bucket": "mys3bucket",
    "s3ObjectKey": "filename.csv.gz"
  },
  "fileFormat": {
    "csvDataFormat": {}
  }
}
```

**Response**

The response will include an upload ID:

```
{
  "status": "SUBMITTED",
  "uploadId": "a11aa11a-1234-1234-1234-ee6555eee5e5"
}
```

#### Check Upload Status

The **uploadId** can be used to check the status.

>[NOTE] We recommend that you wait for up to a minute after creating an upload before you query the status.

**API endpoint**

```
GET /amc/advertiserData/amc12345678/uploads/hashed_identifier_dataset/a11aa11a-1234-1234-1234-ee6555eee5e5
```

**Response**

```
{
  "upload": {
    "createdAt": "2023-09-29T17:36:32Z",
    "instanceId": "amc12345678",
    "dataSetId": "hashed_identifier_dataset",
    "dataSource": {
      "s3Bucket": "mys3bucket",
      "s3ObjectKey": "filename.csv.gz"
    },
    "metrics": {
      "OutputRowCount": 1782332
    },
    "status": "SUCCESS",
    "updatedAt": "2023-09-29T17:51:30Z",
    "uploadId": "a11aa11a-1234-1234-1234-ee6555eee5e5"
  }
}

```

### List all uploads for an instance

The following API request can be used to list all upload associated with an instance. Note that you can filter the list on other parameters such as `status` and creation timestamp. Refer to the [API specification](amc-advertiser-data-upload#tag/Upload/operation/ListUploads) for more information.

**API endpoint**

```
POST /amc/advertiserData/amc12345678/uploads/list
```

**Response**

```
{
  "uploads": [
    {
      "createdAt": "2023-09-29T17:36:32Z",
      "dataSetId": "hashed_identifier_dataset",
      "dataSource": {
        "s3Bucket": "mys3bucket",
        "s3ObjectKey": "filename.csv.gz"
      },
      "instanceId": "myInstance",
      "metrics": {
        "OutputRowCount": 1782332
      },
      "status": "SUCCESS",
      "updatedAt": "2023-09-29T17:51:30Z",
      "uploadId": "a22aa22a-1234-1234-1234-ee6555eee5e6"
    },
    {
      "createdAt": "2023-09-29T17:58:45Z",
      "dataSetId": "mydataset",
      "dataSource": {
        "s3Bucket": "mybucket",
        "s3ObjectKey": "filename.csv.gz"
      },
      "instanceId": "myInstance",
      "message": "No such file or directory 'mybucket'",
      "metrics": {},
      "status": "FAILED",
      "updatedAt": "2023-09-29T18:04:26Z",
      "uploadId": "a11aa11a-5678-5678-5678-ee6555eee5e5"
    }
  ]
}

```


### Uploading multiple files 

AMC supports reading of multiple files for a single upload. 

Multiple files can be uploaded by specifying an S3 prefix key. All files under the given prefix will be uploaded. Alternatively, the system accepts the key of a manifest, containing newline separated S3 paths, all of which will be uploaded. For more information, refer to [S3 bucket source](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-upload#s3-bucket-source).

The following example uses an S3 prefix key.


```
POST /amc/advertiserData/{instanceId}/uploads/{dataSetId}
```

**Sample request payload**

```
{
  "updateStrategy": "ADDITIVE",
  "compressionFormat": "GZIP",
  "dataSource": {
    "s3Bucket": "mys3bucket",
    "s3PrefixKey": "mys3prefix"
  },
  "fileFormat": {
    "csvDataFormat": {}
  }
}
```

## FAQs

**Can the S3 bucket used for data upload be the same bucket linked to the AMC instance? (Bucket where aggregate reports are stored.)**

Yes, the S3 bucket for data upload can be in the **Connected AWS Account** linked to the AMC instance. This AWS account is displayed in the **Instance info** page in the AMC console. We recommend, however, that you use a bucket separate from one created for AMC API report storage.

**Can files stored in the S3 bucket be deleted after an upload is performed?**

Yes, once the upload is complete, data files can be removed from S3 without any impact on the ability to query the data in AMC.

**Despite uploading data successfully, all my queries over the uploaded data are returning empty results?**

One possible reason is results are filtered due to data aggregation thresholds. All advertiser uploaded datasets are treated with a HIGH sensitivity, meaning the output rows need to have minimum 100 distinct users.