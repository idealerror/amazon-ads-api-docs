---
title: Amazon Marketing Cloud SQL functions
description: The functions that can be used with AMC SQL
type: guide
interface: api
---
# AMC SQL functions

## String functions


### CHARINDEX	

Searches for a substring within a string and returns its starting position, with optional start position parameter for refined searches.	

CHARINDEX(substring, string [, start_position])

### CONCAT

The CONCAT function concatenates multiple strings and returns the result as a single string. All inputs will be cast to string data type for joining.

```
CONCAT(expression \[, expression\]\*)
```


### FORMAT	

Formats a value according to a specified pattern, allowing custom string representation of numbers, dates, and other types	

```
FORMAT(string_expression, format_string)
```

### INSTR

Finds the first occurrence position of a substring within a string, returning 0 if not found. Similar to POSITION, but with a different syntax.	

```
INSTR(string, substring)
```




### LEFT	

Extracts a specified number of characters from the left side of a string	

```
LEFT(string, length)
```

### LENGTH	

Calculates the number of characters in a string, including spaces and special characters	

```
LENGTH(string)
```

### LIKE

AMC supports the LIKE operator which matches a column name to a string value specified by the user. LIKE does not require regex syntax and works with wildcards.

```
<column1> LIKE <expression> 
```

- `column1` specifies the column that is in the WHERE clause that needs to be checked for a condition.
- `expression` refers to the expression or value that the column needs to be compared to.

### LOCATE	

Alias of CHARINDEX	

```
LOCATE(substring, string[, start_position])
```

### LOWER	

Converts all characters in a string to lowercase, useful for case-insensitive comparisons	

```
LOWER(string)
```

### LTRIM	

Removes leading whitespace characters from the beginning of a string	

```
LTRIM(string)
```

### POSITION	

Finds the first occurrence position of a substring within a string, returning 0 if not found	

```
POSITION(substring IN string)
```

### REGEXP_REPLACE	

Replaces text matching a regular expression pattern with specified replacement text	

```
REGEXP_REPLACE(string, pattern, replacement)
```

### REGEXP_SUBSTR	

Extracts substring matching a regular expression pattern from a string	

```
REGEXP_SUBSTR(string, pattern)
```

### REGEXP_SPLIT

The REGEXP_SPLIT function splits an input string into a collection of substrings, using a regular expression pattern occurrence to split the string.

```
REGEXP(<string>,<pattern>)
```

* `<string>` The input string that will be split into a collection of substrings
* `<pattern>` The regular expression pattern that is matched to split the string.


### REPLACE	

Substitutes all occurrences of a search string with a replacement string within text	

```
REPLACE(string, search, replacement)
```

### RIGHT	

Extracts a specified number of characters from the right side of a string, useful for suffix extraction	

```
RIGHT(string, length)
```

### RTRIM	

Removes trailing whitespace characters from the end of a string, complementing LTRIM	

```
RTRIM(string)
```

### SIMILAR TO

The SIMILAR TO expression evaluates to true only if the value matches the provided regular expression. The regular expression should follow Java regular expression syntax. Both inputs must be strings. The results returned are in a Boolean value.

```
expression \[NOT\] SIMILAR TO expression
```

### STRPOS	

Returns the position of a substring within a string, similar to INSTR but with different syntax	

```
STRPOS(string, substring)
```

### STR_TO_MAP	

Converts a delimited string into a key-value map structure using specified delimiters	

```
STR_TO_MAP(string, pair_delimiter, key_value_delimiter)
```

### SUBSTRING	

Extracts a portion of a string based on starting position and optional length	

```
SUBSTRING(string FROM start [FOR length])
```

### TRIM

The TRIM() function removes the space character or other specified characters from the start or end of a string. This function is useful when working with strings like advertiser or campaign names.

To remove the leading and trailing space characters from str:

```
TRIM(str)
```

To remove the leading and trailing trimStr characters from str:

```
TRIM(BOTH trimStr FROM str)
```

To remove the leading trimStr characters from str:

```
TRIM(LEADING trimStr FROM str)
```

To remove the trailing trimStr characters from str:

```
TRIM(TRAILING trimStr FROM str)
```


