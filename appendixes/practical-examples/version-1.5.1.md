# version 1.5.1

## Changes log from version 1.5

* @base and @id respect W3C json-ld standard

## GET User Data

```javascript
{
  "@context": {
    "dfc-b": "http://static.datafoodconsortium.org/ontologies/dfc_FullModel.owl#",
    "dfc-p": "http://static.datafoodconsortium.org/ontologies/DFC_ProductOntology.owl#",
    "dfc-u": "http://static.datafoodconsortium.org/data/units.rdf#",
    "dfc-pt": "http://static.datafoodconsortium.org/data/types.rdf#",
    "dfc-p:hasUnit":{
      "@type":"@id"
    },
    "dfc-p:hasType":{
      "@type":"@id"
    },
    "dfc-b:references":{
      "@type":"@id"
    },
    "dfc-b:offeredThroug":{
      "@type":"@id"
    },
    "dfc-b:offeredTo":{
      "@type":"@id"
    },
    "@base": "http://maPlateformeNationale/"
  },
  "@id": "person/personId",
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
      "@id": "entreprise/entrepriseId",
      "@type": "dfc-b:Entreprise",
      "dfc-b:VATnumber": "",
      "dfc-b:defines": [
        {
          "@id": "customerCategory/customerCategoryId1",
          "@type": "dfc-b:CustomerCategory",
          "dfc-b:description": "member"
        },
        {
          "@id": "customerCategory/customerCategoryId2",
          "@type": "dfc-b:CustomerCategory",
          "dfc-b:description": "non member"
        }
      ],
      "dfc-b:supplies": [
        {
          "@id": "suppliedProduct/item3",
          "@type": "dfc-b:SuppliedProduct",
          "dfc-p:hasUnit": "dfc-u:u",
          "dfc-b:quantity": "99.99",
          "dfc-p:hasType": "dfc-pt:egg",
          "dfc-b:description": "supply description 1",
          "dfc-b:totalTheoriticalStock": "999",
          "dfc-b:brand": "supply brand",
          "dfc-b:claim": "supply claim",
          "dfc-b:image": "supply image url",
          "dfc-b:lifeTime": "supply lifeTime",
          "dfc-b:physicalCharacterisctics": "supply physical characterisctics"
        },
        {
          "@id": "suppliedProduct/item4",
          "@type": "dfc-b:SuppliedProduct",
          "dfc-p:hasUnit": "dfc-u:u",
          "dfc-b:quantity": "1",
          "dfc-p:hasType": "dfc-pt:strawberry",
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
          "@id": "catalogItem/catalogItemId1",
          "@type": "dfc-b:CatalogItem",
          "dfc-b:references": "/suppliedProduct/item3",
          "dfc-b:sku": "catalog item gtin or sku",
          "dfc-b:stockLimitation": "999",
          "dfc-b:offeredThrough": [
            {
              "@id": "offer/offerId1",
              "@type": "dfc-b:Offer",
              "dfc-b:offeresTo": "/customerCategory/customerCategoryId1",
              "dfc-b:price": "0",
              "dfc-b:stockLimitation": "999"
            },
            {
              "@id": "offer/offerId2",
              "@type": "dfc-b:Offer",
              "dfc-b:offeresTo": "/customerCategory/customerCategoryId2",
              "dfc-b:price": "999",
              "dfc-b:stockLimitation": "999"
            }
          ]
        },
        {
          "@id": "catalogItem/catalogItemId2",
          "@type": "dfc-b:CatalogItem",
          "dfc-b:sku": "catalog item gtin or sku",
          "dfc-b:stockLimitation": "999",
          "dfc-b:references": "/suppliedProduct/item4",
          "dfc-b:offeredThrough": [
            {
              "@id": "offer/offerId3",
              "@type": "dfc-b:Offer",
              "dfc-b:offeresTo": "/customerCategory/customerCategoryId1",
              "dfc-b:price": "000",
              "dfc-b:stockLimitation": "999"
            },
            {
              "@id": "offer/offerId4",
              "@type": "dfc-b:Offer",
              "dfc-b:offeresTo": "/customerCategory/customerCategoryId2",
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

