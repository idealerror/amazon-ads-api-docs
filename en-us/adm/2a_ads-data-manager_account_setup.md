---
title: adm console account setup
description: Ads data manager account setup.
type: guide
interface: bulk-operations
---
# Manage your Ads data manager account

The Ads data manager console is designed to simplify the process of uploading and sharing your first-party (1P) data. To begin using the Ads data manager, simply set up your advertisers with the manager account, bring in your 1P data, and share it with the linked accounts.

## Set up your manager account for data manager

The steps to link advertiser accounts to a manager account are outlined below:

- It is assumed that you have [created a manager account](https://advertising.amazon.com/help/G69CDSR9MNSWJH95).

> [NOTE] In the current release, you are required to have at least one Amazon DSP Advertiser Account to use Ads Data Manager. We are working to expand capabilities to Sponsored Ads products in future releases.

- Now, link advertiser accounts to the manager account.

  - Invite **owned-accounts**. to the manager account. These accounts could be owned by you; and you have complete access to the accounts. 
      Follow the instructions listed under [Link accounts to your manager account](https://advertising.amazon.com/help/GU3YDB26FR7XT3C8).
  - Or, invite **non-owned accounts** to your manager account. These accounts could be managed by an agency or third-party and you do not have access to the account. This is a two-step process.

    - First, [request access to an advertising account](https://advertising.amazon.com/help/GU3YDB26FR7XT3C8) and then send an approval link to an admin user for approval. The admin of the account you're inviting must accept and redeem your invite. Alternatively, you could also get an approval link to select the account permission to grant to the account.
    - Set the right permissions for the linked accounts:

      - **ADSP Reports**: It will allow you to only view the Amazon DSP reports. Selecting this permission will not allow you to share data with this account through Ads data manager.
      - **Data Sharing**: To allow the sharing of uploaded dataset between accounts.

> [NOTE] Data sharing is automatically applied to advertiser accounts with an “Admin” link permission type. “Admin” links are automatically created when an Advertiser Account is created from within a parent Amazon DSP Entity.

## Set up account hierarchy

Ads data manager operates exclusively at the **manager account** level, which means proper account structure is essential for functionality. To upload and manage your datasets, your manager account must either serve as a parent account to Amazon DSP Advertiser Accounts or maintain Data sharing permissions with them. This hierarchical structure ensures proper data flow and management capabilities within the system.
ADM supports two distinct types of account links to help you manage your advertising data effectively. 

- Admin link
- Data sharing link

### Admin link

The Admin link is designed for Advertiser Accounts that are created directly within your Manager Account. These links provide comprehensive administrative control over the connected accounts and their associated data.

### Data sharing link

Data sharing links enable you to connect with third-party accounts while maintaining control over your resources. This arrangement allows external users to access and utilize shared data for specific advertising purposes, while ensuring that ultimate control of the data remains within your manager account.

## Link accounts

You can connect accounts to ADM through the account linking process following the steps listed below:

1. Navigate to Administration > Account access & settings > Accounts.
2. Click Add account > Link existing account.
3. Choose an account you have access to.
4. Send a request to link the account to your manager account. 

A request will be sent to the invited account's administrator to review and approve the linking.
