---
title: Amazon Marketing Cloud SQL limitations
description: The limitations of AMC SQL
type: guide
interface: api
---

# Limitations and unsupported functions

## Unsupported functions

### LIMIT

AMC does not support LIMIT. However, you can use the RANK() function as a workaround to address your use case.

```
SELECT RANK() 
OVER(
    ORDER BY <column2>
    )
```

### RIGHT JOIN

AMC does not support RIGHT JOIN. However, you may use LEFT JOIN and reverse the side of the table.


### GETDATE()

AMC does not support the GETDATE() function, which returns the current database system date and time. However, you may use the following as a workaround:


```
CAST('today' AS DATE) AS Date_for_today
```


```
CAST('now' AS TIMESTAMP) AS DateTime_for_now 
```


### ORDER BY

AMC does not support ORDER BY clause in a query. However, you may use it within a [PARTITION BY](#guides/amazon-marketing-cloud/amc-sql/functions#partition-by) clause.



## Limitations

-   Aggregation queries must contain at least one aggregation method on a column. This is by design and is subject to change. Aggregation
    thresholds determine the data that is exported from AMC and the columns that can be included in the final select statement.

-   RANK does not operate correctly when referencing as an alias in a HAVING clause. Instead, the RANK expression can be wrapped in a
    sub-query and the alias can be referenced as normal in outer queries.

-   Raw data cannot be exported from AMC. For example, the following is not permitted: `SELECT * FROM DSP_IMPRESSIONS`.

-   `EXTEND_TIME_WINDOW` allows for the period of the input tables to be extended beyond the reporting time window. For instance, if the
    input to a FROM statement is TABLE (`EXTEND_TIME_WINDOW`(`dsp_impressions`, `P14D`, `P1D`)), then the impressions read in will have dates up to 14 days prior to the start of the reporting time window start and up to 1 day after the reporting time window end. All three inputs must be STRING type.