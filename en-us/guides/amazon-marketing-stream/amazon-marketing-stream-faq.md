---
title: Amazon Marketing Stream FAQ
description: FAQs for Amazon Marketing Stream
type: guide
interface: amazon-marketing-stream
tags:
- Reporting
- Sponsored Products
- Sponsored Display
- Onboarding
keywords:
- subscribe to data
- IAM policy
- SQS queue
- schema
- marketplace identifier
---

# Amazon Marketing Stream FAQ

## Subscription Management

### Q: My subscription is stuck in `PENDING_CONFIRMATION` status for more than 1 day. What should I do?

**A:** When your [Amazon Marketing Stream](guides/amazon-marketing-stream/overview) subscription with an [SQS](guides/amazon-marketing-stream/onboarding/sqs/get-started) destination remains in `PENDING_CONFIRMATION` status for an extended period, it indicates that the AWS SNS subscription confirmation process has not completed successfully.

The root cause typically stems from the AWS SNS confirmation [workflow](guides/amazon-marketing-stream/onboarding/sqs/get-started#subscription-status-flow). When you create a Marketing Stream subscription, Amazon Marketing Stream automatically creates an SNS topic and attempts to subscribe your specified destination SQS queue. SNS then sends a confirmation message that must be acknowledged within 2 days. If this confirmation isn't processed correctly, the subscription remains in a pending state for 2 days. After this period, if the confirmation is still not processed, the subscription will move to a `FAILED_CONFIRMATION` state.

**Common underlying issues include:**

- Missing or misconfigured IAM permissions that block SNS from delivering messages.
- Your application's message processing logic doesn't properly handle SNS confirmation messages.
- SQS queue policies that don't allow cross-account message delivery.

**Solution Steps:**

1. Check your SQS queue for unprocessed confirmation messages.
2. Verify your endpoint is accessible and responding correctly to SNS confirmation requests.
3. Ensure your IAM policies allow SNS to deliver messages from the Amazon Marketing Stream account for your specific dataset and region to your SQS queue. Refer to the documentation for the correct account ID for your dataset and region.
4. Always use [CloudFormation templates (IaC)](https://github.com/amzn/ads-advanced-tools-docs/tree/main/amazon_marketing_stream) to ensure consistent infrastructure deployment and eliminate manual configuration errors.
5. Contact [Amazon Ads API support](support/overview) to resend the confirmation message if a subscription is urgently needed and must use same destination. Alternatively, create a new destination queue with proper IAM permissions and recreate subscriptions. Unconfirmed subscriptions move to `FAILED_CONFIRMATION` after 2 days.

### Q: My subscription shows `FAILED_PROVISIONING` status when I try to create it. How do I fix this?

**A:** `FAILED_PROVISIONING` status occurs when Amazon Marketing Stream cannot properly set up the Firehose infrastructure needed to deliver data to your specified Firehose destination.

 This status is often due to permission or configuration issues with your Firehose destination resource. Amazon Marketing Stream attempts to validate that it can deliver messages to your Firehose but encounters access restrictions.

**Common causes include:**

- Insufficient IAM permissions on your Firehose destination resource
- Missing or incorrect resource policies on the destination
- Cross-account access issues when resources are in different AWS accounts
- Destination resource doesn't exist or has been deleted
- Firehose delivery stream configuration errors

**Solution Steps:**

1. Verify your Firehose destination resource exists and is accessible.
2. Check IAM policies allow Amazon Marketing Stream to send messages to your Firehose destination.
3. Ensure resource policies permit cross-account access if applicable.
4. Verify the delivery stream configuration is correct.
5. Test permissions by manually sending a test message to your destination.
6. Review AWS CloudTrail logs for specific permission denial errors.

### Q: I can't see my subscriptions when I call `GET /streams/subscriptions`, but I know I have active subscriptions. What's wrong?

**A:** An empty response from [GET /streams/subscriptions](amazon-marketing-stream/openapi#tag/Stream-Subscription/operation/GetStreamSubscription) despite having active subscriptions can occur due to several reasons, primarily related to authentication and the refresh token being used. 

**Authentication and Authorization Issues:**

- Using a refresh token generated with a different email address
- Missing marketplace-specific permissions
- Outdated or revoked refresh tokens
- Account permissions changed after subscription creation

**Refresh Token Association:**

Stream subscriptions are strictly associated with the refresh token (generated using a specific email address) and your client application. If you're using a new refresh token generated with new email (even for the same advertiser), you won't be able to see subscriptions created with a previous refresh token with a different email address.

**Detached Stream Subscriptions:**

These are subscriptions created with a refresh token that was generated using an email address that is no longer associated with the advertiser account.

Common scenarios include:

- The email address used to generate the refresh token is no longer accessible.
- The person associated with that email has left the company.

**Solution Steps:**

1. **Verify your current authentication:**
  - Ensure you're using the correct refresh token generated with the right email address for API access to view the subscriptions.
  - The refresh token should be associated with the advertiser account for which you're trying to view subscriptions.
  - Check that the Ads API credentials derived from this refresh token have current access to the target advertiser account.
  - Confirm you're querying the correct marketplace (US, EU, FE).
2. **If using a new refresh token:**
  - You won't be able to see or manage subscriptions created with previous refresh tokens (if different email address is being used).
  - You may need to archive old subscriptions (using the old refresh token if available) and create new ones with the current refresh token.
3. **For detached subscriptions:**
   - If you still have access to the old refresh token, archive the historical subscriptions and create new ones.
   - If you don't have access to old refresh tokens, contact Amazon Ads API support for assistance with archiving subscriptions.
4. **General troubleshooting:**
   - Generate new access tokens using your refresh token if they may have expired.
   - Confirm account permissions haven't been revoked since subscription creation.
   - Try listing subscriptions for different advertiser accounts you have access to.

**Important Notes:**

- Changing the email association for historical subscriptions is not supported due to security reasons.
- It's important to securely store refresh tokens to maintain access to your subscriptions.
- If you continue to experience issues after following these steps, contact Amazon Ads API support for further assistance. They can help verify your account status and provide guidance on managing your subscriptions effectively.

### Q: How do I increase my subscription limits? I'm getting "subscription limit exceeded" errors.

**A:** Amazon Marketing Stream has default limits on the number of subscriptions per client to ensure system stability and fair resource allocation.

Subscription limits are enforced at the client ID level and vary by dataset type. When you reach these limits, you'll receive errors preventing new subscription creation. Limits exist to prevent resource exhaustion and ensure reliable service for all users.

**Solution Steps:**

1. Review your current subscription usage across all datasets.
2. Archive unused or inactive subscriptions to free up quota.
3. Consolidate multiple subscriptions where possible.
4. Contact [Amazon Ads API support](support/overview) with your business justification for higher limits.
5. Provide details about your use case and expected volume.
6. Consider using fewer, more comprehensive subscriptions instead of many specific ones.

## Data Delivery and Quality

### Q: I'm not receiving any data from my active subscriptions. What should I check?

**A:** When active subscriptions aren't delivering data, the issue typically lies in the message delivery pipeline or your data processing setup. This can occur due to several factors in the data flow from Amazon Marketing Stream to your destination (SQS queue or Kinesis Firehose). 

Even though your subscription shows as `ACTIVE`, various components in the delivery chain might be preventing data from reaching your destination.

**Common causes include:**

- SNS subscription was accidentally deleted or unsubscribed.
- SQS queue permissions changed after subscription creation.
- Dead letter queue is capturing failed messages.
- Your application isn't polling the queue correctly.
- Messages are being processed but not logged/stored properly.
- If using Kinesis Firehose as destination, delivery stream has configuration/permission issues.

**Solution Steps:**

1. Check your SQS queue for unprocessed messages.
2. Verify SNS subscription is still active and confirmed.
3. Review dead letter queue for failed message deliveries.
4. Test your message processing logic with sample data.
5. Check CloudWatch metrics for your SQS queue activity.
6. If using Kinesis Firehose as destination, verify the delivery stream status and verify whether delivery stream has any configuration/permission issues.

### Q: I'm seeing data discrepancies between Marketing Stream and the Ads Console. Which should I trust?

**A:** Data discrepancies between Marketing Stream and Ads Console can occur due to timing differences, data processing delays, or different aggregation methods.

Marketing Stream provides near real-time data as events occur, while the Ads Console may show data that's been processed through different aggregation pipelines. Small discrepancies are normal, but significant differences usually indicate a specific issue.

**Common causes of discrepancies include:**

- Different attribution windows between systems
- Time zone differences in data aggregation
- Delayed data processing in one system
- Different filtering or aggregation logic
- Upstream data source issues affecting one system

**Solution Steps:**

1. Compare data over longer time periods to identify patterns.
2. Check if discrepancies are consistent or sporadic.
3. Verify time zone settings in both systems.
4. Review attribution window settings for conversion data.
5. Check for any known upstream data issues during the time period.
6. Contact support if discrepancies exceed consistently.
7. Document clear examples with timestamps, advertiser IDs, marketplace and values..

### Q: I'm receiving duplicate messages in my SQS queue. How do I handle this?

**A:** Duplicate messages in SQS queues can occur due to the distributed nature of the Amazon Marketing Stream delivery system and SQS's at-least-once delivery guarantee.

This is a normal characteristic of distributed messaging systems. SQS doesn't guarantee exactly-once delivery, so your application should be designed to handle duplicate messages gracefully through idempotent processing.

**Common causes include:**

- Network issues during message delete operations
- SQS visibility timeout expiration
- Multiple consumers processing the same message
- System failover scenarios
- Message redelivery after processing failures
- SNS-SQS integration (SNS also has at-least-once delivery semantics)

**Solution Steps:**

1. Implement idempotent message processing in your application.
2. Use message deduplication based on idempotencyId in the data.
3. Store processed messages to detect and skip duplicates.
4. Configure appropriate SQS visibility timeouts.
5. Use distinct idempotencyId in your queries when analyzing data.

### Q: Amazon Marketing Stream data contains "Placeholder keyword" values. What does this mean?

**A:** "Placeholder keyword" appears in Amazon Marketing Stream data when the upstream advertising system hasn't yet populated actual dimension values for certain fields.

This occurs when the advertising system is still processing campaign data or when certain dimensions are temporarily unavailable. It's a temporary state that typically resolves as the system completes data processing.

- Newly created campaigns still being processed
- System maintenance affecting dimension data
- Upstream data pipeline delays
- Keyword matching still in progress
- Campaign optimization processes running

**Solution Steps:**

1. Filter out records with "Placeholder keyword" in your data processing.
2. Monitor for resolution of placeholder values over time.
3. Contact support if placeholders persist for more than 24 hours.
4. Consider these records as incomplete data in your analytics.
5. Set up alerts for unusual increases in placeholder values.

### Q: I'm seeing negative values for impressions, clicks, or spend in my data. Is this normal?

**A:** Negative values in Marketing Stream data typically represent corrections or adjustments to previously reported metrics.

These corrections occur when the advertising system identifies and corrects invalid traffic, billing adjustments, or data quality issues. Negative values help maintain data accuracy by offsetting previously reported incorrect positive values.

**Solution Steps:**

1. Include negative values in your data aggregation to get accurate totals.
2. Don't filter out negative values as they're legitimate corrections.
3. Monitor for unusual patterns in negative value frequency.
4. Aggregate data over longer periods to see net effects.
5. Contact support if negative values seem disproportionately large.
6. Implement proper data validation that accounts for corrections.

## Technical Integration

### Q: What IAM permissions do I need to receive Marketing Stream data in my SQS queue?

**A:** Your SQS queue needs specific IAM permissions to allow Amazon Marketing Stream to deliver messages successfully. 

The permissions must allow the Amazon Marketing Stream service accounts (which vary by dataset and region) to send messages to your queue. For more information, see [AMS: Data Guide](guides/amazon-marketing-stream/data-guide).

**Required permissions include:**

- SQS:SendMessage for the Marketing Stream service accounts
- SQS:GetQueueAttributes for queue validation
- Cross-account access if resources are in different accounts
- SNS subscription confirmation permissions

**Solution Steps:**

1. Create an SQS queue policy allowing Marketing Stream service account access.
2. Ensure your IAM user/role has permissions to create subscriptions.
3. Configure cross-account permissions if using different AWS accounts.
4. Use AWS Policy Simulator to validate permission configurations.
5. Review CloudTrail logs for permission-related errors.

**Example SQS Queue Policy:**

```json
{
    "Version": "2008-10-17",
    "Id": "{REPLACE_WITH_ANY_VALUE}",
    "Statement": [
        {
            "Sid": "{REPLACE_WITH_ANY_VALUE}",
            "Effect": "Allow",
            "Principal": {
                "Service": "sns.amazonaws.com"
            },
            "Action": "SQS:SendMessage",
            "Resource": "{REPLACE WITH ARN Of SQS DESTINATION QUEUE}",
            "Condition": {
                "ArnEquals": {
                    "aws:SourceArn": "arn:aws:sns:{REPLACE WITH REGION}:{REPLACE WITH MARKETING STREAM REGION SPECIFIC ACCOUNT}:*"
                }
            }
        },
        {
            "Sid": "{REPLACE_WITH_ANY_VALUE}",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::926844853897:role/ReviewerRole"
            },
            "Action": "SQS:GetQueueAttributes",
            "Resource": "*"
        }
    ]
}
```

### Q: How do I handle Marketing Stream data with Data Firehose?

**A:** [Data Firehose](guides/amazon-marketing-stream/onboarding/firehose/overview) provides a managed way to deliver Marketing Stream data directly to S3, Redshift, or other destinations without managing SQS queues.

Firehose automatically handles batching, compression, and delivery of your Marketing Stream data. However, it requires proper configuration and permissions to work correctly with the Marketing Stream service.

**Key configuration requirements include:**

- Proper IAM roles for Firehose delivery
- Destination configuration (S3 bucket, Redshift cluster, etc.)
- Error handling and backup configurations
- Data transformation settings if needed
- Delivery stream naming and buffering settings

**Solution Steps:**

1. Create a Kinesis Data Firehose delivery stream.
2. Configure IAM roles with necessary permissions.
3. Set up your destination (S3 bucket, etc.) with proper permissions.
4. Configure error handling and backup destinations.
5. Test the delivery stream with sample data.
6. Create Marketing Stream subscription using Firehose ARN.
7. Monitor delivery metrics in CloudWatch.

### Q: My subscription worked before but now shows SUSPENDED status. What happened?

**A:** [SUSPENDED status](guides/amazon-marketing-stream/onboarding/sqs/get-started#subscription-status-flow) typically occurs when Amazon Marketing Stream detects issues with data delivery to your destination or changes in account permissions.

Amazon Marketing Stream avoids sending data when there are consistent delivery failures or unauthorized subscriptions, which helps prevent unnecessary resource consumption and ensures data is only sent when proper access and access to advertiser data are confirmed.

**Common causes include:**

- SQS queue was deleted or permissions changed
- SNS subscription was manually unsubscribed
- Ads account permissions were revoked
- Destination resource became inaccessible
- Repeated delivery failures exceeded retry limits
- User lost access to advertiser data

**Solution Steps:**

1. Check if your destination resource still exists and is accessible.
2. Verify SNS subscription is still active and confirmed.
3. Review IAM permissions for any recent changes.
4. Check CloudWatch logs for delivery failure patterns.
5. Test connectivity to your destination manually.
6. Consider creating a new subscription.

>[NOTE] After being suspended, subscriptions cannot automatically resume or become `ACTIVE` even after fixing the underlying issues. You'll need to create a new subscription.

## Troubleshooting and Monitoring

### Q: How can I monitor the health of my Marketing Stream integration (SQS)?

**A:** Effective monitoring of your Amazon Marketing Stream integration requires tracking multiple metrics across the entire data pipeline.

Monitoring should cover subscription status, message delivery, data processing, and business metrics to ensure your integration remains healthy and provides reliable data for your applications. Set up alerts for data gaps rather than continuously polling subscription status.

**Key monitoring areas include:**

- Subscription status and health checks
- SQS queue metrics (message count, age, processing rate)
- Data processing success/failure rates
- Data freshness and completeness
- Error rates and types
- Business metric validation

**Solution Steps:**

1. Set up CloudWatch alarms for SQS queue metrics.
2. Configure alerts for unexpected gaps in data reception.
3. Track data processing success rates in your application.
4. Implement data freshness checks.
5. Set up alerts for unusual data patterns or volumes.
6. Create dashboards for key integration metrics.
7. Establish baseline metrics for comparison.

**Recommended CloudWatch Metrics:**

- ApproximateNumberOfMessages
- ApproximateAgeOfOldestMessage
- NumberOfMessagesSent
- NumberOfMessagesReceived
- NumberOfMessagesDeleted

>[NOTE] Amazon Marketing Streams provides [CloudFormation template](https://github.com/amzn/ads-advanced-tools-docs/blob/main/amazon_marketing_stream/Stream_CloudWatch_CF_Template.yaml) that can be used to monitor your health of the queue and setting up alerts, please use this, highly recommended.

>[NOTE] If you notice unexpected gaps in data reception, check your subscription status and review the troubleshooting guide for potential issues.

### Q: What should I do if I suspect I'm missing data from my stream (SQS)?

**A:** Missing data can occur due to various issues in the delivery pipeline, processing logic, or upstream data sources.

Data gaps can be difficult to detect without proper monitoring, so it's important to have systematic approaches for identifying and addressing missing data scenarios.

**Common causes of missing data include:**

- Message processing failures in your application
- SQS message visibility timeout issues
- Dead letter queue capturing failed messages
- Application downtime during data delivery

**Solution Steps:**

1. Check your SQS dead letter queue for failed messages.
2. Review application logs for processing errors during the suspected time period.
3. Compare data volumes with historical patterns.
4. Verify SQS queue metrics in CloudWatch for message delivery.
5. Check for application downtime or deployment issues.
6. Contact support with specific time ranges and missing data details.
7. Implement data completeness monitoring for early detection.

### Q: How do I handle time zones correctly in Marketing Stream data?

**A:** Amazon Marketing Stream data includes timestamp fields that need proper time zone handling for accurate reporting and analysis.

The `time_window_start` field in Amazon Marketing Stream data represents the beginning of the reporting period and is provided in ISO 1806 format. Your application needs to handle time zone conversions appropriately for your business requirements.

**Key considerations include:**

- All timestamps are in ISO 1806 format
- Convert to local time zones for reporting
- Handle daylight saving time transitions
- Maintain consistency across different data sources
- Account for marketplace-specific time zones

**Solution Steps:**

1. Parse timestamps from ISO 1806 format to your desired time zone.
2. Use proper date/time libraries that handle time zone conversions.
3. Test time zone handling around daylight saving time transitions.
4. Document your time zone handling approach for consistency.
5. Validate time zone conversions with known data points.

## Account and Access Management

### Q: I'm getting 401 Unauthorized errors when trying to access Marketing Stream APIs. How do I fix this?

**A:** 401 Unauthorized errors indicate authentication issues with your Ads API credentials or token configuration.

This typically occurs when your access tokens are expired, invalid, or don't have the necessary scopes for Marketing Stream access. The error can also happen if you're using credentials for the wrong marketplace or advertiser profile.

**Common causes include:**

- Expired access tokens
- Invalid or revoked refresh tokens
- Missing required API scopes
- Using credentials for wrong marketplace
- Incorrect advertiser profile ID
- Ads API credentials not properly configured

**Solution Steps:**

1. Refresh your access tokens using your refresh token.
2. Verify your Ads API credentials have access to Ads API.
3. Check that you're using the correct Ads API endpoint.
4. Confirm your advertiser profile ID is correct.
5. Test authentication with a simple Ads API call first.
6. Review API documentation for required scopes and permissions.

### Q: Can I use one SQS queue to receive data from multiple Marketing Stream subscriptions?

**A:** Yes, you can configure multiple Marketing Stream subscriptions to deliver data to the same SQS queue, but this requires careful message handling in your application.

Using a single queue for multiple subscriptions can simplify infrastructure management but requires your application to differentiate between different data types and sources when processing messages.

>[WARNING] When dealing with datasets of varying volumes, using a single standard SQS queue may cause processing delays. Because standard SQS queues don't guarantee message ordering, high-volume datasets can potentially overwhelm the queue and delay the processing of lower-volume, potentially more time-sensitive datasets. Consider separate queues for datasets with significantly different volumes or priority requirements.

**Considerations include:**

- Message identification and routing logic
- Different data schemas for different datasets
- Processing order and priority handling
- Error handling for different message types
- Queue capacity and throughput planning
- Dead letter queue configuration

**Solution Steps:**

1. Design message processing logic to handle multiple dataset types.
2. Implement proper error handling for each subscription type.
3. Configure appropriate SQS queue settings for expected volume.
4. Set up monitoring for the combined message flow.
5. Test thoroughly with multiple subscription types.
6. Document message routing and processing logic.

### Q: How do I properly archive or cancel subscriptions I no longer need?

**A:** Properly managing subscription lifecycle helps maintain clean account state and avoid unnecessary resource usage and associated costs.

Unused subscriptions should be properly archived rather than just ignored, as they continue to consume system resources and count against your subscription limits even if you're not processing the data.

**Best practices include:**

- Archive subscriptions through the API rather than just deleting the destination
- Clean up associated AWS resources (SQS queues, SNS subscriptions) after subscription archival
- Document subscription lifecycle for team knowledge
- Regular audit of active subscriptions
- Proper cleanup procedures for development/testing subscriptions

**Solution Steps:**

1. Use the `UPDATE /streams/subscriptions/{subscriptionId}` API call.
2. Clean up associated SQS queues and SNS subscriptions after subscription archival. 
3. Remove any IAM policies specific to the subscription.
4. Update monitoring and alerting configurations.
5. Document the archival in your change management system.
6. Verify the subscription no longer appears in your ACTIVE subscription list.
7. Monitor for any residual message delivery.

## Contact Support

If your question isn't answered in this FAQ or you need additional assistance, contact Amazon Ads API Support through the following channels:

- **Amazon Ads API Support:** Submit a support case through the [Amazon Ads API Support Portal](support/overview)
- **Community Forums:** Check the Amazon Ads API [Github community forums](https://github.com/amzn/ads-advanced-tools-docs/discussions) for similar issues

When contacting support, please provide:

- Your client ID and advertiser profile ID
- Specific error messages or subscription IDs
- Timestamps of when issues occurred
- Steps you've already taken to troubleshoot
- Sample data or logs if relevant to your issue
