[[Home]] > [[How To|HowTo]] > [[Load RDF from the Web|HowTo-Load-RDF-From-The-Web]]

# Load RDF from the Web 

RDF can be loading from web via a URI using the `LoadFromUri()` extension method.  This method exists for both [IGraph](https://dotnetrdf.github.io/api/html/T_VDS_RDF_IGraph.htm) and [ITripleStore](https://dotnetrdf.github.io/api/html/T_VDS_RDF_ITripleStore.htm) e.g.

```csharp

IGraph g = new Graph();
g.LoadFromUri(new Uri("http://example.org/file.rdf"));
```

## Loading in a Specific Format 

Occasionally some server may misinterpret the `Accept` header that dotNetRDF normally uses and return a HTML page instead of a RDF graph.  You can force the loader to attempt to retrieve RDF in a specific format by specifying the parser you want to use e.g.

```csharp

IGraph g = new Graph();
g.LoadFromUri(new Uri("http://example.org/file.rdf"), new TurtleParser());
```

Learn more by reading the [[Reading RDF|UserGuide-Reading-RDF]] documentation.