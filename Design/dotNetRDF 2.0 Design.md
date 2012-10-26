# dotNetRDF 2.0 Design

As we approach dotNetRDF 1.0 we have been thinking a lot about the state of the API and what we would really like to change if we were able to introduce more significant refactors and breaking changes.  This thinking so far forms the basis for this design document, the idea is that we would make a number of significant and breaking changes in the 2.0 API in order to clean up, simplify and modularize the API better.

It is proposed that an initial version of this revised API be published as a transitional 1.9 release so that users can use the new API and provide feedback as it is developed.  Parallel to that we will continue to support the 1.0 release with bug fixes, it is expected that an initial 1.9 will not be ready until around the same time as 1.0 is released.

# API Modularization

Currently the Core API has become quite bloated and it would make sense to divide some features out into their own separate libraries, the rough proposal is as follows:

 - dotNetRDF.dll - Core data model, Parsing, Query, Update and Writing APIs
 - dotNetRDF.Ontology.dll - Ontology API, likely expanded
 - dotNetRDF.Storage.dll - Storage API
 - dotNetRDF.Storage.Virtuoso.dll - dotNetRDF.Data.virtuoso renamed
 - dotNetRDF.Web.dll - Web API
 - dotNetRDF.Web.Asp.dll - ASP.Net implementation of Web API

There are more radical refactors we could consider like splitting SPARQL off into it's own library but my preference is to keep it in the Core library.

# API Refactors

There are a number of API refactors we wish to make as part of a 2.0 release and these are detailed here, they range from minor clean up to fairly significant refactors.


## Data Model Refactor

The core data model has become somewhat large and in places overly complex with multiple ways of achieving the same thing, not to mention confusing and misleading interface names in many places.  There are a variety of refactors we are proposing all of which aim to simplify the API and in some cases reduce the memory footprint and complexity of their usage.

### Nodes Refactor

I propose that we refactor the Nodes API as follows:

 1. Remove the tight coupling between a Node and the Graph it originated from
 1. Allow for free moving of Nodes between different Graphs
 1. Identify Blank Nodes in a way that allows the above