### UPPER	

Converts all characters in a string to uppercase, useful for case normalization	

```
UPPER(string)
```

## Math functions

### ABS	

Returns the absolute value of a numeric expression, converting negative numbers to positive while keeping positive numbers unchanged	

```
ABS(numeric_expression)
```

### CEIL	

Rounds a number up to the nearest integer, returning the smallest integer greater than or equal to the input	

```
CEIL(number)
```

### E()	

Returns the mathematical constant e (approximately 2.71828), used in exponential and natural logarithm calculations	

```
E()
```


### EXP	

Calculates e raised to the power of the input value, the inverse of the natural logarithm function	

```
EXP(number)
```

### FLOOR	

Rounds a number down to the nearest integer, returning the largest integer less than or equal to the input value	

```
FLOOR(number)
```


### LN	

Calculates the natural logarithm (base e) of a number, the inverse of the EXP function	

```
LN(number)
```


### LOG	

Calculates the logarithm of a number with a specified base, generalizing logarithmic calculations	

```
LOG(base, number)
```


### LOG10	

Calculates the base-10 logarithm of a number, commonly used for decimal scale calculations	

```
LOG10(number)
```


### POWER	

Raises a number to the specified power, enabling exponential calculations	

```
POWER(number, power)
```


### RANDOM	

Generates a pseudo-random decimal number between 0 (inclusive) and 1 (exclusive)	

```
RANDOM()
```

### ROUND

The ROUND function rounds numeric values to the specified decimal point. The result of input expression must be a numeric data type. If no rounding is specified, the result will default to 0.


```
ROUND(expression [, integer])
```

### SQRT	

Calculates the square root of a non-negative number, returning NULL for negative inputs	

```
SQRT(number)
```


### TRUNC	

Truncates a number to specified decimal places without rounding	

```
TRUNC(number[, decimals])
```

## Aggregate functions


### AVG

Returns the average of values (arithmetic mean) in columns. This function works with numeric data type as input and ignores null values.
If all input values are null, the function returns a null result.

```
AVG(column_name)
```

* \<column_name\> refers to the column in the table that the function will operate on and return the arithmetic mean of the values.


### COUNT

```
COUNT( [ALL | DISTINCT] column )
```

Total number of values in column. ALL is the default. When DISTINCT is specified it, eliminates all duplicate values. However, DISTINCT can't be specified to get the total number of non-null values.

```
COUNT(column_name)
```

or

```
COUNT(DISTINCT column_name)
```

* \<column_name\> refers to the column in the table that the function will operate on.



### MIN

The MIN function returns the minimum value in a set of rows. Nulls will not be considered. Result will be null if all input values are null.

```
MIN(column_name)
```

* \<column_name\> refers to the column in the table that the function will operate on and return the minimum value.

### MAX

The MAX function returns the maximum value of all inputs. Null values will not be considered. Result will be null if all input values are
null.

```
MAX(column_name)
```

* \<column_name\> refers to the column in the table that the function will operate on and return the maximum value.

### SUM

Returns the sum of all values in the column (or expression). This function works with numeric data type and ignores null values. If all
input values are null, the SUM() returns null.

```
SUM(column_name)

```

* \<column_name\> refers to the column in the table that the function will operate on and return the sum of the values in it.

## Date/Time functions


### CONVERT TIME ZONE FROM UTC

Given a time data type, this function converts UTC timestamp to the timestamp of the specified time zone. The input can either be a
TIMESTAMP of DATE data type and the result will match the input's data type. The input time zone follows tz database name format.

```
CONVERT_TIME_ZONE_FROM_UTC(inputTime, 'timeZone')
```

* \<inputTime\> refers to the timestamp in UTC
* \<timezone\> The time zone for the new timestamp.


### CURRENT_DATE	

Returns the current date in UTC, commonly used for date-based comparisons and calculations	

```
CURRENT_DATE
```

### CURRENT_TIMESTAMP	

Returns the current date and time in UTC, providing precise temporal information	

```
CURRENT_TIMESTAMP
```

### DATE\_TRUNC

