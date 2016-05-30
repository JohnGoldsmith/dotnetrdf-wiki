[[Home]] > [[User Guide]] > [[UserGuide/Storage API|Storage API]] > [[UserGuide/Storage/Transactions|Transaction API]]

= Transactions API =

The Transactions API provides some degree of control over Transactions when working with a store that supports these.  Supporting stores implement the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.ITransactionalStorage|ITransactionalStorage]] and/or [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.IAsyncTransactionalStorage|IAsyncTransactionalStorage]] interfaces.

= Basic Usage =

It is important to note that individual implementations are free to decide whether their transactions are global or somehow scoped (e.g. per-thread).  Please consult the documentation of an implementation to determine which is the case.

These interfaces are fairly rudimentary and provide three simple methods:

== Begin() ==

The //Begin()// method starts a new transaction.

== Commit() ==

The //Commit()// method commits the current transaction.

== Rollback() ==

The //Rollback()// method rolls back the current transaction.