[[Home]] > [[User Guide|UserGuide]] > [[UserGuide/Storage API|Storage API]] > [[UserGuide/Storage/Providers|Storage Providers]]

# Storage Providers 

This area of the [[User Guide|UserGuide]] covers the available [IStorageProvider](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.IStorageProvider) and [IAsyncStorageProvider](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.IAsyncStorageProvider) implementations.  Each provider has its own wiki page which details provider specific behaviour and any special functionality available for that provider.

You should read the [[UserGuide/Triple Store Integration|Triple Store Integration]] page for an overview of how to use the Storage API.

The available providers are as follows:

|= Provider |= Description |
| [[UserGuide/Storage/AllegroGraph|Allegro Graph]] | AllegroGraph 3.x and 4.x |
| [[UserGuide/Storage/Blazegraph|Blazegraph]] | Blazegraph |
| [[UserGuide/Storage/DatasetFile|Dataset Files]] | Read-only view over a NQuads/TriG/TriX file |
| [[UserGuide/Storage/4store|4store]] | 4store |
| [[UserGuide/Storage/Fuseki|Fuseki]] | Apache Jena Fuseki, access any Jena based store via Fuseki |
| [[UserGuide/Storage/InMemory|In-Memory]] | In-Memory store |
| [[UserGuide/Storage/Sesame|Sesame]] | Any Sesame based store is supported e.g. Sesame, OWLIM, BigData |
| [[UserGuide/Storage/SPARQL Query|SPARQL Query Endpoints]] | Any SPARQL Query endpoint |
| [[UserGuide/Storage/SPARQL Query and Update|SPARQL Query and Update Endpoints]] | Any store providing both a query and update endpoint |
| [[UserGuide/Storage/SPARQL Graph Store|SPARQL Graph Store Protocol]] | Any SPARQL Graph Store Protocol endpoint |
| [[UserGuide/Storage/Stardog|Stardog]] | Stardog |
| [[UserGuide/Storage/Virtuoso|Virtuoso]] | Virtuoso Universal Server |

There are also some useful wrappers available:

|= Wrapper |= Description |
| [ReadOnlyConnector](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.ReadOnlyConnector) | Make any other provider read-only |
| [QueryableReadOnlyConnector](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.QueryableReadOnlyConnector) | Make any queryable provider read-only |