The DATE_TRUNC function truncates the input timestamp to a specified level of granularity. Granularity must be single quoted and valid
formats can be SECOND, MINUTE, HOUR, DAY, WEEK, MONTH, QUARTER, YEAR.


```
DATE_TRUNC('granularity', inputTime)
```

* \<granularity\> refers to the date part to which to truncate the timestamp value
* \<inputTime\> refers to the timestamp to truncate

### DAYOFMONTH	

Returns the day of the month as a number between 1 and 31	

```
DAYOFMONTH(date)
```

### DAYOFWEEK	

Returns a number representing the day of the week (1-7, where 1 is Sunday), useful for weekday-based logic	

```
DAYOFWEEK(date)
```

### EPOCH TO UTC TIMESTAMP

This function returns the timestamp value with epoch as input time. The input can result in seconds or milliseconds and the result will assume one or the other based on the magnitude. 

```
EPOCH_TO_UTC_TIMESTAMP(expression)
```

* \<expression\> refers to the arguments passed to the function.

The function will produce a timestamp value from the input time

### EXTEND\_TIME\_WINDOW

The EXTEND/_TIME/_WINDOW function allows for the time period of the input tables to be extended beyond the reporting time window.

```
EXTEND_TIME_WINDOW( <table_name>, <Number_of_Days_Retro>, <Number_of_Days_Future> )
```

* \<`table_name`\> refers to the table that needs to be extended beyond the current timeframe of the query
* \<`Number_of_Days_Retro`\> refers to the Number of Days to extend the table retroactively to pull data from
* \<`Number_of_Days_Future`\> refers to the Number of Days to extend the table in the future, beyond the current timeframe of the query

### EXTRACT

The EXTRACT function extracts one time unit from a DATE or TIMESTAMP value. The time unit can be one of SECOND, MINUTE, HOUR, DAY, DOW, WEEK, MONTH, YEAR.

```
EXTRACT(timeUnit FROM expression)
```

* \<timeUnit\> refers to the specific date part in the column or expression of data type of TIMESTAMP

### HOUR	

Extracts the hour component (0-23) from a timestamp, useful for time-based analysis	

```
HOUR(timestamp)
```

### MINUTE	

Extracts the minute component (0-59) from a timestamp value	

```
MINUTE(timestamp)
```

### MONTH	

Extracts the month number (1-12) from a date value	

```
MONTH(date)
```

### SECOND	

Extracts the seconds component (0-59) from a timestamp value	

```
SECOND(timestamp)
```

### WEEK	

Returns the week number (1-53) of a given date	

```
WEEK(date)
```

### YEAR	

Extracts the year component from a date value	

```
YEAR(date)
```


### SECONDS BETWEEN

The SECONDS_BETWEEN function calculates the time difference in seconds between two datetime values. It returns the number of seconds from firstInputTime to secondInputTime, computed as `secondInputTime `minus `firstInputTime`.

```
SECONDS_BETWEEN(firstInputTime, secondInputTime)
```



## Conditional functions

### CASE

The CASE expression is a conditional expressions that returns the expression from first condition that evaluates to true.


```
CASE
  WHEN booleanExpression1 THEN expression1
  WHEN booleanExpression2 THEN expression2
  WHEN booleanExpressionN THEN expressionN
  ELSE expression
END
```

In the above example, each CASE expression is compared and evaluated to the booleanExpression and returns the first matching condition. If a match is not found, the ELSE clause is returned.



### COALESCE

The COALESCE function takes a list of expressions (all of which must be of the same type) and returns the first non-null argument from the list of inputs. If all of the expressions are null, COALESCE returns null.

```
COALESCE(expression [, expression]*)
```

### IF

The IF expression evaluates the input boolean value to be true or false based on the supplied expressions values. In the sample syntax below, the expression evaluates and returns trueExpression if the booleanExpression is true, otherwise returns the falseExpression.


```
IF(booleanExpression, trueExpression, falseExpression)
```

### IFNULL	

Alias of COALESCE	

```
IFNULL(expression, alternate_value)
```

### IN

AMC supports the IN() function to specify multiple values in a WHERE clause.


```
IN( <value1>, <value2>,..... <valueN> )
```

