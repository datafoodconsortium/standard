---
description: >-
  This page define a Solid client-to-client standard to build DFC compliant
  Solid applications.
---

# 🚧 Solid client protocol

## Conformance

Conformance requirements are expressed with a combination of descriptive assertions and RFC 2119 terminology. The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in the normative parts of this document are to be interpreted as described in RFC 2119. However, for readability, these words do not appear in all uppercase letters in this specification.

All of the text of this specification is normative except sections explicitly marked as non-normative, examples, and notes. [\[RFC2119\]](file:///home/malecoq/Projets/Mycelium/specs/index.html#biblio-rfc2119)

## Introduction

_This section is non-normative_.

## Terminology

_This section is non-normative._

The Solid Client Protocol specification defines the following terms. These terms are referenced throughout this specification.

#### application

Refers to an application that follows this [client protocol](solid-client-protocol.md#client-protocol).

#### client protocol

The DFC Solid Cient Protocol refers to this specification: the document you are currently reading.

## Namespaces

<table><thead><tr><th width="249">Prefix</th><th>Namespace</th><th>Description</th></tr></thead><tbody><tr><td><code>solid</code></td><td><a href="http://www.w3.org/ns/solid/terms">http://www.w3.org/ns/solid/terms#</a></td><td>Solid Terms.</td></tr><tr><td><code>dfc-b</code></td><td><a href="https://www.datafoodconsortium.org">https://www.datafoodconsortium.org#</a></td><td>Data Food Consortium business ontology.</td></tr><tr><td><code>dfc-f</code></td><td>TBD.</td><td>Data Food Consortium facets taxonomy.</td></tr><tr><td><code>dfc-m</code></td><td>TBD.</td><td>Data Food Consortium measures taxonomy.</td></tr><tr><td><code>dfc-pt</code></td><td>TBD.</td><td>DFC product types SKOS taxonomy.</td></tr><tr><td><code>dfc-t</code></td><td>TBD.</td><td>Data Food Consortium technical ontology.</td></tr></tbody></table>

## Configuration

An [application](solid-client-protocol.md#application) SHOULD save its settings in the [user's preferences document](https://solid.github.io/webid-profile/#private-preferences).

In the [user's preferences document](https://solid.github.io/webid-profile/#private-preferences), an [application](solid-client-protocol.md#application) COULD find a link to one or several DFC dedicated `solid:TypeIndex` containing only registrations related to DFC resources. These type indexes can be used to reduce the parsing of general type indexes which can contain a huge amount of registrations. When existing, these DFC specific type indexes MUST be indicated with triples of the form `?preference a dfc-t:typeIndex; solid:instance ?typeIndexFile.` as shown in the following example:

<details>

<summary>Example of a user's preferences file in turtle</summary>

```turtle
@base <https://example.pod/username>.
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
@prefix dfc-t: <TDB>.

<#types-indexes> a dfc-t:typeIndex; 
    solid:instance </datafoodconsortium/publicTypeIndex.ttl>,
        </datafoodconsortium/privateTypeIndex.ttl>.
```

</details>

## Storage and resources

Resources MUST be expressed using the [DFC ontology](semantic-specifications/) version 1.8 or later and taxonomies. Arities defined by the [DFC ontology](semantic-specifications/) MUST be respected. An example dataset could be found at [https://github.com/datafoodconsortium/solid-dataset-example](https://github.com/datafoodconsortium/solid-dataset-example).

An [application](solid-client-protocol.md#application) MUST let users to choose where they want to store data on their POD(s). Resources location MUST be defined in one or several `solid:TypeIndex`. Each used DFC resource or container MUST be registered in these type indexes. See the [TypeIndex section](solid-client-protocol.md#typeindex) for more details.

When users don't know or don't want to choose where to save their data, an [application](solid-client-protocol.md#application) SHOULD propose a default location like `/datafoodconsortium/` as the root LDP container for DFC data. Following the tree structure defined in the [example dataset](https://github.com/datafoodconsortium/solid-dataset-example) is RECOMMENDED.

## Indexing

[Applications](solid-client-protocol.md#application) MUST fill and keep updated several indexes in order to allow quick searching of piece of information. All the following indexes MUST be supported by [`applications`](solid-client-protocol.md#application) and MUST be updated whenever a DFC data is created, edited or deleted.

### Orders

#### Indexing the year and month of orders

This index MUST be registered in one or several type indexes for the class `dfc-t:OrderIndex`. All `dfc-b:Order` SHOULD be indexed.

Such an index can be used to retrieve orders for a certain period of time like a year or a month of a year.

<details>

<summary>Example of a <code>dfc-t:OrderIndex</code> in turtle</summary>

```turtle
@base <https://example.pod/username/datafoodconsortium>.
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
@prefix dfc-b: <https://www.datafoodconsortium.org#>.
@prefix dfc-m: <TBD>.
@prefix index: <TBD>.
@prefix dfc-t: <TBD>.

<>
    a index:Index, dfc-t:OrderIndex;
    a solid:ListedDocument;
    
<#1>
    a index:Registration;
    index:year 2022;
    index:month 12;
    solid:instance </orders/order2.ttl>.
    
<#2>
    a index:Registration;
    index:year 2022;
    index:month 11;
    solid:instance </orders/order1.ttl>.
```

</details>

### Persons

#### Indexing the first letter of the family name

This index MUST be registered in one or several type indexes for the class `dfc-t:PersonIndex`. All `dfc-b:Person` SHOULD be indexed. It MUST contain a registration for each first symbol used in the family name of all the persons.

Such an index can be used to display quickly a person when the user types the beginning of a family name.

<details>

<summary>Example of a <code>dfc-t:PersonIndex</code> in turtle</summary>

```turtle
@base <https://example.pod/username/datafoodconsortium>.
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
@prefix dfc-b: <https://www.datafoodconsortium.org#>.
@prefix index: <TDB>.
@prefix dfc-t: <TBD>.

<>
    a index:Index, dfc-t:PersonIndex;
    a solid:ListedDocument;
    index:forProperty dfc-b:familyName.

# This is indexing persons with a family name starting with the letter "a".
<#ab09fd> a index:Registration;
    index:strstarts "a";
    solid:instance  </agents/persons/person32.ttl>, </agents/persons/person12.ttl>.

# This is indexing persons with a family name starting with the letter "b".
<#zx45yh> a index:Registration;
    index:strstarts "b";
    solid:instance  </agents/persons/person56.ttl>, </agents/persons/person78.ttl>.

# This is indexing persons with a family name starting with the letter "z".
<#sk17vb> a index:Registration;
    index:strstarts "z";
    solid:instance  </agents/persons/person2.ttl>, </agents/persons/person63.ttl>.
```

</details>

### Product types

#### Indexing the product types contained in a catalog

This index MUST be registered in one or several type indexes for the class `dfc-t:ProductTypeIndex`. All `dfc-b:CatalogItem` SHOULD be indexed. It MUST contain registrations mentionning product type.

Such an index can be used to quickly find all the catalog items referencing a product of a given type. For instance, it can be used to find all the tomatoes listed in a catalog.

<details>

<summary>Example of a <code>dfc-t:ProductTypeIndex</code> in turtle</summary>

```turtle
@base <https://example.pod/username/datafoodconsortium>.
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
@prefix dfc-pt: <https://www.datafoodconsortium.org/productTypes#>.
@prefix index: <TBD>.
@prefix dfc-t: <TBD>.

<>
    a index:Index, dfc-t:ProductTypeIndex;
    a solid:ListedDocument.

<#ab09fd> a index:Registration;
    solid:instance </catalogs/default/catalog-items/artichoke.ttl>;
    index:mentions dfc-pt:artichoke.
    
<#tf76pl> a index:Registration;
    solid:instance 
        </catalogs/default/catalog-items/round-tomato.ttl>,
        </catalogs/default/catalog-items/cherry-tomato.ttl>;
    index:mentions dfc-pt:tomato.
```

</details>

### TypeIndex

[Applications](solid-client-protocol.md#application) MUST advertise their DFC resources, containers and indexes in one or several `solid:TypeIndex`.

<details>

<summary>Example of a type index in turtle</summary>

```turtle
@base <https://example.pod/username/datafoodconsortium>.
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
@prefix dfc-b: <https://www.datafoodconsortium.org#>.
@prefix index: <TBD>.

<>
    a solid:TypeIndex;
    a solid:ListedDocument.

<#addresses> a solid:TypeRegistration;
    solid:forClass dfc-b:Address;
    solid:instanceContainer </addresses/>.
    
<#persons> a solid:TypeRegistration;
    solid:forClass dfc-b:Person;
    solid:instanceContainer </agents/person/>.
    
<#enterprises> a solid:TypeRegistration;
    solid:forClass dfc-b:Enterprise;
    solid:instanceContainer </agents/enterprises/>.
    
<#customer-categories> a solid:TypeRegistration;
    solid:forClass dfc-b:CustomerCategory;
    solid:instance </agents/customer-categories.ttl>.
    
<#catalogs> a solid:TypeRegistration;
    solid:forClass dfc-b:Catalog;
    solid:instanceContainer </catalogs/>.
    
<#catalog-items> a solid:TypeRegistration;
    solid:forClass dfc-b:CatalogItem;
    solid:instanceContainer </catalog-items/>.
    
<#functional-products> a solid:TypeRegistration;
    solid:forClass dfc-b:FunctionalProduct;
    solid:instanceContainer </defined-products/functional-products/>.
    
<#supplied-products> a solid:TypeRegistration;
    solid:forClass dfc-b:SuppliedProduct;
    solid:instanceContainer </defined-products/supplied-products/>.
    
<#technical-products> a solid:TypeRegistration;
    solid:forClass dfc-b:TechnicalProduct;
    solid:instanceContainer </defined-products/technical-products/>.
    
<#orders> a solid:TypeRegistration;
    solid:forClass dfc-b:Order;
    solid:instanceContainer </orders/>.
    
<#sale-sessions> a solid:TypeRegistration;
    solid:forClass dfc-b:SaleSession;
    solid:instanceContainer </sale-sessions/>.
    
<#person-index> a solid:TypeRegistration;
    solid:forClass index:PersonIndex;
    solid:instance </agents/persons/index0.ttl>.
    
<#product-type-index> a solid:TypeRegistration;
    solid:forClass index:ProductTypeIndex;
    solid:instance </catalogs/default/index0.ttl>.

<#order-index> a solid:TypeRegistration;
    solid:forClass index:OrderIndex;
    solid:instance </orders/index0.ttl>.
```

</details>

## Paging and sorting

This section defines how data is paged and sorted.

## Access rights

## Notifications

## Error codes

## Revisions and automated backups

Should we be able to automatically backup the storage?

## Encryption

## Appendixes

### Resources locations index

| Resource type             | Resource location in the root container  |
| ------------------------- | ---------------------------------------- |
| `dfc-b:Address`           | `/addresses/`                            |
| `dfc-b:Catalog`           | `/catalogs/`                             |
| `dfc-b:CatalogItem`       | `/catalogs/<catalog>/catalog-items/`     |
| `dfc-b:CustomerCategory`  | `/agents/customer-categories.ttl`        |
| `dfc-b:Enterprise`        | `/agents/enterprises/`                   |
| `dfc-b:FunctionalProduct` | `/defined-products/functional-products/` |
| `dfc-b:Offer`             | `/catalogs/<catalog>/items/<item>#`      |
| `dfc-b:Order`             | `/orders/`                               |
| `dfc-b:OrderLine`         | `/orders/<order>#<order-line>`           |
| `dfc-b:Person`            | `/agents/persons/`                       |
| `dfc-b:SaleSession`       | `/sale-sessions/`                        |
| `dfc-b:SuppliedProduct`   | `/defined-products/supplied-products/`   |
| `dfc-b:TechnicalProduct`  | `/defined-products/technical-products/`  |
