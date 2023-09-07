---
description: >-
  This page define a Solid client-to-client standard to build DFC compliant
  Solid applications.
---

# 🚧 Solid specifications

## Conformance

Conformance requirements are expressed with a combination of descriptive assertions and RFC 2119 terminology. The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in the normative parts of this document are to be interpreted as described in RFC 2119. However, for readability, these words do not appear in all uppercase letters in this specification.

All of the text of this specification is normative except sections explicitly marked as non-normative, examples, and notes. [\[RFC2119\]](file:///home/malecoq/Projets/Mycelium/specs/index.html#biblio-rfc2119)

## Introduction

_This section is non-normative_.

## Namespaces

<table><thead><tr><th width="249">Prefix</th><th>Namespace</th><th>Description</th></tr></thead><tbody><tr><td><code>solid</code></td><td><a href="http://www.w3.org/ns/solid/terms">http://www.w3.org/ns/solid/terms#</a></td><td>Solid Terms.</td></tr><tr><td><code>dfc-b</code></td><td><a href="https://www.datafoodconsortium.org">https://www.datafoodconsortium.org#</a></td><td>Data Food Consortium business ontology.</td></tr><tr><td><code>dfc-f</code></td><td>TBD.</td><td>Data Food Consortium facets taxonomy.</td></tr><tr><td><code>dfc-m</code></td><td>TBD.</td><td>Data Food Consortium measures taxonomy.</td></tr><tr><td><code>dfc-pt</code></td><td>TBD.</td><td>DFC product types SKOS taxonomy.</td></tr><tr><td><code>dfc-t</code></td><td>TBD.</td><td>Data Food Consortium technical ontology.</td></tr></tbody></table>

## Storage

### Root container

The root folder MUST be defined using the `dfc-t:solidRootContainer` attribute. This attribute MUST be part of the user profile document.

If the `dfc-t:solidRootContainer` is not already defined in the user profile document, a DFC application SHOULD ask the user to select the container he/she wants to use as the root folder. A DFC application COULD propose to the user a default folder like a `datafoodconsortium` folder at the root of the user's storage.

### Reserved locations

In this section we assume to use the `dfc-t:solidRootContainer` as the root for all the following listed locations.

The following locations are reserved and SHOULD NOT be edited directly by end users:

* /addresses/
* /agents/
* /agents/customer-categories.ttl
* /agents/enterprises/
* /agents/persons/
* /agents/persons/index0.ttl
* /catalogs/
* /catalogs/\<catalog>/catalog.ttl
* /catalogs/\<catalog>/index0.ttl
* /catalogs/\<catalog>/catalog-items/
* /defined-products/
* /defined-products/functional-products/
* /defined-products/technical-products/
* /defined-products/supplied-products/
* /orders/
* /orders/index0.ttl
* /sale-sessions/

## Resource definition and location

In this section we assume to use the `dfc-t:solidRootContainer` as the root for all the following defined locations.

Resources must expressed using the DFC ontology and taxonomies.

An example dataset could be found at [https://github.com/datafoodconsortium/solid-dataset-example](https://github.com/datafoodconsortium/solid-dataset-example).

### Addresses

A `dfc-b:Address` MUST be stored in the `/addresses/` LDP container. One RDF resource MUST contain only one address.

<details>

<summary>Example of a <code>dfc-b:Address</code> in turtle</summary>

```turtle
@prefix dfc-b: <https://www.datafoodconsortium.org#>.

<>
    a dfc-b:Address;
    dfc-b:street "A street";
    dfc-b:city "A city name";
    dfc-b:postcode "A post code";
    dfc-b:country: "A country".
```

</details>

### Catalogs

A `dfc-b:Catalog` MUST be stored in a `catalog.ttl` file inside a folder in the `/catalogs/` LDP container.

<details>

<summary>Example of a <code>dfc-b:Catalog</code> in turtle</summary>

```turtle
@prefix dfc-b: <https://www.datafoodconsortium.org#>.

<>
    a dfc-b:Catalog;
    dfc-b:lists </catalogs/default/catalog-items/catalog-item1#catalogItem1>.
```

</details>

### Catalog items

A `dfc-b:CatalogItem` MUST be stored in the `/catalogs/<catalog>/items/` LDP container. All the related `dfc-b:Offer` MUST be stored in the catalog item document.

<details>

<summary>Example of a <code>dfc-b:CatalogItem</code> in turtle</summary>

```turtle
@prefix dfc-b: <https://www.datafoodconsortium.org#>.
@prefix dfc-pt: <https://www.datafoodconsortium.org/product-types#>.
@prefix dfc-f: <https://www.datafoodconsortium.org/facets#>.
@prefix dfc-m: <https://www.datafoodconsortium.org/measures#>.

<>
    a dfc-b:CatalogItem;
    dfc-b:references </defined-products/supplied-products/product1.ttl>;
    dfc-b:sku "catalog item gtin or sku";
    dfc-b:stockLimitation 24;
    dfc-b:offeredThrough 
        :offer1,
        :offer2;
    dfc-b:listedIn </catalogs/default/catalog.ttl>.

:offer1
    a dfc-b:Offer;
    dfc-b:offeredTo </agents/customer-categories.tll#default>;
    dfc-b:offers <./>;
    dfc-b:hasPrice :price1;
    dfc-b:stockLimitation 12;
    dfc-b:listedIn </sale-sessions/sale-session1.ttl>.

:offer2
    a dfc-b:Offer;
    dfc-b:offeredTo </agents/customer-categories.ttl#default>;
    dfc-b:offers <./>;
    dfc-b:hasPrice :price2;
    dfc-b:stockLimitation 10.

:price1
    a dfc-b:Price;
    dfc-b:value 3.4;
    dfc-b:VATrate 5.5;
    dfc-b:hasUnit dfc-m:Euro.

:price2
    a dfc-b:Price;
    dfc-b:value 5.4;
    dfc-b:VATrate 5.5;
    dfc-b:hasUnit dfc-m:Euro.
```

</details>

### Customer categories

All `dfc-b:CustomerCategory` MUST be defined in the `/agents/customerCategories.ttl` turtle file.

<details>

<summary>Example of a <code>dfc-b:CustomerCategory</code> in turtle</summary>

```turtle
@prefix : <#>.
@prefix dfc-b: <https://www.datafoodconsortium.org#>.

:default
    a dfc-b:CustomerCategory;
    dfc-b:description "Regular clients".
```

</details>

### Enterprises

A `dfc-b:Enterprise` MUST be stored in the `/agents/enterprises/` [\[LDP\]](file:///home/malecoq/Projets/Mycelium/specs/index.html#biblio-ldp) container. One RDF resource MUST contain only one enterprise.

<details>

<summary>Example of a <code>dfc-b:Enterprise</code> in turtle</summary>

{% code fullWidth="true" %}
```turtle
@prefix dfc-b: <https://www.datafoodconsortium.org#>.

<>
    a dfc-b:Enterprise;
    dfc-b:hasAddress </addresses/address1.ttl>;
    dfc-b:description "Enterprise description.";
    dfc-b:VATnumber 0123456789;
    dfc-b:defines </agents/customer-categories.ttl#default>;
    dfc-b:requests </defined-products/functional-products/product1.ttl>;
    dfc-b:proposes </defined-products/technical-products/product1.ttl>;
    dfc-b:supplies </defined-products/supplied-products/product1.ttl>;
    dfc-b:manages </catalogs/default/catalog-items/catalog-item1.ttl#catalogItem1>.
```
{% endcode %}

</details>

### Functional products

A `dfc-b:FunctionalProduct` MUST be stored in the `/products/functional/` LDP container. One RDF resource MUST contain only one functional product.

<details>

<summary>Example of a <code>dfc-b:FunctionalProduct</code> in turtle</summary>

```turtle
@prefix dfc-b: <https://www.datafoodconsortium.org#>.
@prefix dfc-pt: <https://www.datafoodconsortium.org/product-types#>.

<>
    a dfc-b:FunctionalProduct;
    dfc-b:hasType dfc-pt:tomato;
    dfc-b:description "Tomato";
    dfc-b:requestedBy </agents/enterprises/enterprise1.ttl>;
    dfc-b:satisfiedBy </defined-products/technical-products/product1.ttl>.
```

</details>

### Offers

A `dfc-b:Offer` MUST be stored as a resource of its parent `dfc-b:CatalogItem` document.

<details>

<summary>Example of a <code>dfc-b:Offer</code> in turtle</summary>

See the example in the [Catalog items](solid-specifications.md#catalog-items) section.

</details>

### Orders

A `dfc-b:Order` MUST be stored in the `/orders/` LDP container. One RDF resource MUST contain only one order.

<details>

<summary>Example of a <code>dfc-b:Order</code> in turtle</summary>

```turtle
@prefix : <#>.
@prefix dfc-b: <https://www.datafoodconsortium.org#>.

<>
    a dfc-b:Order;
    dfc-b:orderNumber "0001";
    dfc-b:date "December 26, 2022 15:02:18";
    dfc-b:orderedBy </agents/persons/person1.ttl>;
    dfc-b:hasPart
        :orderLine1, 
        :orderLine2.

:orderLine1
    a dfc-b:OrderLine;
    dfc-b:description "Catalog item 1 solded for 2.5€";
    dfc-b:partOf <>;
    dfc-b:concerns </catalogs/default/catalog-items/catalog-item1#offer1>;
    dfc-b:quantity 2.5.

:orderLine2
    a dfc-b:OrderLine;
    dfc-b:description "Catalog item solded for 1€";
    dfc-b:partOf <>;
    dfc-b:concerns </catalogs/default/catalog-items/catalog-item1#offer2>;
    dfc-b:quantity 1.0.
```

</details>

### Order lines

A `dfc-b:OrderLine` MUST be stored as a resource of its parent `dfc-b:Order` document.

<details>

<summary>Example of a <code>dfc-b:OrderLine</code> in turtle</summary>

See the example of the [Order section](solid-specifications.md#orders).

</details>

### Persons

When there are less than 100 persons

A `dfc-b:Person` MUST be stored in the `/agents/persons/` [\[LDP\]](file:///home/malecoq/Projets/Mycelium/specs/index.html#biblio-ldp) container. One RDF resource MUST contain only one person.

<details>

<summary>Example of a <code>dfc-b:Person</code> in turtle</summary>

```turtle
@prefix dfc-b: <https://www.datafoodconsortium.org#>.

<>
    a dfc-b:Person;
    dfc-b:firstName "A first name";
    dfc-b:familyName "A family name";
    dfc-b:hasAddress </addresses/address1.ttl>.
```

</details>

### Technical products

A `dfc-b:TechnicalProduct` MUST be stored in the `/products/technical/` LDP container. One RDF resource MUST contain only one technical product.

<details>

<summary>Example of a <code>dfc-b:TechnicalProduct</code> in turtle</summary>

```turtle
@prefix dfc-b: <https://www.datafoodconsortium.org#>.
@prefix dfc-pt: <https://www.datafoodconsortium.org/product-types#>.

<>
    a dfc-b:TechnicalProduct;
    dfc-b:hasType dfc-pt:tomato;
    dfc-b:description "Tomato";
    dfc-b:satisfy </defined-products/functional-products/product1.ttl>;
    dfc-b:industrializedBy </defined-products/supplied-products/product1.ttl>.
```

</details>

### Sale sessions

All `dfc-b:SaleSession` MUST be stored in the `/sale-sessions/` LDP container. One RDF resource MUST contain only one sale session.

<details>

<summary>Example of a <code>dfc-b:SaleSession</code> in turtle</summary>

```turtle
@prefix dfc-b: <https://www.datafoodconsortium.org#>.

<>
    a dfc-b:SaleSession;
    dfc-b:beginDate "December 26, 2022 14:00:00";
    dfc-b:endDate "December 26, 2022 18:00:00";
    dfc-b:lists </catalogs/default/catalog-items/catalog-item1.tll#offer1>.
```

</details>

### Supplied products

A `dfc-b:SuppliedProduct` MUST be stored in the `/products/supplied/` LDP container. One RDF resource MUST contain only one supplied product.

<details>

<summary>Example of a <code>dfc-b:SuppliedProduct</code> in turtle</summary>

```turtle
@prefix dfc-b: <https://www.datafoodconsortium.org#>.
@prefix dfc-pt: <https://www.datafoodconsortium.org/product-types#>.
@prefix dfc-f: <https://www.datafoodconsortium.org/facets#>.
@prefix dfc-m: <https://www.datafoodconsortium.org/measures#>.

<>
    a dfc-b:SuppliedProduct;
    dfc-b:hasType dfc-pt:hierloom-tomato;
    dfc-b:description "Hierloom tomato AB";
    dfc-b:hasCertification dfc-f:Organic-AB;
    dfc-b:hasQuantity [
        a dfc-b:QuantitativeValue;
        dfc-b:hasUnit dfc-m:Kilogram;
        dfc-b:value 1
    ];
    dfc-b:image <./product1.jpg>;
    dfc-b:industrializedBy </defined-products/technical-products/product1.ttl>.
```

</details>

## Indexing

### Orders

All `dfc-b:Order` MUST be indexed in the `index0.ttl` file at the root of the `/orders/` container.

<details>

<summary>Example of a <code>dfc-b:Order</code> index in turtle</summary>

```turtle
@prefix : <#>.
@prefix dfc-b: <https://www.datafoodconsortium.org#>.
@prefix dfc-m: <https://www.datafoodconsortium.org/measures#>.

:1
    a dfc-b:OrderIndexEntry;
    dfc-b:indexes </orders/order1.ttl>;
    dfc-b:orderNumber "0001";
    dfc-b:date "December 26, 2022 15:02:18";
    dfc-b:orderedBy </agents/persons/person1.ttl>;
    dfc-b:hasPrice :p1;
    dfc-b:hasParts 2.

:p1
    a dfc-b:Price;
    dfc-b:value 12.20;
    dfc-b:VATrate 5.5;
    dfc-b:hasUnit dfc-m:Euro.

```

</details>

### Persons

#### Indexing the first letter of the family name

All `dfc-b:Person` MUST be indexed in the `/agents/persons/index0.ttl` file.  This index MUST contain triples of the form `#anchor dfc-b:references <person URI>` .

The `#anchor` MUST be named by the first letter of the family name of the person (value of the `dfc-b:familyName` property).

<details>

<summary>Example of a <code>/agents/persons/index0.ttl</code> file</summary>

```turtle
@prefix : <#>.
@prefix dfc-b: <https://www.datafoodconsortium.org#>.

# This is indexing persons with a family name starting with the letter "a".
:a dfc-b:references 
    </agents/persons/person32.ttl>,
    </agents/persons/person12.ttl>.

# This is indexing persons with a family name starting with the letter "b".
:b dfc-b:references  
    </agents/persons/person56.ttl>,
    </agents/persons/person78.ttl>.

# This is indexing persons with a family name starting with the letter "z".    
:z dfc-b:references  
    </agents/persons/person2.ttl>,
    </agents/persons/person63.ttl>.
```

</details>

### Product types

All `dfc-b:CatalogItem` MUST be indexed by their `dfc-b:hasType` property (product type) in the `index0.ttl` file stored at the root of a `dfc-b:Catalog`.&#x20;

This index is currently a combination of the vocabularies from TypeIndex and DFC.&#x20;

<details>

<summary>Example of a product type index in turtle</summary>

```turtle
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
@prefix dfc-b: <https://www.datafoodconsortium.org#>.
@prefix dfc-pt: <https://www.datafoodconsortium.org/productTypes#>.

<#ab09fd> a solid:TypeRegistration;
    dfc-b:hasType <dfc-pt:artichoke>;
    solid:instance <./items/artichoke.ttl>.
```

</details>

### TypeIndex

A DFC Solid compliant application MUST advertise its containers in the public TypeIndex.

<details>

<summary>Example of the registration of DFC containers in a turtle TypeIndex</summary>

```turtle
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
@prefix dfc-b: <https://www.datafoodconsortium.org#>.

<>
    a solid:TypeIndex ;
    a solid:ListedDocument.

<#ab09fd> a solid:TypeRegistration;
    solid:forClass <dfc-b:Enterprise>;
    solid:instanceContainer </agents/enterprises/>.
```

</details>

## Revisions and automated backups

Should we be able to automatically backup the storage?

## Encryption

## Error codes

## Notifications

## Access rights

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
