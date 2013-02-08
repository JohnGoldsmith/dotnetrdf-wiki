[[Home]] > [[User Guide]] > [[User Guide/SPARQL Datasets|SPARQL Datasets]]

= SPARQL Datasets =

A SPARQL dataset refers to the dataset notion from the SPARQL specification, within dotNetRDF it may refer to the concrete ISparqlDataset interface which represents datasets for in-memory queries or it may refer to the dataset description associated with a query.

== Query Dataset Description ==

The dataset description of a query consists of the ##FROM## and ##FROM NAMED## clauses present in the query, these indicate to a query engine which graph(s) to use and where to use them when answering queries.

Graphs specified in the ##FROM## clause are used to form the default graph, this is the graph that queries operate over except when a ##GRAPH## clause is encountered.

Graphs specified in the ##FROM NAMED## clauses are graphs that may be accessed using a ##GRAPH## clause in your query.

== ISparqlDataset ==