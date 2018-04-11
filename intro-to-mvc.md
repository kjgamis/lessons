## framework
- A set of code libraries that help you achieve a certain purpose
- In Rails, the purpose is building large web apps

## MVC
- "Model View Controller"
- A common way of **architecting** an app
- The **architechture** is how its code is structured:
  - What do I name my files and folders
  - Which files should I put my files in
  - What should I name the classes in my files
  - Etc
- Learning Rails will help you learn any other MVC framework

- In MVC, we separate our code into 3 main categories: (separation of concerns)
  1. Model
    - All business logic code is in the Model
  2. View
    - All UI code is in the View. No business logic code is in the View
  3. Controller
    - The controller figures out which Model code and View code is relevant to each request
    - asks the model to execute the relevant business logic,
    - then asks the View to render the relevant template using the data from the model


## Breadth-first Learning
- There are layers upon layers of code behind even the smallest of things in Rails
- If you take a detour to try to understand how a particular method works you might fall behind


## Making an app
```
$ rails server
```
Routes in our app can be found in ```config/routes.rb```

```
$ rails generate controller -controller name-
```
New file is called ```app/controllers/-controller name-_controller.rb``` which should contain the following code:

```
class -Controller name-Controller < ApplicationController

end
```

Define new method in controller class, called index:

```
class -Controller name-Controller < ApplicationController
  def index
  end
end
```

This method will open a *view* with the name ```index.html.erb``` inside ```app/views/pictures```.

```
get 'pictures' => 'pictures#index'
```

This line of code says "match any HTTP ```GET``` Request for the URL ```/pictures``` to the ```index``` method in the ````PicturesController````".

Or more granularly: ```get```: this specifies the type of HTTP request

```'pictures'```: this specifies the URL - i.e. localhost:3000/pictures

```=>```: this is just the syntax for defining routes in Rails

```'pictures```: this is the name of the controller - i.e. the class is called ```PicturesController```

```#```: this is just more of the syntax for defining routes in Routes

```index```: this is the name of the method in the controller that gets executed when someone makes a request to this route
