# version 1.3

### Changes log from version 1.2

We made the following changes from version 1.2:

* Support multiple enterprises for logged-in user
  * In fact, we recentered the data graph root on user
* Use the catalog item
  * The Catalog Item allows us to list all the Defined Products \(Supplied et Tecnical\) on a uniq entreprise.
  * It is possible to add an extra layer between Catalog Item and Entreprise called Repository. It can be used to break down the general catalog into multiple ones.
* Add offers that are linked to the catalog item

## GET Catalogue

```javascript
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

