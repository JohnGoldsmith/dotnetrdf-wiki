[[Home]] > [[User Guide]] > [[UserGuide/Storage API|Storage API]] > [[UserGuide/Storage/Servers|Servers API]]

= Servers API =

The Servers API provides limited management capabilities for 3rd party triple stores.  It is represented by the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.Management.IStorageServer|IStorageServer]] and [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.Management.IAsyncStorageServer|IASyncStorageServer]] interfaces.

These interfaces provide limited abilities to create, delete, get and list stores provided on a server i.e. the ability to manage and access multiple [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.IStorageProvider|IStorageProvider]] instances.