* \<value1\>, \<value2\>, \<valueN\> are the list of values that the column will be specified in the WHERE clause will be compared to. If any of the values match the field, then the IN condition will be true.

The IN() function can also be used with the NOT conditional operator to exclude values

```
NOT IN( <value1>, <value2>,..... <valueN> )
```

### NULLIF	

Returns NULL if two expressions are equal, otherwise returns the first expression	

```
NULLIF(expr1, expr2)
```

### NVL	

Alias of COALESCE

```
NVL(expr1, expr2)
```

### NVL2	
Provides three-way NULL logic, returning expr2 if expr1 is not null, else returns expr3

```
NVL2(expr1, expr2, expr3)
```



## Array functions

### ARRAY\_CAT	

Concatenates two arrays into a single array, preserving the order of elements from both input arrays.	

```
ARRAY_CAT(array1, array2)
```


### ARRAY\_CONTAINS

The `ARRAY_CONTAINS` function returns true if the value is contained in the array. It returns false, if the value is not contained in the array.

```
ARRAY_CONTAINS( <array1>, <value_to_check> )
```

* \<array1\> The source array.
* \<`value_to_check`\> specifies the value that the Array `<array1>` should be checked for



### ARRAY\_COMPLEMENT	

Returns an array containing all elements from the first array that don't exist in the second array.

```
ARRAY_COMPLEMENT(array1, array2)
```

### ARRAY\_DISTINCT	

Removes duplicate elements from an array while maintaining the order of first occurrence of each element	

```
ARRAY_DISTINCT(array)
```

### ARRAY\_INTERSECT

Creates a new array containing only the elements that exist in both input arrays, effectively finding common elements	

```
ARRAY_INTERSECT(array1, array2)
```

### ARRAY\_REMOVE	

Creates a new array with all occurrences of the specified element removed, maintaining the original order of remaining elements	

```
ARRAY_REMOVE(array, element)
```

### ARRAY\_SORT

You can layer on `ARRAY_SORT()` function to sort the collected values in sequence.

```
SELECT
    ARRAY_SORT(COLLECT(DISTINCT <column1>)) AS column1,
    <column2>
  FROM
    <table>
  GROUP BY
    2
```

### ARRAY\_TO\_STRING

The function `ARRAY_TO_STRING`  converts an input array into a string, by concatenating all the elements of the array separated by the separator pattern.

```
ARRAY_TO_STRING(<array>,<separator_pattern>)
```

* <`array`> The input array that will be concatenated into a single string
* <`separator_pattern`> The pattern that is placed between each element of array in the concatenated string

### ARRAY_UNION	
Combines elements from both arrays while removing duplicates, resulting in an array of unique elements from both sources	

```
ARRAY_UNION(array1, array2)
```

### CARDINALITY	

Counts and returns the total number of elements in an array, useful for array size determination	

```
CARDINALITY(array)
```

### COLLECT( \[ALL \| DISTINCT\] column name )

The COLLECT function creates list of all non-unique input values by default. If DISTINCT is specified, the output will only contain unique values. Nulls values are considered in both cases.


```
COLLECT(column_name )
```

or

```
COLLECT( DISTINCT column_name)
```

* \<`column_name`\> refers to the column in the table that the function will operate on and return the list of values.

### EXPLODE	

Transforms an array into multiple rows, creating a new row for each element in the input array	

```
EXPLODE(array)
```

### FLATTEN	

Converts a nested array structure into a single-level array, simplifying complex array hierarchies	

```
FLATTEN(array)
```


## Window functions

Window functions perform calculation over a large set of data, creating windows as columns in aggregate expressions with the following supported operations.

> [NOTE] The OVER clause is used with Window functions and it defines the window specification.

### DENSE\_RANK

Similar to RANK, but if multiple rows have the same value, they will be assigned the same rank and the following row will have the next
consecutive value. RANK would instead assign a non-consecutive value to rows following ties.

```
DENSE_RANK() OVER window
```

The function takes no argument but requires an empty parenthesis along with the OVER clause.


### PARTITION BY

