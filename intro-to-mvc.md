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

- In MVC, we separate our code into 3 main categories:
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
