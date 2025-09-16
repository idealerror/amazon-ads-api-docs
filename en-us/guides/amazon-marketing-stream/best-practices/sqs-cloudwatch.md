---
title: Ensuring message delivery and processing with SQS
description: Best practices for using Amazon SQS with Amazon Marketing Stream
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Display
    - Onboarding
---

# Ensuring message delivery and processing with SQS

Amazon Marketing Stream push-based messaging system provides near real-time updates in the form of JSON messages to partner’s (tool providers, agencies, or direct advertisers) SQS queues for each of their advertisers. Partners process these messages using Lambda functions, EC2 instances, containers, or on-premises applications. However, monitoring the SQS queue is essential to guarantee that all messages are received and processed.

This guide aims to assist Amazon Marketing Stream partners in setting up comprehensive monitoring for their Amazon SQS queues. This will ensure all messages are received and processed, with no messages missed. Additionally, it will facilitate early detection of potential application issues that could be contributing factors to data inconsistencies.

We are also providing a [CloudFormation template](guides/amazon-marketing-stream/best-practices/sqs-cloudwatch#deploying-the-cloudformation-template) that helps with the set up CloudWatch dashboards and alarms. This guide will help you acquire a fundamental understanding of queue monitoring. Then you can adjust the threshold values in the CloudFormation template to tailor it to your needs.

## Understanding SQS monitoring metrics

To begin, understanding and monitoring the right set of Amazon CloudWatch metrics is crucial:

* NumberOfMessagesSent: The number of messages being added to your queue by Amazon Marketing Stream.
* NumberOfMessagesReceived: The number of messages your application is successfully receiving from the queue for processing.
* NumberOfMessagesDeleted: This indicates the count of messages that your application processed successfully and deleted from the queue.
* ApproximateNumberOfMessagesVisible: This number of Amazon Marketing Stream messages in the queue that the your app still needs to receive and process.
* ApproximateAgeOfOldestMessage: This age (in seconds) shows if any Amazon Marketing Stream messages that are stuck in the queue for too long before being received by your app.

Resource: For a detailed explanation of all SQS metrics, refer to the official [AWS documentation](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-available-cloudwatch-metrics.html).

## Setting up CloudWatch dashboards for SQS

A CloudWatch Dashboard offers a centralized view of your metrics, allowing for real-time monitoring and quick assessments. Here's how to create a sample dashboard:

1. Log in to your AWS Management Console and navigate to [CloudWatch](https://console.aws.amazon.com/cloudwatch/).
2. Click **Dashboards** > **Create dashboard**.
3. Give your dashboard a descriptive name, such as "Amazon Marketing Stream {dataset name} SQS Monitoring"
4. Click **Add widget** > **SQS**.
5. Add **Metrics Widgets**: For each key metric mentioned in [Understanding SQS monitoring metrics](guides/amazon-marketing-stream/best-practices/sqs-cloudwatch#understanding-sqs-monitoring-metrics) above, add a widget. Choose the graph style that best suits your monitoring needs (e.g., line graphs for trends or number widgets for current statuses). 
6. Customize the widget's appearance and timeframe as needed.
7. Arrange the widgets on your dashboard for clarity.
8. Click **Save dashboard**.

These dashboards will provide a quick visual overview of your queue's health, allowing you to identify potential mismatches between received and processed messages. For detailed instructions on creating and customizing CloudWatch dashboards, refer to the AWS documentation on [Using Amazon CloudWatch Dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create_dashboard.html).

## Setting up alarms

To ensure you are notified if any issues arise with your Amazon Marketing Stream SQS queues, set up alarms based on the metrics you're monitoring. 

To set up alarms, follow these steps: 

1. Go to the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/) and navigate to the **Alarms** section.
2. Click on **Create Alarm** and select the appropriate metric for the alert you want to set up. Setting alarms for the following key metrics is highly recommended: 
    1. `NumberOfMessagesSent` - This tracks new messages being added to the queue by Amazon Marketing Stream. Monitoring this is useful to understand queue traffic patterns and set baselines. Alarming on spikes/drops can indicate issues with Amazon Marketing Stream sending messages to the queue. Changes in subscriptions, including adding new advertisers and unsubscribing from existing ones, will also affect this metric.
    2. `NumberOfMessagesReceived` - This counts the number of messages successfully received from the queue. Monitoring and alarming on a drop in this metric can indicate issues with your application not running or being able to read from the queue properly.
    3. `NumberOfMessagesDeleted` - This tracks messages that were successfully processed and deleted from the queue by consumers. Monitoring this along with NumberOfMessagesReceived can indicate if your application are having issues processing messages after reading them successfully. Alarming on a divergence between these two metrics can surface such problems.
    4. `ApproximateNumberOfMessagesVisible` - This shows the number of messages currently available in the queue to be processed. Monitoring this can help understand queue backlogs and set expectations around message processing times. Setting alarms when this goes above/below certain thresholds can indicate issues with your application not keeping up.
    5. `ApproximateAgeOfOldestMessage` - This metric shows the approximate age of the oldest non-deleted message in the queue. Monitoring this is important because if messages are getting stuck in the queue for a long time, it could indicate issues with your applications that are supposed to be processing those messages. Setting an alarm when this age crosses a defined threshold can alert you to stuck messages that need attention.
3. Configure the alarm conditions, such as the threshold value and the evaluation period.
4. Specify the appropriate actions to be taken when the alarm is triggered, such as sending a notification to an Amazon SNS topic or invoking an AWS Lambda function.
5. Review and create the alarm.

For detailed instructions on Creating CloudWatch alarms for Amazon SQS metrics please refer to the [AWS documentation](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/set-cloudwatch-alarms-for-metrics.html).

## Ensuring all Stream messages are processed

To make sure all Stream messages are processed, you need to compare the number of messages sent versus received versus deleted.

### Checking messages are received

Track and compare `NumberOfMessagesSent` versus `NumberOfMessagesReceived`. Ideally, the values should be equal.

A lower `NumberOfMessagesReceived` value might indicate messages are not being retrieved for processing.

### Checking messages are processed

To confirm that all messages are processed correctly, look at `NumberOfMessagesReceived` versus `NumberOfMessagesDeleted`. While `NumberOfMessagesReceived` and `NumberOfMessagesDeleted` should be close, meaning all sent messages are processed and removed. However, there can be differences depending on your queue configuration. Specifically, a high max retries setting can lead to a larger `NumberOfMessagesReceived` count. Imagine a message failing processing nine times with retries, resulting in nine receives before a single successful deletion on the 10th attempt. Hence, it is recommended not to set alarms based on this metric difference until these nuances are addressed.

A lower `NumberOfMessagesDeleted` value suggests messages are received but not processed successfully (e.g., errors).

#### Setting up a CloudWatch alarm for processed messages

You can set up an alarm to check for differences in `NumberOfMessagesReceived` versus `NumberOfMessagesDeleted`. You should create alarms for significant discrepancies between these metrics, such as a growing gap between `NumberOfMessagesSent` and `NumberOfMessagesDeleted`, which can alert you to processing delays or failures. You can leverage metric math to accomplish this task. 

1. Navigate to the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/).
2. Select "All Metrics" and then navigate to SQS to find and select the metrics mentioned above.
3. Once you've chosen the metrics, click on "Graphed metrics."
4. In the graphed metrics view, select "Math expression" and start with an empty expression. This action will create a new line for the expression.
5. Define the expression as follows:
    `IF((NumberOfMessagesSent - NumberOfMessagesDeleted) > Threshold, 1, 0)`

    If this difference is greater than the specified threshold, it returns 1; otherwise, it returns 0.
6. After defining the expression, you can create an alarm based on this new metric. Set the alarm to trigger whenever the value is equal to 1, indicating a discrepancy between the two metrics.

For detailed instructions on metric math in CloudWatch, refer to the [AWS documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/using-metric-math.html).


## Monitoring best practices

* Regular Audits: Periodically review these metrics to identify any long-term trends or recurring issues in your message processing pipeline, allowing you to implement optimizations or capacity adjustments.
* Error Handling and Retry Logic: Implement robust error handling in your message processing logic, including retries for temporary issues and dead-letter queues for messages that cannot be processed after several attempts. This helps reduce the discrepancy between received and deleted messages.
* While SQS offers visibility into Amazon Marketing Stream message delivery, it cannot directly monitor your processing application. Implementing application-specific monitoring solutions is crucial for ensuring successful message processing beyond SQS delivery.  

## Note on AWS CloudWatch pricing

Amazon CloudWatch offers a free tier with basic usage, however it's worth knowing few key points:

* For dashboards, you can have up to 3 custom dashboards referencing up to 50 metrics each per month for free in the AWS Free Tier. Beyond that, there is a charge of $3 per custom dashboard per month.
* For alarms, you can have 10 alarm metrics for free in the AWS Free Tier if the alarms list metrics directly (not using a Metrics Insights query). Beyond 10 alarms, standard resolution alarms cost $0.10 per metric per month if listing metrics directly, or $0.10 per alarm-metric analysed if using a Metrics Insights query.
* If you enable Amazon CloudWatch Anomaly Detection on 10 standard resolution metrics per month and only want to alarm on 5 of those metrics, you will create 5 standard resolution anomaly detection alarms.
* One standard resolution anomaly detection alarm costs $0.10 * 3 standard resolution metrics per alarm = $0.30 per month.So for the example of 5 standard resolution anomaly detection alarms, the monthly cost would be: 5 alarms * $0.30 per alarm = $1.50 per month
* Anomaly Detection is available only with standard resolution alarms, not high resolution alarms.

See the [official pricing details](https://aws.amazon.com/cloudwatch/pricing/) for the latest information.

## Deploying the CloudFormation template

We have provided a CloudFormation template to help you quickly set up the CloudWatch dashboards and alerts that you need to monitor your Stream integration.

Deploying template will create:

* A CloudWatch dashboard
* An SNS topic for notifications
* Several alarms to monitor key SQS metrics

### Steps

1. Download the [CloudFormation template from GitHub](https://github.com/amzn/ads-advanced-tools-docs/blob/main/amazon_marketing_stream/Stream_SQS%20_CF_Template.yaml). You will use this to complete your set up with CloudFormation. 
2. Log in to the [AWS console](https://aws.amazon.com/console/). 
3. In the search bar, enter `CloudFormation`.
4. From the CloudFormation homepage, click **Create stack**.
5. Keep the default selections in the form. Under **Template source**, select **Upload a template file** and choose the CloudFormation template you downloaded in step 1. 
![Template upload screen in CloudFormation](/_images/amazon-marketing-stream/cloud-formation-template-upload.png)
6. Click **Next**.
7. Provide the SQS queue name and email address for notifications.
8. Review and create the stack.
9. Monitor stack creation progress.
10. Verify the created resources (CloudWatch dashboard, SNS topic, and alarms).
11. Test the alarms by triggering the monitored conditions.
12. Review notifications received via email.
