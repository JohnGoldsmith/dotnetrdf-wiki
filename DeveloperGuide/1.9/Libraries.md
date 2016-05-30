[Home](../../Home) > [Developer Guide](../../Developer%20Guide) > [1.9 Alpha](Introduction) > [Libraries](Libraries)

# 1.9 Alpha Documentation

**Warning** You are viewing documentation for the 1.9 Alpha series of dotNetRDF releases.  The features and APIs described within these documents are currently undergoing active development and should be considered unstable and subject to change without notice.  If you are looking for the stable release documentation please see visit the [Documentation Overview](../../Home)

----

# Libraries

The 1.9 Alpha releases break the existing functionality of the `dotNetRDF` library out into a number of different libraries.  This page provides the list of the currently available libraries and the functionality they provide.  Note that we are still in the early stages of the 1.9 development and much functionality has yet to be ported forward to the new APIs.

Note that for dependencies we describe only the immediate dependencies of that library.  All libraries are available as NuGet packages which will help manage the transitive dependencies needed if you are using libraries higher up the stack.

## dotNetRDF.Core

**Dependencies:** *None*

The `dotNetRDF.Core` library provides what you might think of as the Core APIs.  This includes  the basic APIs for representing RDF data as well as some interfaces and helpers that we expect to be used across the entire stack:

* `INode`, `Triple`, `Quad`, `IGraph`, `IGraphStore` and `IQuadStore` APIs
* `INamespaceMapper` APIs
* Supporting interfaces for IO and Configuration APIs

## dotNetRDF.IO.Core

**Dependencies:** `dotNetRDF.Core`

The `dotNetRDF.IO.Core` library provides the Core IO APIs.  These are the APIs for reading and writing RDF data from streams and generally for presenting RDF data in both human & machine readable formats.

## dotNetRDF.IO.Json

**Dependencies:** `dotNetRDF.IO.Core`, `Newtonsoft.Json`

The `dotNetRDF.IO.Json` library provides additional implementations of the IO APIs that utilise JSON.  By breaking this out into its own library we avoid the need for users who don't want to use JSON IO to have any dependency on `Newtonsoft.Json`

## dotNetRDF.Sparql.Core

**Dependencies:** `dotNetRDF.Core` *More??*

The `dotNetRDF.Sparql.Core` library provides the Core SPARQL APIs.  This includes the APIs for representing SPARQL Queries and our in-memory SPARQL engine.

**NB** - In future we may break this library out further into smaller sub-modules.