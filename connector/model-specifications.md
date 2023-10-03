---
description: >-
  Our UML model is the source we use to generate the source code of our
  connector in various programming languages thanks to "model to text
  transformation" (M2T).
---

# Model specifications

We have defined a UML model to use the DFC ontology within an application.

## UML profile

### Stereotypes

<table><thead><tr><th width="226">Stereotype</th><th width="129">Range</th><th>Description</th></tr></thead><tbody><tr><td><code>property</code></td><td><code>Property</code></td><td>Marks a class variable.<br><br>Attributes: <code>getter</code>, <code>setter</code>.<br><br>The two attributes both target an operation of the model. The <code>getter</code> attribute should point to the operation responsible to get the value of the class variable. The <code>setter</code> attribute should point to the operation responsible to set the value of the class variable.<br></td></tr><tr><td><code>propertyMultiple</code></td><td><code>Property</code></td><td>Marks a model property used to define a class variable that is a collection.<br><br>Attributes: <code>getter</code>, <code>setter</code>, <code>adder</code>, <code>remover</code>.<br><br>The <code>getter</code> and <code>setter</code> are inherited from the <code>property</code> stereotype. The <code>adder</code> attribute should point to the operation responsible to add a value to the class variable. The <code>remover</code> attribute should point to the operation responsible to remove a value of the class variable.</td></tr><tr><td><code>constructor</code></td><td><code>Operation</code></td><td>Marks a constructor method.</td></tr><tr><td><code>initializer</code></td><td><code>Parameter</code></td><td>Marks a parameter used to initialize the value of a class variable. This stereotype should be applied on constructor parameters.<br><br>Attributes: <code>property</code>.<br><br>The <code>property</code> attribute should point to the class variable the parameter will initialize.</td></tr><tr><td><code>initializerParent</code></td><td><code>Parameter</code></td><td>Marks a parameter used to initialize the value of a parent class variable. This stereotype should be applied on  constructor parameters.<br><br>Attributes: <code>property</code>.<br><br>The <code>property</code> attribute should point to the class variable the parameter will initialize.</td></tr><tr><td><code>getter</code></td><td><code>Operation</code></td><td>Marks an operation used to get the value of a class variable.</td></tr><tr><td><code>setter</code></td><td><code>Operation</code></td><td>Marks an operation used to set the value of a class variable.</td></tr><tr><td><code>adder</code></td><td><code>Operation</code></td><td>Marks an operation used to add a value to a class variable.</td></tr><tr><td><code>remover</code></td><td><code>Operation</code></td><td>Marks an operation used to remove a value from a class variable.</td></tr><tr><td><code>semantic</code></td><td><code>Element</code></td><td>Marks an element mapped to a term of an ontology.<br><br>Attributes: <code>map</code>.<br><br>The <code>map</code> attribute should contain the URI of the mapped ontology term.</td></tr><tr><td><code>blankNode</code></td><td><code>Class</code> , <code>Interface</code></td><td>Marks a class or interface as a blank node.</td></tr><tr><td><code>skosBroader</code></td><td><code>Property</code></td><td>Marks.<br><br>Attributes: <code>map</code>.<br><br>The <code>map</code> attribute should contain the URI of the mapped SKOS term.</td></tr></tbody></table>

## Interfaces

To be written.

## Classes

To be written.
