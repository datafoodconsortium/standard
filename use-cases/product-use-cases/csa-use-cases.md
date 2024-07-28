---
description: Use Cases that apply to CSA's
---

# CSA Use Cases

Community Supported Agriculture operations provide risk-sharing between Producer and Eater. It is generally characterized by contracts for recurring delivery of produce on a regular (weekly, fortnightly or monthly basis). Often the customer will subscribe to a share of the harvest, with the exact content & quantity set by the Producer at each delivery. This therefore requires a recurring contract to represent the subscription and a generic Product to represent the Share (which can them be Transformed into the exact contents by the Producer).

*



1. As a CSA Producer I need to represent a recurring subscription of a consumer so that I know how many products to harvest.
   1. The Onotology will include a `Contract` class, that has a `startDate`, `frequency,`  `endDate` (optional) & `NoOfOccurences` (optional)`.`&#x20;
   2. `Contracts` will be linked to `Agents` (both a managing & contracted `Agent`) and one or more`CataologItems`.&#x20;
      1. `Contracts` will generate `Orders`.\

2. As a CSA Producer I need to represent the content of each Share (which will vary week on week) so that I can browse the history
   1. Share `Products` will be mapped via `TransformationFlows` to the individual `Products` they contain.
   2. A new `Product` must be created for each recurrence of the Share.\

3. As a CSA Producer I need to represent a recurring distribution session to allow planning.
   1. A `Contract` will be linked to mutliple `SaleSession` detailing the recurring distributions of the Share `Product`\






*
