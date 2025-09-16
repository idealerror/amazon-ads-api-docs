---
title: AMC migration hub
description: How to migrate
type: guide
interface: api
---


# Migrating AMC instance-level APIs to Amazon Ads API

Migrating AMC APIs to Amazon Ads requires you to first request for API access and then onboard your application to Amazon Ads API.

**Step 1**: Go to https://developer.amazon.com/ and click **Developer console** on the top-right of the page. 
Create an Amazon developer account. If your account is created successfully, the Amazon  developer - Dashboard page shows up.

>[NOTE] Create an Amazon developer account using the email address you use access the Amazon Ads console.  

**Step 2**: Click **Login with Amazon** and then click **Create a New Security profile**.
In the Security Profile Management page, provide the following details:

- **Security Profile Name**: A name to identify the client application, for example AMC APIs.
- **Security Profile Description**: A description to identify the security profile you are creating. 
- **Consent Privacy Notice URL**: This is the URL address of your profile's privacy policy. 

Click **Save**. Your Login with Amazon account and a security profile created. 

>[WARNING] As a policy, Amazon Ads enables API access for a single client ID per company. There is no need for multiple client IDs, as an unlimited number of users can use the same client ID within a company. Additional user permissions can be managed later in the Amazon Developer console. However, only the original creator of the Amazon Developer account may complete the initial onboarding sequence.

**Step 3**: Click the gear icon alongside the profile you just created and select **Web Settings**. 

