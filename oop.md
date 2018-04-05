### What are Objects and Classes?
An object-oriented program involves classes and objects. A class is the blueprint from which individual objects are created. In object-oriented terms, we say that your bicycle is an instance of the class of objects known as bicycles.

Take the example of any vehicle. It comprises wheels, horsepower, and gas tank capacity. These characteristics form the data members of the class Vehicle. You can differentiate one vehicle from the other with the help of these characteristics.

A vehicle can also have certain functions, such as halting, driving, and speeding. Even these functions form the data members of the class Vehicle. You can, therefore, define a class as a combination of characteristics and functions.

## Defining Classes
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

## Instance Methods
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