AMC supports the PARTITION BY clause which divides the result set into partitions based on the specified column. PARTITION BY is generally used with window functions like SUM(), MAX(), RANK() etc. which perform the computation on the windows specified by the Partition BY clause.


```
SELECT
   <column1>, <window function>
   OVER(
    PARTITION BY <column2> 
    [ORDER BY <column3>]
    )
FROM table
```

* \<`window_function`\> specifies the operation that should be performed on the sets of rows within a partition
* \<`column2`\> specifies the field that should be used to partition the rows ORDER BY is the optional clause that can be used within the PARTITION BY clause to sort the rows in a specific order
* \<`column3`\> specifies the field that should be used for sorting the rows

### RANK

Evaluates and returns the rank of a row of values, if all rows in the tables were to be ordered by the elements specified in the ORDER BY
clause.

```
RANK() OVER window
```

The function takes no argument but requires an empty parenthesis along
with the OVER clause.


### ROW_NUMBER

Evaluates to the row number based on the ordering of rows specified in the ORDER BY clause


```
ROW_NUMBER OVER window
```

The function takes no argument but requires an empty parenthesis along with the OVER clause.


## Comparison functions

### GREATEST

The GREATEST function returns the largest evaluated expression from a collection of expressions. All expressions must evaluate to numerical data types.

Null will be considered. Result will be null if any input columns value is null.

> [NOTE] To work with null values, use the COALESCE() function.

```
GREATEST(numericExpression [, numericExpression]*)
```

### LEAST

The LEAST function returns the lowest evaluated expression from a collection of expressions. All expressions must evaluate to numerical data types. Null will be considered. Result will be null if any input columns value is null.

> [NOTE] To work with null values, use the COALESCE() function.

```
LEAST(numericExpression [, numericExpression]*)
```

## Hash functions

### HASH64	

Generates a 64-bit hash value for the input	

```
HASH64(expression)
```


### MD5	

Generates a 128-bit MD5 hash value for a string	

```
MD5(string)
```

### SHA1	

Generates a 160-bit SHA-1 hash value for a string	

```
SHA1(string)
```

## VARBINARY functions

### FROM_VARBINARY	

Converts a binary expression into its string representation, useful for working with binary-encoded data. Supported encoding types: 'hex', 'base64', and 'utf-8'	

```
FROM_VARBINARY(binary_expression, type)
```

### TO_VARBINARY	

Converts a string to its binary representation, useful for binary data handling. Supported encoding types: 'hex', 'base64', and 'utf-8'	

```
TO_VARBINARY(string, type)
```

## JSON functions	

### JSON_EXTRACT	

Retrieves a specific value from a JSON string using a path expression, enabling JSON data manipulation	

```
JSON_EXTRACT(json_string, json_path)
```

### JSON_SERIALIZE	

Converts a JSON value into its string representation, useful for storing or transmitting JSON data	

```
JSON_SERIALIZE(json_value)
```


## Advanced functions

### CAST

CAST lets you convert a value or expression to a data type.

```
CAST(expression AS dataType)
```

### NAMED ROW

AMC supports the NAMED_ROW() function which helps group multiple columns under a single name.

```
SELECT NAMED_ROW('label1', <column1>, 'label2', <column2>) AS
<column3>
```

* \<label1\> and \<label2\> refer to labels to reference column1 and column2

## Statistical functions

### Standard deviation and variance

AMC provides support for several statistical functions that calculate the standard deviation and variance of either a population or of a sample set of data.

The functions STDDEV\_POP() and VAR\_POP() calculate the standard deviation and variance of the population, respectively. Use STDEV\_POP() and VAR\_POP() when the values represent the entire population, and are not a sample.

The functions STDDEV\_SAMP() and STDDEV() are synonyms for the same function. These functions can be used interchangeably because they calculate the standard deviation of a sample set of data, and provide the same results.

Similarly, the functions VAR\_SAMP() and VARIANCE() are synonyms for the same function, and calculate the variance of a sample set of data. Use these functions when the values are a sample, and not for the entire population.

