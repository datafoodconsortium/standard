# Exemples d'usage du standard

```json
{
  "@context": {
    "DFC": "http://datafoodconsortium.org/ontologies/DFC_FullModel.owl#",
    "@base": "http://maPlateformeNationale"
  },
  "@id": "/entreprise/maGrandeEntreprise",
  "@type": "DFC:Entreprise",
  "DFC:supplies":[
    {
      "@id":"/suppliedProduct/item3",
      "DFC:hasUnit":{
        "@id":"/unit/kg"
      },
      "DFC:quantity":"0,5",
      "DFC:description":"Aillet botte 1 pièce"
    },
    {
      "@id":"/suppliedProduct/item4",
      "DFC:hasUnit":{
        "@id":"/unit/unit"
      },
      "DFC:quantity":"1",
      "DFC:description":"Aromates-Romarin Botte 1 pièce"
    }
  ]
}
```
