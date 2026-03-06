# Introduction

*This section is non-normative.*

In order to interoperate, DFC platforms need to discover and advertise data, respectively from and to other platforms of the DFC ecosystem. For instance when a user creates a new product on a platform "A", the platform "A" needs to know where to create that product on the user's other platforms. To do so the DFC protocol defines entry points who take the form of URIs called [WebID](#webids). Two kinds of WebIDs are defined:

- A platform's WebID leading to platform-wide information like the services and the indexes it exposes;
- A user's WebID leading to information related to a particular user like the organization(s) he is affiliated to.

On each platform, each user has his own WebID. Therefore a mechanism to find equivalent users WebID between platforms is needed. This specification defines the platform [Identity Service](#platform-identity-service) that a platform can query to obtain a WebID in exchange of an OIDC token.

A platform can provide optional [specific indexes]() to help to discover some data at the platform or organization level. At the organization level an index can allow to get all orders for a particular time interval for instance. At the platform level indexes can be defined to advertise which kinds of product are sold by producers or to find organizations by name.

# Namespaces

| Prefix | IRI | Description |
| ------ | --- | ----------- |
| `dc` | http://purl.org/dc/terms/ | [[DC-TERMS](#dc-terms)] |
| `dfc-b` |   | DFC business ontology. |
| `dfc-t` |   | DFC technical ontology. |
| `foaf` | http://xmlns.com/foaf/0.1/ | Friend of a Friend ontology. |
| `idx` | https://ns.inria.fr/idx/terms# | Shacl shape-based indexing ontology. |
| `ldp` | http://www.w3.org/ns/ldp# |  |
| `pim` | http://www.w3.org/ns/pim/space# |  |
| `posix` | http://www.w3.org/ns/posix/stat | POSIX File Status |
| `rdfs` | http://www.w3.org/2000/01/rdf-schema# |  |
| `sh` | http://www.w3.org/ns/shacl# |  |
| `solid` | http://www.w3.org/ns/solid/terms# | [Social Linked Data](https://solidproject.org/) terms. |
| `xsd` | http://www.w3.org/2001/XMLSchema# | |

# Conformance

All assertions, diagrams, examples, and notes are non-normative, as are all sections explicitly marked non-normative. Everything else is normative.
The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” are to be interpreted as described in [BCP 14](https://tools.ietf.org/html/bcp14) [[RFC2119](#rfc2119)] [[RFC8174](#rfc8174)] when, and only when, they appear in all capitals, as shown here.

# Resource Containment

Like the Solid protocol ([source](https://solidproject.org/TR/protocol#resource-containment)), the DFC protocol "*has the notion of containers to represent a collection of linked resources to help with resource discovery and lifecycle management*". The representation and behavior of containers in DFC corresponds to [LDP basic container](https://www.w3.org/TR/ldp/#ldpbc) [[LDP](#ldp)] and MUST be supported by DFC platforms.

Like the Solid protocol, the DFC protocol has the notion of [hierarchical resource containment](https://solidproject.org/TR/protocol#server-hierarchical-containment) [[SOLID](#solid)]. The Solid protocol states:

> There is a 1-1 correspondence between containment triples and relative reference within the path name hierarchy. [[Source](https://github.com/solid/specification/issues/98#issuecomment-547506617)]. It follows that all resources are discoverable from a container and that it is not possible to create orphan resources. [[Source](https://github.com/solid/specification/issues/97#issuecomment-547459396)]

Accordingly to this statement, if the resource `resource` is contained in the container `https://platform.ex/container/`, this resource URI MUST be `https://platform.ex/container/resource`. A same URI can not be listed by two different containers in the scope of this specification. Platforms are encouraged to follow these requirements for the containers they define outside the scope of this specification.

Container descriptions are not limited to containment triples. To further support client navigation and application interaction, servers can include resource metadata about contained resources as part of the container description, as described in the section [4.2.1 Contained Resource Metadata](https://solidproject.org/TR/protocol#contained-resource-metadata) of the Solid protocol [[SOLID](#solid)].

The below example shows the retrieving of a LDP container.

The request to retrieve the representation of the container hosted at `https://platform.ex/container/` :

```http
GET /container/ HTTP/1.1
Host: platform.ex
Accept: application/ld+json
```

The response:

```http
HTTP/1.1 200 OK 
Content-Type: application/ld+json; charset=UTF-8
Link: <http://www.w3.org/ns/ldp#BasicContainer>; rel="type", <http://www.w3.org/ns/ldp#Resource>; rel="type"
Allow: OPTIONS,HEAD,GET,POST,PUT,PATCH
Accept-Post: application/ld+json, image/bmp, image/jpeg

{
  "@context": {
    "ldp": "http://www.w3.org/ns/ldp#",
    "dc": "http://purl.org/dc/terms/",
    "posix": "http://www.w3.org/ns/posix/stat#",
    "xsd": "http://www.w3.org/2001/XMLSchema#"
  },
  "@graph": [
    {
      "@id": "https://platform.ex/container/",
      "@type": ["ldp:Container", "ldp:BasicContainer", "ldp:Resource"],
      "dc:modified": {
        "@type": "xsd:dateTime",
        "@value": "2025-10-21T10:14:27.000Z"
      },
      "posix:mtime": 1761041667,
      "ldp:contains": ["https://platform.ex/container/resource"]
    },
    {
      "@id": "https://platform.ex/container/resource",
      "@type": ["ldp:Resource", "https://www.w3.org/ns/iana/media-types/application/ld+json#Resource"],
      "dc:modified": {
        "@type": "xsd:dateTime",
        "@value": "2025-10-21T10:14:27.000Z"
      },
      "posix:size": 132
    }
  ]
}
```


# WebIDs

According to the [WebID specification section 1](https://www.w3.org/2005/Incubator/webid/spec/identity/#introduction), a WebID can be defined as:

> an HTTP URI which refers to an Agent (Person, Organization, Group, Device, etc.). A description of the WebID can be found in the Profile Document, a type of web page that any Social Network user is familiar with.

A WebID MUST be available as `application/ld+json`.  We RECOMMEND that platforms also provide WebIDs as `text/turtle` [[turtle](#turtle)] to offer compliance with Solid applications.

WebIDs MUST be publicly accessible.

## DFC platform WebID

Platforms MUST expose a public platform WebID [[WEBID](#webid)]. A platform's WebID MUST advertise the protocol version(s) it supports using the `dfc-t:supportedProtocolVersion` predicate. A platform's WebID MUST also provide a link to its platform [Identity Service](#platform-identity-service) using the `dfc-t:hasIdentityService` predicate and MAY also provide other information like [public indexes].

```json
{
    "@graph": [
        {
            "@id": "https://platform.ex/webid",
            "@type": "foaf:PersonalProfileDocument",
            "foaf:maker": "https://platform.ex/webid#me",
            "foaf:primaryTopic": "https://platform.ex/webid#me"
        },
        {
            "@id": "https://platform.ex/webid#me",
            "@type": [
                "dfc-t:Platform",
                "foaf:Agent"
            ],
            "foaf:name": "Example platform",
            "dfc-t:supportedProtocolVersion": "2.0.0",
            "dfc-t:supportedOntologyVersion": "1.16.0",
            "dfc-t:hasIdentityService": "https://platform.ex/identity-service"
        }
    ]
}
```

## DFC user WebIDs

Platforms MUST expose a public WebID [[WEBID](#webid)] for each of their DFC users. A DFC user WebID is linked to a private preferences document with the `pim:preferencesFile` predicate. This preferences document is linked to a private TypeIndex using the `solid:privateTypeIndex` predicate. The private TypeIndex registers instance(s) of `dfc-b:Organization`.

```json
{
    "@graph": [
        {
            "@id": "https://platform.ex/users/john/webid",
            "@type": "foaf:PersonalProfileDocument",
            "foaf:maker": "https://platform.ex/users/john/webid#me",
            "foaf:primaryTopic": "https://platform.ex/users/john/webid#me"
        },
        {
            "@id": "https://platform.ex/users/john/webid#me",
            "@type": "foaf:Agent",
            "pim:preferencesFile": "https://platform.ex/user/john/prefs"
        }
    ]
}
{
    "@graph": [
        {
            "@id": "https://platform.ex/user/john/prefs",
            "@type": "pim:ConfigurationFile"
        },
        {
            "@id": "https://platform.ex/users/john/webid#me",
            "solid:privateTypeIndex": "https://platform.ex/user/john/privateTypeIndex"
        }
    ]
}
{
    "@graph": [
        {
            "@id": "https://platform.ex/user/john/privateTypeIndex",
            "@type": ["solid:TypeIndex", "solid:ListedDocument"]
        },
        {
            "@id": "https://platform.ex/user/john/privateTypeIndex#reg1",
            "@type": "solid:TypeRegistration",
            "solid:forClass": "dfc-b:Organization",
            "solid:instance": "https://platform.ex/organizations/johns/index"
        }
    ]
}
```

*Note: the organization(s) related to a WebID are not directly linked into the WebID using the dfc-b:affiliatedBy predicate. Instead a private TypeIndex is used. This mechanism is needed to ensure a proper separation between the identity or user account (WebID) and the DFC Agent.*

## Extended WebID profile

To restrict the access to user's private data, an [extended WebID profile](https://solid.github.io/webid-profile/#extended-profile) [[SOLID-WEBID](#solid-webid)] can be defined in a private [RDF document](https://www.w3.org/TR/2014/REC-rdf11-concepts-20140225/#dfn-rdf-document) [[RDF11-CONCEPTS](#rdf11-concepts)] containing triples with the WebID as subject. This extended WebID profile document is linked from the root WebID profile document using the `rdfs:seeAlso` predicate.

```json
{
    "@graph": [
        {
            "@id": "https://platform.ex/users/john/webid",
            "@type": "foaf:PersonalProfileDocument",
            "foaf:maker": "https://platform.ex/users/john/webid#me",
            "foaf:primaryTopic": "https://platform.ex/users/john/webid#me"
        },
        {
            "@id": "https://platform.ex/users/john/webid#me",
            "@type": "foaf:Agent",
            "rdfs:seeAlso": "https://platform.ex/users/john/extended"
        }
    ]
}
```

The below example presents the private extended profile document hosted at `https://platform.ex/users/john/extended` :

```json
{
    "@graph": [
        {
            "@id": "https://platform.ex/users/john/webid#me",
            "@type": "dfc-b:Person",
            "dfc-b:firstName": "John"
        }
    ]
}
```

The complete user's profile can be assembled by "collecting all statements that have the WebID as the subject or the object, regardless of their originating documents". See the [[SOLID-WEBID](#solid-webid)] profile specification [section 2](https://solid.github.io/webid-profile/#discovery) for more details.

# DFC objects

*The following APIs are a way to represent and exchange data. The way the data is internally stored by a DFC platform can be different.*

DFC objects are organized into LDP containers. The root container is the *organization container*. All the other DFC objects (catalogs, supplied products and so on) are direct descendant of this *organization container*. The location of these objects within the *organization container* is listed in the following table:

| Object type | Location within the organization container |
| ------ | ---------------------------------------- |
| `dfc-b:Catalog` | `/catalogs/` |
| `dfc-b:SuppliedProduct` | `/supplied-products/` |
| `dfc-b:Order` | `/orders/` |

For instance, if the *organization container* is located at `https://platform.ex/organizations/johns/`, the catalogs container will be located at `https://platform.ex/organizations/johns/catalogs/`.

## Organization

*The following API is a way to represent and exchange data. The way the data is internally stored is left to the DFC platform.*

An organization (`dfc-b:Organization`) is the object that contains all the others (catalogs, supplied products and so on). It is the entry point object that is registered into the user's TypeIndex.

The *organization container* is a LDP container which contain an `index` resource describing the organization like in the below example:

```json
{
    "@base": "https://platform.ex/organizations/",
    "@graph": [
        {
            "@id": "johns/index",
            "@type": "dfc-b:Organization",
            "dfc-b:name": "John's organization"
        },
    ]
}
```

An enteprise object follows the SHACL [[SHACL](#shacl)] shape:

```turtle
:OrganizationShape a sh:NodeShape;
    sh:targetClass dfc-b:Organization;
    sh:closed false;

    sh:property [
		sh:path dfc-b:name;
        sh:message "The name of the organization.";
        sh:datatype xsd:string;
        sh:maxCount 1;
	];

    sh:property [
		sh:path dfc-b:description;
        sh:message "The description of the organization.";
        sh:datatype xsd:string;
	];

    sh:property [
		sh:path dfc-b:date;
        sh:message "The date of the organization.";
        sh:datatype xsd:dateTime;
	];

    sh:property [
		sh:path dfc-b:hasAddress;
        sh:message "The different addresses of the organization.";
        sh:datatype dfc-b:Address;
	];

    sh:property [
		sh:path dfc-b:hasPhoneNumber;
        sh:message "The different phone numbers of the organization.";
        sh:datatype dfc-b:PhoneNumber;
	];

    sh:property [
		sh:path dfc-b:hasSocialMedia;
        sh:message "The different social medias of the organization.";
        sh:datatype dfc-b:SocialMedia;
	];

    sh:property [
		sh:path dfc-b:logo;
        sh:message "The different logos of the organization.";
        sh:datatype xsd:anyURI;
	];

    sh:property [
		sh:path dfc-b:orders;
        sh:message "The different orders ordered by the organization.";
        sh:datatype dfc-b:Order;
	];

    sh:property [
		sh:path dfc-b:requests;
        sh:message "To be defined.";
        sh:datatype dfc-b:FunctionalProduct;
	];

    sh:property [
		sh:path dfc-b:owns;
        sh:message "To be defined.";
        sh:datatype dfc-b:PhysicalProduct;
	];

    sh:property [
		sh:path dfc-b:email;
        sh:message "To be defined.";
        sh:datatype xsd:string;
	];

    sh:property [
		sh:path dfc-b:websitePage;
        sh:message "To be defined.";
        sh:datatype xsd:anyURI;
	];

    sh:property [
		sh:path dfc-b:VATnumber;
        sh:message "To be defined.";
        sh:datatype xsd:string;
        sh:maxCount 1;
	];

    sh:property [
		sh:path dfc-b:defines;
        sh:message "To be defined.";
        sh:datatype dfc-b:CustomerCategory;
	];

    sh:property [
		sh:path dfc-b:hasMainContact;
        sh:message "To be defined.";
        sh:datatype dfc-b:Person;
        sh:maxCount 1;
	];

    sh:property [
		sh:path dfc-b:supplies;
        sh:message "To be defined.";
        sh:datatype dfc-b:SuppliedProduct;
	];

    sh:property [
		sh:path dfc-b:manages;
        sh:message "To be defined.";
        sh:datatype dfc-b:CatalogItem;
	];

    sh:property [
		sh:path dfc-b:proposes;
        sh:message "To be defined.";
        sh:datatype dfc-b:TechnicalProduct;
	];

    sh:property [
		sh:path dfc-b:transforms;
        sh:message "To be defined.";
        sh:datatype dfc-b:AsPlannedLocalTransformation;
	];

    sh:property [
		sh:path dfc-b:maintains;
        sh:message "To be defined.";
        sh:datatype dfc-b:Catalog;
	];

    sh:property [
		sh:path dfc-b:affiliates;
        sh:message "To be defined.";
        sh:datatype dfc-b:Person;
	];

    sh:property [
		sh:path dfc-b:coordinatedBy;
        sh:message "To be defined.";
        sh:datatype dfc-b:Coordination;
	].
```

## Catalogs

*The following API is a way to represent and exchange data. The way the data is internally stored is left to the DFC platform.*

Within the *organization container*, catalogs are stored into the `catalogs` container.

A particular catalog (`dfc-b:Catalog`) is itself a LDP container which contain an `index` resource describing the catalog. The index resource also contains the catalog items, the offers and the prices of the catalog.

Within that index document:
- Catalog items can be found by listing the subjects `?subject` where `?subject a dfc-b:CatalogItem`.
- Offers can be found by listing the subjects `?subject` where `?subject a dfc-b:Offer`.
- Sold products can be found by listing the objects `?object` where `?subject dfc-b:references ?object`.

Catalogs can be listed by requesting the representation of the `catalogs` container. In the following HTTP requests example, the `catalogs` container contains only one catalog (`https://platform.ex/organizations/johns/catalogs/catalog1/`):

```http
GET /organizations/johns/catalogs/ HTTP/1.1
Host: platform.ex
Accept: application/ld+json
```

The response:

```http
HTTP/1.1 200 OK 
Content-Type: application/ld+json; charset=UTF-8
Link: <http://www.w3.org/ns/ldp#BasicContainer>; rel="type", <http://www.w3.org/ns/ldp#Resource>; rel="type"
Allow: OPTIONS,HEAD,GET,POST,PUT,PATCH
Accept-Post: application/ld+json, image/bmp, image/jpeg

{
  "@context": {
    "ldp": "http://www.w3.org/ns/ldp#"
  },
  "@graph": [
    {
      "@id": "https://platform.ex/organizations/johns/catalogs/",
      "@type": ["ldp:Container", "ldp:BasicContainer"],
      "ldp:contains": ["https://platform.ex/organizations/johns/catalogs/catalog1/"]
    }
  ]
}
```

Below is an example of the catalog `/catalogs/catalog1/index`:

```json
{
    "@base": "https://platform.ex/organizations/johns/catalogs/",
    "@graph": [
        {
            "@id": "catalog1/index",
            "@type": "dfc-b:Catalog",
            "dfc-b:maintainedBy": "https://platform.ex/organizations/johns/index",
            "dfc-b:name": "Catalog example",
            "dfc-b:image": "catalog1/image1.jpg",
        },
        {
            "@id": "catalog1/index#catalogItem1",
            "@type": "dfc-b:CatalogItem",
            "dfc-b:references": "https://platform.ex/organizations/johns/supplied-products/tomato/index",
            "dfc-b:offeredThrough": "catalog1/index#offer1"
        },
        {
            "@id": "catalog1/index#offer1",
            "@type": "dfc-b:Offer",
            "dfc-b:hasPrice": "catalog1/index#price1",
            "dfc-b:offers": "catalog1/index#catalogItem1"
        },
        {
            "@id": "catalog1/index#price1",
            "@type": "dfc-b:price",
            "dfc-b:value": "2.4"
        },
    ]
}
```

## Supplied products

*The following API is a way to represent and exchange data. The way the data is internally stored is left to the DFC platform.*

Within the *organization container*, supplied products are stored into the `supplied-products` container.

A particular supplied product (`dfc-b:SuppliedProduct`) is itself a LDP container which contain an `index` resource describing the product.

```json
{
    "@base": "https://platform.ex/organizations/johns/supplied-products/",
    "@graph": [
        {
            "@id": "suppliedProduct1/index",
            "@type": "dfc-b:SuppliedProduct",
            "dfc-b:producedBy": "https://platform.ex/organizations/johns/index",
            "dfc-b:name": "Tomato",
            "dfc-b:hasType": "dfc-pt:tomato",
        },
    ]
}
```

A supplied product object follows the SHACL [[SHACL](#shacl)] shape:

```turtle
:SuppliedProductShape a sh:NodeShape;
    sh:targetClass dfc:SuppliedProduct;
    sh:closed false;

    sh:property [
		sh:path dfc:name;
        sh:message "The name of the defined product.";
        sh:datatype xsd:string;
        sh:maxCount 1;
	];

    sh:property [
		sh:path dfc:description;
        sh:message "The description of the defined product.";
        sh:datatype xsd:string;
	];

  sh:property [
		sh:path dfc:date;
        sh:message "The date of the defined product.";
        sh:datatype xsd:dateTime;
	];

  sh:property [
		sh:path dfc:consumedBy;
        sh:datatype dfc:AsPlannedConsumptionFlow;
	];

  sh:property [
		sh:path dfc:hasAllergenCharacteristic;
        sh:datatype dfc:AllergenCharacteristic;
	];

  sh:property [
		sh:path dfc:hasBrand;
        sh:datatype xsd:string;
	];

  sh:property [
		sh:path dfc:hasCertification;
        sh:datatype skos:Concept;
	];

  sh:property [
		sh:path dfc:hasCharacteristic;
        sh:datatype dfc:DFC_BusinessOntology_Characteristic;
	];

  sh:property [
		sh:path dfc:hasClaim;
        sh:datatype skos:Concept;
	];

  sh:property [
		sh:path dfc:hasContainerInformation;
        sh:datatype skos:Concept;
	];

  sh:property [
		sh:path dfc:hasNatureOrigin;
        sh:datatype skos:Concept;
	];

  sh:property [
		sh:path dfc:hasPartOrigin;
        sh:datatype skos:Concept;
	];

  sh:property [
		sh:path dfc:hasGeographicalOrigin;
        sh:datatype skos:Concept;
	];

  sh:property [
		sh:path dfc:hasIngredient;
        sh:datatype dfc:Ingredient;
	];

  sh:property [
		sh:path dfc:hasLabellingCharacteristic;
        sh:datatype dfc:LabellingCharacteristic;
	];

  sh:property [
		sh:path dfc:hasNutrientCharacteristic;
        sh:datatype dfc:NutrientCharacteristic;
	];

  sh:property [
		sh:path dfc:hasPercentageOfAlcoholByVolume;
        sh:datatype xsd:float;
	];

  sh:property [
		sh:path dfc:hasPhysicalCharacteristic;
        sh:datatype dfc:PhysicalCharacteristic;
	];

  sh:property [
		sh:path dfc:hasQuantity;
        sh:datatype dfc:QuantitativeValue;
	];

  sh:property [
		sh:path dfc:hasType;
        sh:datatype xsd:anyURI;
	];

  sh:property [
		sh:path dfc:hasUnit;
        sh:datatype skos:Concept;
	];

  sh:property [
		sh:path dfc:hasVariant;
        sh:datatype dfc:DefinedProduct;
	];

  sh:property [
		sh:path dfc:image;
        sh:datatype xsd:anyURI;
	];

  sh:property [
		sh:path dfc:isVariantOf;
        sh:datatype dfc:DefinedProduct;
	];

  sh:property [
		sh:path dfc:lifetime;
        sh:datatype xsd:float;
        sh:maxCount 1;
	];

  sh:property [
		sh:path dfc:referencedBy;
        sh:datatype dfc:CatalogItem;
	];

  sh:property [
		sh:path dfc:URL;
        sh:datatype xsd:anyURI;
	];

  sh:property [
		sh:path dfc:specificCondition;
        sh:datatype xsd:string;
	];

  sh:property [
		sh:path dfc:availabilityTime;
        sh:datatype xsd:duration;
	];

  sh:property [
		sh:path dfc:deliveryCondition;
        sh:datatype xsd:string;
	];

  sh:property [
		sh:path dfc:frozen;
        sh:datatype xsd:boolean;
        sh:maxCount 1;
	];

  sh:property [
		sh:path dfc:hasTemperature;
        sh:datatype dfc:Temperature;
	];

  sh:property [
		sh:path dfc:industrializes;
        sh:datatype dfc:TechnicalProduct;
	];

  sh:property [
		sh:path dfc:producedBy;
        sh:datatype dfc:AsPlannedProductionFlow;
	];

  sh:property [
		sh:path dfc:referenceOf;
        sh:datatype dfc:LocalizedProduct;
	];

  sh:property [
		sh:path dfc:refrigerated;
        sh:datatype xsd:boolean;
        sh:maxCount 1;
	];

  sh:property [
		sh:path dfc:suppliedBy;
        sh:datatype dfc:organization;
        sh:maxCount 1;
	];

  sh:property [
		sh:path dfc:totalTheoreticalStock;
        sh:datatype xsd:float;
        sh:maxCount 1;
	].
```

## Orders

## Optional organization indexes

TODO: these indexes compile organization's data in order to provide better performances. For instance, we could have an index to get all the orders of the week at once.

## Optional platform indexes

TODO: define the different supported indexes a platform can provide. Do we want a precise vocabulary with a predicate for each type of index? Do we want to provide a list of indexes and let the client determine each index purpose? Do we want both mechanisms?
TODO: this kind of indexes is optional

```json
{
  "@graph": [
      {
        "@id": "https://platform.ex/organizationIndex",
        "@type": "idx:Index"
      },
      {
        "@id": "https://platform.ex/organizationIndex#byCertification",
        "@type": "sh:NodeShape",
        "sh:closed": "false",
        "sh:property": [
          {
            "sh:path": "rdf:type",
            "sh:hasValue": {
              "@id": "dfc-b:Organization"
            }
          },
          {
            "sh:path": "dfc-b:hasCertification"
          }
        ]
      },
      {
        "@id": "https://platform.ex/organizationByCertificationIndex#Organic",
        "@type": "idx:IndexEntry",
        "idx:hasShape": "https://platform.ex/organizationIndex#byCertification",
        "idx:hasSubIndex": "https://platform.ex/organizationByCertificationIndex"
      }
    ]
  }
{
  "@graph": [
      {
        "@id": "https://platform.ex/organizationByCertificationIndex",
        "@type": "idx:Index"
      },
      {
        "@id": "https://platform.ex/organizationByCertificationIndex#target",
        "@type": "sh:NodeShape",
        "sh:closed": "false",
        "sh:property": [
          {
            "sh:path": "rdf:type",
            "sh:hasValue": {
              "@id": "dfc-b:Organization"
            }
          },
          {
            "sh:path": "dfc-b:hasCertification"
          }
        ]
      },
      {
        "@id": "https://platform.ex/organizationByCertificationIndex#Organic",
        "@type": "idx:IndexEntry",
        "idx:hasShape": "https://platform.ex/organizationByCertificationIndex#target",
        "idx:hasSubIndex": "https://platform.ex/organicEuorganizationIndex"
      }
    ]
  }
{
  "@graph": [
      {
        "@id": "https://platform.ex/organicEuorganizationIndex",
        "@type": "idx:Index"
      },
      {
        "@id": "https://platform.ex/organicEuorganizationIndex#target",
        "@type": "sh:NodeShape",
        "sh:closed": "false",
        "sh:property": [
          {
            "sh:path": "rdf:type",
            "sh:hasValue": {
              "@id": "dfc-b:Organization"
            }
          },
          {
            "sh:path": "dfc-b:hasCertification",
            "sh:hasValue": "dfc-f:Organic-EU"
          }
        ]
      },
      {
        "@id": "https://platform.ex/organicEuorganizationIndex#johns",
        "@type": "idx:IndexEntry",
        "idx:hasShape": "https://platform.ex/organicEuorganizationIndex#target",
        "idx:target": "https://platform.ex/organization/johns/index"
      }
    ]
  }
```

# Platform Identity Service

The Identity Service (IS) allows a client to get the WebID of the user on the queried platform in exchange of an OIDC token containing the user's email address. This service is required as each DFC platform have a dedicated WebID per user. A matching mechanism is therefore need. The Identity Service is a public HTTP service provided by each DFC platform.

The Identity Service waits for incoming [HTTP HEAD requests](https://datatracker.ietf.org/doc/html/rfc2068#section-9.4) [[RFC9112](#rfc9112)] at a single endpoint, let's say `/identity-service`. When receiving a HTTP HEAD request containing a valid DFC OIDC token (containing the user's email) the Identity Service MUST respond with a [303 HTTP response](https://datatracker.ietf.org/doc/html/rfc2068#section-10.3.4) [[RFC9112](#rfc9112)] with the `Location` header pointing to the user's WebID on the queried platform. If the user's WebID does not exist yet, the queried platform MUST create it before returning the response.

If the OIDC token provided in the HTTP HEAD request is not valid, the Identity Server MUST respond with a [403 Forbidden HTTP response](https://datatracker.ietf.org/doc/html/rfc2068#section-10.4.4). When the OIDC token is malformed or does not contain a user's email address, the Identity Service MUST return a [HTTP 400 Bad Request](https://datatracker.ietf.org/doc/html/rfc2068#section-10.4.1) response.

To get the user's WebID, a platform SHALL use the email address contained in the OIDC token. The implementation of the matching mechanism between the user's email address and the user's WebID is left to the discretion of the platform. For instance a platform MAY keep the matching in a database or in a RDF store.

Any DFC platform MUST expose an Identity Service (IS). The platform Identity Service MUST be advertised in the public [platform's WebID](#dfc-platform-webid) using the `dfc-t:hasIdentityService` predicate.

The following HTTP exchange sample shows the process of getting a user's WebID on a DFC platform giving a valid OIDC token.

Request to `https://platform.ex/identity-service`:

```http
HEAD /identity-service HTTP/1.1
Host: platform.ex
Authenticate: OIDC token here
```

Response:

```http
HTTP/1.1 303 See other
Date: Tue, 08 Jul 2025 16:02:43 GMT
Location: https://platform.ex/users/john/webid#me
```

# References

## Normative references

### [DC-TERMS]

Dublin Core Metadata Terms, version 1.1. DCMI Usage Board. DCMI. 11 October 2010. DCMI Recommendation. URL: http://dublincore.org/documents/2010/10/11/dcmi-terms/.

### [LDP]

Linked Data Platform 1.0. Steve Speicher; John Arwe; Ashok Malhotra. W3C. 26 February 2015. W3C Recommendation. URL: https://www.w3.org/TR/ldp/.

### [RDF11-CONCEPTS]

Richard Cyganiak, David Wood, Markus Lanthaler. RDF 1.1 Concepts and Abstract Syntax. W3C Recommendation, 25 February 2014. URL: http://www.w3.org/TR/2014/REC-rdf11-concepts-20140225/. The latest edition is available at http://www.w3.org/TR/rdf11-concepts/.

### [RFC2119]

Key words for use in RFCs to Indicate Requirement Levels. S. Bradner. IETF. March 1997. Best Current Practice. URL: https://datatracker.ietf.org/doc/html/rfc2119.

### [RFC8174]

Ambiguity of Uppercase vs Lowercase in RFC 2119 Key Words. B. Leiba. IETF. May 2017. Best Current Practice. URL: https://datatracker.ietf.org/doc/html/rfc8174.

### [RFC9112]

HTTP/1.1. R. Fielding, M. Nottingham, J. Reschke, Eds. IETF. June 2022. Internet Standard. URL: https://www.rfc-editor.org/rfc/rfc9112.

### [SHACL]

Shapes Constraint Language (SHACL). Holger Knublauch; TopQuadrant, Inc; Dimitris Kontokostas; University of Leipzig. URL: https://www.w3.org/TR/shacl/.

### [SOLID]

Solid Protocol. Sarven Capadisli; Tim Berners-Lee; Kjetil Kjernsmo. Draft Community Group Report, 12 May 2024. URL: https://solidproject.org/TR/protocol.

### [SOLID-WEBID]

Solid WebID Profile. Virginia Balseiro; Jeff Zucker; Sarven Capadisli. Draft Community Group Report, 12 May 2024. URL: https://solid.github.io/webid-profile/.

### [TYPEINDEX]

Type indexes. Timea Turdean. Version 1.0.0, Editor’s Draft, 2023-03-13. URL: https://solid.github.io/type-indexes/.

### [WEBID]

Andrei Sambra; Stéphane Corlosquet. WebID 1.0, Web Identity and Discovery. W3C Draft Community Group Report, 05 March 2014 . URL: https://www.w3.org/2005/Incubator/webid/spec/identity/.

## Informative references

[turtle]
RDF 1.1 Turtle. Eric Prud'hommeaux; Gavin Carothers. W3C. 25 February 2014. W3C Recommendation. URL: https://www.w3.org/TR/turtle/.