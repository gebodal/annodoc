---
layout: entry
title: Numerical Atlas Documentation
---

Welcome to the Numerical Atlas annotation project documentation. The following sections will introduce you to the annotation schema will shall be using for this project, and the annotation tool which shall be employing (brat rapid annotation tool).

## Table of Contents

* [Project Goal](#project-goal)

## Project Goal

The goal of this project is to produce a tool to automatically extract measurements from the astrophysical literature. 'Measurement' in this context refers to a numerical value associated with a named entity. We hope to be able to extract and record the names, symbols, measured values, uncertainties, units, and confidence limits of measurements present in the text of astrophysics papers.

Our initial work towards this may be found [here](https://arxiv.org/abs/1902.00027).

To move further with the project we now require a corpus of annotated examples to use as training data for future models. 



## Annotation Entities and Relations

{% for e in site.entity %}
* [{{ e.title  }}]({{ e.url | remove_first:'/'  }}): {{ e.shortdef  }}
{% endfor %}

{% for e in site.relation %}
* [{{ r.title  }}]({{ r.url | remove_first:'/'  }}): {{ r.shortdef  }}
{% endfor %}




~~~ ann
Barack Obama is the current president.
T1 PERSON 0 12 Barack Obama
~~~
