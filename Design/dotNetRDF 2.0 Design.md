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
 1. Don't require any work to create Node values to be done in Nodes
 1. Move all Node classes to the Nodes namespace

The first point means removing the Graph and GraphUri properties from the INode interface, doing so removes the coupling between a Node and its originating Graph.  This has particular benefits for things like creating wrappers around factories and graphs since it will eliminate some of the current issues we can encounter involving Graph reference mismatches.

Once this is done point 2 becomes easy because we can now freely copy Nodes around because they just represent node values provided we fix point 3.  As a result the existing CopyNode() methods can all be deprecated and ultimately removed.

Point 5 is primarily just a clean up activity to better organize the code going forward.

#### Blank Node Identification

Point 3 is somewhat trickier, currently we use user defined/auto-assigned string labels to identify blank nodes.  This has proved to be a poor implementation decision requiring tons of hacks and workarounds to have blank nodes be properly scoped and avoid collisions.  My initial though was to identify blank nodes by a combination of two Guids, one is the Node ID identifier and one the Factory identifier.  This means that Blank Nodes don't strictly meet point 1 because this means it is tied to the factory, in practice this is likely not be necessary.

Thus it will depend on whether we allow users to create Blank Nodes using an explicit Guid, if we do then we need the Factory ID to ensure correct scoping.  If we don't allow this we can likely get away with a single Guid since it is for all intents and purposes a unique identifier so will also provide inherent scoping.

One thing we will have to do in making this change is still allow users to create blank nodes by human readable label and simply map these consistently to Guids internally (within the scope of a factory).  This is required to keep parsers playing nice and likely makes minimal difference to memory footprint as we already keep a similar map to avoid collisions between user and auto-assigned identifiers.  Moving to Guids simply replaces that map with an alternative map.

#### Node Creation

Point 4 refers to the fact that internally some nodes are creating by passing a Graph reference and having them call back to the graph to get data such as new Blank Node ID or resolving the QName.  This again was a poor design decision and so we will remove the relevant constructors and instead require the Node Factory creating the Node to provide us with the Blank Node ID or resolved URI.


### Triple refactor

In light of the Nodes refactor we will make some similar changes to the Triple API:

 - Remove reference to the Graph on a Triple
 - Remove the rarely used Context parameter on a Triple

This is intended to simplify the Triple class, reduce it's memory footprint and get to a data model where it purely represents a Triple.  It also simplifies the constructor for Triple since we no longer need to validate that the Nodes originate from the same Graph.  Removing the Context parameter only really affects the rarely used N3 function contexts which is a feature we don't truly support anyway and so I would prefer to remove support for.

With the removal of the Graph property we need to introduce a properly Quad class to represent Quads, the Quad class will have essentially the same API as a Triple but with the addition of a Graph property which will return a Uri *not* a IGraph.

Implementation wise I intend to make Quad a standalone class not an extension because we don't want to allow implicit casting of Quad to Triple. Users should always be aware that this is a lossy operation, an AsTriple() method will be provided to do this explicitly.  Conversely Triple will likely provide an AsQuad(Uri graphUri) method.  This decision also means we can implement Quad as a decorator over a Triple allowing us to reduce memory footprint.

### IGraph Refactor

The Nodes refactor will result in a couple of minor changes to the IGraph API as it currently stands:

 - There will no longer need to be a two argument form of Merge() since Nodes don't have a reference to their Graph URI to be preserve
 - GetNextBlankNodeID() becomes obsolete and eventually removed

As a result of this and the Nodes refactor there will be some implementation benefits for IGraph.  For example Merge() becomes super simple, just Assert() the triples from one graph into the other since there is no need to worry about mapping Blank Node IDs.

The actual change we propose for the IGraph API are as follows:

 1. Rename the BaseUri property as the Name property since this more accurately describes it's purpose.  BaseUri would remain as an obsoleted synonym property in initial 
releases.
 1. Add a Quads property to retrieve the Triple's in Quad form

### ITripleStore/ISparqlDataset Refactor

The ITripleStore interface is both poorly named and a messy API, it also overlaps heavily with the ISparqlDataset API.  I suggest consolidating the functionality of the two APIs (while removing irrelevant functionality) into a single IGraphStore API.  The new name more accurately reflects the purpose and would have a cleaner API, excerpts of the proposed API are given below:

    IGraphStore
    {
      IEnumerable<Uri> GraphUris { get; }

      IEnumerable<IGraph> Graphs { get; }

      IGraph this[Uri u] { get; }

      bool Add(IGraph g);

      bool Add(IGraph g, Uri graphUri); //Add under the given named graph

      bool Copy(Uri srcUri, Uri destUri);

      bool Move(Uri srcUri, Uri destUri);

      bool Remove(Uri u);

      //Get all Triples with given Subject in given Graph(s), must eliminate duplicates if multiple graphs are specified
      IEnumerable<Triple> GetTriplesWithSubject(IEnumerable<Uri> graphUris, INode subj);

      //Get all Quads in the store
      IEnumerable<Quad> Quads { get; } 
  
      //Get all quads that match the given subject
      IEnumerable<Quad> GetQuadsWithSubject(INode subj)
    }

Note that while this interface outline is by no means complete it does not include the active and default graph management portions of the ISparqlDataset API.  It is proposed that the burden of tracking what constitutes those graphs is the job of the query engine.
