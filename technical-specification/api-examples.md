# Exemples d'usage du standard

## récupérer le catalogue V1.2

supposition : une seul entreprise par user connecté (OIDC)

```json
{
  "@context": {
    "dfc": "http://datafoodconsortium.org/ontologies/dfc_FullModel.owl#",
    "@base": "http://maPlateformeNationale"
  },
  "@id": "/entreprise/maGrandeEntreprise",
  "@type": "dfc:Entreprise",
  "dfc:supplies":[
    {
      "@id":"/suppliedProduct/item3",
      "dfc:hasUnit":{
        "@id":"/unit/kg"
      },
      "dfc:quantity":"0,5",
      "dfc:description":"Aillet botte 1 pièce"
    },
    {
      "@id":"/suppliedProduct/item4",
      "dfc:hasUnit":{
        "@id":"/unit/unit"
      },
      "dfc:quantity":"1",
      "dfc:description":"Aromates-Romarin Botte 1 pièce"
    }
  ]
}
```

## supporter plusieurs entreprises par user connecté

Cela revient a recentrer la racine du graph retourné sur le user

```json
{
  "@context": {
    "dfc": "http://datafoodconsortium.org/ontologies/dfc_FullModel.owl#",
    "@base": "http://maPlateformeNationale"
  },
  "@id": "/personId",
  "@type": "dfc:Person",
  "dfc:affiliates" : [
    {
      "@id": "/entrepriseId",
      "@type": "dfc:Entreprise",
      "dfc:supplies":[
        {
          "@id":"/suppliedProduct/item3",
          "dfc:hasUnit":{
            "@id":"/unit/kg"
          },
          "dfc:quantity":"0,5",
          "dfc:description":"Aillet botte 1 pièce"
        },
        {
          "@id":"/suppliedProduct/item4",
          "dfc:hasUnit":{
            "@id":"/unit/unit"
          },
          "dfc:quantity":"1",
          "dfc:description":"Aromates-Romarin Botte 1 pièce"
        }
      ]
    }
  ]
}
```

## Utilisation du Catalog Item

Catalog Item à été créé pour faire face au fait que Simon ne voyait pas comment une entreprise pouvait disposer d'un catalogue . Cela est en fait possible en cumulant les relations dfc:supplies>dfc:SuppliedProduct, dfc:proposes>dfc:TechnicalProduct et dfc:request>dfc:FunctionnalProduct
La nouvelle Ontologie Technique à réglé toutes les autre problématiques qui pouvaient être lié au plateformes.

Il et possible de rajouter un intermédiare entre CatalogItem et Entreprise qui est le Repository pour découper le catalogue global en plusieurs.

Ce CatalogItem ne semble plus indispensable

```json
{
  "@context": {
    "dfc": "http://datafoodconsortium.org/ontologies/dfc_FullModel.owl#",
    "@base": "http://maPlateformeNationale"
  },
  "@id": "/personId",
  "@type": "dfc:Person",
  "dfc:affiliates" : [
    {
      "@id": "/entrepriseId",
      "@type": "dfc:Entreprise",
      "dfc:supplies":[
        {
          "@id":"/suppliedProduct/item3",
          "dfc:hasUnit":{
            "@id":"/unit/kg"
          },
          "dfc:quantity":"0,5",
          "dfc:description":"Aillet botte 1 pièce"
        },
        {
          "@id":"/suppliedProduct/item4",
          "dfc:hasUnit":{
            "@id":"/unit/unit"
          },
          "dfc:quantity":"1",
          "dfc:description":"Aromates-Romarin Botte 1 pièce"
        }
      ],
      "dfc:manages":[
        {
          "@id":"/catalogItemId1",
          "@type":"dfc:CatalogItem",
          "dfc:references":{
            "@type":"@id",
            "@id":"/suppliedProduct/item3"
          }
        },
        {
          "@id":"/catalogItemId2",
          "@type":"dfc:CatalogItem",
          "dfc:references":{
            "@type":"@id",
            "@id":"/suppliedProduct/item4"
          }
        }
      ]
    }
  ]
}
```

