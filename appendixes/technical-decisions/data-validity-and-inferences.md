# Data validity and inferences

What we do when data is not __completed__ = with the correct backlinks or inverse links (e.g. `CatalogItem:references` / `DefinedProduct:referencedBy`).

**Is it the responsibility of the sender to link data (inferences) or the receiver?**

It isn't reasonable to expect all receiving applications to implement an Ontology Reasoner/Inferrer, so we need the sender to provide all the links (including inverse links).

This gives us the most robust implementation of the standard: allowing an application to start from any point and traverse the ontology data in different directions, which supports the widest range of the use cases.

## Decision

* When transmitting data via the DFC standard any and all inverse relations (that exist in the ontology) MUST be provided for each transferred property.
