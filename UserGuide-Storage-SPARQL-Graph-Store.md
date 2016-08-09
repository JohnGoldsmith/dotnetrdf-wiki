[[Home]] > [[User Guide|UserGuide]] > [[UserGuide/Storage API|Storage API]] > [[UserGuide/Storage/Providers|Storage Providers]] > [[UserGuide/Storage/SPARQL Graph Store|SPARQL Graph Store Protocol Endpoints]]

# SPARQL Graph Store Protocol Endpoints 

Any store which provides a [[http://www.w3.org/TR/sparql11-http-rdf-update/|SPARQL Graph Store Protocol]] endpoint may be connected to via the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.SparqlHttpProtocolConnector|SparqlHttpProtocolConnector]].  This protocol is different from the normal SPARQL query and update protocol, if you wish to connect to a store that uses them see the [[UserGuide/Storage/SPARQL Query|SPARQL Query Endpoints]] documentation instead.

## Supported Capabilities 

* Load, Save, Delete and Additive Updates for Graphs

## Creating a Connection 

To connect to the server you need to know the Base URI at which it exposes the protocol:

```csharp

SparqlHttpProtocolConnector sparql = new SparqlHttpProtocolConnector(new Uri("http://example.org/sparql"));
```

Optionally you may provide a proxy server if necessary.