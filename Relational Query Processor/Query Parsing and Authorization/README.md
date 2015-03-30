Query Parsing and Authorization
===============================

1. Fully qualify all table names: Alias/what have you to server.database.schema.table (four part name)
1. Ask the Catalog Manager if those table names exist.
1. Ask the Catalog Manager if the column names are correct.
1. Ask the Catalog Manager if the column types are correct.
1. Check if the user has permissions to actions requested on columns found.
1. Send to Query Rewrite.