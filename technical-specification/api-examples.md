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

### version 1.4
#### version 1.3 upgrade
* préfix différent selon la partie de l'ontologie importée
* rdfs:label replace by dfc-b description in CutomerCatgory
* unité issue du referenceiel
* contexte plus explicite sur le type @id pour dimplifier les datas

#### example
```json
{
  "@context": {
    "dfc-b": "http://datafoodconsortium.org/ontologies/dfc_FullModel.owl#",
    "dfc-p": "http://datafoodconsortium.org/ontologies/dfc_ProductGlossary.owl#",
    "dfc-b:hasUnit":{
      "@type":"@id"
    },
    "dfc-b:references":{
      "@type":"@id"
    },
    "dfc-b:offeresTo":{
      "@type":"@id"
    },
    "@base": "http://maPlateformeNationale"
  },
  "@id": "/personId",
  "@type": "dfc-b:Person",
  "dfc-b:familyName": "Doe",
  "dfc-b:firtsName": "Jhon",
  "dfc-b:hasAdress": {
    "@type": "dfc-b:Address",
    "dfc-b:city": "",
    "dfc-b:country": "",
    "dfc-b:postcode": "",
    "dfc-b:street": ""
  },
  "dfc-b:affiliates": [
    {
      "@id": "/entrepriseId",
      "@type": "dfc-b:Entreprise",
      "dfc-b:VATnumber": "",
      "dfc-b:defines": [
        {
          "@id": "/customerCategoryId1",
          "@type": "dfc-b:CustomerCategory",
          "dfc-b:description": "member"
        },
        {
          "@id": "/customerCategoryId2",
          "@type": "dfc-b:CustomerCategory",
          "dfc-b:description": "non member"
        }
      ],
      "dfc-b:supplies": [
        {
          "@id": "/suppliedProduct/item3",
          "dfc-b:hasUnit": "dfc-p:u",
          "dfc-b:quantity": "99.99",
          "dfc-b:description": "supply description 1",
          "dfc-b:totalTheoriticalStock": "999",
          "dfc-b:brand": "supply brand",
          "dfc-b:claim": "supply claim",
          "dfc-b:image": "supply image url",
          "dfc-b:lifeTime": "supply lifeTime",
          "dfc-b:physicalCharacterisctics": "supply physical characterisctics"
        },
        {
          "@id": "/suppliedProduct/item4",
          "dfc-b:hasUnit": "dfc-p:u",
          "dfc-b:quantity": "1",
          "dfc-b:description": "supply description 2",
          "dfc-b:totalTheoriticalStock": "999",
          "dfc-b:brand": "supply brand",
          "dfc-b:claim": "supply claim",
          "dfc-b:image": "supply image url",
          "dfc-b:lifeTime": "supply lifeTime",
          "dfc-b:physicalCharacterisctics": "supply physical characterisctics"
        }
      ],
      "dfc-b:manages": [
        {
          "@id": "/catalogItemId1",
          "@type": "dfc-b:CatalogItem",
          "dfc-b:references": "/suppliedProduct/item3",
          "dfc-b:sku": "catalog item gtin or sku",
          "dfc-b:stockLimitation": "999",
          "dfc-b:offeredThrough": [
            {
              "@id": "offerId1",
              "@type": "dfc-b:Offer",
              "dfc-b:offeresTo": "/customerCategoryId1",
              "dfc-b:price": "0",
              "dfc-b:stockLimitation": "999"
            },
            {
              "@id": "offerId2",
              "@type": "dfc-b:Offer",
              "dfc-b:offeresTo": "/customerCategoryId2",
              "dfc-b:price": "999",
              "dfc-b:stockLimitation": "999"
            }
          ]
        },
        {
          "@id": "/catalogItemId2",
          "@type": "dfc-b:CatalogItem",
          "dfc-b:sku": "catalog item gtin or sku",
          "dfc-b:stockLimitation": "999",
          "dfc-b:references": "/suppliedProduct/item4",
          "dfc-b:offeredThrough": [
            {
              "@id": "offerId3",
              "@type": "dfc-b:Offer",
              "dfc-b:offeresTo": "/customerCategoryId1",
              "dfc-b:price": "000",
              "dfc-b:stockLimitation": "999"
            },
            {
              "@id": "offerId4",
              "@type": "dfc-b:Offer",
              "dfc-b:offeresTo": "/customerCategoryId2",
              "dfc-b:price": "999",
              "dfc-b:stockLimitation": "999"
            }
          ]
        }
      ]
    }
  ]
}
```

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
  "dfc:familyName": "Doe",
  "dfc:firtsName": "Jhon",
  "dfc:hasAdress": {
    "@type": "dfc:Address",
    "dfc:city": "",
    "dfc:country": "",
    "dfc:postcode": "",
    "dfc:street": ""
  },
  "dfc:affiliates": [
    {
      "@id": "/entrepriseId",
      "@type": "dfc:Entreprise",
      "dfc:VATnumber": "",
      "dfc:defines": [
        {
          "@id": "/customerCategoryId1",
          "@type": "dfc:CustomerCategory",
          "rdfs:label": "member"
        },
        {
          "@id": "/customerCategoryId2",
          "@type": "dfc:CustomerCategory",
          "rdfs:label": "non member"
        }
      ],
      "dfc:supplies": [
        {
          "@id": "/suppliedProduct/item3",
          "dfc:hasUnit": {
            "@id": "/unit/kg",
            "rdfs:label": "kilogram"
          },
          "dfc:quantity": "99.99",
          "dfc:description": "supply description 1",
          "dfc:totalTheoriticalStock": "999",
          "dfc:brand": "supply brand",
          "dfc:claim": "supply claim",
          "dfc:image": "supply image url",
          "lifeTime": "supply lifeTime",
          "dfc:physicalCharacterisctics": "supply physical characterisctics"
        },
        {
          "@id": "/suppliedProduct/item4",
          "dfc:hasUnit": {
            "@id": "/unit/unit",
            "rdfs:label": "unit"
          },
          "dfc:quantity": "1",
          "dfc:description": "supply description 2",
          "dfc:totalTheoriticalStock": "999",
          "dfc:brand": "supply brand",
          "dfc:claim": "supply claim",
          "dfc:image": "supply image url",
          "lifeTime": "supply lifeTime",
          "dfc:physicalCharacterisctics": "supply physical characterisctics"
        }
      ],
      "dfc:manages": [
        {
          "@id": "/catalogItemId1",
          "@type": "dfc:CatalogItem",
          "dfc:references": {
            "@type": "@id",
            "@id": "/suppliedProduct/item3"
          },
          "dfc:sku": "catalog item gtin or sku",
          "dfc:stockLimitation": "999",
          "dfc:offeredThrough": [
            {
              "@id": "offerId1",
              "@type": "dfc:Offer",
              "dfc:offeresTo": {
                "@type": "@id",
                "@id": "/customerCategoryId1"
              },
              "dfc:price": "0",
              "dfc:stockLimitation": "999"
            },
            {
              "@id": "offerId2",
              "@type": "dfc:Offer",
              "dfc:offeresTo": {
                "@type": "@id",
                "@id": "/customerCategoryId2"
              },
              "dfc:price": "999",
              "dfc:stockLimitation": "999"
            }
          ]
        },
        {
          "@id": "/catalogItemId2",
          "@type": "dfc:CatalogItem",
          "dfc:sku": "catalog item gtin or sku",
          "dfc:stockLimitation": "999",
          "dfc:references": {
            "@type": "@id",
            "@id": "/suppliedProduct/item4"
          },
          "dfc:offeredThrough": [
            {
              "@id": "offerId3",
              "@type": "dfc:Offer",
              "dfc:offeresTo": {
                "@type": "@id",
                "@id": "/customerCategoryId1"
              },
              "dfc:price": "000",
              "dfc:stockLimitation": "999"
            },
            {
              "@id": "offerId4",
              "@type": "dfc:Offer",
              "dfc:offeresTo": {
                "@type": "@id",
                "@id": "/customerCategoryId2"
              },
              "dfc:price": "999",
              "dfc:stockLimitation": "999"
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
