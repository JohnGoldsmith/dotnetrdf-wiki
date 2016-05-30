[[Home]] > [[User Guide]]

# dotNetRDF User Guide

Welcome to the dotNetRDF user guide, this provides an introduction to dotNetRDF and aims to cover how to carry out a variety of common tasks in dotNetRDF.  Using this guide you can learn the basics of working with the library in order to enable you the user to build applications using dotNetRDF.

You may also be interested in our [[FAQs]] or our quick [[How To]] guides.

If you are already an experienced dotNetRDF user you may wish to look at the [[Developer Guide]] instead which covers project architecture and advanced topics.

## Basic Tutorial

This series of pages aims to introduce you to the core concepts of dotNetRDF and get you up and running with the library, reading in order is suggested for new users:

# [[UserGuide/Getting Started|Getting Started]]
# [[UserGuide/Library Overview|Library Overview]]
# [[UserGuide/Hello World|Hello World]]
# [[UserGuide/Reading RDF|Reading RDF]]
# [[UserGuide/Writing RDF|Writing RDF]]
# [[UserGuide/Working with Graphs|Working with Graphs]]
# [[UserGuide/Typed Values and Lists|Typed Values and Lists]]
# [[UserGuide/Working with Triple Stores|Working with Triple Stores]]
# [[UserGuide/Querying with SPARQL|Querying with SPARQL]]
# [[UserGuide/Updating with SPARQL|Updating with SPARQL]]

## General Topics

If you want to look up a specific namespace, class or method in the API please see the [[http://www.dotnetrdf.org/api/|Formal API documentation]] which is MSDN style documentation of our libraries.

The following pages cover some general topics about the library:

* [[UserGuide/Exceptions|Exceptions]]
* [[UserGuide/Equality and Comparison|Equality and Comparison]]
* [[UserGuide/Event Model|Event Model]]
* [[UserGuide/Utility Methods|Utility Methods]]
* [[UserGuide/Extension Methods|Extension Methods]]
* [[UserGuide/Using the Namespace Mapper|Using the Namespace Mapper]]

## 3rd Party Triple Store Integration

We provide integration with a variety of 3rd party triple stores, see the following topics:

* [[UserGuide/Storage API|Storage API]]
** [[UserGuide/Triple Store Integration|Triple Store Integration]]
** [[UserGuide/Storage/Providers|Storage Providers]]
** [[UserGuide/Storage/Servers|Servers API]]
** [[UserGuide/Storage/Transactions|Transactions API]]

## SPARQL Features

The basic tutorial covers simple SPARQL query and updates, we have a selection of topics on advanced SPARQL features available:

* [[UserGuide/Advanced SPARQL|Advanced SPARQL]]
** [[UserGuide/Result Formatting|Result Formatting]]
** [[UserGuide/SPARQL Datasets|SPARQL Datasets]]
** [[UserGuide/Full Text Querying with SPARQL|Full Text Querying with SPARQL]]
** [[UserGuide/Advanced SPARQL Operations|Advanced SPARQL Operations]]

The [[DeveloperGuide/SPARQL%20Engine|SPARQL Engine]] section of the [[Developer%20Guide|Developer Guide]] may also be relevant to advanced users.

## Ontologies, Inference and Reasoning

Please see the following for documentation of ontology, inference and reasoning support:

* [[UserGuide/Ontology API|Ontology API]]
* [[UserGuide/Inference and Reasoning|Inference and Reasoning]]

## ASP.Net Integration

Please see the [[UserGuide/ASP.Net Integration|ASP.Net Integration]] page for an overview of how we integrate into ASP.Net applications, or you can jump to specific topics below:

* [[UserGuide/ASP/Creating SPARQL Endpoints|Creating SPARQL Endpoints]]
* [[UserGuide/ASP/Deploying with rdfWebDeploy|Deploying with rdfWebDeploy]]

## Advanced APIs

The following documentation covers what are considered advanced topics but which may still be of value to everyday users of dotNetRDF.

* [[UserGuide/Global Options|Global Options]]
* [[UserGuide/Formatting API|Formatting API]]
* [[UserGuide/Configuration API|Configuration API]]
* [[UserGuide/Handlers API|Handlers API]]

## Tools

See the [[UserGuide/Tools|Tools]] page for documentation pertaining to our GUI and command line tools.

* [[UserGuide/Tools/rdfConvert|rdfConvert]] 
* [[UserGuide/Tools/rdfEditor|rdfEditor]] 
* [[UserGuide/Tools/rdfOptStats|rdfOptStats]] 
* [[UserGuide/Tools/rdfQuery|rdfQuery]]
* [[UserGuide/Tools/rdfServer|rdfServer]] 
  * [[UserGuide/Tools/rdfServerGui|rdfServerGui]]
* [[UserGuide/Tools/rdfWebDeploy|rdfWebDeploy]] 
* [[UserGuide/Tools/soh|soh]]
* [[UserGuide/Tools/SparqlGui|SparqlGui]]
* [[UserGuide/Tools/Store Manager|Store Manager]]

## Notes

Note that where we refer to the user in this guide we are referring to you the developer who is using the API.

All examples in the User Guide are given using C#.Net