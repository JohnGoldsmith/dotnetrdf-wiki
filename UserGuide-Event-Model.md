[[Home]] > [[User Guide|UserGuide]] > Event Model

# Event Model

dotNetRDF makes limited use of events itself but various classes generated useful events that you can attach handlers to as desired, this document gives a quick overview of the various events available.

## Warning Events

The parsers and serializers for RDF all support `Warning` or `StoreWarning` events which are used to notify of non-fatal issues with RDF being read/written. This simply output a String which contains a message describing the issue with the RDF and what action the parser has taken (if appropriate).

## Data Model Events

dotNetRDF raises a variety of events which bubble up internally through multiple layers:

### Triple Store Events

| Event | Occurrence |
|-------|------------|
| `ITripleStore.GraphAdded` | Occurs when a Graph is added to a Store
| `ITripleStore.GraphRemoved` | Occurs when a Graph is removed from a Store
| `ITripleStore.GraphChanged` | Occurs when a Graph is changed. This event is a bubbled event triggered by the `IGraph.GraphChanged` event |
| `ITripleStore.GraphCleared` | Occurs when a Graph is cleared.  This event is a bubbled event triggered by the `IGraph.GraphCleared` event |
| `ITripleStore.GraphMerged` | Occurs when a Graph has a merge operation performed on it

### Graph Events

| Event | Occurrence |
|-------|------------|
| `IGraph.TripleAsserted` | Occurs when a Graph has a Triple asserted in it.  This event is a bubbled event triggered by the `BaseTripleCollection.TripleAdded` event |
| `IGraph.TripleRetracted` | Occurs when a Graph has a Triple retracted from it.  This event is a bubbled event triggered by the `BaseTripleCollection.TripleRemoved` event |
| `IGraph.Changed` | Occurs when a Graph changes |
| `IGraph.ClearRequested` | Occurs when a `Clear()` operation is requested on a Graph |
| `IGraph.Clear` | Occurs when a Graph is cleared |
| `IGraph.MergeRequested` | Occurs when a `Merge()` operation is requested on a Graph |
| `IGraph.Merged` | Occurs when a Graph is merged |

### Triple Collection Events

| Event | Occurrence |
|-------|------------|
| `BaseTripleCollection.TripleAdded` | Occurs when a Triple is added to the collection |
| `BaseTripleCollection.TripleRemoved` | Occurs when a Triple is removed from the collection |