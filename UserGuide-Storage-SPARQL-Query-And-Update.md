[[Home]] > [[User Guide|UserGuide]] > [[UserGuide/Storage API|Storage API]] > [[UserGuide/Storage/Providers|Storage Providers]] > [[UserGuide/Storage/SPARQL Query and Update|SPARQL Query and Update Endpoints]]

# SPARQL Query and Update Endpoints 

You can treat any publicly accessible SPARQL store which has both Query and Update endpoints as a read-write store using the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.ReadWriteSparqlConnector|ReadWriteSparqlConnector]].

**Note:** If you were looking for documentation on querying a SPARQL endpoint please see [[Querying with SPARQL|UserGuide-Querying-With-SPARQL]]

## Supported Capabilities 

* Save, Load, Delete, Update and List Graphs
** Note that updating a graph may not work correctly for blank node containing graphs
* SPARQL Query
* SPARQL Update

## Creating a Connection 

You can create a connection either just by providing the endpoint URIs like so:

```csharp

SparqlConnector sparql = new SparqlConnector(new Uri("http://example.org/query"), new Uri("http://example.org/update"));
```

Or you can provide a [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.SparqlRemoteEndpoint|SparqlRemoteEndpoint]] and [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.SparqlRemoteUpdateEndpoint|SparqlRemoteUpdateEndpoint]] instance like so:

```csharp

SparqlRemoteEndpoint queryEndpoint = new SparqlRemoteEndpoint(new Uri("http://example.org/query"), "http://default-graph-uri");
SparqlRemoteUpdateEndpoint updateEndpoint = new SparqlRemoteUpdateEndpoint(new Uri("http://example.org/update"));

ReadWriteSparqlConnector sparql = new SparqlConnector(queryEndpoint, updateEndpoint);
```

In both cases there is an overload which takes a [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.SparqlConnectorLoadMethod|SparqlConnectorLoadMethods]] which determines whether the //LoadGraph()// method operates by making a ##CONSTRUCT## or a ##DESCRIBE## query, the default is ##CONSTRUCT##