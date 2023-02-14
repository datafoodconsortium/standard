# Version 1.8

### Changes log from version 1.7

* Change range of relation concerns : DefinedProduct -> Offer&#x20;
* Add of discount dataproperty on Offer, Order and OrderLine
* Rename Repository to Catalog
* Remove DataProperty for social media (Facebook, Instagram, Twitter and Linkedin) and add Concept SocialMedia
* Add of Concept PhoneNumber with a phone number and a phone code
* Add of missing properties : affiliates, offeredTo
* Add of Price on OrderLine
* Add of date on Order

## GET User Data

```
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
          "dfc-b:street": "",
          "dfc-b:longitude": "",
          "dfc-b:latitude": ""
        },
        "dfc-b:hasPhoneNumber": {
          "@type": "dfc-b:PhoneNumber",
          "dfc-b:phoneNumber": "",
          "dfc-b:countryCode": ""
        },
        "dfc-b:hasSocialMedia": {
          "@type": "dfc-b:SocialMedia",
          "dfc-b:name": ""
        },
        "dfc-b:affiliatesBy": [
          "entreprise/entrepriseId"
        ],
        "dfc-b:orders [
          "order/orderId1"
        ]
      },

      {
        "@id": "entreprise/entrepriseId",
        "@type": "dfc-b:Entreprise",
        "dfc-b:VATnumber": "",
        "dfc-b:affiliates": [
          "person/personId"
        ]
        
        "dfc-b:defines": [
          "customerCategory/customerCategoryId1",
          "customerCategory/customerCategoryId2"
        ],
        "dfc-b:supplies": [
          "suppliedProduct/item1",
          "suppliedProduct/item2"
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
        "dfc-b:description":"label brand 1"
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
        "@id": "suppliedProduct/item1",
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
            "dfc-b:hasUnit":"dfc-u:Gram",
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
          "suppliedProduct/item2"
        ],
        "dfc-b:image": "supply image url",
        "dfc-b:lifeTime": "supply lifeTime"
        
        "dfc-b:referencedBy": "catalogItem/catalogItemId1"
      },
      {
        "@id": "suppliedProduct/item2",
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
        
        "dfc-b:referencedBy": "catalogItem/catalogItemId2"
      },


      {
        "@id": "catalogItem/catalogItemId1",
        "@type": "dfc-b:CatalogItem",
        "dfc-b:sku": "catalog item gtin or sku",
        "dfc-b:stockLimitation": "999",
        "dfc-b:references": "/suppliedProduct/item1",
        "dfc-b:listedIn" : "catalog/catalog",
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
        "dfc-b:references": "/suppliedProduct/item2",
        "dfc-b:listedIn" : "catalog/catalog",
        "dfc-b:offeredThrough": [
          "offer/offerId3",
          "offer/offerId4"
        ]
      },
      {
        "@id": "catalog/catalog",
        "@type": "dfc-b:Catalog",
        "dfc-b:lists" : [
            "catalogItem/catalogItemId1",
            "catalogItem/catalogItemId2"
        ]
      }

      {
        "@id": "offer/offerId1",
        "@type": "dfc-b:Offer",
        "dfc-b:offeredTo": "/customerCategory/customerCategoryId1",
        "dfc-b:stockLimitation": "999",
        "dfc-b:hasPrice": {
            "@type": "dfc-b:Price",
            "dfc-b:value": "10",
            "dfc-b:VATrate": "5",
            "dfc-b:hasUnit": "dfc-m:Euro"
        },

        "dfc-b:listedIn": [
            "salessession/salessessionId1"
        ],
        "dfc-b:offers : catalogItem/catalogItemId1
      },
      {
        "@id": "offer/offerId2",
        "@type": "dfc-b:Offer",
        "dfc-b:offeredTo": "/customerCategory/customerCategoryId2",
        "dfc-b:price": "999",
        "dfc-b:stockLimitation": "999",
        "dfc-b:hasPrice": {
            "@type": "dfc-b:Price",
            "dfc-b:value": "10",
            "dfc-b:VATrate": "5",
            "dfc-b:hasUnit": [
                "dfc-m:Euro"
            ]
        },
        "dfc-b:listedIn": [
            "salessession/salessessionId1"
        ],
        "dfc-b:offers : [
          catalogItem/catalogItemId1
        ]
      },
      {
        "@id": "offer/offerId3",
        "@type": "dfc-b:Offer",
        "dfc-b:offeredTo": "/customerCategory/customerCategoryId1",
        "dfc-b:stockLimitation": "999",
        "dfc-b:hasPrice": {
            "@type": "dfc-b:Price",
            "dfc-b:value": "10",
            "dfc-b:VATrate": "5",
            "dfc-b:hasUnit": [
                "dfc-m:Euro"
            ]
        },
        "dfc-b:listedIn": [
            "salessession/salessessionId1"
        ],
        "dfc-b:offers : [
          catalogItem/catalogItemId2
        ]
      },
      {
        "@id": "offer/offerId4",
        "@type": "dfc-b:Offer",
        "dfc-b:offeredTo": "/customerCategory/customerCategoryId2",
        "dfc-b:price": "999",
        "dfc-b:stockLimitation": "999",
        "dfc-b:discount":"5",
        "dfc-b:hasPrice": {
            "@type": "dfc-b:Price",
            "dfc-b:value": "10",
            "dfc-b:VATrate": "5",
            "dfc-b:hasUnit": [
                "dfc-m:Euro"
            ]
        },
        "dfc-b:listedIn": [
            "salessession/salessessionId1"
          ],
        "dfc-b:offers : [
          catalogItem/catalogItemId4
        ]
      },

      {
        "@id": "salessession/salessessionId1",
        "@type": "dfc-b:SaleSession",
        "dfc-b:beginDate": "",
        "dfc-b:endDate": "",
        "dfc-b:quantity": "",
        "dfc-b:lists": [
            "offer/offerId1",
            "offer/offerId2",
            "offer/offerId3",
            "offer/offerId4"
          ],
      },

      {
        "@id": "order/orderId1",
        "@type": "dfc-b:Order",
        "dfc-b:orderNumber": "0001",
        "dfc-b:discount": "0",
        "dfc-b:date": "2023-02-13T14:30:00+01:00",
        "dfc-b:belongsTo": "salessession/salessessionId1",

        "dfc-b:hasPart": [
            "orderline/orderlineId1"
        ],
        "dfc-b:orderedBy": "person/personId"
      },
      {
        "@id": "orderline/orderlineId1",
        "@type": "dfc-b:OrderLine",
        "dfc-b:quantity": "4",
        "dfc-b:discount": "0",
        "dfc-b:belongsTo": "salessession/salessessionId1",
        "dfc-b:hasPrice": {
            "@type": "dfc-b:Price",
            "dfc-b:value": "40",
            "dfc-b:VATrate": "5",
            "dfc-b:hasUnit": "dfc-m:Euro"
        },
        
        "dfc-b:concerns": "offer/offerId1",
        "dfc-b:partOf": "order/orderId1"
      }
      
    ]
  }
```
