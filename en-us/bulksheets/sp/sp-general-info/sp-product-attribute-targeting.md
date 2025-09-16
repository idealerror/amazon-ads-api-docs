---
title: Bulksheets legacy information
description: Information about the legacy version of bulksheets.
type: guide
interface: bulk-operations
---

# Product Attribute Targeting

## Targeting Predicates

Following is a list describing the available targeting predicates along with sample expressions.

| Targeting Predicate         | Sample Expression                                                            |
|-----------------------------|------------------------------------------------------------------------------|
| category same as            | category="9"                                                                 |
| asin same as                | asin="B0794SNF6C"                                                            |
| brand same as               | category="5524098011" brand="8046181011"                                     |
| price less than             | category="5524098011" price<10                                               |
| price between               | category="5524098011" price=10-20                                            |
| price greater than          | category="5524098011" price>10                                               |
| review rating less than     | category="5524098011" rating<4                                               |
| review rating between       | category="5524098011" rating=4.5-5                                           |
| review rating greater than  | category="5524098011" rating>4.5                                             |
| prime shipping eligible     | category="5524098011" prime-shipping-eligible="true"                         |
| age range                   | category="166099011" age-range="5442387011" prime-shipping-eligible="true"   |
| genre                       | category="8619203011" prime-shipping-eligible="true" age-range="1" genre="1” |


 <br/> 
 <br/> 

## Product Targeting Expression Rules

* All IDs passed for category, genre, age range and brand predicates must be valid IDs in our browse system.
* Expressions must specify either a category predicate or an ASIN predicate, but never both.
* Only one category may be specified per targeting expression.
* Only one brand may be specified per targeting expression.
* Only one asin may be specified per targeting expression.
* To exclude a brand from a targeting expression you must create a negative targeting expression in the same ad group as the positive targeting expression.
* Brand, price, and review, and Prime Shipping eligible predicates are optional and may only be specified if category is also specified.
* Review predicates accept numbers between 0 and 5 and are inclusive.
* When using either of the ‘between’ strings to construct a targeting expression, the format of the string is ‘numeric-numeric where the first numeric must be smaller than the second numeric. Prices are not inclusive.
* Age Range and Genre predicates are optional, and may only be specified for categories that support the refinement. 

 <br/> 
 <br/> 