---
description: >-
  DFC Protocol is defined here but one specific API is requiered on Platform .
  In addition DFC server provides some sepecific API.
---

# Specifics API

## Platform

### User

Platform have to provide an url to obtain data about user thanks to OIDC Authentification. All users data must be return by the API and platform have to take OIDC Token into account to identify and authentify user.  Those data contains sufficient data to get all other data linked to user as product, offers and all class defined in [Business Ontology](../semantic-specifications/business-ontology.md).

This API have to implement [technical specification](./)  (LDP, OIDC,Ontologies...). [Example](../appendixes/practical-examples/version-1.5.1.md) API is designed for this use.

Platform have to contact DFC to add this url to be inclued in the Consortium.&#x20;

## Prototype

### link

Main usage of DFC server is to provide identifier reconciliation of platforms included in consortium. A non LDP API provides this feature to platform : "What are identifiers (uri) of data in others platforme from my uri".

```
https://server/link/myPlatformDataUri
```

Result of this API use [Technical Ontology](../semantic-specifications/technical-ontology.md) whith pivot.&#x20;

```
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

### linkSimple

This more compact and more Simple API compute link API to standard sameAs predicates.

```
https://server/linkSimple/myPlatformDataUri
```

```
{
    @id : myPlatformDataUri,
    owl:sameAs : [
        otherPlatformDataUri1,
        otherPlatformDataUri2
    ]
}
```

### my-data

Return prototype data of a platform data. owl:sameAs added to platform provided data

```
https://server/my-data/myPlatformDataUri
```

```
{
	@id": myPlatformDataUri,
	predicate : value
	owl:sameAs: [
	        otherPlatformDataUri1,
	        otherPlatformDataUri2
	]
```
