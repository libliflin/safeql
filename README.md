safeql
======

SQL: The safe parts.

Philosophy
----------

1. Built in, referenceable authentication. 
2. Built in, referenceable authorization.
3. Built in, referenceable schema versioning.
4. Built in, referenceable data versioning.

Opinions
--------

1. Tables must have *one* and only one id.
2. Id's *must* be an id type (guid, int, idc, just needs to be an id type)
3. selects cannot use comma in from.
4. all joins must be left. 
5. all inner joins must come before outer joins.
6. all cross table comparisons *must* be fk/pk based.
7. No Unions.
8. Result sets N-Dimentional by default.
9. All joins are *sets* only. (no more of this almost set theory but realy just nested for loops crap)
10. Group/where can only reference inner joins.


Example
-------

For example (syntax i just made up off the top of my head). given schema given by:

    CD:
      released Date

    Artist:
      stage_name utf8
      appears_in CD

    Producer:
      stage_name utf8
      contributed_to CD

The query:

    select 
      CD.released:
        Artist.stage_name
        Producer.stage_name
    from 
      CD
    outer join 
      Artist.appears_in
    outer join
      Producer.contributed_to 
    group by
      CD.released

would give the java equivalent of HashMap&lt;Date, List&lt;String&gt;&gt; (if your Strings are utf8 java strings are utf16 by default.) representing the list of producers and artists that contributed to or appeared in a CD released on that date. 
