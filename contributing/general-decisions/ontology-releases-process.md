---
description: This page lists what to be done when releasing a new ontology version.
---

# Ontology releases process

* [ ] Create or update definitions for new or existing concepts/relations
* [ ] Update metadata tags:&#x20;
  * [ ] ontology  version
  * [ ] required: taxonomy file versions &#x20;
* [ ] Update the Changelog file:
  * [ ] Move the content from the unreleased section into a section named like "M.m.p - YYYY-MM-DD" where M.m.p corresponds to the version of the release and YYYY-MM-DD corresponds to the date at which the release will be merged into the master branch.
  * [ ] Ensure to follow the principles from [https://keepachangelog.com/](https://keepachangelog.com/)
  * [ ] Write a little resume at the top of the release section to indicate what the release does.
  * [ ] Update the links section at the end of the Changelog ensuring  the "unreleased" link compares the latest version to master and the link of the release compares it to the previous release.
* [ ] Create `.owl` and `.rdf` files
* [ ] Create versioned context file&#x20;
* [ ] Update the schema/diagram in the repo and update it in the Gitbook
* [ ] Update the Widoco documentation (regenerate)
* [ ] Create an issue to update UML model and add it to in the "Todo" section of the ontology task board
