TBD.

# Structure

This document describes the structure of the data in a enterprise, the main class of which is `dfc-b:Enterprise`. The enterprise data is a set of RDF triples - linked data links. When reading an enterprise, where there are links, those must be followed, and no assumptions made about the URIs.

An enterprise is all built inside a single folder (LDP Container). Let us call the URI of that folder -- less its final '/' -- $ROOT. It could be say, https://example.org/myEnterprise. Developers must never make assumptions about where an enterprise is stored as enterprises are used within other apps which create the container for them.

For instance, a request to get the enterprise folder at https://example.org/myEnterprise/:
```http
GET /myEnterprise/ HTTP/1.1
Host: example.org
Accept: application/ld+json
```

Should respond something like:
```http
HTTP/1.1 200 OK
Content-Type: application/ld+json

{
    "@context": {
        "ldp": "http://www.w3.org/ns/ldp#",
        "ldp:contains": {
            "@type": "@id"
        }
    },
    "@base": "https://example.org/myEnterprise/",
    "@graph": [
        {
            "@id": "./",
            "@type": "ldp:BasicContainer",
            "ldp:contains": [
                "./index",
                "./typeIndex"
            ]
        }
    ]
}
```

Within that folder, the enterprise object is normally defined at $ROOT/index#this. The enterprise object MUST have the type `dfc-b:Enterprise` and the `dfc-b:name` and `dfc-b:hasTypeIndex` properties MUST be defined. Other properties can be defined like listed in the enterprise shape below.

A request to get the root document of the enterprise https://example.org/myEnterprise/index#this:
```http
GET /myEnterprise/index HTTP/1.1
Host: example.org
Accept: application/ld+json
```

Should respond something like:

```http
HTTP/1.1 200 OK
Content-Type: application/ld+json

{
    "@context": "https://www.datafoodconsortium.org",
    "@base": "https://example.org/myEnterprise/",
    "@graph": [
        {
            "@id": "./index#this",
            "@type": "dfc-b:Enterprise",
            "dfc-b:name": "My enterprise",
            "dfc-b:hasTypeIndex": "./typeIndex"
        }
    ]
}
```

The enterprise folder can contain 

# Internal type index

Within the enterprise root document the value of the property `dfc-b:hasTypeIndex` MUST link to a `solid:TypeIndex` document contained into the enterprise folder (normally at the root level). This type index contains links to where certain type of data can be found into the enterprise folder (LDP container).

The semantics of the Solid Type Indexes applies. 

```http
GET /myEnterprise/typeIndex HTTP/1.1
Host: example.org
Accept: application/ld+json
```

Here is an example of a complete type index:
```http
HTTP/1.1 200 OK
Content-Type: application/ld+json

{
    "@context": "https://www.datafoodconsortium.org",
    "@base": "https://example.org/myEnterprise/",
    "@graph": [
        {
            "@id": "/myEnterprise/typeIndex",
            "@type": "solid:TypeIndex"
        },
        {
            "@id": "#functionalProducts",
            "@type": "solid:TypeRegistration",
            "solid:forClass": "dfc:FunctionalProduct",
            "solid:instance": "./products"
        },
        {
            "@id": "#technicalProducts",
            "@type": "solid:TypeRegistration",
            "solid:forClass": "dfc:TechnicalProduct",
            "solid:instance": "./products"
        },
        {
            "@id": "#suppliedProducts",
            "@type": "solid:TypeRegistration",
            "solid:forClass": "dfc:SuppliedProduct",
            "solid:instance": "./products"
        },
        {
            "@id": "#localizedProducts",
            "@type": "solid:TypeRegistration",
            "solid:forClass": "dfc:LocalizedProduct",
            "solid:instance": "./products"
        },
        {
            "@id": "#physicalProducts",
            "@type": "solid:TypeRegistration",
            "solid:forClass": "dfc:PhysicalProduct",
            "solid:instance": "./products"
        },
        {
            "@id": "#persons",
            "@type": "solid:TypeRegistration",
            "solid:forClass": "dfc:Person",
            "solid:instanceContainer": "./thirdParties/"
        },
        {
            "@id": "#enterprises",
            "@type": "solid:TypeRegistration",
            "solid:forClass": "dfc:Enterprise",
            "solid:instanceContainer": "./thirdParties/"
        },
        {
            "@id": "#catalogs",
            "@type": "solid:TypeRegistration",
            "solid:forClass": "dfc:Catalog",
            "solid:instanceContainer": "./catalogs/"
        },
        {
            "@id": "#customerCategories",
            "@type": "solid:TypeRegistration",
            "solid:forClass": "dfc:CustomerCategory",
            "solid:instance": "./persons"
        },
        {
            "@id": "#saleSessions",
            "@type": "solid:TypeRegistration",
            "solid:forClass": "dfc:SaleSession",
            "solid:instanceContainer": "./saleSessions/"
        },
        {
            "@id": "#orders",
            "@type": "solid:TypeRegistration",
            "solid:forClass": "dfc:Order",
            "solid:instanceContainer": "./orders/"
        }
    ]
}
```

