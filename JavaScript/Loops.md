# **Loops**

# For

```javascript
for(let i = jeanineArray.length - 1; i >= 0; i--) {
    console.log(jeanineArray[i]);
}
 for(let exercise = 1; exercise < 4; exercise++) {
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

ForEach can be used instead of the for/of loop, but you cannot break out of it.

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

