Query Rewrite
=============

1. Expand all Views. (this is recursive)
2. Expand all queries over partitions.
3. Evaluate all constant arithmatic.
4. Test boolean expressions for basic satisfiability a < b && b < a ==> false.
5. Semantic Optimization: Remove unaccessed, but joined, tables
6. Semantic Optimization: Test basic expressions against constraints. (where x = 4 on a <3 constraint'd column)
7. Subquery flattening: To prepare for optimizer, flatten to one select from where as much as possible.
8. Send to Query Optimizer.