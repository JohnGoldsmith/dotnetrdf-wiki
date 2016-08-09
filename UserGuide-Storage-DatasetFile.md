[[Home]] > [[User Guide|UserGuide]] > [[UserGuide/Storage API|Storage API]] > [[UserGuide/Storage/Providers|Storage Providers]] > [[UserGuide/Storage/DatasetFile|Dataset Files]]

# Dataset Files 

You can treat a dataset file (NQuads, TriG or TriX) as a read-only triple store using the [DatasetFileManager](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.DatasetFileManager) class.

## Supported Capabilities 

* Read-Only access
* SPARQL Query

## Creating a Connection 

You can create a connection to a dataset file like so:

```csharp

DatasetFileManager dataset = new DatasetFileManager("example.trig", false);
```

The boolean parameter here controls whether to load the file in async, if set to `true` your code will continue immediately but you won't be able to use the store immediately.  If you use async loading the `IsReady` property will indicate when the store is ready for use.