| Functions                     | Calculates         | Population or Sample? |
| ----------------------------- | ------------------ | --------------------- |
| STDDEV\_POP\(\)               | Standard deviation | Population            |
| VAR\_POP\(\)                  | Variance           | Population            |
| STDDEV\_SAMP\(\) / STDDEV\(\) | Standard deviation | Sample                |
| VAR\_SAMP\(\) / VARIANCE\(\)  | Variance           | Sample                |

#### STDDEV\_POP(), VAR\_POP()

```
STDDEV_POP(<values>)
VAR_POP(<values>)
```

* \<values\>  A set of numeric values (integer, decimal, or floating-point)

##### Query template

The query below calculates the standard deviation and variation of the clicks from the population of all clicks from cost-per-click (CPC) sponsored ads campaigns.

```
-- Instructional Query: How to use STDDEV_POP() and  VAR_POP() --

-- The population of clicks from CPC sponsored ads campaigns
WITH click_spend AS (
  SELECT
    -- spend is reported as microcents. Divide by 100,000,000 to get the cost in dollars/your currency.
    ad_product_type,
    spend / 100000000 AS spend
  FROM
    sponsored_ads_traffic
  WHERE
    clicks > 0
    and line_item_price_type = 'CPC'
)
SELECT
ad_product_type,
  STDDEV_POP(spend) AS STDDEV_POP_click_cost,
  VAR_POP(spend) as VAR_POP_click_cost
FROM
  click_spend
  group by 1

```

##### Example query results

| ad\_product\_type   | stddev\_pop\_click\_cost | var\_pop\_click\_cost |
| ------------------- | ------------------------ | --------------------- |
| sponsored\_brands   | 1.55863                  | 2.42933               |
| sponsored\_products | 1.15449                  | 1.33286               |

#### STDDEV(), VARIANCE()

```
STDDEV(<values>)
VARIANCE(<values>)
```

* \<values\>  A set of numeric values (integer, decimal, or floating-point)

##### Query template

The query below calculates the standard deviation and variance from a random sample of values for illustrative purpose.

```
-- Instructional Query: How to use STDDEV(), VARIANCE() --

-- Test data below is a sample of the population for illustrative purposes. 
WITH test_data(test) AS (
  VALUES
  
    (20), 
    (45), 
    (30), 
    (66), 
    (50), 
    (60), 
    (85), 
    (70)

)
SELECT
  STDDEV(test) as STDDEV_sample_stardard_deviation,
  VARIANCE(test) as VARIANCE_sample_variance

  
FROM
  test_data
```

##### Example query results

| stddev\_sample\_stardard\_deviation | variance\_sample\_variance |
| ----------------------------------- | -------------------------- |
| 21.45261                            | 460.2142                   |

#### STDDEV\_SAMP(), VAR\_SAMP()

```
STDDEV_SAMP(<values>)
VAR_SAMP(<values>)
```

* \<values\>  A set of numeric values (integer, decimal, or floating-point)

##### Query template

The query below calculates the standard deviation and variance from a random sample of clicks from cost-per-click (CPC) sponsored ads campaigns. Taking a random sample has the potential to result in better query performance, with statistically similar (but not exact) results from the population.

```
-- Instructional Query: How to use STDDEV_SAMP() and VAR_SAMP --

--10% random sample of clicks

WITH click_spend AS (
 SELECT
    -- spend is reported as microcents. Divide by 100,000,000 to get the cost in dollars/your currency.
    ad_product_type,
    spend / 100000000 AS spend
  FROM
    sponsored_ads_traffic
  WHERE
    clicks > 0
    and line_item_price_type = 'CPC'
    and random() <= 0.1
)

SELECT
ad_product_type,
  STDDEV_SAMP(spend) AS STDDEV_SAMP_click_cost,
  VAR_SAMP(spend) AS VAR_SAMP_click_cost
FROM
  click_spend
  group by 1
```

##### Example query results

Note that the sample standard deviation and variance results are slightly different from the population in the previous section.

| ad\_product\_type   | stddev\_samp\_click\_cost | var\_samp\_click\_cost |
| ------------------- | ------------------------- | ---------------------- |
| sponsored\_brands   | 1\.55965                  | 2\.43012               |
| sponsored\_products | 1\.15518                  | 1\.33995               |

### SKEWNESS ()

