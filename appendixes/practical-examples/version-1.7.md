# version 1.7

## Changes log from version 1.6

* replace raw context by inline (optional)
* add and replace properties of ontology V?
  * facet refactor
  * characteristic refactor
* focus to supplied product instead catalogItem (same ontology, just an other perspective)


## GET User Data

* @base and @id respect W3C json-ld standard

```javascript
{
  "@context": "http://static.datafoodconsortium.org/ontologies/context.json",
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
      "dfc-b:referencedBy":[
          "/catalogItem/catalogItemId1"
      ], # Ontology not ready
      "dfc-b:hasQuantity":{
        "@type":"dfc-b:QuantitativeValue",
        "dfc-b:hasUnit": "dfc-u:u",
        "dfc-b:value": "99.99",
      },
      "dfc-p:hasType": "dfc-pt:egg",
      "dfc-b:description": "supply description 1",
      "dfc-b:totalTheoriticalStock": "999",
      "dfc-b:hasBrand":[
        {
          "@type":"dfc-p:Brand",
          "dfc-p:description": "supply brand" # Ontology not ready; identifier could specifiy external subject
        }
      ],
      "dfc-b:hasClaim":[
        {
          "@type":"dfc-p:NutritionalClaim",
          "dfc-p:description": "supply claim" # Ontology not ready; identifier could specifiy external subject
        }
      ],
      "dfc-b:image": "supply image url",
      "dfc-b:lifeTime": "supply lifeTime",
      "dfc-b:hasCharacteristic":[
        {
          "@type":"dfc-b:NutrientCharacteristic", # scope/file could be ProductGlosssary
          "dfc-b:hasUnit": "dfc-u:u",
          "dfc-b:value": "nutriment value" # identifier could specifiy external subject
        },
        {
          "@type":"dfc-b:AllergenCharacteristic", scope/file could be ProductGlosssary
          "dfc-b:hasUnit": "dfc-u:u",
          "dfc-b:value": "alergen value" # identifier could specifiy external subject
        },
        {
          "@type":"dfc-b:PhysicalCharacteristic", scope/file could be ProductGlosssary
          "dfc-b:hasUnit": "dfc-u:u",
          "dfc-b:value": "physical value" # identifier could specifiy external subject
        }
      ]
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
