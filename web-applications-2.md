### ```server.rb```

- add require 'sinatra'
- add routes
  - such as routes to the homepage, other links such as contact
  - include ```redirect to(/----)```
- create a subdirectory called "views" which has our ```erb``` files
  - files names are consistent
- inside routes, link the erb files to the server in each route using the convention: ```erb :--```


### ```erb``` file
- when linking to another page, add attribute ```href = "/about"```

### ```layout.erb```
- saved in views subdirectory
- has the template for webpage
- in modified areas, write ```<%= yield %>``` (can only be used once)
- for titles
  - use instance variable to routes
  - on layout, add the <%= @instance_variable %>

### CSS file
- add subdirectory called ```public```
- make file called ```style.css```
- in template link ```href = "/style.css"```
  - sinatra knows it's on public subdirectory
- any other images should be saved on ```public``` subdirectory

### ```.gitignore```
- include ```.sqlite3``` file

**note:** run your code often
