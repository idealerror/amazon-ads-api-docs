---
title: The adassociation entity in the Amazon Ads API
description: Conceptual overview of the adassociation entity in the Amazon Ads API
type: guide
interface: api
tags:
    - Campaign management
keywords:
    - unified
    - entity guides
---

# AdAssociations

Ad associations apply to Amazon DSP ad groups only and correspond to [ad creative associations](guides/dsp/creative-associations) in earlier versions of the campaign managment API.

>View the [technical specification for the AdAssociations resource](amazon-ads/1-0/openapi#tag/AdAssociations).

## AdAssociation parameters by ad product

**Key:** ✓ required | ○ optional | □ read-only | ◐ nested object

| Field | Common model |Amazon DSP |
| --- | --- | --- | 
| adAssociationId                              	| □	| □	| 
| adGroupId                                    	| ✓	| ✓	| 
| adId                                         	| ✓	| ✓	| 
| endDateTime                                  	| ○	| ○	| 
| startDateTime                                	| ○	| ○	| 
| state                                        	| ✓	| ✓	| 
| weight                                       	| ○	| ○	| 


