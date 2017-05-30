# JSON-LD API

The namespace `VDS.RDF.JsonLd` contains classes related to the processing of JSON-LD documents. This includes methods to expand, compact and frame JSON-LD documents. For more information about what these terms mean please refer to the [JSON-LD Specification](https://json-ld.org/spec/latest/json-ld/). However, please note that for consistency with other RDF formats supported by dotNetRDF, the parser and writer are found in the namespaces `VDS.RDF.Parsing` and `VDS.RDF.Writing` respectively.

The APIs we implement are based on the [JSON-LD API Specification](https://json-ld.org/spec/latest/json-ld-api/), and are implemented as static methods on the class `VDS.RDF.JsonLd.JsonLdProcessor`. Where JSON objects or arrays are passed through the APIs, we use the Newtonsoft.JSON library's LINQ APIs to represent those objects.

## Features

* [[Expansion|UserGuide-JsonLd-Expansion]]
* [[Compaction|UserGuide-JsonLd-Compaction]]
* [[Flattening|UserGuide-JsonLd-Flattening]]
* [[Framing|UserGuide-JsonLd-Framing]]
* [[Processor Configuration|UserGuide-JsonLd-Configuration]]
* [[Error Handling|UserGuide-JsonLd-ErrorHandling]]
