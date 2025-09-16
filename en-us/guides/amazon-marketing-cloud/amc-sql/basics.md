---
title: Amazon Marketing Cloud SQL basics
description: The basics of AMC SQL
type: guide
interface: api 
---


# AMC SQL basics

## SELECT

The SELECT statement is used to select records from a table.

```
SELECT <column1>, <column2>, ...
FROM <table1>
```

* \<column1\>, \<column2\> refer to the field names in the table 
* \<table1\> refers to the table name that you want to select data from

> [NOTE] AMC does not support selecting all columns from a table (SELECT *) due to privacy reasons


## DISTINCT

The SELECT DISTINCT statement is used to return only distinct (unique)
values from a column in a database.

```
SELECT DISTINCT <column1>, <column2>, ...
FROM <table1>
```

* \<column1\>, \<column2\> refer to the field names in the table 
* \<table1\> refers to the table name that you want to select data from

## FROM

The FROM statement is used to specify the table to select the data from.


```
SELECT DISTINCT \<column1\>, \<column2\>, \...
FROM \<table1\>
```

* \<column1\>, \<column2\> refer to the field names in the table 
* \<table1\> refers to the table name that you want to select data from


## JOIN

AMC supports the JOIN clause which combines rows from two or more
tables, based on a matching column between them.

```
SELECT <table1>.<column1>, <table2>.<column2>, <table2>.<column1>,....
FROM table1
JOIN table2
ON <table1>.<matching_column1> = <table2>.<matching_column2>
```


* \<table1\> refers to the first table
* \<column1\> refers to a column in the first table
* \<table2\> refers to the second table
* \<column2\> refers to a column in the second table
* \<matching_column1\> refers to a column in table1 that should be used to match with a column in table2
* \<matching_column2\> refers to a column in table 2 that should be used to match with a column in table1


## Supported JOIN clauses

The types of JOIN clauses that AMC supports are:

-   **Inner Join / Join**

This JOIN returns only the matching rows from both the tables in the JOIN.

-   **Left Join/ Left Outer Join**

This JOIN returns all the rows of the table on the left side of the join and matching rows for the table on the right side of the join. For the rows that don't match, it will return Null as the matched values.

> [NOTE] AMC does not support **RIGHT JOIN**. As a workaround, please use LEFT JOIN and swap the tables.

-   **Full Outer Join**

This JOIN returns all the matched and unmatched rows from both the tables. For the rows that don't match, it will return Null as the
matched values.

-   **Cross Join**

A CROSS JOIN returns the cartesian product of the rows from the tables that are joined.

-   **Non-Equi Join**

A Non-Equi JOIN retrieves data from multiple tables by matching column values between them based on an inequality operator like \>, \<, != etc.

Syntax

```
SELECT <table1>.<column1>, <table2>.<column2>, <table2>.<column1>,....
FROM table1
JOIN table2
ON <table1>.<matching_column1> [> | <| >= | <=] <table2>.<matching_column2>
```

* \<table1\> refers to the first table
* \<column1\> refers to a column in the first table
* \<table2\> refers to the second table
* \<column2\> refers to a column in the second table
* \<matching_column1\> refers to a column in table1 that should be used to match with a column in table2
* \<matching_column2\> refers to a column in table 2 that should be used to match with a column in table1


##  JOIN vs UNION

JOIN clause combines rows from two or more tables, based on a matched column between them.

UNION clause combines the results obtained from two queries.

### Differences between JOIN and UNION clause

|      JOIN                                                                                                                                                                                                                                                                                                                                                                           |      UNION                                                                                                                                                                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     Combines rows from two or more tables, based on a   matched column between them                                                                                                                                                                                                                                                                                                 |     Combines results of two or more queries or SELECT   statements                                                                                                                                                                                                                                                        |
|     The two or more tables used in the JOIN may have   different columns, with one or more foreign keys for JOINing to each other   that are based on defined conditions.                                                                                                                                                                                                           |     The two or more SELECT statements used in the UNION   should have the same number and data type of columns                                                                                                                                                                                                            |
|     The result set may have more columns as the JOIN tries   to match and retrieve data from the JOINed table. If a row from the first   table has keys that match multiple rows in the second table, there will be a   row in the Example Query Results table for each combination. The result set   may or may not have the same number of rows as the tables used in the JOIN    |     A UNION will have all the rows from the first data   source + all the rows from the second. Hence, the result set will have more   rows, but the same set of columns, as the UNION consolidates the data in a   single set. UNION keeps unique records. UNION ALL keeps all records,   including duplicate records    |                  

