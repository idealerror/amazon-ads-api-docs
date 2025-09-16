---
title: Amazon Ads API Authorization Overview
description: Overview of authentication and authorization in the Amazon Ads API using Login with Amazon to generate authorization codes and access tokens.
type: guide
interface: api
tags:
    - Authorization
    - Account management
keywords:
    - access
    - headers
    - client application
    - OAuth
    - Amazon-Advertising-API-ClientId
    - Amazon-Advertising-API-Scope
    - unauthorized
---

# Amazon Ads API authorization overview

## Access to the Amazon Ads API

The Amazon Ads API provides programmatic access to Amazon Ads data and services. Like all Amazon users, Amazon Ads customers control access to their data. Because of this consideration, accessing the Amazon Ads API requires a careful authorization process.

### Access to data and services

As with the web-based user interface, Amazon Ads advertisers control access to their data and services via an Amazon login identity, which is an email address and password. A given advertiser must explicitly grant authorization for the advertiser's account to be accessed through the API.

### Access to the API

Amazon Ads enables *client applications* administered by **Login with Amazon** (LwA) to make calls to the API. To gain access, a client must be approved for access by Amazon Ads, and an approved *client identifier* is a required credential for any API call.

- For specific information on applying for API access, see our [Onboarding Walkthrough](guides/onboarding/overview).
- For general information on Login with Amazon, see the [Login with Amazon documentation](https://developer.amazon.com/docs/login-with-amazon/conceptual-overview.html)

### Advertiser and client

*Both* of the entities described above — advertiser and client application — must participate to make a successful call to the Amazon Ads API. The advertiser must first grant authorization to the client application, then the client application may pass the resulting credentials to the API to access *that specific* advertiser's data and services.

>[TOPIC:Accounts and identity]Each Amazon customer account uses an email address as its unique key. A single user account may have access to various Amazon services, such Amazon Ads or Amazon Developer. It is possible (though likely inadvisable) for the "advertiser" as described here to share login credentials with the Amazon Developer account that administers the client application.<br/> <br/>*Even in such a case*, the advertiser must explicitly authorize the client to access the API on the advertiser's behalf using the process described below.

## Authorization flow

To understand how an advertiser grants authorization to a client application, and how the client application then presents that authorization to the API, consider that authentication and authorization in the Amazon Ads API follow an [OAuth 2.0 flow](https://tools.ietf.org/html/rfc6749). 

### OAuth 2.0

OAuth 2.0 is designed to enable a third party to obtain limited access to the resources or services associated with a resource owner. 

For example:

![OAuth Conceptual Diagram](/_images/setting-up/oauth_flow.png)

In this diagram, the **client** represents a third party that acts on behalf of a **resource owner**. The client creates a request for the resource owner to consent to sharing resources. This request typically includes a scope that defines the resources and permissions to which the owner is granting access. 

The client retrieves an **authorization grant** representing the resource owner's consent. The client then submits the authorization grant to the **authorization server**, which responds with an **access token**. The access token is a credential that is typically valid for a short period of time.

The client passes the access token to the **resource server** and has permission to access the resource owner's data and services defined by the scope for the time the access token is valid.

### OAuth 2.0 in the Amazon Ads API

Authorization in the Amazon Ads API follows a similar conceptual flow. The diagram below illustrates this more specifically.

![OAuth 2.0 in the Amazon Ads API](/_images/setting-up/advertising_api_auth_flow.png)

In the Amazon Ads API, the **authorization server** is provided by [Login with Amazon](https://developer.amazon.com/docs/login-with-amazon/conceptual-overview.html). 

The **client** is a Login with Amazon application, which has an associated **client identifier** and **client secret**. 

The **resource owner** is an Amazon advertiser. By visiting a URL associated with the client identifier, the advertiser can use the Login with Amazon user interface to confirm their consent.

Login with Amazon creates a one-time **authorization code** associated with both the advertiser and the client. The client then passes this authorization code to Login with Amazon, along with the client identifier and client secret, to retrieve an **access token**. The client also retrieves a **refresh token** that can be used to generate new access tokens without repeating the steps above.

The client can pass the access token to the Amazon Ads API to retrieve a **profile identifier** associated with the advertiser that granted consent. Profiles in the Amazon Ads API represent an advertiser's account in a specific marketplace.

The client identifier, access token, and profile identifier are used to access the advertiser's data and services through the Amazon Ads API. 

## Required authorization headers

Based on the patterns described above, each request to the Amazon Ads API requires the following headers:
    
- **`Amazon-Advertising-API-ClientId`**: The client identifier of an LwA application authorized to access the API.
- **`Authorization`**: The string `Bearer ` *prepended* to an **access token** representing that application's permission to access data and services for a given Amazon user.

Nearly all resources available through the Amazon Ads API also require an additional header:

- **`Amazon-Advertising-API-Scope`**: A profile identifier representing an Amazon Ads account belonging to the Amazon user associated with the access token. 

If authorization headers are not included or are incorrect, a `401 Unauthorized` response is returned. If the `Amazon-Advertising-API-Scope` header is not included when required or is incorrect, a `401 Unauthorized` or `400 Bad Request` response is returned.

## Key authorization concepts

- Amazon Ads advertisers control access to their data and services.
- Amazon Ads API callers are client applications administered by Login with Amazon and approved by Amazon for API access.
- Advertisers can choose to delegate access to their data and services to approved client applications via an OAuth 2.0 flow.
- An API caller with an authorization grant from an advertiser can retrieve an access token for that advertiser via Login with Amazon.
- An API caller can access the Amazon Ads API by providing the following in the request headers:
    - The caller's client identifier.
    - An access token representing permission from a specific advertiser.
    - A profile identifier representing that advertiser's account in a specific marketplace.

## Learn more

Learn more about concepts from this document:

- [Authorization Grants](guides/account-management/authorization/authorization-grants)
- [Access Tokens](guides/account-management/authorization/access-tokens)
- [Profiles](guides/account-management/authorization/profiles)
- [Login with Amazon documentation](https://developer.amazon.com/docs/login-with-amazon/documentation-overview.html)

To follow a step-by-step guide to establishing authorization in the Amazon Ads API, see our [Onboarding Walkthrough](guides/onboarding/overview).
