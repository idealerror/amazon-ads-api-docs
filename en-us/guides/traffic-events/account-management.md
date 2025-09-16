---
title: Account management for the traffic events API
description: Information about permissions and access in the traffic events API
type: guide
interface: api
tags:
    - Reporting
keywords:
    - Permissions
    - Account management
    - Authorization
---

# Advertiser Account Management for traffic events API Access

Access to the traffic events API is tied to the permissions that you have on the Ads accounts you are attempting to query. Instead, you need to own or be invited to an existing Sponsored Ads or DSP account with the permissions described below. You do not need any special permissions for traffic events API specifically.

To manage account permissions use the "Account access & settings" link in the navigation bar.

![Account access and setting panel](/_images/traffic-events/account-access.png)

### Sponsored Ads

Sponsored Ad users must have one of the following permissions to access the traffic events API: `advertiser_campaign_view, advertiser_campaign_edit, nemo_report_view, nemo_report_edit`

![Sponsored ads account management](/_images/traffic-events/sponsored-ads.png)

### DSP

DSP users must have the following permission to access the traffic events API: `view_performance_dashboard`

![DSP access management](/_images/traffic-events/dsp.png)
