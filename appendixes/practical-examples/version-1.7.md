# version 1.7

### Changes log from version 1.6.2

* replace raw context by inline (optional)
* add and replace properties of ontology V?
  * facet refactor
  * characteristic refactor
* focus to supplied product instead catalogItem (same ontology, just an other perspective)
* hasType in Businees ontology and not product ontology
* hasUnit in Businees ontology and not product ontology

## GET User Data

```javascript
  {
  "@context": [
      "http://static.datafoodconsortium.org/ontologies/context.json",
      {
        "@base":"http://maPlateformeNationale/"
      }
    ],
  "@graph" : [
    {
     "@id": "person/personId",
      "@type": "dfc-b:Person",
      "dfc-b:familyName": "Doe",
      "dfc-b:firstName": "Jhon",
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
      "@id": "brand/brandId1",
      "@type": "dfc-b:Brand",
      "dfc-b:description": "brand description 1",
      "rdfs:label":"label brand 1"
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
      "dfc-b:hasQuantity":{
          "@type":"dfc-b:QuantitiveValue",
          "dfc-b:hasUnit":"dfc-m:Gram",
          "dfc-b:value":"1000"
      },
      "dfc-p:hasType": "dfc-pt:egg",
      "dfc-b:description": "supply description 1",
      "dfc-b:totalTheoriticalStock": "999",
      "dfc-b:hasBrand": "brand/brandId1",
      "dfc-b:hasCertification": "dfc-f:Organic-AB",
      "dfc-b:hasClaim": [
          "dfc-f:NoAddedSugars",
          "dfc-f:LowSodiumSalt",
          "dfc-f:IncreasedNutrient"
        ],
      "dfc-b:hasNutrientCharacteristic": {
          "@type":"dfc-b:NutrientCharacteristic",
          "dfc-b:hasNutrientDimension":"dfc-m:Magnesium",
          "dfc-b:hasUnit":"dfc-m:Gram",
          "dfc-b:value":"10"
      },
      "dfc-b:hasPhysicalCharacteristic": {
          "@type":"dfc-b:PhysicalCharacteristic",
          "dfc-b:hasPhysicalDimension":"dfc-m:Weight",
          "dfc-b:hasUnit":"dfc-m:Gram",
          "dfc-b:value":"100"
      },
      "dfc-b:hasAllergenCharacteristic": {
          "@type":"dfc-b:AllergenCharacteristic",
          "dfc-b:hasAllergenDimension":"dfc-m:allergenCharacteristic1",
          "dfc-b:hasUnit":"dfc-m:Allenit1",
          "dfc-b:value":"1"
      },
      "dfc-b:hasGeographicalOrigin": "dfc-f:CentreValLoire",
      "dfc-b:hasNatureOrigin": "dfc-f:PlantOrigin",
      "dfc-b:hasPartOrigin": "dfc-f:PlantPartOrigin",
      "dfc-b:hasProcess": [
        "suppliedProduct/item4"
      ],
      "dfc-b:image": "supply image url",
      "dfc-b:lifeTime": "supply lifeTime",
    },
   {
      "@id": "suppliedProduct/item4",
      "@type": "dfc-b:SuppliedProduct",
      "dfc-b:hasQuantity":{
          "@type":"dfc-b:QuantitiveValue",
          "dfc-b:hasUnit":"dfc-m:Gram",
          "dfc-b:value":"1000"
      },
      "dfc-p:hasType": "dfc-pt:strawberry",
      "dfc-b:description": "supply description 2",
      "dfc-b:totalTheoriticalStock": "999",
      "dfc-b:image": "supply image url",
      "dfc-b:lifeTime": "supply lifeTime",
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
