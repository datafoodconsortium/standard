# Introduction

*This section is non-normative.*

In order to interoperate, DFC platforms need to discover and advertise data, respectively from and to other platforms of the DFC ecosystem. For instance when a user creates a new product on a platform "A", the platform "A" needs to know where to create that product on the user's other platforms. To do so the DFC protocol defines entry points who take the form of URIs called [WebID](#webids). Two kinds of WebIDs are defined:

- A platform's WebID leading to platform-wide information like the services and the indexes it exposes;
- A user's WebID leading to information related to a particular user like the enterprise(s) he is affiliated to.

On each platform, each user has his own WebID. Therefore a mechanism to find equivalent users WebID between platforms is needed. This specification defines the platform [Identity Service](#platform-identity-service) that a platform can query to obtain a WebID in exchange of an OIDC token.

A platform can provide optional [specific indexes]() to help to discover some data at the platform or enterprise level. At the enterprise level an index can allow to get all orders for a particular time interval for instance. At the platform level indexes can be defined to advertise which kinds of product are sold by producers or to find enterprises by name.

# Namespaces

| Prefix | IRI | Description |
| ------ | --- | ----------- |
| `dfc-b` |   | DFC business ontology. |
| `dfc-t` |   | DFC technical ontology. |
| `foaf` | http://xmlns.com/foaf/0.1/ | Friend of a Friend ontology. |
| `idx` | https://ns.inria.fr/idx/terms# | Shacl shape-based indexing ontology. |
| `ldp` | http://www.w3.org/ns/ldp# |  |
| `pim` | http://www.w3.org/ns/pim/space# |  |
| `rdfs` | http://www.w3.org/2000/01/rdf-schema# |  |
| `sh` | http://www.w3.org/ns/shacl# |  |
| `solid` | http://www.w3.org/ns/solid/terms# | [Social Linked Data](https://solidproject.org/) terms. |

# Conformance

All assertions, diagrams, examples, and notes are non-normative, as are all sections explicitly marked non-normative. Everything else is normative.
The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” are to be interpreted as described in [BCP 14](https://tools.ietf.org/html/bcp14) [[RFC2119](#rfc2119)] [[RFC8174](#rfc8174)] when, and only when, they appear in all capitals, as shown here.

# Resource Containment

Like the Solid protocol ([source](https://solidproject.org/TR/protocol#resource-containment)), the DFC protocol "*has the notion of containers to represent a collection of linked resources to help with resource discovery and lifecycle management*". The representation and behavior of containers in DFC corresponds to [LDP basic container](https://www.w3.org/TR/ldp/#ldpbc) [[LDP](#ldp)] and MUST be supported by DFC platforms.

Like the Solid protocol, the DFC protocol has the notion of [hierarchical resource containment](https://solidproject.org/TR/protocol#server-hierarchical-containment) [[SOLID](#solid)]. The Solid protocol states:

> There is a 1-1 correspondence between containment triples and relative reference within the path name hierarchy. [[Source](https://github.com/solid/specification/issues/98#issuecomment-547506617)]. It follows that all resources are discoverable from a container and that it is not possible to create orphan resources. [[Source](https://github.com/solid/specification/issues/97#issuecomment-547459396)]

Accordingly to this statement, if the resource `resource` is contained in the container `https://platform.ex/container/`, this resource URI MUST be `https://platform.ex/container/resource`. A same URI can not be listed by two different containers in the scope of this specification. Platforms are encouraged to follow these requirements for the containers they define outside the scope of this specification.

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
    "ldp": "http://www.w3.org/ns/ldp#"
  },
  "@graph": [
    {
      "@id": "https://platform.ex/container/",
      "@type": ["ldp:Container", "ldp:BasicContainer"],
      "ldp:contains": ["https://platform.ex/container/resource"]
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

Platforms MUST expose a public WebID [[WEBID](#webid)] for each of their DFC users who can be producers or enterprises. A DFC user WebID is linked to a private preferences document with the `pim:preferencesFile` predicate. This preferences document is linked to a private TypeIndex using the `solid:privateTypeIndex` predicate. The private TypeIndex register instance(s) of `dfc-b:Enterprise`.

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
            "@type": "solid:TypeIndexRegistration",
            "solid:forClass": "dfc-b:Enterprise",
            "solid:instance": "https://platform.ex/enterprises/john-s-enterprise/index"
        }
    ]
}
```

*Note: the enterprise(s) related to a WebID are not directly linked into the WebID using the dfc-b:affiliatedBy predicate. Instead a private TypeIndex is used. This mechanism is needed to ensure a proper separation between the identity or user account (WebID) and the DFC Agent.*

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

DFC objects are organized into LDP containers. The root container is the *enterprise container*. All the other DFC objects (catalogs, supplied products and so on) are direct descendant of this *enterprise container*. The location of these objects within the *enterprise container* is listed in the following table:

| Object type | Location within the enterprise container |
| ------ | ---------------------------------------- |
| `dfc-b:Catalog` | `/catalogs/` |
| `dfc-b:SuppliedProduct` | `/supplied-products/` |
| `dfc-b:Order` | `/orders/` |

For instance, if the *enterprise container* is located at `https://platform.ex/enterprises/john-s-enterprise/`, the catalogs container will be located at `https://platform.ex/enterprises/john-s-enterprise/catalogs/`.

## Enterprise

*The following API is a way to represent and exchange data. The way the data is internally stored by a DFC platform can be different.*

An enterprise (`dfc-b:Enterprise`) is the object that contains all the others (catalogs, supplied products and so on). It is the entry point object that is registered into the user's TypeIndex.

The *enterprise container* is a LDP container which contain an `index` resource describing the enterprise like in the below example:

```json
{
    "@base": "https://platform.ex/enterprises/",
    "@graph": [
        {
            "@id": "john-s-enterprise/index",
            "@type": "dfc-b:Enterprise",
            "dfc-b:name": "John's enterprise"
        },
    ]
}
```

## Catalogs

*The following API is a way to represent and exchange data. The way the data is internally stored by a DFC platform can be different.*

Within the *enterprise container*, catalogs are stored into the `catalogs` container.

A particular catalog (`dfc-b:Catalog`) is itself a LDP container which contain an `index` resource describing the catalog. The index resource also contains the catalog items, the offers and the prices of the catalog.

Within that index document:
- Catalog items can be found by listing the subjects `?subject` where `?subject a dfc-b:CatalogItem`.
- Offers can be found by listing the subjects `?subject` where `?subject a dfc-b:Offer`.
- Sold products can be found by listing the objects `?object` where `?subject dfc-b:references ?object`.

Catalogs can be listed by requesting the representation of the `catalogs` container. In the following HTTP requests example, the `catalogs` container contains only one catalog (`https://platform.ex/enterprises/john-s-enterprise/catalogs/catalog1/`):

```http
GET /enterprises/john-s-enterprise/catalogs/ HTTP/1.1
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
      "@id": "https://platform.ex/enterprises/john-s-enterprise/catalogs/",
      "@type": ["ldp:Container", "ldp:BasicContainer"],
      "ldp:contains": ["https://platform.ex/enterprises/john-s-enterprise/catalogs/catalog1/"]
    }
  ]
}
```

Below is an example of the catalog `/catalogs/catalog1/index`:

```json
{
    "@base": "https://platform.ex/enterprises/john-s-enterprise/catalogs/",
    "@graph": [
        {
            "@id": "catalog1/index",
            "@type": "dfc-b:Catalog",
            "dfc-b:maintainedBy": "https://platform.ex/enterprises/john-s-enterprise/index",
            "dfc-b:name": "Catalog example",
            "dfc-b:image": "catalog1/image1.jpg",
        },
        {
            "@id": "catalog1/index#catalogItem1",
            "@type": "dfc-b:CatalogItem",
            "dfc-b:references": "https://platform.ex/enterprises/john-s-enterprise/supplied-products/tomato/index",
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

*The following API is a way to represent and exchange data. The way the data is internally stored by a DFC platform can be different.*

Within the *enterprise container*, supplied products are stored into the `supplied-products` container.

A particular supplied product (`dfc-b:SuppliedProduct`) is itself a LDP container which contain an `index` resource describing the product.

```json
{
    "@base": "https://platform.ex/enterprises/john-s-enterprise/supplied-products/",
    "@graph": [
        {
            "@id": "suppliedProduct1/index",
            "@type": "dfc-b:SuppliedProduct",
            "dfc-b:producedBy": "https://platform.ex/enterprises/john-s-enterprise/index",
            "dfc-b:name": "Tomato",
            "dfc-b:hasType": "dfc-pt:tomato",
        },
    ]
}
```

## Orders

## Optional enterprise indexes

TODO: these indexes compile enterprise's data in order to provide better performances. For instance, we could have an index to get all the orders of the week at once.

## Optional platform indexes

TODO: define the different supported indexes a platform can provide. Do we want a precise vocabulary with a predicate for each type of index? Do we want to provide a list of indexes and let the client determine each index purpose? Do we want both mechanisms?
TODO: this kind of indexes is optional

```json
{
  "@graph": [
      {
        "@id": "https://platform.ex/enterpriseIndex",
        "@type": "idx:Index"
      },
      {
        "@id": "https://platform.ex/enterpriseIndex#byCertification",
        "@type": "sh:NodeShape",
        "sh:closed": "false",
        "sh:property": [
          {
            "sh:path": "rdf:type",
            "sh:hasValue": {
              "@id": "dfc-b:Enterprise"
            }
          },
          {
            "sh:path": "dfc-b:hasCertification"
          }
        ]
      },
      {
        "@id": "https://platform.ex/enterpriseByCertificationIndex#Organic",
        "@type": "idx:IndexEntry",
        "idx:hasShape": "https://platform.ex/enterpriseIndex#byCertification",
        "idx:hasSubIndex": "https://platform.ex/enterpriseByCertificationIndex"
      }
    ]
  }
{
  "@graph": [
      {
        "@id": "https://platform.ex/enterpriseByCertificationIndex",
        "@type": "idx:Index"
      },
      {
        "@id": "https://platform.ex/enterpriseByCertificationIndex#target",
        "@type": "sh:NodeShape",
        "sh:closed": "false",
        "sh:property": [
          {
            "sh:path": "rdf:type",
            "sh:hasValue": {
              "@id": "dfc-b:Enterprise"
            }
          },
          {
            "sh:path": "dfc-b:hasCertification"
          }
        ]
      },
      {
        "@id": "https://platform.ex/enterpriseByCertificationIndex#Organic",
        "@type": "idx:IndexEntry",
        "idx:hasShape": "https://platform.ex/enterpriseByCertificationIndex#target",
        "idx:hasSubIndex": "https://platform.ex/organicEuEnterpriseIndex"
      }
    ]
  }
{
  "@graph": [
      {
        "@id": "https://platform.ex/organicEuEnterpriseIndex",
        "@type": "idx:Index"
      },
      {
        "@id": "https://platform.ex/organicEuEnterpriseIndex#target",
        "@type": "sh:NodeShape",
        "sh:closed": "false",
        "sh:property": [
          {
            "sh:path": "rdf:type",
            "sh:hasValue": {
              "@id": "dfc-b:Enterprise"
            }
          },
          {
            "sh:path": "dfc-b:hasCertification",
            "sh:hasValue": "dfc-f:Organic-EU"
          }
        ]
      },
      {
        "@id": "https://platform.ex/organicEuEnterpriseIndex#john-s-enterprise",
        "@type": "idx:IndexEntry",
        "idx:hasShape": "https://platform.ex/organicEuEnterpriseIndex#target",
        "idx:target": "https://platform.ex/enterprise/john-s-enterprise/index"
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