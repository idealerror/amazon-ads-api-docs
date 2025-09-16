---
title: Tips and best practices for the ADSP bid adjustments API
description: A description of best practices for using the ADSP bid adjustments API
type: guide
interface: api
---

# Bid adjustments dimensions

## Dimensions with strict enumerations

The below dimensions **only allow** the following enumerations, in the exact data type specified. If anything else is provided (e.g., String instead of Integer, or an incorrect value), the API will respond with a 4xx client error.

|Dimension Name	|Valid Values	|Datatype	|Explanation	|
|---	|---	|---	|---	|
|adFormat	|["DISPLAY", "AUDIO", "VIDEO"]	|String	|There are currently 3 valid formats. Values must choose from the list.	|
|deviceType	|["Tablet", "PC", "TV", "Phone", "SetTopBox", "Unknown", "ConnectedDevice", "Robot"]	|String	|There are currently 8 valid formats. Values must choose from the list.	|
|slotPosition	|["ABOVE", "BELOW", "UNKNOWN"]	|String	|There are currently 3 valid formats. Values must choose from the list.	|
|supplySourceType	|["AMAZON", "AMAZON\_PUBLISHER\_DIRECT", "THIRD\_PARTY\_EXCHANGE"]	|String	|There are currently 3 valid formats. Values must choose from the list.	|

## String type dimensions matching

All the string type dimensions are **case insensitive** matching ***except*** `appId`.

## Supported dimensions list

