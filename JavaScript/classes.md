# Classes

Classes are templates for objects, and are a way to reduce duplicate code and debugging time.

## Constructor

JavaScript calles the `constructor()` method every time it creates a *new instance* of a class.

```javascript
class Dog {
  constructor(name) {
    this.name = name;
    this.behavior = 0;
  }
}
```

- `Dog` is the name of our class. By convention, we capitalize and CamelCase class names.
- JavaScript will invoke the `constructor()` method every time we create a new instance of our `Dog` class.
- This `constructor()` method accepts one argument, `name`.
- Inside of the `constructor()` method, we use the `this` keyword. In the context of a class, `this` refers to an instance of that class. In the `Dog` class, we use `this` to set the value of the Dog instance's `name` property to the `name` argument.
- Under `this.name`, we create a property called `bahevior`, which will keep track of the number of times a dog misbehaves. The `behavior` property is always initialized to zero.

## Instance

An *instance* is an object that contains the property names and methods of a class, but with unique property values.

```javascript
class Dog {
  constructor(name) {
    this.name = name;
    this.behavior = 0;
  } 
}
 
const halley = new Dog('Halley'); // Create new Dog instance
console.log(halley.name); // Log the name value saved to halley
// Output: 'Halley'
```

We use the `new` keyword to create an instance of our `Dog` class.

- We create a new variable named `halley` that will store an instance of our `Dog` class.
- We use the `new` keyword to generate a new instance of the `Dog` class. The `new` keyword calls the `constructor()`, runs the code inside of it, and then returns the new instance.
- We pass the `'Halley'` string to the `Dog` constructor, which sets the `name` property to `'Halley'`.
- Finally, we log the value saved to the `name` key in our `halley` object, which logs `'Halley'` to the console.

## Methods

Class method and getter syntax is the same as it is for objects except you can not include commas between methods.

```javascript
class Dog {
  constructor(name) {
    this._name = name; //underscores indicate these properties should not be accessed directly
    this._behavior = 0;
  }
 
  get name() {
    return this._name;
  }
 
  get behavior() {
    return this._behavior;
  }
 
  incrementBehavior() {
    this._behavior++;
  }
}
```

## Method Calls

```javascript
const halley = new Dog('Halley');

let nikko = new Dog('Nikko'); // Create dog named Nikko
nikko.incrementBehavior(); // Add 1 to nikko instance's behavior
let bradford = new Dog('Bradford'); // Create dog name Bradford
console.log(nikko.behavior); // Logs 1 to the console
console.log(bradford.behavior); // Logs 0 to the console
```

The syntax for calling methods and getters on an instance is the same as calling them on an object - append the instance with a period, then the property or method name. For methods, you must also include opening and closing parentheses.

## Inheritance

When multiple classes share properties or methods, they become candidates for *inheritance* - a tool developers use to decrease the amount of code they need to write.

With inheritance, you can create a *parent* class (also known as a superclass) with properties and methods that multiple *child* classes (also known as subclasses) share. The child classes inherit the properties and methods from their parent class. 

So in the above code example of a `Dog` class, we can change it to `Animal` and then `Dog` would inherit `Animal` with no additional properties or methods.

```javascript
class Cat extends Animal {
  constructor(name, usesLitter) {
    super(name);
    this._usesLitter = usesLitter;
  }
}

const bryceCat = new Cat('Bryce', false); 
console.log(bryceCat.name); // output: Bryce as we're able to use the parent's methods from inheritance
```

- The `extends` keyword makes the methods of the animal class available inside the cat class.
- The constructor, called when you create a new `Cat` object, accepts two arguments, `name` and `usesLitter`.
- The `super` keyword calls the constructor of the parent class. In this case, `super(name)` passes the name argument of the `Cat` class to the constuctor of the `Animal` class. When the `Animal` constructor runs, it sets `this._name = name` for new `Cat` instances.
- `_usesLitter` is a new property that is unique to the `Cat` class, so we set it in the `Cat` constructor.

Child classes can contain their own properties, getters, setters, and methods while still being able to access those of the parent class.

## Static Methods

Sometimes you will want a class to have methods that aren't available in individual instances, but that you can call directly from the class.

```javascript
class Animal {
  constructor(name) {
    this._name = name;
    this._behavior = 0;
  }
 
  static generateName() {
    const names = ['Angel', 'Spike', 'Buffy', 'Willow', 'Tara'];
    const randomNumber = Math.floor(Math.random()*5);
    return names[randomNumber];
  }
} 
```

We create a `static` method called `.generateName()` that returns a random name when it's called. Because of the `static` keyword, we can only access `.generateName()` by appending it to the `Animal` class. We call the `.generateName()` method with the following syntax:

```javascript
console.log(Animal.generateName()); // returns a name
```

You cannot access the method from instances of the class or instances of its subclasses as it will result in an error.
