---
description: >-
  The connector is a library that can be used by applications as a facility to
  implement our standard. It provides especially the mapping between our
  ontology and the application programming language.
---

# Connector specifications

## Features

The connector MUST be able to...

### Export to JSON-LD

The connector MUST provide an `export` method.

### Import from JSON-LD

The connector MUST provide an `import` method.

Users MUST be able to import DFC taxonomies with this method.

### Getters and setters

### Copy constructors

### Named parameters

## Data model

The interfaces, classes and methods of the connector MUST be named as their respective element of the DFC UML model.

The connector source code and the related unit tests SHOULD be generated from the DFC UML model. The DFC provides a code generator that COULD be used to generate the connector source code.

### RDF datatype

The connector SHOULD use the Semantizer library which can be found here.

The factory method to create new objects must use a generated mapping. Mapping MUST be generated according to the DFC UML model (to ensure they are updated and synchronized with the model).

## Taxonomy loaders

The DFC standard provides SKOS taxonomies to express product types, different units or facets of a product. When passing a taxon as parameter of a method, the connector MUST enforce that the type of the taxon is exactly the one expected by the method. For instance a connector user MUST NOT be able to pass a product type instead of a currency.

To ensure taxon type correctness, the connector MUST use the programming language type checking capabilities, if available. Otherwise, the connector MUST check by itself if the passed type is correct. For this alternative solution, the connector COULD use macanisms like assertions or conditions.

The connector MUST provide a method to load the different SKOS taxonomies of the DFC. This method MUST accept as parameter the taxonomy data in JSON-LD format. It COULD also accept alternative formats like other RDF serializations (ex: RDF/XML or XML/TTL).

The SKOS taxonomy MUST be represented as a tree structure?

The taxonomy loader method SHOULD use the same JSON-LD parser as the one used in the Semantizer library. This recommendation SHOULD be followed to reduce the size of the connector library by reducing the number of its required dependencies.

## Error messages

<table><thead><tr><th width="141">Error code</th><th>Error description</th></tr></thead><tbody><tr><td></td><td></td></tr><tr><td></td><td></td></tr><tr><td></td><td></td></tr></tbody></table>

## Unit tests

Each concreate classe and each method MUST be tested.

The connector unit tests SHOULD be generated from the DFC UML model.

## List of methods

| Method name | Method Parameters |   |
| ----------- | ----------------- | - |
| import      |                   |   |
| export      |                   |   |
| shorten?    |                   |   |
