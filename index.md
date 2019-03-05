---
layout: entry
title: Numerical Atlas Annotation Documentation
---

Welcome to the Numerical Atlas annotation project documentation. The following sections will introduce you to the annotation schema will shall be using for this project, and the annotation tool which shall be employing (brat rapid annotation tool).

## Table of Contents

* [Project Goal](#project-goal)


## Project Goal

The goal of this project is to produce a tool to automatically extract measurements from the astrophysical literature. 'Measurement' in this context refers to a numerical value associated with a named entity - for example the "Hubble constant", or "the mass of Jupiter". We hope to be able to extract and record the names, symbols, measured values, uncertainties, units, and confidence limits of measurements present in the text of astrophysics papers.

WE HAVE ALREADY WRITTEN A PAPER ON OUR BASELINE MODELS SUBMITTED TO ETC
Our initial work towards this may be found [here](https://arxiv.org/abs/1902.00027 "Machine-Assisted Meta-Studies").

To move further with the project we now require a corpus of annotated examples to use as training data for future models TALK ABOUT DEEP LEARNING. That is where you will prove useful. This documentation will help explain the basics of annotating abstracts of astrophysical papers using brat and the annotation schema we have created.

### Glossary

A brief glossary to help you with the natural language processing (NLP) terminology:

* Entity:
* Relation:
* Attribute:

## brat

The brat rapid annotation tool ([brat](http://brat.nlplab.org/ "brat rapid annotation tool")) allows for annotators to create and edit annotations from a browser. Google Chrome and Safari (desktop) are the only fully suppported browsers (Firefox has visualization support, but only partial editing support, and is not recommended). Mobile browsers are not supported at all.

A link and login to our brat server will be emailed to you when you sign up to help with this project. Please get in touch if these do not work. Once you have logged in, you should see the brat interface.

<img style="width:60%" src="http://brat.nlplab.org/img/collection-browser-1.png">

From here, navigate to your annotation directory, select a paper, and you may begin annotating.

Entity annotations are made by selecting spans of text with the cursor. Once selected, a window will appear allowing you to select the appropriate entity label, and any necessary attributes for the annotation. Relation annotations are made by clicking-and-dragging from one annotation to another. If a potential relation exists between the start and end annotations (and do note that some annotations are directional) a window will appear asking you to confirm the relation type. Note that once the appropriate entity/relation label has been selected in these windows, the selection may be confirmed by pressing the Enter key.

An introduction to the brat interface may be found [here](http://brat.nlplab.org/manual.html "brat manual").

## Annotations

### Entities

The primary goal of this project is to link numerical measurements to physical entities in free text, and as such this is the backbone of our annotation process. Numerical measurements are relatively easy to identify in free text, and here are classified into two forms: measured values, and constraints. Physical entities are a little more complex in the ways they may be represented in text. Here we annotate them using three primary Entity labels: `Parameter Name`, `Parameter Symbol`, and `Object Name`. These different labels are discussed below:

#### Measured Value

An example of a `measured value` would be:

<div markdown="1">
~~~ ann
H _ { 0 } = 71 \pm 0.04 { km s } ^ { -1 } Mpc ^ { -1 } ( 1 \sigma )
T1 ParameterSymbol 0 9 H _ { 0 }
T2 MeasuredValue 12 54 71 \pm 0.04 { km s } ^ { -1 } Mpc ^ { -1 }
T3 ConfidenceLimit 55 67 ( 1 \sigma )
R1 Measurement Arg1:T1 Arg2:T2
R2 Confidence Arg1:T2 Arg2:T3
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0709.2195" target="_blank">arXiv</a>)</span>
</div>

When annotating a measured value, please include the central value, along with any uncertainties and units that may be given. Measured values in a scientific context should have at least one uncertainty associated with them, but it is possible that this uncertainty may be given somewhere else in the text. INCLUDE SEPARATEUNCERTAINTY If you encounter a value with no uncertainty, which is nonetheless clearly linked to some physical entity, it may require an attribute to be attached to the annotation (which may be done from the window which appears after a span is selected with the cursor, or when double-clicking on an existing annotation) such as the `From Literature` attribute. If none of the attributes seem appropriate, please consider carefully whether this value really constitutes a measurement, or if it is merely some contingent value to another statement in the text. For example, in the following:

<div markdown="1">
~~~
We achieve a distance measure at redshift z = 0.275 , of r _ { s } ( z _ { d } ) / D _ { V } ( 0.275 ) = 0.1390 \pm 0.0037 ( 2.7 % accuracy )
T1 ParameterName 13 29 distance measure
T2 ParameterSymbol 57 102 r _ { s } ( z _ { d } ) / D _ { V } ( 0.275 )
T3 MeasuredValue 105 122 0.1390 \pm 0.0037
R1 Name Arg1:T1 Arg2:T2
R2 Measurement Arg1:T2 Arg2:T3
~~~
{:.ann tabs="yes"}
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0907.1660" target="_blank">arXiv</a>)</span>
</div>

The "redshift z = 0.275" is definitely not a MeasuredValue, despite having a name, symbol, and value.

A MeasuredValue annotation may also require an "Attribute" to be set to indicate additional information about the stated measurement. These are discussed [below](#attributes "Attributes").

#### Constraint

And an example of a `constraint` would be:

<div markdown="1">
~~~ ann
-0.0179 < \Omega _ { k } < 0.0081 \mbox { ( 95 \% CL ) }
T1 Constraint 0 33 -0.0179 < \Omega _ { k } < 0.0081
T2 ConfidenceLimit 44 49 95 \%
R1 Confidence Arg1:T1 Arg2:T2
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0803.0732" target="_blank">arXiv</a>)</span>
</div>

When annotating a constraint, please include the value and any associated units, the equality operator (`<`, `>`, `\gt`, `\lt`, `\geq`, etc.), and the mathematical symbol being used in the expression, if present. It is possible that some of these components may not be present in the constraint, as in the following example:

<div markdown="1">
~~~ ann
Allowing the inclination to be a free parameter we find a lower limit for the spin of 0.90 , this value increases to that of a maximal rotating black hole when the inclination is set to that of the orbital plane of J1655-40 .
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0902.1745" target="_blank">arXiv</a>)</span>
</div>

In this case, HOW TO DEAL WITH THIS.

A MeasuredValue annotation may also require an "Attribute" to be set to indicate additional information about the stated measurement. These are discussed [below](#attributes "Attributes").

#### Physical Entities

A physical entity here is considered to be a property (either of the Universe or a specific object) which may be measured in some way to produce a numerical result. For instance, we may measure the Hubble Constant (property of the Universe), or the radius of the Milky Way Galaxy.

##### ParameterName

Property names are identified using the `Parameter Name` annotation. This annotation is specifically for written (i.e. linguistic) names of physical entities. For example in the following sentence:

<div markdown="1">
~~~ ann
...this time-delay estimate yields a Hubble parameter of H _ { 0 } = 52 ^ { +14 } _ { -8 } ~ { } { km } ~ { } { s ^ { -1 } } ~ { } { Mpc ^ { -1 } } ( 95 \% confidence level ) where...
T1 ParameterName 37 53 Hubble parameter
T2 ParameterSymbol 57 66 H _ { 0 } 
T3 MeasuredValue 69 147 52 ^ { +14 } _ { -8 } ~ { } { km } ~ { } { s ^ { -1 } } ~ { } { Mpc ^ { -1 } }
T4 ConfidenceLimit 150 155 95 \%
R1 Name Arg1:T1 Arg2:T2
R2 Measurement Arg1:T2 Arg2:T3
R3 Confidence Arg1:T3 Arg2:T4
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/astro-ph/0007136" target="_blank">arXiv</a>)</span>
</div>

note that the written name has a separate annotation to the mathematical symbol. The two should be linked by the annotator, however, as discussed [below](#measurement "Measurement").

Parameter names can become complicated due to being dependant on other parts of the text (for instance, when the word "radius" is used as a parameter name, what is it the radius of?). This is discussed below in the section on the [Object entity](#objectname "ObjectName").

Please annotate all instances of Parameter Names that you encounter - even if they are not directly tied to a mathematical symbol or measurement. For instance, in the following sentence:

<div markdown="1">
~~~ ann
...suggesting that the combination of these two independent phenomena provides an interesting method to constrain the Hubble constant .
T1 ParameterName 129 144 Hubble constant
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0709.2195" target="_blank">arXiv</a>)</span>
</div>

the annotation is helpful, as it aids the entity recoginition phase of our model training.

##### ParameterSymbol

Mathematical symbols are annotated separately to the written names of physical entities. When a mathematical symbol appears next to a written name, please ensure that you give separate labels for each, as in the following example:

<div markdown="1">
~~~ ann
...significantly reduces the uncertainty in the optical depth parameter , \tau .
T1 ParameterName 48 71 optical depth parameter
T2 ParameterSymbol 74 78 \tau
R1 Name Arg1:T1 Arg2:T2
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0803.0732" target="_blank">arXiv</a>)</span>
</div>

<div markdown="1">
~~~ ann
...where r _ { s } ( z _ { d } ) is the comoving sound horizon at the baryon drag epoch...
T1 ParameterSymbol 9 32 r _ { s } ( z _ { d } )
T2 ParameterName 40 87 comoving sound horizon at the baryon drag epoch
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0907.1660" target="_blank">arXiv</a>)</span>
</div>

Note that due to our text being derived from LaTeX source files, there may be some fairly convoluted TeX syntax within the spans for mathematical symbols. Please take the time to properly determine the start and end of these symbols, as this will be very helpful for training our future models.

As for ParameterNames, please annotate all instances of ParameterSymbols you encounter, even if they appear independently of any measurements or ParameterNames.

<div markdown="1">
~~~ ann
Observations of megamaser disks in other galaxies will further reduce the uncertainty in H _ { 0 } as measured by the MCP .
T1 ParameterSymbol 89 98 H _ { 0 }
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/1005.1955" target="_blank">arXiv</a>)</span>
</div>

A caveat to this is in the case of equations: a separate annotation, `Definition`, exists for this situation. See [below](#definition "Definition"). 

##### Object

Global properties (e.g. Hubble constant) may usually be identified in free text by their name (or symbol, if a recognisable one exists) alone, but this is often not the case for properties of objects. For example, consider the following sentence:

<div markdown="1">
~~~ ann
The inclination of this system is estimated at 55 ^ { +2 } _ { -7 } degrees .
T1 ParameterName 4 15 inclination
T2 MeasuredValue 47 75 55 ^ { +2 } _ { -7 } degrees
R1 Measurement Arg1:T1 Arg2:T2
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0902.1745" target="_blank">arXiv</a>)</span>
</div>

Here we have no indication of what object "inclination" relates to. In cases such as this, however, the object is generally referenced somewhere else in the text, and we use the `Object Name` annotation to identify this object. We may then link the Object Name to the Parameter Name or Parameter Symbol annotation (preferring the written name if available) using the Property relation as described in the [relations section](#property "Property").

In the case where the object and property appear in the same sentence, please annotate the Object Name and Property Name separately, as in the following examples:

EXAMPLE WHERE OBJECT AND PARAMETER ARE NEXT TO EACH OTHER

EXAMPLE WHERE OBJECT AND PARAMETER ARE SEPARATED IN SAME SENTENCE

Please annotate all instances of Object Names that you encounter, even if they are not directly linked to properties or measurements, as this aids our entity recognition models.

<div markdown="1">
~~~ ann
This nuclear GC has luminosity , color , and structural parameters similar to that of \omega Cen and M 54...
T1 Object 86 96 \omega Cen
T2 Object 101 105 M 54
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0709.2195" target="_blank">arXiv</a>)</span>
</div>

#### Confidence Limit

An additional detail included with many measurements is the confidence limit of the stated uncertainties, as in the following example:

<div markdown="1">
~~~ ann
H _ { 0 } = 71 \pm 0.04 { km s } ^ { -1 } Mpc ^ { -1 } ( 1 \sigma )
T1 ParameterSymbol 0 9 H _ { 0 }
T2 MeasuredValue 12 54 71 \pm 0.04 { km s } ^ { -1 } Mpc ^ { -1 }
T3 ConfidenceLimit 55 67 ( 1 \sigma )
R1 Measurement Arg1:T1 Arg2:T2
R2 Confidence Arg1:T2 Arg2:T3
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0709.2195" target="_blank">arXiv</a>)</span>
</div>

In such cases, please annotate the confidence limit value and any necessary "unit" (e.g. "\sigma" or "%"), but leave off any additional text (e.g. "confidence limit" or "C.L."), as in:

<div markdown="1">
~~~ ann
The new limit on the tensor-to-scalar ratio is r < 0.22 \mbox { ( 95 \% CL ) }
T1 ParameterName 21 43 tensor-to-scalar ratio
T2 Constraint 47 55 r < 0.22
T3 ConfidenceLimit 66 71 95 \%
R1 Measurement Arg1:T1 Arg2:T2
R2 Confidence Arg1:T2 Arg2:T3
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0803.0732" target="_blank">arXiv</a>)</span>
</div>

This annotation may then be linked to a measurement annotation using the `Confidence` relation, as discussed [here](#confidence "Confidence").

#### Definition

It is beneficial to distinguish mathematical symbols appearing inside equations from those appearing as independant entities in a sentence. To this end, we also have a `Definition` annotation, which should be used to annotate equations of the form `SYMBOL = EXPRESSION`, as in:

<div markdown="1">
~~~ ann
...mass density parameter \Omega _ { M } = 1 - \Omega _ { \Lambda }...
T1 ParameterName 3 25 mass density parameter
T2 ParameterSymbol 26 40 \Omega _ { M }
T3 Definition 43 67 1 - \Omega _ { \Lambda }
R1 Name Arg1:T1 Arg2:T2
R2 Defined Arg1:T2 Arg2:T3
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0709.2195" target="_blank">arXiv</a>)</span>
</div>

If you encounter a mathematical expression outside of an equation (i.e. there is no equals sign present), determine if the expression qualifies as a physical quantity in its own right - for instance, is there a measurement of this quantity provided in the text? In such cases, annotate the expression as a ParameterSymbol, and associate it normally with any ParameterName, MeasuredValue, or Constraint annotations. An example of this would be:

<div markdown="1">
~~~ ann
\Omega _ { b } h ^ { 2 } = 0.034 ^ { +0.007 } _ { -0.007 }
T1 ParameterName 0 24 \Omega _ { b } h ^ { 2 }
T2 MeasuredValue 27 58 0.034 ^ { +0.007 } _ { -0.007 }
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/astro-ph/0212497" target="_blank">arXiv</a>)</span>
</div>

### Relations

In addition to identifying spans of text which correspond to certain entities (names, symbols, numbers, etc.), we are also interested in the relationships between these entities - hence we define "Relations" between entities. Practically, these relations are created by clicking-and-dragging from one entity annotation to another. A window will appear in the brat interface allowing you to specify which type of relation you wish to create between the two entities. Note that the interface will only present the possible relations which may exist between the two entities, as each relation has defined start and end types.

In all cases, the ideal annotation format would be of the form: Object → ParameterName → ParameterSymbol → Measurement (MeasuredValue or Constraint) → ConfidenceLimit.

The available relations are discussed in the following sections.

#### Measurement

A `Measurement` relation starts at a ParameterName or ParameterSymbol, and ends at a MeasuredValue or Constraint. This relation indicates that the MeasuredValue or Constraint is a stated measurement of the physical entity denoted by the ParameterName/ParameterSymbol. Ideally, all MeasuredValue and Constraint annotations should have an associated Measurement relation, but there are edge cases where it is very difficult to identify an appropriate ParameterName or ParameterSymbol. In such cases, please annotate as fully as possible.

An example of a standard Measurement relation would be:

<div markdown="1">
~~~ ann
H _ { 0 } = 71 \pm 0.04 { km s } ^ { -1 } Mpc ^ { -1 } ( 1 \sigma )
T1 ParameterSymbol 0 9 H _ { 0 }
T2 MeasuredValue 12 54 71 \pm 0.04 { km s } ^ { -1 } Mpc ^ { -1 }
T3 ConfidenceLimit 55 67 ( 1 \sigma )
R1 Measurement Arg1:T1 Arg2:T2
R2 Confidence Arg1:T2 Arg2:T3
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0709.2195" target="_blank">arXiv</a>)</span>
</div>

It is possible that a given ParameterName or ParameterSymbol may have multiple measurement values associated with it. In which case simple create multiple relations from a single ParameterName/ParameterSymbol.

#### Name

A `Name` relation is between a ParameterName and a ParameterSymbol, and is used to indicate that the given ParameterName is the linguistic name of the ParameterSymbol. For instance, the phrase "Hubble constant" is the linguistic name (ParameterName) for the symbol "H\_0" (ParameterSymbol).

It is very likely that many mathematical symbols will be given in the text without reference to a linguistic name, especially in contexts where the symbol is commonly agreed upon within the field (for instance, it is not uncommon for "H\_0" and "\Omega\_\Lambda" to be used in cosmology papers without explanation). However, please do attempt to find an appropriate span in the text, and remember that the name and symbol may appear in different sentences.

You will also encounter situations in which a name and symbol appear together without a nearby measurement. In such cases there will often be a measurement of the physical entity given later in the text (probably using only the symbol). When this occurs, please ensure you create a relation between the name and symbol which appear together, but do not create a Name relation between the ParameterName at the start of the document and the ParameterSymbol at the end (provided that the symbol uses exactly the same text). For example:

EXAMPLE - NEEDS PICTURE

Any "missing" relations may be easily reconstructed automatically, and the resulting markup in the brat interface much more readable.

However, if the only name/symbol pair in the text is greatly separated, please do annotate this - we require at least one relation between the spans in a given document.

#### Property

A `Property` relation is between an Object and a ParameterName or ParameterSymbol, and is used to indicate that the name or symbol is given as a property of the annotated object. For example:

<div markdown="1">
~~~ ann
...we infer the spin parameter of SWIFT J1753.5-0127 to be 0.76 ^ { +0.11 } _ { -0.15 } .
T1 ParameterName 16 30 spin parameter
T2 Object 34 52 SWIFT J1753.5-0127
T3 MeasuredValue 59 87 0.76 ^ { +0.11 } _ { -0.15 }
R1 Property Arg1:T2 Arg2:T1
R2 Measurement Arg1:T1 Arg2:T3
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0709.2195" target="_blank">arXiv</a>)</span>
</div>

Please include these relations wherever possible, as they are prone to linguistic ambiguity - especially in the cases where there are multiple objects referenced in the same text.

#### Confidence

A `Confidence` relation is between a MeasuredValue or Constraint and a ConfidenceLimit, and is used to relate a measurement with a stated confidence. Note that in many cases a single confidence annotation may be used for several measurements. In which case, simply link the multiple measurements to the single annotation - this is perfectly acceptable in the brat interface.

As an example:

<div markdown="1">
~~~ ann
H _ { 0 } = 71 \pm 0.04 { km s } ^ { -1 } Mpc ^ { -1 } ( 1 \sigma )
T1 ParameterSymbol 0 9 H _ { 0 }
T2 MeasuredValue 12 54 71 \pm 0.04 { km s } ^ { -1 } Mpc ^ { -1 }
T3 ConfidenceLimit 55 67 ( 1 \sigma )
R1 Measurement Arg1:T1 Arg2:T2
R2 Confidence Arg1:T2 Arg2:T3
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0709.2195" target="_blank">arXiv</a>)</span>
</div>

#### Defined

A `Defined` relation is between a ParameterName or ParameterSymbol and a Definition, and is used to indicate that the physical entity in question is defined by the mathematical expression in the Definition. An example of this would be:

<div markdown="1">
~~~ ann
...mass density parameter \Omega _ { M } = 1 - \Omega _ { \Lambda }...
T1 ParameterName 3 25 mass density parameter
T2 ParameterSymbol 26 40 \Omega _ { M }
T3 Definition 43 67 1 - \Omega _ { \Lambda }
R1 Name Arg1:T1 Arg2:T2
R2 Defined Arg1:T2 Arg2:T3
~~~
0709.2195
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0709.2195" target="_blank">arXiv</a>)</span>
</div>

### Attributes

An attribute is a label associated with an entity annotation. This is used to provide additional information about that annotation - for example, information implied by the context, but not actually included in the span itself.

An attribute may be added to an annotation when the annotation is created, or by double clicking on an existing annotation. At the bottom of the window which appears in these cases will be a list of available attributes for the annotation - simply click on the attribute name to toggle it on/off. Multiple attributes may be placed on the same annotation. Note that attributes are entity-type specific, and so it is possible that no options for attributes may appear for a given entity type.

PICTURE

For this project, only MeasuredValue and Constraint entities have available attributes. These are detailed below.

#### FromLiterature

The `FromLiterature` attribute is used to indicate that a given MeasuredValue or Constraint is a quoted value from the scientific literature (either from a specific, named source, or merely an accepted value in the community). Such values are usually clearly declared as such in the text.

#### Incorrect

The `Incorrect` attribute is used to indicate that the measurement annotation has been deemed incorrect by the authors of the paper. Usually this will be for a quoted value, where the authors are referencing a value from literature which their work disagrees with (they may have a competing value of their own given in the text). However, it is also possible that the authors provide their own determination of a given quantity, and note its disagreement with the literature value, and conclude that there is some underlying problem in their assumptions or technique - whilst rare, this can occur (REFERENCE HIGH H\_0 VALUE PAPER).

#### AcceptedValue

The `AcceptedValue` attribute is used to indicate that a given measurement is presented as the correct one - this is useful in cases where there are multiple determinations of a given quantity presented by the authors. If one measurement is indicated as being the accepted/final value, please add this attribute.

## Notes



Need:
Bad examples
	Messy TeX span
	Confusing details case
	Cases to ignore
Edge cases
	Details
Don't annotate hyper-specific examples
	Percentages relating to specific catalogue/sample
	Details regarding specific catalogue/sample
Important
	Please link names and symbols even if they do not occur with a numerical measurement
	In cases where an parameter/object is mentioned multiple times, please try and make links between linguistically significant pairs
When not to annotate an abstract
Can send link from brat to specific annotation

<div markdown="1">
~~~ ann
The half-light radii and ellipticities of the GCs in our sample ( \bar { r _ { h } } \simeq 3.3 pc , \overline { \epsilon } \simeq 0.1 ) are similar to those of old GCs in the Magellanic Clouds and to those of “ Old Halo ” ( OH ) GCs in our Galaxy , but not as extended and spherical as the Galactic “ Young Halo ” ( YH ) GCs ( \bar { r _ { h } } \simeq 7.7 pc , \overline { \epsilon } \simeq 0.06 ) .
~~~
0803.0551
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/" target="_blank">arXiv</a>)</span>
</div>

Complicated examples:
* 0803.0551
* 0905.3401

The following is a very clean example of the kind of text spans we are looking for:

<div markdown="1">
~~~ ann
H _ { 0 } = 71 \pm 0.04 { km s } ^ { -1 } Mpc ^ { -1 } ( 1 \sigma )
T1 ParameterSymbol 0 9 H _ { 0 }
T2 MeasuredValue 12 54 71 \pm 0.04 { km s } ^ { -1 } Mpc ^ { -1 }
T3 ConfidenceLimit 55 67 ( 1 \sigma )
R1 Measurement Arg1:T1 Arg2:T2
R2 Confidence Arg1:T2 Arg2:T3
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0709.2195" target="_blank">arXiv</a>)</span>
</div>

Most examples will not be this neat, but many will follow this general pattern. For example:

<div markdown="1">
~~~ ann
Hubble constant H _ { \lower 2.0 pt \hbox { \$ \scriptstyle 0 \$ } } = 61.7 ^ { +1.2 } _ { -1.1 } \hbox { km } \hbox% { sec } ^ { -1 } \hbox { Mpc } ^ { -1 }
T1 ParameterName 0 15 Hubble constant
T2 ParameterSymbol 16 68 H _ { \lower 2.0 pt \hbox { \$ \scriptstyle 0 \$ } }
T3 MeasuredValue 71 158 61.7 ^ { +1.2 } _ { -1.1 } \hbox { km } \hbox% { sec } ^ { -1 } \hbox { Mpc } ^ { -1 }
R1 Name Arg1:T1 Arg2:T2
R2 Measurement Arg1:T2 Arg2:T3
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0709.2535" target="_blank">arXiv</a>)</span>
</div>

This follows the same basic pattern, but with TeX structs taking up much more space.

<div markdown="1">
~~~ ann
the determination of the equation-of-state parameter of dark energy , w = P / ( \rho c ^ { 2 } )
T1 ParameterName 25 67 equation-of-state parameter of dark energy
T2 ParameterSymbol 70 71 w
T3 Definition 74 96 P / ( \rho c ^ { 2 } )
R1 Name Arg1:T1 Arg2:T2
R2 Defined Arg1:T2 Arg2:T3
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0905.0695" target="_blank">arXiv</a>)</span>
</div>

<div markdown="1">
~~~ ann
We found that 13 nights of V5116 Sgr observations in the year 2006 are modulated with a period of 0.1238 \pm 0.0001 d ( 2.9712 \pm 0.0024 h )
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0710.5701" target="_blank">arXiv</a>)</span>
</div>

<div markdown="1">
~~~ ann
The mass ratio of non–baryonic dark matter to baryonic matter is 3.1 ^ { +2.5 } _ { -2.4 }
T1 ParameterName 4 61 mass ratio of non–baryonic dark matter to baryonic matter
T2 MeasuredValue 65 90 3.1 ^ { +2.5 } _ { -2.4 }
R1 Measurement Arg1:T1 Arg2:T2
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0709.2535" target="_blank">arXiv</a>)</span>
</div>

<div markdown="1">
~~~ ann
this time-delay estimate yields a Hubble parameter of H _ { 0 } = 52 ^ { +14 } _ { -8 } ~ { } { km } ~ { } { s ^ { -1 } } ~ { } { Mpc ^ { -1 } } ( 95 \% confidence level ) where the errors include time-delay as well as model uncertainties
T1 ParameterName 34 50 Hubble parameter
T2 ParameterSymbol 54 63 H _ { 0 }
T3 MeasuredValue 66 145 52 ^ { +14 } _ { -8 } ~ { } { km } ~ { } { s ^ { -1 } } ~ { } { Mpc ^ { -1 } }
T4 ConfidenceLimit 148 153 95 \%
R1 Name Arg1:T1 Arg2:T2
R2 Measurement Arg1:T2 Arg2:T3
R3 Confidence Arg1:T3 Arg2:T4
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/astro-ph/0007136" target="_blank">arXiv</a>)</span>
</div>

<div markdown="1">
~~~ ann
D _ { A } ( z ) is the angular diameter distance and H ( z ) is the Hubble parameter 
T1 ParameterSymbol 0 15 D _ { A } ( z )
T2 ParameterName 23 48 angular diameter distance
T3 ParameterSymbol 53 60 H ( z )
T4 ParameterName 68 84 Hubble parameter
R1 Name Arg1:T1 Arg2:T2
R2 Name Arg1:T3 Arg2:T4
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0907.1660" target="_blank">arXiv</a>)</span>
</div>

<div markdown="1">
~~~ ann
\sigma _ { 8 } \Omega _ { m } ^ { 0.6 } = 0.54 ( 0.40 , 0.73 )
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/astro-ph/0006170" target="_blank">arXiv</a>)</span>
</div>

Ambiguous - should the z = 0.275 be included in the ParameterName or as a separate Details?

<div markdown="1">
~~~ ann
We achieve a distance measure at redshift z = 0.275 , of r _ { s } ( z _ { d } ) / D _ { V } ( 0.275 ) = 0.1390 \pm 0.0037 ( 2.7 % accuracy )
T1 ParameterName 13 29 distance measure
T2 ParameterSymbol 57 102 r _ { s } ( z _ { d } ) / D _ { V } ( 0.275 )
T3 MeasuredValue 105 122 0.1390 \pm 0.0037
R1 Name Arg1:T1 Arg2:T2
R2 Measurement Arg1:T2 Arg2:T3
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0907.1660" target="_blank">arXiv</a>)</span>
</div>

<div markdown="1">
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
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0709.2195" target="_blank">arXiv</a>)</span>
</div>

<div markdown="1">
~~~ ann
We achieve a distance measure at redshift z = 0.275 , of r _ { s  } ( z _ { d  }  ) / D _ { V  } ( 0.275  ) = 0.1390 \pm 0.0037 ( 2.7 % accuracy  ) , where r _ { s  } ( z _ { d  }  ) is the comoving sound horizon at the baryon drag epoch , D _ { V  } ( z  ) \equiv [ ( 1 + z  ) ^ { 2  } D _ { A  } ^ { 2  } cz / H ( z  )  ] ^ { 1 / 3  } , D _ { A  } ( z  ) is the angular diameter distance and H ( z  ) is the Hubble parameter .
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0907.1660" target="_blank">arXiv</a>)</span>
</div>

<div markdown="1">
~~~ ann
From the maximum magnitude – rate of decline relation , we estimate the maximum absolute visual magnitude of M _ { Vmax } = -8.85 \pm 0.04 mag
T1 ParameterName 72 105 maximum absolute visual magnitude
T2 ParameterSymbol 109 121 M _ { Vmax }
T3 MeasuredValue 124 142 -8.85 \pm 0.04 mag
R1 Name Arg1:T1 Arg2:T2
R2 Measurement Arg1:T2 Arg2:T3
~~~
<span style="float:right;font-size:75%;opacity:0.5">(See on <a href="https://arxiv.org/abs/0710.5701" target="_blank">arXiv</a>)</span>
</div>

## Annotation Entities and Relations

A full list of the available annotation entities is given below:

{% for e in site.entity %}
* [{{ e.title  }}]({{ e.url | remove_first:'/'  }}): {{ e.shortdef  }}
{% endfor %}

A full list of the annotation relations is given below:

{% for r in site.relation %}
* [{{ r.title  }}]({{ r.url | remove_first:'/'  }}): {{ r.shortdef  }}
{% endfor %}

## Contact

If you encounter issues with your annotation work, or have questions regarding logging in or using brat, please contact us by email [here](mailto:example@name.org).
