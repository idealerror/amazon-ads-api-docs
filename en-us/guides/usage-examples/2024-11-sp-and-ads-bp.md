---
title: Ads + Selling Partner APIs Onboarding and Best Practices
description: Ads + Selling Partner APIs Onboarding and Best Practices
type: guide
interface: api
keywords:
    - Selling Partner
    - Ads API
---

# Ads + Selling Partner APIs Onboarding and Best Practices

The goal of this SOP is to share the best practices and FAQs with dual developers who use both Ads and Selling Partner APIs.

# How to get started with SP-API for Developers

If you are just starting with Selling Partner APIs, please note that Amazon Ads API also uses Login with Amazon as well, however the applications and credentials cannot be used across the two API gateways; i.e. credentials for Ads APIs cannot be used for Selling Partner APIs and vice versa.

1. **Create developer account**: You must register as a Selling Partner API developer before you can [**register your Selling Partner API application**](https://developer-docs.amazon.com/sp-api/docs/registering-your-application?ld=ELXXSPAPI-pt-br-group.readme.io)**.** For a demo of developer registration and a walkthrough of Seller Central, refer to the [**Register as an SP-API Developer video**](https://www.youtube.com/watch?v=01kTSfgiKuY&ld=ELXXSPAPI-pt-br-group.readme.io) **** on [**SP-API Developer University.**](https://www.youtube.com/@amazon-sp-api?ld=ELXXSPAPI-pt-br-group.readme.io) **** We recommend a generic email distribution list to avoid the account tied to an individual.
2. **Read about SP-API roles and SP-API policies:** [A role is the mechanism used by Selling Partner APIs](https://developer-docs.amazon.com/sp-api/docs/roles-in-the-selling-partner-api?ld=ELXXSPAPI-pt-br-group.readme.io) to determine whether a developer or application has access to an operation or resource. As a developer, you must request and qualify for a particular role, or you will not be able to access the operations and resources grouped under that role.
    1. [AUP](https://sellercentral.amazon.com/mws/static/policy?documentType=AUP&locale=en_US&ld=ELXXSPAPI-pt-br-group.readme.io)
    2. [DPP](https://sellercentral.amazon.com/mws/static/policy?documentType=DPP&locale=en_US&ld=ELXXSPAPI-pt-br-group.readme.io)
3. **Registering as a developer:** You must register as a Selling Partner API developer before you can register your Selling Partner API application. The [Selling Partner API Developer Hub](https://developer.amazonservices.com/register) contain steps on how to do it.
4. **Create a** **Sandbox** **Selling Partner API Application:** [**The Selling Partner API provides two sandbox environments.**](https://developer-docs.amazon.com/sp-api/docs/the-selling-partner-api-sandbox?ld=ELXXSPAPI-pt-br-group.readme.io)A Static sandbox where mocked parameters allows you to test request and responses and dynamic sandbox restricted to [Vendor direct fulfillment](https://developer-docs.amazon.com/sp-api/docs/vendor-direct-fulfillment-dynamic-sandbox-guide), [Fulfillment Outbound API](https://developer-docs.amazon.com/sp-api/docs/fulfillment-outbound-dynamic-sandbox-guide) and [FBA Inventory](https://developer-docs.amazon.com/sp-api/docs/fba-inventory-api-v1-dynamic-sandbox-guide) returning realistic responses based on the request parameters. For the static sandbox you need to rely on the [static sandbox JSON objects](https://developer-docs.amazon.com/sp-api/docs/the-selling-partner-api-sandbox#static-sandbox-json-objects) to understand which parameters you use to call the API. 
5. **Registering your Selling Partner API Application in Developer Central**: To create your application you can check the [Registering your Selling Partner API Application](https://developer-docs.amazon.com/sp-api/docs/registering-your-application?ld=ELXXSPAPI-pt-br-group.readme.io). When creating your application you will be able to assign roles approved in your developer profile. So, if you didn't apply in your developer profile, it will not be available in the application register form.
6. **Authorizing Selling Partner API Applications:** The authorization model for the Selling Partner API is based on [Login with Amazon,](https://developer.amazon.com/docs/login-with-amazon/documentation-overview.html?ld=ELXXSPAPI-pt-br-group.readme.io) Amazon's implementation of OAuth 2.0.
    
    1. [Self-authorization](https://developer-docs.amazon.com/sp-api/docs/self-authorization): This model is typically used by sellers or vendors to access their own data. It allows developers to create applications that interact with their own Amazon seller accounts without needing approval from other sellers.
    2. [Selling Partner Appstore authorization workflow](https://developer-docs.amazon.com/sp-api/docs/selling-partner-appstore-authorization-workflow): This model is used for applications listed in the Selling Partner Appstore. It allows sellers to grant permissions to third-party applications directly through Amazon's interface. This workflow typically involves redirecting users to Amazon's authorization page.
    3. [Website authorization workflow](https://developer-docs.amazon.com/sp-api/docs/website-authorization-workflow): This model is used for applications not listed in the Selling Partner Appstore. It allows developers to create custom authorization flows on their own websites, giving sellers the ability to grant access to their data through a more customized process.
7. **Connect to Selling Partner API Application:** Before your application [can connect to the Selling Partner API](https://developer-docs.amazon.com/sp-api/docs/connecting-to-the-selling-partner-api?ld=ELXXSPAPI-pt-br-group.readme.io), you must register it and it must be [authorized by a selling partner as indicated in step 1.](https://developer-docs.amazon.com/sp-api/docs/authorization-with-the-restricted-data-token?ld=ELXXSPAPI-pt-br-group.readme.io)
8. **Test Selling Partner API Endpoints:**
    1. [How to test Selling Partner API endpoints](https://developer-docs.amazon.com/sp-api-blog/docs/how-to-test-selling-partner-api-endpoints?ld=ELXXSPAPI-pt-br-group.readme.io)
    2. [Using Postman for Selling Partner API models](https://developer-docs.amazon.com/sp-api/docs/using-postman-for-selling-partner-api-models?ld=ELXXSPAPI-pt-br-group.readme.io)

### **Why most Most 4xx errors occur?**

There are many possible reasons for a 4xx response. The first check is to ensure the following:

* Required roles are approved for your profile and application. Most 403 errors (Access Denied) occur due to profile or application not having the correct access assigned to them.
* A seller account is active. If not and a 400 error is being returned for that seller, then contact the seller and request that they navigate to Seller Central > Settings > Account Info > Charge Information. If the Charge Information page displays a banner requesting a credit card update, then ask the seller to update their credit card information. Allow 48 hours for the updates to process through all Amazon systems. If a seller account has not been used for 90 or more days, the account will be marked dormant, and the seller will be advised to update their credit card. Allow 48-hour waiting period to ensure a successful update. Afterwards, API calls can be made by that account.
* If you are receiving 429 errors, that means the request submissions exceed the steady-state request rate and burst limits, SP-API fails the limit-exceeding requests and returns `429 Too Many Requests` error responses to the client. If you receive these error responses, you can resubmit the failed requests in a way that is rate limiting while also complying with rate limits. This [document](https://developer-docs.amazon.com/sp-api/docs/strategies-to-optimize-rate-limits-for-your-application-workloads) describes how to use the defined rate limits for each SP-API operation in an optimized manner and provides strategies for managing API throttling in your applications workloads. 
* For other information regarding 400 errors check [here](https://developer-docs.amazon.com/sp-api/docs/resolving-400-errors).

### **FAQs**

* **I want to see a list of shopping products i.e. all ASINs listed on Amazon by their clients (both sellers and vendors) - clients can use**
    * [Catalog Items API](https://developer-docs.amazon.com/sp-api/docs/catalog-items-api-v2022-04-01-reference) - is used to retrieve information about items in the Amazon catalog such as Summarized item details, Attributes, Product identifiers, Sales rankings, Relationships, Images, Search by identifiers or keywords
    * [Listings Items API](https://developer-docs.amazon.com/sp-api/docs/listings-items-api-v2021-08-01-use-case-guide) - is used to create, edit, delete, and retrieve details about Amazon listings (SKUs) for a selling partner.
* **How do we tie the information from both APIs? For example we are looking for ad metrics such as impression, clicks, orders, CTR, conversion rate etc. to show in SP-API** **reports.**
    * Ad metrics are available via ads data and retail metrics are available via Selling partner APIs. To tie the two datasets, we suggest using ASIN as the joining key across ads and retail datasets. For non-ASIN-based (brand-based ad campaigns), we suggest using brand name (which is an attribute of ASIN) as the joining key. Some partners use a combination of brand name + product category (which are again attributes of an ASIN) for their ad campaigns.
* **How to look for remaining inventory data related to products?**
    *  Inventory reports can be used to obtain a file summarizing seller's product listings with the price and quantity for each SKU. Check them [here](https://developer-docs.amazon.com/sp-api/docs/report-type-values#inventory-reports)
* **Is it possible to use a single token that I received from Ads API OAuth process?** 
    *  Teams are looking into it but not possible yet.
* **How Reauthorization works?** 
    * Reauthorization means, sellers and vendors need to extend the usage of the app or reauthorize it. Developers should build one app for all their accounts and use oAuth workflow which they need to renew once a year for that one application which should have all developer's sellers and vendors they are servicing. 
* **What is LWA Rotation?** 
    * Is a security measure that requires developer to go into their seller central app and rotate the credentials every 180 days. A developer should build a public app, that all their accounts give authorization to. Then they only have to do [LWA rotation once every 6 months](https://developer-docs.amazon.com/sp-api/docs/rotating-your-apps-lwa-credentials#rotate-the-login-with-amazon-lwa-credential-client-secret-for-your-application-programmatically) (and not for each app every 6 months) for that single app.
* **How to [Determine App type](https://developer-docs.amazon.com/sp-api/docs/determine-app-type) - different application types are created with SP-API**
    * Public developers: Applications that are publicly available and are authorized by sellers or by vendors using OAuth. In accordance with the [Amazon Services API Developer Agreement](https://sellercentral.amazon.com/mws/static/agreement), public developers must list their app in the Amazon Selling Partner Appstore.
    * Private Seller: Applications for sellers that are available only to your organization, and are self-authorized.
    * Private Vendor - Vendor applications that are available only to your organization, and are self-authorized.
* **What happens if a developer adds a new role later on to their developer profile?**
    * You will have to reauthorize their app
    * Refresh token needs to be renewed to get new access
    * If a developer adds a new role into their developer profile, they need to add the new role in their app as well
* **How to calculate Organic sales data?**
    * This has to be calculated manually: Subtract Ads reporting data at the ASIN level (use `Advertised product` report for SP and SD and `Purchased Product` Report for SB) from the Selling Partner API reports (`GET_VENDOR_SALES_REPORT` for vendors and `GET_SALES_AND_TRAFFIC_REPORT` for sellers), to get Organic data. 

# How to get started with Ads API for 3p Developers


**ONBOARDING** - https://advertising.amazon.com/API/docs/en-us/guides/onboarding/create-lwa-app
 
Below is the summary of steps to get access to Ads APIs. Here is the [documentation](https://advertising.amazon.com/API/docs/en-us/guides/onboarding/overview) for details – 

**Onboarding** 

1. **Join Amazon Ads Partner Network** - Before an agency/partner get started with Amazon Ads APIs, they will have to set up their Amazon Business Account in Amazon Ads Partner Network -  
    1. Create Amazon Ads business account [HERE](https://advertising.amazon.com/partners/network) and follow the "Join Partner Network" registration flow.
    2. They will be prompted to create - 
        1. Login with Amazon account and
        2. Amazon Ads Partner Network account (needed for business registration)
    3. Once completed (~1 biz day), they will receive an  email notifying them of Amazon Ads Partner Network Registration.
    4. **Complete the Onboarding of Amazon Ads API** by creating Login With Amazon (LWA) client application to call Ads APIs. In this step you will register as a developer to start the LWA application. We recommend using an email address that is managed by multiple individuals in your organization. When approved, this email address will be associated to your Amazon Ads API permissions. SLA for this step is up to 48 hours.
        1. [Complete the LWA application](https://advertising.amazon.com/API/docs/en-us/guides/onboarding/create-lwa-app)
        2. [Apply for API access](https://advertising.amazon.com/API/docs/en-us/guides/onboarding/apply-for-access) as a Partner 
        3. [Assign Ads API access](https://advertising.amazon.com/API/docs/en-us/guides/onboarding/assign-api-access) to your LWA client application (once Step 1.b is approved)
        4. Scope required to make requests to the Ads APIs is `advertising::campaign_management`
2. **Authorize your LwA application** - Follow [Getting started steps](https://advertising.amazon.com/API/docs/en-us/guides/get-started/overview) for an Amazon user account to authorize your LwA application to access their advertising data and services through the API
3. **Acceptance of Ads Agreement** - This is the prerequisite for registering an ads account through the Amazon Ads API. The purpose of surfacing [terms token](https://advertising.amazon.com/API/docs/en-us/guides/account-management/accounts/create-accounts#step-1---terms-and-conditions-token) is to ensure that legal Terms & Conditions (“T&Cs”) are surfaced by the API integrator and have been accepted by the LWA’s user (customer who signed into the partner’s API service by using “login with amazon”) . It is a 2-step process -  
    1. The API integrator must call `/termsTokens`  API to generate a `termsToken` and `termsUrl` . The terms token url will contain a link to the [Ads Agreement](https://advertising.amazon.com/terms). Upon clicking that link, an Amazon Ads Agreement page along with the Accept & Continue button will be surfaced to the user. 
        
        `{ "termsToken": "4de3327e-a595-44cb-b689-f7f746805c02", "termsUrl": "https://advertising.amazon.com/terms/agreement/4de3327e-a595-44cb-b689-f7f746805c02" }
        `
    2. The LWA’d user must accept the terms and conditions. Upon the acceptance of the terms and conditions, a token is activated. Each token is associated with one account request (single or batch). The terms acceptance rule is the same for single account and bulk account registration i.e. the terms token is created once and redeemed. 
        
        **Note**: 
        1. We recommend showing terms and conditions as a one-time pop-up window presented to the advertiser after LwA (login with Amazon) is complete and it auto-closes when a user clicks Accept & Continue.
4. **Billing Setup for a newly created account** - [Setting up Billing APIs process](https://advertising.amazon.com/API/docs/en-us/guides/account-management/billing/getting-started) establishes WHO is going to pay for your advertising spend with Amazon Ads by setting a billing profile and WHAT payment method will be used by setting a payment profile.
5. **Register an Ads account -** Amazon Ads supports [registration via API](https://advertising.amazon.com/API/docs/en-us/guides/account-management/accounts/create-accounts#step-2---register-an-account)for businesses that do and do not sell on Amazon.
6. **User Permissions and Invitations** - A user account permission plays a crucial role in managing access and control over advertising accounts. Permissions define what actions a user can perform within their Amazon Ads account.  In the Amazon Ads API, permissions associated with the user account that initiates the authorization grant are crucial for determining whether subsequent API requests will be authorized.
    1. Setting Permissions - By default, the creator of a sponsored ads account has **Admin** permissions. Admin users can choose from two methods to enable access for other user accounts: manager accounts and access controls.
        1. Manager Account - allows advertisers and agencies to link and manage multiple advertising accounts. Note - In API, user performing the linking of accounts must have Admin permission. 
        2. Access Controls - To invite user account to the advertising account and determine that user account's permission level, call [User Invitation APIs](https://advertising.amazon.com/API/docs/en-us/user-invitations). These APIs involve a series of operations to handle invitations from creation, listing to expiration or revocation.
7. **Create Campaigns** of your choice - SB, SD, SP and ADSP

 
****Amazon Ads API Postman collection****
Our collection for Postman includes scripts to help manage auth, as well as readymade templates for some common requests to the API. To begin, see [Amazon Ads API Postman collection](https://advertising.amazon.com/API/docs/en-us/guides/get-started/using-postman-collection).

**Reports across Ads and Retail APIs**

To match reports across both APIs, store information at the ASIN (or individual product, SKU) level to allow for granular reporting for optimum insights. E.g. - 

1. Track inventory quantities as it negatively affects the customer experience and impacts the campaign's performance as well as the eligibility to be selected as the Featured Offer. Below is the impact for when inventory is out of stock - 
    1. **Sponsored Products**
        1. The visibility of the product will automatically pause. If it is the only product in the campaign, the entire campaign will pause.
    2. **Sponsored Brands**
        1. The visibility of the product will automatically pause. If you are no longer the Featured Offer, Sponsored Brands and DSP campaigns will continue to deliver impressions. If this happens you might spend money on Amazon Ads campaigns that are driving traffic and conversion to a product detail page where another selling partner has the Featured Offer.
    3. **Sponsored Display**
        1. Sponsored Display ads will automatically stop if your products are no longer available to purchase or are no longer the Featured Offer.

**Use cases for Ads and Selling Partner APIs** 

1. In order to manage and keep your inventory up to date you can use the [Building Listings Management Workflows Guide](https://developer-docs.amazon.com/sp-api/docs/building-listings-management-workflows-guide#updating-a-listings-item). Also you can track your inventory using [`ITEM_INVENTORY_EVENT_CHANGE`](https://developer-docs.amazon.com/sp-api/docs/notifications-api-v1-use-case-guide#item_inventory_event_change) notification or [`FBA_INVENTORY_AVAILABILITY_CHANGES`](https://developer-docs.amazon.com/sp-api/docs/notifications-api-v1-use-case-guide#fba_inventory_availability_changes) which is a notification  that includes a snapshot of the FBA inventory in all eligible marketplaces in a particular region. You can measure the performance of advertising-attributed inventory sold, and measure the performance to make decisions. 
    
2. Improve your product catalog content (Product title, Product description, Images etc.); by leveraging [Catalog Items API](https://developer-docs.amazon.com/sp-api/docs/catalog-items-api-v2022-04-01-use-case-guide) to view the catalog content for detail page optimization (Product title, Product description, Images etc.) and **Retail Readiness** (Retail readiness is a scoring system for products to classify products that are 'ready for ads' and helps ensure [Amazon.com](http://amazon.com/) customers have a positive shopping experience and the right information to make a purchase decision) which is important for an advertising campaign to drives traffic to a brand on Amazon.com. You should ensure that any product you are advertising is retail ready prior to setting up advertising campaigns.

 https://advertising.amazon.com/library/courses/optimize-for-retail-readiness 


1. Ads and Selling Partner APIs together also allow users to optimize campaign based on TACOS (total ACOS - which measures your advertising spend relative to your total revenue generated) ad spend/total sales (ad-attributed sales + organic sales). With Retail API you get the total sales for a product and with Ads API you get the advertising spend for a product. A lower TACoS value is desirable as it indicates that the advertising efforts are efficiently translating into sales revenue, making the advertising approach more effective and profitable for the business.


**Some of the common Retail metrics leveraged by Dual Developers are -**

* Inventory status
* Page views
* Sessions (Visits to your offer pages for a set time period)
* Total units
* Orders
* order item session % (dividing the number of orders of an item by the number of distinct sessions I.e. unique visits to your Amazon product detail pages for a certain time period), and 
* total sales

**Common asked retail metrics** - 

* Buy box
* Coupons/deals
* SNS



#### What are the basic reports used for vendors vs. sellers?

**Basic reports for Vendors**

1. [Brand Analytics reports](https://developer-docs.amazon.com/sp-api/docs/report-type-values#brand-analytics-reports) - provides 
    1. data on items that are most commonly purchased in combination with the items in the customer's basket (cart) at checkout.
    2. data on the top clicked ASINs by search keyword and department for a marketplace
    3. data on the quantity of repeated purchases of the selling partner's items
2. [Vendor Retail Analytics reports](https://developer-docs.amazon.com/sp-api/docs/report-type-values#vendor-retail-analytics-reports) - provides
    1. key retail sales metrics such as ordered/shipped revenue and units
    2.  key retail traffic metrics such as detail page views (i.e. glance views)
    3. inventory and operational health metrics such as sellable on hand units 
3. Vendor Direct Fulfillment APIs + Vendor Retail Procurement APIs - allows vendors in the direct fulfillment (DF) program to manage their direct fulfillment operations programmatically, helping to maintain performance at scale, increase operational efficiency, reducing errors, and improving performance. Through this API, vendors can: 
    1. Get a list of ordered based on a certain date range
    2. Get order details of a specific order by purchase order number
    3. Acknowledge (accept or reject) single or multiple orders  https://developer-docs.amazon.com/sp-api/docs

**Basic reports for Sellers**

1. Analytics APIs - such as Orders API, Reports API, Finances API can be used in combination or alone to derive insights on retail performance
2. Fulfillment APIs - such as FBA inventory, FBA small and light API and Shipment invoicing API can be leveraged to manage inventory and order information, confirm shipments, return package/pallet labels, or programmatically access Amazon’s shipping service. 
3. Data Kiosk is a reporting suite in the Amazon Selling Partner API (SP-API). It's a GraphQL-based tool for accessing bulk data. Developers who build reporting solutions on behalf of Selling Partners can use the [Data Kiosk API](https://developer-docs.amazon.com/sp-api/v0/docs/data-kiosk-api-v2023-11-15-reference) to easily discover and select data fields from a comprehensive Amazon retail dataset to generate business insights.
4. Buyer Selling Messaging APIs – Reach shoppers with APIs such as Solicitations API where you can use insights derived from other APIs (like the ones mentioned above), to send non-critical solicitations to shoppers; or use the Messaging API to send supported message types to customers. 


 