## Ajout de l'offre

```json
{
  "@context": {
    "dfc": "http://datafoodconsortium.org/ontologies/dfc_FullModel.owl#",
    "@base": "http://maPlateformeNationale"
  },
  "@id": "/personId",
  "@type": "dfc:Person",
  "dfc:affiliates" : [
    {
      "@id": "/entrepriseId",
      "@type": "dfc:Entreprise",
      "dfc:defines" :[
        {
          "@id":"/customerCategoryId1",
          "@type":"dfc:CustomerCategory",
          "rdfs:label":"membre"
        },
        {
          "@id":"/customerCategoryId2",
          "@type":"dfc:CustomerCategory",
          "rdfs:label":"non membre"
        }
      ],
      "dfc:supplies":[
        {
          "@id":"/suppliedProduct/item3",
          "dfc:hasUnit":{
            "@id":"/unit/kg"
          },
          "dfc:quantity":"0,5",
          "dfc:description":"Aillet botte 1 pièce"
        },
        {
          "@id":"/suppliedProduct/item4",
          "dfc:hasUnit":{
            "@id":"/unit/unit"
          },
          "dfc:quantity":"1",
          "dfc:description":"Aromates-Romarin Botte 1 pièce"
        }
      ],
      "dfc:manages":[
        {
          "@id":"/catalogItemId1",
          "@type":"dfc:CatalogItem",
          "dfc:references":{
            "@type":"@id",
            "@id":"/suppliedProduct/item3"
          },
          "dfc:offeredThrough":[
            {
              "@id":"offerId1",
              "@type":"dfc:Offer",
              "dfc:offeresTo":{
                "@type":"@id",
                "@id":"/customerCategoryId1"
              },
              "dfc:price":"000"
            },
            {
              "@id":"offerId2",
              "@type":"dfc:Offer",
              "dfc:offeresTo":{
                "@type":"@id",
                "@id":"/customerCategoryId2",
              },
              "dfc:price":"999"
            }
          ]
        },
        {
          "@id":"/catalogItemId2",
          "@type":"dfc:CatalogItem",
          "dfc:references":{
            "@type":"@id",
            "@id":"/suppliedProduct/item4"
          },
          "dfc:offeredThrough":[
            {
              "@id":"offerId3",
              "@type":"dfc:Offer",
              "dfc:offeresTo":{
                "@type":"@id",
                "@id":"/customerCategoryId1"
              },
              "dfc:price":"000"
            },
            {
              "@id":"offerId4",
              "@type":"dfc:Offer",
              "dfc:offeresTo":{
                "@type":"@id",
                "@id":"/customerCategoryId2",
              },
              "dfc:price":"999"
            }
          ]
        }
      ]
    }
  ]
}
```

## Simplification en supprimant le Catalog Item

en supposant que nous supprimions le Catalog Item et que Offer possede les propriété que possédait Catalog Item

### 1er version

dans une logique centré sur l'offre