The SKEWNESS function returns the estimate of the skewness of a set of numeric values (integer, decimal, or floating point). Skewness is a measure of the asymmetry of the distribution. The function will return one value per dimension.

**Skewed to the right**: A positive skewness value above 0.5 indicates that the skew is positive, also known as ‘skewed to the right’ or ‘right tailed’; this means that most of the distribution is concentrated on the left, and the right tail is longer.

**Skewed to the left**: A negative skewness value below -0.5 indicates that the skew is negative, also known as ‘skewed to the left’ or ‘left tailed’; this means that most of the distribution is concentrated on the right, and the left tail is longer.

**Symmetrical**: A skewness value around 0 (between -0.5 - 0.5) indicates that the distribution is nearly symmetrical on the left and the right. For example, a normal distribution has a skewness of zero.

```
SKEWNESS(<values>)
```

* \<values\>  A set of numeric values (integer, decimal, or floating-point)

##### Query template

```
-- Instructional Query: How to use SKEWNESS --

WITH click_spend AS (
    SELECT
    -- spend is reported as microcents. Divide by 100,000,000 to get the cost in dollars/your currency.
    ad_product_type,
    spend / 100000000 AS spend
  FROM
    sponsored_ads_traffic
  WHERE
    clicks > 0
    and line_item_price_type = 'CPC'
)
SELECT
        ad_product_type,
  SKEWNESS(spend) as SKEWNESS_click_cost
FROM
  click_spend
  group by 1
```

##### Example query results

The example results below for illustrative purposes show that both Sponsored Brands and Sponsored Products clicks are skewed to the right, or positively skewed. Sponsored Brands has a longer right tail, since it’s skewness value is higher about 35% larger than Sponsored Products.

| ad\_product\_type   | skewness\_click\_cost |
| ------------------- | --------------------- |
| sponsored\_brands   | 3\.23764              |
| sponsored\_products | 2\.39026              |

### Percentile and Median

#### PERCENTILE(), APPROX\_PERCENTILE ()

The function PERCENTILE() calculates the value at a given percentile of a distribution of values. APPROX\_PERCENTILE calculates the approximate percentile of a distribution of values.

```
PERCENTILE(<values>, <percentile>)
APPROX_PERCENTILE(<values>, <percentile>)
```

* \<values\>  A set of numeric values (integer, decimal, or floating-point)
* \<percentile\> The percentile of the distribution to be found. Value must be between 0 and 1.

#### MEDIAN()

MEDIAN() function calculates the middle value of a set of numbers.

```
MEDIAN(<values>)
```

* \<values\>  A set of numeric values (integer, decimal, or floating-point)

##### Query template

```
-- Instructional Query: How to use PERCENTILE(), APPROX_PERCENTILE() and MEDIAN()--

WITH click_spend AS (
    SELECT
    -- spend is reported as microcents. Divide by 100,000,000 to get the cost in dollars/your currency.
    ad_product_type,
    spend / 100000000 AS spend
  FROM
    sponsored_ads_traffic
  WHERE
    clicks > 0
    and line_item_price_type = 'CPC'
)
SELECT
  ad_product_type,
  PERCENTILE(spend, .1) as PERCENTILE10_click_cost,
  PERCENTILE(spend, .9) as PERCENTILE90_click_cost,
  APPROX_PERCENTILE(spend, .1) as APPROX_PERCENTILE10_click_cost,
  APPROX_PERCENTILE(spend, .9) as APPROX_PERCENTILE90_click_cost,
  MEDIAN(spend) as MEDIAN_click_cost

FROM
  click_spend
  group by 1
```

##### Example query results

| ad\_product\_type   | percentile10\_click\_cost | percentile90\_click\_cost | approx\_percentile10\_click\_cost | approx\_percentile90\_click\_cost | median\_click\_cost |
| ------------------- | ------------------------- | ------------------------- | --------------------------------- | --------------------------------- | ------------------- |
| sponsored\_products | 0\.18                     | 2\.57                     | 0\.18                             | 2\.57                             | 0\.81               |
| sponsored\_brands   | 0\.37                     | 3\.258                    | 0\.37                             | 3\.27                             | 1\.09               |
