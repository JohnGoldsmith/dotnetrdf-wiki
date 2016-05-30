[[Home]] > [[Developer Guide]] > [[DeveloperGuide/SPARQL Engine|SPARQL Engine]] > [[DeveloperGuide/SPARQL/Function Libraries|Function Libraries]]

= Function Libraries =

Function Libraries are collections of extension functions that may be used in SPARQL queries and updates.  These are supported by our in-memory [[DeveloperGuide/SPARQL/Leviathan Engine|Leviathan Engine]] and some may be supported by other triple stores as well.

If you are looking to add your own extension functions please see the [[DeveloperGuide/SPARQL/Extension%20Functions|Extension Functions]] documentation for details on how to do that.

|= Function Library |= Description |
| [[http://jena.apache.org/documentation/query/library-function.html|ARQ]] | The ARQ function library contains several useful extension functions |
| [[DeveloperGuide/SPARQL/XPath Functions|XPath]] | The XPath Function library has a wide variety of functions, many of these now have SPARQL equivalents as of SPARQL 1.1 |
| [[DeveloperGuide/SPARQL/Leviathan Functions|Leviathan]] | Our own function library with a number of useful numeric and trigonometric functions |