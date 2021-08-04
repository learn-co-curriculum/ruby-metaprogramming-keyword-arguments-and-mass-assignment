# Keyword Arguments and Mass Assignment

## Objectives

1. Define keyword arguments and learn why they are useful.
2. Define mass assignment and learn why it is useful.
3. Practice using keyword arguments and mass assignment.

## Introduction

At this point, we're very familiar with the fact that methods can be defined to
take in arguments. We also know that methods can be defined to take in
_multiple_ arguments. Let's take a look at an example:

```ruby
def print_name_and_greeting(greeting, name)
  puts "#{greeting}, #{name}"
end

print_name_and_greeting("'sup", "Hillary Clinton")
# 'sup, Hillary Clinton
# => nil
```

As it currently stands, whoever uses our method needs to remember exactly what
order to pass in the arguments. They need to know that the first argument is
greeting and the second argument is a name. What happens if they forget? What
happens if another developer who is working on our code base _doesn't_ see the
file where the method is defined, but only sees the method being invoked? Maybe
it's not clear to them which argument is which––after all, when you invoke
methods, the arguments aren't labeled or anything. Let's take a look at what
type of disaster would befall us:

```ruby
print_name_and_greeting("Kanye", "hello")
# Kanye, hello
# => nil
```

That would be a weird way to greet Kanye. Let's take a look at another example,
this time using arguments of two different data types:

```ruby
def happy_birthday(name, current_age)
  puts "Happy Birthday, #{name}"
  current_age += 1
  puts "You are now #{current_age} years old"
end

# happy_birthday("Beyonce", 31)
# Happy Birthday, Beyonce
# You are now 32 years old
# => nil
```

But what happens if we accidentally pass the arguments in in the wrong order?

```ruby
happy_birthday(31, "Beyonce")
# Happy Birthday, 31
# TypeError: no implicit conversion of Fixnum into String
```

Oh no! We broke our program! Clearly, we have a need to regulate the passing in
of multiple arguments. It would be especially helpful if we could _name_ the
arguments that we pass in, _when we invoke the method_. Guess what? We can!
(Okay, you probably saw that one coming.)

## Keyword Arguments

Keyword arguments are a special way of passing arguments into a method. They
behave like hashes, pairing a key that functions as the argument name, with its
value. Let's walk through it together and refactor our `happy_birthday` method:

```ruby
def happy_birthday(name: "Beyonce", current_age: 31)
  puts "Happy Birthday, #{name}"
  current_age += 1
  puts "You are now #{current_age} years old"
end
```

Here, we've defined our method to take in keyword arguments. Our keyword
arguments consist of two key/value pairs, `:name` and `:current_age`. We've
given our keyword arguments default values of `"Beyonce"` and `31`, but we
didn't have to:

```ruby
def happy_birthday(name:, current_age:)
  puts "Happy Birthday, #{name}"
  current_age += 1
  puts "You are now #{current_age} years old"
end
```

Notice that we can reference `name` and `current_age` inside our method body, as
if they were barewords, _even though they are the keys in our argument hash_.
That's how keyword arguments work, they allow you to _name_ the arguments that
you pass in as keys in a hash. Then, the method body can use the values of those
keys, referenced by their name. Let's call our method:

```ruby
happy_birthday(current_age: 31, name: "Carmelo Anthony")
# Happy Birthday, Carmelo Anthony
# You are now 32 years old
```

Notice that even though we changed the order of our key/value pairs, our method
didn't break! Not only is this method more robust (i.e. more resistant to
breakage) than the previous one, it is also more explicit. Anyone looking at its
invocation can tell exactly what kind of data you are passing in.

### Mass Assignment

Another benefit of using keyword arguments is the ability to "mass assign"
attributes to an object. Let's revisit our `Person` class from an earlier
lesson. We'd like to initialize individual people with a name and an age:

```ruby
class Person
  attr_accessor :name, :age

  def initialize(name, age)
    @name = name
    @age = age
  end
end
```

As it stands, our initialize method is vulnerable to the same issues we
discussed above. Let's refactor it to take in a person's attributes as keyword
arguments:

```ruby
class Person
  attr_accessor :name, :age

  def initialize(name:, age:)
    @name = name
    @age = age
  end
end
```

Now, we have the added benefit of being able to use something called **mass
assignment** to instantiate new people objects. If a method is defined to accept
keyword arguments, we can create the hash that the method is expecting to accept
as an argument, set that hash equal to a variable, and simply pass that variable
in to the method as an argument. Let's take a look:

```ruby
person_attributes = {name: "Sophie", age: 26}
sophie = Person.new(person_attributes)
# => #<Person:0x007f9bd5814ae8 @name="Sophie", @age=26>
```

This might not seem particularly useful now, but when we start building web
applications, we'll understand more about how necessary this trick really is.
For now, just take our word for it.
