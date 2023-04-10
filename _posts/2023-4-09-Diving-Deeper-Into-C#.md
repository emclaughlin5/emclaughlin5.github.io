---
layout: post
title: Diving Deeper Into C#
---
## Introduction
Hello readers, this is my second blog post and in this I will be exploring Object Oriented Programming in C# in further detail.

## Object Oriented Programming in C#
The principles of object oriented programming are the same across languages, so everything we have learned in our OOP classes is transferable from Java to C#. To refresh our memories the four basic principles of OOP are:

### Encapsulation
Is the idea of giving access to data and processes only for those that need it and restricting access otherwise. Implementing this through the use of public, private and protected will protect the data in objects to ensure that they are only being changed as neccessary.

### Abstraction
Is a concept of something without the actual implementation. Easier to think of an example such as an oven, we input parameters, time and temperature and let it do the rest. The oven could be electric or gas but we do not need to know which it is to use the oven.

### Encapsulation
This is one way to express a relationship between objects, it can also allow us to reuse code. In java if we use the extends keyword to create a extend another class the subclass will inherit all the properties and methods of it's super class.

### Polymorphism
Means something can take many forms. In OOP this refers to overloading, creating two or more methods with the same name but different parameters, and overwriting ,a subclass has a method with the same signature as one in its super class but with a different implementation.

### Syntax
The syntax of OOP operations in C# is very similar to Java with some small differences. There is no need to use the "extends" or "implements" keywords in C# it can be done simply like so:

```
public class DigitalClock : Clock, ITime {
  ...
}
```
In this example DigitalClock is a subclass of Clock and implements the ITime interface. According to convention interfaces should come after super classes and interface classes should start with an "I" to make it easier to understand a classes signature. Some OOP languages such as C++ support inheriting multiple classes, neither C# nor java do. However a class may inherit mulitple interfaces.

In many cases it is better to use composition over inheritance, you still get the benefits of reusing code from other objects but it is easier to maintain and is more resilient. Can also compose a class with many different objects to have a managable version of multi-inheritance.

```
class VehicleOwner {
  private Vehicle car;
  private Person owner;
}
class Vehicle{...}
class Person{...}
```

To override a method from a super class the "override" keyword must be in the method signature:
```
public override int Calculate (int a, int b){
  ...
}
```

### Virtual
By default methods cannot be overwritten by classes that inherit it unless the "virtual" keyword is used in the method signature of the superclass.
```
public virtual double Area(){
  return x * y;
}
```
A virtual method is used to define the default behavior of a method that can be overwritten. Using virtual methods can result in expected behavior and should only be used if there is strong justification, in most cases it is encouraged to use abstract classes/methods instead.

### Abstract
Abstract classes work the exact same in C# as we have learned in Java. An abstract class cannot be instantiated and subclasses must implment all abstract methods before they can be compiled. Though abstract clases can contain properties and non-abstract methods as well.
```
abstract class Vehicle{
  private string make;
  private string model;

  public abstract void Start();
  public abstract void Drive();
}
```

### Interface
The highest level of abstraction in Object Oriented Programming, an interface contains no implmented code and merely defines the behavior that any classes that implement it must have.
```
public interface IAnimal {
  public void Eat();
  public void Sleep();
  public void DisposeWaste();
  public void Reproduce();
}
```
When creating interfaces it is best to make them small with a narrow scope, remember classes can implement many interfaces so if an interface starts to get large consider splitting it up into smaller pieces.

### Lists
Lists in C# act the same way as they do in Java, they are merely data structures consisting of objects and do not have a fixed sized like arrays. The most common types of lists used by C# developers include ArrayList, List<T>, Queue, Stack and LinkedList<T>. Notice List<T> and LinkedList<T> are using a generic to define what type of object can be stored. Declaring a list in C# and using it is much the same as it is in Java.
```
List<String> list = new List<String>();
```

### Property
One of my favorite features in C# is this feature which allows you to easily make getters and setters for the fields of a class. There are several ways to do it depending on if you want special behavior for your getters and setters. In C# we do not use "getName()" or "setName(String name)" methods.
Expression-bodied:
```
private string street;
public string Street {
  set => street = value;
  get => Street;
}
```
There is no need to declare the types or names of the input, the compiler knows based on propery declaration.

