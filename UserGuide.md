[[Home]] > User Guide

# dotNetRDF User Guide

Welcome to the dotNetRDF user guide, this provides an introduction to dotNetRDF and aims to cover how to carry out a variety of common tasks in dotNetRDF.  Using this guide you can learn the basics of working with the library in order to enable you the user to build applications using dotNetRDF.

You may also be interested in our [[FAQs]] or our quick [[How To|HowTo]] guides.

If you are already an experienced dotNetRDF user you may wish to look at the [[Developer Guide|DeveloperGuide]] instead which covers project architecture and advanced topics.

## Basic Tutorial

This series of pages aims to introduce you to the core concepts of dotNetRDF and get you up and running with the library, reading in order is suggested for new users:

1. [[Getting Started|UserGuide-Getting-Started]]
1. [[Library Overview|UserGuide-Library-Overview]]
1. [[Hello World|UserGuide-Hello-World]]
1. [[Reading RDF|UserGuide-Reading-RDF]]
1. [[Writing RDF|UserGuide-Writing-RDF]]
1. [[Working with Graphs|UserGuide-Working-With-Graphs]]
1. [[Typed Values and Lists|UserGuide-Typed-Values-And-Lists]]
1. [[Working with Triple Stores|UserGuide-Working-With-Triple-Stores]]
1. [[Querying with SPARQL|UserGuide-Querying-With-SPARQL]]
1. [[Updating with SPARQL|UserGuide-Updating-With-SPARQL]]

## General Topics

The following pages cover some general topics about the library:

* [[Exceptions|UserGuide-Exceptions]]
* [[Equality and Comparison|UserGuide-Equality-And-Comparison]]
* [[Event Model|UserGuide-Event-Model]]
* [[Utility Methods|UserGuide-Utility-Methods]]
* [[Extension Methods|UserGuide-Extension-Methods]]
* [[Using the Namespace Mapper|UserGuide-Using-The-Namespace-Mapper]]

## 3rd Party Triple Store Integration

We provide integration with a variety of 3rd party triple stores, see the following topics:

* [[Storage API|UserGuide-Storage-API]]
  * [[Triple Store Integration|UserGuide-Triple-Store-Integration|]]
  * [[Storage Providers|UserGuide-Storage-Providers]]
  * [[Servers API|UserGuide-Storage-Servers]]
  * [[Transactions API|UserGuide-Storage-Transactions]]

## SPARQL Features

The basic tutorial covers simple SPARQL query and updates, we have a selection of topics on advanced SPARQL features available:

* [[Advanced SPARQL|UserGuide-Advanced-SPARQL]]
  * [[Result Formatting|UserGuide-Result-Formatting]]
  * [[SPARQL Datasets|UserGuide-SPARQL-Datasets]]
  * [[Full Text Querying with SPARQL|UserGuide-Full-Text-Querying-With-SPARQL]]
  * [[Advanced SPARQL Operations|UserGuide-Advanced-SPARQL-Operations]]

The [[SPARQL Engine|DeveloperGuide-SPARQL-Engine]] section of the [[Developer Guide|DeveloperGuide]] may also be relevant to advanced users.

## Ontologies, Inference and Reasoning

Please see the following for documentation of ontology, inference and reasoning support:

* [[Ontology API|UserGuide-Ontology-API]]
* [[Inference and Reasoning|UserGuide-Inference-And-Reasoning]]

## ASP.Net Integration

Please see the [[ASP.Net Integration|UserGuide-ASPNET-Integration]] page for an overview of how we integrate into ASP.Net applications, or you can jump to specific topics below:

* [[Creating SPARQL Endpoints|UserGuide-ASP-Creating-SPARQL-Endpoints]]
* [[Deploying with rdfWebDeploy|UserGuide-ASP-Deploying-With-rdfWebDeploy]]

## Advanced APIs

The following documentation covers what are considered advanced topics but which may still be of value to everyday users of dotNetRDF.

* [[Global Options|UserGuide-Global-Options]]
* [[Formatting API|UserGuide-Formatting-API]]
* [[Configuration API|UserGuide-Configuration-API]]
* [[Handlers API|UserGuide-Handlers-API]]
* [[JSON-LD API|UserGuide-JsonLd-API]]

## Tools

See the [[Tools|UserGuide-Tools]] page for documentation pertaining to our GUI and command line tools.

* [[rdfConvert|UserGuide-Tools-rdfConvert]] 
* [[rdfEditor|UserGuide-Tools-rdfEditor]] 
* [[rdfOptStats|UserGuide-Tools-rdfOptStats]] 
* [[rdfQuery|UserGuide-Tools-rdfQuery]]
* [[rdfServer|UserGuide-Tools-rdfServer]] 
  * [[rdfServerGui|UserGuide-Tools-rdfServerGui]]
* [[rdfWebDeploy|UserGuide-Tools-rdfWebDeploy]] 
* [[soh|UserGuide-Tools-soh]]
* [[SparqlGui|UserGuide-Tools-SparqlGui]]
* [[Store Manager|UserGuide-Tools-Store-Manager]]

## Notes

Note that where we refer to the user in this guide we are referring to you the developer who is using the API.

All examples in the User Guide are given using C#.Net