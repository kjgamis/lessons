### What are Objects and Classes?
An object-oriented program involves classes and objects. A class is the blueprint from which individual objects are created. In object-oriented terms, we say that your bicycle is an instance of the class of objects known as bicycles.

Take the example of any vehicle. It comprises wheels, horsepower, and gas tank capacity. These characteristics form the data members of the class Vehicle. You can differentiate one vehicle from the other with the help of these characteristics.

A vehicle can also have certain functions, such as halting, driving, and speeding. Even these functions form the data members of the class Vehicle. You can, therefore, define a class as a combination of characteristics and functions.

### Defining Classes
To implement object-oriented programming by using Ruby, you need to first learn how to create objects and classes in Ruby.

A class in Ruby always starts with the keyword class followed by the name of the class. The name should always be in initial capitals. The class Person can be displayed as:

```
class Person
end
```

You terminate a class by using the keyword end. All the data members in the class are between the class definition and the end keyword.

### Creating Objects (Instances) of a Class

```
jim = Person.new
shelly = Person.new
```

### Instance Methods
Instance methods only work on an instance and thus you have to create a new instance to use them (Person.new). Again, there are more ways to define instance methods than this, especially if you look into meta programming. The below is an example of an instance method.

```
class Person
  def hello
    puts "Hello Ruby!"
  end
end
```

### Local Variables
Local variables are the variables that are defined in a method. Local variables are not available outside the method. Local variables begin with a lowercase letter or _.

### Instance Variables
Instance variables are available across methods within a class for any particular object. That means that the values of instance variables are distinct from object to object. Instance variables are preceded by the at sign (@) followed by the variable name.

### Readers and Writers
Let's say you have a class of Person.

```
class Person
end

person = Person.new
person.name # => no method error
```

Obviously we never defined a method called name. Let's do that.

```
class Person
  def name
    @name # simply returning an instance variable @name
  end
end

person = Person.new
person.name # => nil
person.name = "Dennis" # => no method error
```

Aha, we can read the name, but that doesn't mean we can assign the name. Those are two different methods. The method that simply returns the name is called the reader (or the getter). The method that allows us to assign the name is called the writer (or the setter). We didn't create the writer yet so let's do that.

```
class Person
  def name
    @name
  end

  def name=(str)
    @name = str
  end
end

person = Person.new
person.name = 'Dennis'
person.name # => "Dennis"
```

The equal sign after the writer method name is special Ruby syntax that indicates that the value can be assigned using the equal sign, rather than the typical syntax of calling a method by passing the argument in between round brackets.

Awesome! Now we can read and write the instance variable @name using our reader and writer methods.

Shorthand: 
```
attr_reader :name
attr_writer :name

attr_accessor :name
```
### Multi-class Programs

- ```require```
Each class should be defined in a separate file. In order to use class One inside class Two, the file containing class Two must load (or require) the file containing class One. Otherwise Ruby will have no idea what you're referring to when you try to make use of class One

- ```require_relative```
Classes work together to make the entire program functional. We let classes use other classes by importing a file. The way this is done is using a method called ```require_relative```, and basically what it does is get that file and stick it right in that line where the ```require_relative``` method was called.

### Class Variables

A class variable is a variable that's declared at the class level and shared across all objects of the same type


### Class methods
So far, all of the methods we've created were **instance methods**. Instance methods can only be used by a specific instance of an object, not the class itself (hence the name!). Put another way, and instance method is an action that an instance knows how to perform.

Class methods are performed by the class itself and are usually reserved for actions that operate on the whole set of objects of that type. Instantiating a new contact and storing it in the list of all contacts affects the collection of all contacts so it's a good use case for a class method. It wouldn't make senese to give that responsibility to any single contact object. You can call a class method on the name of the class itself, eg. ```Contact.super_fun_class_method```.

You define a class method by prefixing the name of the method with ```self```.

What purpose do class methods serve and why would we want to use them? This is a great example of where object oriented design comes into play. Instance methods should involve logic that only make sense being applied to an instance of something, such as updating the email of a specific person. A class method involves logic that should be applied on the whole scale of the model, such as cleaning up the email addresses of all the contacts. So the first example would be ```some_contact.update_email(email)``` and the second ```Contact.clean_email_addresses```.

Go ahead and implement the ```create ```method as follows:

```
# remember, we preface the method name with 'self.' if it is a class method
def self.create(first_name, last_name, email, note)
  new_contact = Contact.new(first_name, last_name, email, note)
  @@contacts << new_contact
  return new_contact
end
```

Our initialize method should be responsible for setting the first name, last name, email, and note that get passed in from the ```create``` method. Additionally, it should set the id of the contact and increment the class ```@@id``` variable so that the next contact will get a different id.

```
def initialize(first_name, last_name, email, note)
  .
  .
  .
  @id = @@id
  @@id += 1 # this way the next contact will get a different id
end
```

### Other Methods
The implementation of the following methods are left up to you:

- ```self.all```
- ```self.find```
- ```self.find_by```
- ```self.delete_all```
- ```full_name```
- ```update```
- ```delete```
