[[Home]] > [[User Guide]] > [[UserGuide/Configuration API|Configuration API]] > [[UserGuide/Configuration/SPARQL Operators|SPARQL Operators]]

# Configuring SPARQL Operators 

SPARQL Operators are a SPARQL extension that allows you to extend how certain operators in SPARQL are evaluated.  You can learn more about operators in the [[DeveloperGuide/SPARQL/SPARQL Operators|SPARQL Operators]] page of the [[Developer Guide]].

These may be configured quite simply provided they implement the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Operators.ISparqlOperator|ISparqlOperator]] interface and have an unparameterized constructor.

{{{
#!turtle

@prefix dnr: <http://www.dotnetrdf.org/configuration#> .

[] a dnr:SparqlOperator ;
  dnr:type "VDS.RDF.Query.Operators.DateTime.DateTimeAddition" .
```