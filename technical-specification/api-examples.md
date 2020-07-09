# Practical Examples

## Get Data linked to Connected Users

* No specification for url syntaxe
* Request have to fill Authorization Header with JWT OIDC Token obtain thanks to login.lescommuns.org
* Basic authentification
  * Server has to validate token thanks to signature and parse token to obtain preferred\_username.
  * This preferred\_username can be linked to the Platform User. To Link OIDC user and Platform User, platforms can add OIDC authentification feature.
* Outdated Token
  * If token is outdated, platforms can refuse request.
  * If platforms have to request another \(DFC or Other\) and receive outaded input token, Platforms can remember refresh token \(when OIDC authentication features execution\) and ask new access to do out request.

### version 1.3

#### version 1.2 upgrade

We made the following changes from version 1.2:

* Support several enterprises per logged-in user
  * _Cela revient a recentrer la racine du graph retourné sur le user_
* Use the catalog item
  * Le Catalog Item permet de lister dans un seul prédicat de l'Entreprise tous les Defined Product \(Supplied et Tecnical\)
  * Il et possible de rajouter un intermédiare entre CatalogItem et Entreprise qui est le Repository pour découper le catalogue global en plusieurs.
* Add offers that are linked to the catalog item

#### example

```javascript
{
  "@context": {
    "dfc": "http://datafoodconsortium.org/ontologies/dfc_FullModel.owl#",
    "@base": "http://maPlateformeNationale"
  },
  "@id": "/personId",
  "@type": "dfc:Person",
  "dfc:familyName":"Doe",
  "dfc:firtsName":"Jhon",
  "dfc:hasAdress":{
    "@type":"dfc:Address",
    "dfc:city":"",
    "dfc:country":"",
    "dfc:postcode":"",
    "dfc:street":""
  },
  "dfc:affiliates" : [
    {
      "@id": "/entrepriseId",
      "@type": "dfc:Entreprise",
      "dfc:VATnumber":"",
      "dfc:defines" :[
        {
          "@id":"/customerCategoryId1",
          "@type":"dfc:CustomerCategory",
          "rdfs:label":"member"
        },
        {
          "@id":"/customerCategoryId2",
          "@type":"dfc:CustomerCategory",
          "rdfs:label":"non member"
        }
      ],
      "dfc:supplies":[
        {
          "@id":"/suppliedProduct/item3",
          "dfc:hasUnit":{
            "@id":"/unit/kg",
            "rdfs:label":"kilogram"
          },
          "dfc:quantity":"99.99",
          "dfc:description":"supply description 1",
          "dfc:totalTheoriticalStock":"999",
          "dfc:brand":"supply brand",
          "dfc:claim":"supply claim",
          "dfc:image":"supply image url",
          "lifeTime":"supply lifeTime",
          "dfc:physicalCharacterisctics":"supply physical characterisctics",
          "dfc:quantity":"supply quantity"
        },
        {
          "@id":"/suppliedProduct/item4",
          "dfc:hasUnit":{
            "@id":"/unit/unit",
            "rdfs:label":"unit"
          },
          "dfc:quantity":"1",
          "dfc:description":"supply description 2",
          "dfc:totalTheoriticalStock":"999",
          "dfc:brand":"supply brand",
          "dfc:claim":"supply claim",
          "dfc:image":"supply image url",
          "lifeTime":"supply lifeTime",
          "dfc:physicalCharacterisctics":"supply physical characterisctics",
          "dfc:quantity":"supply quantity"
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
          "dfc:sku":"catalog item gtin or sku",
          "dfc:stockLimitation":"999",
          "dfc:offeredThrough":[
            {
              "@id":"offerId1",
              "@type":"dfc:Offer",
              "dfc:offeresTo":{
                "@type":"@id",
                "@id":"/customerCategoryId1"
              },
              "dfc:price":"000",
              "dfc:stockLimitation":"999",
            },
            {
              "@id":"offerId2",
              "@type":"dfc:Offer",
              "dfc:offeresTo":{
                "@type":"@id",
                "@id":"/customerCategoryId2",
              },
              "dfc:price":"999",
              "dfc:stockLimitation":"999",
            }
          ]
        },
        {
          "@id":"/catalogItemId2",
          "@type":"dfc:CatalogItem",
          "dfc:sku":"catalog item gtin or sku",
          "dfc:stockLimitation":"999",
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
              "dfc:price":"000",
              "dfc:stockLimitation":"999",
            },
            {
              "@id":"offerId4",
              "@type":"dfc:Offer",
              "dfc:offeresTo":{
                "@type":"@id",
                "@id":"/customerCategoryId2",
              },
              "dfc:price":"999",
              "dfc:stockLimitation":"999",
            }
          ]
        }
      ]
    }
  ]
}
```

### version 1.2

In version 1.2 we had only one enterprise per logged in user:

```javascript
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

