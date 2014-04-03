safeql
======

SQL: The safe parts. Some guy favorited my tweet, so now I'm expanding. 

<blockquote class="twitter-tweet" lang="en"><p>Code Idea: SafeQL: a (not-strict) subset of SQL that has strict requirements for normalized ids, joins, selects; safe dynamic where clauses.</p>&mdash; William Laffin (@redwepainted) <a href="https://twitter.com/redwepainted/statuses/451167319784701952">April 2, 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

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

would give the java equivalent of HashMap&lt;Date, List&lt;String&gt;&gt; (if your Strings are utf8 that is, java strings are utf16 by default.) representing the list of producers and artists that contributed to or appeared in a CD released on that date. 

TODO &amp; THOUGHTS
-------------------

Wow, reading that above there are so many questions I have now, like, what happens if there are two different CDs in the join chain? can you not join a table to itself? (recursive, manually or otherwise)? Maybe throw a compile time error saying "ambiguous join chain, specify alias"

Not sure I like that resultsets are maps by default, but it makes sense sort of from a REST point of view right? Each select query gets it's own GET resource URL? something like db.push([above query]) => add a url to the "UDDI" of sorts. 

How do i feel about nested queries? I use them all the time at work, but is there a better way?

still need to nail down that pesky safe dynamic where clause. 

Maybe switch from interpretted to compiled for security? so that you have to submit your query to the db (and have rights to do so) and then you can only reference said statement?

The above example auto threads with the group by. Is that a non-ambiguous operation? Intuitively it seems like there is a static way to check if it is ambiguous or not, if not completely non-ambiguous. 