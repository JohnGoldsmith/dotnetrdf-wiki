[[User Guide|UserGuide]] > [[JSON-LD API|UserGuide-JsonLd-Api]] > Processor Options

# Processor Options

The class `VDS.RDF.JsonLd.JsonLdProcessorOptions` is used to pass various processing options to the JSON-LD APIs. This class exposes each processor option as a property with getter and setter.

| Option Property | Purpose
|-----------------|-----------|
| Base            | If provided, this URI overrides the base URI of the document being processed for the purposes of relative URI resolution.
| Loader          | If provided, this function will be used to resolve a URI and load a JSON document from it. See below for more details about implementing a custom loader.
| ProcessingMode  | Sets the processing mode for the JSON-LD processor. See below for more details.
| ExpandContext   | Sets a context that is used to initialize the active context when expanding a document. This value may be a JValue (with a string value which is the URI of the context to be retrieved), a JObject (representing a parsed context), or a JArray of JValue or JObject items.
| CompactArrays   | A boolean flag indicating if arrays of one element should be replaced by a single value during compaction. Defaults to `true`.
| ProduceGeneralizedRdf | A boolean flag indicating if the JSON-LD processor may emit blank nodes for triple predicates. If this option is false, then triples with blank node predicates will be omitted. Defaults to `true`.
| Embed | Sets the default value object embed flag used in the Framing Algorithm. Defaults to `JsonLdEmbed.Last`
| Explicit | A boolean flag indicating if explicit inclusion is enabled in the Framing Algorithm. Under explicit inclusion, only properties that are mentioned in a frame are included in the output. Default to `false`.
| RequireAll | A boolean flag controlling how frames are matched in the Framing Algorithm. When true, all non-keyword properties must match the frame for a node to match. When false, any of the non-keyword properties may match the frame for a node to match. Defaults to `false`.
| FrameDefault | A boolean flag indicating if the JSON-LD processor should frame the default graph only (when true) or a merged graph (when false). Defaults to `false`.
| PruneBlankNodeIdentifiers | A boolean flag indicating if the Framing operation should remove unreferenced blank node identifiers from the output. Defaults to `true`.

## Specifying Processor Mode

There are three supported JSON-LD processing modes:

* `JsonLd10` - JSON-LD 1.0 processing only. JSON-LD 1.1 features are not supported and if encountered in an input document will result in a `JsonLdProcessorException` being raised with the error code `ProcessingModeConflict`.
* `JsonLd11` - JSON-LD 1.1 processing mode. This is the default processing mode.
* `JsonLd11FrameExpansion` - JSON-LD 1.1 processing with frame expansion options enabled. This mode is only useful when expanding a JSON-LD frame document as it will cause the expansion processor to retain certain features that are unique to JSON-LD frame documents.

## Custom JSON Loader

The `Loader` property of `JsonLdProcessorOptions` allows the developer to override the default remote JSON loading functionality of the JsonLdProcessor. It is a function that takes a `System.Uri` as its only parameter and returns a `VDS.RDF.JsonLd.RemoteDocument` instance representing the loaded JSON. By implementing a custom loader function it is possible to implement your own resolution logic such as redirecting requests to another location or using  a local cache.

The RemoteDocument class has three properties of which two MUST be set and one MAY be set:

| RemoteDocument Property | Value Expected
|-------------------------|------------------------|
| Document                | REQUIRED. The JSON document. The value may be either a `Newtonsoft.Json.Linq.JToken` or a string. If the value is a string, the document will be parsed from the string provided.
| DocumentUrl             | REQUIRED. The final URL of the loaded document, taking into account any redirects. This is required for proper relative URI resolution during processing.
| ContextUrl              | OPTIONAL. If the response from the remote server contains an HTTP Link Header (RFC5988) using the http://www.w3.org/ns/json-ld#context link relation in the response, then the ContextUrl property should be set to that value *unless* the response content type is `application/ld+json` (in which case the link header is ignored). NOTE: it is an error for the server to return multiple context link headers and this should be indicated by having the loader raise a `JsonLdProcessorException` with the error code `JsonLdErrorCode.MultipleContextLinkHeaders`