---
title: Advertiser data upload - prepare data
description: Prepare data
type: guide
interface: api
---
# Prepare data

Before you begin creating datasets and uploading data, thoroughly read this topic to understand guidelines and requirements for the files and data you will upload, including compression options, file formats, and standardization and hashing data containing personally identifiable information (PII).

## Compression options

To facilitate faster uploads, we recommend that files are compressed before uploading. 

AMC supports file compressions with algorithms BZIP, GZIP, ZIP, SNAPPY, or no compressions with NONE.

## Large files

Large files should be separated for optimal upload.  We recommend partitioning files larger than 1GB into multiple files for upload. When uploading files larger than 1GB, the file read operation may fail during the upload process.

## File format requirements

AMC accepts the following formats of files for upload:

- CSV
- JSON
- Parquet

All options have specific formatting requirements for the upload files.

### CSV file requirements

CSV files must be **UTF-8 encoded**.

The CSV format allows for any **fieldDelimiter**. For example, you can use a comma-separated values (",") or tab-separated values ("\t"); pipe-separated values ("|") are also common.

**Example CSV file**

```
name,sku,quantity,pur_time
Product A,11352987,2,2022-06-23T19:53:58Z
Product B,18467234,2,2022-06-24T19:53:58Z
Product C,27264393,2,2022-06-25T19:53:58Z
Product A,48572094,2,2022-06-25T19:53:58Z
Product B,18278476,1,2022-06-24T19:53:58Z
```

**Example CSV request body**

The following is an example of how to define the CSV **fileFormat** in the Create Upload API request body:

```
"fileFormat": {
  "csvDataFormat": {
    "recordDelimiter": "\n",
    "quoteEscapeCharacter": "\"\"",
    "quoteCharacter": "\"",
    "fieldDelimiter": ",",
    "commentCharacter": "#"
  }
}
```

**Example TSV file**

The example below shows an example Tab-separated values (TSV) file.

```
name  sku   quantity  pur_time
Product A   11352987    2   2022-06-23T19:53:58Z
Product B   18467234    2   2022-06-24T19:53:58Z
Product C   27264393    2   2022-06-25T19:53:58Z
Product A   48572094    2   2022-06-25T19:53:58Z
Product B   18278476    1   2022-06-24T19:53:58Z
```

**Example TSV request body**

The following is an example of how to define the TSV **fileFormat** in the Create Upload API request body:

```
"fileFormat": {
  "csvDataFormat": {
    "fieldDelimiter": "\t"
  }
}

```

### JSON file requirements

JSON files must contain one object per row of data. An example of the accepted JSON format can be seen below.

**Example JSON file**

```
{"name": "Product A", "sku": 11352987, "quantity": 2, "pur_time": "2022-06-23T19:53:58Z"}
{"name": "Product B", "sku": 18467234, "quantity": 2, "pur_time": "2022-06-24T19:53:58Z"}
{"name": "Product C", "sku": 27264393, "quantity": 2, "pur_time": "2022-06-25T19:53:58Z"}
{"name": "Product A", "sku": 48572094, "quantity": 2, "pur_time": "2022-06-25T19:53:58Z"}
{"name": "Product B", "sku": 18278476, "quantity": 1, "pur_time": "2022-06-26T13:33:58Z"}
```

> [NOTE] Each row is a top-level object. No parent object or array is present in the JSON file.

### Parquet file requirements

While there are no specific requirements for preparing parquet files, we recommend that you look up instructions to help you get started, depending on what you are using:

