[[Home]] > [[Developer Guide]] > [[DeveloperGuide/SPARQL Engine|SPARQL Engine]]

= SPARQL Engine =

This page acts as a hub for topics about our SPARQL Engine, if you are interested in learning how to make queries and updates please see the [[UserGuide/Querying with SPARQL|Querying with SPARQL]] and [[UserGuide/Updating with SPARQL|Updating with SPARQL]] pages.

== Leviathan ==

Leviathan is the code name for our block based in-memory SPARQL evaluation engine, it has the following capabilities:

* Execution
** Full SPARQL 1.0 and SPARQL 1.1 queries and updates are supported. Please see the [[http://www.dotnetrdf.org/content.asp?pageID=Known%20Issues|Known Issues]] page for current issues with our SPARQL support.  Also see [[DeveloperGuide/SPARQL/Conformance|SPARQL Conformance]] for current conformance status.
** Support for adding [[DeveloperGuide/SPARQL/Extension Functions|Extension Functions]] per the [[http://www.w3.org/TR/sparql11-query/#extensionFunctions|SPARQL specification]]
* Optimisation
** Powerful query optimizations described on the [[DeveloperGuide/SPARQL/SPARQL Optimization|SPARQL Optimization]] page
* Extensions
** Full Text Query, see [[UserGuide/Full Text Querying with SPARQL|Full Text Querying with SPARQL]]
** ##LET## assignments permitted with semantics equivalent to [[http://jena.apache.org/documentation/query/index.html|ARQ]]
** Additional ##NMAX## and ##NMIN## aggregates (Numeric Maximum and Minimum)
** Additional ##MEDIAN and ##MODE## aggregates
** Additional [[DeveloperGuide/SPARQL/Function Libraries|Function libraries]]
*** Support for some of the [[DeveloperGuide/SPARQL/XPath Functions|XPath function library]]
*** Support for the [[http://jena.apache.org/documentation/query/library-function.html|ARQ function library]]
*** Support for our own [[DeveloperGuide/SPARQL/Leviathan Functions|Leviathan function library]]

You can learn more about this engine on the [[DeveloperGuide/SPARQL/Leviathan Engine|Leviathan Engine]] page.

== Further Reading ==

* [[DeveloperGuide/SPARQL/SPARQL Optimization|SPARQL Optimization]]
** [[DeveloperGuide/SPARQL/Implementing Custom Optimizers|Implementing Custom Optimizers]]
* [[DeveloperGuide/SPARQL/SPARQL Extensions|SPARQL Extensions]]
* [[DeveloperGuide/SPARQL/Performance|SPARQL Performance]]
* [[DeveloperGuide/SPARQL/Conformance|SPARQL Conformance]]