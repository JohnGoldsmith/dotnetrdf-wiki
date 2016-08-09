[[Home]] > [[User Guide|UserGuide]] > [[UserGuide/Storage API|Storage API]] > [[UserGuide/Storage/Providers|Storage Providers]] > [[UserGuide/Storage/SPARQL Query|SPARQL Query Endpoints]]

# SPARQL Query Endpoints 

You can treat any publicly accessible SPARQL Query endpoint as a read-only store using the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.SparqlConnector|SparqlConnector]].

**Note:** If you were looking for documentation on querying a SPARQL endpoint please see [[Querying with SPARQL|UserGuide-Querying-With-SPARQL]]

## Supported Capabilities 

* Load and List Graphs
* SPARQL Query

## Creating a Connection 

You can create a connection either just by providing the endpoint URI like so:

```csharp

SparqlConnector sparql = new SparqlConnector(new Uri("http://example.org/sparql"));
```

Or you can provide a [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.SparqlRemoteEndpoint|SparqlRemoteEndpoint]] instance like so:

```csharp

SparqlRemoteEndpoint endpoint = new SparqlRemoteEndpoint(new Uri("http://example.org/sparql"), "http://default-graph-uri");

SparqlConnector sparql = new SparqlConnector(endpoint);
```

In both cases there is an overload which takes a [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.SparqlConnectorLoadMethod|SparqlConnectorLoadMethods]] which determines whether the `LoadGraph()` method operates by making a `CONSTRUCT` or a `DESCRIBE` query, the default is `CONSTRUCT`