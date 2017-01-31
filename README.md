# The Tell programming language
**Document version:** 2017-01-18:1

**Language API version:** 0.1.0

**Project status:** Drafting of language spec in progress

## Motivation

### Why create Tell?

1. [For the fun of it](https://www.youtube.com/watch?v=DC-bjR6WeaM)

1. Because I've built a list over the past couple of years of

    * language features that I like, 
    
    * language features I dislike, and 
    
    * library features that I wish would be available at a language level. 
    
  It would be interesting to see if some of these ideas can be put together in a language.
  
1. I want to see if I can instill some of the best software engineering practices I have learned and developed in my career thus far into a language that encourages high quality, clean code.

### What does the name mean?

The name Tell primarily comes from the object oriented design principle of "Tell, don't ask". The language is designed so that, insofar as objects are concerned, you must do exactly that.

As a secondary inspiration, the Scala based actor library Akka uses the method 'tell', or its alias '!' to send data from one actor to another. Since all classes in Tell are actors, even if the syntax is different, 'telling' is a big part of using the language.

## Philosophy

Tell is a rather opinionated language. Even though the motivations for the language are more feature driven than philosophical, there are still some core principles and purposes that the language must follow in order to be pleasant to work with and further to stay focused on and true to the ideas behind the original feature set as the language evolves over time.

### Be nice

This principle is a page taken straight out of Ruby and its creator Matz's book. Tell should be a language that enables you to solve problems easily and enjoyably. In fact, joy of use is more important than ease, although the two are probably related. There's plenty of room for features that makes the programming experience fun and productive. There's no room for features that cause agony and frustration. The aim is to err on the side of happiness.

### Application Programming

The focus of Tell is programming applications. Features that are more systems oriented such as control over memory, control over concurrency and low level datatypes are excluded. 

It is my belief that you cannot make a language that offers both good high level and good low level programming models, because the programmer will always be facing the temptation to use the wrong abstractions in the wrong places, which leads to all kinds of code deficiencies. In order to prevent this you would need a whole network of complicated rules which would lead to an unpleasant and unintuitive programming experience. So, in order to be nice, you might say, we try to do one thing and do it well.

Another consequence of an opinionated focus on application programming is that Tell does not provide any metaprogramming or introspection syntax. The general idea is that if you can't express your automation by means of the abstractions you have built, then you need to build better abstractions. There are no secret pathways to get you your values or references. This is a significant difference to Ruby from which Tell draws a lot of inspiration. It is my belief this leads to a cleaner and more predictable language.

### Easy to integrate

Because Tell is so opinionated in its feature set there are many problems that are better solved in other languages. Therefore it is important that Tell can play well with others. This can be achieved through

* Good interfacing with native code, e.g. FFI or similar.

* Good I/O abstractions in the core library

* Batteries included approach in core library: Offer built-in HTTP client and server, JSON-parser, etc. 

### Throw away bad features

Tell aims to be a slim and opinionated language much like many recent languages such as for instance Go. To keep the language slim and clean, many 'bad' features found in similar languages were discarded because they matched one or more of the following characterisations:

#### Features you don't need

Some features are redundant because there are better means to express the same intent. Some do not have any real use case in a high level programming model.

#### Features that let you write smelly code

When writing code in Tell, generally speaking, the path of least resistance should lead the programmer to write clean code. Certain features tend to lead to code that is difficult to test or maintain, and we want to avoid such features if we can do fine without them.

#### Features that belong in systems languages

As described above, Tell is a high level applications language, and the inclusion of low level or systems oriented features would water down the intentionality and focus of the rest of the language. 

## Features

abc

### Duck typing

abc

### Values

abc

### Structs

abc

### Closures

### Classes

Classes, like in OOP terminology, are definitions of 'boxes' that hold data and methods that operate on that data. Objects are specific instances of classes with some given state.

#### Classes are actors

abc

#### Methods

Classes have methods. Calling methods on objects are the equivalent of invoking actor actions. They can modify instance state or trigger methods on other objects. Methods can be defined to take zero, one, two or three parameters. Parameters can be values, structs or closures. Other objects cannot be passed as parameters.

Methods can have either public or private visibility. Public methods are defined using the keyword `pm`, whereas private methods are defined with the keyword `m`.

The reason why other objects cannot be passed as parameters (or as fields in a struct parameter) is that we do not want to break encapsulation. No actions in another object should be able to cause any side effects on this object's state without going through an explicit method call, and no other object should need to have any knowledge about this object's internal state. We do, however need a way to provide callbacks to methods on this object as it is calling methods on other objects, e.g. a request-response kind of pattern like when you send a message back to the `sender` actor in Akka. 

For this purpose we have closures, which are valid parameters, that can close over the current object and be sent as a parameter in another object. A closure has access to invoking both public and private methods on the object in which it was created. All change of an object's state must happen through method calls, so we cannot change an object's instance variables directly from a closure. Instead we can provide private methods that can be used by the closure during a callback. Let's take an example to clarify this.

```
class Car

  pm init()
    @engine = Engine.new()
    @started = false
  end
  
  pm start()
    # We pass a closure to @engine's start() method.
    # The closure has access to the car instance's private methods
    @engine.start { |success| mark_started() if success }   
  end
  
  m mark_started()
    @started = true
  end
  
end
```

An objects gains ownership over a struct when it is passed as a parameter to a method on that object. Until the object passes along the struct to another object's method, that object has exlusive access (read and write) to the struct. Therefore it is ok to mutate struct parameters.

Unlike other languages, because classes are actors, method invocations on an instance of a class are atomic, i.e. only one method can be executed at a time on an instance.

Tell only has instance methods, it does not have class or static methods.

#### Only instance state

abc

### No modules

abc

### No inheritance

abc

### Monkey patches

abc

## Syntax

abc

### Mostly ruby-like

abc

### Namespaces

abc

### Match instead of switch

abc

## Roadmap

abc
