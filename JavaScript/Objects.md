```javascript
let objectName = {
  property: value,
  property: value
};
```

When calling objects, you can either use <u>dot</u> notation or <u>bracket</u> notation
<u>Brackets</u> allow you to use expressions, such as the variable holding the property name
<u>Dots</u> are used most.

Functions are values, a method is also a property, so functions can be added to an object.

```javascript
const jeanine = {
    firstName: 'Jeanine',
    lastName: 'Kimball',
    birthYear: 1971,
    job: 'programmer',
    friends: ['Miguel', 'Greg', 'Keith'],
    hasDriversLicense: true,
    calcAge:function() {
        return 2021 - this.birthYear;
    }
};
```

'This' is the object that is calling it.