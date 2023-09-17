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

In the [user's preferences document](https://solid.github.io/webid-profile/#private-preferences), an [application](solid-client-protocol.md#application) COULD find a link to a DFC dedicated `solid:TypeIndex` containing only registrations related to DFC resources. This type index can be used to reduce the parsing of an entire `solid:TypeIndex` which can contain a huge amount of registrations. This type index MUST be indicated with triples of the form `?preference a dfc-t:typeIndex; solid:instance ?typeIndexFile.` as shown in the following example:

<details>

<summary>Example of a user's preferences file in turtle</summary>

```turtle
@base <https://example.pod/username>.
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
@prefix dfc-t: <TDB>.

<#zb78gj> a dfc-t:typeIndex; 
    solid:instance </datafoodconsortium/typeIndex.ttl>.
```

</details>

## Storage

Resources location MUST be defined in a `solid:TypeIndex`. An [application](solid-client-protocol.md#application) MUST let the user choose where he/she want to store data and write the location in the type index. See the [TypeIndex section](solid-client-protocol.md#typeindex) for more details.&#x20;

Resources that share the same `rdf:type` related to the DFC ontology SHOULD be stored in a same LDP container. For instance, all resource with a `rdf:type` being `dfc-b:Enterprise` SHOULD be stored in one container like `/enterprises/`.

An [application](solid-client-protocol.md#application) SHOULD propose a default location for every kind of DFC resources. If no DFC resources are found on the user's POD, an [application](solid-client-protocol.md#application) SHOULD propose a default root location like `/datafoodconsortium/` which contain a default tree structure like the following one:

<details>

<summary>Example of a default tree structure</summary>

* datafoodconsortium/
  * addresses/
  * agents/
    * enterprises/
    * persons/
  * catalogs/
    * default
      * catalog.ttl
      * index0.ttl
  * catalog-items/
  * defined-products/
    * functional-products/
    * supplied-products/
    * technical-products/
  * orders/
    * index0.ttl
  * sale-sessions/

</details>

### Resource definition

Resources MUST be expressed using the DFC ontology and taxonomies.

An example dataset could be found at [https://github.com/datafoodconsortium/solid-dataset-example](https://github.com/datafoodconsortium/solid-dataset-example).

All the following resource SHOULD be stored in dedicated LDP containers.

#### Addresses

All `dfc-b:Address` SHOULD be stored in a dedicated LDP container. One address RDF resource MUST contain only one address.

{% tabs %}
{% tab title="Shape of a dfc-b:Address" %}
```turtle
@prefix sh: <http://www.w3.org/ns/shacl#>.
@prefix xsd: <http://www.w3.org/2001/XMLSchema#>.
@prefix dfc-b: <https://www.datafoodconsortium.org#>.

<>
	a sh:NodeShape;
	sh:targetClass dfc-b:Address;
	sh:property [
		sh:path dfc-b:street;
		sh:maxCount 1;
		sh:datatype xsd:string;
	];
	sh:property [
		sh:path dfc-b:postcode;
		sh:maxCount 1;
		sh:datatype xsd:string;
	];
	sh:property [
		sh:path dfc-b:city;
		sh:maxCount 1;
		sh:datatype xsd:string;
	];
	sh:property [
		sh:path dfc-b:country;
		sh:maxCount 1;
		sh:datatype xsd:string;
	];
	sh:closed true;
	sh:ignoredProperties ( rdf:type ).
```
{% endtab %}

{% tab title="Example of a dfc-b:Address in turtle" %}
```turtle
@prefix dfc-b: <https://www.datafoodconsortium.org#>.

<>
    a dfc-b:Address;
    dfc-b:street "A street";
    dfc-b:city "A city name";
    dfc-b:postcode "A post code";
    dfc-b:country: "A country".
```
{% endtab %}
{% endtabs %}

#### Catalogs

All `dfc-b:Catalog` MUST be stored in a file inside the LDP container dedicated to this catalog. This file SHOULD be named `catalog.ttl`.

<details>

<summary>Example of a <code>dfc-b:Catalog</code> in turtle</summary>

```turtle
@prefix dfc-b: <https://www.datafoodconsortium.org#>.

<>
    a dfc-b:Catalog;
    dfc-b:lists </catalogs/default/catalog-items/catalog-item1#catalogItem1>.
```

</details>

#### Catalog items

All `dfc-b:CatalogItem` SHOULD be stored in a dedicated LDP container. All the related `dfc-b:Offer` MUST be stored in the catalog item document.

When the user want to edit a `dfc-b:CatalogItem`, an [application](solid-client-protocol.md#application) MUST ask the user if he/she want to edit all the occurrences of the catalog item (i.e. in all the other catalogs) or if he/she want to edit the catalog item only for one or several catalogs (it might be the catalog being edited). If the user want to edit the catalog item for one or several catalogs only, the catalog item MUST be copied to a new resource.

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

#### Customer categories

All `dfc-b:CustomerCategory` MUST be defined in a file like `/agents/customerCategories.ttl`.

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

#### Enterprises

All `dfc-b:Enterprise` SHOULD be stored in a dedicated [\[LDP\]](file:///home/malecoq/Projets/Mycelium/specs/index.html#biblio-ldp) container. One enterprise RDF resource MUST contain only one enterprise.

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

#### Functional products

All `dfc-b:FunctionalProduct` SHOULD be stored in a dedicated LDP container. One functional product RDF resource MUST contain only one functional product.

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

#### Offers

All `dfc-b:Offer` MUST be stored as a resource of its parent `dfc-b:CatalogItem` document.

<details>

<summary>Example of a <code>dfc-b:Offer</code> in turtle</summary>

See the example in the [Catalog items](solid-client-protocol.md#catalog-items) section.

</details>

#### Orders

All `dfc-b:Order` SHOULD be stored in a dedicated LDP container. One order RDF resource MUST contain only one order.

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

#### Order lines

All `dfc-b:OrderLine` MUST be stored as a resource of its parent `dfc-b:Order` document.

<details>

<summary>Example of a <code>dfc-b:OrderLine</code> in turtle</summary>

See the example of the [Order section](solid-client-protocol.md#orders).

</details>

#### Persons

All `dfc-b:Person` SHOULD be stored in a dedicated [\[LDP\]](file:///home/malecoq/Projets/Mycelium/specs/index.html#biblio-ldp) container. One person RDF resource MUST contain only one person.

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

#### Technical products

All `dfc-b:TechnicalProduct` SHOULD be stored in a dedicated LDP container. One technical product RDF resource MUST contain only one technical product.

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

#### Sale sessions

All `dfc-b:SaleSession` SHOULD be stored in a dedicated LDP container. One sale session RDF resource MUST contain only one sale session.

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

#### Supplied products

All `dfc-b:SuppliedProduct` SHOULD be stored in a dedicated LDP container. One supplied product RDF resource MUST contain only one supplied product.

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

Any DFC application need to be able to find information quickly and indexing is a way to do it in Solid. This DFC standard provides several indexes to find some useful piece of data. These indexes are updated when data changes (i.e. data is added, edited or deleted).

### Orders

#### Indexing the year and month of orders

Thins index can be used to retrieve orders for a certain period of time like a year or a month of a year.

All `dfc-b:Order` MUST be indexed by their year and month in the `index0.ttl` file at the root of the `/orders/` container.

<details>

<summary>Example of the <code>/orders/index0.ttl</code> index file in turtle</summary>

```turtle
@base <https://example.pod/username/datafoodconsortium>.
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
@prefix dfc-b: <https://www.datafoodconsortium.org#>.
@prefix dfc-m: <https://www.datafoodconsortium.org/measures#>.
@prefix index: <TBD>.

<>
    a index:Index;
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

Such an index can be used to display quickly a person when the user types the beginning of a family name.

All `dfc-b:Person` MUST be indexed in the `/agents/persons/index0.ttl` file. This index MUST contain a registration for each first symbol used in the family name of all the persons.

<details>

<summary>Example for the <code>/agents/persons/index0.ttl</code> file</summary>

```turtle
@base <https://example.pod/username/datafoodconsortium>.
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
@prefix dfc-b: <https://www.datafoodconsortium.org#>.
@prefix index: <TDB>.

<>
    a index:Index;
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

This index can be used to quickly find all the catalog items referencing a product of a given type. For instance, it can be used to find all the tomatoes listed in a catalog.

All `dfc-b:CatalogItem` MUST be indexed in the `index0.ttl` file at the root of a `dfc-b:Catalog`. It MUST contain registrations mentionning product type.

<details>

<summary>Example for the <code>/catalogs/default/index0.ttl</code> file in turtle</summary>

```turtle
@base <https://example.pod/username/datafoodconsortium>.
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
@prefix dfc-pt: <https://www.datafoodconsortium.org/productTypes#>.
@prefix index: <TBD>.

<>
    a index:Index;
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

A DFC Solid compliant application MUST advertise its containers and all the indexes in the public TypeIndex.

<details>

<summary>Example of a DFC app public TypeIndex in turtle</summary>

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
    
<#indexes> a solid:TypeRegistration;
    solid:forClass index:Index;
    solid:instance 
        </agents/persons/index0.ttl>,
        </catalogs/default/index0.ttl>,
        </orders/index0.ttl>.
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
