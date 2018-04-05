### ActiveRecord CheatSheet
The ActiveRecord docs are notoriously hard to read, so we've compiled a list of methods that you should be aware of:

### ActiveRecord Class Methods
- .all
- .count
- .find
  - accepts an id as an argument
- .find_by
  - accepts a key-value pair as an argument (attribute and value)
- .new
  - accepts a hash as an argument (attributes as keys and values as...values)
- .create
  - accepts a hash as an argument (same way new does)

### ActiveRecord Instance Methods
- .delete
- .save
- .update AKA .update_attributes
  - accepts a hash as an argument (same way new and create do)

Does this list of methods seem partially familiar? That's because we purposefully asked you to follow this pattern of methods when writing your original Contact class.

## 1.  Moving to ActiveRecord
With our database now setup, we now need to modify our Contact class and get it working with a database.

### Transforming the Contact class into an ActiveRecord Model
Next we're going to transform our Contact class into an ActiveRecord model by *inheriting* from ```ActiveRecord::Base```.

As soon we inherit from this Base class, we'll have access to all the special ActiveRecord methods that allow us to interface with the database.

Example:

```
require 'active_record'
require 'mini_record'

ActiveRecord::Base.establish_connection(adapter: 'sqlite3', database: 'crm.sqlite3')

class Contact < ActiveRecord::Base

  def full_name
    "#{ first_name } #{ last_name }"
  end

end
```

As soon as we inherit from ```ActiveRecord::Base```, ActiveRecord will also start to consider this class to represent a single database table. That means that every time we create a new Contact record, it will automatically be inserted into the ```contacts``` database table.

However, it still doesn't know which columns belong to this ```contacts table```, so we'll need to define properties for each column we want. This is where MiniRecord will help us along.

The columns we want to store are identical to the instance variables that we defined inside our ```Contact``` class, so this makes things easy.

```
class Contact < ActiveRecord::Base

  field :first_name, as: :string
  field :last_name,  as: :string
  field :email,      as: :string
  field :note,       as: :text

  def full_name
    "#{ first_name } #{ last_name }"
  end

end
```

Note that this automatically sets up reader and writer methods for each of these fields, which is why we deleted them from the class a few minutes ago.

When you define a field, you need to provide the name of the column as a symbol, and then the data type the field will store. ```:string``` should be straightforward, but ```:text``` maybe not so much. Basically you can store more characters in a ```:text``` field than a ```:string``` field, which is limited to 256 characters. (You might want to write an extensive file on each contact!)

One thing you might notice is that we do not have to define a field for :id anymore. ActiveRecord will take care of creating an ```:id``` field that automatically increments!

Once the database fields are defined, we must add the following statement ```Contact.auto_upgrade!``` after the end of the class definition. This one takes care of effecting any changes to the underlying structure of the tables or columns.

```
require 'active_record'
require 'mini_record'

ActiveRecord::Base.establish_connection(adapter: 'sqlite3', database: 'crm.sqlite3')

class Contact < ActiveRecord::Base

  field :first_name, as: :string
  field :last_name,  as: :string
  field :email,      as: :string
  field :note,       as: :text

  def full_name
    "#{ first_name } #{ last_name }"
  end

end

Contact.auto_upgrade!
```

### Ensuring the database connection is closed
By default, SQLite allows 5 concurrent connections. Unfortunately, MiniRecord will open connections, but it won't close them automatically. What this means is that every 6th time you restart your CRM, there won't be any connections left and you'll get a mysterious Timeout error.

To fix this, add the following snippet of code to the bottom of your ```crm.rb``` file:

```
at_exit do
  ActiveRecord::Base.connection.close
end
```

This will ensure that as long as your program shuts down gracefully, it'll close the connection to the database.


### Starting SQL on the command line
Whenever you'd like to practice some SQL, feel free to open up the console and work with SQLite on the command line, just like in Step 0.

```
$ sqlite3
```

You can also start sqlite on the command line with a specific database in mind, such as the one we just created. Inside your project folder you can do:

```
$ sqlite3 crm.sqlite3
```

Note that if make any changes to data, you will be making actual changes to your database.

Here are the official sql command line docs: https://www.sqlite.org/sqlite.html

### CREATE a new table
Create a new table called ```users```. It should have the columns ```id```, ```first_name```, and ```last_name```.

```
CREATE TABLE _table_ (
  id INT PRIMARY KEY AUTOINCREMENT,
  _column1_ _datatype1_,
  _column2_ _datatype2_
);
```

Fill in the blanks where there are underscores. Don't forget your semi-colons! Your query won't execute without them.

### INSERT a new record
Insert a few records inside your ```users``` table.

```
INSERT INTO _table_ (_column1_, _column2_) VALUES (_valueforcolumn1_, _valueforcolumn2_);
```

### SELECT records
To select every record and every column:

```
SELECT * FROM _table_;
```

You can also select only specific column names by passing them in instead of *

```
SELECT _column_ FROM _table_;
```

### SELECT records conditionally
You can use a **WHERE** clause to specify certain conditions in your query.

```
SELECT * FROM _table_ WHERE _column_ = _value_;
```
Depending on the "value", you might have to wrap it in quotes.

Here are some examples of where clauses that you can use:

### UPDATE a record
Updating allows you to change values of existing rows inside your table.

```
UPDATE _table_ SET _column_ = _value_ WHERE _some_condition_;
```

It's probably best to always include a condition when you're updating rows, because otherwise the update will apply to every single row.

### DELETE a record
You can delete records from a table with the following statement.

```
DELETE FROM _table_ WHERE _some_condition_;
```

**NOTE:** If you don't set a condition in the DELETE statement, you will delete everything also.

### DROP an entire table
If you want to completely remove a database table, you'll need to "drop" it.

```
DROP TABLE _table_;
```
