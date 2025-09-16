---
title: Get started with Amazon Data Firehose
description: Get started with Firehose
type: guide
interface: amazon-marketing-stream
tags:
    - Amazon Data Firehose
keywords:
    - Amazon Marketing Stream
    - Amazon Data Firehose
    - getting started guide
---

# Setting up Amazon Data Firehose manually

This guide outlines the manual steps for creating a Data Firehose stream, subscribing to the stream, and delivering data to an S3 bucket. While this example focuses on S3 as the data output location, the same principles can be applied to other destinations like Redshift and Snowflake and others, so you can customize the data delivery according to your specific needs.

>[TIP] This article outlines the steps for setting up Amazon Data Firehose manually. For a more streamlined experience, you can skip these steps and [set up Firehose with a CloudFormation template instead](guides/amazon-marketing-stream/onboarding/firehose/get-started). We strongly recommend using the CloudFormation template, unless you plan to use a data output source other than S3. 

Make sure to review the the [Getting started guide](guides/amazon-marketing-stream/onboarding/firehose/get-started) for prerequisites and next steps to subscribe to Amazon Marketing Stream.

The following steps assume that you are logged in to your AWS account [in the appropriate region](guides/amazon-marketing-stream/onboarding/firehose/get-started##get-started-with-amazon-data-firehose). 

To configure Amazon Data Firehose manually, you’ll complete these three main steps—-more details about each step are below:

1 - Create Amazon Data Firehose delivery stream 
<br>
2 - Create FIREHOSE\_SUBSCRIPTION\_ROLE
<br>
3 - Create FIREHOSE\_SUBSCRIBER\_ROLE <br>

### Step 1. Create Amazon Data Firehose delivery stream 

| [IMAGE] |   |
| ---- | ---- |
|
1 \- From [the AWS console](https://aws.amazon.com/console/), search for “Amazon Data Firehose” > click **Create Firehose stream** <br><br>2 \- In the “Source” dropdown, select **Direct PUT** <br><br>3 \- In the “Destination” dropdown, select whichever option you prefer from the list. For this example, we will select **Amazon S3** <br><br>4 \- In the “Firehose stream name” field, enter a name (or leave the default). For this example, we’ll name our stream “Firehose-test-stream”  <br><br>You can skip the **Transform and convert records** section, and jump to **Destination settings**.| ![Create Firehose stream](/_images/amazon-marketing-stream/1-create-firehose-stream.png) |


The next steps to **create the Firehose delivery stream** will involve configuring the destination settings for your Firehose stream. Since we chose S3 as the destination for this example, we’ll specify the settings for an S3 bucket. For S3, you can either browse your computer for an existing bucket, enter the URL of an existing bucket, or click **Create** to create a new bucket. For this example, we will create a new bucket. <br>

| [IMAGE] |   |
| ---- | ---- |
|
5 - Under **Destination settings**, click **Create**. This will open a new page titled **Create bucket** **→** <br> <br>6 - Be sure the correct region is displayed. Our example uses **US East (N. Virginia) us-east-1** <br><br>7 - For the “Bucket type,” the recommended option is **General purpose** <br><br>8 - Enter a name for your bucket using lowercase letters. In this example, the bucket name is **amazon-data-firehose-test-bucket** |![Create S3 bucket](/_images/amazon-marketing-stream/2-create-s3-bucket.png) |


_You can skip the **Object ownership** and other remaining sections to leave the default settings._<br>

9  - Scroll down, and click **Create bucket**

10 - Navigate back to the window that contains the **Destination settings** page

11 - Under **Destination settings**, click **Browse** to select the bucket you created:

![Firehose destination settings](/_images/amazon-marketing-stream/3-destination-settings.png "Firehose destination settings") 

*You can skip the **Dynamic partitioning** and other remaining sections to leave the default settings.* 

12 - Scroll down, and click **Create Firehose stream**. 

>[TIP]After the Firehose stream is successfully created, note the **arn** value—in the example below, the value is **arn:aws:firehose:us-east-1:345383015193:deliverystream/Firehose-test-stream**. You will need this “delivery stream” arn in the next steps when you create subscription and subscriber roles.

![Firehose stream details](/_images/amazon-marketing-stream/4-firehose-successfully-created-arn.png "Firehose stream details")  

### Step 2. Create Firehose subscription role 

The Firehose subscription role has the necessary permissions for an SNS topic to deliver data to the Firehose delivery stream. Follow the steps below to create this role:

| [IMAGE] |   |
| ---- | ---- |
|
<br>1 - From [the AWS console](https://quip-amazon.com/HZByAoTv0n0q/console.aws.amazon.com), search for "IAM" and navigate to the page<br><br>2 - From the left side navigation, click **Roles** > **Create role**<br><br>3 - Under **Trusted entity type**, select “Custom trust policy” → <br><br><br>Next, you’ll replace the code shown in the “Custom trust policy” section → |![Select trusted entity](/_images/amazon-marketing-stream/5-subscription-select-trusted-entity.png) |

4 - Replace the "Custom trust policy" with the following code:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "sns.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        },
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::926844853897:role/ReviewerRole"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

<br>
5 - Scroll down and click **Next**. You’ll arrive on the “Add permissions” page, *but we will skip this step for now*.

| [IMAGE] |   |
| ---- | ---- |
|
<br>6 - Scroll down and click **Next**. You will arrive on the “Name, review, and create” page --> <br><br>7 - For the “Role name,” enter **FIREHOSE\_SUBSCRIPTION\_ROLE**  <br><br>8 - For the “Description,” enter **This role has necessary permission for an SNS topic to deliver data to firehose delivery stream**. <br><br>9 - Under **Step 1: Select trusted entities**, paste the same “Custom trust policy” code that you entered above in Step 4 → <br><br>We will add permissions later, so you can now scroll down and click **Create role**|![Name review and create subscription role](/_images/amazon-marketing-stream/6-name-review-create-subscription-role.png) |


After you create the role, you will land on the “Roles” page, where you can see the Firehose subscription role that you just created. 

10 - Click **FIREHOSE\_SUBSCRIPTION\_ROLE**.


11 - Under **Permissions policies**, from the “Add permissions” dropdown, select “Create inline policy” as shown below:

![Create inline policy](/_images/amazon-marketing-stream/7-subscription-create-inline-policy.png "Create inline policy")  

This will take you to the “Specify permissions” page. 

| [IMAGE] |   |
| ---- | ---- |
|
12 - Toggle to the **JSON**  editor →  <br><br>13 - In the “Policy editor” section, replace the default statement code with the code shown below. <br><br>*Be sure to replace the “Resource” line with your own arn and region from the subscription account you created.* |![Specify permissions](/_images/amazon-marketing-stream/8-json-specify-permissions.png) |

Paste the following code into the “Policy editor” section. Note the **region** as well as the **arn** numeric value—these must be replaced with your own information:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "firehose:DescribeDeliveryStream",
                "firehose:ListTagsForDeliveryStream",
                "firehose:ListDeliveryStreams",
                "firehose:PutRecord",
                "firehose:PutRecordBatch"
            ],
            "Resource": "arn:aws:firehose:**us-east-1**:**123456789**:deliverystream/Firehose-test-stream",
            "Effect": "Allow"
        }
    ]
}
```

<br>
14 - Click **Next**. <br>
15 - Provide a name for your policy—e.g., **Firehose_SubscriptionRole_Policy**. <br>
16 - Click **Create policy**.

Next, you will create the FIREHOSE\_SUBSCRIBER\_ROLE. 

### Step 3. Create Firehose subscriber role

The Firehose subscriber role will allow Amazon Marketing Stream to create an SNS subscription on behalf of customers. The following steps assume that you are logged in to your AWS account in the appropriate region. 

| [IMAGE] |   |
| ---- | ---- |
|
<br>1 - From [the AWS console](https://aws.amazon.com/console/), search for **IAM** and navigate to the page <br><br>2 - From the left side navigation, click **Roles** > **Create role** <br><br>3 - Under **Trusted entity type**, select “Custom trust policy” → <br><br>Next, you’ll replace the code shown in the “Custom trust policy” section → |![Select trusted entity](/_images/amazon-marketing-stream/9-subscriber-select-trusted-entity.png) |

4 - Replace the "Custom trust policy" with the following code:


```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": [
                    "arn:aws:iam::926844853897:role/SubscriberRole"
                ]
            },
            "Action": [
                "sts:AssumeRole",
                "sts:TagSession"
            ]
        }
    ]
}
```


5 - Scroll down and click **Next**. You’ll arrive on the “Add permissions” page, *but we will skip this step for now*.<br>


| [IMAGE] |   |
| ---- | ---- |
|
<br>6 - Scroll down and click **Next**. You will arrive on the “Name, review, and create” page. <br><br> 7 - For the “Role name,” enter **FIREHOSE\_SUBSCRIBER\_ROLE** <br><br>8 - For the “Description,” enter **This role will allow Amazon Marketing Stream to create SNS subscription on behalf of customers**.  <br><br>9 - Under **Step 1: Select trusted entities**, paste the same “Custom trust policy” code that you entered above in Step 4 →  <br><br>We will add permissions later, so you can now scroll down and click **Create role** |![Name review create subscriber role](/_images/amazon-marketing-stream/10-name-review-create-subsciber-role.png) |

After you create the role, you will land on the “Roles” page, where you can see the subscriber role that you just created.


10 - Click **FIREHOSE\_SUBSCRIBER\_ROLE**.

11 - Under **Permissions policies**, from the “Add permissions” dropdown, select “Create inline policy” as shown below:

![Create inline policy](/_images/amazon-marketing-stream/11-subscriber-create-inline-policy.png "Create inline policy")  <br>This will take you to the “Specify permissions” page. 

| [IMAGE] |   |
| ---- | ---- |
|
12 - Toggle to the **JSON**  editor →  <br><br>13 - In the “Policy editor” section, replace the default statement code with the code shown below. <br><br>*Be sure to replace the “Resource” line with your own arn and region from the subscription account you created.* |![Specify permissions](/_images/amazon-marketing-stream/8-json-specify-permissions.png) | 

 14 - Paste the following code into the “Policy editor” section. Note the **region (us-east-1)** as well as the **arn (123456789)** numeric value—these must be replaced with your own information:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": "iam:PassRole",
            "Resource": "arn:aws:iam::**123456789**:role/FIREHOSE_SUBSCRIPTION_ROLE",
            "Effect": "Allow"
        },
        {
            "Action": [
                "sns:Subscribe",
                "sns:Unsubscribe"
            ],
            "Resource": "**Please use dataset and region-specific value from the below table**",
            "Effect": "Allow"
        }
    ]
}
```

