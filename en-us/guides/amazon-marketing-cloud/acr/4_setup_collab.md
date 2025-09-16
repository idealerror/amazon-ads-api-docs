---
title: Amazon Marketing Cloud - AWS Clean Rooms
description: AMC - AWS Clean Rooms
type: guide
interface: api
---
# Set up a collaboration instance

AMC customers can use AWS Clean Rooms for securely associating 1P data to a collaboration and collecting data outputs from queries across their data and AMC data.

## Set up AMC-AWS Clean Rooms collaboration

To set up AMC-AWS Clean Rooms, create an AWS Clean Rooms backed - AMC instance that you will use to create a collaboration with AWS Clean Rooms.

To create a collaboration instance, it is assumed that you have performed the following tasks:

- [Signed up for AWS](https://signin.aws.amazon.com/signup?request_type=register)
- [Created an AWS administrator user for AWS Clean Rooms](https://docs.aws.amazon.com/clean-rooms/latest/userguide/setting-up.html#setting-up-create-iam-user)

Note: Only users with Admin permissions for both AMC and AWS Clean Rooms can perform the steps listed on this page.

## Create an AWS Clean Rooms backed AMC instance

Use the POST operation of the `/amc/instances` endpoint to create an instance.

The mandatory header parameters are listed below:

| Header                               | Description                                                                                                                                                                          |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Amazon-Advertising-API-ClientId      | The client ID related to a Login with Amazon application.                                                                                                                            |
| Amazon-Advertising-API-MarketplaceId | The marketplace identifier for the advertiser's [marketplace](https://advertising.amazon.com/API/docs/en-us/guides/amazon-marketing-cloud/get-started/make-your-first-call#marketplace). |
| Amazon-Advertising-API-AdvertiserId  | The AMC account identifier. See [Retrieve accounts](https://advertising.amazon.com/API/docs/en-us/guides/amazon-marketing-cloud/admin/account-management).                               |
| Authorization                        | Login with Amazon token in the form of Bearer {token}.                                                                                                                               |

In the request body, provide the following mandatory parameters:

- `instanceName`: Name of the AMC instance.
- `awsAccountId`: The primary AWS account ID to which this instance will be connected and query results received.
- `acrBacked`: Set to true for AWS Clean Rooms backed instances.

> [WARNING] When creating an ACR-backed AMC account, do not include the S3 bucket name. This will be required later when joining the AWS Clean Rooms collaboration. Requests that the `s3BucketName` in the request body will be rejected with a 400 error.

You can optionally pass the name of the advertiser (`advertiserName`) associated with the instance you are creating, ensure that the advertiser name can only contain letters (both uppercase and lowercase), numbers, and spaces. Special characters (except spaces) are not allowed in the advertiser name, and the advertiser name cannot start or end with a space.

### Sample request

```
{
   "instanceName": "my_collaboration_instance",
    "awsAccountId": "682015411111",
    "acrBacked": "true"
}
```

The following optional parameters can be passed as the request body:

- `advertiserName:` Optionally, pass the name of the advertiser (`advertiserName`) associated with the instance you are creating, ensure that the advertiser name can only contain letters (both uppercase and lowercase), numbers, and spaces. Special characters (except spaces) are not allowed in the advertiser name, and the advertiser name cannot start or end with a space.

A successful request creates a creates an instance in AMC and a collaboration in AWS Clean Rooms with you as a member.

The name and description of your collaboration instance is listed in the `acrCollaboration` section of the response. In our sample response below, your collaboration instance created is named: AMC amc11111111.

### Sample response:

```
  {

    "instance": {
    "instanceId": "amc11111111",
    "instanceName": "myacrinstance",
	  "instanceType": "STANDARD",
	  "customerCanonicalName": "Advertiser A",
	  "creationStatus": "REQUESTED",
	  "creationDatetime": null,
	  "s3BucketName": "",
  	"awsAccountId": "123456789000",
	  "entities": [
	     "NA"
		  ],
    "dataUploadAwsAccountId": "811639200769",
     "apiEndpoint": **null**,
      "acrCollaboration": {
         "id": "",
         "name": "AMC amc11111111",
         "description": "AMC Clean Room collaboration for instance amc11111111"
       },
       "optionalDatasets": [],
       "advertiserTypes": []

```

In case you want to your collaboration instance to access data from more than one AWS account, you can create a **multi-party collaboration instance**. To do so, you will need to include the additional AWS account IDs (of the parties) to the instance you are creating. The associated AWS accounts can bring their data to the collaboration instance for AMC to be able to perform measurement queries on them and also create audiences. This use case is particularly suited for advertisers or partners that have data stored in more than one AWS account.

The following is a sample request body to create a multi-party collaboration instance:

```
{
   "instanceName": "myacrinstance",
   "advertiserName": "Advertiser A",
   "awsAccountId": "123456789000",
   "acrBacked": "true",
   "acrCustomerPartners": [
      {
         "awsAccountId": "123456789001",
         "displayName": "Source Data 1"
      },
      {
         "awsAccountId": "123456789000",
         "displayName": "Source Data 2"
      }
   ]
}
```

A successful request creates an instance in AMC and a collaboration in AWS Clean Rooms with the `awsAccountId` as a member that accepts data from the associated AWS data provider IDs listed under `acrCustomerPartners`.

NOTE: The query results will be sent to the primary AWS account ID.

The collaboration in AWS Clean Rooms will also house members as listed below:

- **Advertiser**: You
- **Amazon Advertising Data Provider**: The AWS account owned by AMC that contains Amazon's data.
- **Lookalike Audience Generator**: The AWS account owned by AMC, used to generate lookalike audiences.
- **Advertiser Uploaded Data Provider**: The AWS account owned by AMC that contains the 3P advertiser data uploaded into AMC.
- **Third-party Paid Data Provider**: The AWS account owned by AMC that contains the subscribed paid feature data.
- **AMC Results Receiver**: The AWS account owned by AMC, used to create audiences in Amazon DSP.
- **Query Submitter**: The AWS account owned by AMC that submits the queries to the AWS Clean Rooms service.
- **AMC service**: The AWS account owned by AMC that facilitates the creation of collaborations.

To complete creating your instance, you will need accept the collaboration. You can accept the collaboration either by accessing AWS Clean Rooms through your AWS console or by logging into your AMC console. The collaboration instance will be operational once you [create a membership](#create-membership) in the primary AWS account ID, after which  you will be able to run queries.

> [WARNING] After your collaboration instance is created, you must add advertisers to the instance. For details, see [Add or remove advertisers](guides/amazon-marketing-cloud/admin/instance-management#add-or-remove-advertisers) from/to your instance.

You will also need permissions to create service roles or to work with an administrator in your organization to create these roles.
See [Set up service roles for AWS Clean Rooms](https://docs.aws.amazon.com/clean-rooms/latest/userguide/setting-up-roles.html) for more details.

### Request permissions

At this point, you should have:

- [Created IAM roles for collaboration members](https://docs.aws.amazon.com/clean-rooms/latest/userguide/setting-up.html#create-role-DP)
- [Created a service role to read data](https://docs.aws.amazon.com/clean-rooms/latest/userguide/setting-up.html#create-service-role-procedure)
- [Created a service role to receive results](https://docs.aws.amazon.com/clean-rooms/latest/userguide/setting-up.html#create-role-write-results) (if you are the result receiver)

> [WARNING] **For the current deployment, only the *us-east-1* region is supported. For all parameters requesting a region, use *us-east-1*.**

### Manage collaboration membership

When the AMC collaboration instance is provisioned, you are notified through either the AWS Clean Rooms Account, under **Collaborations Available to join**:

Or, through a banner notification that shows up when you log in to the AMC console:

Both options allow you to [create a membership](#create-membership) for the collaboration instance the AMC team created. A [membership](https://docs.aws.amazon.com/clean-rooms/latest/userguide/create-membership.html) is a resource that is created when a member joins a collaboration in AWS Clean Rooms. You can create and manage memberships.

### Create membership

[Create membership](https://docs.aws.amazon.com/clean-rooms/latest/userguide/create-membership.html) to join the collaboration for the instance and set the destination of your query results (Amazon S3 bucket), the format of query results, and member abilities.

> [WARNING] Ensure that the S3 bucket for receiving the query results is a different location from the S3 bucket that contains the data that you bring into the collaboration.

#### Create membership using the AWS Clean Rooms console

To create a membership using the AWS Clean Rooms console:

1. Access the AWS Clean Rooms console.
2. Click **View collaborations** and then navigate to the **Available to join** tab.
3. Click the instance you created (in our example, the instance created is AMC amc11111111) to view the details of the collaboration and then click **Create membership**.
4. On the [Create membership](https://docs.aws.amazon.com/clean-rooms/latest/userguide/create-membership.html) page, review the collaboration **Overview** details (including your assigned member abilities).
5. In the Query results setting section, specify the **Results destination in Amazon S3** by entering the S3 destination or choose **Browse S3** to select from a list of available S3 buckets.
   - Choose the **Result format** (either **CSV** or **PARQUET**).
6. Click **Create membership** to join the collaboration for the instance.
7. Your membership is created and associated with the AMC collaboration instance.

#### Create membership using the AMC console

To create a membership using the AMC console:

1. Access the AMC console.
2. Navigate to the **Instances info** tab in the AMC console.
3. Click **Go to AWS Clean Rooms** link to access your AWS Clean Rooms account collaboration.
4. The **Create membership** page appears in the AWS Clean Rooms console.
5. On the [Create membership](https://docs.aws.amazon.com/clean-rooms/latest/userguide/create-membership.html) page, review the collaboration **Overview** details (including your assigned member abilities) set the destination of your query results (Amazon S3 bucket), the format of query results, specify your service access permissions.
6. Click **Create membership** to join the collaboration for the instance.
7. Your membership is created and associated with the AMC collaboration instance.

   Note: Updating the status of instance in AMC can take several minutes.

You can go back to the AMC console to view the status of your instance. Once the status of your instance is updated to Active , you can invite users from your organization to join the collaboration instance in AMC. Users can be invited using either [the AMC console](https://advertising.amazon.com/help/GJHUF3HF98KVR4RM) or by using [the user management APIs.](guides/amazon-marketing-cloud/admin/account-management#invite-users)
Once your users accept your invite, they can  start writing queries using Amazon data. The results of your queries will be sent to the Amazon S3 bucket that you configured when creating your membership.

You can now [bring your data](guides/amazon-marketing-cloud/acr/6_upload_event) to your collaboration instance or perform a [rule-based matching of signals](guides/amazon-marketing-cloud/acr/7_upload_identity) to integrate your 1P data with your collaboration instance or [upload data](guides/amazon-marketing-cloud/advertiser-data-upload/advertiser-data-overview) using the AMC Data Upload.

#### Manage your collaboration instance membership

To make any changes to your collaboration profile and membership, you will need to again access the AWS Clean Rooms console to be able to [manage your collaboration details](https://docs.aws.amazon.com/clean-rooms/latest/userguide/manage-collaboration.html).

When you [leave a collaboration](https://docs.aws.amazon.com/clean-rooms/latest/userguide/manage-collaboration.html#leave-collab), you will not be allowed to access your collaboration instance in AMC any further. However, the results of your queries will remain in the Amazon S3 bucket associated with your collaboration instance.

Other members in the collaboration can see the status of your membership through the AWS Clean Rooms console under **Collaborations \> your collaboration \> Members**. The **Member status** is updated as **Removed**.

In the AMC console, the **Instance info** tab shows your AWS Clean Rooms collaboration status as **Left**.

> [WARNING] Once you leave the collaboration instance, you will not be able to join the same instance again. To join a collaboration instance again, you will have to request for a new collaboration instance. The new instance will **not** be reinstated with any data that your previous instance held.
