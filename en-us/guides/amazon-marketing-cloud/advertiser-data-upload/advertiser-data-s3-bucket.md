---
title: Create and manage s3 bucket
description: Managing the s3 bucket before creating datasets
type: guide
interface: api
tags:
  - amc
  - adu
keywords:
  - s3 bucket
  - data upload
  - permissions
  - kms encryption
  - S3 bucket tags
---

# Set up an S3 bucket

Follow the steps in this topic to set up an S3 bucket and configure it for storing data files to be uploaded. The process below covers creating an S3 bucket and applying permissions so that AMC can read data files from the bucket.

- [Create the S3 bucket](#create-the-s3-bucket)
- [Optional: KMS Encryption Key Usage](#optional-kms-encryption-key-usage)
- [Set permissions for the S3 bucket](#set-permissions-for-the-s3-bucket)
- [Define tags](#define-tags)

## Create an S3 bucket

The S3 bucket can be within the **Connected AWS Account** associated to the AMC instance you will be uploading data to, or within a different AWS account.

> [NOTE] If you are using an S3 bucket within the **Connected AWS Account**, we recommend using a bucket separate from one created for AMC API report storage. You can also use the `Data upload AWS account ID` with the required [permissions](#set-permissions-for-the-s3-bucket) and [tagging](#define-tags). The  `Data upload AWS account ID` is an AMC account ID which is used to upload data to your AMC instance. This account must have "GetObject" access to pull data when an upload job is invoked. 

1. Navigate to the S3 console and click "Create Bucket".
2. Under "General configuration" give the bucket a name, e.g., "data-upload-tutorial" and select the AWS region. The remaining settings can be left to their default options.
3. Click "Create bucket".

## Optional: KMS Encryption Key Usage

AMC supports customer encrypted data using [AWS Key Management Service (KMS)](https://aws.amazon.com/kms/). This is optional. If you do not choose to provide a KMS encryption key, the data will be encrypted at rest by default.

The benefit to using a customer generated encryption key is the ability to revoke AMC’s access to previously uploaded data at any point. In addition, customers can monitor encryption key access via AWS CloudTrail event logs.

> [WARNING] To use AMC Data Upload with a customer-specified KMS key, you must create a [multi-region key](https://docs.aws.amazon.com/kms/latest/developerguide/multi-region-keys-overview.html). Additionally, changing a KMS key is not supported.

In order to enable server-side encryption of customer datasets with AWS Key Management Service, consider the following process:

1. Create symmetric encryption key in AWS KMS.

   - During the key creation process, grant usage permissions to the AMC instance to perform decryption and encryption operations with the key.
2. Apply default encryption to S3 bucket where datasets are stored via the key created in step 1. See [Enable default encryption with AWS Key Management Service (KMS)](#optional-enable-default-encryption-with-aws-key-management-service-kms).
3. When creating a new dataset, you must include the key ARN. See [Encrypt the Dataset with AWS Key Management Service (KMS)](#create-encryption-key-with-aws-key-management-service-kms).

### Create Encryption Key with AWS Key Management Service (KMS)

If you have selected to use KMS encrytption keys, complete the the following steps to create the keys:

1. In the AWS console, navigate to Key Management Service (KMS) by searching for **KMS** in the global search bar.
2. From the KMS service homepage, click on the create key button in the top right.
3. Under **Step 1 \> Configure Key**, select **Symmetric** for **Key Type.** In Advanced options, leave **KMS** selected and select **Multi-Region key**. Click Next.
4. Under **Step 2 \> Add Labels**, add the key alias which will serve as the display name. Complete the remaining optional fields as necessary. Click next.
5. Under **Step 3 \> Define key administrative permissions,** select the users and/or roles who can administer the key. Click next.
6. Under **Step 4 > Define key usage permissions** add the AMC Instance AWS accounts to the **Other AWS accounts** section.
   - For each AMC instance that will be using the KMS key, [key access](https://docs.aws.amazon.com/kms/latest/developerguide/key-policy-modifying-external-accounts.html) will need to be provided to the **Data upload AWS account ID**. This account ID can be located in the **Instance info** page in the AMC console or use the GET operation of the `/amc/instances` resource to get the Data Upload AWS account ID.

> [WARNING] If the key does not exist in **us-east-1**, then you need to create a replica key in the **us-east-1** region.  The replica key also needs to grant access to the data upload account.
> You also must provide the key arn in the `customerEncryptionKeyArn` field when creating the dataset.

7. Click Next. On the last page (Step 5) review the key details and click **Finish** to complete the key creation process.

### Enable default encryption with AWS Key Management Service (KMS)**

If you have selected to use KMS encryption keys and have created the keys, complete the following steps to enable encryption for the S3 bucket.

1. Open the newly created S3 bucket, and navigate to the **Properties** tab.
2. In **Default encryption**, click Edit and use the following settings:

- **Server-side encryption with AWS Key Management Service keys**: Select to enable.
- **AWS KMS key:** Select from created KMS keys or provide key ARN.

> [NOTE] The key region must match the bucket region. For details, see [Configuring default encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/default-bucket-encryption.html)

## Set permissions for the S3 bucket

Next, set permissions so the AMC instance can read data files from the S3 bucket.

1. Open the S3 bucket, and navigate to the **Permissions** tab.
2. Within the **Permissions** tab, add the following permissions object under **Bucket Policy \> Edit**.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::{Data upload AWS account ID}:root"
      },
      "Action": [
        "s3:GetObject",
        "s3:GetObjectVersion",
        "s3:ListBucket",
        "s3:GetObjectTagging",
        "s3:GetBucketTagging"
      ],
      "Resource": [
        "arn:aws:s3:::{bucket_name}/*",
        "arn:aws:s3:::{bucket_name}"
      ]
    }
  ]
}

```

- Replace **{bucket_name}** with the name of the newly created bucket.
- Replace **{Data upload AWS account ID}** with the **Data upload account ID** on the **Instance info** page of the AMC UI.

## Define tags
>[WARNING] If using a **Connected AWS account**, no tagging is required. For other AWS accounts, tagging the S3 bucket or objects with the `instanceId` as described here is necessary to grant the AMC instance access.


While defining your bucket policy, note that you included "tagging options" to the **Action** element. These options ensure that your S3 bucket and/or the objects within the selected bucket are tagged to the correct instance to which data is being uploaded from. After you include the `s3:GetObjectTagging` and `s3:GetBucketTagging` options, you will need to define tags for either the bucket or the object to associate them with your instance.

> [NOTE]  If you are tagging instances to objects, you must add `s3:GetObjectTagging` and `s3:GetBucketTagging` options under the Action element. However, if you are tagging your instance at the bucket level, include only the  `s3:GetBucketTagging` under the Action element.

To define a tag for your instance, perform the following steps:

1. Navigate to your Amazon S3 console and click on the bucket name you want to associate tags to.
2. Click **Properties** and scroll to the **Tags** section. Click **Edit**.
3. Click **Add tag** to define a key for the tag and provide a value for the key. For our purpose, define the key as "instanceId" and type the identifier of the instance you use to upload data as the value for the key.

   You can also associate multiple instanceIds by using a space-separated list of instance IDs as the tag value. For example: `instanceId1 instanceId2 instanceId3.`

   This allows all specified instances to access the designated S3 bucket or object. Note that a maximum of 25 instance IDs can be included in a single tag value.
4. Click **Save**.

> [NOTE] There is a limit of 100 unique buckets per upload. This applies to manifest uploads where you can choose your uploads from any of your defined buckets.

To tag an instance to an object, perform the following steps:

1. Navigate to your Amazon S3 console and click on the bucket name to view the objects associated with the bucket.
2. Click the specific object you want to associate your instance with.
3. The object details page appears, listing all properties associated with the object.

> [NOTE] You can select multiple objects and associate those with the same tag (instance).

4. Scroll to the **Tags** section. Click **Edit**.
5. Click **Add tag** to define a key for the tag and provide a value for the **Key**. For our purpose, define the key as “instanceId” and type the identifier of the instance you use to upload data as the value for the key.
6. Be sure to click **Save**.

The S3 bucket is ready for use, and you can start creating datasets upload your data.

## Next steps

After setting up the S3 bucket, you can:

1. Create a data set for the uploaded data. Refer to [Data set](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-sets).
2. Create an upload to transfer data from the S3 bucket to AMC. Refer to [Upload](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-upload).
3. Optionally, add, modify, or delete columns from a dataset. Refer to [Dataset Column](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-column).
