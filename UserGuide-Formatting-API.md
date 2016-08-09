[[Home]] > [[User Guide]] > [[UserGuide/Formatting API|Formatting API]]

# Formatting API 

The Formatting API is an collection of APIs found in the ##VDS.RDF.Writing.Formatting## namespace, it is concerned with turning objects like nodes, triples and SPARQL results into strings for display.  The formatting API underpins the writers already seen in the basic tutorial on the [[UserGuide/Writing RDF|Writing RDF]] page.

The API consists of a number of interfaces:

|= Interface |= Formatting Capabilities |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.ICharFormatter|ICharFormatter]] | Formats individual characters |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.IUriFormatter|IUriFormatter]] | Formats URIs |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.IBaseUriFormatter|IBaseUriFormatter]] | Formats Base URI declarations |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.INamespaceFormatter|INamespaceFormatter]] | Formats namespace declarations |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.INodeFormatter|INodeFormatter]] | Formats [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.INode|INode]] instances |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.ITripleFormatter|ITripleFormatter]] | Formats [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Triple|Triple]] instances |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.IResultFormatter|IResultFormatter]] | Formats [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.SparqlResult|SparqlResult]] instances |

# Basic Usage 

Generally you will only want to use one of the higher level interfaces such as ##INodeFormatter## or ##ITripleFormatter##.  Both these interfaces define //Format(…)// methods which take either a ##Triple## or an ##INode## and return a string representation of them. You can also call //ToString(…)// overloads on ##Triple## and ##INode## which take in a formatter and return the String representation as formatted by that formatter.

In general any formatter usually provides one or more //Format()// or //FormatX()// methods which are used to format specific things.  These methods take the thing to be formatted and return a string.

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

|= Formatter |= Format Produced |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.CsvFormatter|CsvFormatter]] | CSV |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.Notation3Formatter|Notation3Formatter]] | Notation 3 |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.NQuadsFormatter|NQuadsFormatter]] | NQuads |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.NTriplesFormatter|NTriplesFormatter]] | NTriples |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.SparqlFormatter|SparqlFormatter]] | SPARQL style, can also format queries |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.TsvFormatter|TsvFormatter]] | TSV |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.TurtleFormatter|TurtleFormatter]] | Turtle |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.UncompressedNotation3Formatter|UncompressedNotation3Formatter]] | Uncompressed Notation 3 |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Writing.Formatting.UncompressedTurtleFormatter|UncompressedTurtleFormatter]] | Uncompressed Turtle |

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