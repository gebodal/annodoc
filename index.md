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

To move further with the project we now require a corpus of annotated examples to use as training data for future models. That is where you come in. This documentation will help explain the basics of annotating abstracts of astrophysical papers using brat and the annotation schema we have created.

### Glossary

A brief glossary to help you with the natural language processing (NLP) terminology:

* Entity:
* Relation:
* Attribute:

## brat

The brat rapid annotation tool ([brat](http://brat.nlplab.org/)) allows for annotators to create and edit annotations from a browser. Google Chrome and Safari (desktop) are the only fully suppported browsers (Firefox has visualization support, but only partial editing support, and is not recommended).

A link and login to our brat server will be emailed to you when you sign up to help with this project. Please get in touch if these do not work. Once you have logged in, you should see the brat interface.

<img style="width:100%" src="http://brat.nlplab.org/img/collection-browser-1.png">

From here, navigate to your annotation directory, select a paper, and you may begin annotating.

Entity annotations are made by selecting spans of text with the cursor. Once selected, a window will appear allowing you to select the appropriate entity label, and any necessary attributes for the annotation. Relation annotations are made by clicking-and-dragging from one annotation to another. If an potential relation exists between the start and end annotations (and do note that some annotations are directional) a window will appear asking you to confirm the relation type. Note that once the appropriate entity/relation label has been selected in these windows, the selection may be confirmed by pressing the Enter key.

A brief introduction to the brat interface may be found [here](http://brat.nlplab.org/manual.html).


~~~ ann
H _ { 0 } = 71 \pm 0.04 { km s } ^ { -1 } Mpc ^ { -1 } ( 1 \sigma )
T1 ParameterSymbol 0 9 H _ { 0 }
T2 MeasuredValue 12 54 71 \pm 0.04 { km s } ^ { -1 } Mpc ^ { -1 }
T3 ConfidenceLimit 55 67 ( 1 \sigma )
R1 Measurement Arg1:T1 Arg2:T2
R2 Confidence Arg1:T2 Arg2:T3
~~~

~~~ ann
In the framework of the \Lambda CDM model our joint analysis yields a value of H _ { 0 } = 71 \pm 0.04 { km s } ^ { -1 } Mpc ^ { -1 } ( 1 \sigma ) with the best fit density parameter \Omega _ { M } = 0.27 \pm 0.03 .
T1 ParameterSymbol 79 88 H _ { 0 }
T2 MeasuredValue 91 133 71 \pm 0.04 { km s } ^ { -1 } Mpc ^ { -1 }
T3 ConfidenceLimit 134 146 ( 1 \sigma )
T4 ParameterSymbol 183 197 \Omega _ { M }
T5 MeasuredValue 200 213 0.27 \pm 0.03
R1 Measurement Arg1:T1 Arg2:T2
R2 Confidence Arg1:T2 Arg2:T3
R3 Measurement Arg1:T4 Arg2:T5
~~~

## Annotation Entities and Relations

A full list of the available annotation entities is given below:

{% for e in site.entity %}
* [{{ e.title  }}]({{ e.url | remove_first:'/'  }}): {{ e.shortdef  }}
{% endfor %}

A full list of the annotation relations is given below:

{% for r in site.relation %}
* [{{ r.title  }}]({{ r.url | remove_first:'/'  }}): {{ r.shortdef  }}
{% endfor %}




~~~ ann
Barack Obama is the current president.
T1 PERSON 0 12 Barack Obama
~~~
