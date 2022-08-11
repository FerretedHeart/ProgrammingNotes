# **Objects**

Objects can be assigned to variables just like any JavaScript type. We use curly braces to designate an *object literal*. The data is organized into *key-value pairs*. A key is like a variable name that points to a location in memory that holds a value. A key's value can be of any data type in the language including functions or other objects. Key-value pairs are separated by commas.

```javascript
let objectName = {
  property: value,
  property: value
};
```

When calling objects, you can either use <u>dot</u> notation or <u>bracket</u> notation.
<u>Brackets</u> allow you to use expressions, such as the variable holding the property name (same as when indexing an array).
<u>Dots</u> are used most.

Functions are values, a method is also a property, so functions can be added to an object.

```javascript
let spaceship = {
  'Fuel Type': 'Turbo Fuel',
  'Active Duty': true,
  homePlanet: 'Earth',
  color: 'silver'
};

// Dot notation
spaceship.homePlanet; // Returns 'Earth'
spaceship.color; // Returns 'silver'

// Bracket notation
spaceship['Active Duty']; // Returns true
spaceship['Fuel Type']; // Returns 'Turbo Fuel'
spaceship['!!!!']; // Returns undefined
```

With bracket notation you can also use a variable inside the brackets to select the keys of an object. This can be especially helpful when working with functions:

```javascript
let returnAnyProp = (objectName, propName) => objectName[propName];

returnAnyProp(spaceship, 'homePlanet'); // Returns 'Earth'
```



## Property Assignment

Objects are *mutable* meaning they can be updated after being created. We can use either dot notation or bracket notation and the assignment operator to add new key-value pairs to an object or change an existing property.

- If the property already exists on the object, whatever value it held before will be replaced with the newly assigned value.
- If there was no property with that name, a new property will be added to the object.

```typescript
const spaceship = {type: 'shuttle'};
spaceship = {type: 'alien'}; // TypeError: Assignment to constant variable
spaceship.type = 'alien'; // Changes the value of the type property
spaceship.speed = 'Mach S'; // Creates a new key of 'speed' with a value of 'Moch S'
```

You can delete a property from an object with the `delete` operator.

```javascript
const spaceship = {
  'Fuel Type': 'Turbo Fuel',
  homePlanet: 'Earth',
  mission: 'Explore the universe'
};

delete spaceship.mission; // Removes the mission property
```



## Methods

