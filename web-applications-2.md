### ```server.rb```

- add require 'sinatra'
- add routes
  - such as routes to the homepage, other links such as contact
  - include ```redirect to(/----)```
- create a subdirectory called "views" which has our ```erb``` files
  - files names are consistent
- inside routes, link the erb files to the server in each route using the convention: ```erb :--```

## Getting Started
1. Create a new directory called ```public``` at the root of your project.
2. Create a file called ```styles.css``` in the ```public``` directory.
3. Create a file called ```layout.erb``` in the ```views``` directory.


### ```erb``` file
- when linking to another page, add attribute ```href = "/about"```


### ```layout.erb```
- saved in ```views``` subdirectory
- has the template for webpage
- in modified areas, write ```<%= yield %>``` (can only be used once)
  - allows us to dynamically insert other views inside this file. The yield statement should be wherever you want the page-specific content to appear.
- for titles
  - use instance variable to routes
  - on layout, add the <%= @instance_variable %>

### CSS file
- add subdirectory called ```public```
- make file called ```styles.css```
- in template link ```href = "/style.css"```
  - sinatra knows it's on public subdirectory
- any other images should be saved on ```public``` subdirectory

### ```.gitignore```
- include ```.sqlite3``` file

## dynamic routes
Our goal is to enable users to view the details of any individual contact on its own page. To do this, we can add a **dynamic route** that contains the id of the contact that the user wants to view. No two contacts have the same id, so this is the best attribute to use to allow us to understand what contact the user wants to see.

### Defining the new route
You can define a dynamic route in Sinatra by describing the pattern of the URLs that this route should handle, which means identifying the part of the URL that will change from request to request with a colon (```:```) and a name. In this case, we'll identify that the end of these URLs will contain an id:

```
...
get '/contacts/:id' do
  # instructions for how to handle requests to this route will go here
end
...
```

This route will match ```get``` requests to any URL that matches this format, including ```localhost:4567/contacts/1```, ```localhost:4567/contacts/1234567```, ```localhost:4567/contacts/55```, etc. It will also match URLs that don't contain valid ids, such as ```localhost:4567/contacts/mittens```, so eventually we'll have to consider what to do if that's the case. For now we'll make the route work with the assumption that the id in the URL refers to a real contact's id.

### Adding a new view
Create a new view called ```show_contact.erb``` and render it as the response from the route we just made. Put a heading in the view so you can test to make sure it's rendering properly.

To test it, try going to l```ocalhost:4567/contacts/1```, or a URL with any other id in it. Try changing the id.

### Showing the right data
Our strategy for displaying the right contact data will be to retrieve the contact from the database, store it in an instance variable, and use that variable in the view.

The id in the URL tells us which contact we need to retrieve from the database. But how can we access the id part of the URL from our Sinatra code? When handling requests to dynamic routes, Sinatra stores the dynamic part of the URL (in this case, the id) in the ```params``` hash under a key that matches the name we gave to that part of the URL. Meaning that in our route block we can access the specific id for that request by writing ```params[:id]```

You may recall from past assignments that our contact class's ```find_by``` method allows us to retrieve contacts from the database. If we pass in a hash where the key is the name of a database column (such as "id") and the value is the actual value to search for in the database (the actual id in this case), then ```find_by``` will return the contact that matches that id.

Putting all these ideas together, we can finish writing our route code:

```
...
get '/contacts/:id' do
  # params[:id] contains the id from the URL
  @contact = Contact.find_by({id: params[:id].to_i})
  erb :show_contact
end
...
```

Test your links our and commit once they're working.


### What about invalid ids?
What happens currently if you try to visit a contact page that doesn't exist, such as localhost:4567/mittens? You should be getting a similar error to this:

```
NoMethodError at /contacts/mittens
undefined method `first_name' for nil:NilClass
```

This is because when we can't find the contact matching the id in the request (there is no contact with ```mittens``` as its id), which means ```@contact``` is nil, and our view can't handle ```nil``` values.

We should do what a typical web application does in these circumstances and return a 404 Not Found response status code if we can't find what the user is asking for:

```
...
get '/contacts/:id' do
  @contact = Contact.find_by(id: params[:id].to_i)
  if @contact
    erb :show_contact
  else
    raise Sinatra::NotFound
  end
end
...
```

After adding that code, if you try an id that you know doesn't exist in your database you should get a 404 page back saying "Sinatra doesn't know this ditty...".

**note:** run your code often
