# Semantic specifications

Before diving into the technical aspect of the standard, we will focus a bit on the semantic aspect of it as it will help to get a better overview of the core objects and their interactions.

## Why semantic?

The DFC Consortium chose to build the standard using semantic standards. In terms of interoperability, this approach makes total sense and allows us to use robust technologies to complete our objectives. It is an important decision, it will help us to improve the standard in the future, and also to give us more possibility in interoperability automation.

You can find more information about this choice in this [appendix](https://github.com/datafoodconsortium/standarddocumentation/tree/e4ac9c376381d4593ae7677f61ae06e3a252ad97/appendixes/technical-decisions/service-standard.html#de-facto-standards-or-semantic-web-).

## Ontologies

Semantic specifications are expressed with files called onotologies. It is using Web Ontology Language \([OWL](https://fr.wikipedia.org/wiki/Web_Ontology_Language)\), based on other standards such as XML and RDF.

We are actually using 3 main files \(and a fourth one to synthesise the three\):

* [Product ontology](product-ontology.md)
* [Business ontology](business-ontology.md)
* [Technical ontology](technical-ontology.md)
* Full ontology published on [LOV](https://lov.linkeddata.es/dataset/lov/vocabs/dfc)

## Access to code source

These ontologies are publicly available and you can find the source files on:

* [Github](https://github.com/datafoodconsortium/ontology) which is used in order to maintain and share the source files.
* [Linked Open Vocabularies](https://lov.linkeddata.es/dataset/lov/vocabs/dfc) which is a well known library helping our standard to be discovered by other people using and reusing semantic ontologies.



