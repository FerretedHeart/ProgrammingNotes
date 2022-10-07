# Classes and Methods

## Methods

- [Methods](https://codegym.cc/groups/posts/34-methods)
- [Method signature](https://codegym.cc/groups/posts/26-method-signatures)
- [Method overriding](https://codegym.cc/groups/posts/100-how-method-overriding-works)
- [Getters and setters](https://codegym.cc/groups/posts/13-getters-and-setters)
- [Fixed values in Java](https://codegym.cc/groups/posts/30-fixed-values-in-java-final-constants-and-immutable)
- [Inheritance and instanceof](https://codegym.cc/groups/posts/31-instanceof-and-inheritance-101-)
  - [How instanceof works](https://codegym.cc/groups/posts/105-how-the-instanceof-operator-works)
- [Wrappers, unboxing and boxing](https://codegym.cc/groups/posts/32-wrappers-unboxing-and-boxing)
- [Enum](https://codegym.cc/groups/posts/154-how-to-use-the-enum-class)
- [Common mistakes](https://codegym.cc/groups/posts/172-8-common-mistakes-made-by-rookie-programmers)

## Classes

- [Base class constructors](https://codegym.cc/groups/posts/12-base-class-constructors--)
- [Relationships between classes](https://codegym.cc/groups/posts/96-relationships-between-classes-inheritance-composition-and-aggregation)
- [Principles of encapsulation](https://codegym.cc/groups/posts/98-principles-of-encapsulation)
- [Polymorphism](https://codegym.cc/groups/posts/99-how-to-use-polymorphism)
- [Abstract classes](https://codegym.cc/groups/posts/103-specific-examples-of-abstract-classes-in-java)
- [Interfaces](https://codegym.cc/groups/posts/101-why-interfaces-are-necessary-in-java)
- [Default methods in interfaces](https://codegym.cc/groups/posts/102-default-methods-in-interfaces)
- [Abstract vs Interface](https://codegym.cc/groups/posts/104-the-difference-between-abstract-classes-and-interfaces)
- [Annotations Part 1](https://codegym.cc/groups/posts/312-annotations-part-1--a-little-boring?utm_source=eSputnik-$email-digest-27&utm_medium=email&utm_campaign=$email-digest-27&utm_content=853853028)
- [Annotations Part 2](https://codegym.cc/groups/posts/313-annotations-part-2-lombok)
- [Singleton and Serialization](https://javarevealed.wordpress.com/tag/singleton-and-serialization/)
- [Anonymous Classes](https://codegym.cc/groups/posts/261-anonymous-classes)
- [Nested Inner Classes](https://codegym.cc/groups/posts/262-nested-inner-classes)
- [Static Nested Classes](https://codegym.cc/groups/posts/207-static-nested-classes)
- [Nested Classes (Oracle)](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html)
- [Inner Classes in a Local Method](https://codegym.cc/groups/posts/263-inner-classes-in-a-local-method)
- [Final Keyword](https://codegym.cc/groups/posts/206-lets-talk-about-final)
- [Inheritance of Nested Classes](https://codegym.cc/groups/posts/260-examples-of-inheritance-of-nested-classes)
- [Enum Practical Examples](https://codegym.cc/groups/posts/258-enum-practical-examples-adding-constructors-and-methods)

## General form of a method

```java
access modifier, **return** type, method name (parameter list) {
   *// method body
 }*
```

1. **Access modifier:** always first and indicates how other classes see them; adding static is optional (can be used without a reference to a specific object of the class)
2. **Return type:** used to return value; if no value is being returned, then void is used
3. **Parameter list:** 

1. - Allows there to be more than one method with the same name provided the parameters differ
   - Referred to as method overloading
   - "String...names" will pass a collection of names to the parameter and can be retrieved      like an array with a for each loop
   - Order of parameter matters
   - Constructors are also methods
   - Nulls can be converted to a specific reference type such as : (String) null but primitives do not need it since primitives cannot be null

```java
// Example:

public class Dog {
   String name;
   public Dog(String name) {
    this.name = name;
  }
  public static void main(String[] args) {
    Dog max = new Dog("Max");
    max.woof();
   }
   public void woof() {
    System.out.println("A dog named " + name + " says \"Woof, woof!\"");
  }
   public void run(int distanceInFeet) {
    System.out.println("A dog named " + name + " ran " + distanceInFeet + " feet!");
  }
   public String getName() {
    return name;
  }
 }
```

 **Reflection** is a class's ability to obtain information about itself. Java has special classes: Field and Method, which are similar to the Class class for classes. Just as Class objects let you get information about a class, Field objects provide information about a field, and the Method object provides information about a method. And look at what you can do with them:

| Java  Code                                                   | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Class[] interfaces = List.class.getInterfaces();             | Gets a list of  Class objects for the List class's interfaces |
| Class parent = String.class.getSuperclass();                 | Gets the Class  object of the String class's parent class    |
| Method[] methods = List.class.getMethods();                  | Gets a list of the  List class's methods                     |
| String s = String.class.newInstance();                       | Creates a new  String                                        |
| String s = String.class.newInstance();<br />Method m = String.class.getMethod("length");<br />int length = (int) m.invoke(s) | Gets the String  class's length method and calls it on the String s |

##  Nested Classes

A class declared inside another class is a nested class. Non-static nested classes are called inner classes.

Objects of an inner class are nested inside objects of the outer class and can therefore access the outer class's variables.

```java
public class Car {
  int height = 160;
	ArrayList doors = new ArrayList();
  public Car {
    doors.add(new Door());
    doors.add(new Door());
    doors.add(new Door());
    doors.add(new Door());
  }
  class Door() {
    public int getDoorHeight() {
      return (int)(height * 0.80);
    }
  }
}
```

- You can't create a Door object inside a static method of the Car class because static methods don't contain a reference to the Car object that is implicitly passed to the Door constructor.

```java
public class Car {
  public static Door createDoor() {
    Car car = new Car(); 
    return** car.new Door();
  }
  public class** Door {
    int width, height;
  }
}
```

- The Door class cannot contain static variable or methods.
- If the inner class is declared as public, instances of it can be created outside of the outer class, but an instance of the outer class must exist first.

```java
Car car = new Car();
Car.Door door = car.new Door();
Car.Door door = new Car().newDoor();
```

- To access a variable from the     outer class when it is hidden, or to access 'this' inside an inner class,     simply write "YourCl'ssName.this'.

```java
Car.this
Car.Door.this
Car.Door.InnerClass2.InnerClass3.this
```

- When creating objects of a nested class outside the outer class, you must also use the dot operator to specify the name of the outer class.

```java
Zoo.Mouse mouse = new Zoo.Mouse();
```

- The Zoo.Mouse class and its objects have access to the Zoo class's private static variables and methods (since the Mouse class is also declared inside the Zoo class).

## Anonymous Inner Classes

When a class needs to inherit several classes, since Java doesn't support multiple inheritance, you create inner classes.

```java
class Tiger extends Cat {
  public void tigerRun() {
    .....
  }
  public void startTiger() {
    thread.start();
  }
  private TigerThread thread = new TigerThread();
  private class TigerThread extends Thread {
    public void run() {
      tigerRun();
    }
  }
}
```

We can combine four things into one with anonymous inner classes:

1. Declaration of a derived class
2. Method overriding
3. Declaration of a variable
4. Creation of an instance of a derived class

| Without  anonymous class                                     | With  anonymous class            |
| ------------------------------------------------------------ | -------------------------------- |
| Cat tiger = new Tiger();  class Tiger extends Cat     {     } | Cat tiger = **new** Cat()  {  }; |