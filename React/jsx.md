# JSX

### JSX Syntax

JSX is a syntax extension of JavaScript. It's used to create DOM elements which are then rendered in the React DOM. JSX will be rendered as HTML in the browser. Attributes work the same.

```react
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
	<h1>Render me!</h1>,  // JSX expression or component instance to be compiled and rendered
  document.getElementById('app') // HTML element we want to append it to
);

//As of React 18, you need to create the root, then render the component.
ReactDOM.createRoot(document.getElementById('app'))
  .render(<h1>Render me!</h1>);
//or...
const root = ReactDOM.createRoot(document.getElementById('app'));
root.render(<h1>Render me!</h1>);
```

You can't use the word `class`, instead you have to use `className` as `class` is a reserved word, but will be rendered correctly as the class attribute.

Empty elements must be closed using a closing slash at the end of the their tag: `<tagName />`. Examples include `<br />` and `<img />`.

```react
<br />
<img src="example_url" />
```

The `key` attribute is used to uniquely identify individual elements. It is delcared like other attributes. They can help performance because they allow React to keep track of whether individual list items should be rendered, or if the order of individual items is important.

```react
<ul>
	<li key="key1">One</li>
  <li key="key2">Two</li>
  <li key="key3">Three</li>
  <li key="key4">Four</li>
</ul>
```



### Nested JSX Elements

In order for the code to compile, a JSX expression must have exactly one outermost element.

```react
const myClasses = {
  <a href="https://www.codecademy.com">
  	<h1>
  		Sign up!
  	</h1>
  </a>
};
```



### Multiline JSX Expression

A JSX expression that spans multiple lines must be wrapped in parenthesis.

```react
const myList = (
	<ul>
  	<li>item 1</li>
  	<li>item 2</li>
  	<li>item 3</li>
  </ul>
);
```



### Embedding JavaScript in JSX

JavaScript expressions may be embedded within JSX expressions but must be wrapped in curly braces. Any text between JSX tags will be read as text content not JavaScript.

```react
let expr = <h1>{10 * 10}</h1>;
// will be rendered as <h1>100</h1>
```



### Conditionals

In JSX, `&&` is used to render an element based on a boolean condition. Works best in conditionals that will sometimes do an action but other times do nothing at all.

```react
const tasty = (
	<ul>
  	<li>Applesauce</li>
    { !baby && <li>Pizza</li> }
    { age > 15 && <li>Brussels Sprouts</li> }
    { age > 20 && <li>Oysters</li> }
    { age > 25 && <li>Grappa</li> }
  </ul>
);
```

JSX does not support `if/else` in embedded syntax. Use instead:

1. a ternary within curly braces

2. an `if` statement outside of the JSX element

3. the && operator

   ```react
   // Using ternary operator
   const headline = (
     <h1>
       { age >= drinkingAge ? 'Buy Drink' : 'Do Teen Stuff' }
     </h1>
   );
    
   // Using if/else outside of JSX 
   let text;
    
   if (age >= drinkingAge) { text = 'Buy Drink' }
   else { text = 'Do Teen Stuff' }
    
   const headline = <h1>{ text }</h1>
    
   // Using && operator. Renders as empty div if length is 0
   const unreadMessages = [ 'hello?', 'remember me!'];
    
   const update = (
     <div>
       {unreadMessages.length > 0 &&
         <h1>
           You have {unreadMessages.length} unread messages.
         </h1>
       }
     </div>
   );
   ```

   

### Event Listeners

Event listeners are specified as attributes on elements. An event listener attribute's *name* should be written in camelCase, such as `onClick` for an `onclick` event and `OnMouseOver` for an `onmouseover` event. The *value* should be a function.

```react
// Basic example
const handleClick = () => alert("Hello world!");
 
const button = <button onClick={handleClick}>Click here</button>;
 
// Example with event parameter
const handleMouseOver = (event) => event.target.style.color = 'purple';
 
const button2 = <div onMouseOver={handleMouseOver}>Drag here to change color</div>;
```



### JSX .map() method

The array method `map()` is used often to create a list of elements from a given array.

```react
const strings = ['Home', 'Shop', 'About Me'];

const listItems = strings.map(string => <li>{string}</li>);

<ul>{listItems}</ul>

---------------
  
const names = ["alice", "bob", "charlie", "danielle"]
// -->        ["Alice", "Bob", "Charlie", "Danielle"]

const capitalized = names.map((name) => {
    return name[0].toUpperCase() + name.slice(1)
})
```

