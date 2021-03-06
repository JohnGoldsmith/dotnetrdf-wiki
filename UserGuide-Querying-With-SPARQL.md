[[Home]] > [[User Guide|UserGuide]] > [[Querying with SPARQL|UserGuide-Querying-With-SPARQL]]

# Querying with SPARQL 

SPARQL is the standard query language for the Semantic Web and can be used to query over large volumes of RDF data. dotNetRDF provides support for querying both over local in-memory data using it's own SPARQL implementation and for querying remote data using SPARQL endpoints or through other stores SPARQL implementations.

If you want to learn about SPARQL you should take a look at the [SPARQL Query Language Specification](http://www.w3.org/TR/sparql11-query/) which provides examples of all the various query forms as well as the full formal specifcation.

Advanced Users may want to take a look at the [[Advanced SPARQL|UserGuide-Advanced-SPARQL]] and [[SPARQL Optimization|DeveloperGuide-SPARQL-Optimization]] pages for more details about how our in-memory SPARQL engine functions.

When using SPARQL you'll want to import the `VDS.RDF.Query` namespace using the following statement at the start of your code files:

```csharp

using VDS.RDF.Query;
```

If you are going to parse SPARQL queries yourself you will also need to use the `VDS.RDF.Parsing` namespace.

# Representing Queries 

While some parts of the library will allow you to pass a raw SPARQL query as a string often you will need to parse a [SparqlQuery](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_SparqlQuery.htm) object around. A `SparqlQuery` can be created in a couple of ways, firstly you can simply parse a raw SPARQL string into a query like so:

```csharp

using System;
using VDS.RDF.Parsing;
using VDS.RDF.Query;

public class QueryParsingExample
{
	public static void Main(String[] args)
	{
		//First we need an instance of the SparqlQueryParser
		SparqlQueryParser parser = new SparqlQueryParser();

		//Then we can parse a SPARQL string into a query
		SparqlQuery q = parser.ParseFromString("SELECT * WHERE { ?s a ?type }");
	}
}
```

Queries can be parsed from strings, files or streams as desired. This method works well if you have a relatively simple query but can become cumbersome if you are generating complicated queries in code because you have to build up the string in memory and ensure it is properly formatted yourself. If this is the case you will often be better off using the [SparqlParameterizedString](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_SparqlParameterizedString.htm) class to build your query string, it provides a `SqlCommand` style interface for building a query string:

```csharp

using System;
using VDS.RDF.Query;

public class SparqlParameterizedStringExample
{
	public static void Main(String[] args)
	{
		//Create a Parameterized String
		SparqlParameterizedString queryString = new SparqlParameterizedString();

		//Add a namespace declaration
		queryString.Namespaces.AddNamespace("ex", new Uri("http://example.org/ns#"));

		//Set the SPARQL command
		//For more complex queries we can do this in multiple lines by using += on the
		//CommandText property
		//Note we can use @name style parameters here
		queryString.CommandText = "SELECT * WHERE { ?s ex:property @value }";

		//Inject a Value for the parameter
		queryString.SetUri("value", new Uri("http://example.org/value"));

		//When we call ToString() we get the full command text with namespaces appended as PREFIX
		//declarations and any parameters replaced with their declared values
		Console.WriteLine(queryString.ToString());

		//We can turn this into a query by parsing it as in our previous example
		SparqlQueryParser parser = new SparqlQueryParser();
		SparqlQuery query = parser.ParseFromString(queryString);
	}
}
```

# Accessing Results 

The key classes for accessing results when using SPARQL are the [SparqlResultSet](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_SparqlResultSet.htm) and [SparqlResult](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_SparqlResult.htm) class, these represent a Result Set and an individual Result respectively. When you make any kind of SPARQL query through any of the methods described in this article you will always get a `SparqlResultSet` or an `IGraph` in return (unless an error occurs).

## Result Sets 

The `SparqlResultSet` class is used to represent the results of SELECT and ASK queries. A Result Set either contains a table of `SparqlResult` items in the case of a SELECT query or a single boolean value in the case of an ASK query. The following are the key properties of the `SparqlResultSet`:

### ResultsType 

The `ResultsType` property is used to determine what type of result set you have received. The possible values are from the [SparqlResultsType](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_SparqlResultsType.htm) enumeration and are as follows:

| Type | Meaning |
| --- | --- |
| `SparqlResultsType.Boolean` | Is a Boolean results set (ASK Query results) |
| `SparqlResultsType.VariableBindings` | Is a table of results (SELECT Query results) |
| `SparqlResultsType.Unknown` | Unknown results, usually means that nothing has been loaded into the instance yet |

### Result 

The `Result` property gives the boolean result of an ASK query or in the case of a SELECT query always returns true.

### Results 

The `Results` property gives the set of `SparqlResult` objects as a strongly typed `List<SparqlResult>`. You can use this to enumerate results or you can enumerate directly over the result set since it is `IEnumerable<SparqlResult>` e.g.

```csharp

//Enumerating via the Results property
foreach (SparqlResult result in rset.Results)
{
	//Do what you want with each result
}

//Enumerating directly
foreach (SparqlResult result in rset)
{
	Console.WriteLine(result.ToString());
}
```

Generally it is best to use the second form since this means you can do LINQ operations more efficiently on the Result Set.

### Variables 

The `Variables` property gives an `IEnumerable<String>` which lists all the variables that are bound in the Result Set. A variable which is listed in this enumeration does not necessarily appear in every result.
Result

## Result Rows 

A `SparqlResultSet` is composed of a table of results which are enumerated via the `Results` property as seen above. Each row in this table is an instance of the `SparqlResult` class which has a number of methods and properties to allow you to access the variables and values for that result row.

### Count 

The `Count` property tells you how many variable/value pairs are present in the result row

### Variables 

The `Variables` property enumerates the variables actually present in the result row. Note that this may differ from the Variables property of the containing SparqlResultSet since not every result necessarily has every variable in it e.g. empty rows, rows from different sides of a UNION etc.

### Accessing Values 

Values from a row may be accessed in three ways:

```csharp

//Assuming our result row is in a variable r

//With Named Indexing
INode value = r["var"];

//With Indexing
INode value = r[0];

//With method
INode value = r.Value("var");
```

**Warning:** All of the above return a value/null if the variable is present (but possibly unbound) in the result or throw an error if the variable is not present. Consider using the `HasValue()` method to check whether a given variable is present in a result before attempting to retrieve it.

# Making a Query 

Now we'll look at the different ways in which you can actually make a query, there are several ways depending on what you are querying.

| Method | Purpose |
| --- | --- |
| Query Processors | General purpose abstraction for making queries, highly recommended since it allows you to wrap any of the other query methods in abstracted code |
| Remote Query | Make a query against a remote SPARQL endpoint |
| Native Query | Make a query against an external SPARQL engine |
| Direct Triple Store Query | Make a query directly against a `IInMemoryQueryable` (`deprecated`) |

## Query Processors 

Query Processors are classes use to evaluate queries which abstract away from whatever the underlying query engine is. The [ISparqlQueryProcessor](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_ISparqlQueryProcessor.htm) interface defines two methods for evaluating queries both called `ProcessQuery()`.  Query processors are the preferred means of evaluating queries in dotNetRDF and should used in preference to other methods wherever possible.

The first `ProcessQuery(SparqlQuery query)` takes in a `SparqlQuery` and returns either a `SparqlResultSet` or an `IGraph` instance. The second `ProcessQuery(IRdfHandler rdfHandler, ISparqlResultsHandler resultsHandler, SparqlQuery query)` is for advanced users and gives much more detailed control over the processing of results.

For example you can use the standard [LeviathanQueryProcessor](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_LeviathanQueryProcessor.htm) to evaluate queries in-memory e.g.

```csharp 

using System;
using VDS.RDF;
using VDS.RDF.Parsing;
using VDS.RDF.Query;
using VDS.RDF.Writing.Formatting;

public class LeviathanQueryProcessorExample
{
	public static void Main(String[] args)
	{
		TripleStore store = new TripleStore();

		//Assume that we fill our Store with data from somewhere

		//Create a dataset for our queries to operate over
		//We need to explicitly state our default graph or the unnamed graph is used
		//Alternatively you can set the second parameter to true to use the union of all graphs
		//as the default graph
		InMemoryDataset ds = new InMemoryDataset(store, new Uri("http://mydefaultgraph.org"));

		//Get the Query processor
		ISparqlQueryProcessor processor = new LeviathanQueryProcessor(ds);

		//Use the SparqlQueryParser to give us a SparqlQuery object
		//Should get a Graph back from a CONSTRUCT query
		SparqlQueryParser sparqlparser = new SparqlQueryParser();
		SparqlQuery query = sparqlparser.ParseFromString("CONSTRUCT { ?s ?p ?o } WHERE {?s ?p ?o}");
		results = processor.ProcessQuery(query);
		if (results is IGraph)
		{
			//Print out the Results
			IGraph g = (IGraph)results;
			NTriplesFormatter formatter = new NTriplesFormatter();
			foreach (Triple t in g.Triples)
			{
				Console.WriteLine(t.ToString(formatter));
			}
			Console.WriteLine("Query took " + query.QueryTime + " milliseconds");
		}
	}
}
```

A key thing to notice here is that we create a [ISparqlDataset](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_Datasets_ISparqlDataset.htm) instance which wraps our `IInMemoryQueryableStore` instance. This dataset allows us to control which graph is used as the default graph for queries or even to use the union of all graphs as the default graph.

In this example we have only printed results in full to the Console, to learn more about how to format results for display see [[Result Formatting|UserGuide-Result-Formatting|]].

### Common Errors 

#### Default Graph 

A common error with making queries is that queries by default typically operate only over the unnamed default graph in the store (depending on your query processor). Therefore executing queries may yield no results depending on what graphs your data is in and whether you configured your dataset correctly. Please see the [[SPARQL Datasets|UserGuide-SPARQL-Datasets]] page for discussions of configuring different kinds of dataset.  You can also look at [[Debugging SPARQL Queries|HowTo-Debug-SPARQL-Queries]] for a method to debug what is happening with your query when using the in-memory SPARQL engine.

The typical cause of this is that when you call `LoadFromFile()` or `LoadFromUri()` the library automatically assigns the graph a name based on the data source so when you add it a store instance it is a named graph rather than the default graph.  The easiest way to resolve this is to simply set the `BaseUri` property of your graph instance to `null` after loading it and before you execute queries with it.

#### FROM and FROM NAMED 

Another common error stems from misunderstandings about the purpose of `FROM` and `FROM NAMED` clauses.  These are used simply to identify the graphs used for the rest of the query and these graphs **MUST** exist in the dataset you are querying.  Note that providing a graph name does not cause that graph to be retrieved from a file or the web though we do provide the [DiskDemandTripleStore](https://dotnetrdf.github.io/api/html/T_VDS_RDF_DiskDemandTripleStore.htm) and the [WebDemandTripleStore](https://dotnetrdf.github.io/api/html/T_VDS_RDF_WebDemandTripleStore.htm) which can be used to add this behaviour if desired.

The graphs identified by the `FROM` clause are merged together and these form the default graph for the query, this is the graph that all triple patterns not contained in a `GRAPH` clause must match.  The graphs identified by the `FROM NAMED` clause are used individually for matching triple patterns contained within `GRAPH` clauses.

It is also important to understand that using these clauses it is possible to define datasets that your queries can never match.  For example if you have a `FROM` clause but no `FROM NAMED` then there are by definition no named graphs for the purposes of evaluating that query and any `GRAPH` clause would match nothing.

### Available Query Processors 

The library includes the following query processors:

| Processor | Description |
| --- | --- |
| [LeviathanQueryProcessor](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_LeviathanQueryProcessor.htm) | Standard in-memory query processor |
| [ExplainQueryProcessor](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_ExplainQueryProcessor.htm) | Extension of the `LeviathanQueryProcessor` which executes queries and prints explanations to the Console |
| [RemoteQueryProcessor](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_RemoteQueryProcessor.htm) | Executes queries against a remote SPARQL endpoint |
| [GenericQueryProcessor](https://dotnetrdf.github.io/api/html/T_VDS_RDF_GenericQueryProcessor.htm) | Executes a query against a [IQueryableStorage](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Storage_IQueryableStorage.htm) implementation |

### Customizing Query Behaviour 

When you use the `ProcessQuery()` overload that takes a `SparqlQuery` object you have the option of setting some properties on it which control its behaviour with regards to execution timeout. Since some queries can take a very long time to run it is often sensible to limit how long queries can run for, the `Timeout` property of the `SparqlQuery` allows you to specify the timeout. If you wish to get results back even when a timeout occurs then you can set the `PartialResultsOnTimeout` property to ensure you get some results even if a timeout occurs.

**However** there is no guarantee that a query processor implementation will respect these properties.

# Remote Query 

Remote SPARQL endpoints can be queried using the [SparqlRemoteEndpoint](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_SparqlRemoteEndpoint.htm) class. This class is a wrapper around a remote endpoint which sends queries to the endpoint and then turns the response into a `SparqlResultSet` or `IGraph` as appropriate.

A remote endpoint is a combination of an endpoint URI and an optional default Graph URI. A `SparqlRemoteEndpoint` provides specific strongly typed methods for making queries meaning that you don't need to type check and cast the result. The `QueryWithResultGraph(String sparqlQuery)` method can be used to make a CONSTRUCT or DESCRIBE query while the `QueryWithResultSet(String sparqlQuery)` method can be used to make SELECT and ASK queries. You can also use the `QueryRaw(String sparqlQuery, out String ctype)` method if you wish to get the raw response stream from the endpoint and process it yourself.

A remote endpoint can be used as follows:

```csharp

using System;
using VDS.RDF;
using VDS.RDF.Query;

public class SparqlRemoteEndpointExample
{
	public static void Main(String[] args)
	{
		//Define a remote endpoint
		//Use the DBPedia SPARQL endpoint with the default Graph set to DBPedia
		SparqlRemoteEndpoint endpoint = new SparqlRemoteEndpoint(new Uri("http://dbpedia.org/sparql"), "http://dbpedia.org");

		//Make a SELECT query against the Endpoint
		SparqlResultSet results = endpoint.QueryWithResultSet("SELECT DISTINCT ?Concept WHERE {[] a ?Concept}");
		foreach (SparqlResult result in results)
		{
			Console.WriteLine(result.ToString());
		}

		//Make a DESCRIBE query against the Endpoint
		IGraph g = endpoint.QueryWithResultGraph("DESCRIBE ");
		foreach (Triple t in g.Triples)
		{
			Console.WriteLine(t.ToString());
		}
	}
}
```

## Native Query 

We use the term native query to refer to queries where you utilise the SPARQL implementation of other Triple Stores directly. This feature is provided by classes which implement the `INativelyQueryableStore` interface, we now provide support for doing this with any of the supported backing Stores. If you take a look at the [[Working with Triple Stores|UserGuide-Working-With-Triple-Stores]] page you'll see an example of using the `PersistentTripleStore` class to query any of our supported stores.

Alternatively you can make a query direct to a store without using any abstractions simply by using an instance of the [IQueryableStorage](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Storage_IQueryableStorage.htm) interface  which most of our available [IStorageProvider](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Storage_IStorageProvider.htm) implementations also support, please see the [[Triple Store Integration|UserGuide/Triple Store Integration]] page for an example of this.

## Direct Triple Store Query 

In-memory representations of Triple Stores which implement the [IInMemoryQueryableStore](https://dotnetrdf.github.io/api/html/T_VDS_RDF_IInMemoryQueryableStore.htm) interface can be queried locally using the libraries in-memory SPARQL implementation.

**Note:** This method of making queries is considered deprecated and should be avoided in preference of using a `ISparqlQueryProcessor` wherever possible.

The following example shows how this works:

```csharp

using System;
using VDS.RDF;
using VDS.RDF.Parsing;
using VDS.RDF.Query;

public class InMemoryTripleStoreExample
{
	public static void Main(String[] args)
	{
		TripleStore store = new TripleStore();

		//Assume that we fill our Store with data from somewhere

		//Execute a raw SPARQL Query
		//Should get a SparqlResultSet back from a SELECT query
		Object results = store.ExecuteQuery("SELECT * WHERE { { ?s ?p ?o } UNION { GRAPH ?g { ?s ?p ?o } } }");
		if (results is SparqlResultSet)
		{
			//Print out the Results
			SparqlResultSet rset = (SparqlResultSet)results;
			foreach (SparqlResult result in rset)
			{
				Console.WriteLine(result.ToString());
			}
		}

		//Use the SparqlQueryParser to give us a SparqlQuery object
		//Should get a Graph back from a CONSTRUCT query
		SparqlQueryParser sparqlparser = new SparqlQueryParser();
		SparqlQuery query = sparqlparser.ParseFromString("CONSTRUCT { ?s ?p ?o } WHERE { { ?s ?p ?o } UNION { GRAPH ?g { ?s ?p ?o } } }");
		results = store.ExecuteQuery(query);
		if (results is IGraph)
		{
			//Print out the Results
			IGraph g = (IGraph)results;
			foreach (Triple t in g.Triples)
			{
				Console.WriteLine(t.ToString());
			}
			Console.WriteLine("Query took " + query.QueryExecutionTime.ToString());
		}
	}
}
```

The return type from the `ExecuteQuery()` method is an `Object` since it may be either a `SparqlResultSet` or an `IGraph` so you will need to do an `is` test and then cast as shown in the example. Alternatively you can use the `as` operator though we tend to avoid this ourselves since it can unintentionally hide errors in query evaluation.

As previously mentioned a common error here is that you get no results because you aren't querying the data you expect.  If you have a single graph you are querying just call `ExecuteQuery()` directly on the graph instead of creating an intermediary `TripleStore` which guarantees that your query operates over your graph.  If you have more than one graph we **strongly** recommend that you use the query processor method of querying instead since that requires you to explicitly configure your dataset which makes errors like these much less likely to happen.

If you are still having problems with no results you can also look at [[Debugging SPARQL Queries|HowTo-Debug-SPARQL-Queries]] for a method to debug what is happening with your query when using the in-memory SPARQL engine.

# Loading/Saving Results 

A `SparqlResultSet` may be loaded/saved using the [ISparqlResultsReader](https://dotnetrdf.github.io/api/html/T_VDS_RDF_ISparqlResultsReader.htm) and [ISparqlResultsWriter](https://dotnetrdf.github.io/api/html/T_VDS_RDF_ISparqlResultsWriter.htm) interfaces respectively. These are functionally very similar to the `IRdfReader` and `IRdfWriter` interfaces described on the [[Reading RDF|UserGuide-Reading-RDF]] and [[Writing RDF|UserGuide-Writing-RDF]] pages.

A quick example is as follows:

```csharp

using System;
using VDS.RDF;
using VDS.RDF.Query;

public class SaveLoadResultsExample
{
	public static void Main(String[] args)
	{
		//Create endpoint and make our query
		SparqlRemoteEndpoint endpoint = new SparqlRemoteEndpoint("http://dbpedia.org/sparql");
		SparqlResultSet results = endpoint.QueryWithResultSet("SELECT DISTINCT ?type WHERE { ?s a ?type } LIMIT 100");

		//Now save this to disk as SPARQL JSON
		SparqlJsonWriter writer = new SparqlJsonWriter();
		writer.Save(results, "example.srj");

		//We can then read this pack in again
		SparqlJsonReader reader = new SparqlJsonReader();
		SparqlResultSet results2 = new SparqlResultSet();
		reader.Load(results2, "example.srj");
	}
}
```

Note that one important difference between reading SPARQL results versus reading RDF is that you cannot read SPARQL results into a non-empty result set - doing so results in an exception.

----

# Tutorial Navigation 

The previous topic was [[Working with Triple Stores|UserGuide-Working-With-Triple-Stores]], the next topic is [[Updating with SPARQL|UserGuide-Updating-With-SPARQL]]

Users wishing to learn more may wish to jump straight to the following topics:
* [[Advanced SPARQL|UserGuide-Advanced-SPARQL]]
* [[Result Formatting|UserGuide-Result-Formatting|]]
* [[Triple Store Integration|UserGuide-Triple-Store-Integration]] 
* [[Configuration API - Query Processor|UserGuide-Configuration-Query-Processors]]