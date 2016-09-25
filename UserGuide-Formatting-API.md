[[Home]] > [[User Guide|UserGuide]] > [[Formatting API|UserGuide-Formatting-API]]

# Formatting API 

The Formatting API is an collection of APIs found in the `VDS.RDF.Writing.Formatting` namespace, it is concerned with turning objects like nodes, triples and SPARQL results into strings for display.  The formatting API underpins the writers already seen in the basic tutorial on the [[Writing RDF|UserGuide-Writing-RDF]] page.

The API consists of a number of interfaces:

| Interface | Formatting Capabilities |
| --- | --- |
| [ICharFormatter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.ICharFormatter) | Formats individual characters |
| [IUriFormatter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.IUriFormatter) | Formats URIs |
| [IBaseUriFormatter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.IBaseUriFormatter) | Formats Base URI declarations |
| [INamespaceFormatter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.INamespaceFormatter) | Formats namespace declarations |
| [INodeFormatter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.INodeFormatter) | Formats [INode](https://dotnetrdf.github.io/api/html/T_VDS_RDF_INode.htm) instances |
| [ITripleFormatter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.ITripleFormatter) | Formats [Triple](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Triple.htm) instances |
| [IResultFormatter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.IResultFormatter) | Formats [SparqlResult](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.SparqlResult) instances |

# Basic Usage 

Generally you will only want to use one of the higher level interfaces such as `INodeFormatter` or `ITripleFormatter`.  Both these interfaces define `Format(…)` methods which take either a `Triple` or an `INode` and return a string representation of them. You can also call `ToString(…)` overloads on `Triple` and `INode` which take in a formatter and return the String representation as formatted by that formatter.

In general any formatter usually provides one or more `Format()` or `FormatX()` methods which are used to format specific things.  These methods take the thing to be formatted and return a string.

## Example 1 

For example we can format specific nodes:

```csharp

//Assumes that we already have a Graph in the variable g
NTriplesFormatter formatter = new NTriplesFormatter();

//Want to get only the triples defining types - assumes rdf: prefix is appropriately defined for this Graph
UriNode rdfType = g.CreateUriNode("rdf:type");

//This prints only the subjects of the Triples we find with
//the predicate rdf:type using NTriples formatting
foreach (Triple t in g.GetTriplesWithPredicate(rdfType))
{
	Console.WriteLine(t.Subject.ToString(formatter));
}
```

## Standard Implementations 

Currently the library has the following formatters available but you can easily define your own:

| Formatter | Format Produced |
| --- | --- |
| [CsvFormatter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.CsvFormatter) | CSV |
| [Notation3Formatter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.Notation3Formatter) | Notation 3 |
| [NQuadsFormatter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.NQuadsFormatter) | NQuads |
| [NTriplesFormatter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.NTriplesFormatter) | NTriples |
| [SparqlFormatter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.SparqlFormatter) | SPARQL style, can also format queries |
| [TsvFormatter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.TsvFormatter) | TSV |
| [TurtleFormatter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.TurtleFormatter) | Turtle |
| [UncompressedNotation3Formatter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.UncompressedNotation3Formatter) | Uncompressed Notation 3 |
| [UncompressedTurtleFormatter](http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.UncompressedTurtleFormatter) | Uncompressed Turtle |

## Example 2 

Here's another example of formatting Triples for display on the console:

```csharp

using System;
using System.Collections.Generic;
using System.Linq;
using VDS.RDF;
using VDS.RDF.Writing.Formatting;

public class FormattingTriplesExample
{
  public static void Main(String[] args)
  {
    IGraph g = new Graph();

    //Assume we fill our graph with data from somewhere...

    //Create a formatter
    ITripleFormatter formatter = new TurtleFormatter(g);

    //Print triples with this formatter
    foreach (Triple t in g.Triples)
    {
      Console.WriteLine(t.ToString(formatter));
    }
  }
}
```