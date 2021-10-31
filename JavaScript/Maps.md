The Map object holds key-value pairs and remembers the original insertion order of the keys. Any value (both objects and primitive values) may be used as either a key or a value.

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map 

Methods to manipulate are similar to sets.

```javascript
const rest = new Map();
rest.set('name', 'Classico Italiano');
rest.set(1, 'Firenze, Italy');
console.log(rest.set(2, 'Lisbon, Portugal'));

 rest
  .set('categories', ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'])
  .set('open', 11)
  .set('close', 23)
  .set(true, 'We are open')
  .set(false, 'We are closed');
 
console.log(rest.get('name'));
console.log(rest.get(true));
 
const time = 8;
console.log(rest.get(time > rest.get('open') && time < rest.get('close')));
 
console.log(rest.has('categories'));
rest.delete(2);
// rest.clear();
 
const arr = [1, 2];
rest.set(arr, 'Test');
rest.set(document.querySelector('h1'), 'Heading');
console.log(rest);
console.log(rest.size);
 
console.log(rest.get(arr));

const question = new Map([
  ['question', 'What is the best programming language in the world?'],
  [1, 'C'],
  [2, 'Java'],
  [3, 'JavaScript'],
  ['correct', 3],
  [true, 'Correct'],
  [false, 'Try again'],
]);
console.log(question);

 // Convert object to map
console.log(Object.entries(openingHours));
const hoursMap = new Map(Object.entries(openingHours));
console.log(hoursMap);

 // Quiz app
console.log(question.get('question'));
for (const [key, value] of question) {
  if (typeof key === 'number') console.log(`Answer${key}: ${value}`);
}

// const answer = Number(prompt('Your answer: '));
const answer = 3;
console.log(answer);
 
console.log(question.get(answer === question.get('correct')));
 
// Convert map to array
console.log([...question]);
console.log(question.entries());
console.log([...question.keys()]);
console.log([...question.values()]);
```

