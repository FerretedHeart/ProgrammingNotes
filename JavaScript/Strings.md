# **Strings**

```javascript
const airline = 'TAP Air Portugal';
const plane = 'A320';

console.log(plane[0]);
console.log(airline.length);

console.log(airline.indexOf('r')); // first instance
console.log(airline.lastIndexOf('r')); // last instance
console.log(airline.indexOf('Portugal')); // first instance of word
 
console.log(airline.slice(4)); // extraction start point
console.log(airline.slice(4, 7)); // second number is where it ends
 
console.log(airline.slice(0, airline.indexOf(' '))); // find first space
console.log(airline.slice(airline.lastIndexOf(' ') + 1)); // find last space
 
console.log(airline.slice(-2)); // returns last two characters
console.log(airline.slice(1, -1)); // removes first and last characters
 
const checkMiddleSeat = function (seat) {
  // B and E are middle seats
  const s = seat.slice(-1);
  if (s === 'B' || s === 'E') console.log('You got the middle seat.');
  else console.log('You got lucky');
};
 checkMiddleSeat('11B');
checkMiddleSeat('23C');
checkMiddleSeat('3E');

const airline = 'TAP Air Portugal';
 console.log(airline.toLowerCase());
console.log(airline.toUpperCase());
 
// Fix capitalization in name
const passenger = 'jOnAS'; // Jonas
const passengerLower = passenger.toLowerCase();
const passengerCorrect =
  passengerLower[0].toUpperCase() + passengerLower.slice(1);
console.log(passengerCorrect);
 
const fixName = function (name) {
  const nameLower = name.toLowerCase();
  return nameLower[0].toUpperCase() + nameLower.slice(1);
};
console.log(fixName('jeAnInE'));
 
// Comparing email
const email = 'hello@jonas.io';
const loginEmail = '  Hello@Jonas.Io \n';

// const lowerEmail = loginEmail.toLowerCase();
// const trimmedEmail = lowerEmail.trim(); //removes spaces and special characters at ends

const normalizedEmail = loginEmail.toLowerCase().trim();
console.log(normalizedEmail);
console.log(email === normalizedEmail);
 
// Replacing
const priceGB = '288,97£';
const priceUS = priceGB.replace('£', '$').replace(',', '.'); // replaces first instance
console.log(priceUS);
 
const announcement =
  'All passengers come to boarding door 23. Boarding door 23!';
 console.log(announcement.replaceAll('door', 'gate')); // replaces all instances
console.log(announcement.replaceAll(/door/g, 'gate'));
 
// Boolean
const plane = 'Airbus A320neo';
console.log(plane.includes('A320'));
console.log(plane.includes('Boeing'));
console.log(plane.startsWith('Air'));
 if (plane.startsWith('Airbus') && plane.endsWith('neo')) {
  console.log('Part of the new Airbus family');
}
 
// Practice exercise
const checkBaggage = function (items) {
  const baggage = items.toLowerCase();
  if (baggage.includes('knife') || baggage.includes('gun')) {
    console.log('You are not allowed on board');
  } else {
    console.log('Welcome aboard!');
  }
};

checkBaggage('I have a laptop, some Food, and a pocket Knife');
checkBaggage('I have socks and camera');
checkBaggage('Got some snacks and a gun for protection');

// Split and join
console.log('a+very+nice+string'.split('+'));
console.log('Jeanine Kimball'.split(' '));
 
const [firstName, lastName] = 'Jeanine Kimball'.split(' ');
 
const newName = [['Mrs.', firstName, lastName.toUpperCase()].join(' ')];

console.log(newName);
 
const capitalizeName = function (name) {
  const names = name.split(' ');
  const namesUpper = [];
   for (const n of names) {
    // namesUpper.push(n[0].toUpperCase() + n.slice(1));
    namesUpper.push(n.replace(n[0], n[0].toUpperCase()));
  }
  console.log(namesUpper.join(' '));
};
capitalizeName('jessica ann smith davis');
capitalizeName('jeanine kimball');
 
// Padding a string
const message = 'Go to gate 23!';
console.log(message.padStart(25, '+').padEnd(35, '+'));
console.log('Jeanine'.padStart(25, '*'));
 
const maskCreditCard = function (number) {
  const str = number + ''; // Same as String(number)
  const last = str.slice(-4);
  return last.padStart(str.length, '*');
};
 console.log(maskCreditCard(433792928393938399));
console.log(maskCreditCard('83892898392'));
 
// Repeat
const message2 = 'Bad weather... All departures delayed... ';
console.log(message2.repeat(5));
 
const planesInline = function (n) {
  console.log(`There are ${n} playes in line ${'✈️'.repeat(n)}`);
};
planesInline(5);
planesInline(3);
planesInline(12);
```