# Shape

```
:EnterpriseShape a sh:NodeShape;
    sh:targetClass dfc-b:Enterprise;
    sh:closed true;

    sh:property [
		sh:path dfc-b:name;
        sh:message "An enterprise must have a name.";
        sh:datatype xsd:string;
        sh:count 1;
	];

    sh:property [
		sh:path dfc-b:hasAddress;
        sh:datatype dfc-b:Address;
        sh:message "An enterprise must have at least one address.";
        sh:minCount 1;
    ];

    sh:property [
		sh:path dfc-b:description;
        sh:message "An enterprise can have one description text.";
        sh:datatype xsd:string;
        sh:maxCount 1;
    ];

    sh:property [
		sh:path dfc-b:VATnumber;
        sh:message "An enterprise can have at most 1 VAT number.";
        sh:datatype xsd:string;
        sh:maxCount 1;
    ];

    sh:property [
		sh:path dfc-b:hasMainContact;
        sh:datatype dfc-b:Person;
        sh:maxCount 1;
    ];

    sh:property [
		sh:path dfc-b:requests;
        sh:message "An enterprise can request one or several functional products.";
        sh:datatype dfc-b:FunctionalProduct;
    ];

    sh:property [
		sh:path dfc-b:proposes;
        sh:message "An enterprise can propose one or several technical products.";
        sh:datatype dfc-b:TechnicalProduct;
    ];

    sh:property [
		sh:path dfc-b:supplies;
        sh:datatype dfc-b:SuppliedProduct;
    ];

    sh:property [
		sh:path dfc-b:maintains;
        sh:datatype dfc-b:Catalog;
    ];

    sh:property [
		sh:path dfc-b:manages;
        sh:datatype dfc-b:CatalogItem;
    ];

    sh:property [
		sh:path dfc-b:affiliates;
        sh:datatype dfc-b:Person;
    ];

    sh:property [
		sh:path dfc-b:defines;
        sh:datatype dfc-b:CustomerCategory;
    ];

    sh:property [
		sh:path dfc-b:coordinatedBy;
        sh:datatype dfc-b:Coordination;
    ];

    sh:property [
		sh:path dfc-b:orders;
        sh:message "Any order placed by the enterprise.";
        sh:datatype dfc-b:Order;
    ];

    sh:property [
		sh:path dfc-b:transforms;
        sh:datatype dfc-b:AsPlannedLocalTransformation;
    ];
```

# Examples

```json
{
    "@context": "https://www.datafoodconsortium.org",
    "@base": "https://example.org/myEnterprise/",
    "@graph": [
        {
            "@id": "./index#this",
            "@type": "dfc-b:Enterprise",
            "dfc-b:name": "My enterprise",
            "dfc-b:maintains": "./catalogs/catalog1/index#this"
        }
        {
            "@id": "./catalogs/catalog1/index#item1",
            "@type": "dfc-b:CatalogItem",
            "dfc-b:name": "Tomato",
            "dfc-b:references": "./products#tomato",
            "dfc-b:offeredThrough": "./catalogs/catalog1/index#offer1"
        },
        {
            "@id": "./products#tomato",
            "@type": "dfc-b:SuppliedProduct",
            "dfc-b:name": "Tomato"
        },
        {
            "@id": "./catalogs/catalog1/index#offer1",
            "@type": "dfc-b:Offer",
        }
    ]
}
```

# Products

# Catalogs

# Sale sessions

# Orders

## Placed orders

## Received orders

# Indexes

```json
{
    "@context": "https://www.datafoodconsortium.org",
    "@graph": [
        {
            "@id": "https://example.org/enterprise/index#this",
            "@type": "dfc-b:Enterprise",
            "dfc-b:name": "My enterprise",
            "dfc-b:hasTypeIndex": "./typeIndex",
            "dfc-b:placedOrderByDateIndex": "",
            "dfc-b:receivedOrderByDateIndex": "",
            "dfc-b:saleSessionByDateIndex": ""
        }
    ]
}
```

# Discovery

The RDF class used to register an instance of a catalog is `dfc-b:Enterprise`.

Type Indexes may be used with that class to register enterprises of a storage.

To make a link to an enterprise, one does not have to be the owner of it. The access control does not have to match: it is possible to have a private link to a public resource and vice-versa. 