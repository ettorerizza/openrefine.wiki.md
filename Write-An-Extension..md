{
  "name":"OpenCorporates Reconciliation Service",
  "identifierSpace":"http://rdf.freebase.com/ns/type.object.id",
  "schemaSpace":"http://rdf.freebase.com/ns/type.object.id",
  "view":{
    "url":"http://opencorporates.com{{id}}"
  },
  "preview":{
    "url":"http://opencorporates.com{{id}}/preview",
    "width":430,
    "height":300
  },
  "suggest" : {
    "entity" : {
      "service_url" : "http://opencorporates.com",
      "service_path" : "/reconcile/suggest",
      "flyout_service_path" : "/reconcile/flyout"
    },
    "property" : {
      "service_url" : "http://opencorporates.com",
      "service_path" : "/reconcile/suggest/properties",
      "flyout_service_path" : "/reconcile/flyout/properties"
    }
  },
  "defaultTypes":[{
      "id":"/organization/organization",
      "name":"Organization"
    }
  ]
}
