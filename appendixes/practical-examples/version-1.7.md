# version 1.7



### Changes log from version 1.6.1

* replace raw context by inline (optional)
* add and replace properties of ontology V?
  * facet refactor
  * characteristic refactor
* focus to supplied product instead catalogItem (same ontology, just an other perspective)
* hasType in usinees ontology and not product ontology
* hasUnit in usinees ontology and not product ontology

### GET User Data

* @base and @id respect W3C json-ld standard

```javascript
  {
  "@context":   [
    "http://static.datafoodconsortium.org/ontologies/context.json",
    {
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
      "dfc-b:description": "La ferme beausoleil",
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
      ],
      "dfc-b:owns": [
        "brand/brand1",
      ]
    },
    {
      "@id": "customerCategory/customerCategoryId1",
      "@type": "dfc-b:CustomerCategory",
      "dfc-b:description": "particulier"
    },
    {
      "@id": "customerCategory/customerCategoryId1",
      "@type": "dfc-b:CustomerCategory",
      "dfc-b:description": "particulier"
    },
    {
      "@id": "brand/brand1",
      "@type": "dfc-b:Brand",
      "dfc-b:description": "ma marque"
    },
    {
      "@id": "suppliedProduct/item3",
      "@type": "dfc-b:SuppliedProduct",
      "dfc-b:description": "Magnifique lot de 6 yaourts",
      "dfc-b:hasType": "dfc-pt:cow-dairy-product",
      "dfc-b:referencedBy":[
          "/catalogItem/catalogItemId1"
      ],
      "dfc-b:hasQuantity":{
        "@type":"dfc-b:QuantitativeValue",
        "dfc-b:hasUnit": "dfc-u:u",
        "dfc-b:value": "1",
      },
      "dfc-b:specificCondition": "Conserver au frais",
      "dfc-b:totalTheoriticalStock": "999",
      "dfc-b:hasBrand":"brand/brand1",
      "dfc-b:hasCertification":[
        "dfc-c:OrganicLabel"
      ],
      "dfc-b:hasClaim":[
        "dfc-a:nutriScore"
      ],
      "dfc-b:image": "https://cdn.pixabay.com/photo/2013/07/13/09/50/yoghurt-156133_960_720.png",
      "dfc-b:lifeTime": "supply lifeTime",
      "dfc-b:hasAllergenCharacteristic":[
        {
          "@type":"dfc-b:AllergenCharacteristic",
          "dfc-b:hasUnit": "dfc-u:Percent",
          "dfc-b:hasAllergenDimension": "dfc-d:LactoseMilks",
          "dfc-b:value": "90"
        }
      ],
      "dfc-b:hasNutrimentCharacteristic":[
        {
          "@type":"dfc-b:NutrimentCharacteristic",
          "dfc-b:hasUnit": "dfc-u:Percent",
          "dfc-b:hasNutrimentDimension": "dfc-d:Fat",
          "dfc-b:value": "6,2"
        }
      ],
      "dfc-b:hasPhysicalCharacteristic":[
        {
          "@type":"dfc-b:PhysicalCharacteristic",
          "dfc-b:hasUnit": "dfc-u:Centimetre",
          "dfc-b:hasPhysicalDimension": "dfc-d:Weight",
          "dfc-b:value": "300"
        },
        {
          "@type":"dfc-b:PhysicalCharacteristic",
          "dfc-b:hasUnit": "dfc-u:Centimetre",
          "dfc-b:hasPhysicalDimension": "dfc-d:Height",
          "dfc-b:value": "10"
        },
        {
          "@type":"dfc-b:PhysicalCharacteristic",
          "dfc-b:hasUnit": "dfc-u:Centimetre",
          "dfc-b:hasPhysicalDimension": "dfc-d:Width",
          "dfc-b:value": "30"
        },
        {
          "@type":"dfc-b:PhysicalCharacteristic",
          "dfc-b:hasUnit": "dfc-u:Centimetre",
          "dfc-b:hasPhysicalDimension": "dfc-d:Depth",
          "dfc-b:value": "20"
        }        
      ],
      "dfc-b:hasGeographicalOrigin":"dfc-g:france"
    },
   {
      "@id": "suppliedProduct/item4",
      "@type": "dfc-b:SuppliedProduct",
      "dfc-b:referencedBy":[
          "/catalogItem/catalogItemId2"
      ],
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
      "dfc-b:sku": "123456789",
      "dfc-b:stockLimitation": "999",
      "dfc-b:offeredThrough": [
          "offer/offerId1"
      ]
    },
    {
      "@id": "catalogItem/catalogItemId2",
      "@type": "dfc-b:CatalogItem",
      "dfc-b:sku": "catalog item gtin or sku",
      "dfc-b:stockLimitation": "999",
      "dfc-b:offeredThrough": [
        "offer/offerId3",
        "offer/offerId4"
      ]
    },
    {
      "@id": "offer/offerId1",
      "@type": "dfc-b:Offer",
      "dfc-b:offeres": "/customerCategory/customerCategoryId1",
      "dfc-b:price": "2",
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