- In the Security Profile Management - Web Settings tab, provide a value for **Allowed Return URLs**. We'll refer to this value as `YOUR_RETURN_URL`. 
- Click **Client secret**. Copy the Client ID (we will refer to this value `YOUR_LWA_CLIENT_ID` as and the Client Secret (we'll refer to this value as `YOUR_SECRET_KEY`). 

>[NOTE] You will need this information later to retrieve an authorization code to access the AMC APIs.

**Step 4**: Depending on your business [Apply for API access](guides/onboarding/apply-for-access), either as a [Partner](https://advertising.amazon.com/partners/network) or as a [direct advertiser](https://advertising.amazon.com/partner-network/register-api?ref_=a20m_us_api_drctad).

>[WARNING] Remember to use your Amazon Developer account to sign in and complete the form to request for API access. 

Your application could take up to 1 business day to get approved. 
If your API access request is approved, you will receive an email from Amazon Ads informing you of the approval and a link that you will use for accessing your APIs.

**Step 5**: Click the link in the approval email to [assign API access](guides/onboarding/assign-api-access) to the LwA application you created earlier.

Select the client application listed to which you want to assign API access. 
Note the scopes that the client application has been approved to use. 
>[NOTE] For most API calls, you will need the `advertising::campaign_management` scope. 
If you need access to the Advertiser audiences or the Advertiser data upload APIs, you must request for **Data provider** access when [applying for API access](guides/onboarding/apply-for-access). If your access if approved, you will receive the `advertising::audiences` scope too.    

At this point, your client application has been approved to access Amazon Ads APIs. You will now set up the authorization workflow for the client application. 

**Step 6**: You will now need to [create an authorization URL](guides/get-started/create-authorization-grant#create-an-authorization-url). The output of the authorization URL is an authorization code  that you will use to generate an access token. To create the authorization URL, you'll need the following:

- Application's LwA client Id. (`YOUR_LWA_CLIENT_ID`)
- Scope(s) to authorize. If you have more than one scope, separate the two scopes with a space. The browser will encode a space at runtime. 
- Application's redirect\_uri. (`YOUR_RETURN_URL`)
- Response type. This will always be "code".

The authorization URL for a user in the North America region will be in the following format:

```
https://www.amazon.com/ap/oa?client_id=YOUR_LWA_CLIENT_ID&scope=advertising::campaign_management&response_type=code&redirect_uri=YOUR_RETURN_URL
```

> [NOTE] In case of multiple scopes, for example if you've been approved for the scopes: `advertising::campaign_management` and `advertising::test:create_account`, then  your URL will be in the following format: `https://www.amazon.com/ap/oa?client_id=YOUR_LWA_CLIENT_ID&scope=advertising::test:create_account%20advertising::campaign_management&response_type=code&redirect_uri=https://YOUR_RETURN_URL.`

**Step 7**: If the parameters in the above URL are valid, the page refreshes to display your client application, asking you for permission to use the application. Click Allow. 
You will now be redirected to the `redirect url`. The address bar of your browser also displays the authorization code that you will use to generate an access token. The format of the returned URL is listed below:

```
https://<redirect_url>/?code=xxxxxxxxxxxxxxxxxxx&scope=advertising%3A%3Acampaign_management
```

**Step 8**: Copy the code generated here, we'll call this the `AUTH_CODE`. You will need it for the next step.

>[NOTE] The code displayed here is a single use code and expires after 5 minutes. If required, repeat the steps to generate a new code. 

For your initial setup, you will need you'll need the following to  [generate an access token](guides/get-started/retrieve-access-token):

- Application's LwA client id. (`YOUR_LWA_CLIENT_ID`)
- Application's client secret. (`YOUR_SECRET_KEY`)
- Application's redirect uri. (`YOUR_RETURN_URL`)
- Application authorization code that your authorization URL generated. (`AUTH_CODE`)
- The grant\_type to use, which is `authorization_code`. 

**Step 9:** Make an HTTP request using the following curl command (after inserting valid values for the listed parameters) to generate tokens from `authorization_code`:

```
curl --request POST \
    --url https://api.amazon.com/auth/o2/token \
     --data 'grant_type=authorization_code' \
    --data 'code=AUTH_CODE'
    --data 'client_id=YOUR_LWA_CLIENT_ID' \
    --data 'client_secret=YOUR_SECRET_KEY' \
    --data 'redirect_uri=YOUR_RETURN_URL'
``` 

This will return an access token (value that is prepended with "Atza") and a refresh token (value prepended with "Atzr")
The access token can be used to authenticate all requests by providing it in a header.
Now that you have a valid access token and values for the common header parameters, you are ready to make your first call.  

For authenticating your access using Postman, see [Quickstart guide: Getting started using the Amazon Ads API Postman collection.](guides/get-started/using-postman-collection)

Next steps: [Make your first call](guides/amazon-marketing-cloud/get-started/make-your-first-call)


## Updating S3 bucket policy to receive query results. 
Your query execution results (`POST /amc/reporting/{instanceId}/workflowExecutions`) can either be [downloaded by retrieving a pre-signed URL](guides/amazon-marketing-cloud/reporting/workflow-management-service#download-your-report) or accessed through an S3 bucket that you configured when your instance was created. After migrating to the Amazon Ads API, to continue to accessing your query execution results from your S3 bucket, you will need to update the Identity Access Management (IAM) policy associated with your output bucket to allow your S3 bucket to receive the results.  
To update your bucket policy, perform the following steps: 

1. Log in to your AWS Account. Access your S3 Console.
2. Access your bucket through your S3 account. 
3. Navigate to the Permissions tab and scroll to the Bucket policy section.
4. Click Edit.
5. Update the bucket policy to grant 811639200769 `s3:PutObject` permissions. Depicted below is how the updated policy for a sample S3 bucket named `amc-bucket01` will look. 

```
{
    "Version": "2012-10-17",
    "Id": "AggregationDeliveryPolicy",
    "Statement": [
        {
            "Sid": "AggregationDelivery",
            "Effect": "Allow",
            "Principal": {
                "AWS": [
                    "arn:aws:iam::811639200769:root"
                ]
            },
            "Action": [
                "s3:PutObject",
                "s3:PutObjectAcl"
            ],
            "Resource": "arn:aws:s3:::amc-bucket01/*"
        },
    ]
}
```

>[WARNING] If the required permissions to the new AWS account (through the policy update) is not provided by May 1, 2024, query results will not be delivered to the your S3 bucket configured to the connected AWS account ID. 

>[NOTE] For ADU 2.0, ensure that your S3 bucket is configured as described [here](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-s3-bucket#create-an-s3-bucket).

## Migrating from Advertiser Data Upload 1.0 to Advertiser Data Upload APIs on Amazon Ads

>[WARNING] With the deprecation of AMC instance-level APIs on **August 1, 2024**, you will no longer be able to upload data using ADU 1.0, or query ADU 1.0 data using the AMC UI or APIs.

Before you migrate from Advertiser Data Upload v1 (ADU 1.0) to  Advertiser Data Upload APIs on Amazon Ads (ADU 2.0), when applying for API access, obtain approved access for the "Data Provider" scope in addition to the "Campaign Management" scope and [set up your S3 bucket](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-s3-bucket#create-an-s3-bucket) to configure it for the files you're uploading. 

If you are migrating from ADU 1.0 to AMC Advertiser Data Upload APIs on Amazon Ads (v2) and want to use the same datasets you uploaded using ADU 1.0, you will need to:

1. Recreate the datasets using Advertiser Data Upload APIs on Amazon Ads (`POST /amc/advertiserData/{instanceId}/dataSets`). Refer to [Create dataset](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-sets). 

>[NOTE] You must use a unique `dataSetID` for when creating the new datasets. For example, if the datasets created using ADU 1.0 had `"dataSetId": "myfirstdataset"`, you must create the new dataset with a different name (for example, `"dataSetId": "myfirstdatasetv2"`).

2. Upload the data to the newly created datasets (`POST {api_url}/amc/advertiserData/{instanceId}/uploads/{dataSetId}`). Refer to [Upload data to datasets](guides/amazon-marketing-cloud/advertiser-data-upload/adu-uploads). 

> [NOTE] Note that you will not be able to upload to, read, or modify ADU 1.0 datasets using the ADU 2.0 AMC Data Upload APIs. To edit datasets created using the ADU 1.0 APIs, you can continue to use the ADU 1.0 API paths.


