# URI Redirection

We have introduced URI redirection to futureproof changes to how we store/serve DFC ontology & taxonomy files.

We are utilising the [w3id.org](https://w3id.org) permanent redirection service.

All DFC URI's can be accessed via the <https://w3id.org/dfc> route.

## Ontology URI's

To access the Ontology releases, use the following URI pattern(s):

### Business Ontology Files

All Ontology files are stored in the `/src/` folder of the [ontology repo](https://github.com/datafoodconsortium/ontology).

The latest version can be accessed via the URI: <https://w3id.org/dfc/ontology/src/DFC_BusinessOntology.owl>

We strongly recommend all applications use URI's for the specific version they are accessing. Those follow the pattern:

<https://w3id.org/dfc/ontology/{full-version-number, including v}/src/DFC_BusinessOntology.owl>

For example to access the v1.16.0 Ontology file, you would use: <https://w3id.org/dfc/ontology/v1.16.0/src/DFC_BusinessOntology.owl>

### Context Files

Context files are stored in the `/contexts/` folder of the ontology repo.

All context files include the version number in the filename, so MUST be accessed explicitly.

To access a context file, use the following URI pattern:

<https://w3id.org/dfc/ontology/contexts/context.{full-version-number, including v}.json>

It is recommended not to include the release version earlier in the URI pattern; this will access the specific context file in the latest ontology release.

## Taxonomy Files

All taxonomy files can be accessed via the following URI patterns:

### Latest version

To access the current version use the pattern:

<https://w3id.org/dfc/taxonomies/{required taxonomy file, including rdf/json suffix}>

### Specific version

To access a specific version use the pattern:

<https://w3id.org/dfc/taxonomies/{full-version-number, including v}/{required taxonomy file, including rdf/json suffix}>

So to access the v2.0.0 facets file in RDF format, you would specify:

<https://w3id.org/dfc/taxonomies/v2.0.0/facets.rdf>
