[[Home]] > [[Developer Guide]] > [[DeveloperGuide/SPARQL Engine|SPARQL Engine]] > [[DeveloperGuide/SPARQL/Function Libraries|Function Libraries]] > [[DeveloperGuide/SPARQL/XPath Functions|XPath Function Library]]

= XPath Function Library =

The XPath Function Library is a library of functions which form part of the W3C specification for XPath. Please follow the appropriate links to see the documentation for the function, we have noted where we don't support all forms of a function. As a general rule for string functions which have forms which support a collation argument we do not support that form.

**Note:** Many of these functions now have built-in equivalents in SPARQL 1.1 which often offer better functionality since they are slightly modified to work with RDF where appropriate.

= Namespace =

The namespace for XPath functions is ##http://www.w3.org/2005/xpath-functions/# ## and the preferred prefix is ##fn##

= Available Functions =

dotNetRDF supports a subset of this library which is as follows:

== Aggregate Functions ==

|= XPath Function |= SPARQL Equivalent |= SPARQL Version |= Description |
| [[http://www.w3.org/2005/xpath-functions/#string-join|fn:string-join()]] | ##GROUP_CONCAT()## | 1.1 | Joins the members of a group into a string with the given selector |

== Boolean Functions ==

|= XPath Function |= SPARQL Equivalent |= SPARQL Version |= Description |
| [[http://www.w3.org/2005/xpath-functions/#not|fn:not()]] | ##!## | 1.0 | Equivalent to the ##!## operator i.e. logical negation of the expression it is applied to |
| [[http://www.w3.org/2005/xpath-functions/#boolean|fn:boolean()]] | | 1.0 | Computes the effective boolean value of the expression it is applied to.  SPARQL automatically computes effective boolean value when necessary |
| [[http://www.w3.org/2005/xpath-functions/#true|fn:true()]] | ##true## | 1.0 | Equivalent to the plain literal ##true##
| [[http://www.w3.org/2005/xpath-functions/#false|fn:false()]] | ##false## | 1.0 | Equivalent to the plain literal false |

== Date Time Functions ==

|= XPath Function |= SPARQL Equivalent |= SPARQL Version |= Description |
| [[http://www.w3.org/2005/xpath-functions/#year-from-dateTime|fn:year-from-dateTime()]] | ##YEAR()## | 1.1 | Returns the year portion of a Date Time |
| [[http://www.w3.org/2005/xpath-functions/#month-from-dateTime|fn:month-from-dateTime()]] | ##MONTH()## | 1.1 | Returns the month portion of a Date Time |
| [[http://www.w3.org/2005/xpath-functions/#day-from-dateTime|fn:day-from-dateTime()]] | ##DAY()## | 1.1 | Returns the day portion of a Date Time |
| [[http://www.w3.org/2005/xpath-functions/#hours-from-dateTime|fn:hours-from-dateTime()]] | ##HOURS()## | 1.1 | Returns the hours portion of a Date Time |
| [[http://www.w3.org/2005/xpath-functions/#minutes-from-dateTime|fn:minutes-from-dateTime()]] | ##MINUTES()## | 1.1 | Returns the minutes portion of a Date Time |
| [[http://www.w3.org/2005/xpath-functions/#seconds-from-dateTime|fn:seconds-from-dateTime()]] | ##SECONDS()## | 1.1 | Returns the seconds portion of a Date Time |
| [[http://www.w3.org/2005/xpath-functions/#timezone-from-dateTime|fn:timezone-from-dateTime()]] | ##TIMEZONE()## and ##TZ()## | 1.1 | Returns the time zone of a Date Time |

== Numeric Functions ==

|= XPath Function |= SPARQL Equivalent |= SPARQL Version |= Description |
| [[http://www.w3.org/2005/xpath-functions/#abs|fn:abs()]] | ##ABS()## | 1.1 | Returns the absolute value of the expression |
| [[http://www.w3.org/2005/xpath-functions/#ceiling|fn:ceiling()]] | ##CEIL()## | 1.1 | Returns the value of the expression rounded up to a whole number |
| [[http://www.w3.org/2005/xpath-functions/#floor|fn:floor()]] | ##FLOOR()## | 1.1 | Returns the value of the expression rounded down to a whole number |
| [[http://www.w3.org/2005/xpath-functions/#round|fn:round()]] | ##ROUND()## | 1.1 | Returns the value of the expression rounded to the nearest whole number |
| [[http://www.w3.org/2005/xpath-functions/#round-half-to-even|fn:round-half-to-even()]] | | | Returns the value of the expression rounded to the nearest whole number, where the number is exactly between two whole numbers it rounds to the nearest even number. |

== String Functions ==

|= XPath Function |= SPARQL Equivalent |= SPARQL Version |= Description |
| [[http://www.w3.org/2005/xpath-functions/#matches|fn:matches()]] | ##REGEX()## | 1.0 | Equivalent to the ##REGEX## function |
| [[http://www.w3.org/2005/xpath-functions/#contains|fn:contains()]] | ##CONTAINS()## | 1.1 | Determines whether a given string contains another string. |
| [[http://www.w3.org/2005/xpath-functions/#starts-with|fn:starts-with()]] | ##STRSTARTS()## | 1.1 | Determines whether a given string starts with another string. |
| [[http://www.w3.org/2005/xpath-functions/#ends-with|fn:ends-with()]] | ##STRENDS()## | 1.1 | Determines whether a given string ends with another string. |
| [[http://www.w3.org/2005/xpath-functions/#string-length|fn:string-length()]] | ##STRLEN()## | 1.1 | Returns the length of the given string. |
| [[http://www.w3.org/2005/xpath-functions/#concat|fn:concat()]] | ##CONCAT()## | 1.1 | Concatenates any number of string arguments without a separator |
| [[http://www.w3.org/2005/xpath-functions/#substring|fn:substring()]] | ##SUBSTR()## | 1.1 | Returns a substring from a string, uses a 1 based index |
| [[http://www.w3.org/2005/xpath-functions/#substring-before|fn:substring-before()]] | ##STRBEFORE()## | 1.1 | Gets the part of the string that occurs before a given string |
| [[http://www.w3.org/2005/xpath-functions/#substring-after|fn:substring-after()]] | ##STRAFTER()## | 1.1 | Gets the part of the string that occurs before a given string |
| [[http://www.w3.org/2005/xpath-functions/#normalize-space|fn:normalize-space()]] | | | Normalizes space in a string |
| [[http://www.w3.org/2005/xpath-functions/#normalize-unicode|fn:normalize-unicode()]] | | | Normalizes a string to a given Unicode normalization form, not all forms are supported |
| [[http://www.w3.org/2005/xpath-functions/#upper-case|fn:upper-case()]] | ##UCASE()## | 1.1 | Converts a string to upper case |
| [[http://www.w3.org/2005/xpath-functions/#lower-case|fn:lower-case()]] | ##LCASE()## | 1.1 | Converts a string to lower case |
| [[http://www.w3.org/2005/xpath-functions/#encode-for-uri|fn:encode-for-uri()]] | ##ENCODE_FOR_URI()## | 1.1 | Encodes a String for use in a URI |
| [[http://www.w3.org/2005/xpath-functions/#escape-html-uri|fn:escape-html-uri()]] | | | Escapes a URI for insertion in HTML |
| [[http://www.w3.org/2005/xpath-functions/#replace|fn:replace()]] | ##REPLACE()## | 1.1 | Performs a regular expression based replace on strings |
| [[http://www.w3.org/2005/xpath-functions/#compare|fn:compare()]] | | | Compares two strings and returns -1, 0 or 1 to indicate their relative ordering |