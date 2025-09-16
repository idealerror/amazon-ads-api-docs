---
title: Amazon Marketing Stream reference implementation using AWS CDK and Python
description: This documents provides details on a reference implementation for Stream created using the AWS CDK and Python.
type: guide
interface: amazon-marketing-stream
tags:
    - Reporting
    - Onboarding
keywords:
    - AWS CDK 
    - Python
    - example application
    - GitHub
    - installation
---

# Amazon Marketing Stream reference implementation

The reference implementation is a Python-based application built using the [AWS CDK](https://docs.aws.amazon.com/cdk/api/v2/). It is available for all developers in [GitHub](https://github.com/amzn/amazon-marketing-stream-examples).

>[WARNING] This is a reference implementation, and not the only definitive way to consume Amazon Marketing Stream data. Note that this implementation is subject to change and future releases may not be backwards compatible. 

## What is the purpose of the reference implementation?

This reference implementation programmatically performs steps 1, 2, and 4 in the in the [Onboarding guide](guides/amazon-marketing-stream/onboarding). The package also supports the ability to subscribe to Stream datasets (step 3), using a CLI. 

If you are not familiar with a CLI or Python, you can also use the [AWS SQS UI](guides/amazon-marketing-stream/onboarding) or [CloudFormation template](guides/amazon-marketing-stream/cloud-formation) to complete the onboarding process. 

## Who should use this reference implementation?

Developers familiar with AWS can use the reference implementation as a basis for their Stream integration. 

## Does use of the reference application incur any additional cost?

The reference application provisions a number of AWS resources for use with Stream. You are responsible for all AWS costs associated with a Stream integration. For more information on optimizing based on cost, see [SQS best practices](amazon-marketing-stream/aws-best-practices). 

## Architecture 

This application is developed using Python and AWS Cloud Development Kit (CDK).

![Stream reference implementation architecture diagram](/_images/amazon-marketing-stream/stream-reference-implementation-architecture.png)

The application provisions the following AWS infrastructure components for each dataset and region combination:

- An [SQS queue](https://docs.aws.amazon.com/sqs/index.html) (StreamIngressQueue) that receives initial messages from Stream. 
- A [lambda](https://docs.aws.amazon.com/lambda/index.html) (StreamFanoutLambda) that identifies whether a message contains subscription details or data.
- A second SQS queue (SubscriptionConfirmationQueue) that forwards subscription confirmation messages to a second lambda (SubscriptionConfirmationLambda) that confirms the subscription.
- An [SNS topic](https://docs.aws.amazon.com/sns/index.html) (StreamFanoutDataTopic that) forwards data through a [KinesisDateFirehouse](https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html) (StreamStorageFirehose) to an [S3 bucket](https://docs.aws.amazon.com/s3/index.html) (StreamStorageBucket) where the data is stored. 

Note: The provisioning of each SQS queue also includes an associated [dead-letter queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html).

## How do I get started with the reference application?

For prerequisites and installation details, see the [README](https://github.com/amzn/amazon-marketing-stream-examples/blob/main/README.md) in GitHub.

Follow along with a video demo of the reference application.

<iframe sandbox="allow-scripts allow-same-origin" width="560" height="315" src="https://www.youtube.com/embed/_4o6QuTZxxs" title="Demo: Amazon Marketing Stream reference application" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

0:00-5:16 - Intro 

5:16-8:15 - Demo: overview 

8:15-9:00 - Demo: venv & pip install  

9:00-10:50 - Demo: cdk synth & infrastructure deep dive 

10:50-11:56 - Demo: cdk out

11:56-13:18 - Demo: cdk bootstrap 

13:18-14:31 - Demo: cdk commands and credentials

14:31-18:45 - Demo: cdk deploy na-sp-traffic 

18:45-20:41 - Demo: subscribe 

20:41-22:31 - Outro and additional resources