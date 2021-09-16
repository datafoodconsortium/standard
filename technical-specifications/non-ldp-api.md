---
description: >-
  DFC Protocol is defined here but one specific API is requiered on Platform .
  In addition DFC server provides some sepecific API.
---

# Specifics API

## Platform

### User

Platform have to provide an url to obtain data about user thanks to OIDC Authentification. All users must be return by the sale API and pltform have to take OIDC Token into account to identify identify which user is concerned.  Those data contains sufficient data to get all other data linked to user as product, offers and all class defined in [Business Ontology](../semantic-specifications/business-ontology.md).

This API have to implement [technical specification](./)  \(LDP, OIDC,Ontologies...\). [Example](../appendixes/practical-examples/version-1.5.1.md) API is designed for this use.

Platform have to contact DFC to add this url to be inclued in the Consortium. 

## Prototype

### Pivot

Main usage of DFC server is to provide identifier reconciliation of platforms included in consortium. A non LDP API provides this feature to platform : "What are identifiers \(uri\) of data in others platforme from my uri".

```text
https://server/pivot?uri=myPlatformDataUri
```

Result of this API use [Technical Ontology](../semantic-specifications/technical-ontology.md) whith pivot. 

```text
{
    @id : myPlatformDataUri,
    host : {
        @id : myPlatformUri,
        label : my platform
    }
    pivot : {
        @id : pivotUri,
        represents : [
            {
                @id : otherPlatformDataUri1,
                host : {
                    @id : otherPlatformUri1,
                    label : other platform 1
                }
            },
            {
                @id : otherPlatformDataUri2,
                host : {
                    @id : otherPlatformUri2,
                    label : other platform 2
                }
            }
        ]
    }
}
```

### Same As

This more compact and more Simple API compute Pivot API to standard sameAs predicates.

```text
https://server/sameAs?uri=myPlatformDataUri
```

```text
{
    @id : myPlatformDataUri,
    sameAs : [
        otherPlatformDataUri1,
        otherPlatformDataUri2
    ]
}
```

