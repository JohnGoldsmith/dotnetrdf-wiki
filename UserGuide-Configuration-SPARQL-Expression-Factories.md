[[Home]] > [[User Guide|UserGuide]] > [[Configuration API|UserGuide-Configuration-API]] > [[SPARQL Expression Factories|UserGuide-Configuration-SPARQL-Expression-Factories]]

# Configuring SPARQL Expression Factories 

Expression Factories are classes that are used in the parsing of SPARQL Query and Update requests to generate implementations of extension functions. They allow for the introduction of additional functions into your SPARQL endpoints for specialised data processing, manipulation of other functions.

These classes must implement the [SparqlCustomExpressionFactory](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Expressions.ISparqlCustomExpressionFactory) interface and have an unparameterised constructor, see the [[DeveloperGuide/SPARQL/Extension%20Functions|Extension Functions]] page for details on how to implement custom expression factories.

They can be configured quite simply as follows:

```turtle

@prefix dnr: <http://www.dotnetrdf.org/configuration#> .

_:factory a dnr:SparqlExpressionFactory ;
  dnr:type "YourNamespace.YourType, YourAssembly" .
```

In this example an expression factory from an external namespace is configured. As described in [[Configuration API - HTTP Handlers|UserGuide/Configuration/HTTP Handlers]] expression factories can be attached to HTTP Handlers using the `dnr:expressionFactory` property.