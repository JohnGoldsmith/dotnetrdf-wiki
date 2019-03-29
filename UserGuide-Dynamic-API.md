[[Home]] > [[User Guide|UserGuide]] > Dynamic API

# Dynamic API 
Nodes in a graph can be thought of as dictionaries (maps, hashes) where keys are predicates of outgoing statements (where given node is subject) and values are collections of objects of those statements.

For example the following graph
```turtle
@prefix :<http://example.com/> .

:s
  :p1
    :o1,  # t1
    "o2", # t2
    [];   # t3
  :p2
    :o4,  # t4
    "o5", # t5
    [].   # t6
```

![File](UserGuide-Dynamic-API/1.svg)

is eqivalent to this dictionary:
```csharp
var f = new NodeFactory();
var p1 = f.CreateUriNode(new Uri("http://example.com/p1"));
var o1 = f.CreateUriNode(new Uri("http://example.com/o1");
var o2 = f.CreateLiteralNode("o2");
var o3 = f.CreateBlankNode();
var p2 = f.CreateUriNode(new Uri("http://example.com/p2"));
var o4 = f.CreateUriNode(new Uri("http://example.com/o4");
var o5 = f.CreateLiteralNode("o5");
var o6 = f.CreateBlankNode();

var s = new Dictionary<INode, ICollection<INode>>
{
  {
    p1,
    new INode[]
    {
      o1, // t1
      o2, // t2
      o3, // t3
    }
  },
  {
    p2,
    new INode[]
    {
      o4, // t4
      o5, // t5
      o6, // t6
    }
  },
};
```

In the case of resource objects (IRI or blank nodes, not literals), the process can be repeated recursively, i.e. items in the value collection are dictionaries themselves.

For example the following graph
```turtle
@prefix :<http://example.com/> .

:s1
  :p
    :s2,  # t1
    _:s3. # t2

:s2
  :p
    "o1". # t3

_:s3
  :p
    "o2". # t4
```

![File](UserGuide-Dynamic-API/2.svg)

is eqivalent to the this dictionary:
```csharp
var f = new NodeFactory();
var p = f.CreateUriNode(new Uri("http://example.com/p"));
var o1 = f.CreateLiteralNode("o1");
var o2 = f.CreateLiteralNode("o2");

var s1 = new Dictionary<INode, ICollection<object>>
{
  {
    p,
    new object[]
    {
      new Dictionary<INode, ICollection<object>> // t1
      {
        p,
        new INode[]
        {
          o1, // t3
        }
      },
      new Dictionary<INode, ICollection<object>> // t2
      {
        p,
        new INode[]
        {
          o2, // t4
        }
      }
    }
  },
};
```

Literal objects on the other hand can be translated to their corresponding primitive types.

For example the following graph
```turtle
@prefix : <http://example.com/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

:s
  :p
    "o" ,                                              # t1
    true ,                                             # t2
    9223372036854775807 ,                              # t3
    1.79769313486232E+308 ,                            # t4
    "255"^^xsd:unsignedByte ,                          # t5
    "9999-12-31T23:59:59.999999+00:00"^^xsd:dateTime , # t6
    "79228162514264337593543950335"^^xsd:decimal ,     # t7
    "3.402823E+38"^^xsd:float ,                        # t8
    "P10675199DT2H48M5.4775807S"^^xsd:duration .       # t9
```

![Filex](UserGuide-Dynamic-API/3.svg)

is eqivalent to this dictionary:
```csharp
var f = new NodeFactory();
var p = f.CreateUriNode(new Uri("http://example.com/p"));

var s = new Dictionary<INode, ICollection<object>>
{
  {
    p,
    new object[]
    {
      "o",                     // t1
      true,                    // t2
      long.MaxValue,           // t3
      double.MaxValue,         // t4
      byte.MaxValue,           // t5
      DateTimeOffset.MaxValue, // t6
      decimal.MaxValue,        // t7
      float.MaxValue,          // t8
      TimeSpan.MaxValue        // t9
    }
  },
};
```











We can further 
```csharp
var s = new Dictionary<Uri, ICollection<INode>>
{
  { new Uri("http://example.com/p1"), new[] { "o1", "o2" } },
  { new Uri("http://example.com/p2"), new[] { "o3", "o4" } },
};

```

```csharp
var s = new Dictionary<string, ICollection<INode>>
{
  { "http://example.com/p1", new[] { "o1", "o2" } },
  { "http://example.com/p2", new[] { "o3", "o4" } },
};

```

```csharp
var s = new Dictionary<string, ICollection<INode>>
{
  { "p1", new[] { "o1", "o2" } },
  { "p2", new[] { "o3", "o4" } },
};

```


## section 

description