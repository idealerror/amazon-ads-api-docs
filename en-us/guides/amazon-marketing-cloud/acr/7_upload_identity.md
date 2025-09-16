---
title: Amazon Marketing Cloud - AWS Clean Rooms
description: AMC - AWS Clean Rooms
type: guide
interface: api
---
# Bring identity data into the collaboration instance

> [NOTE] **Use rule-based matching for identity resolution only if you want to join Amazon data with your identity data**.

After you've joined a collaboration, you can use AMC and AWS Entity Resolution to securely integrate your **1P identity** data with AMC's data natively, within the AWS Clean Rooms environment, without needing to the need to move them outside of your own AWS cloud environment.

The identity data of your records is matched with Amazon Ads’ identity data with the help of a mapping table. To allow you to perform the match, AMC-AWS Clean Rooms leverages the [multi-key matching rules offered by AWS Entity Resolution](https://docs.aws.amazon.com/connect/latest/adminguide/how-rule-based-identity-resolution-works.html) without revealing underlying data to one another.

You can then start using your data with AMC data for typical AMC use cases such as measurement analysis, perform joint analysis, and activation on the combined datasets to run matching queries on your 1P identity data and AMC data.

Before you [begin to integrate your 1P data with AMC data](#prepare-identity-data-for-aws-entity-resolution), ensure that you have completed the [Setup tasks for AWS Entity Resolution](#setup-tasks-for-aws-entity-resolution).

Also, ensure that you have the following:

- An Amazon S3 bucket in which [your datafiles are stored](#step-1-prepare-and-store-identity-data-in-your-s3-bucket) or will be stored.

> [WARNING] Ensure that the S3 bucket for receiving the query results is a different location from the S3 bucket that contains the data that you bring into the collaboration.

- Access to [AWS Glue for cataloging the datafiles](#step-2-catalog-data-as-aws-glue-table).
- Access to AWS Entity Resolution for [setting up the ID namespaces](https://docs.aws.amazon.com/entityresolution/latest/userguide/create-id-namespace.html).
- You would have also [configured IAM roles and permissions](https://docs.aws.amazon.com/entityresolution/latest/userguide/create-id-namespace-source.html) for the SOURCE Id namespace pass role.

## Setup tasks for AWS Entity Resolution

The setup tasks for AWS Entity Resolution require you to create a [Create a SOURCE ID namespace pass role](#create-a-source-id-namespace-pass-role).

### Step 1: Create a SOURCE ID namespace pass role

To define the roles that will use the collaboration, [create an IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html).

1. Sign in to the IAM console at [https://console.aws.amazon.com/iam/](https://console.aws.amazon.com/iam/) with your administrator account.
2. **Create Policy** and copy and paste [this custom role permissions](#rolepermission) into the JSON editor.
3. [Create an IAM role](https://docs.aws.amazon.com/entityresolution/latest/userguide/create-iam-role.html).

   1. For **Trusted entity type**, choose **Custom trust policy**,
   2. Copy and paste [this custom trust policy](#step-3-assign-role-permission-to-the-source-id-namespace-pass-role) into the JSON editor.
   3. In the **Add permissions** section, attach the policy created in step 2.
4. Complete the rest of the steps to create your IAM role.

Samples of the trust policy and role permissions for the created role are illustrated below:

### Step 2: Attach trust policy to SOURCE ID namespace pass role

```
AWS Entity Resolution Service Trust Policy
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "TargetAssumeRolePolicy",
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "entityresolution.amazonaws.com"
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}

```

### Step 3: Assign role permission to the SOURCE ID namespace pass role

```
SOURCE IdNamespace Pass Role Permissions
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Sid": "S3InputPermissions",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket",
                "s3:GetBucketLocation"
            ],
            "Resource": [
                "arn:aws:s3:::{{input-buckets}}",
                "arn:aws:s3:::{{input-buckets}}/*" (provide input bucket location (the S3 buckets that contains the underlying data objects of AWS Glue for AWS Entity Resolution to read from. ))
            ],
            "Condition": {
                "StringEquals": {
                    "s3:ResourceAccount": [
                        "{{awsAccountId}}"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Sid": "GluePermissions",
            "Action": [
                "glue:GetDatabase",
                "glue:GetTable",
                "glue:GetPartition",
                "glue:GetPartitions",
                "glue:GetSchema",
                "glue:GetSchemaVersion",
                "glue:BatchGetPartition"
            ],
            "Resource": [
                "arn:aws:glue:{{aws-region}}:{{awsAccountId}}:database/{{input-databases}}",
                "arn:aws:glue:{{aws-region}}:{{awsAccountId}}:table/{{input-databases}}/{{input-tables}}",
                "arn:aws:glue:{{aws-region}}:{{awsAccountId}}:catalog"
            ]
        }
    ]
}
```

| Parameter       | Description                                                                                                                                                                                                                                        |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| input-buckets   | Amazon S3 buckets that contains the underlying data objects of AWS Glue from where AWS Entity Resolution will read.                                                                                                                                |
| region          | AWS region of your resources. Your AWS Glue resources, underlying Amazon S3 resources and AWS KMS resources must be in the same AWS Region as the AWS Entity Resolution. For the current deployment, only**us-east-1*** region is supported. |
| awsAccountId    | Your AWS account ID.                                                                                                                                                                                                                               |
| input-databases | AWS Glue databases from where AWS Entity Resolution will read.                                                                                                                                                                                     |
| input-tables    | AWS Glue tables from where AWS Entity Resolution will read.                                                                                                                                                                                        |

> [NOTE] Remember to replace *{{awsAccountId}}* parameter in AWS Entity Resolution Service Trust Policy for the Source Id Namespaces, with the AWS AccountId of the entity owning the SOURCE IdNamespace.

## Prepare identity data for AWS Entity Resolution

After you have joined a collaboration, prepare your 1P identity data for to upload to Amazon S3 buckets.

The data preparation process requires you to perform a five-step workflow:

1. [Prepare and store identity data in your S3 bucket](#step-1-prepare-and-store-identity-data-in-your-s3-bucket)
2. [Catalog data as AWS Glue table](#step-2-catalog-data-as-aws-glue-table)
3. [Create a schema mapping](#step-3-create-schema-mapping-corresponding-to-the-aws-glue-table)
4. [Create a Source ID namespace in AWS Entity Resolution](#step-4-create-a-source-id-namespace-wrapping-input-source-dataset-in-aws-glue-table)
5. [Associate the Source ID namespace with the AWS Clean Rooms collaboration](#step-5-associate-the-source-id-namespace-with-the-aws-clean-rooms-collaboration)

### Step 1: Prepare and store identity data in your S3 bucket

Before bringing your data to your S3 bucket, you must prepare your identity records so that they conform to the requirements required for further processing.

#### Prepare identity data for AWS Entity Resolution

Before you store your data in an S3 bucket, ensure that you have prepared your data as described below:

- AWS Entity Resolution supports identity data stored in CSV and Parquet file formats only.

> [NOTE] For improved query performance in AMC, we recommend **Parquet** files.

- The column names in the Glue tables must be an exact match to the header rows in the corresponding CSV files. All headers and column in the AWS Glue table must exist in the root file (the base table).
- All column headers must be in lowercase.
- Event table(s) must have a reference that is mapped to a unique key **corresponding to every single row in the identity population table**. The identity population could be in any of the following:

  - PII
  - MAIDS ID
  - Experian ID
  - Merkle ID
  - LiveRamp ID
- The Experian ID, Merkle ID, and LiveRamp ID must be transcoded into Amazon namespace.
- AWS Entity Resolution can support up to 25 columns (24 identity columns and a single column containing the unique ID) in a schema mapping.
- A schema mapping consists of a subset of columns from the given Glue tables.
- If the identity data has more than 25 identity columns with a Glue table, the data must be stored in separate Amazon S3 locations, and a separate Glue table must be created for each S3 location.
- If the identity data contains more than 25 rows with the same unique key value, only 25 rows will be considered during the execution of the mapping workflow. Any additional rows with the same unique key values will be ignored. This limitation will result in lower match rates because some relevant data will be excluded from the mapping process.
- When you have more than 25 rows for the same individual and want to ensure that all records are considered for the mapping workflow, then you will need to collapse those records into a single row. For example, if there are two rows:
- - Row 1 - Unique key1, Address1
  - Row 2 - Unique key1, Address2

  Then, collapse these into a single row with the structure:

  - Unique key1, Address1, Address2
- You can store either raw PII data or hashed PII data.

  - If you want to bring in hashed PII data, you will need to normalize the data using Amazon Ads' normalization logic and then hash it using the SHA-256 algorithm. The [normalization rules can be found here](https://advertising.amazon.com/help/GCCXMZYCK4RXWS6C).
  - If you bring in raw PII of 3P ids, AWS Entity Resolution will automatically normalize the ids for you. However, note that normalized data is also supported.
- Alternatively, you can utilize the [AMCFilePreparationTool](https://public-file-hosting-repository.s3.amazonaws.com/File+Preparation+Tool.zip) to normalize and hash your data.
- The column names in the Glue tables must be an exact match with the header rows in the corresponding CSV files.
- All column headers must be in lowercase.
- Event table(s) must have a reference that is mapped to the unique key in the identity population table.

See [sample tables for reference](guides/amazon-marketing-cloud/acr/5_data_prep#sample-tables-for-reference)

#### Upload data to your S3 bucket

1) Sign in to the **AWS Management Console** and navigate to the Amazon S3 console at [https://console.aws.amazon.com/s3/](https://console.aws.amazon.com/s3/).

2) Select "Buckets" and choose an existing bucket (or create a new bucket) in the **us-east-1 region** to store your event data table.

> [NOTE] The data should have a column titled of datatype string, as the foreign key that will map with the unique key column in your identity dataset. In our example the foreign key is `customerid`

3) Click "Upload" and follow the on-screen prompts.

4) Go to the "Objects" tab to view the prefix where your data is stored. Make a note of the folder name.

5) Select the folder to view the data table.

> [NOTE] Upload identity separately and then proceed to the next step of creating a crawler to scan the uploaded data in S3 bucket to define a glue table in the **us-east-1** region.

### Step 2: Catalog data as AWS Glue table

Create an [AWS Glue table on the S3 data](https://docs.aws.amazon.com/glue/latest/dg/catalog-and-crawler.html) either by setting up a crawler or [manually](https://docs.aws.amazon.com/glue/latest/dg/start-data-catalog.html) creating the glue table.

> [NOTE] If your data is in .csv format, ensure that the header row matches exactly with the column names and are in the same order in the AWS Glue table for the mapping to be successful.

After creating the table, verify if it is successfully created by navigating to AWS Athena and performing the following steps:

- On the left side, navigate to the database, from the table drop down select the newly created table.
- Run
  ```
  SELECT * FROM "database"."table_name" limit 10;
  ```
- Ensure query runs successfully and results line up with expectations

Data is cataloged and ready for use in the next steps.

### Step 3: Create schema mapping corresponding to the AWS Glue Table

To define the input data that is to be resolved, you must create a schema mapping. The schema mapping defines the data to be resolved by specifying the input fields and attribute types.

The schema definition supports one mandatory unique ID column, and up to 25 input fields.

[Create a schema mapping](https://docs.aws.amazon.com/entityresolution/latest/userguide/create-schema-mapping-import-existing.html) through your AWS Entity Resolution console or use the [CreateSchemaMapping](https://docs.aws.amazon.com/entityresolution/latest/apireference/API_CreateSchemaMapping.html) API.

- These input fields are used to map the identity data with Amazon's identity data.
- Depending on the identity columns available, include or exclude the appropriate fields in the JSON payload.
- For each input field, assign an input type to classify the data.
- The input type will be used to determine the normalization rules to be applied to the field.
- Assign a "Match key" to each input field to enable field comparison.
- If multiple input fields represent the same type of data, assign them the same "Match key" so they can be compared together. For example, if there are multiple email addresses, the "Match key" should be set to "Email" for all those fields.
- The input data can be either raw data or hashed data.
- If the data is hashed, users should set the "hashed" attribute to "true".
- If the data is not hashed (raw), the "hashed" attribute should be set to "false".
- If the "hashed" attribute is not specified, the default value is "false".

Use the following template of an API payload to create a schema mapping:

**JSON payload**

```
{
    "schemaName":"{{schemaName}}",
    "mappedInputFields": [
          {
             "fieldName": "{{userid}}",
             "type": "UNIQUE_ID"
         },
         {
             "fieldName": "{{firstname}}",
             "type": "NAME_FIRST",
             "matchKey": "FirstName",
             "hashed": [true|false]
         },
         {
             "fieldName": "{{lastname}}",
             "type": "NAME_LAST",
             "matchKey": "LastName",
             "hashed": [true|false]
         },
         {
             "fieldName": "{{email}}",
             "type": "EMAIL_ADDRESS",
             "matchKey": "Email",
             "hashed": [true|false]
         },
         {
             "fieldName": "{{phone}}",
             "type": "PHONE",
             "matchKey": "Phone",
             "hashed": [true|false]
         },
         {
             "fieldName": "{{address}}",
             "type": "ADDRESS",
             "matchKey": "Address",
             "hashed": [true|false]
         },
         {
             "fieldName": "{{city}}",
             "type": "ADDRESS_CITY",
             "matchKey": "City",
             "hashed": [true|false]
         },
         {
             "fieldName": "{{state}}",
             "type": "ADDRESS_STATE",
             "matchKey": "State",
             "hashed": [true|false]
         },
         {
             "fieldName": "{{zip}}",
             "type": "ADDRESS_POSTALCODE",
             "matchKey": "Postal",
             "hashed": [true|false]
         },
         {
             "fieldName": "{{country}}",
             "type": "ADDRESS_COUNTRY",
             "matchKey": "Country",
             "hashed": [true|false]
         },
         {
             "fieldName": "{{maid}}",
             "type": "STRING",
             "matchKey": "MAID",
             "hashed": "false"
         },
         {
             "fieldName": "{{liverampid}}",
             "type": "STRING",
             "matchKey": "LRP",
             "hashed": "false"
         },
         {
             "fieldName": "{{experianid}}",
             "type": "STRING",
             "matchKey": "EXP",
             "hashed": "false"
         },
         {
             "fieldName": "{{merkleid}}",
             "type": "STRING",
             "matchKey": "MKL",
             "hashed": "false"
         }
     ]
 }
```

> [NOTE] If you have a sub set of the above fields or have multiple fields, then adjust the API payload accordingly.

| Parameter  | Description                                                                                                                                                                                                                                 |
| ---------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| schemaName | Name of the schema mapping.                                                                                                                                                                                                                 |
| userid     | Glue column name for the Unique User Id field. Specify the column that can be used to distinctly reference each row of your data. For example: Primary\_key, Row\_ID, or Record\_ID. The event data should have the same value as this column. |
| firstname  | Glue column name for first name field.                                                                                                                                                                                                      |
| lastname   | Glue column name for last name field.                                                                                                                                                                                                       |
| email      | Glue column name for email field.                                                                                                                                                                                                           |
| phone      | Glue column name for phone field.                                                                                                                                                                                                           |
| address    | Glue column name for address field that has the street number and the street name.                                                                                                                                                          |
| city       | Glue column name for city name field.                                                                                                                                                                                                       |
| state      | Glue column name for state field.                                                                                                                                                                                                           |
| zip        | Glue column name for zip code field.                                                                                                                                                                                                        |
| country    | Glue column name for country field.                                                                                                                                                                                                         |
| maid       | Glue column name for MAID Id field.                                                                                                                                                                                                         |
| liverampid | Glue column name for Liveramp Id field transcoded in Amazon Identity space.                                                                                                                                                                 |
| merkleid   | Glue column name for Merkle Id field.                                                                                                                                                                                                       |
| experianid | Glue column name for Experian Id field.                                                                                                                                                                                                     |

 After substituting the placeholders in the API payload template with values specified by the user, save this updated payload into a file named `create-schema-mapping.json`.

Use this file's path in the following AWS Entity Resolution CLI command **create-schema-mapping** to initiate the creation of a schemaMapping from AWS CLI.

```
aws entityresolution create-schema-mapping --region us-east-1 --cli-input-json file://{{apiPayloadJsonFilePath}}
```

| Parameter              | Description                                                                    |
| ---------------------- | ------------------------------------------------------------------------------ |
| apiPayloadJsonFilePath | Path to Json file "create-schema-mapping.json", storing api request parameters |

### Step 4: Create a SOURCE ID namespace wrapping input SOURCE dataset in AWS Glue Table

An ID namespace is a resource that contains metadata explaining datasets across multiple AWS accounts and how to use these datasets in an ID mapping workflow.

An ID namespace SOURCE contains configurations for the source data that will be processed in an ID mapping workflow. The ID namespace TARGET contains a configuration of the target data to which all sources will resolve to. To deﬁne the input data that you want to resolve across two AWS accounts, create an ID namespace source and an ID namespace target to translate your data from one set (SOURCE) to another (TARGET).

After you and another member create ID namespaces and run an ID mapping workflow, you can join a collaboration in AWS Clean Rooms to run a multi-table join on the ID mapping table, and analyze the data.

To create an ID namespace SOURCE:

1. Use AWS CLI to execute the [CreateIDNamespace](https://docs.aws.amazon.com/entityresolution/latest/apireference/API_CreateIdNamespace.html) API, using the sample template of the API payload to create an IdNamespace of type SOURCE.

**JSON payload**

```
{
    "idNamespaceName": "{{idNamespaceName}}",
    "description": "Advertiser owner SOURCE Id Namespace",
    "type": "SOURCE",
    "inputSourceConfig": [
              {
           	"inputSourceARN": “arn:aws:glue:us-east-1:{{aws_account}}:table/{{database_name}}/{{table_name}}",
                    "schemaName": "{{schemaName}}"
        }
    ],
    "idMappingWorkflowProperties": [
        {
            "idMappingType": "RULE_BASED",
                    "ruleBasedProperties": {
                        "ruleDefinitionTypes": ["TARGET"]
                    }
        }
    ],
    "roleArn": "{{idNamespacePassRoleArn}}"
}
```

Up to 20 tables can be associated with a single namespace. In case there are more than 20 tables, we recommend that you consolidate the data without any duplicate rows within the same table. Duplicate values can, however, exist across tables.

| Parameter              | Description                                                                                                     |
| ---------------------- | --------------------------------------------------------------------------------------------------------------- |
| idNamespace            | Name of the SOURCE IdNamespace.                                                                                 |
| awsAccount             | AWS Account in which the glue table was created.                                                                |
| databaseName           | The database name in which the glue table was created.                                                          |
| tableName              | The Glue table name                                                                                             |
| schemaName             | [Name of the schema mapping](#step-3-create-schema-mapping-corresponding-to-the-aws-glue-table)                    |
| idNamespacePassRoleArn | [Id Namespace Pass Role Arn](#step-4-create-a-source-id-namespace-wrapping-input-source-dataset-in-aws-glue-table) |

2. After substituting the user input parameters in the API payload template with values specified by the user, save this updated payload into a file named **create-source-id-namespace.json**.
3. Use this file's path in the following AWS Entity Resolution CLI command **create-id-namespace** to initiate the creation of an IdNamespace.

```
aws entityresolution create-id-namespace  --region *us-east-1* --cli-input-json file://*{{apiPayloadJsonFilePath}}*
```

> [NOTE] Replace each user input parameter indicated in *{{ }}* with your own information.

| Parameter              | Description                                                                             |
| ---------------------- | --------------------------------------------------------------------------------------- |
| apiPayloadJsonFilePath | Path to Json file "*create-source-id-namespace.json*", storing api request parameters |

### Step 5: Associate the SOURCE ID namespace with the AWS Clean Rooms Collaboration

Use AWS CLI to pass the API payload to associate your AWS Entity Resolution ID with your AWS Clean Rooms collaboration.

Use the following template API payload for the `create-id-namespace-association` command, which associates your AWS Entity Resolution ID namespace to the collaboration and prepares it for collaborative ID mapping.

```
{
    "name": "*{{idNamespaceAssociationName}*}",
    "description": "*{{idNamespaceAssociationDescription}}*",
    "inputReferenceConfig": {
        "inputReferenceArn": "*{{idNamespaceArn}}*",
        "manageResourcePolicies": true
    }
}
```

| Parameter                         | Description                                                                    |
| --------------------------------- | ------------------------------------------------------------------------------ |
| idNamespaceAssociationName        | Name of the association resource to be created in AMC–AWS Clean Rooms.        |
| idNamespaceAssociationDescription | Description of the association resource to be created in AMC–AWS Clean Rooms. |
| inputReferenceArn                 | ID namespace resource ARN created                                              |

After creating the payload, store it in a file. In this example, we name the file `create-source-id-namespace-association.json`.

Use the following CLI command to create the ID namespace association in AWS Clean Rooms.

```
aws cleanrooms create-id-namespace-association --region us-east-1 --membership-identifier {{membershipIdentifier}} --cli-input-json file://{{apiPayloadJsonFilePath}}
```

> [NOTE] Replace each user input parameter indicated in *{{ }}* with your own information.

| Parameter              | Description                                                                                                                                                                                                           |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| membershipIdentifier   | Your membership identifier in the AWS Clean Rooms collaboration you are associating the resource to. This value can be obtained from the details tab of the collaboration in AWS Clean Rooms through the AWS console. |
| apiPayloadJsonFilePath | Path to Json file `create-source-id-namespace-association.json`, storing api request parameters.                                                                                                                    |

Your identity data is now associated with your collaboration instance.

After your data is prepared and stored in your S3 bucket, you will need to map your identity data with AMC data. This step is performed through AMC APIs.

## Create an ID mapping table for identity data

The identity data that you prepared and stored in your S3 bucket must be mapped to AMC data.

The steps in this section are performed through AMC APIs. To execute these steps, you should have already onboarded to [AMC API on Amazon Ads](guides/amazon-marketing-cloud/amc-migration-hub/migration-guide).

The ID mapping process requires you to perform a five-step workflow:

1. [Identify your AMC account identifier](#step-1-identify-your-amc-account-identifier)
2. [Retrieve your collaboration instance information](#step-2-retrieve-your-collaboration-instance-information-instanceid)
3. [Retrieve the ID namespaces associated to a collaboration](#step-3-retrieve-the-id-namespaces-associated-to-a-collaboration)
4. [Create an ID mapping table](#step-4-create-an-id-mapping-table)
5. [Retrieve the ID mapping job status](#step-5-retrieve-the-id-mapping-job-status)

Once the job status is “complete”, the mapping table is ready to be used in your queries.

After you have created the mapping table, if you want to re-populate the mapping table, then call [the API to perform a table refresh](amc-administration#tag/Instances/operation/refreshCollaborationIdMappingTable). The table refresh can be done only once in 14 days and should be triggered only when there is change to the input data. The incremental changes to the input data will automatically be populated in the mapping table (provided the rows with the unique keys are not duplicated).

The [delete mapping table](amc-administration#tag/Instances) endpoint deletes the mapped table.

### Step 1: [Identify your AMC account](guides/amazon-marketing-cloud/admin/account-management) identifier

To identify your AMC [Identify your AMC account](guides/amazon-marketing-cloud/admin/account-management) ID (AccountID), follow the steps listed in [View AMC account identifier](guides/amazon-marketing-cloud/admin/account-management).

### Step 2: Retrieve your collaboration instance information (instanceId)

To view collaboration metadata for an AMC instance, use the GET operation of the [/amc/instances/{instanceId}/collaboration](amc-administration#tag/Instances/operation/listCollaborationIdNamespaces) endpoint.

> [NOTE] Collaboration metadata is only available for instances backed by AWS Clean Rooms.

Listed below is the response schema:

| Parameter              | Description                                                                                                                                                                                                                   |
| ---------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ID                     | Unique identifier of the collaboration instance\.                                                                                                                                                                             |
| name                   | The collaboration’s display name.                                                                                                                                                                                            |
| description            | Description of the collaboration\.                                                                                                                                                                                            |
| customerAcceptanceLink | Deep link to the collaboration acceptance page in the customer's AWS account\.                                                                                                                                                |
| status                 | The current status of the collaboration\. The following table lists available statuses:                                                                                                                                       |
|                        | **NOT\_STARTED**: Collaboration setup has not started\.                                                                                                                                                                 |
|                        | **IN\_PROGRESS**: Collaboration setup is in progress\.                                                                                                                                                                  |
|                        | **PENDING\_ACCEPTANCE**: Waiting for the AMC–AWS Clean Rooms user to accept collaboration\.                                                                                                                            |
|                        | **ACTIVE**: The collaboration was successfully setup and is now active in AWS Clean Rooms\.                                                                                                                             |
|                        | **REJECTED**: The AMC–AWS Clean Rooms customer did not accept the collaboration\.                                                                                                                                      |
|                        | **DELETED**: The collaboration was deleted and is no longer active in  AWS Clean Rooms\.                                                                                                                                |
| creator                | Type of collaboration member, in this case, creator\. Your collaboration can have creator, query submitter, customer, and AMC data provider types of members\. Each type of member has details for the following parameters: |
|                        | **Description**: Metadata about a collaboration member\.                                                                                                                                                                |
|                        | **accountId**: The AMC–AWS Clean Rooms user’s AWS account ID\.                                                                                                                                                        |
|                        | **membershipId**: Unique identifier of the AMC–AWS Clean Rooms user’s membership in the collaboration\.                                                                                                               |
|                        | **displayName**: The AMC–AWS Clean Rooms user’s display name/membership abilities\.                                                                                                                                   |
|                        | **status**: The status of the member in the collaboration\. Valid values are:INVITED; ACTIVE; LEFT; REMOVED                                                                                                             |

### Step 3: Retrieve the ID namespaces associated to a collaboration

To retrieve the ID namespaces not associated to a collaboration for the AMC  instance you retrieved in the previous step, use the POST operation of the [/amc/instances/{instanceId}/collaboration/idnamespaces/list](amc-administration#tag/Instances/operation/listCollaborationIdNamespaces) endpoint.

This operation will return the ID namespaces that are not connected to an ID mapping table in the collaboration in the requested AMC instance.  Select one of the not-connected namespaces returned in the response body to create a mapping table.

### Step 4: Create an ID mapping table

To create an ID mapping table in the retrieved/requested AMC instance collaboration and trigger the data population job:

1. Use the POST operation of the [/amc/instances/{instanceId}/collaboration/idmappingtables](amc-administration#tag/Instances/operation/updateInstanceCustomerAwsAccountMetadata) endpoint.
2. Use the following as the request body payload:

```
{ "idMappingTableName": {{id Mapping Table Name}},
"sourceIdNamespaceArn": {{ARN for the id Namespace created for customer population}}
}
```

Listed below is the response schema:

| Parameter              | Description                                                                                        |
| ---------------------- | :------------------------------------------------------------------------------------------------- |
| idMappingWorkflowJobId | Unique identifier of the job started to populate the id mapping table\.                            |
| idMappingTable         | The collaboration ID mapping table with the following parameters:                                  |
|                        | -**description**: The description of the table\.                                             |
|                        | -**queryable**: Indicates if the table is can be queried\.                                   |
|                        | -**createTime**: Time when the table was created\.                                           |
|                        | -**workflowArn**: The ARN of the id mapping workflow processing the data backing the table\. |
|                        | -**Name**: The name of the table\.                                                           |
|                        | -**updateTime**: The time the table was last updated\.                                       |
|                        | -**Id**: Unique identifier of the table\.                                                    |
|                        | -**ARN**: The ARN of the table\.                                                             |
| collaborationId        | Unique identifier of the collaboration the ID mapping table is part of\.                           |

### Step 5: Retrieve the ID mapping job status

To retrieve the ID mapping job status for the requested AMC instance, use the GET operation of the [/amc/instances/{instanceId}/collaboration/idmappingtables/{idMappingTableId}/jobs/{jobId}](amc-administration#tag/Instances/operation/getCollaborationIdMappingJob) endpoint.

### Manage ID mapping tables

You can use the following AMC API operations to manage ID mapping tables.

- Update the data in the given ID Mapping Table in the collaboration for the requested AMC instance.

```
POST /amc/instances/{instanceId}/collaboration/idmappingtables/{idMappingTableId}/refresh
```

- Retrieve the ID mapping job metadata for the requested AMC instance.

```
GET /amc/instances/{instanceId}/collaboration/idmappingtables/{idMappingTableId}/jobs/{jobId}
```

- Delete the given ID Mapping Table in the collaboration for the requested AMC instance.

```
DELETE /amc/instances/{instanceId}/collaboration/idmappingtables/{idMappingTableId}
```

> [NOTE] An instance can have up to 5 mapping tables.

#### Unsupported AMC API operations

The following operations will not work with collaboration instances:

- `DELETE /amc /advertiserData /{instanceId}/dataSets/{dataSetId}` will not delete any data sets from your collaboration instance. However, you can delete data sets uploaded through ADU.

For more detailed instructions and features of the AMC API, go through the [Amazon Marketing Cloud API developer guides](guides/amazon-marketing-cloud/overview).
