# version 1.6

## Changes log from version 1.5.1

* support of flat graph structure



## GET User Data

* @base and @id respect W3C json-ld standard

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
    "dfc-b:supplies":{
      "@type":"@id"
    }
    "dfc-b:defines":{
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
      }
      "dfc-b:supplies": [
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
      ]
    }
  ]
}
```

