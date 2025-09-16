---
title: adm data upload
description: Information about data upload methods
type: guide
interface: bulk-operations
---
# Datasets and data upload methods

The Datasets page is the landing page of the Ads data manager, which provides a comprehensive overview of all your uploaded datasets. The page also provides a way to upload and manage your data from Amazon Ads.

The page lists detailed information regarding the total number of datasets, datasets in use, active destinations, and the number of accounts linked to the manager account.

For each dataset listed on the page, you can access comprehensive information like schema, source, creation date, and the status and utilization of your first-party data within the Amazon Ads platform.

Individual data records can only be uploaded within a dataset, and will be displayed under the "Recent Uploads" section of the dataset detail page.

Utilizing datasets, you can create advertiser audiences directly as well as analyze granular details such as record counts.

> [NOTE] Datasets comprise of all data brought to Ads data manager, including audiences, events, or datasets with custom schemas.

Ads data manager allows you to choose your options to upload your data depending on your convenience, technical capability, or integration preferences. There are three primary methods for uploading data:

- **[Ads data manager console upload](adm/4_adm-console-upload)**: The dataset page provides a simple browse-drag-drop method through the Ads data manager console, that allows for direct upload of .csv files.
- **[Source connector integrations](adm/5_adm-source-connector)**: For customers already using a Customer Data Platform (CDP), Ads data manager offers integration connectors with popular platforms. This method allows for automated, real-time data syncing between your CDP and Ads data manager.
- **[API upload](guides/ads-data-manager/adm-overview)**: You can choose to bring in your data programmatically using the Ads data manager APIs. The APIs allow you to send your data via JSON. This method is perfect on frequent data uploads and can be integrated into your existing data pipelines. Refer to the API guide to set up Ads data manager and work with it programmatically. 

>[WARNING] Advertisers and partners can also import existing uploaded datasets from the Advertiser Audience and Conversions API to Ads Data Manager. For details, see [Ads data manager console upload](adm/4_adm-console-upload##importing-datasets-from-existing-upload-sources).

In the next topic,  we'll dive deeper into each upload method, providing step-by-step instructions and best practices.
