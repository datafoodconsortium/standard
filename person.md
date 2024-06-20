A person is...

This document describes the structure of the data of a person.

# Structure

A person is an object of the class `dfc-b:Person`. The person data is a set of RDF triples - linked data links. When reading a person, where there are links, those must be followed, and no assumptions made about the URIs.

A person is all built inside a single folder (LDP Container). Let us call the URI of that folder -- less its final '/' -- $ROOT. It could be say, https://example.org/somePerson. Developers must never make assumptions about where an enterprise is stored as persons are used within other apps which create the container for them.

For instance, a request to get the person folder at https://example.org/somePerson/:
```http
GET /somePerson/ HTTP/1.1
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
    "@base": "https://example.org/somePerson/",
    "@graph": [
        {
            "@id": "./",
            "@type": "ldp:BasicContainer",
            "ldp:contains": [
                "./index",
                "./orders/"
            ]
        }
    ]
}
```

Within that folder the person object is normally defined at $ROOT/index#this. This document is a WebId. The person object MUST be of type `dfc-b:Person` and the `dfc-b:name` property MUST be valued. Other properties can be defined like listed in the person shape below.

A request to get the root document of the person https://example.org/somePerson/index#this:
```http
GET /somePerson/index HTTP/1.1
Host: example.org
Accept: application/ld+json
```

Should respond something like:

```http
HTTP/1.1 200 OK
Content-Type: application/ld+json

{
    "@context": "https://www.datafoodconsortium.org",
    "@base": "https://example.org/somePerson/",
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
                "foaf:Person",
                "dfc-b:Person"
            ],
            "dfc-b:name": "Georges Brassens",
            "dfc-b:firstName": "Georges",
            "dfc-b:familyName": "Brassens",
            "solid:publicTypeIndex": "",
            "solid:preferencesFile": "",
            "rdfs:seeAlso": "",
            "owl:sameAs": ""
        }
    ]
}
```

# Orders

The orders placed by the person MUST be stored in the $ROOT/orders folder (LPD container). This folder MUST contain an index document that contains the orders identifier.

# Shape

```
:PersonShape a sh:NodeShape;
    sh:targetClass dfc-b:Person;
    sh:closed true;

    sh:property [
		sh:path dfc-b:name;
        sh:message "A person must have a name.";
        sh:datatype xsd:string;
        sh:count 1;
	];

    sh:property [
		sh:path dfc-b:firstName;
        sh:message "A person must have a first name.";
        sh:datatype xsd:string;
        sh:count 1;
	];

    sh:property [
		sh:path dfc-b:familyName;
        sh:message "A person must have a last name.";
        sh:datatype xsd:string;
        sh:count 1;
	];

    sh:property [
		sh:path dfc-b:hasAddress;
        sh:datatype dfc-b:Address;
        sh:message "A person must have at least one address.";
        sh:minCount 1;
    ];

    sh:property [
		sh:path dfc-b:description;
        sh:message "A person can have one description text.";
        sh:datatype xsd:string;
        sh:maxCount 1;
    ];

    sh:property [
		sh:path dfc-b:affiliatedBy;
        sh:datatype dfc-b:Enterprise;
    ];

    sh:property [
		sh:path dfc-b:orders;
        sh:message "Any order placed by the person.";
        sh:datatype dfc-b:Order;
    ];

    sh:property [
		sh:path dfc-b:requests;
        sh:message "A person can request one or several functional products.";
        sh:datatype dfc-b:FunctionalProduct;
    ];
```