---
title: Billing invoicing report overview
description: A guide to billing invoicing report overview in the Amazon Ads API
type: guide
interface: api
keywords:
    - billing invoicing
    - global invoicing report
---

# Global invoicing report 

Global invoicing report allows sponsored ads and DSP advertisers to easily download a global CSV report containing a summary of all paid and unpaid worldwide invoices with billable amount, taxes, and purchase order among other details. Advertisers can focus on one country at a time or can look at the consolidated global spends to manage worldwide advertising payments.

## Before you begin

* Complete the Amazon Ads API [Onboarding](guides/onboarding/overview) and [Getting started](guides/get-started/overview) processes to obtain your access token and profile ID. You will need these to make all the calls referenced in the tutorial.
* Understand the concept of global account IDs and have a global account ID to setup billing. For more information, please refer to the [account creation](guides/account-management/accounts/create-accounts) and [retrieving accounts](guides/account-management/accounts/retrieve-accounts) guides.
* Have a global sponsored ads account. Refer to the [retrieving accounts](guides/account-management/accounts/retrieve-accounts#tag/Targeting-Expression-Localization/operation/getLocalizedTargetingExpression) guide to find your global account ID for your sponsored ads account. You can also create a new global account by following the [creating accounts](guides/account-management/accounts/create-accounts) steps. Downloading invoicing reports requires specific permission (mentioned with the respective API). You can check the permission associated to a user by following the [permissions](guides/account-management/permissions) guide.

## Download global invoicing report

### Step 1: Create invoicing report

User sends a request to [POST /billingStatements](billing#tag/Billing-Statements) endpoint.

```json
curl --location --request POST 'https://advertising-api.amazon.com/billingStatements' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxxx' \
--header 'Authorization: Bearer Atza|xxxxxx' \
--header 'Content-Type: application/json' \
--data-raw '{
    "locale": "en_US",
    "format": "CSV",
    "startDate": "20231124",
    "endDate": "20240222",
}'
```

More details on each attribute on the request can be found in the [billing statement API specification](billing#tag/Billing-Statements). 

A user can trigger global invoicing report flow for the countries in which they have `nemo_transaction_view/nemo_transaction_edit` within a global account. If the permissions are valid, the API returns a response with the following information:

```json
{
    "billingStatementId": "YW16bjEuYWRzLWFjY291bnQuZy42d3V6YmtocHc4a3RsMGlkOGtvOGlnMGU5O0dMT0JBTF9BQ0NPVU5UJkNTVjtlbl9VUztBVUQ7MjAyMzExMjQ7MjAyNDAyMjI7MDAwMDAwMDAx",
    "details": "SUCCESS"
}
```

### Step 2: Get invoicing report status and download link

User sends a request to [GET /billingStatements/{billingStatementRequestId}](billing#tag/Billing-Statements/operation/GetBillingStatement) endpoint with the `billingStatementRequestId` obtained from the response of Step 1.

**Sample request**

```json
curl --location --request GET 'https://advertising-api.amazon.com/billingStatements/YW16bjEuYWRzLWFjY291bnQuZy42d3V6YmtocHc4a3RsMGlkOGtvOGlnMGU5O0dMT0JBTF9BQ0NPVU5UJkNTVjtlbl9VUztBVUQ7MjAyMzExMjQ7MjAyNDAyMjI7MDAwMDAwMDAx' \
--header 'Amazon-Advertising-API-ClientId: amzn1.application-oa2-client.xxxxx' \
--header 'Authorization: Bearer Atza|xxxxx' \
```

User can download global invoicing report flow for the countries in which they have `nemo_transaction_view/nemo_transaction_edit` within a global account. If the permissions and the `billingStatementRequestId` is valid, the client implements a polling mechanism to continuously send GET requests to the API at regular intervals until the report is either a `SUCCESS` or `FAILED` status. During this polling process, the client expects to receive intermediate status updates, specifically `IN_PROGRESS`. A successful call to the API includes a response with the latest status of the invoicing report creation request along with the download link (if the report status is `SUCCESS`).

**Sample response**

```
{
    "details": "Billing Statement request generated successfully",
    "reportStatus": "SUCCESS",
    "s3DownloadLink": "https://advertisingbillingapiservice-billing-statement-test.s3.amazonaws.com/amzn1.ads-account.g.42kh2ozhrcsqjzm11ja3rx9h020231125-16836924218328554524.csv?X-Amz-Security-Token=FwoGZXIvYXdzEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaDLLbLfKr1qenL4G8pyK1ASsiRJVpLA3V482t9YmQFT5rhBhtIWgFSucy42Oeg9WRRXFQikTiD7mPxTmruToqKsoz0iuc6%2Fbi8h3OK9bVuh3KG7VvHCUQ1iz4kUtDVKk3BuhkrHHsnhcdhCMP%2Fkp64LQDS9XRnb7ygFvwgW9DfBBBXEC1Ydb5ZkeMUbUWbstmrziL5DIozi1%2FylEheivBoHYMsamq%2BrLB3HK86EjWlukXnqSx8eI1jIRvn51gOwm1VPFOXHsoi6jergYyLTAZI%2BNlO19BGfw2zMAwq18nuTNiCEGzUZKr4zUa8nwnJsLTuTDKV1Rzw0yZ7A%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240222T183555Z&X-Amz-SignedHeaders=host&X-Amz-Expires=10799&X-Amz-Credential=ASIATD7WSTURKWEN34UZ%2F20240222%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=8d72e6208c3d092b25bd1fc83b89e04ad66481e1163b5b9b9bcdd95aa1abee82"
}
```
