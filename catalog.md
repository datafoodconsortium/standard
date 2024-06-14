
# Abstract

A catalog is an object which holds a collection of products that is shown to clients and prospects. A catalog is usualy created and maintained by producers or by people responsible of the distribution. Then a catalog is shared to its audience who can view the different items in the catalog and buy some products if the catalog contains some offers and the used application provides the feature.

# Catalog

This document describes the structure of the data in a catalog, the main class of which is `dfc-b:Catalog`. The catalog data is a set of RDF triples - linked data links. When reading a catalog, where there are links, those must be followed, and no assumptions made about the URIs.

A catalog is all built inside a single folder (LDP Container). Let us call the URI of that folder -- less its final '/' -- $ROOT. It could be say, https://example.org/catalog1. Developers must never make assumptions about where a catalog is stored as catalogs are used within other apps which create the container for them.

Within that folder, the catalog object is normally defined at $ROOT/index#this. There MUST be only one `dfc-b:Catalog` instance in a catalog.

Below is an example of a catalog where all the data is contained in a single document:
```json
{
    "@context": "https://www.datafoodconsortium.org",
    "@graph": [
        {
            "@id": "https://example.org/catalog1/index#this",
            "@type": "dfc-b:Catalog",
            "dfc-b:name": "Main catalog",
            "dfc-b:maintainedBy": "https://example.org/enterprise/index#this",
            "dfc-b:lists": [
                "https://example.org/catalog1/index#item1"
            ]
        },
        {
            "@id": "https://example.org/enterprise/index#this",
            "@type": "dfc-b:Enterprise",
            "dfc-b:name": "Name of the farm",
            "dfc-b:maintains": "https://example.org/catalog1/index#this"
        }
        {
            "@id": "https://example.org/catalog1/index#item1",
            "@type": "dfc-b:CatalogItem",
            "dfc-b:name": "Tomato",
            "dfc-b:references": "https://example.org/enterprise/products#tomato",
            "dfc-b:offeredThrough": "https://example.org/catalog1/index#offer1"
        },
        {
            "@id": "https://example.org/enterprise/products#tomato",
            "@type": "dfc-b:SuppliedProduct",
            "dfc-b:name": "Tomato"
        },
        {
            "@id": "https://example.org/catalog1/index#offer1",
            "@type": "dfc-b:Offer",
        }
    ]
}
```

```json
{
    "@context": "https://www.datafoodconsortium.org",
    "@graph": [
        {
            "@id": "./index#this",
            "@type": "dfc-b:Catalog",
            "dfc-b:name": "Main catalog",
            "dfc-b:maintainedBy": "../../index#this",
            "dfc-b:lists": [
                "./#item1"
            ]
        },
        {
            "@id": "./index#item1",
            "@type": "dfc-b:CatalogItem",
            "dfc-b:name": "Tomato",
            "dfc-b:references": "../../products#tomato",
            "dfc-b:offeredThrough": "./index#offer1"
        },
        {
            "@id": "../../products#tomato",
            "@type": "dfc-b:SuppliedProduct",
            "dfc-b:name": "Tomato"
        },
        {
            "@id": "./index#offer1",
            "@type": "dfc-b:Offer",
        }
    ]
}
```

# Discovery

The RDF class used to register an instance of a catalog is `dfc-b:Catalog`.

Type Indexes may be used with that class to register catalogs of a storage.

To make a link to a catalog, one does not have to be the owner of it. The access control does not have to match: it is possible to have a private link to a public resource and vice-versa. 