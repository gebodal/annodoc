---
layout: entry
title: "Measurement"
shortdef: "Relationship between an entity name/symbol and a measurement/constraint of that entity"
---

The `FAMILY` relation is annotated between two `PERSON` entities that
are stated to stand in any familial relationship.

<!-- details -->

Example:

~~~ ann
Michelle Obama is the wife of Barack Obama.
T1 PERSON 0 14 Michelle Obama
T2 PERSON 30 42 Barack Obama
R1 FAMILY Arg1:T1 Arg2:T2
~~~
