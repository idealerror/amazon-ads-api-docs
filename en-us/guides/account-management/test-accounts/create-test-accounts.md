---
title: Creating test accounts
description: Instructions for creating test accounts in the Amazon Ads API, including example request JSON and information about the types of test accounts that can be created.
type: guide
interface: api
tags:
    - Testing
    - Account management
keywords:
    - vendor code
    - author account
    - accountMetaData
    - test ebook asins
    - Test_title
    - Test_author
---

# Creating test accounts

The `/testAccounts` endpoint in the Amazon Ads API enables users to:

- Request creation of a test advertising account via a `POST` request
- Check account creation status via a `GET` request. 

[Technical specifications for the test accounts API.](test-accounts/openapi)

## Authorization headers

Requests to the `/testAccounts` endpoint require the following headers:

- **Amazon-Advertising-API-ClientId**: Your Login with Amazon client ID.
- **Authorization**: An access token representing permission for the client application to carry out test account creation on behalf of an Amazon user account, as described in [Authorization for test accounts](guides/account-management/test-accounts/authorization-for-test-accounts). The string `Bearer ` is prepended.

For more information about authorization headers in the Amazon Ads API, see [Required authorization headers](guides/account-management/authorization/overview#required-authorization-headers).

## Creating a test account

A new test account can be created by a `POST` request at `/testAccounts`.

The JSON request body has three parameters:

- **countryCode**: The two-character country code for the marketplace in which the account will be created, one of: US, CA, MX, BR, UK, DE, FR, ES, IT, AE, SA, NL, SE, TR, PL, BE, EG, JP, AU, SG
- **accountType**: Either `VENDOR` or `AUTHOR`.
- **accountMetaData** *(optional)*: An object with a single key, `vendorCode`, whose value is a vendor code to be applied to the test account.

### Supported marketplaces by account type

| Account type | Supported marketplaces |
| --- | --- |
| `VENDOR` | US, CA, BR, MX, JP, AU, SG, UK, IT, ES, FR, DE, AE, SA, NL, TR, PL, SE, EG, BE |
| `AUTHOR` | US, UK, IT, ES, FR, DE |

>[TIP:Vendor codes]A vendor code is a five-character code for a registered vendor. Test accounts created with a vendor code can use the linked vendor's product ASINs and metadata for test campaign creation.<br /><br /><details><summary>Steps for locating your vendor code in the Amazon Ads console *(click to expand)*</summary><ol><li>Log in to the Amazon Ads console using a registered vendor account.</li><li>Click the "gear" icon to access the **Administration** menu.</li><li>Select **Access and settings**.</li><li>From the menu at left, select **Selling account**.</li><li>Under **Account Associations** at right, click to expand the account listed under **Vendor Central account**.</li><li>The five-character codes that appear are vendor codes. Select one to include in your request to the API.</li></ol></details>

### Example JSON request body
    
```json
{
    "countryCode": "US",
    "accountMetaData": {
        "vendorCode": "ABCDE"
    },
    "accountType": "VENDOR"
}
```

### Example request using cURL

```shell
curl -X POST 'https://advertising-api.amazon.com/testAccounts' \
-H 'amazon-advertising-api-clientid: YOUR_LWA_CLIENT_ID' \
-H 'authorization: Bearer YOUR_ACCESS_TOKEN' \
-H 'content-type: application/json' \
-d '{"countryCode": "US","accountMetaData":{"vendorCode":"YOUR_VENDOR_CODE"},"accountType": "VENDOR"}'
```

>[NOTE]If you do not have access to a registered vendor code, omit the `accountMetaData` key.

>[NOTE]Only *one* author advertising test account can be created for an Amazon user account in each marketplace.

### Response

A successful response returns a `200` code and a JSON body in the following format:

```json
{ "requestId": "A7BCDGCEVXQ1CJJ4301V" }
```

The `requestId` can be used to retrieve status information as described in the next section.

See the [technical specifications](test-accounts/openapi#tag/Create-test-account) for information on error responses.

## Checking the status of an account creation request

Creation status and details about created accounts can be retrieved by a `GET` request at `/testAccounts`. 

A `requestId` retrieved from a successful `POST` request can be passed as a query parameter to retrieve details about that request. A request without the `requestId` parameter will return a list of all test accounts created with the current client ID and authorizing user account.

### Example request using cURL

```shell
curl -X GET \
https://advertising-api.amazon.com/testAccounts?requestId=A7BCDGCEVXQ1CJJ4301V \
-H 'amazon-advertising-api-clientid: YOUR_LWA_CLIENT_ID' \
-H 'authorization: Bearer YOUR_ACCESS_TOKEN'
```

>[NOTE]"A7BCDGCEVXQ1CJJ4301V" is a sample `requestId`. Use the `requestId` retrieved in the response to an account creation request to check the status of that request.

### Response

For a request using the `requestID` query parameter, a successful response returns a `200` code and JSON body in the following format:

```json
[
    {
        "countryCode": "US",
        "asins": [],
        "accountType": "VENDOR",
        "id": "ENTITY012345678910",
        "status": "COMPLETED"
    }
]
```

>[TIP:Author accounts]The example above shows the response after creation of a vendor test account. For author accounts, 3 test ebook ASINs linked to the account are also created. These ASINs will be listed under the `asins` key in the response from this request and are available for use in test campaign creation after 48 hours.

See the [technical specifications](test-accounts/openapi#tag/Get-test-account-information) for full information on the response schema and error responses.

Once the status is COMPLETED, your advertising test account is ready to be used. 

To get started, see [Using test accounts](guides/account-management/test-accounts/use-test-accounts).
