Process Manager
===============

1. This will be goroutine based. 
2. chans will move requests though in a linear fashion. 
3. The go runtime will manage what runs where and when. (GOPROCS etc.)
4. fan out, machine learning style profiling, and units of work will be estimated everywhere.
5. Build tools will manage debug logging (remove from source based on flags).
6. Each transaction will have a dedicated goroutine for logging. Each transaction goroutine will have another separate goroutine looping through the log and persisting to disk.
7. Shared nothing and shared everything options will be facilitated by structs with locks; as well as optional goroutines that manage them.
--1. Shared everything: use locks on storage and cache page memory.
--2. Shared disk: use chans for cache page but locks for storage.
--3. Shared nothing: use chans for both.

