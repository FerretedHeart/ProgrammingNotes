# Hooks

Hooks are functions that let us manage the internal state of components and handle post-rendering side effects directly from our function components. Hooks don't work inside classes.

Built-in hooks:

- `useState()`
- `useEffect()`
- `useReducer()`
- `useRef()`
- [Full List](https://reactjs.org/docs/hooks-reference.html)



## Rules for Using Hooks

There are two main rules:

1. Only call Hooks from React function components.
2. Only call Hooks at the top level, to be sure that Hooks are called in the same order each time a component renders.

Common mistakes to avoid are calling Hooks inside of loops, conditions, or nested functions.



## The State Hook

The State Hook is a named export from the React library.

```react
import React, { useState } from 'react';

const [currentState, stateSetter] = useState(initialState);
```

`useState()` is a JavaScript function defined in the React library. When we call this function, it returns an array with two values:

- *current state* - the current value of this state
- *state setter* - a function that we can use to separate the value of this state

Because React returns two values in an array, we assign them to local variables.

```react
import React, { useState } from 'react';

function Toggle() {
	const [toggle, setToggle] =  useState();
  
  return (
    <div>
    	<p>The toggle is {toggle}</p>
      <button onClick={() => setToggle("On")}>On</button>
      <button onClick={() => setToggle("Off")}>Off</button>
    </div>
  );
}

```

Calling the state setter signals to React that the component needs to re-render, so the whole function defining the component is called again. The magic of `useState()` is that is allows React to keep track of the current value of state from one render to the next.



### Initialize State

To initialize our state with any value we want, we pass the initial value as an argument to the `useState()` function call.

```react
const [isLoading, setIsLoading] = useState(true);
```

1. During the first render, the *initial state argument* is used.
2. When the state setter is called, React ignores the initial state argument and uses the new value.
3. When the component re-renders for any other reason, React continues to use the same value from the previous render.



### Use State Setter Outside of JSX

Changing value of a string as a user types into a text input field:

```react
import React, { useState } from 'react';

export default function EmailTextInput() {
  const [email, setEmail] = useState('');
  const handleChange = (event) => {
    const updatedEmail = event.target.value;
    setEmail(updatedEmail);
  }

	return (
  	<input value={email} onChange={handleChange} />
  );
}

// Simplify handleChange
const handleChange = (event) => setEmail(event.target.value);

// Object destructuring
const handleChange = ({target}) => setEmail(target.value);
```



### Set From Previous State

Often the next value of our state is calculated using the current state. In this case, it is best practice to update state with a callback function. If we do not, we risk capturing outdated, or "stale", state values.

```react
import React, {useState} from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);
  
  const increment = () => setCount(prevCount => prevCount + 1); // pass a callback function instead of a value
  
  return (
  	<div>
    	<p>Wow, you've clicked that button: {count} times</p>
      <button onClick={increment}>Click here!</button>
    </div>
  );
}
```



### Arrays in State

When updating an array in state, we do not just add new data to the previous array. We replace the previous array with a brand new array. This means that any information that we want to save from the previous array needs to be explicitly copied over to our new array.

```react
import React, { useState } from "react";
import ItemList from "./ItemList";
import { produce, pantryItems } from "./storeItems";

export default function GroceryCart() {
  // declare and initialize state 
  const [cart, setCart] = useState([]);
  const addItem = (item) => {
    setCart((prev) => {
      return [item, ...prev];
    });
   };

  const removeItem = (targetIndex) => {
    setCart((prev) => {
      return prev.filter((item, index) => index !== targetIndex)
    });
  };

  return (
    <div>
      <h1>Grocery Cart</h1>
      <ul>
        {cart.map((item, index) => (
          <li onClick={() => removeItem(index)} key={index}>
            {item}
          </li>
        ))}
      </ul>
      <h2>Produce</h2>
      <ItemList items={produce} onItemClick={addItem} />
      <h2>Pantry Items</h2>
      <ItemList items={pantryItems} onItemClick={addItem} />
    </div>
  );
}

```



### Objects in State

When working with a set of related variables, it can be very helpful to group them in an object.

```react
export default function Login() {
  const [formState, setFormState] = useState({});
 
  const handleChange = ({ target }) => {
    const { name, value } = target;
    setFormState((prev) => ({
      ...prev,
      [name]: value
    }));
  };
 
  return (
    <form>
      <input
        value={formState.firstName}
        onChange={handleChange}
        name="firstName"
        type="text"
      />
      <input
        value={formState.password}
        onChange={handleChange}
        type="password"
        name="password"
      />
    </form>
  );
}
```

- Use a state setter callback function to update state based on the previous value
- The spread syntax is the same for objects as for arrays: `{...oldObject, newKey: newValue}`
- We reuse our event handler across multiple inputs by using the input tag's `name` attribute to identify which input the change event came from

When updating the state with `setFormState()` inside a function component, we do not modify the same object. We must ocpy over the values from the previous object when setting the next value of state.



## The Effect Hook

The Effect Hook runs some JavaScript code after each render, such as:

- fetching data from a backend service
- subscribing to a stream of data
- managing timers and intervals
- reading from and making changes to the DOM

Key moments when the Effect Hook can be utilized:

1. When the component is first added, or *mounted*, to the DOM and renders
2. When the state or props change, causing the component to re-render
3. When the component is removed, or *unmounted*, from the DOM

```react
import React, { useState, useEffect } from 'react';
 
function PageTitle() {
  const [name, setName] = useState('');
 
  useEffect(() => {
    document.title = `Hi, ${name}`;
  });
 
  return (
    <div>
      <p>Use the input field below to rename this page!</p>
      <input onChange={({target}) => setName(target.value)} value={name} type='text' />
    </div>
  );
}
```



### Clean Up Effects

Some effects require cleanup. When we add event listeners to the DOM, it is important to remove those event listeners when we are done with them to avoid memory leaks.

```react
useEffect(() => {
  document.addEventListener('keydown', handleKeyPress);
  return () => {
    document.removeEventListener('keydown', handleKeyPress);
  };
})
```

Because effects run after ever render and not just once, React calls our cleanup function before each re-render and before unmounting to clean up each effect call.

If our effect returns a function, then the `useEffect()` Hook always treats that as a cleanup function. React will call this cleanup function before the component re-renders or unmount. Since this cleanup function is optional, it is our responsibility to return a cleanup function from our effect when our effect code could create memory leaks.



### Control When Effects are Called

It is common, when defining function components, to run an effect only when the component mounts (renders the first time), but not when the component re-renders. If we want to only call our effect after the first render, we pass an empty array to `useEffect()` as the second argument. This second argument is called the *dependency array*.

The dependency array is used to tell the `useEffect()` method when to call our effect and when to skip it. Our effect is always called after the first render but only called again if something in our dependency array has changed values between renders.

```react
useEffect(() => {
  alert('component rendered for the first time');
  return () => {
    alert('component is being removed from the DOM');
  };
}, []);
```

Without passing an empty array as the second argument to the `useEffect()`, those alerts would be displayed before and after every render of our component, which is clearly not when those messages are meant to be displayed. Simply passing `[]` to the `useEffect()` function is enough to configure when the effect and cleanup functions are called.



### Fetch Data

When the data that our components need to render doesn't change, we can pass an empty dependency array, so that the data is fetched after the first render. When the response is received from the server, we can use a state setter from the State Hook to store the data from the server's response in our local component state for future renders.

An empty dependency array signals to the Effect Hook that our effect never needs to be re-run, that it doesn't depend on anything. Specifying zero dependencies means that the result of running that effect won't change and calling our effect once is enough.

A dependency array that is not empty signals to the Effect Hook that it can skip calling our effect after re-renders unless the value of one of the variables in our dependency array has changed. If the value of a dependency has changed, then the Effect Hook will call our effect again.

```react
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // Only re-run the effect if the value stored by count changes
```

