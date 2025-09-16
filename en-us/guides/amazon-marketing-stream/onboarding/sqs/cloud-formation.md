---
title: Creating SQS queues with CloudFormation
description: A guide for setting up SQS queues and applying policies with a CloudFormation template in Amazon Marketing Stream 
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Campaign management
    - Sponsored Products
    - Sponsored Display
    - Onboarding
keywords:
    - SQS queues 
    - AWS
    - CloudFormation
    - Set up SQS queue
    - IAM policy

---
# Creating SQS queues with CloudFormation

To subscribe to an Amazon Marketing Stream dataset, you first need to set up an SQS queue and apply the relevant resource-based IAM policy. In this guide, we will show you how to use a tool called [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) to programatically set up queues and apply IAM policies. CloudFormation allows you to use an Infrastructure as Code (IaC) approach to managing your queues and policies. An IaC approach can result in consistency in configuration, minimal human error, and increased efficiency in infrastructure setup for Amazon Marketing Stream.

You can also complete this process manually by following steps 1 and 2 of the [Onboarding guide](guides/amazon-marketing-stream/onboarding).

## Before you begin

To complete this process, you must have an AWS account with permission to:

1. Access Amazon Simple Queue Service (SQS), including permission to create, update, and delete a queue and its messages.
2. Apply policy permissions on SQS queues. An SQS policy is a document that defines who is allowed to perform what actions on the queue.
3. Upload AWS CloudFormation templates.

## Procedure

1. Download the [CloudFormation template from GitHub](https://github.com/amzn/ads-advanced-tools-docs/blob/main/amazon_marketing_stream/Stream_SQS%20_CF_Template.yaml). You will use this to complete your set up with CloudFormation. 
2. Log in to the [AWS console](https://aws.amazon.com/console/). 
3. In the search bar, enter `CloudFormation`.
4. From the CloudFormation homepage, click **Create stack**.
5. Keep the default selections in the form. Under **Template source**, select **Upload a template file** and choose the CloudFormation template you downloaded in step 1. 
![Template upload screen in CloudFormation](/_images/amazon-marketing-stream/cloud-formation-template-upload.png)
6. Click **Next**.
7. Enter the relevant parameters based on the template. 
    1. Enter a name for your stack. 
    2. Choose a **StreamDatasetId** from the dropdown. [Learn more about available datasets](guides/amazon-marketing-stream/data-guide). 
    3. Enter a **StreamDestinationQueueName**. This will be the name of your SQS queue.
    4. Select a **StreamRealm**. The StreamRealm is the marketplace associated with the queue. **You must create the queue in the AWS region that corresponds to the marketplace**. The following table shows the correct AWS region for each marketplace:

    | Marketplace | AWS region |
    |-------------|------------|
    | NA | us-east-1 |
    | EU | eu-west-1 |
    | FE | us-west-2 |

    ![Enter template parameters](/_images/amazon-marketing-stream/cloud-formation-template-parameters.png)
    
    >[NOTE] If you want to subscribe to multiple datasets or regions, you need to repeat this process for each dataset and region combination, selecting the desired **StreamDatasetId** and **StreamRealm** each time.
    
8. Click **Next**.
9. On the **Configure stack options** page, keep the default settings and move on to the next page. 
10. Review the configuration and click **Create stack**. If you see a warning stating **The template has changed**, select the checkbox to continue creating your stack. 
11. Go to the SQS homepage. You should see your new queue listed.
    ![A created queue in the SQS console](/_images/amazon-marketing-stream/created-queue.png)

    >[NOTE] It may take a few minutes for your queue to appear. 
12. Click on your queue and go to the **Access policy** tab. You should see an access policy listed. 
    ![A correctly applied IAM policy](/_images/amazon-marketing-stream/applied-policy.png)

## Next steps

Once you can see your queue with a valid access policy, you can move onto the next onboarding step: [Subscribe to a dataset](guides/amazon-marketing-stream/onboarding#step-3-subscribe-to-amazon-marketing-stream-datasets).


