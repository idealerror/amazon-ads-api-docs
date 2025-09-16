---
title: How to add Creatives to and remove Creatives from Line Items
description: How to add Creatives to and remove Creatives from Line Items
type: guide
interface: api
tags:
    - DSP
keywords:
    - creatives
---

# How to add Creatives to and remove Creatives from Line Items

>[WARNING]The DSP line items resource is now deprecated, with a planned shutoff date of March 31, 2025.<br><br>New integrators should view [our updated guide to ADSP creative management](guides/dsp/creative-management). For existing API callers, see our migration guides for [ADSP campaign management](reference/migration-guides/adsp-campaign-management) and [ADSP creative management](reference/migration-guides/dsp-creatives).

## Prerequisites

If you haven't already, you must [complete the prerequisites for this tutorial](guides/dsp/tutorials/prerequisites).

For this tutorial, you'll require:

* Your Amazon Ads API client identifier.
* Your Amazon Ads API scope.
* An authorization token.
* Your Advertiser identifier.

## Associating a Creative to a Line Item

Before you can associate a Creative to a Line Item, you require values for the following fields:

**Line Item identifier**

You'll use the `GET` operation of the [Line Item resource](dsp-campaigns/#tag/LineItem) to retrieve the Line Item identifer. 

**Creative identifier**

Before you can work with Creatives in the Amazon DSP API, you must have created the Creative using the Amazon DSP User Interface. Once the Creative has been created, you'll use the `GET` operation of the [Creative resource](dsp-campaigns/#tag/Creative) to retrieve the Creative identifier.

Once you have values for these fields, you'll use the `POST` operation of the [Line Item Creative Associations resource](dsp-campaigns/#tag/LineItemCreativeAssociation) to associate the Creative and the Line Item. The following fields are required:

* `advertiserId`: The advertiser identifier you retrieved in the prerequisites section.
* `operation`: For associating a Creative to a Line Item, this value is always `CREATE`.

Note that the `POST` operation supports associating a single Creative to a Line Item only. To attach multiple Creatives to a Line Item, you must first retrieve the list of Creatives using the `GET` operation of the [Creative resource](dsp-campaigns/#tag/Creative). Then, loop through the list of Creatives and use the `POST` operation of the [Line Item Creative Associations resource](dsp-campaigns/#tag/LineItemCreativeAssociation) as described above on each one.

## Updating the attributes for a Creative associated with a Line Item

Before you can update the attributes for a Creative that's associated with a Line Item, you require values for the following fields:

**LineItemCreativeAssociation**

You'll use the `GET` operation of the [Line Item Creative Association resource](dsp-campaigns/#tag/LineItemCreativeAssociation) to retrieve the list of `LineItemCreativeAssociation` objects using the identifier of the desired Line Item. Review each object in the list to find which ones have attributes you'd like to update.

Next, the following fields can be updated in each of the objects:

* `startDate` and `endDate`
* `weight`: You must specify a value for this field if you've set the `creativeRotationType` field to `WEIGHTED` in the Line Item to which you're associating this Creative.
* The object retrieved in the `GET` operation above.

Finally, you'll use the `PUT` operation of the [Line Item Creative Association resource](dsp-campaigns/#tag/LineItemCreativeAssociation), including the updated objects in the request body to update the attributes. 

Note that the `PUT` operation supports updating the attributes of a single Creative only. To update the attributes of multiple Creatives, you must first retrieve the list of Line Item Creative Associations using the `GET` operation of the [Line Item Creative Association resource](dsp-campaigns/#tag/LineItemCreativeAssociation). Then, loop through the list of Creatives and use the `PUT` operation of the [Line Item Creative Association resource](dsp-campaigns/#tag/LineItemCreativeAssociation) as described above.

## Removing a Creative association from a Line Item

Before you can remove the association between a Creative and a Line Item, you require values for the following fields:

**LineItemCreativeAssociation**

You'll use the `GET` operation of the [Line Item Creative Association resource](dsp-campaigns/#tag/LineItemCreativeAssociation) to retrieve the list of `LineItemCreativeAssociation` objects using the identifier of the desired Line Item. Review each object in the list to find the ones you'd like to remove the assocation.

Next, you'll use the `POST` operation of the [Line Item Creative Association resource](dsp-campaigns/#tag/LineItemCreativeAssociation) to remove the association between a Creative and a Line Item. In the request body of this operation, specify the following:

* `advertiserId`: The advertiser identifier you retrieved in the prerequisites section.
* `operation`: Set to `DELETE`.
* The object retrieved in the `GET` operation above.

Note that the `POST` operation supports removing the associaton between a single Creative and a Line Item only. To remove the association between multiple Creatives and Line Items, you must first retrieve the list of Line Item Creative Associations using the `GET` operation of the [Line Item Creative Association resource](dsp-campaigns/#tag/LineItemCreativeAssociation). Then, loop through the list of Creatives and use the `POST` operation of the [Line Item Creative Association resource](dsp-campaigns/#tag/LineItemCreativeAssociation) as described above on each one.