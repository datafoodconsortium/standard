# version 1.4

## Changes log from version 1.3

* cahnges prefix according to ontology
* rdfs:label was replaced by dfc-b description on CutomerCategory
* units were added
* context on @id was made more easier

## GET Catalogue

```javascript
{
  "@context": {
    "dfc-b": "http://datafoodconsortium.org/ontologies/dfc_FullModel.owl#",
    "dfc-u": "http://datafoodconsortium.org/data/units.rdf#",
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
          "@type": "dfc-b:SuppliedProduct",
          "dfc-b:hasUnit": "dfc-u:u",
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
          "@type": "dfc-b:SuppliedProduct",
          "dfc-b:hasUnit": "dfc-u:u",
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