```json
{
  "@context": {
    "dfc": "http://datafoodconsortium.org/ontologies/dfc_FullModel.owl#",
    "@base": "http://maPlateformeNationale"
  },
  "@id": "/personId",
  "@type": "dfc:Person",
  "dfc:affiliates" : [
    {
      "@id": "/entrepriseId",
      "@type": "dfc:Entreprise",
      "dfc:defines" :[
        {
          "@id":"/customerCategoryId1",
          "@type":"dfc:CustomerCategory",
          "rdfs:label":"membre"
        },
        {
          "@id":"/customerCategoryId2",
          "@type":"dfc:CustomerCategory",
          "rdfs:label":"non membre"
        }
      ],
      "dfc:supplies":[
        {
          "@id":"/suppliedProduct/item3",
          "dfc:hasUnit":{
            "@id":"/unit/kg"
          },
          "dfc:quantity":"0,5",
          "dfc:description":"Aillet botte 1 pièce"
        },
        {
          "@id":"/suppliedProduct/item4",
          "dfc:hasUnit":{
            "@id":"/unit/unit"
          },
          "dfc:quantity":"1",
          "dfc:description":"Aromates-Romarin Botte 1 pièce"
        }
      ],
      "dfc:manages":[
        {
          "@id":"offerId1",
          "@type":"dfc:Offer",
          "dfc:offeresTo":{
            "@type":"@id",
            "@id":"/customerCategoryId1"
          },
          "dfc:references":{
            "@type":"@id",
            "@id":"/suppliedProduct/item3"
          },
          "dfc:price":"000"
        },
        {
          "@id":"offerId2",
          "@type":"dfc:Offer",
          "dfc:offeresTo":{
            "@type":"@id",
            "@id":"/customerCategoryId2",
          },
          "dfc:references":{
            "@type":"@id",
            "@id":"/suppliedProduct/item3"
          },
          "dfc:price":"999"
        },
        {
          "@id":"offerId3",
          "@type":"dfc:Offer",
          "dfc:offeresTo":{
            "@type":"@id",
            "@id":"/customerCategoryId1"
          },
          "dfc:references":{
            "@type":"@id",
            "@id":"/suppliedProduct/item4"
          },
          "dfc:price":"000"
        },
        {
          "@id":"offerId4",
          "@type":"dfc:Offer",
          "dfc:offeresTo":{
            "@type":"@id",
            "@id":"/customerCategoryId2",
          },
          "dfc:references":{
            "@type":"@id",
            "@id":"/suppliedProduct/item4"
          },
          "dfc:price":"999"
        }
      ]
    }
  ]
}
```

### 2eme version

parfaitement identique conceptuellement mais centré sur les Defined Products

```json
{
  "@context": {
    "dfc": "http://datafoodconsortium.org/ontologies/dfc_FullModel.owl#",
    "@base": "http://maPlateformeNationale"
  },
  "@id": "/personId",
  "@type": "dfc:Person",
  "dfc:affiliates" : [
    {
      "@id": "/entrepriseId",
      "@type": "dfc:Entreprise",
      "dfc:defines" :[
        {
          "@id":"/customerCategoryId1",
          "@type":"dfc:CustomerCategory",
          "rdfs:label":"membre"
        },
        {
          "@id":"/customerCategoryId2",
          "@type":"dfc:CustomerCategory",
          "rdfs:label":"non membre"
        }
      ],
      "dfc:supplies":[
        {
          "@id":"/suppliedProduct/item3",
          "dfc:hasUnit":{
            "@id":"/unit/kg"
          },
          "dfc:quantity":"0,5",
          "dfc:description":"Aillet botte 1 pièce",
          "dfc:offeredThrough":[
            {
              "@id":"offerId1",
              "@type":"dfc:Offer",
              "dfc:offeresTo":{
                "@type":"@id",
                "@id":"/customerCategoryId1"
              },
              "dfc:price":"000"
            },
            {
              "@id":"offerId2",
              "@type":"dfc:Offer",
              "dfc:offeresTo":{
                "@type":"@id",
                "@id":"/customerCategoryId2",
              },
              "dfc:price":"999"
            }
          ]
        },
        {
          "@id":"/suppliedProduct/item4",
          "dfc:hasUnit":{
            "@id":"/unit/unit"
          },
          "dfc:quantity":"1",
          "dfc:description":"Aromates-Romarin Botte 1 pièce",
          "dfc:offeredThrough":[
            {
              "@id":"offerId3",
              "@type":"dfc:Offer",
              "dfc:offeresTo":{
                "@type":"@id",
                "@id":"/customerCategoryId1",
              },
              "dfc:price":"000"
            },
            {
              "@id":"offerId4",
              "@type":"dfc:Offer",
              "dfc:offeresTo":{
                "@type":"@id",
                "@id":"/customerCategoryId2",
              },
              "dfc:price":"999"
            }
          ]
        }
      ]
    }
  ]
}
```
