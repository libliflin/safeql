Plan Executor
=============

"This is typically a directed dataflow graph that connects operators that encapsulate base-table access and various query execution algorithms."

Operators are iterators. Iterators couple data flow with control flow.

    interface Iterator {
    	Input(Iterator[])
    	Init()
    	Next() Tuple
    	Close()
    }


Types of iterators
1. Filescan
2. Indexscan
3. Sort
4. NestedLoopJoin
5. MergeJoin
6. HashJoin
7. DuplicateElimination
8. GroupedAggregation


Query Execution Survey: 
G. Graefe, “Query evaluation techniques for large databases,” Computing Surveys, vol. 25, pp. 73–170, 1993.

PostgreSQL has sophisticated Iterators. See it's source code.


I think there should be at least two executor inputs.

1. Opcodes. 
2. Relational Algebra Iterator interface based.

I think there should be at least 3 executor implementations

1. Iterator (interface above) factory
1. goroutine and chan based.
2. goroutine and shared memory based.

Section 4.4.2 and onward for another day.