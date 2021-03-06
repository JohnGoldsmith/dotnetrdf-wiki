[[Home]] > [[User Guide|UserGuide]] > [[Configuration API|UserGuide-Configuration-API]] > [[SPARQL Optimisers|UserGuide-Configuration-SPARQL-Optimisers]]

# Configuring SPARQL Optimisers 

Optimisers come in two forms both of which can be configured using the Configuration API:

* [IQueryOptimiser](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_Optimisation_IQueryOptimiser.htm) are optimisers which optimise Graph Patterns in a Query
* [IAlgebraOptimiser](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_Optimisation_IAlgebraOptimiser.htm) are optimisers which optimise SPARQL Algebra

Please see [[Configuration API - HTTP Handlers|UserGuide/Configuration/HTTP Handlers]] for details on how to attach optimisers to HTTP Handlers.

# Configuring Query Optimisers 

## Basic Configuration 

Basic Configuration looks like the following:

```turtle

@prefix dnr: <http://www.dotnetrdf.org/configuration#> .

_:optimiser a dnr:QueryOptimiser ;
  dnr:type "VDS.RDF.Query.Optimisation.DefaultOptimiser" .
```

The above configures the default optimiser which is used when no other optimiser is configured.

## No Reorder Optimiser 

The [NoReorderOptimiser](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_Optimisation_NoReorderOptimiser.htm) is an optimiser that only places `FILTER` clauses but does not otherwise reorder Graph Patterns. It can be configured like so:

```turtle

@prefix dnr: <http://www.dotnetrdf.org/configuration#> .

_:optimiser a dnr:QueryOptimiser ;
  dnr:type "VDS.RDF.Query.Optimisation.NoReorderOptimiser" .
```

## Weighted Optimiser 

The [WeightedOptimiser](https://dotnetrdf.github.io/api/html/T_VDS_RDF_Query_Optimisation_WeightedOptimiser.htm) reorders Graph Patterns based on weighting calculated from statistics about data. These statistics must be invented/computed and then linked as a Graph to the optimiser like so:

```turtle

@prefix dnr: <http://www.dotnetrdf.org/configuration#> .

_:optimiser a dnr:QueryOptimiser ;
  dnr:type "VDS.RDF.Query.Optimisation.WeightedOptimiser" ;
  dnr:usingGraph _:stats .

_:stats a dnr:Graph ;
  dnr:type "VDS.RDF.Graph" ;
  dnr:fromFile "stats.ttl" .
```

# Configuring Algebra Optimisers 

You can configure any `IAlgebraOptimiser` that has a public unparameterized constructor like so:

```turtle

@prefix dnr: <http://www.dotnetrdf.org/configuration#> .

_:optimiser a dnr:AlgebraOptimiser ;
  dnr:type "VDS.RDF.Query.Optimisation.AskBgpOptimiser" .
```

You should only need to configure Algebra optimisers if using a custom optimiser. All the built-in optimisers are automatically registered and used where relevant.