# **Arrays**

Arrays are objects, so they have their own methods.

**Push** - Add element to the end of the array
**Unshift** - Add element to the beginning of the array
**Pop** - Removes the last element of the array
**Shift** - Removes the first element of the array
**IndexOf** - Returns the index number of the provided element
**Includes** - Returns whether the provided element is part of the array (true/false)
**Slice** - Returns elements from the array
**Splice** - Similar to slice except that it mutates the original array
**Reverse** - Returns the array in the opposite order, which mutates the array
**Concat** - Returns the two arrays together as a string but doesn't mutate either one
**Join** - Joins the string elements into an array
**Map** - Returns a new array containing the results of applying an operation on all original array elements
**Filter** - Returns a new array containing the array elements that passed a specified test condition
**Reduce** - Boils ("reduces") all array elements down to one single value (e.g. adding all elements together)

For loops with arrays:

```javascript
const jeanineArray - [
	'Jeanine',
	'Kimball',
	2021 - 1971,
	'programmer',
	['Miguel', 'Greg', 'Keith']
];

const types = [];
 for (let index = 0; index < jeanineArray.length; index++) {
    // Reading from the array
    console.log(jeanineArray[index], typeof jeanineArray[index]);
    // Writing into array
    types[index] = typeof jeanineArray[index];
OR
    types.push(typeof jeanineArray[index]);
}

let arr = ['a', 'b', 'c', 'd', 'e'];

 // Slice just sends back elements from the array
console.log(arr.slice(2)); // gets third element and all others after
console.log(arr.slice(2,4)); //gets third and fourth element
console.log(arr.slice(-2)); //last 2 elements
console.log(arr.slice(-1)); //last element
console.log(arr.slice(1,-2)); //gets second element and third element
console.log(arr.slice());
console.log(...arr);
 
// Splice changes (mutates) the original array
// console.log(arr.splice(2));
console.log('--- Splice ---');
arr.splice(-1); //everything but the last element
console.log(arr);
arr.splice(1,2);
console.log(arr);
 
// Reverse changes (mutates) the original array
console.log('--- Reverse ---');
const arr2 = ['j', 'i', 'h', 'g', 'f'];
console.log(arr2.reverse());
console.log(arr2);
 
// Concat connects arrays but doesn't change the original arrays
console.log('--- Concat ---');
const letters = arr.concat(arr2);
console.log(letters);
console.log(...arr, ...arr2);
 
// Join
console.log('--- Join ---');
console.log(letters.join('-'));

```

