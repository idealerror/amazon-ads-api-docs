---
title: NCS table
description: The NCS CPG insights table
type: guide
interface: api
---

# NC Solutions (NCS) CPG Insights Stream

**Analytics table:** 

* `ncs_cpg_insights_stream`

**Audience table:** 

* There is no associated audience table.

NC Solutions (NCS) CPG Insights Stream provides offline modeled transactions at the household level, representing the total US population of 127MM+ households. This data is derived by NCS by applying advanced machine learning models that utilize seed dataset comprising over 2.7 trillion observed transaction records representing 98 million+ US households. These records are collected from over 20+ major national and regional CPG retailers across 56K+ retail locations, and are combined with robust household-level information from Nielsen to identify shopping patterns and similarities across households.

With NCS CPG Insights Stream, you can bring granular offline retail sales data into AMC. It allows you to holistically measure the impact of your online media, not just on driving online purchases but also on driving offline purchases. You can then use these insights to inform campaign and channel optimization strategies.

| Column Name | Data Type | Column Type | Column Description | Aggregation thresholds |
|------------|------------|--------------|-------------------|---------------------|
| brand | STRING | DIMENSION | NCS syndicated brand name | LOW |
| category | STRING | DIMENSION | NCS syndicated product category name | LOW |
| department | STRING | DIMENSION | NCS syndicated department name | LOW |
| household\_purchase\_id | STRING | DIMENSION | Unique ID for the household purchase event, can be used to count distinct household purchases. | VERY\_HIGH |
| ncs\_household\_id | STRING | DIMENSION | NCS household ID | VERY\_HIGH |
| product\_category | STRING | DIMENSION | NCS syndicated subscription resource object; associated with the department, super\_category and category attributes | LOW |
| purchase\_dollar\_amount | DecimalDataType(precision=38, scale=2) | METRIC | US Dollar amount of products purchased | LOW |
| purchase\_quantity | INTEGER | METRIC | Quantity of products purchased | LOW |
| sub\_brand | STRING | DIMENSION | NCS syndicated sub brand name | LOW |
| super\_category | STRING | DIMENSION | NCS syndicated super product category name | LOW |
| user\_id | STRING | DIMENSION | User ID | VERY\_HIGH |
| user\_id\_type | STRING | DIMENSION | Type of user ID | LOW |
| week\_end\_dt | DATE | DIMENSION | Week end date. Datestamp of last day included in the weekly transaction file. Datestamps will always be the Saturday of each week and represents purchases over the last 7 full days; Sunday-Saturday. | LOW |