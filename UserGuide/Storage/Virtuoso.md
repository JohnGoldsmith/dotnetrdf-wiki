[[Home]] > [[User Guide]] > [[UserGuide/Storage API|Storage API]] > [[UserGuide/Storage/Providers|Storage Providers]] > [[UserGuide/Storage/Virtuoso|Virtuoso]]

= Virtuoso =

You can connect to an [[http://virtuoso.openlinksw.com|OpenLink Virtuoso]] server using the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Storage.VirtuosoManager|VirtuosoManager]] class.  Since this requires additional dependencies in the form of the Virtuoso ADO.Net provider this functionality is in the separate library **dotNetRDF.Data.Virtuoso.dll**

== Supported Capabilities ==

* Load, Save, Delete, Update and List Graphs
* SPARQL Query and Update

Note that SPARQL Query and Update is subject to the peculiarities of the Virtuoso implementation which is well known for having various quirks and non-standard SPARQL extensions.

== Creating a Connection ==

Assuming a standard Virtuoso installation you can connect like so:

{{{
#!csharp

VirtuosoManager virtuoso = new VirtuosoManager("localhost", VirtuosoManager.DefaultDB, "username", "password");
}}}

If you are running with a non-standard port then you can specify that like so:

{{{
#!csharp

VirtuosoManager virtuoso = new VirtuosoManager("localhost", 1234, VirtuosoManager.DefaultDB, "username", "password");
}}}

**Note:** The port you provide is that of the SQL interface not that of the HTTP server, in a default installation this is #1111# or the constant ##VirtuosoManager.DefaultPort##

Advanced users may wish to configure the Virtuoso connection string manually in which case you can do this:

{{{
#!csharp

VirtuosoManager virtuoso = new VirtuosoManager("Connection String");
}}}

== Using Virtuoso SPARQL Extensions ==

Our implementation does its best to cope with Virtuoso SPARQL extensions but this is not always perfect, if you run into trouble please ask for [[Support]]