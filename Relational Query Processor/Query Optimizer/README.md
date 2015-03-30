Query Optimizer
===============

Bahoovian cajoleing Optimizers are complicated.

Goal: transform our parsed/rewritten query to a query plan.

Query Plan: a bunch of Operators. See Plan Executor.

Basic foundations: Selinger et al. on the System R optimizer.

4.3 page 182 [79]

P. G. Selinger, M. Astrahan, D. Chamberlin, R. Lorie, and T. Price, “Access
path selection in a relational database management system,” in Proceedings of
ACM-SIGMOD International Conference on Management of Data, pp. 22–34,
Boston, June 1979.

Extensions:

1. Plan space. don't just focus on "left-deep" query plans.
2. Selectivity estimation is too naïve. 
--1. You must look at distributions of values, histograms, and other summary statistics. You short circuit this cost with sampling. (TABLE STATS) 
--2. Check for dependencies between columns. (Use TPC-DS instead of the TPC-D and TPC-H for benchmarks for real data.)
--3. Errors here ruin optimization attempts later.
3. Search Algorithms. Implement all of them.
--1. Dynamic programming
--2. "top-down" search schem based on Cascades.
--3. Heuristics: exploit indexes and foreign keys.
4. Parallelization! I can't find the article now, but recently there were reports on finding the dependency chains (some fancy word for this...) and automatically parallelizing the query.
--1. Optimizer should be consious of parallel operators.
--2. Optimizer should also be able to do two steps: find best serial plan, find how to best optimize that serial plan.
--3. Thrashing, and network costs need to be effectively estimated.
5. Auto-Tuning Feedback loop. All estimations and planning need to be consistently checked and explored.
--1. What if there was an index?
--2. Is the current model of the hardware costs accurate?
