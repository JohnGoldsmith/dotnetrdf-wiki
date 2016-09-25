[[Home]] > [[User Guide|UserGuide]] > [[Storage API|UserGuide-Storage-API]] > [[Storage Providers|UserGuide/Storage/Providers]] > [[SPARQL Query and Update Endpoints|UserGuide-Storage-SPARQL-Query-And-Update]]

# SPARQL Query and Update Endpoints 

You can treat any publicly accessible SPARQL store which has both Query and Update endpoints as a read-write store using the [ReadWriteSparqlConnector](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Storage_ReadWriteSparqlConnector.htm.

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

Or you can provide a [SparqlRemoteEndpoint](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.SparqlRemoteEndpoint) and [SparqlRemoteUpdateEndpoint](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.SparqlRemoteUpdateEndpoint) instance like so:

```csharp

SparqlRemoteEndpoint queryEndpoint = new SparqlRemoteEndpoint(new Uri("http://example.org/query"), "http://default-graph-uri");
SparqlRemoteUpdateEndpoint updateEndpoint = new SparqlRemoteUpdateEndpoint(new Uri("http://example.org/update"));

ReadWriteSparqlConnector sparql = new SparqlConnector(queryEndpoint, updateEndpoint);
```

In both cases there is an overload which takes a [SparqlConnectorLoadMethods](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Storage_SparqlConnectorLoadMethod.htm which determines whether the `LoadGraph()` method operates by making a `CONSTRUCT` or a `DESCRIBE` query, the default is `CONSTRUCT`