- [Python/Pandas](https://pandas.pydata.org/pandas-docs/version/1.1/reference/api/pandas.DataFrame.to_parquet.html)
- [Spark SQL](https://spark.apache.org/docs/latest/sql-data-sources-parquet.html)

## Fact vs dimension datasets

Before data can be uploaded, a dataset must be created to store that data. AMC supports two types of datasets: Fact and Dimension.

**FACT datasets** are used to store time-series data (event-level data), and require either a TIMESTAMP or DATE column for AMC to partition the uploaded files. Queries run on **fact** data will only evaluate against events that fall within the query's time range. Example: Uploading purchase events, each with a TIMESTAMP type column, then querying for purchases that occurred on Tuesday.

**Dimension datasets** can be used to upload a static table, or any information which is not time-bound. Queries run on **dimension** data will evaluate against the current set of records that have been uploaded at the time the query executes. Example: Uploading a customer list, which is a fixed set of records, with no time component, then querying for the count of customers aggregated by zip code.

If a dataset has a column flagged as main event time (`mainEventTime`), it will be a fact dataset. Otherwise, it will be a dimension dataset.

## Data types, timestamp, and date formats

Dataset columns can be defined with the following data types. Ensure all values conform to the specified data types and format before uploading.

Pay close attention to the accepted formats for **TIMESTAMP** and **DATE** columns.

Where possible, string values will be coerced to the corresponding numerical type and vice-versa, but no guarantees are made on the casting process.

The table below lists the format of accepted data types:


| Data Type                  | Format                                             | Example              |
| ---------------------------- | ---------------------------------------------------- | ---------------------- |
| STRING                     | UTF-8 encoded character data                       | My string data       |
| DECIMAL                    | Numerical with two floating point level precision. | 123.45               |
| INTEGER (int 32-bit)       | 32-bit numerical, no floating points               | 12345                |
| LONG (int 64-bit)          | 64-bit numerical, no floating points               | 1233454565875646     |
| TIMESTAMP (must be in UTC) | `yyyy-MM-ddThh:mm:ssZ`                             | 2022-08-02T08:00:00Z |
| DATE                       | `yyyy-MM-dd`                                       | 2022-08-02           |

> [NOTE] The upload may fail if values do not meet the accepted formats.

## Partition Scheme (Period)

Before you begin uploading data, consider the partition scheme you want for the dataset. The partition scheme controls how granularly you can query the dataset. For example, if set to hourly, you can run queries down to the hour for time-based events. Although the default is daily, you can configure the partition size (the `period` parameter) when you create the dataset.

The following options are supported:

- PT1H (hourly partition)
- P1D (daily partition) - this is the default option
- P1M (monthly partition)

> [NOTE] For optimal query performance, select a partition scheme that satisfies the granularity of the queries you want to run.

Some examples:

If you specify your dataset period to be monthly (P1M), then your queries corresponding to last Thursday will involve reading a month's worth of data. This is because Thursday is stored in a single partition with all other data for the month.

If you specify the dataset as hourly (PT1H), and upload records that take place over the span of a year and query analytics for last Thursday, then the queries will read data within 24 hourly partitions that correspond to Thursday, causing this query to be more efficient.

So, if you have a small volume of events and frequently query for analytics across long time windows, then hourly partitioning could cause queries to run more slowly. For example, if there are only a handful of events per day, and data are partitioned hourly (PT1H), and queries run once a quarter, many small partitions are read, when fewer, larger partitions would be more efficient.

## ID resolution and supported identifiers

ID resolution is the process of matching the identifiers in the uploaded file to a corresponding Amazon user record. This allows you to join your uploaded data with Amazon data at the user level.

The ID resolution process is as follows:

- When uploading data, each record is processed, and if successful, will be matched with an existing Amazon user record.
- When a match is found, a value for `user_id `(an anonymous user ID corresponding to an Amazon user) will be inserted in the row. The original identifiers are discarded and will not be made available to query in AMC.
- Records that contain a value for `user_id` can then be joined with Amazon data, enabling advertisers to attribute off-Amazon events to Amazon media events within AMC.

### Hashed PII column options

To facilitate ID resolution, you can upload a file containing identifiers such as e-mail address or phone numbers.

> [WARNING] All PII must be standardized and hashed before uploading to AMC. If identifiers are not hashed, the upload will be rejected. See [Standardization and hashing).](#standardization-and-hashing)

To perform ID resolution, the uploaded data should contain one or more of the supported identifier columns.


| Parameter | Description                                                                                                                                                                                                                                    |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| user_id   | Column name for the unique `user_id` field\. Specify the column that can be used to distinctly reference each row of your data\. For example, Primary\_key, Row\_ID, or Record\_ID\. The event data should have the same value as this column\. |
| FIRST_NAME | Column name for first name field.                                            |
| LAST_NAME  | Column name for last name field.                                             |
| EMAIL     | Column name for email field.                                                 |
| PHONE     | Column name for phone number.                                                |
| ADDRESS   | Column name for address field\(street number and street name\).              |
| CITY      | Column name for city name field.                                             |
| STATE     | Column name for state field.                                                 |
| ZIP       | Column name for zip code field.                                              |
| COUNTRY_CODE   | Column name for country field.                                               |

The following code snippet is an example of defining a hashed email column when creating the dataset:

```
{
  "name": "hashed_email",
  "description": "The user's hashed email address",
  "dataType": "STRING",
  "columnType": "DIMENSION",
  "nullable": true,
  "externalUserIdType": {
    "hashedPii": "EMAIL"
  }
}
```

### External identifier column options


| Parameter  | Description                                                                 |
| ------------ | ----------------------------------------------------------------------------- |
| MAID       | Column name for Mobile Ad ID (MAID)  field                                  |
| LIVERAMP | Column name for LiveRamp (RampID) field transcoded in Amazon identity space |
| EXPERIAN | Column name for Experian id field                                           |

#### Mobile Ad ID requirements

When a dataset is defined with a MAID column, AMC will attempt to resolve those IDs to an Amazon ID. The Amazon ID will be used to join with existing datasets.

> [NOTE] MAID values are treated as case-insensitive.


## Setting country codes

The country code is 2-character string in the ISO 3166 format that indicates from which country the data was collected (for example, US or GB).

Although this is an optional field, it is highly recommended that you specify a country code when uploading data.


| [TILES] Did you know?                                                                                                                                                                                                 |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| For any data you upload that does not specify a country code, AMC will automatically default to a country code of`UNKNOWN`. This will result in lower match rates, and lower usability for data provided to our APIs. |

Therefore, it is strongly encouraged to include country codes in your first-party datasets.

The country code can be set on several levels: **row-level**, **upload-level**, or **dataset-level**.

> [NOTE] The **Priority order** in the table below shows which country code is selected if there is a conflict. For example, the row-level country code value will override the value set at the upload-level or dataset-level, if they are in conflict.


| If you want to...                                     | Use...        | The result                                                                                   | Priority order | For more information...                                                                                                                                                                         |
| ------------------------------------------------------- | --------------- | ---------------------------------------------------------------------------------------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Upload data from different countries in same file     | Row-level     | This allows you to set different country codes to different rows, in a more granular manner. | 1              | Refer to [Setting the country code (row-level or dataset-level)](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-sets#setting-the-country-code-row-level-or-dataset-level). |
| Upload data from multiple countries in multiple files | Upload-level  | The country code will be applied to all records in the upload.                               | 2              | Refer to [Setting the country code at upload-level](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-upload#setting-the-country-code-at-upload-level).                       |
| Upload data from a single country                     | Dataset-level | The country code will be applied to all records in the dataset.                              | 3              | Refer to [Setting the country code (row-level or dataset-level)](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-sets#setting-the-country-code-row-level-or-dataset-level). |

## Standardization and hashing

Any files that contain PII, such as e-mail addresses or phone numbers, must be standardized and hashed before uploading to AMC.

> [WARNING] Do not perform standardization and hashing for files containing LiveRamp identifiers, Experian identifiers, or MAID.

### Standardization Rules

Raw values in a hashed identifier column must be standardized before they go through the hashing process. Standardization is the process of transforming raw data to a more standard and consistent format (for example, converting all letters to lowercase).

For the majority of fields, the standardization process is simple. Certain fields like phone number and address require additional transformations. 

The full set of rules for hashed data, including all conventions for street post and prefixes [can be found here (link only available for Ads Console users)](https://advertising.amazon.com/dsp/help/ss/en/audiences/advertiser-audiences/advertiser-hashed-audience/#GA6BC9BW52YFXBNE). To access this content, you must log into your Ads Console account.

### Hash Method – SHA-256

Once the data has been standardized, all hashed identifier columns will need to be hashed via the **SHA-256** hash method. An example of how to hash an email value in Python 3 can be seen below.

```
>>> import hashlib

>>> hashlib.sha256(b"abc@example.com").hexdigest()
'9eceb13483d7f187ec014fd6d4854d1420cfc634328af85f51d0323ba8622e21'
```

## Next steps

Create an S3 bucket, if not already created, and configure it for storing data files to be uploaded. For more information, refer to [Set up the S3 bucket](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-s3-bucket).
