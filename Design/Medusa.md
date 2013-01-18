# Medusa Query Engine

Medusa is the code name for a proposed streaming query engine for dotNetRDF.  

### Motivation

Our current Leviathan engine is what is typically characterised as a block based engine, this means that it works by evaluating one chunk of the query in full before moving onto the next.  While this has some advantages it also some key disadvantages, namely:

- Very memory inefficient for queries with large intermediate results
- Can't return any results until all results are generated

While the current version of Leviathan does have some limited support for lazy evaluation of certain queries this is by no means fantastic and is limited to a very small subset of queries.

A streaming engine by constrast is designed to evaluate the query as lazily as possible, in the .Net context this means it will be implemented almost entirely using LINQ.  The advantages of a streaming engine are that it avoids the stated disadvantages of a block based engine.  A streaming engine has much lower memory overhead because it only needs enough memory to calculate the next answer, this also means that it can start returning results as soon as it finds it's first results.  Bear in mind that it does still have some disadvantages:

- May end up doing more work than a block based engine for some kinds of query
- May be harder to parallelize evaluation for some kinds of query

Note that since we will use LINQ to build the Medusa engine using PLINQ to parallelize things is an obvious option and one Leviathan already uses internally to great effect.  However with a streaming engine we will have to be careful of when to parallelize because some things will not be amenable to parallelization e.g. ORDER BY.

## Design

The Medusa query engine will be implemented as an implementation of the `ISparqlQueryAlgebraProcessor` interface, eventually like Leviathan it will also implement `ISparqlQueryProcessor` but for the initial POC we confine ourselves to algebra processing.

The proposed type signature is as follows:

    public class MedusaQueryProcessor
      : ISparqlQueryAlgebraProcessor<IEnumerable<ISet>, ISet>

This states that Medusa produces enumerables of `ISet` as its results and takes an `ISet` as its context information.  The `ISet` passed as the context serves to constrain the return of evaluating different algebra operators.  Let us consider `Join` as an example, since joins are expensive we want to minimize the effort expended here.  We can do this if we feed the results of evaluating the LHS into the RHS so that for each result on the LHS we only consider results from the RHS that are likely to be compatible.  This may be easier to understand given sample code:

    IEnumerable<ISet> lhs = this.ProcessAlgebra(join.Lhs, context);
    
    if (join.Lhs.Variables.IsDisjoint(join.Rhs.Variables))
    {
        IEnumerable<ISet> rhs = this.ProcessAlgebra(join.Rhs, context);

        //Do a product
        return (from x in lhs
                from y in rhs
                select x.Join(y));
     }
     else
     {
        //Do a join
        ISparqlAlgebra rhsAlgebra = join.Rhs;
        List<String> vars = join.Lhs.Variables.Where(v => join.Rhs.Variables.Contains(v)).ToList();
        return (from x in lhs
                from y in this.ProcessAlgebra(rhsAlgebra, x)
                where x.IsCompatibleWith(y, vars)
                select x.Join(y));
     }

## Implementation Barriers

There are some barriers to implementing the proposal namely concerned with the fact that 