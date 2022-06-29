# version 1.6.1



### Changes log from version 1.6

* context fix
* @base and @id respect W3C json-ld standard

## GET User Data

```javascript
  {
    "@context": {
      "dfc": "http://static.datafoodconsortium.org/ontologies/DFC_FullModel.owl#",
      "dfc-b": "http://static.datafoodconsortium.org/ontologies/DFC_BusinessOntology.owl#",
      "dfc-p": "http://static.datafoodconsortium.org/ontologies/DFC_ProductOntology.owl#",
      "dfc-t": "http://static.datafoodconsortium.org/ontologies/DFC_TechnicalOntology.owl#",
      "dfc-u": "http://static.datafoodconsortium.org/data/units.rdf#",
      "dfc-pt": "http://static.datafoodconsortium.org/data/productTypes.rdf#",
      "dfc-a": "http://static.datafoodconsortium.org/data/claims.rdf#",
      "dfc-d": "http://static.datafoodconsortium.org/data/dimensions.rdf#",
      "dfc-c": "http://static.datafoodconsortium.org/data/certifications.rdf#",
      "dfc-g": "http://static.datafoodconsortium.org/data/geoOrigin.rdf#",
      "dfc-p:hasUnit":{
        "@type":"@id"
      },
      "dfc-b:hasUnit":{
        "@type":"@id"
      },
      "dfc-p:hasType":{
        "@type":"@id"
      },
      "dfc-b:hasType":{
        "@type":"@id"
      },
      "dfc-b:references":{
        "@type":"@id"
      },
      "dfc-b:referencedBy":{
        "@type":"@id"
      },
      "dfc-b:offeres":{
        "@type":"@id"
      },
      "dfc-b:supplies":{
        "@type":"@id"
      },
      "dfc-b:defines":{
        "@type":"@id"
      },
      "dfc-b:affiliates":{
        "@type":"@id"
      },
      "dfc-b:hasQuantity":{
        "@type":"@id"
      },
      "dfc-b:manages":{
        "@type":"@id"
      },
      "dfc-b:offeredThrough":{
        "@type":"@id"
      },
      "dfc-b:hasBrand":{
        "@type":"@id"
      },
      "dfc-b:hasGeographicalOrigin":{
        "@type":"@id"
      },
      "dfc-b:hasClaim":{
        "@type":"@id"
      },
      "dfc-b:hasAllergenDimension":{
        "@type":"@id"
      },
      "dfc-b:hasNutrimentDimension":{
        "@type":"@id"
      },
      "dfc-b:hasPhysicalDimension":{
        "@type":"@id"
      },
      "dfc:owner":{
        "@type":"@id"
      },
      "dfc-t:hostedBy":{
        "@type":"@id"
      },
      "dfc-t:hasPivot":{
        "@type":"@id"
      },
      "dfc-t:represent":{
        "@type":"@id"
      },
      "@base": "http://maPlateformeNationale/"
    },
    "@graph" : [
      {
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
          "entreprise/entrepriseId"
        ]
      },
      {
        "@id": "entreprise/entrepriseId",
        "@type": "dfc-b:Entreprise",
        "dfc-b:VATnumber": "",
        "dfc-b:defines": [
        "customerCategory/customerCategoryId1",
        "customerCategory/customerCategoryId2"
      ],
      "dfc-b:supplies": [
        "suppliedProduct/item3",
        "suppliedProduct/item4"
      ],
      "dfc-b:manages": [
        "catalogItem/catalogItemId1",
        "catalogItem/catalogItemId2"
      ]
    },
    {
      "@id": "customerCategory/customerCategoryId1",
      "@type": "dfc-b:CustomerCategory",
      "dfc-b:description": "member"
    },
    {
      "@id": "customerCategory/customerCategoryId2",
      "@type": "dfc-b:CustomerCategory",
      "dfc-b:description": "non member"
    },
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
    },
    {
      "@id": "catalogItem/catalogItemId1",
      "@type": "dfc-b:CatalogItem",
      "dfc-b:references": "/suppliedProduct/item3",
      "dfc-b:sku": "catalog item gtin or sku",
      "dfc-b:stockLimitation": "999",
      "dfc-b:offeredThrough": [
          "offer/offerId1",
          "offer/offerId2"
      ]
    },
    {
      "@id": "catalogItem/catalogItemId2",
      "@type": "dfc-b:CatalogItem",
      "dfc-b:sku": "catalog item gtin or sku",
      "dfc-b:stockLimitation": "999",
      "dfc-b:references": "/suppliedProduct/item4",
      "dfc-b:offeredThrough": [
        "offer/offerId3",
        "offer/offerId4"
      ]
    },
    {
      "@id": "offer/offerId1",
      "@type": "dfc-b:Offer",
      "dfc-b:offeres": "/customerCategory/customerCategoryId1",
      "dfc-b:price": "0",
      "dfc-b:stockLimitation": "999"
    },
    {
      "@id": "offer/offerId2",
      "@type": "dfc-b:Offer",
      "dfc-b:offeres": "/customerCategory/customerCategoryId2",
      "dfc-b:price": "999",
      "dfc-b:stockLimitation": "999"
    },
    {
      "@id": "offer/offerId3",
      "@type": "dfc-b:Offer",
      "dfc-b:offeres": "/customerCategory/customerCategoryId1",
      "dfc-b:price": "000",
      "dfc-b:stockLimitation": "999"
    },
    {
      "@id": "offer/offerId4",
      "@type": "dfc-b:Offer",
      "dfc-b:offeres": "/customerCategory/customerCategoryId2",
      "dfc-b:price": "999",
      "dfc-b:stockLimitation": "999"
    }
  ]
}
```