## WHERE

The WHERE clause is used to filter the query results based on a
condition.


```
SELECT <column1>, <column2>, ...
FROM <table1>
WHERE <condition>
```

* \<column1>\, \<column2>\ refer to the field names in the table that you want to select data from
* \<table1\> refers to the table name
* \<condition\> refers to the condition that should be true to filter down the query results

## GROUP BY

The GROUP BY statement is used with aggregate functions MIN(), MAX(),
SUM(), AVG() etc. to group the data by one or more columns.


```
SELECT <column1>, <FUNC>( <column2>), ...
FROM <table1>
WHERE <condition1>
GROUP BY <column1>

```

* \<column1\>, \<column2\> refer to the field names in the table that you
want to select data from

* \<table1\> refers to the table name

* \<FUNC\> refers to an aggregated function like MIN(), MAX() etc.

* \<condition1\> refers to the condition that should be true to filter
down the query results

## HAVING

The HAVING clause is similar to the WHERE clause as it helps filter the
data based on specific conditions. HAVING clause can be used with
aggregate functions, but a WHERE clause cannot be used.


```
SELECT <column1>, <FUNC>( <column2>), ...
FROM <table1>
WHERE <condition1>
GROUP BY <column1>
HAVING <condition2>
```


* \<column1\>, \<column2\> refer to the field names in the table that you
want to select data from

* \<table1\> refers to the table name

* \<FUNC\> refers to an aggregated function like MIN(), MAX() etc.

* \<condition1\> and \<condition2\> refer to the condition that should be
true to filter down the query results

## ALIASES

Aliases are used to give a table, or a column, a temporary name.


```
SELECT <column1> as <alias_name1>, <column2>, ...
FROM <table1> <alias_name2>
```


* \<column1\>, \<column2\> refer to the field names in the table that you want to select data from

* \<table1\> refers to the table name

## VALUES

The VALUES command specifies the values to include in a common table
expression. Use [Common Table Expressions](#common-table-expression) to
define values to filter the queries.


```
WITH <table1> ( <column1>) AS (
  VALUES
    (<value1>),
    (<value2>),
    (<value3>)
)
```


* \<column1\> refers to the field names in the table

* \<table1\> refers to the temporary table name

* \<value1\>, \<value2\> etc. refers to the values that will be included
in the table

## REGEX

Regex or Regular Expressions, is a sequence of characters that are used
to construct search strings or patterns that can be used for filtering
data. A Regex can be a combination of integers, special characters etc.

|     Pattern    |     Usage                  |     Example                   |     Explanation                                                           |
|----------------|----------------------------|-------------------------------|---------------------------------------------------------------------------|
|     ^          |     Begins with            |     ^Toast                    |     Begins with toast. Example: toaster                                   |
|     (?!)       |     Case Insensitive       |     (?!)consideration         |     This will match with Consideration, CONSIDERATION,   conSIDerATION    |
|     ‘          |     Escape single quote    |     men''s electric shaver    |     This will match men’s electric shaver                                 |
|     "          |     Escape double quote    |     red 48'''' rod            |     This will match red 48" rod                                           |                           

## COMMON TABLE EXPRESSION

A Common Table Expression or a CTE is a temporary result set derived
using a query, which can then be referenced in a SELECT statement as a
table. Note that there is a comma separating multiple CTE definitions
and no comma prior to the terminal query.


```
WITH <cte_name1> 
AS (<query>),

<cte_name2> 
AS (<query>)
SELECT <column1>,<column2> 
FROM <cte_name>
```

* \<cte_name\> refers to the name of the temporary table

* \<query\> refers to the query used to derive the result set

