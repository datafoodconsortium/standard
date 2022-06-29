# version 1.6.2



### Changes log from version 1.6.1

* compose context by remote official context combine with @base

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
