[[Home]] > [[User Guide|UserGuide]] > [[UserGuide/Configuration API|Configuration API]] > [[UserGuide/Configuration/Readers and Writers|Readers and Writers]]

# Configuring Readers and Writers 

Configuring Readers and Writers allows you to register custom readers/writers or change the default implementations used by the library.

## Basic Configuration 

You can configure any type of reader/writer supported via the library using this mechanism.  There is some common vocabulary that is used to associate a reader/writer with specific data types.

The `http://www.w3.org/ns/formats/media_type` URI is used to reference MIME types with which the reader/writer should be associated and the `http://www.w3.org/ns/formats/preferred_suffix` URI is used to reference file extensions with which the reader/writer should be associated.

The following example shows how to change the default RDF/XML writer to be the [PrettyRdfXmlWriter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.PrettyRdfXmlWriter):

{{{
#!turtle
@prefix dnr: <http://www.dotnetrdf.org/configuration#> .
@prefix fmt: <http://www.w3.org/ns/formats/>.

# Customise to use PrettyRdfXmlWriter

[] a dnr:RdfWriter ;
	dnr:type "VDS.RDF.Writing.PrettyRdfXmlWriter" ;
	fmt:media_type "application/rdf+xml" ;
	fmt:preferred_suffix "rdf" .
```

You can use the `AutoConfigureReadersAndWriters(IGraph g)` method to have the API automatically load reader and writer configuration.

The same basic configuration applies regardless of whether you are configuring a reader/writer, you merely need to swap out `dnr:RdfWriter` for the class representing what you are registering:

| RDF Class | .Net Class |
| --- | --- |
| `dnr:RdfWriter` | [IRdfWriter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.IRdfWriter) |
| `dnr:RdfParser` | [IRdfReader](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.IRdfReader) |
| `dnr:DatasetParser` | [IStoreReader](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.IStoreReader) |
| `dnr:DatasetWriter` | [IStoreWriter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.IStoreWriter) |
| `dnr:SparqlResultsParser` | [ISparqlResultsReader](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.ISparqlResultsReader) |
| `dnr:SparqlResultsWriter` | [ISparqlResultsWriter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.ISparqlResultsWriter) |