[[Home]] > [[User Guide|UserGuide]] > [[Configuration API|UserGuide-Configuration-API]] > [[Query Processors|UserGuide-Configuration-Query-Processors]]

# Configuring Query Processors 

Query Processors are classes that can process SPARQL Queries are return a [SparqlResultSet](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_SparqlResultSet.htm) or [IGraph](https://dotnetrdf.github.io/api/html/T_VDS_RDF_IGraph.htm) as appropriate. Query Processors implement the [ISparqlQueryProcessor](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_ISparqlQueryProcessor.htm) interface and the library provides 5 concrete implementations of this all of which can be configured using the Configuration API.

# Basic Configuration 

Basic Configuration for a Query Processor looks like the following:

```turtle

@prefix dnr: <http://www.dotnetrdf.org/configuration#> .

_:proc a dnr:SparqlQueryProcessor ;
  dnr:type "VDS.RDF.Query.LeviathanQueryProcessor" .
```

## Leviathan Query Processor 

The Leviathan Query Processor is used to process queries on in-memory stores using the library's Leviathan SPARQL Engine. It is configured quite simply by adding a `dnr:usingStore` property to the basic configuration, the object pointed to by this property must be a Triple Store which implements the [IInMemoryQueryableStore](https://dotnetrdf.github.io/api/html/T_VDS_RDF_IInMemoryQueryableStore.htm) interface e.g.

```turtle

@prefix dnr: <http://www.dotnetrdf.org/configuration#> .

_:proc a dnr:SparqlQueryProcessor ;
  dnr:type "VDS.RDF.Query.LeviathanQueryProcessor" ;
  dnr:usingStore _:store .

_:store a dnr:TripleStore ;
  dnr:type "VDS.RDF.TripleStore" .
```

For information on how to configure Triple Stores see [[Configuration API - Triple Stores|UserGuide-Configuration-Triple-Stores]].

Alternatively you may use the `dnr:usingDataset` property to connect it to a Dataset instead. See [[Configuration API - Datasets|UserGuide-Configuration-SPARQL-Datasets]] for details. If both `dnr:usingDataset` and `dnr:usingStore` are present then `dnr:usingDataset` has priority and `dnr:usingStore` will be ignored.

## Generic Query Processor 

The Generic Query Processor is used to process queries against some arbitrary store's SPARQL engine where the store you wish to connect to has an implementation of [IQueryableStorage](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Storage_IQueryableStorage.htm). To configure these handlers simply add a `dnr:storageProvider` property to the basic configuration like so:

```turtle
@prefix dnr: <http://www.dotnetrdf.org/configuration#> .

_:proc a dnr:SparqlQueryProcessor ;
  dnr:type "VDS.RDF.Query.GenericQueryProcessor" ;
  dnr:storageProvider _:manager .

_:manager a dnr:StorageProvider ;
  dnr:type "VDS.RDF.Storage.FusekiConnector" ;
  dnr:server <http://localhost:3030/dataset> .
```

The above specifies a Query Processor which passes the queries to the SPARQL engine of the specified Fuseki store. See [[Configuration API - Storage Providers|UserGuide-Configuration-Storage-Providers]] for more detail on configuring storage providers.

## Simple Query Processor 

Similar to the Generic Query Processor the Simple Query Processor passes queries to the `ExecuteQuery()` method of a Triple Store that implements the [INativelyQueryablerStore](https://dotnetrdf.github.io/api/html/T_VDS_RDF_INativelyQueryableStore.htm) interface. To configure this add a using Store property that points to a Triple Store that implements the relevant interface e.g.

```turtle

@prefix dnr: <http://www.dotnetrdf.org/configuration#> .

_:proc a dnr:SparqlQueryProcessor ;
  dnr:type "VDS.RDF.Query.SimpleQueryProcessor" ;
  dnr:usingStore _:store .

_:store a dnr:TripleStore ;
  dnr:type "VDS.RDF.NativeTripleStore" ;
  dnr:storageProvider _:manager .

_:manager a dnr:StorageProvider ;
  dnr:type "VDS.RDF.Storage.TalisPlatformConnector" ;
  dnr:storeID "space" .
```

The above configuration is effectively identical to the previous example, generally using Generic Query Processors is better and easier to configure.

## Remote Query Processor 

The Remote Query Processor is used to farm out queries to a remote SPARQL endpoint, it can be configured to use any endpoint configurable as described on [[Configuration API - SPARQL Endpoints|UserGuide-Configuration-SPARQL-Endpoints]].

```turtle

@prefix dnr: <http://www.dotnetrdf.org/configuration#> .

_:proc a dnr:SparqlQueryProcessor ;
  dnr:type "VDS.RDF.Query.RemoteQueryProcessor" ;
  dnr:queryEndpoint _:endpoint .

_:endpoint a dnr:SparqlQueryEndpoint ;
  dnr:type "VDS.RDF.Query.SparqlRemoteEndpoint" ;
  dnr:queryEndpointUri <http://example.org/sparql> .
```

The above configures a Remote Query Processor which passes queries to the endpoint at `http://example.org/sparql`

## Pellet Query Processor 

The Pellet Query Processor is used to pass queries to the SPARQL service provided by a knowledge base on some remote Pellet Server. It is configured by adding the `dnr:server` and `dnr:storeID` properties to the basic configuration like so:

```turtle

@prefix dnr: <http://www.dotnetrdf.org/configuration#> .

_:proc a dnr:SparqlQueryProcessor ;
  dnr:type "VDS.RDF.Query.PelletQueryProcessor" ;
  dnr:server "http://ps.clarkparsia.com" ;
  dnr:storeID "wine" .
```

This would configure a Pellet Query Processor which sends queries to the SPARQL service of the `wine` knowledge base on the Pellet Server at `http://ps.clarkparsia.com`