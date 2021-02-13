[[User Guide|UserGuide]] > [[JSON-LD API|UserGuide-JsonLd-Api]] > Examples

# JSON-LD basic scenarios

This section is intended to provide a number of basic scenarios of processing JSON-LD, showing both the source data and corresponding queries.

## Overview Documents

* [[Namespaces|DeveloperGuide-Architecture-Namespaces]]
* [[Design|DeveloperGuide-Architecture-Design]]

## Sample data


The sample JSON-LD is based on the examples found at https://json-ld.org/playground/ so that you can easily compare the output there with the results of the queries that you write with dotNetRDF.


The following examples assume a local JSON-LD file named "Place.json" is present on your machine, with the following content:

```json
{
  "@context": {
    "name": "http://schema.org/name",
    "description": "http://schema.org/description",
    "image": {
      "@id": "http://schema.org/image",
      "@type": "@id"
    },
    "geo": "http://schema.org/geo",
    "latitude": {
      "@id": "http://schema.org/latitude",
      "@type": "xsd:float"
    },
    "longitude": {
      "@id": "http://schema.org/longitude",
      "@type": "xsd:float"
    },
    "xsd": "http://www.w3.org/2001/XMLSchema#"
  },
  "name": "The Empire State Building",
  "description": "The Empire State Building is a 102-story landmark in New York City.",
  "image": "http://www.civil.usherbrooke.ca/cours/gci215a/empire-state-building.jpg",
  "geo": {
    "latitude": "40.75",
    "longitude": "73.98"
  }
}
```





## Loading the data

There are a number of methods for loading JSON-LD data depending on where you are retrieving it from.  You can either invoke directly from the `Load` method on `JsonLdParser` or, use the extension methods available on `TripleStore`.  See the `LoadFrom*` methods section under [[Extension Methods|UserGuide-Extension-Methods]] and [[Reading RDF|UserGuide-Reading-RDF#reading-rdf-from-common-sources]] for more details.

#### LoadFromFile

```csharp
var targetPath = @"C:\Users\UserName\Documents\Place.json";

var parser = new JsonLdParser();
var store = new TripleStore();

store.LoadFromFile(targetPath, parser);
```


#### LoadFromString

```csharp
// Create string from file for purposes of this example
var targetPath = @"C:\Users\UserName\Documents\Place.json";
var jsonStr = File.ReadAllText(targetPath);

var parser = new JsonLdParser();
var store = new TripleStore();

store.LoadFromString(jsonStr, parser);
```

In both of the above cases, creating the JsonLdParser object isn't required, but it does allow for the inclusion of an options object to change the default parser settings if needed.  See [[JsonLd ProcessorOptions|UserGuide-JsonLd-JsonLdProcessorOptions]] for more details.


## Expanding the data

Although not required, it can be useful to see the expanded version of your JSON-LD data and you can use the static `Expand` on `JsonLdProcessor` to help with this: 


```csharp
// jsonStr from 'LoadFromString' above
var strRdr = new StringReader(jsonStr);

JToken element;
using (var reader = new JsonTextReader(strRdr) { DateParseHandling = DateParseHandling.None })
{
	element = JToken.ReadFrom(reader);
}
var expandedElement = JsonLdProcessor.Expand(element);

Console.WriteLine(expandedElement.ToString());
```

This outputs the following json:

```json
{
  "http://schema.org/name": [
    {
      "@value": "The Empire State Building"
    }
  ],
  "http://schema.org/description": [
    {
      "@value": "The Empire State Building is a 102-story landmark in New York City."
    }
  ],
  "http://schema.org/image": [
    {
      "@id": "http://www.civil.usherbrooke.ca/cours/gci215a/empire-state-building.jpg"
    }
  ],
  "http://schema.org/geo": [
    {
      "http://schema.org/latitude": [
        {
          "@value": "40.75",
          "@type": "http://www.w3.org/2001/XMLSchema#float"
        }
      ],
      "http://schema.org/longitude": [
        {
          "@value": "73.98",
          "@type": "http://www.w3.org/2001/XMLSchema#float"
        }
      ]
    }
  ]
}
```

As you can see, all of the prefixes that were contained in the context have been expanded and properties now include JSON-LD keywords such as @value highlighting the difference between node objects (as URI nodes) and value objects (as Literal nodes). 




## Querying the data

There are three main approaches to querying with dotNetRDF:

* Nodes (see *Selecting Nodes* section in [[Working With Graphs|UserGuide-Working-With-Graphs]])
* Triples (see *Selecting Triples* section in [[Working With Graphs|UserGuide-Working-With-Graphs]])
* SPARQL (see [[Querying with SPARQL|UserGuide-Querying-With-SPARQL]])

We'll have a look at each of these but first, we'll get hold of the default graph object from the `store` (see 'Loading the data' section above):

```csharp
IGraph g = store.Graphs.FirstOrDefault();
```

#### Working with Nodes

