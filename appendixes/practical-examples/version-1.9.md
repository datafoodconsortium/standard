# Version 1.9

### Changes log from version 1.8

* add of contact and phone number information on "PhysicalPlace"
* "uses" relation changed to "pickedUpAt", "deliversAt" and "hasPaymentMethod"
* "pickedUpAt" and "deliversAt" are relations between, respectively, "PickUpOption" and "DeliveryOption", and "PhysicalPlace"
* Temperature information are added through numeric range on "Temperature" class, and boolean properties with "frozen" and "refrigerated"

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
      "@id": "person/person1",
      "@type": "dfc-b:Person",
      "dfc-b:familyName" : "Doe",
      "dfc-b:firstName": "John",
      "dfc-b:mainContactOf" : "physicalPlace/physicalPlace1"
    },
    {
      "@id": "entreprise/entrepriseId",
      "@type": "dfc-b:Entreprise",
      "dfc-b:supplies": [
        "suppliedProduct/item1"
      ],
      "dfc-b:manages": [
        "catalogItem/catalogItemId1"
      ]
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
      "dfc-b:hasCertification": "dfc-f:Organic-AB",
      "dfc-b:hasContainerInformation": "dfc-b:Package",
      "dfc-b:referencedBy": "catalogItem/catalogItemId1",
      "dfc-b:referenceOf": "localizedProduct/item1",
      "dfc-b:refrigerated": "true",
      "dfc-b:frozen": "true",
      "dfc-b:hasTemperature" : {
        "@type" : "dfc-b:Temperature",
        "dfc-b:hasUnit": "dfc-m:Celsius",
        "dfc-b:minValue" : "-5",
        "dfc-b:maxValue" : "0"
      }
    },
    {
      "@id": "localizedProduct/item1",
      "@type": "dfc-b:LocalizedProduct",
      "dfc-b:hasReference": "suppliedProduct/item1",
      "dfc-b:representedBy": "physicalProduct/item1",
      "dfc-b:constituedBy": "theoreticalStock/item1"
    },
    {
      "@id": "theoreticalStock/item1",
      "@type": "dfc-b:TheoreticalStock",
      "dfc-b:constitues": "localizedProduct/item1",
      "dfc-b:localizedBy": "physicalPlace/physicalPlace1"
    },
    {
      "@id": "physicalProduct/item1",
      "@type": "dfc-b:PhysicalProduct",
      "dfc-b:represents": "localizedProduct/item1",
      "dfc-b:constituedBy": "realStock/item1"
    },
    {
      "@id": "realStock/item1",
      "@type": "dfc-b:RealStock",
      "dfc-b:constitues": "physicalProduct/item1",
      "dfc-b:storedIn": "physicalPlace/physicalPlace1"
    },
    {
      "@id": "physicalPlace/physicalPlace1",
      "@type": "dfc-b:PhysicalPlace",
      "dfc-b:localizes": "theoreticalStock/item1",
      "dfc-b:stores": "physicalProduct/item1",
      "dfc-b:hasPhoneNumber" : {
        "@type": "dfc-b:PhoneNumber",
        "dfc-b:countryCode": "",
        "dfc-b:phoneNumber" : ""
      },
      "dfc-b:hasMainContact": "person/person1",
      "dfc-b:hasAdress": {
        "@type": "dfc-b:Address",
        "dfc-b:city": "",
        "dfc-b:country": "",
        "dfc-b:postcode": "",
        "dfc-b:street": "",
        "dfc-b:longitude": "",
        "dfc-b:latitude": ""
      }
    },
    {
      "@id": "catalogItem/catalogItemId1",
      "@type": "dfc-b:CatalogItem",
      "dfc-b:sku": "catalog item gtin or sku",
      "dfc-b:stockLimitation": "999",
      "dfc-b:references": "/suppliedProduct/item1",
      "dfc-b:offeredThrough": [
          "offer/offerId1"
      ]
    },
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
      "dfc-b:offers" : "catalogItem/catalogItemId1"
    },
    {
      "@id": "salessession/salessessionId1",
      "@type": "dfc-b:SaleSession",
      "dfc-b:beginDate": "",
      "dfc-b:endDate": "",
      "dfc-b:quantity": "",
      "dfc-b:lists": [
          "offer/offerId1"
        ]
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
      "dfc-b:orderedBy": "person/personId",
      "dfc-b:selects": "pickupOption/pickUpOption1",
      "dfc-b:hasPaymentMethod" : {
        "@type": "dfc-b:PaymentMethod",
        "dfc-b:paymentMethodType": "Credit Card",
        "dfc-b:hasPrice" : {
          "@type": "dfc-b:Price",
          "dfc-b:value": "10",
          "dfc-b:VATrate": "5",
          "dfc-b:hasUnit": "dfc-m:Euro"
        }
      }
    },
    {
      "@id": "orderline/orderlineId1",
      "@type": "dfc-b:OrderLine",
      "dfc-b:quantity": "4",
      "dfc-b:belongsTo": "salessession/salessessionId1",
      "dfc-b:concerns": "offer/offerId1",
      "dfc-b:partOf": "order/orderId1"
    },
    {
      "@id": "pickupOption/pickUpOption1",
      "@type": "dfc-b:PickUpOption",
      "dfc-b:accessibilityInformation" : "",
      "dfc-b:deliveryConstraint": "",
      "dfc-b:pickedUpAt": "physicalPlace/physicalPlace1"
    },
    {
      "@id": "order/orderId2",
      "@type": "dfc-b:Order",
      "dfc-b:orderNumber": "0001",
      "dfc-b:discount": "0",
      "dfc-b:date": "2023-02-13T14:30:00+01:00",
      "dfc-b:belongsTo": "salessession/salessessionId1",
      "dfc-b:hasPart": [
          "orderline/orderlineId2"
      ],
      "dfc-b:orderedBy": "person/personId",
      "dfc-b:selects": "deliveryOption/deliveryOption1"
    },
    {
      "@id": "orderline/orderlineId2",
      "@type": "dfc-b:OrderLine",
      "dfc-b:quantity": "4",
      "dfc-b:belongsTo": "salessession/salessessionId1",
      "dfc-b:concerns": "offer/offerId1",
      "dfc-b:partOf": "order/orderId1"
    },
    {
      "@id": "deliveryOption/deliveryOption1",
      "@type": "dfc-b:DeliveryOption",
      "dfc-b:accessibilityInformation" : "",
      "dfc-b:deliveryConstraint": "",
      "dfc-b:deliveredAt": "physicalPlace/physicalPlace2"
    },
    {
      "@id": "physicalPlace/physicalPlace2",
      "@type": "dfc-b:PhysicalPlace",
      "dfc-b:hasPhoneNumber" : {
        "@type": "dfc-b:PhoneNumber",
        "dfc-b:countryCode": "",
        "dfc-b:phoneNumber" : ""
      },
      "dfc-b:hasMainContact": {
        "@type": "dfc-b:Person",
        "firstName": "",
        "familyName": ""
      },
      "dfc-b:hasAdress": {
        "@type": "dfc-b:Address",
        "dfc-b:city": "",
        "dfc-b:country": "",
        "dfc-b:postcode": "",
        "dfc-b:street": "",
        "dfc-b:longitude": "",
        "dfc-b:latitude": ""
      }
    }
  ]
}
```
