# CQL Query Execution

Following steps are involved in processing a query.

1. Parsing and Rewriting the Query
2. Generating the logical query plan
3. Generating the physical query plan
4. Query execution

Query optimizations can happen in both steps 2 and 3. Logical query plan will be expressed in *Logical Algebra* while physical query plan will be in *Physical Algebra*. Main purposes of this document are to define a **'Logical Algebra (LA)'** and a **'Physical Algebra (PA)'** for distributed CQL and define CQL to LA and LA to PA translation as much as possible including defaults and query equivalences.

# Logical Algebra for CQL

