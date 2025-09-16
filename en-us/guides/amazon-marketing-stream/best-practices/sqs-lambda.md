---
title: SQS best practices
description: Best practices for using Amazon SQS with Amazon Marketing Stream
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Sponsored Products
    - Sponsored Display
    - Onboarding
keywords:
    - stream 
    - SQS queues
    - SQS pricing
    - SQS cost
    - AWS SQS free tier
    - long poling
    - batching
    - AWS Lambda 
    - data transfer
---

# Optimizing Amazon Marketing Stream costs with AWS

A successful implementation of Amazon Marketing Stream relies on a strong understanding of AWS infrastructure, including [AWS SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) and [AWS Lambda](https://docs.aws.amazon.com/lambda/index.html). This document provides some suggestions and best practices to optimize your AWS costs as you scale Amazon Marketing Stream. 

>[NOTE] AWS provides many options for custom configurations based on your business needs. This guide contains some general recommendations, but we encourage you to work with someone familiar with AWS when you design and implement your Stream integration.

## AWS SQS

### Understanding SQS pricing

[AWS Simple Queue Service](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) (SQS) is the main AWS tool that you need to use for Amazon Marketing Stream. To optimize SQS costs, it’s important to understand how SQS pricing works. 

As part of the Stream [onboarding process](guides/amazon-marketing-stream/onboarding), you set up a queue and subscribe that queue to Stream datasets. Stream then sends records containing campaign data and events to your SQS queue.

There is a cost associated to each action you take on your queue. All of the actions described in the [SQS API operations reference](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_Operations.html) incur a cost. For example, once messages are in your queue, you need to send a request to SQS to receive messages (either through the SQS console or programmatically using the [ReceiveMessage](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ReceiveMessage.html) request in the SQS API). 

As part of the AWS SQS Free Tier, you can make one million free requests per month. Each additional set of a million requests incurs a charge based on your region (refer to the [SQS cost calculator](https://aws.amazon.com/sqs/pricing/) for the most up-to-date pricing). 

Data transferred between SQS and EC2 or Lambda in different regions also incurs a charge. Refer to the [cost calculator](https://aws.amazon.com/sqs/pricing/#:~:text=How%20are%20Amazon%20SQS%20charges%20metered%3F) for details. 

### Best practices

To optimize your SQS costs, we recommend implementing long polling and batching as [suggested by AWS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/reducing-costs.html). 

#### Long polling

To reduce the likelihood of being charged for empty messages, we recommend configuring long polling in your SQS queues. 

SQS offers two options for receiving messages from queue: short and long polling. By default, queues use short polling, which means `ReceiveMessage` requests result in an immediate response, even if no messages are found in the queue. With long polling, you only receive a message if there is one found during polling, or if the polling wait time expires. For example, if you send a `ReceiveMessage` request, but there are no Stream messages in your queue, you will get an empty response. The empty response will still count as an action that incurs a cost in SQS. You can validate the performance by reviewing the [NumberOfEmptyReceives](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-available-cloudwatch-metrics.html) CloudWatch metric.

More information:

* [Understanding short vs. long polling](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-short-and-long-polling.html)
* [Setting up long polling](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/examples-sqs-long-polling.html)

#### Batch of `ReceiveMessage` requests 

SQS pricing is based on the number of requests you send. If you reduce your number of requests, you can reduce costs. Batching requests is one way to reduce the number of requests made. 

Consider the following example: Amazon Ads sends five new messages to your queue. You could send five separate `ReceiveMessage` requests to SQS to retrieve the messages, and be charged five separate times. However, each of these messages is relatively small--in this example, let’s say 10 KB each for a total of 50 KB. AWS allows you to batch messages together, and as long as the batch does not exceed 65 KB, you will only be charged once. In this example, you could batch all five messages together for a total payload size of 50 KB, thus reducing the total number of messages your are charged for from five messages to one. 

AWS allows you to batch up to ten messages together. Messages sent from Stream should always be less than 6 KB; therefore, you should be able to set batching to the maximum allowable value of ten and be safely under the 64 KB payload threshold. 

More information:

* [Send and receive batches of messages with SQS using an AWS SDK](https://docs.aws.amazon.com/code-library/latest/ug/sqs_example_sqs_Scenario_SendReceiveBatch_section.html)
* [Python wrapper to send and receive messages](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/python/example_code/sqs/message_wrapper.py)
* [SQS batch operations reference](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-batch-api-actions.html)

>[TIP] If you are using AWS Lambda to consume messages from the queue, you should implement an [event source mapping](https://docs.aws.amazon.com/lambda/latest/dg/invocation-eventsourcemapping.html) that does long polling and batch by default.  Learn more about [Configuring a queue to use with Lambda](https://docs.aws.amazon.com/lambda/latest/dg/with-sqs.html#events-sqs-queueconfig).

#### Avoid data transfer across regions

If you need to transfer data, we recommend only sending data within the same region. Transferring data out to a different region results in [increased costs](https://aws.amazon.com/sqs/pricing/#:~:text=How%20are%20Amazon%20SQS%20charges%20metered%3F). 

## AWS Lambda

You can also use [AWS Lambda](https://docs.aws.amazon.com/lambda/index.html) to run recurring processes on messages received in SQS. 

If you are using AWS Lambda, you may want to consider the following blog posts from AWS on managing Lambda costs:

* [Optimizing your AWS Lambda costs: Part 1](https://aws.amazon.com/blogs/compute/optimizing-your-aws-lambda-costs-part-1/)
* [Optimizing your AWS Lambda costs: Part 2](https://aws.amazon.com/blogs/compute/optimizing-your-aws-lambda-costs-part-2/)
* [Estimating cost for Amazon SQS message processing using AWS Lambda](https://aws.amazon.com/blogs/compute/estimating-cost-for-amazon-sqs-message-processing-using-aws-lambda/)