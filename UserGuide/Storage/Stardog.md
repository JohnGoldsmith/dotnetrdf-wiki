[[Home]] > [[User Guide]] > [[UserGuide/Storage API|Storage API]] > [[UserGuide/Storage/Providers|Storage Providers]] > [[UserGuide/Storage/Stardog|Stardog]]

= Stardog =

[[http://stardog.com|Stardog]] the RDF database from Clark & Parsia can be connected to via the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.StardogConnector|StardogConnector]].  This connector uses the Stardog HTTP interface so requires a running Stardog HTTP server.

The ##StardogConnector## assumes the latest version of Stardog is being used, currently these are the 3.x releases.  A [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.StardogV1Connector|StardogV1Connector]], [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.StardogV2Connector|StardogV2Connector]] and [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.StardogV3Connector|StardogV3Connector]] are provided if you wish to be explicit about the version of Stardog you are connecting to.

In terms of backwards compatibility the newer connectors may be able to talk to an older server but we can't guarantee this will work correctly, also newer connectors have support for newer Stardog features which will not work if used against an older server.

== Supported Capabilities ==

* Load, Save, Delete, Update and List Graphs
* SPARQL Query
** Configurable query reasoning levels (for Stardog 1.x and 2.x)
* SPARQL Update for Stardog 2.x and 3.x connections
* Transactions

== Creating a Connection ==

Connecting to a Stardog database requires knowing the Base URI for the server, a database ID and user credentials if security is enabled e.g.

{{{
#!csharp

StardogConnector stardog = new StardogConnector("http://localhost:5820", "example", "username", "password");
}}}

== Configurable Query Reasoning ==

Stardog 1.x and 2.x supports configurable reasoning levels on a per-query basis.  The current reasoning level is controlled by the //Reasoning// property and takes a value from the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.StardogReasoningMode|StardogReasoningMode]] enum.

Note that from Stardog 3.x the reasoning level is controlled at the database level and cannot be changed at the connection level.

This may be changed any time you like and affects any subsequent queries made via the //Query()// method.

== Transactions ==

Stardog is one of the few stores currently supported that support transactions, see the [[UserGuide/Storage/Transactions|Transactions API]] page for an overview of those APIs.  If the Transaction APIs are not explicitly used each operation occurs in its own transaction which is auto-committed at the end of its operation.

= Managing a Server =

We support managing a server via the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.Management.StardogServer|StardogServer]] class which implements our [[UserGuide/Storage/Servers|Servers API]]

The ##StardogServer## assumes the latest version of Stardog is being used, currently these are the 3.x releases.  A [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.Management.StardogV1Server|StardogV1Server]], [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.Management.StardogV2Server|StardogV2Server]] and [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.Management.StardogV3Server|StardogV3Server]] are provided if you wish to be explicit about the version of Stardog you are connecting to.

A ##StardogConnector## provides access to its associated server via the //ParentServer// or //AsyncParentServer// property.

== Connecting to a Server ==

You can also connect directly to a server by creating an instance of the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.Management.StardogServer|StardogServer]] class.  This requires you to know the Base URI of the server:

{{{
#!csharp

StardogServer server = new StardogServer("http://localhost:5822", "username", "password");
}}}

== Creating Stores ==

When creating a store the ##StardogServer## will provide templates from the following selection:

|= Template |= Description |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.Management.Provisioning.Stardog.StardogDiskTemplate|StardogDiskTemplate]] | Used to create a Stardog disk store |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.Management.Provisioning.Stardog.StardogMemTemplate|StardogMemTemplate]] | Used to create a Stardog memory store |

Both of these derive from the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.Management.Provisioning.Stardog.BaseStardogTemplate|BaseStardogTemplate]] which provides a large range of properties that can be used to configure Stardog database options.