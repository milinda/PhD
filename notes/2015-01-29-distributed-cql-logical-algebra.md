# CQL Query Execution

Following steps are involved in processing a query.

1. Parsing and Rewriting the Query
2. Generating the logical query plan
3. Generating the physical query plan
4. Query execution

Query optimizations can happen in both steps 2 and 3. Logical query plan will be expressed in *Logical Algebra* while physical query plan will be in *Physical Algebra*. Main purposes of this document are to define a **'Logical Algebra (LA)'** and a **'Physical Algebra (PA)'** for distributed CQL and define CQL to LA and LA to PA translation as much as possible. In addition to that this document describes defaults and query equivalences as needed by the LA and PA definitions.

# Logical Algebra for CQL

## Extended Relation Algebra Operators <sup>[1](https://courses.cs.washington.edu/courses/cse544/15wi/lectures/lecture6-execution.pdf)</sup>

* Union (∪), Intersection (∩), Difference (-)
* Selection (σ)
* Projection (π)
* Join (⨝)
* Duplicate Elimination (δ)
* Grouping and Aggegation (γ)
* Sorting (τ)
* Rename (ρ)


# References
[1] https://courses.cs.washington.edu/courses/cse544/15wi/lectures/lecture6-execution.pdf
