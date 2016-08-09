[[Home]] > [[User Guide]] > [[UserGuide/Storage API|Storage API]] > [[UserGuide/Storage/Providers|Storage Providers]] > [[UserGuide/Storage/InMemory|In-Memory]]

# In-Memory 

This is a wrapper over one of dotNetRDF's internal in-memory [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.IInMemoryQueryableStore|IInMemoryQueryableStore]] or [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Datasets.ISparqlDataset|ISparqlDataset]] implementations.  It is primarily intended as a useful convenience for testing and prototyping prior to deploying the actual production store you intend to use.  This functionality is provided by the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.InMemoryManager|InMemoryManager]] class.

## Supported Capabilities 

* Load, Save, Update, Delete and List Graphs
* SPARQL Query and Update

## Creating a connection 

You can create a connection to a completely fresh empty dataset like so:

```csharp

InMemoryManager mem = new InMemoryManager();
```

Or you can wrap an existing dataset/store:

```csharp

TripleStore store = new TripleStore();

//Assume we fill the store from somewhere

InMemoryManager mem = new InMemoryManager(store);
```