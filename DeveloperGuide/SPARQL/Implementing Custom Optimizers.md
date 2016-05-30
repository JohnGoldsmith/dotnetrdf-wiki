[[Home]] > [[Developer Guide]] > [[DeveloperGuide/SPARQL Engine|SPARQL Engine]] > [[DeveloperGuide/SPARQL/Implementing Custom Optimizers|Implementing Custom Optimizers]]

= Implementing Custom Optimizers =

As discussed on the [[DeveloperGuide/SPARQL/SPARQL Optimization|SPARQL Optimization]] and [[UserGuide/Advanced SPARQL Operations|Advanced SPARQL Operations]] pages our [[DeveloperGuide/SPARQL/Leviathan Engine|Leviathan SPARQL Engine]] supports two kinds of optimisers which can be customised if desired.

= Implementing Query Optimizers =

Query Optimizers implement the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Optimisation.IQueryOptimiser|IQueryOptimiser]] interface which has a single Optimise() method which takes in a Graph Pattern to optimise and an enumeration of variables that have occurred prior to that Graph Pattern.

A Query Optimiser should do three things:

* Reorder Triple Patterns
* Place FILTERs appropriately
* Place assignments (i.e. ##BIND##) appropriately

Typically implementations will only want to alter how triple patterns are reordered and this can be done fairly easily be deriving your implementation from [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Optimisation.BaseQueryOptimiser|BaseQueryOptimiser]]. This class provides the outline for a Query Optimiser and you implement/override various methods and properties in order to customise it.

For example if you wanted to change how the triple patterns are ordered you'd derive from this class and them implement the //GetRankingComparer()// method. This method returns a ##IComparer<ITriplePattern>## which is used to order the set of [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Patterns.ITriplePattern|ITriplePattern]] instances found in the Graph Pattern.

**Important:** Note that ##BaseQueryOptimiser## implements a two-pass reordering by default. The second pass may change the initial ordering provided by your comparer, to disable the second pass you can override the //ShouldReorder// property to return false.

== Useful Methods ==

The following methods of GraphPattern are designed for use primarily by query optimisers:

* //SwapTriplePatterns()// swaps the position of two Triple Patterns in a Graph Pattern
* //InsertFilter()// inserts a Filter at a specific position in a Graph Pattern
* //InsertAssignment()// inserts an IAssignmentPattern at a specific position in a Graph Pattern

= Implementing Algebra Optimizers =

Algebra Optimisers implement the [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Optimisation.IAlgebraOptimiser|IAlgebraOptimiser]] interface which has an //Optimise()// method and an //IsApplicable()// method. The latter is used to determine whether an algebra optimisation can be applied to a query while the former actually applies the Optimiser.

Algebra Optimisers are typically used to transform the algebra to use special operators which help evaluate certain forms of query faster e.g. the built in [[http://www.dotnetrdf.org/api/index.asp?Topic=VDS.RDF.Query.Optimisation.AskBgpOptimiser|AskBgpOptimiser]] which transforms the algebra of ##ASK## queries so they evaluate much faster.  The [[UserGuide/Advanced SPARQL Operations|Advanced SPARQL Operations]] page details some of the default optimizers used.