|Category	|Dimension	|Data Type	| Definition and Values |
|---	|---	|---	|---    |
|Supply/ Site / Slot	|domain	|string	| Domain of a site. <br/><br/>*Example:* "foo.com". <br/><br/>Values are case insensitive. |
|Supply/ Site / Slot	|appId	|string	| Application identifiers unique to the app and independent of the exchange. On Android, this should be a bundle or package name. On iOS, it is typically a numeric ID. <br/><br/>*Example*: "com.foo.mygame". <br/><br/>Values are case sensitive.	 |
|Supply/ Site / Slot	|appName	|string	| Name of the app. <br/><br/>*Example*: "My Weather App". <br/><br/>Values are case insensitive.  |
|Supply/ Site / Slot	|dealId	|string	| Deal id to match against. <br/><br/>*Example*: "346361066859317070", "296560903047013107". <br/><br/>Values are case insensitive. Retrieve deals using [dsp deals api](https://advertising.amazon.com/API/docs/en-us/dsp-deals-3p)  |
|Supply/ Site / Slot	|slotSize	|string	| The slot's pixel size for the ad. This functions the same as the creative size with our slot-creative matching process. <br/><br/>*Example*: 300x50.   |
|Supply/ Site / Slot	|slotPosition	|string	| Whether the slot is above, or below the fold. <br/><br/>*Valid Values*: "ABOVE", "BELOW", "UNKNOWN". <br/><br/>Values are case insensitive.  |
|Supply/ Site / Slot	|supplySource	|string	| Supply source of the inventory. <br/><br/>*Example*: "Twitch", "Amazon Publisher Direct", "Verizon Media Exchange". <br/><br/>Values are case insensitive, derived from [inventory sources api](https://advertising.amazon.com/API/docs/en-us/dsp-discovery-inventory-source-exchanges).  |
|Supply/ Site / Slot	|supplySourceType	|string	| Filter category of the supply source. <br/><br/>*Valid Values*: "AMAZON", "AMAZON\_PUBLISHER\_DIRECT", "THIRD\_PARTY\_EXCHANGE". <br/><br/>Values are case insensitive, derived from [inventory sources api](https://advertising.amazon.com/API/docs/en-us/dsp-discovery-inventory-source-exchanges).  |
|Geography	|country	|string	| The ISO 3166-1 alpha-2 country code, based on the IP address of the user. <br/><br/>*Example:* "JP". <br/><br/>*Valid values:* download [ADSPBidAdjustmentsDimensionValueReference.xlsx](https://d3a0d0y2hgofx6.cloudfront.net/en-us/_images/assets/ADSPBidAdjustmentsDimensionValueReference.xlsx) for valid value inputs. Values must be chosen from the list. <br/><br/> Values are case insensitive.	                             |
|Geography	|region	|string	| The geographical state, based on the IP address of the user. <br/><br/>*Valid values:* download [ADSPBidAdjustmentsDimensionValueReference.xlsx](https://d3a0d0y2hgofx6.cloudfront.net/en-us/_images/assets/ADSPBidAdjustmentsDimensionValueReference.xlsx) for reference. Values may fall outside the list. You may use combination of region and country to avoid ambiguity. <br/><br/> Values are case insensitive.	               |
|Geography	|dma	|string	| Nielsen Designated Market Area code. <br/><br/>*Example*: “DMA501" would be used for Nielsen DMA corresponding to New York, NY. <br/><br/>*Valid values:* download [ADSPBidAdjustmentsDimensionValueReference.xlsx](https://d3a0d0y2hgofx6.cloudfront.net/en-us/_images/assets/ADSPBidAdjustmentsDimensionValueReference.xlsx) for valid value inputs. Values must be chosen from the list. <br/><br/>  Values are case insensitive.	 |
|Geography	|city	|string	| Full name of the city. <br/><br/>*Valid values:* download [ADSPBidAdjustmentsDimensionValueReference.xlsx](https://d3a0d0y2hgofx6.cloudfront.net/en-us/_images/assets/ADSPBidAdjustmentsDimensionValueReference.xlsx) for valid value inputs. Values must be chosen from the list.  You may use combination of city, region and country to avoid ambiguity. <br/><br/> Values are case insensitive.	                                  |
|Geography	|postalCode	|string	| The postal code with 2-letter country code prefix, based on the IP address of the user.  Use '-' as a separator between country code and postal code without space. Space, hyphen or any special characters within a valid postal code should be kept. <br/><br/>*Example: US-33166, JP-344-0063.* <br/><br/>Values are case insensitive.	                                                                                        |
|Environment	|deviceType	|string	| The device type of the user viewing the ad. <br/><br/>*Valid Values*:  "Phone", "Tablet", "PC", "TV", "ConnectedDevice", "SetTopBox", "Robot", "Unknown". <br/><br/> Values are case insensitive.	 |
|Environment	|deviceMake	|string	| The make of the device. <br/><br/>*Example*: "APPLE", "GOOGLE", "SAMSUNG", "AMAZON". <br/><br/> *Valid values:* download [ADSPBidAdjustmentsDimensionValueReference.xlsx](https://d3a0d0y2hgofx6.cloudfront.net/en-us/_images/assets/ADSPBidAdjustmentsDimensionValueReference.xlsx) for reference. Values may fall outside the list. <br/><br/> Values are case insensitive.	                                                        |
|Environment	|operatingSystem	|string	| The operating system of the user viewing the page. <br/><br/> *Example*: "MacOS", "Windows", "iOS", "Android", "Fire OS". <br/><br/>*Valid values:* download [ADSPBidAdjustmentsDimensionValueReference.xlsx](https://d3a0d0y2hgofx6.cloudfront.net/en-us/_images/assets/ADSPBidAdjustmentsDimensionValueReference.xlsx) for reference. Values may fall outside the list. <br/><br/> Values are case insensitive.	                    |
|Environment	|browser	|string	| The browser family of the user viewing the page. <br/><br/> *Example:* "Chrome", "Mozilla", "Safari", "Firefox". <br/><br/>*Valid values:* download [ADSPBidAdjustmentsDimensionValueReference.xlsx](https://d3a0d0y2hgofx6.cloudfront.net/en-us/_images/assets/ADSPBidAdjustmentsDimensionValueReference.xlsx) for reference. Values may fall outside the list. <br/><br/> Values are case insensitive.	                             |
|Ad Format	|adFormat	|string	| The media type of an ad.<br/><br/> *Valid values:* "DISPLAY", "AUDIO", "VIDEO". <br/><br/> Values are case insensitive. |
|Audience / Segment	|behavioralSegment	|string	| Unique identifier for Amazon Audiences. Selection of segments in Audience for the campaign is required in order to use them in bid adjustments. <br/><br/> *Example*: "407259404244917362". |

## Supported dimension value reference

[ADSPBidAdjustmentsDimensionValueReference.xlsx](https://d3a0d0y2hgofx6.cloudfront.net/en-us/_images/assets/ADSPBidAdjustmentsDimensionValueReference.xlsx)
