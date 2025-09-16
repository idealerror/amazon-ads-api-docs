---
title: Bulksheets legacy information
description: Information about the legacy version of bulksheets.
type: guide
interface: bulk-operations
---

# Dynamic Bidding Strategy Options

| Bid Strategy                  | Description                                                                                                                                                                                                    | Dynamic Increase | Dynamic Decrease |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|------------------|
| Fixed Bids                    | Uses your exact bid and any placement adjustments you set, and is not subject to dynamic bidding.                                                                                                              | 0                | 0                |
| Dynamic bidding (up and down) | Increases or decreases your bids in real time by a maximum of 100%. With this setting bids increase when your ad is more likely to convert to a sale, and bids decrease when less likely to convert to a sale. | 100              | -100             |
| Dynamic bidding (down only)   | Lowers your bids in real time when your ad may be less likely to convert to a sale. Campaigns created before the release of the bidding controls feature used this setting by default.                         | 0                | -100             |