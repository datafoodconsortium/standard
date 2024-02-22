---
description: >-
  You will find here the list of procedures for contributing to the DFC
  Standard.
---

# 🚧 Procedures

We are using [Semantic Versioning](https://semver.org/).\


The process for proposing logical changes/enhancements to our repositories is as follows:

1. To propose a change/enhancement to an area of the Ontology, open a `Discussion` in the appropriate repository.
2. When agreed, the `Discussion` will be  closed and an `Issue` created, including Acceptance Criteria .
3. `Issues` can then be mapped to future releases in the roadmap.
4. [Required changes](#user-content-fn-1)[^1] are made and pushed to a dedicated branch/fork for that `Issue/Feature`
5. Changes are merged into `next{repositoryName}` via a `Pull Request` (with reviews as required by release procedures)

The process for physically releasing the changes is as follows:

1. Once all PR's are complete & changes merged into the dedicated `Release branch` of each repository
2. A release will be made from that branch, marked as pre-release & latest.
3. An announcement of the release & invitation to test are made via GitHub & Slack, including cut-off date
4. On the cut-off date, all changes form the `Release branch` are squashed & merged into `master`&#x20;
5. The pre-release tab is removed and a GitHub action is triggered to publish the new versions of the files for public use.
6. The `Release branch` is deleted

For detailed requirements for different release types... see detailed sub-sections:

## Update the ontology

Our ontology describes the data concepts & relationships between them that our standard uses to describe the short supply chain ecosystem. The ontology files are written in Web Ontology Language (OWL) and also expressed in RDF/XML.

See the [dedicated section](updates-to-the-ontology/).

## Update the taxonomies

The Taxonomy files hold the various (SKOS compliant) hierarchies & reference lists used within the ontology. The Taxon files are expressed in JSON-LD and RDF/XML.

See the [dedicated section](taxonomy-enrichment/).

[^1]: For details of the prerequisite steps for Taxonomy updates, see the [Taxonomy Updates section](taxonomy-enrichment/taxonomy-updates.md).
