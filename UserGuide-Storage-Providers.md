[[Home]] > [[User Guide|UserGuide]] > [[Storage API|UserGuide-Storage-API]] > [[Storage Providers|UserGuide/Storage/Providers]]

# Storage Providers 

This area of the [[User Guide|UserGuide]] covers the available [IStorageProvider](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.IStorageProvider) and [IAsyncStorageProvider](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.IAsyncStorageProvider) implementations.  Each provider has its own wiki page which details provider specific behaviour and any special functionality available for that provider.

You should read the [[Triple Store Integration|UserGuide/Triple Store Integration]] page for an overview of how to use the Storage API.

The available providers are as follows:

| Provider | Description |
| --- | --- |
| [[Allegro Graph|UserGuide-Storage-AllegroGraph]] | AllegroGraph 3.x and 4.x |
| [[Blazegraph|UserGuide-Storage-Blazegraph]] | Blazegraph |
| [[Dataset Files|UserGuide-Storage-DatasetFile]] | Read-only view over a NQuads/TriG/TriX file |
| [[4store|UserGuide-Storage-4store]] | 4store |
| [[Fuseki|UserGuide-Storage-Fuseki]] | Apache Jena Fuseki, access any Jena based store via Fuseki |
| [[In-Memory|UserGuide-Storage-InMemory]] | In-Memory store |
| [[Sesame|UserGuide-Storage-Sesame]] | Any Sesame based store is supported e.g. Sesame, OWLIM, BigData |
| [[SPARQL Query Endpoints|UserGuide-Storage-SPARQL-Query]] | Any SPARQL Query endpoint |
| [[SPARQL Query and Update Endpoints|UserGuide-Storage-SPARQL-Query-And-Update]] | Any store providing both a query and update endpoint |
| [[SPARQL Graph Store Protocol|UserGuide-Storage-SPARQL-Graph-Store]] | Any SPARQL Graph Store Protocol endpoint |
| [[Stardog|UserGuide-Storage-Stardog]] | Stardog |
| [[Virtuoso|UserGuide-Storage-Virtuoso]] | Virtuoso Universal Server |

There are also some useful wrappers available:

| Wrapper | Description |
| --- | --- |
| [ReadOnlyConnector](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.ReadOnlyConnector) | Make any other provider read-only |
| [QueryableReadOnlyConnector](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.QueryableReadOnlyConnector) | Make any queryable provider read-only |