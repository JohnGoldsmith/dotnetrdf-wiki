[[Home]] > [[User Guide]] > [[UserGuide/Advanced SPARQL|Advanced SPARQL]] > [[UserGuide/Advanced SPARQL Operations|Advanced SPARQL Operations]]

= Advanced SPARQL Operations =

This article details advanced features of our of SPARQL implementation, if you are just interesting in using basic SPARQL see [[UserGuide/Querying with SPARQL|Querying with SPARQL]] and [[UserGuide/Updating with SPARQL|Updating with SPARQL]].

On this page we cover the following features:

* Custom Optimisers
* Thread-Safety
* Transactions

**Important** The functionality described here is implementation specific to dotNetRDF i.e. there are no guarantees that other implementations will support these features or handle the features detailed here in the same way.

= Custom Optimisers =

As discussed on the SPARQL Optimisation page our Leviathan SPARQL Engine has a fairly powerful optimiser built into it which aims to optimise queries as far as possible. Despite this you may sometimes want to control query optimisation yourself by injecting custom optimisers into the engine. There are two kinds of optimiser supported - Query Optimisers and Algebra Optimisers - and you can customise both if desired.

== Query Optimisers ==

Query Optimisers are applied to queries at the end of parsing, a Query Optimiser reorders triple patterns and places filters and assignments to try and optimise queries. A single query optimiser is applied, while you can apply subsequent optimisers this is not recommended and may cause unexpected results. The optimiser that is applied can be changed globally or locally.

The library includes three different query optimisers:

|= Optimizer |= Description |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Optimisation.DefaultOptimiser|DefaultOptimiser]] | The default optimiser which does reordering based on simple rules and places ##FILTER## and assignments |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Optimisation.NoReorderOptimiser|NoReorderOptimiser]] | An optimiser which doesn't reorder triple patterns but still places ##FILTER## and assignments |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Optimisation.WeightOptimiser|WeightedOptimiser]] | An optimiser which does the reordering based on weighting calculated from provided statistics about the data, also places ##FILTER## and assignments |

=== Global Query Optimiser ===

The global optimiser setting is changed by setting the //QueryOptimiser// property of the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Optimisation.SparqlOptimiser|SparqlOptimiser]] static class. This optimiser is used by all [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Parsing.SparqlQueryParser|SparqlQueryParser]] instances unless changed locally.

=== Local Query Optimiser ===

The local optimiser setting is controlled by setting the //QueryOptimiser// property of a ##SparqlQueryParser## instance. The local setting always overrides the global setting.

=== HTTP Handler Configuration ===

If you are configuring a SPARQL endpoint via our [[UserGuide/Configuration API|Configuration API]] you can configure the Query Optimiser to use via the ##dnr:queryOptimiser## property. See [[UserGuide/Configuration/HTTP Handlers|HTTP Handlers]] and [[UserGuide/Configuration/SPARQL Optimisers|Configuration API - Optimizers]] for details on how to do this.

== Algebra Optimisers ==

Algebra Optimisers are applied to queries when they are transformed into SPARQL Algebra and aim to replace the standard operators with optimised forms where possible. Multiple optimisers may be applied to the initial Algebra transformation of a query. Algebra Optimisers can be registered both globally and locally but local optimisers are applied ahead of global optimisers.

The library includes the following algebra optimisers which are automatically registered globally and applied in the following order:

|= Optimizer |= Purpose |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Optimisation.AskBgpOptimiser|AskBgpOptimiser]] | An optimiser which optimises the algebra form for ##ASK## queries to use the special operators where possible. These are operators designed to find the first possible solution and then return as that is sufficient for ##ASK## queries. |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Optimisation.LazyBgpOptimiser|LazyBgpOptimiser]] | An optimiser which optimises queries with ##LIMIT## clauses to use the special operators where possible. These are operators designed to find the requisite number of solutions and then return in order to minimise the work done. |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Optimisation.StrictAlgebraOptimiser|StrictAlgebraOptimiser]] | Transforms the basic generated algebra into the strict form as far as possible. This makes the algebra easier to traverse for subsequent optimisers. |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Optimisation.IdentityFilterOptimiser|IdentityFilterOptimiser]] | Optimises filters of the form ##FILTER(?x = ex:constant)## for more efficient evaluation. |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Optimisation.ImplicitJoinOptimiser|ImplicitJoinOptimiser]] | Optimises queries where a ##FILTER## embodies an implict join e.g. ##FILTER(?x = ?y)## or ##FILTER(SAMETERM(?x, ?y))## which can significantly improve performance. |
| [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Optimisation.FilteredProductOptimiser|FilteredProductOptimiser]] | Optimisers queries where a ##FILTER## occurs over a cross product to evaluate the filter as part of the cross product to improve performance. |

=== Global Algebra Optimisers ===

The global optimisers are changed by using the //AddOptimiser()// and //RemoveOptimiser()// methods of the ##SparqlOptimiser## static class. These optimisers are used when transforming any query into its Algebra form for evaluation.

=== Local Algebra Optimisers ===

The local optimisers are changed by setting the //AlgebraOptimisers// property on a ##SparqlQuery## instance. These optimisers are used when transforming the query into the Algebra form and apply ahead of any global optimisers.

=== HTTP Handler Configuration ===

If you are configuring a SPARQL endpoint via our [[UserGuide/Configuration API|Configuration API]] you can configure the Algebra Optimisers to use via the ##dnr:algebraOptimiser## property. See [[UserGuide/Configuration/HTTP Handlers|Configuration API - HTTP Handlers]] and [[UserGuide/Configuration/SPARQL Optimisers|Configuration API - Optimizers]] for details on how to do this.

= Thread-Safety =

Firstly our Leviathan SPARQL Engine which is used for all in-memory queries within the library ensures that Queries and Updates are thread-safe operations as far as possible. This thread-safety is MRSW (Multiple Reader Single Writer) so you can have either many queries running at one time or a single update running. Locking is all handled automatically so there should be no need to manage this yourself.

== When Thread-Safety applies ==

If the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Datasets.ISparqlDataset|ISparqlDataset]] you are using also implements the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Datasets.IThreadSafeDataset|IThreadSafeDataset]] interface then that dataset will be globally thread safe however many times you reuse it.

If it does not then the use of the dataset is thread safe only when used via a single Query/Update processor.

== Breaking Thread-Safety ==

While in principle queries and updates are thread safe it is possible to write code that will allow you to break this e.g. wrapping the same [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.IInMemoryQueryableStore|IInMemoryQueryableStore]] in multiple ##ISparqlDataset## instances. We strongly recommend that you avoid doing this as behaviour in such cases is unpredictable.

= Transactions =

Transactions are an advanced non-standard feature of our SPARQL Update implementation. Transactions track the sequence of actions that a [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Update.SparqlUpdateCommandSet|SparqlUpdateCommandSet]] performs and only commit/rollback the changes at the end of processing a command set. By default if you process commands individually (i.e. by calling the relevant //ProcessXCommand()// method directly) these are auto-committed unless you change the //AutoCommit// property for the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Update.LeviathanUpdateProcessor|LeviathanUpdateProcessor]] you are using.

Like thread-safety you do not need to do anything special to use transactions unless you want to control them in detail. Calling the //Flush()// or //Discard()// method on a ##ISparqlDataset## that supports transactions will have the effect of committing or rolling back the current Transaction (if any)

For example if you tried to process the following commands an error would be thrown and any temporary changes made to the state of the dataset would be rolled back:

{{{
CREATE GRAPH <http://example.org/graph>;
CREATE GRAPH <http://example.org/graph>
}}}

== When Transactions apply ==

Transactions apply when using a ##ISparqlDataset## implementation which derives from [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Datasets.BaseTransactionalDataset|BaseTransactionalDataset]] or [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Datasets.BaseTransactionalQuadDataset|BaseTranscationalQuadDataset]]. Note that 3rd party implementations may implement transactions without using this base class.