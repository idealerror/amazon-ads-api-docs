---
title: Ads data manager
description: How to use Ads data manager
type: guide
interface: api
---
# Get started

Before you begin using Ads data manager APIs, ensure you have completed the following tasks:

- Amazon Ads API [Onboarding processes.](guides/onboarding/overview)
- [Getting started processes.](guides/get-started/overview)
  - Obtained your access token and profile ID
  - Onboarded Amazon Ads API using the Data Provider scope **`advertising::campaign_management`**
- [Created a manager account.](guides/account-management/authorization/manager-accounts#create-a-manager-account)
- [Associated advertisers with the manager account](guides/account-management/authorization/manager-accounts#associate-or-disassociate-linked-accounts)

    Setting up your Manager Account hierarchy should be done prior to using Ads data manager for data sharing. This setup can be done in through the Manager Account user interface and APIs today. Setting up invitations to accounts you do not own must be handled through the Accounts access & settings page available under the Administration page of the Ads console.

> [NOTE] If you are accessing Ads data manager using the APIs before accessing the Ads data manager console, complete the following tasks:

- [Accept Ads data manager terms and conditions](#accept-ads-data-manager-terms-of-use)
- [Create a data room](#create-a-data-room)

## Accept Ads data manager terms of use

If you are using Ads data manager using the APIs before accessing the Ads data manager console, you need to agree to the terms and conditions required for using Ads data manager. You can agree to the terms and conditions either through the Ads data manager console or by following specific steps in the API. However, if you have already completed these steps in the console, you don't need to repeat them when using the API.

- Use the **Terms API** to accept the terms necessary for the use of Ads data manager. To do so, perform the following steps:

  - Retrieve the terms first, using the `GET/adm/terms`.
  - Upon agreement, resubmit term acceptance using the `PATCH/adm/terms`request with the `agreementToken` provided as part of the GET request.

## Create a data room

A data room is a secure logical storage space within the Ads data manager that is provisioned for advertisers to upload their data.

If you are setting up Ads data manager using the APIs only, you are required to create a data room before uploading data. If you have already accessed Ads data manager through the UI, a data room is automatically created for you in your manager account.

Use `GET/managerAccounts` to get a list of manager accounts associated with your Login with Amazon (Amazon Developer account) client id.

To create a data room, use the following API:

1. Use the `POST/adm/datarooms` to create a data room, with the following mandatory header parameters.

| Header parameter                | Description                                                             |
| ------------------------------- | ----------------------------------------------------------------------- |
| Amazon-Advertising-API-ClientId | The identifier of a client associated with an Amazon Developer account. |
| Amazon-Ads-Manager-Account-ID   | Id of the Manager account.                                              |
| Authorization                   | Login with Amazon token in the form of Bearer {token}.                  |

> [NOTE] Only one data room can be created per manager account.

2. Once the data room is created, you can view it using `GET/adm/datarooms`.

You can now begin [creating datasets and uploading data.](guides/ads-data-manager/dataset-management)


### Get data room metadata

To retrieve high-level metadata about a specific data room under a specific manager account, use the `get/adm/datarooms/metadata` endpoint. 

The response returned will include information about the data sets linked to the data room, the number of active destinations, and linked accounts.