|Dataset|NA|EU|FE|
|--|--|--|--|
|sp-traffic|arn:aws:sns:us-east-1:906013806264:*|arn:aws:sns:eu-west-1:668473351658:*|arn:aws:sns:us-west-2:074266271188:*|
|sp-conversion|arn:aws:sns:us-east-1:802324068763:*|arn:aws:sns:eu-west-1:562877083794:*|arn:aws:sns:us-west-2:622939981599:*|
|sd-traffic|arn:aws:sns:us-east-1:370941301809:*|arn:aws:sns:eu-west-1:947153514089:*|arn:aws:sns:us-west-2:310605068565:*|
|sd-conversion|arn:aws:sns:us-east-1:877712924581:*|arn:aws:sns:eu-west-1:664093967423:*|arn:aws:sns:us-west-2:818973306977:*|
|sb-traffic|arn:aws:sns:us-east-1:709476672186:*|arn:aws:sns:eu-west-1:623198756881:*|arn:aws:sns:us-west-2:485899199471:*|
|sb-conversion|arn:aws:sns:us-east-1:154357381721:*|arn:aws:sns:eu-west-1:195770945541:*|arn:aws:sns:us-west-2:112347756703:*|
|sb-clickstream|arn:aws:sns:us-east-1:091028706140:*|arn:aws:sns:eu-west-1:219513501272:*|arn:aws:sns:us-west-2:632322331982:*|
|sb-rich-media|arn:aws:sns:us-east-1:010312603579:*|arn:aws:sns:eu-west-1:662188760626:*|arn:aws:sns:us-west-2:618223300352:*|
|budget-usage|arn:aws:sns:us-east-1:055588217351:*|arn:aws:sns:eu-west-1:675750596317:*|arn:aws:sns:us-west-2:100899330244:*|
|sponsored-ads-campaign-recommendations|arn:aws:sns:us-east-1:084590724871:*|arn:aws:sns:eu-west-1:059061853903:*|arn:aws:sns:us-west-2:489995134625:*|
|campaigns|arn:aws:sns:us-east-1:570159413969:*|arn:aws:sns:eu-west-1:834862128520:*|arn:aws:sns:us-west-2:527383333093:*|
|adgroups|arn:aws:sns:us-east-1:118846437111:*|arn:aws:sns:eu-west-1:130948361130:*|arn:aws:sns:us-west-2:668585072850:*|
|ads|arn:aws:sns:us-east-1:305370293182:*|arn:aws:sns:eu-west-1:648558082147:*|arn:aws:sns:us-west-2:802070757281:*|
|targets|arn:aws:sns:us-east-1:644124924521:*|arn:aws:sns:eu-west-1:503759481754:*|arn:aws:sns:us-west-2:248074939493:*|
|adsp-campaigns|arn:aws:sns:us-east-1:153247821255:*|arn:aws:sns:eu-west-1:599052634802:*|arn:aws:sns:us-west-2:216875695489:*|
|adsp-campaign-flights|arn:aws:sns:us-east-1:700228448367:*|arn:aws:sns:eu-west-1:633559263003:*|arn:aws:sns:us-west-2:451213518288:*|
|adsp-adgroups|arn:aws:sns:us-east-1:222778752755:*|arn:aws:sns:eu-west-1:682324742468:*|arn:aws:sns:us-west-2:360850786875:*|
|adsp-adgroup-targets|arn:aws:sns:us-east-1:419834811630:*|arn:aws:sns:eu-west-1:764057072099:*|arn:aws:sns:us-west-2:178122609971:*|
|sp-budget-recommendations|arn:aws:sns:us-east-1:678715897637:*|arn:aws:sns:eu-west-1:158915609581:*|arn:aws:sns:us-west-2:007292432803:*|

15 - Click **Next**.<br>

16 - Provide a name for your policy—e.g., **Firehose\_SubscriberRole\_Policy**.<br>

17 - Click **Create policy**.


The next step is to [subscribe to datasets](guides/amazon-marketing-stream/onboarding/firehose/subscribing-to-datasets). 