Auto-properties:
```
public int BuildingNumber {get; set;}
public string City {get; set;}
public string Province {get; set;}
public string ZipCode {get; set;}
```
A shorthand way of creating a field in a class and its getter and setter, used when you only need the basic get/set functionality.

Custom properties:
```
public string Address {
  get {return $"{BuildingNumber} {Street} {City} {Province} {ZipCode}"}
}
```
Used for when you want to define special behavior. Could also be used to validate data before being stored.
To access these getters and setters it is simply as follows.
```
Building cityHall = new Building();
cityHall.Street = "Macleod Trail";
cityHall.BuildingNumber = 800;
cityHall.City = "Calgary";
cityHall.Province = "Alberta";
cityHall.ZipCode = "T2G 5E6";
Console.WriteLine(cityHall.Address);
```
There is also a special property called an indexer, it provides a way to access fields using a key. A class that defines an indexer allows the class to be indexed like an array.
```
public class Block {
  private Building[] block = new Buidling[10];

  public Building this[int i]{
    get {
      return block[i];
    }
    set {
      block[i] = value;
    }
  }
}
```

### Enum
Enumeration is a special type, similar to a class but it is a group of constants. When a enum is instantiated the value must be one of the defined valued.
```
enum Priority {
  High,
  Medium,
  Low
}
```
The values of the enum are indexed starting at 0. Indexes can be assigned.
```
enum Priority {
  High = 2,
  Medium = 1,
  Low = 0
}
```
Enums can be accessed as if they were constants in a class and indexes can be accessed through a cast.
```
var priority = Priority.High;
int priorityLevel = (int) Priority.Low;
```

### Struct
Are data structures that can typically be used in place of classes that are flat, meaning classes that only contain primitive values and do not contain other objects. As apposed to classes when a struct is passed into a method it is passed by value while classes are passed by reference. This results in structs being faster but requiring more memory to work with since all values must be copied.
As an example let's turn our Building class from earlier into a struct.
```
struct Building {
  public string Street;
  public int BuildingNumber;
  public string City;
  public string Province;
  public string ZipCode;

  public Building (string street, int buildingNumber, string city, string province, string zipCode){
    Street = street;
    BuildingNumber = buildingNumber;
    City = city;
    Province = province;
    ZipCode = zipCode;
  }
}
```

### ref
The "ref" keyword may be used in method signatures to allow primitive data types to be passed by reference. Normally when for example an int is passed into a method it cannot be changed within the method, but using the "ref" keyword allows us to do so. Parameters marked with the "ref" keyword must have been initialized prior to passing it.
```
public void Square (ref int x){
  x = x * x;
}
```
If we were to pass a value into this method the variable would be changed in the caller location.

### in
Another way to use pass by reference, it is exactly like "ref" but it does not allow the value to be changed. Useful for reducing memory usage.
```
public void PrintAddress (in Building building) {
  Console.WriteLine(building.Address);
}
```

### out
This keyword can be used to allow a method to return multiple values. Parameters marked with the "out" keyword are passed by reference. It is similar to "ref" but the variable being passed does not need to have been initialized first, though it must be initialized in the method itself before being returned. Both the method and the caller must use the "out" keyword.
```
public boolean CalculatePositiveHypotenuse (int a, int b, out int c) {
  if (a < 1 || b < 1>){
    c = 0;
    return false;
  } else {
    c = Math.Sqrt((a * a) + (b * b));
    return true;
  }
}
```
This funtion will attempt to find the hypotenuse of a triangle but will only accept postive numbers, it will set c to 0 and return false if the given parameters are invalid. Otherwise it will calculate the hypotenuse and return true;

Typically C# developers will almost never use "ref", "in" or "out" but they are useful to be familiar with. "out" has the most practical use in returning a boolean to represent validity/success along with another return value.

### Demonstration
For this blog post I have created a program for a bike shop that will allow the user to enter a number of invoices into the program and display their current invoice, save their invoice to a list and save that list of invoices to a file. Also to read a list of invoices from a file. And to display the list of invoices in a table.

[Video Demonstration](https://www.youtube.com/watch?v=qJxPkjNIRRs)

[Repository](https://github.com/emclaughlin5/BikeShop)

### Conclusion
I hope you have enjoyed my blog posts and learned something new about C# and Object Oriented Programming. If you want to extend your knowledge further I would recommend using the main resource I used, it has a lot more than I could reasonably include in a couple blog posts. It can be found [here](https://github.com/Almantask/CSharp-From-Zero-To-Hero).