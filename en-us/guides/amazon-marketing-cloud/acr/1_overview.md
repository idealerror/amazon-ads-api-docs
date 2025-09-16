---
title: Amazon Marketing Cloud - AWS Clean Rooms
description: AMC - AWS Clean Rooms
type: guide
interface: api
---
# Overview

Amazon Marketing Cloud (AMC) on AWS Clean Rooms enables advertisers (you) to seamlessly collaborate with Amazon Ads exclusive signals, generate enhanced insights, and discover new audiences, all without having to move any underlying data outside of your Amazon Web Services (AWS) environment. AMC on AWS Clean Rooms delivers a comprehensive view of customer interactions and business outcomes that can be activated and optimized in the Amazon DSP.

## Intended audience for this guide

This guide describes how to get started with the Amazon Marketing Cloud (AMC) powered by Amazon Web Services (AWS) Clean Rooms, the setup tasks to get started, and the API endpoints for using AMC-AWS Clean Rooms programmatically.

The tasks described in this guide are intended for the AMC account administrator and the administrator of your organization's AWS account.

Use this guide if you are an AMC administrator and meet the following prerequisites to be able to use the associated AWS service:

- Possess the AWS access credentials associated with your AMC account
- Have access to your organization's AWS S3 account
- Can sign in to the [AWS Management console](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/what-is.html)
- Can access AWS Entity Resolution to [create new IAM policies and roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html)
- Can [create schema mappings using AWS Entity Resolution](https://docs.aws.amazon.com/entityresolution/latest/userguide/create-schema-mapping.html)
- Can use [AWS Glue to catalog data](https://docs.aws.amazon.com/glue/latest/dg/catalog-and-crawler.html)
- Have [set up and can access AWS Clean Rooms to join a collaboration](https://docs.aws.amazon.com/clean-rooms/latest/userguide/setting-up.html)
- [Have onboarded the AMC APIs](guides/onboarding/overview)


## How does AMC leverage AWS Clean Rooms and AWS Entity Resolution?

AMC leverages the "collaboration" feature in [AWS Clean Rooms](https://docs.aws.amazon.com/clean-rooms/latest/userguide/what-is.html).

A [collaboration](https://docs.aws.amazon.com/clean-rooms/latest/userguide/create-collaboration.html) is a secure logical boundary in AWS Clean Rooms in which one member (such as an advertiser) can perform SQL queries on configured tables from other members (such as an advertiser partner or other publisher). AMC and advertiser collaborations in AWS Clean Rooms enable AMC customers to use AWS Clean Rooms to associate advertiser first-party (event and identity) data with AMC’s data, to collaborate with Amazon Ads unique signals including matching audience identifiers enabled by [AWS Entity Resolution](https://docs.aws.amazon.com/entityresolution/latest/userguide/what-is-service.html), and generate differentiated insights to activate more relevant campaigns with Amazon Ads, all without having to share or move your data from your AWS cloud environment.

## What are the benefits of AMC–AWS Clean Rooms?

The advantages of this integration are:

- **No identity or event signal mobility:** AMC–AWS Clean Rooms users are able to easily and securely collaborate with Amazon Ads signals and advertiser first-party signals stored in AWS, without the need to share or move proprietary hashed PII or event signals into AMC. Users can also send the aggregate results of the joint analysis to an Amazon Simple Storage Service (S3) destination of their choice.
- **Usability:** AMC–AWS Clean Rooms users can automatically apply signal refresh and signal processing policies when collaborating with Amazon Ads signals in AMC. This reduces the number of steps needed to collaborate with Amazon Ads.
- **Audience creation**: Leverage the power of AWS Clean Rooms to apply privacy-enhancing techniques when building your AMC Audiences or machine learned audiences.
- **Leverage AMC features and bespoke solutions**: Access to all features that AMC has to offer including AMC bespoke solutions are available to AMC-AWS Clean Rooms users through the AMC console.