[Built-in Object Methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object#Methods)

When the data stored on an object is a function we call that a *method*. A property is what an object has, while a method is what an object does.

```javascript
const alienShip = {
  invade: function() {
    console.log('Hello! We have come to dominate your planet. Instead of Earth, it shall be called New Xaculon.')
  }
};

// ES6 syntax
const alienShip = {
  invade () {
    console.log('Hello! We have come to dominate your planet. Instead of Earth, it shall be called New Xaculon.')
  }
};
```

Object methods are invoked by appending the object's name with the dot operator followed by the method name and parentheses.

```javascript
alienShip.invade();
```

:warning:Arrow functions inherently *bind*, or tie, an already defined `this` value to the function itself that is NOT the calling object. **Avoid** using arrow functions when using `this` in a method.

## Nested Objects

An object might have another object as a property which in turn could have a property that's an array of even more objects.

```javascript
const spaceShip = {
  telescope: {
    yearBuilt: 2018,
    model: '91031-XLT',
    focalLength: 2032
  },
  crew: {
    captain: {
      name: 'Sandra',
      degree: 'Computer Engineering',
      encourageTeam() {
        console.log('We got this!')
      }
    }
  },
  engine: {
    model: 'Nimbus2000'
  },
  nanoelectronics: {
    computer: {
      terabytes: 100,
      monitors: 'HD'
    },
    'back-up': {
      battery: 'Lithium',
      terabytes: 50
    }
  }
};
```

We can chain operators to access nested properties.

```javascript
spaceship.nanoelectronics['back-up'].battery; // Returns 'Lithium'
```



## Pass By Reference

Objects are *passed by reference*. This means when we pass a variable assigned to an object into a function as an argument, the computer interprets the parameter name as pointing to the space in memory holding that object. As a result, functions which change object properties actually mutate the object permanently (even when the object is assigned to a `const` variable).

```javascript
const spaceship = {
  homePlanet: 'Earth',
  color: 'silver'
};

let paintIt = obj => {
  obj.color = 'glorious gold'
};

paintIt(spaceship);

spaceship.color // returns 'glorious gold'
```



## Looping Through Objects

We can iterate through arrays using their numerical indexing but the key-value pairs in objects aren't ordered. For objects we use the `for...in` syntax, which will execute a given block of code for each property in an object.

```javascript
let spaceship = {
  crew: {
    ccaptain: {
      name: 'Lily',
      degree: 'Computer Engineering',
      cheerTeam() {console.log('You got this!')}
    },
    'chief officer': {
      name: 'Dan',
      degree: 'Aerospace Engineering',
      agree() {console.log('I agree, captain!')}
    },
    medic: {
      name: 'Clementine',
      degree: 'Physics',
      announce() {console.log('Jets on!')}
    },
    translator: {
      name: 'Shauna',
      degree: 'Conservation Science',
      powerFuel() {console.log('The tank is full!')}
    }
  }
};

// for...in
for (let crewMember in spaceship.crew) {
  console.log(`${crewMember}: ${spaceship.crew[crewMember].name}`);
}
```



## The this Keyword

The `this` keyword references the *calling object* which provides access to the calling object's properties.

```javascript
const goat = {
  dietType: 'herbivore',
  makeSound() {
    console.log('baaa');
  },
  diet() {
    console.log(this.dietType);
  }
};

goat.diet(); // Output: herbivore
```



## Getters/Setters

*Getters* are methods that get and return the internal properties of an object.

*Setters* reassign values of existing properties within an object.

```javascript
// Getter
const person = {
  _firstName: 'John',
  _lastName: 'Doe',
  get fullName() {
    if(this._firstName && this._lastName) {
      return `${this._firstName} ${this._lastName}`;
    } else {
      return 'Missing a first name or a last name.';
    }
  }
}

// To call the getter method:
person.fullName; // 'John Doe'

// Setter
const person = {
  _age: 37,
  set age(newAge) {
    if(typeof newAge === 'number') {
      this._age = newAge;
    } else {
      console.log('You must assign a number to age');
    }
  }
};

// To use the setter method:
person.age = 40;
console.log(person._age); // Logs: 40
person.age = '40'; // Logs: You must assign a number to age
```

Getter/setter methods do not need to be called with a set of parentheses. Syntactically, it looks like we're accessing/reassigning the value of a property.

- Getters can perform an action on the data when getting a property.
- Getters can return different values using conditionals.
- In a getter, we can access the properties of the calling object using `this`.
- The functionality of the code is easier for other developers to understand.

Properties cannot share the same name as the getter/setter function. If we do so, then calling the method will result in an infinite call stack error. One workaround is to add an underscore before the property name.



## Factory Functions

A factory function is a function that returns an object and can be reused to make multiple object instances. Factory functions can also have parameters allowing to customize the object that gets returned.

```javascript
const monsterFactory = (name, age, energySource, catchPhrase) => {
  return {
    name: name,
    age: age,
    energySource: energySource,
    scare() {
      console.log(catchPhrase);
    }
  }
};

const ghost = monsterFactory('Ghouly', 251, 'ectoplasm', 'BOO!');
ghost.scare(); // 'BOO!'

// Property value shorthand
const monsterFactory = (name, age) => {
  return {
    name,
    age
  }
};
```



## Destructured Assignment

In destructured assignment we create a variable with the name of an object's key that is wrapped in curly braces and assign it to the object.

```javascript
const vampire = {
  name: 'Dracula',
  residence: 'Transylvania',
  preferences: {
    day: 'stay inside',
    night: 'satisfy appetite'
  }
};

const residence = vampire.residence;
console.log(residence); // Prints 'Transylvania'

// Destructured assignment
const { residence } = vampire;
console.log(residence); // Prints 'Transylvania'

// Nested properties
const { day } = vampire.preferences;
console.log(day); // Prints 'stay inside'

----------------------------

let destinations = { x: 'LA', y: 'NYC', z: 'MIA'};
let {x, y, z} = destinations;
console.log(x, y, z); // Prints LA NYC MIA
```



