https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set

The Set object lets you store unique values of any type, whether primitive values or object references, unlike arrays which allows repeats. There are also no indexes.

You get the length of arrays with length. With sets you use size.
To find a value, with arrays you use include, with sets you use has.

You can also add to and delete from the set very easily with methods.

```javascript
const ordersSet = new Set([
  'Pasta',
  'Pizza',
  'Pizza',
  'Risotto',
  'Pasta',
  'Pizza',
]);

console.log(ordersSet);
 console.log(new Set('Jeanine'));
 console.log(ordersSet.size);
console.log(ordersSet.has('Pizza')); true
ordersSet.add('Garlic Bread');
ordersSet.delete('Risotto');
// ordersSet.clear();
console.log(ordersSet);

 for (const order of ordersSet) console.log(order);

 // Example
const staff = ['Waiter', 'Chef', 'Waiter', 'Manager', 'Chef', 'Waiter'];

const staffUnique = [...new Set(staff)];
console.log(staffUnique);
console.log(new Set(staff).size); //3
console.log(new Set('jeanine').size); //5

```

