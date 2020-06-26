# Exemples d'usage du standard

## Get Data linked to Connected Users
* No spécification for url syntaxe
* Request have to fill Authorization Header with JWT OIDC Token obtain thanks to login.lescommuns.org
* Basic authentification
  * Server have to validate token thanks to signature and parse token to obtain preferred_username.
  * This preferred_username can be linked to the Platform User. To Link OIDC user and Platform User, the platforms can add OIDC authentification feature.
* Outdated Token
  * If token is outdated, plateform can refuse request.
  * If platform have to request an other (DFC or Other) and receive outaded input token, Platform can remember refresh token (when OIDC authentication featur execution) and ask new access to do out request.

### version 1.3

#### version 1.2 upgrade

* supporter plusieurs entreprises par user connecté
  * Cela revient a recentrer la racine du graph retourné sur le user
* Utilisation du Catalog Item
  * Le Catalog Item permet de lister dans un seul prédicat de l'Entreprise tous les Defined Product (Supplied et Tecnical)
  * Il et possible de rajouter un intermédiare entre CatalogItem et Entreprise qui est le Repository pour découper le catalogue global en plusieurs.
* Ajout de la notion d'offre lié au Catalog Item

#### example

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

### version 1.2

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
