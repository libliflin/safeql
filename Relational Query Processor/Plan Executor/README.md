Plan Executor
=============

"This is typically a directed dataflow graph that connects operators that encapsulate base-table access and various query execution algorithms."

Operators are iterators. Iterators couple data flow with control flow.

    interface Iterator {
    	Open(sarg SARG)
    	Input(Iterator[])
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
9. FullTableScan

SARG = Search Argument.

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


Where is the data?
==================

While you can imagine each iterator would need at least pointers to the actual data, where they are stored is either in the buffer pool BP or in memory M.
1. *BP-tuples*: Naturally, references to the buffer pool must be kept track of so that pages do not get pushed out of buffer pools prematurely. 
2. *M-tuples*: always copy the data out of buffer pools, so that M-tuples are the only in flight data. However, unecessary memory copies are bad for performance.

The solution of course is to have tuple descriptors that copy bytes to memory when you are pinning a buffer full of memory for too little usage.

    add tuple implmentation to cost analysis!

For DML statements (INSERT, DELETE, UPDATE) the authors suggest to materialize (M-tuple) all row ids, and then run dml statements rowid at a time.

For this project though, I would like to simply add multi-version support from the storage engine. Buffer pool writes logged transactionally, so that 
    get(x) = 4
    write(x=5)
    get(x) still 4; the write won't commit until the end.



Getting a Tuple descriptor -- Access Iterators
==============================================

* Heap
* B+ tree index
* hash index
* R tree
* Generalize Search Tree
* RD tree
* Multi-Dimensional Clustered Index
* bitmap index

32, 39. 40, 66, 65

Physical or logical address of row
==================================
1. Physical implies fast lookup, with penalty of forwarding pointer and or index rebuild if moved. Also Storage of B+ trees becomes unreasonably slow.
2. Logical implies slow lookup.

Again, the answer is to have both.

Cost analysis is key here. if it is going to be cheaper in the long run to keep the row where it is and add the other part of the row somewhere else, *then do that!*.

Some tables should be optimized for fast single insertion, deletion and update. (B+ trees) but some should be optimized for retrieval(Bitmaps[65]).

Batch Insertion can and should be optimized for (bulk insert methods).

SNAPSHOT ISOLATION can give *moment in time* views of data.

Materialized views create their own tables and must be
1. selected
2. refreshed
3. available for ad-hoc.


section 4.6.4 for another day.