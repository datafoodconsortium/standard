# version 1.7.4

### Changes log from version 1.7.1

* change context
* fix dfc-p obsolet prefix to dfc-b for hasType predicate

## GET User Data

```json
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
      "dfc-b:firstName": "John",
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
      "dfc-b:description": "brand description 1"
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
      "dfc-b:name": "Farmers Eggs",
      "dfc-b:hasQuantity":{
          "@type":"dfc-b:QuantitiveValue",
          "dfc-b:hasUnit":"dfc-m:Gram",
          "dfc-b:value":"1000"
      },
      "dfc-b:hasType": "dfc-pt:egg",
      "dfc-b:description": "20 fresh eggs",
      "dfc-b:totalTheoriticalStock": "999",
      "dfc-b:hasBrand": "brand/brandId1",
      "dfc-b:hasCertification": "dfc-f:Organic-AB",
      "dfc-b:hasClaim": [
          "dfc-f:NoAddedSugars",
          "dfc-f:LowSodiumSalt",
          "dfc-f:IncreasedNutrient"
        ],
      "dfc-b:hasNutrientCharacteristic": [
          {
              "@type":"dfc-b:NutrientCharacteristic",
              "dfc-b:hasNutrientDimension":"dfc-m:Magnesium",
              "dfc-b:hasUnit":"dfc-m:Gram",
              "dfc-b:value":"10"
          },
          {
              "@type":"dfc-b:NutrientCharacteristic",
              "dfc-b:hasNutrientDimension":"dfc-m:Salt",
              "dfc-b:hasUnit":"dfc-m:Gram",
              "dfc-b:value":"0.01"
          }
      ],
      "dfc-b:hasPhysicalCharacteristic": [
          {
          "@type":"dfc-b:PhysicalCharacteristic",
          "dfc-b:hasPhysicalDimension":"dfc-m:Weight",
          "dfc-b:hasUnit":"dfc-m:Gram",
          "dfc-b:value":"100"
          },
          {
          "@type":"dfc-b:PhysicalCharacteristic",
          "dfc-b:hasPhysicalDimension":"dfc-m:Width",
          "dfc-b:hasUnit":"dfc-m:Centimetre",
          "dfc-b:value":"4.5"
          }
      ],
      "dfc-b:hasAllergenCharacteristic": {
          "@type":"dfc-b:AllergenCharacteristic",
          "dfc-b:hasAllergenDimension":"dfc-m:Eggs",
          "dfc-b:hasUnit":"dfc-m:Unit",
          "dfc-b:value":"1"
      },
      "dfc-b:hasGeographicalOrigin": "dfc-f:CentreValLoire",
      "dfc-b:hasNatureOrigin": "dfc-f:PlantOrigin",
      "dfc-b:hasPartOrigin": "dfc-f:PlantPartOrigin",
      "dfc-b:hasProcess": [
        "suppliedProduct/item4"
      ],
      "dfc-b:image": "supply image url",
      "dfc-b:lifeTime": "15 days"
    },
   {
      "@id": "suppliedProduct/item4",
      "@type": "dfc-b:SuppliedProduct",
      "dfc-b:name": "Plougastel's Strawberries",
      "dfc-b:hasQuantity":{
          "@type":"dfc-b:QuantitiveValue",
          "dfc-b:hasUnit":"dfc-m:Gram",
          "dfc-b:value":"1000"
      },
      "dfc-b:hasType": "dfc-pt:strawberry",
      "dfc-b:description": "Strawberries coming from Andrew's farm.",
      "dfc-b:totalTheoriticalStock": "999",
      "dfc-b:image": "supply image url",
      "dfc-b:lifeTime": "4 days"
    },
    {
      "@id": "catalogItem/catalogItemId1",
      "@type": "dfc-b:CatalogItem",
      "dfc-b:references": "/suppliedProduct/item3",
      "dfc-b:sku": "27.208.7.40.",
      "dfc-b:stockLimitation": "999",
      "dfc-b:offeredThrough": [
          "offer/offerId1",
          "offer/offerId2"
      ]
    },
    {
      "@id": "catalogItem/catalogItemId2",
      "@type": "dfc-b:CatalogItem",
      "dfc-b:sku": "27.208.7.41.",
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
      "dfc-b:hasPrice": {
        "@type": "dfc-b:Price",
        "dfc-b:value": "10.2",
        "dfc-b:VATrate": "5",
        "dfc-b:hasUnit": "dfc-m:Euro"
      },
      "dfc-b:stockLimitation": "999"
    },
    {
      "@id": "offer/offerId2",
      "@type": "dfc-b:Offer",
      "dfc-b:offeres": "/customerCategory/customerCategoryId2",
      "dfc-b:hasPrice": {
        "@type": "dfc-b:Price",
        "dfc-b:value": "8.67",
        "dfc-b:VATrate": "5",
        "dfc-b:hasUnit": "dfc-m:Euro"
      },
      "dfc-b:stockLimitation": "999"
    },
    {
      "@id": "offer/offerId3",
      "@type": "dfc-b:Offer",
      "dfc-b:offeres": "/customerCategory/customerCategoryId1",
      "dfc-b:hasPrice": {
        "@type": "dfc-b:Price",
        "dfc-b:value": "12.4",
        "dfc-b:VATrate": "5",
        "dfc-b:hasUnit": "dfc-m:Euro"
      },
      "dfc-b:stockLimitation": "999"
    },
    {
      "@id": "offer/offerId4",
      "@type": "dfc-b:Offer",
      "dfc-b:offeres": "/customerCategory/customerCategoryId2",
      "dfc-b:hasPrice": {
        "@type": "dfc-b:Price",
        "dfc-b:value": "22",
        "dfc-b:VATrate": "5",
        "dfc-b:hasUnit": "dfc-m:Euro"
      },
      "dfc-b:stockLimitation": "999"
    }
  ]
}
```
