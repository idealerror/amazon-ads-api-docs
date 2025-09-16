---
title: Test accounts overview
description: Overview of test accounts in the Amazon Ads API, with information on feature support and limitations.
type: guide
interface: api
tags:
    - Testing
    - Account management
    - Onboarding
keywords:
    - advertising::test:create_account
    - sandbox
    - demo account
    - dummy account
---

# Test accounts

Advertising test accounts are a type of advertising account dedicated to testing. Test accounts do not serve ads or have active billing, allowing Amazon Ads customers to make API requests to test and learn about API functionality.

The [test account creation API](test-accounts/openapi) allows users to provision an advertising test account, which can then be used to test API functionality. 

An additional permission scope, `advertising::test:create_account`, is required to use the test account creation API. 

- For setup instructions, begin with [authorization for test accounts](guides/account-management/test-accounts/authorization-for-test-accounts).

## Features and functionality not supported

While we strive to have as much feature coverage as possible with test accounts, some features and aspects of functionality are currently not supported.

>[NOTE]**Lock Screen Ads**, **Posts**, and **Stores** are currently not supported. These features are inaccessible by test accounts.

### Other features not supported

- Performance data: Sponsored ads created in test accounts will not be served to shoppers. As a result, performance data related to campaigns created with test accounts will not be available. This includes, but is not limited to, data such as clicks, impressions, sales, ACOS, and spend.
- Insights API: Used for retrieving the top audiences that overlap with the provided audience.
- For author test accounts, a test KDP account will be created for the Amazon user account. Manual creation of books will be blocked from bookshelf within this account. During test account creation, test e-books are automatically created for use in test campaigns.
- Author test account creation is supported only for the following marketplaces: US, UK, ES, IT, FR and DE.

### Features partially supported

The following features and APIs will work for test accounts, but will not provide the full experience intended.

- Billing: Test accounts can be used to call the [invoices API](invoices/#/invoice). However, no invoice data will generally be available, as the test ads are not served to shoppers in production.

    >[NOTE]On certain occasions, test accounts may have billing invoices generated for them. In such instances the invoices will be written off every month by Amazon Ads for the test account. You are not required to make payments for invoices in test accounts.

- Brand metrics: Test accounts can be used to request and retrieve brand metrics reports. However, generated reports will not contain data due the lack of sales data in test accounts.
- Reports: Test accounts can be used to request and retrieve Sponsored Products and Sponsored Brands reports. However, generated reports will not contain data due to the lack of performance data for test campaigns.
- Creative Assets: Test accounts can be used to work with IMAGE assets only. VIDEO assets are currently not supported for test accounts.
