---
title: Amazon Marketing Cloud SQL expressions
description: The expressions that can be used with AMC SQL
type: guide
interface: api
---

# Supported expressions

## CONTAINED IN

The CONTAINED IN function returns true if the result of the input expression is equal to any of the expressions that follow the input expressions. If no match is found, the function returns FALSE.

```
CONTAINED_IN(testExpression, expression [, expression]*)
```

## BUILT-IN PARAMETER

This function returns a built-in parameter by name.


```
BUILT_IN_PARAMETER('parameterName')
```

> [NOTE] Currently AMC supports TIME\_WINDOW\_START and TIME\_WINDOW\_END. Both return the start or end of the overall workflow time window.

## CUSTOM PARAMETER

Return input parameter value by name set at the workflow definition level. 


```
CUSTOM_PARAMETER('parameterName')
```

## UUID

Return random universally unique identifier (UUID) string.


```
UUID()
```