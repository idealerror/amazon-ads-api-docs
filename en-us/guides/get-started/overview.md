---
title: Amazon Ads API Getting Started Overview
description: Overview of getting started with the Amazon Ads API
type: guide
interface: api
tags:
    - Onboarding
    - Authorization
keywords:
    - access tokens
    - refresh tokens
    - retrieve a profile
    - how to
    - Postman
---

# Getting started with the Amazon Ads API

<div class="breadcrumb-top" style="display: block; font-size: .9em; margin: 0px; margin: -10px 0 20px 0; padding: 10px; background: #f6f6f6; border-left: 8px solid #4f7cb1;">Getting started : <span style="white-space: nowrap;">[**Overview**](guides/get-started/overview) </span> | <span style="white-space: nowrap;">[1\. Create auth grant](guides/get-started/create-authorization-grant) | [2\. Generate access token](guides/get-started/retrieve-access-token) | [3\. Retrieve profiles](guides/get-started/retrieve-profiles)</span></div>

The Amazon Ads API enables you to manage the advertising resources associated with an Amazon advertiser using a REST API. Three credentials are required to authorize most requests to the API:

- the client ID of a Login with Amazon application
- an access token representing permission for the client application to act on behalf of a user account
- the profile ID for the user's advertising account in a specific marketplace

This guide explains how to retrieve these required credentials.

## Before you begin

To complete this guide, you will need:

- A Login with Amazon *client application* approved to use the Amazon Ads API. If you have not yet established an approved client, see [Onboarding](guides/onboarding/overview).
- Login credentials for an Amazon user account that manages Amazon Ads accounts. (If you do not have login credentials for an Amazon Ads account, [learn about test accounts](guides/account-management/test-accounts/overview).)

>[TOPIC:Authorization and identity in the Amazon Ads API]Requests to the Amazon Ads API are made by a **client application** on behalf of a **user account** with access to Amazon Ads accounts. A user account must grant explicit authorization to a client application using **Login with Amazon (LwA)**.<br> <br>For conceptual information about this relationship, you may wish to read the following before proceeding:<ul><li>[Amazon Ads API authorization overview](guides/account-management/authorization/overview)</li><li>[Login with Amazon (LwA) concepts](https://developer.amazon.com/docs/login-with-amazon/conceptual-overview.html)</li></ul>

## Steps in this process

Retrieving credentials to access data and services for an advertiser involves three steps:

1. [**Create an authorization grant for an advertising account.**](guides/get-started/create-authorization-grant) Enable an advertising user account to grant access to your LwA client.

2. [**Generate access and refresh tokens.**](guides/get-started/retrieve-access-token) Redeem the authorization grant for an access token, which is used to call the API, and a refresh token, which is used to generate new access tokens.

3. [**Retrieve a profile.**](guides/get-started/retrieve-profiles) Retrieve a profile ID representing the user account in a specific advertising marketplace.

Along with your LwA client ID, the access token and profile ID retrieved here can be used to authorize future requests to the API.

>[TIP:Getting started with our Postman collection]For some customers, the Amazon Ads API Postman collection may offer a simplified approach to completing these steps through the Postman user interface. [Learn more about getting started with the Amazon Ads API Postman collection.](guides/get-started/using-postman-collection)

## Get started

If you're ready to begin calling the Amazon Ads API, follow one of these two pathways:

- **"Getting Started" walkthrough**

    Our step-by-step walkthrough includes instructions for creating authorization grants, generating access tokens, and retrieving profiles. To begin, see [Create an authorization grant](guides/get-started/create-authorization-grant).

    >[NOTE]This process requires technical understanding of HTTP requests and the ability to send requests and read responses. Examples using `cURL` are provided for each step.

- **Amazon Ads API Postman collection**

    Our collection for Postman includes scripts to help manage auth, as well as readymade templates for some common requests to the API. To begin, see [Using the Amazon Ads API Postman collection](guides/get-started/using-postman-collection).
