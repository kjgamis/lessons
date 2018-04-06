## 1 to Many

**Relationships:**

Each article is written by one author

One author writes many different articles

Therefore there is a 1-to-many relationship between them.


### Designing Database Tables
Each class should have a table and each attribute of the class should correspond to a column in that table. Each table should also have a primary key (id) column, and potentially some foreign key columns to facilitate the relationships that exist between your tables.

### Placing foreign keys
For every 1-to-many relationship in your app you must place a foreign key on the "many side". For example:

Each author is associated with multiple articles. Each article is associated with one author. The "many side" is article, and the ```articles``` table must have an ```author_id``` foreign key column.

This allows each article to keep track of who its author is.


## Many to many

### Placing join tables
A join table is a table with two foreign key columns. It "joins" together the tables/models that those two foreign keys refer to.

A join table can represent a class itself (as with the ```bookmarks``` join table), in which case it will have columns other than foreign keys. Alternatively, a join table can consist of only foreign keys therefore correspond to no class outside of the database (as was the case for the join table between ```users``` and ```articles``` in the first solution).

