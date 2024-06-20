The enterprise is the entity that gather information about a particular individual producer or a collective organization. An enterprise may produce some products, publish some catalogs and may allow people to buy products during a sale session.

This document describes the structure of the data in a enterprise.

# Structure

An enterprise is an object of the class `dfc-b:Enterprise`. The enterprise data is a set of RDF triples - linked data links. When reading an enterprise, where there are links, those must be followed, and no assumptions made about the URIs.

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
                "./products/",
                "./catalogs/"
            ]
        }
    ]
}
```

Within that folder the enterprise object is normally defined at $ROOT/index#this. This document is a WebId. The enterprise object MUST be of type `dfc-b:Enterprise` and the `dfc-b:name` property MUST be valued. Other properties can be defined like listed in the enterprise shape below.

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
            "@id": "./index",
            "@type": "foaf:personalProfileDocument",
            "foaf:primaryTopic": "./index#this"
        },
        {
            "@id": "./index#this",
            "@type": [
                "foaf:Agent",
                "foaf:Organization",
                "dfc-b:Enterprise"
            ],
            "dfc-b:name": "My enterprise",
            "dfc-b:hasAddress": {
                "@type": "dfc-b:Address",
                "dfc-b:country": "France"
            },
            "dfc-b:VATnumber": "12346789",
            "solid:publicTypeIndex": "",
            "solid:preferencesFile": "",
            "rdfs:seeAlso": "",
            "owl:sameAs": ""
        }
    ]
}
```

# Products

The products of the entreprise MUST be stored in the $ROOT/products folder (LPD container). This folder MUST contain an index document that contains the products identifier.

Getting the products folder:
```http
GET /myEnterprise/products/ HTTP/1.1
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
    "@base": "https://example.org/myEnterprise/products/",
    "@graph": [
        {
            "@id": "./",
            "@type": "ldp:BasicContainer",
            "ldp:contains": [
                "./index"
            ]
        }
    ]
}
```

Within the products folder there MUST be an index document. This document defines products identifier which will be used by other objects like catalogs. This document can provide all the information of products or only a part. Additional information could be gave in other documents using the `rdf:seeAlso` predicate. This is particulary useful when some information must be access restricted. For instance, the index document could be public and links to other protected document for certain details about the products.

Getting the index document of the products folder:
```http
GET /myEnterprise/products/index HTTP/1.1
Host: example.org
Accept: application/ld+json
```

Should respond something like:
```http
HTTP/1.1 200 OK
Content-Type: application/ld+json

{
    "@context": "www.datafoodconsortium.org",
    "@base": "https://example.org/myEnterprise/products/index",
    "@graph": [
        {
            "@id": ".",
            "@type": "dfc-b:ProductBook"
        },
        {
            "@id": "#p1",
            "@type": "dfc-b:SuppliedProduct",
            "dfc-b:name": "Tomato",
            "rdfs:seeAlso": "./protected"
        },
        {
            "@id": "#p2",
            "rdfs:seeAlso": "./protected" 
        }
    ]
}
```

The two products https://example.org/myEnterprise/products/index#p1 and https://example.org/myEnterprise/products/index#p2.

Getting a protected access product document:
```http
GET /myEnterprise/products/protected HTTP/1.1
Host: example.org
Accept: application/ld+json
```

Should respond something like:
```http
HTTP/1.1 200 OK
Content-Type: application/ld+json

{
    "@context": "www.datafoodconsortium.org",
    "@base": "https://example.org/myEnterprise/products/index",
    "@graph": [
        {
            "@id": "#p1",
            "dfc-b:description": "Description is access protected."
        },
        {
            "@id": "#p2",
            "@type": "dfc-b:SuppliedProduct",
            "dfc-b:name": "My product",
            "dfc-b:description": "Description is access protected." 
        }
    ]
}
```

# Catalogs

The catalogs of the entreprise MUST be stored in the $ROOT/catalogs folder (LPD container). This folder MUST contain an index document that contains the catalogs identifier.

Getting the catalogs folder:
```http
GET /myEnterprise/catalogs/ HTTP/1.1
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
    "@base": "https://example.org/myEnterprise/catalogs/",
    "@graph": [
        {
            "@id": "./",
            "@type": "ldp:BasicContainer",
            "ldp:contains": [
                "./index"
            ]
        }
    ]
}
```

Within the catalogs folder there MUST be an index document. This document defines catalogs identifier which will be used by other objects like offers. This document can provide all the information of catalogs or only a part. Additional information could be gave in other documents using the rdf:seeAlso predicate. This is particulary useful when some information must be access restricted. For instance, the index document could be public and links to other protected document for certain details about the catalogs.

Getting the index document of the catalogs folder:
```http
GET /myEnterprise/catalogs/index HTTP/1.1
Host: example.org
Accept: application/ld+json
```

Should respond something like:
```http
HTTP/1.1 200 OK
Content-Type: application/ld+json

{
    "@context": "www.datafoodconsortium.org",
    "@base": "https://example.org/myEnterprise/catalogs/index",
    "@graph": [
        {
            "@id": ".",
            "@type": "dfc-b:CatalogBook"
        },
        {
            "@id": "#catalog1",
            "@type": "dfc-b:Catalog",
            "dfc-b:name": "My catalog",
            "rdfs:seeAlso": "./protected"
        },
        {
            "@id": "#catalog2",
            "rdfs:seeAlso": "./protected" 
        }
    ]
}
```

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
            "@id": "https://example.org/myEnterprise/index#this",
            "@type": "dfc-b:Enterprise",
            "dfc-b:name": "My enterprise",
            "dfc-b:placedOrderByDateIndex": "",
            "dfc-b:receivedOrderByDateIndex": "",
            "dfc-b:saleSessionByDateIndex": ""
        }
    ]
}
```

# Access control

The access control for an enterprise must be set to reflect the user's wishes. One can recognize three roles: the owner of the enterprise who creates it, the salaries who are allowed to write to it, and the viewers who are allowed to read it. 

# Discovery

The RDF class used to register an instance of a catalog is `dfc-b:Enterprise`.

Type Indexes may be used with that class to register enterprises of a storage.

To make a link to an enterprise, one does not have to be the owner of it. The access control does not have to match: it is possible to have a private link to a public resource and vice-versa. 


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
