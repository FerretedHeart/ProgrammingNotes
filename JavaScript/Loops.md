# **Loops**

Repeat a given block of code on its own. Also known as iterators.

[Iterator Methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)



# For

Includes an *iterator variable* that usually appears in all three expressions. The iterator variable is initialized, checked against the stopping condition, and assigned a new value on each loop iteration.

1. an *initialization* starts the loop and can be used to declare the iterator variable
2. a *stopping condition* is the condition that the iterator variable is evaluated against
3. an *iteration statement* is used to update the iterator variable on each loop

```javascript
for(let i = jeanineArray.length - 1; i >= 0; i--) { // reverse for loop
    console.log(jeanineArray[i]);
}
 for(let exercise = 1; exercise < 4; exercise++) { // forward for loop
    console.log(`---- Starting exercise ${exercise}`);
     for(let rep = 1; rep < 6; rep++) {
        console.log(`Lifting weight repetition ${rep}`);
    }
}
```

There is also a for/of loop introduced in ES6.

A regular for/of loop that displays each item from an array:

```javascript
for (const item of menu) console.log(item);
```

This version allows you to get the index as well as the item from the array using destructuring:

```javascript
for (const [i, el] of menu.entries()) {
  console.log(`${i + 1}: ${el}`);
}
```



# While

Best used when the number of times is unknown. Conditional variable is set outside of the loop.

```javascript
let rep = 1;
while(rep <= 10) {
    console.log(`Lifting weights repetition ${rep}`);
    rep++;
}
 let dice = Math.trunc(Math.random() * 6) + 1;
 while(dice !== 6) {
    console.log(`You rolled a ${dice}`);
    dice = Math.trunc(Math.random() * 6) + 1;
}
```



# Do...While

When you want a piece of code to run at least once and then loop based on the condition.

```javascript
let countString = '';
let i = 0;

do {
  countString = countString + i;
  i++;
} while (i < 5);
console.log(countString);

----------------------------------------------

let cupsOfSugarNeeded = 3;
let cupsAdded = 0;

do {
  cupsAdded++;
  console.log(cupsAdded);
} while (cupsAdded < cupsOfSugarNeeded);
```



# The break Keyword

The `break` keyword allows programs to "break" out of the loop from within the loop's block.

```javascript
for(let i = 0; i < 99; i++) {
  if (i > 2) {
    break;
  }
  console.log('Banana');
}
console.log('Orange you glad I broke out the loop!');
```



# Looping Objects

```javascript
// Property NAMES
const properties = Object.keys(openingHours);
console.log(properties);

 let openStr = `We are open on ${properties.length} days: `;

 for (const day of properties) {
  openStr += `${day}, `;
}
console.log(openStr);

 // Property VALUES
const values = Object.values(openingHours);
console.log(values);

 // Entire object
const entries = Object.entries(openingHours);
console.log(entries);

 for (const [key, { open, close }] of entries) {
  console.log(`On ${key} we open at ${open} and close at ${close}`);
}
```



# ForEach

ForEach can be used instead of the for/of loop, but you cannot break out of it. It will execute the same code for each element of an array but does not change the array and returns `undefined`.

```javascript
console.log('-- ForEach ---');
movements.forEach(function(movement) {
  if(movement > 0) {
    console.log(`You deposited ${movement}`);
  } else {
    console.log(`You withdrew ${movement}`);
  }
});

 movements.forEach(function(mov, i, arr) {
  if(mov > 0) {
    console.log(`Movement ${i + 1}: You deposited ${mov}`);
  } else {
    console.log(`Movement ${i + 1}:You withdrew ${Math.abs(mov)}`);
  }
});
```



## The .map() Method

When `.map()` is called on an array, it takes an argument of a callback function and returns a new array.

```javascript
const numbers = [1, 2, 3, 4, 5];

const bigNumbers = numbers.map(number => {
  return number * 10;
});
```



## The .filter() Method

This method returns a new array, however `.filter()` returns an array of elements after filtering out certain elements from the original array. The callback function for the `.filter()` method should return `true` or `false` depending on the element that is passed to it.

```javascript
const words = ['chair', 'music', 'pillow', 'brick', 'pen', 'door'];

const shortWords = words.filter(word => {
  return word.length < 6;
});
```



## The .findIndex() Method

Calling `.findIndex()` on an array will return the index of the first element that evaluates to `true` in the callback function. It returns `-1` if none of the elements in the array satisfies the condition.

```javascript
const jumbledNums = [123, 25, 78, 5, 9];

const lessThanTen = jumbledNums.findIndex(num => {
  return num < 10;
});
```



## The .reduce() Method

The `.reduce()` method returns a single value after iterating through the elements of an array, thereby reducing the array.

```javascript
const numbers = [1, 2, 4, 10];

const summedNums = numbers.reduce((accumulator, currentValue) => {
  return accumulator + currentValue
})

console.log(summedNums); // Output: 17
```

You can also add an optional second parameter to set an initial value for `accumulator` (the first argument is the callback function).

```javascript
const newNumbers = [1, 3, 5, 7];

const newSum = newNumbers.reduce((accumulator, currentValue) => {
  console.log('The value of accumulator: ' + accumulator);
  console.log('The value of currentValue: ' + currentValue);
  return accumulator + currentValue;
}, 10);

console.log(newSum);
```

