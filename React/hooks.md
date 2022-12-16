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
2. Only call Hooks at the top level (parent), to be sure that Hooks are called in the same order each time a component renders.

Common mistakes to avoid are calling Hooks inside of loops, conditions, or nested functions.



# The State Hook

The State Hook is a named export from the React library.

```react
import React, { useState } from 'react';

const [currentState, stateSetter] = useState(initialState);
```

`useState()` is a JavaScript function defined in the React library. When we call this function, it returns an array with two values:

- *current state* - the current value of this state
- *state setter* - a function that we can use to separate the value of this state

Because React returns two values in an array, we assign them to local variables by destructuring.

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

## Initialize State

To initialize our state with any value we want, we pass the initial value as an argument to the `useState()` function call.

```react
const [isLoading, setIsLoading] = useState(true);
```

1. During the first render, the *initial state argument* is used.
2. When the state setter is called, React ignores the initial state argument and uses the new value.
3. When the component re-renders for any other reason, React continues to use the same value from the previous render.
4. If there is a need to have a complicated computation done for the initial value, put it in a function so that it is only called once.

## Use State Setter Outside of JSX

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

## Set From Previous State

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

## Arrays in State

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

## Objects in State

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



# The Effect Hook

[A Complete Guide to useEffect](https://overreacted.io/a-complete-guide-to-useeffect/)

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

`useEffect()` accepts two arguments:

1. A callback function which is executed after the component renders
2. A dependency array which determines how often the effect is executed

## Clean Up Effects

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

## Control When Effects are Called

It is common, when defining function components, to run an effect only when the component mounts (renders the first time), but not when the component re-renders. If we want to only call our effect after the first render, we pass an empty array to `useEffect()` as the second argument. This second argument is called the *dependency array*.

The dependency array is used to tell the `useEffect()` method when to call our effect and when to skip it. Our effect is always called after the first render but only called again if something in our dependency array has changed values between renders.

```react
useEffect(() => {
  alert('component rendered for the first time');
  return () => {
    alert('component is being removed from the DOM');
  };
}, []); // dependency array
```

Without passing an empty array as the second argument to the `useEffect()`, those alerts would be displayed before and after every render of our component, which is clearly not when those messages are meant to be displayed. Simply passing `[]` to the `useEffect()` function is enough to configure when the effect and cleanup functions are called.

## Fetch Data

When the data that our components need to render doesn't change, we can pass an empty dependency array, so that the data is fetched after the first render. When the response is received from the server, we can use a state setter from the State Hook to store the data from the server's response in our local component state for future renders.

An empty dependency array signals to the Effect Hook that our effect never needs to be re-run, that it doesn't depend on anything. Specifying zero dependencies means that the result of running that effect won't change and calling our effect once is enough.

A dependency array that is not empty signals to the Effect Hook that it can skip calling our effect after re-renders unless the value of one of the variables in our dependency array has changed. If the value of a dependency has changed, then the Effect Hook will call our effect again.

```react
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // Only re-run the effect if the value stored by count changes
```

| Dependency Array | Effect called after first render               |
| ---------------- | ---------------------------------------------- |
| undefined        | every re-render                                |
| Empty array      | no re-renders                                  |
| Non-empty array  | when any value in the dependency array changes |



# The Ref Hook

Refs in React are incredibly useful for accessing and manipulating DOM elements directly. Refs are also amazing at persisting data between renders which makes it possible to store persisted component data without causing a re-render when it is changed.

In order to work with refs in React you need to first initialize a ref which is what the `useRef` hook is for. This hook is very straightforward, and takes an initial value as the only argument.

```react
useRef(initialValue)
```

This hook then returns a ref for you to work with.

```react
const myRef = useRef(null)
```

In the above example we have created a ref called `myRef` and set its default value to `null`. This means that `myRef` is now equal to an object that looks like this.

```react
{ current: null }
```

This is because a ref is always an object with a single `.current` property which is set to the current value of the ref. If we were to instead create a ref with a default value of `0` it would look like this.

```react
const myRef = useRef(0)
console.log(myRef)
// { current: 0 }
```

Now this seems like a lot of work in order to save a single value, but what makes refs so powerful is the fact that they are persisted between renders. I like to think of refs very similarly to state, since they persist between renders, but refs do not cause a component to re-render when changed.

Imagine that we want to count the number of times a component re-renders. Here is the code to do so with state and refs.

```react
function State() {
  const [rerenderCount, setRerenderCount] = useState(0);

  useEffect(() => {
    setRerenderCount(prevCount => prevCount + 1);
  });

  return <div>{rerenderCount}</div>;
}
function Ref() {
  const rerenderCount = useRef(0);

  useEffect(() => {
    rerenderCount.current = rerenderCount.current + 1;
  });

  return <div>{rerenderCount.current}</div>;
}
```

Both of these components will correctly display the number of times a component has been re-rendered, but in the state example the component will infinitely re-render itself since setting the state causes the component to re-render. The ref example on the other hand will only render once since setting the value of a ref does not cause any re-renders.

## How To Use Refs

Now that we understand refs are just an object for storing a value that persists between renders, let's talk about when you would need to use a ref.

The most common use case for refs in React is to reference a DOM element. Because of how common this use case is every DOM element has a ref property you can use for setting a ref to that element. For example, if you wanted to focus an input element whenever a button was clicked you could use a ref to do that.

```jsx
function Component() {
  const inputRef = useRef(null)

  const focusInput = () => {
    inputRef.current.focus()
  }

  return (
    <>
      <input ref={inputRef} />
      <button onClick={focusInput}>Focus Input</button>
    </>
  )
}
```

As you can see in the code above we use the `ref` property on the input element to set the current value of `inputRef` to the input element. Now when we click the button it will call `focusInput` which uses the current value of the `inputRef` variable to set the focus on the input element.

Being able to access any DOM element directly with a ref is really useful for doing things like setting focus or managing other attributes that you cannot directly control in React, but it can be easy to abuse this power. I often see newer React developers using refs to dynamically add and remove elements (`appendChild`, `removeChild`, etc.) in a component instead of having React do that for you. This leads to inconsistencies between the actual DOM and the React virtual DOM which is very bad.

## Using Refs Beyond The DOM

While most use cases for refs lie with referencing DOM elements, refs can also be used for any form of storage that is persisted across component renders. A very common use case for this would be storing the previous value of a state variable.

```jsx
function Component() {
  const [name, setName] = useState('Kyle')
  const previousName = useRef(null)

  useEffect(() => {
    previousName.current = name
  }, [name])

  return (
    <>
      <input value={name} onChange={e => setName(e.target.value)} />
      <div>{previousName.current} => {name}</div>
    </>
  )
}
```

The above code will update the `previousName` ref every time the name changes so that it always has the previous value of the name variable stored in it.

# The Memo Hook

The most basic form of memoization in React is the `useMemo` hook. The syntax for this hook is actually the exact same as `useEffect` since they both work in a similar way. The first argument of `useMemo` is a function that does the complex calculation you want to memoize, and the second argument is an array of all dependencies for that memoization.

```js
const result = useMemo(() => {
  return slowFunction(a)
}, [a])
```

As you can see in the above example, we want to memoize `slowFunction` which depends on `a`. To do this, all we did was wrap the `slowFunction` in our `useMemo` function and used the argument `a` in the array of dependencies. This code essentially does the exact same thing as our previous code for memoization, since as long as `a` stays the same the `slowFunction` will not be re-run and instead the cached value will be used. This is the most common way `useMemo` is used, but there is a second common use case which is referential equality.

### Referential Equality

If you are unfamiliar with referential equality it essentially defines whether or not the references of two values are the same. For example `{} === {}` is false because it is checking referential equality. While both of the objects are empty, they reference different places in memory where the object is stored. Because of this, they are not referentially equal and this comparison returns false. *If you are interested in learning more about reference vs value comparisons checkout [this video](https://youtu.be/-hBJz2PPIVE).*

This referential equality is important when it comes to dependency arrays, for example in `useEffect`.

```js
function Component({ param1, param2 }) {
  const params = { param1, param2, param3: 5 }

  useEffect(() => {
    callApi(params)
  }, [params])
}
```

At first glance it may seem this `useEffect` works properly, but since the `params` object is created as a new object each render this is actually going to cause the effect to run every render since the reference of `params` changes each render. `useMemo` can fix this, though.

```js
function Component({ param1, param2 }) {
  const params = useMemo(() => {
    return { param1, param2, param3: 5 }
  }, [param1, param2])

  useEffect(() => {
    callApi(params)
  }, [params])
}
```

Now if `param1` and `param2` do not change the `params` variable will be set to the cached version of `params` which means the reference for `params` will only change if `param1`, or `param2` change. This referential equality is really useful when comparing objects in dependency arrays, but if you need to use a function in a dependency array you can use the `useCallback` hook.

# The Callback Hook

`useCallback` works nearly identically to `useMemo` since it will cache a result based on an array of dependencies, but `useCallback` is used specifically for caching functions instead of caching values.

```js
const handleReset = useCallback(() => {
  return doSomething(a, b)
}, [a, b])
```

This syntax may look exactly the same as `useMemo`, but the main difference is that `useMemo` will call the function passed to it whenever its dependencies change and will return the value of that function call. `useCallback` on the other hand will not call the function passed to it and instead will return a new version of the function passed to it whenever the dependencies change. This means that as long as the dependencies do not change then `useCallback` will return the same function as before which maintains referential equality.

In order to further understand the differences between `useCallback` and `useMemo` here is a quick example where both will return the same value.

```js
useCallback(() => {
  return a + b
}, [a, b])

useMemo(() => {
  return () => a + b
}, [a, b])
```

As you can see `useCallback` will return the function passed to it, while `useMemo` is returning the result of the function passed to it.

### Referential Equality

Just like with `useMemo`, `useCallback` is used to maintain referential equality.

```jsx
function Parent() {
  const [items, setItems] = useState([])
  const handleLoad = (res) => setItems(res)

  return <Child onLoad={handleLoad} />
}

function Child({ onLoad }) {
  useEffect(() => {
    callApi(onLoad)
  }, [onLoad])
}
```

In the above example the `handleLoad` function is re-created every time the `Parent` component is rendered. This means that the `Child` component's `useEffect` will re-run ever render since the `onLoad` function has a different referential equality each render. To fix this we need to wrap the `handleLoad` in a `useCallback`.

```jsx
function Parent() {
  const [items, setItems] = useState([])
  const handleLoad = useCallback((res) => setItems(res), [])

  return <Child onLoad={handleLoad} />
}

function Child({ onLoad }) {
  useEffect(() => {
    callApi(onLoad)
  }, [onLoad])
}
```

Now the `handleLoad` function will never change, thus the `useEffect` in the `Child` component will not be called on each re-render.

# The Context Hook

To use context in a function component, you no longer need to wrap your JSX in a consumer. All you need to do is pass your context to the `useContext` hook and it will do the rest.

```react
function GrandChildComponent() {
  const {theme, setTheme} = useContext(ThemeContext)
  
  return (
  	<>
    	<div>The theme is {theme}</div>
    	<button onClick={() => setTheme('light')}>Change to Light Theme</button>
  )
}
```

We were able to cut out all of the consumer portion of the context and remove all of the complex nesting. Now context works like a normal function where you call the context and it will give you the values inside of it for you to use later in the code. This drastically simplifies code related to context and makes working with context much easier.

Setting up a context provider for use with the `useContext` hook is exactly the same as you would do for a normal context consumer, so you can use all the same code for the context provider portion of the class component.

# Custom Hooks

[Custom hooks](https://reactjs.org/docs/hooks-custom.html) are JavaScript functions that use other hooks. They help share stateful logic that we may otherwise need to reuse throughout our application.

As a convention, custom hooks should have names that start with `use`. Additionally, they should also follow the rules of hooks.

```react
// useToggle.js
export const useToggle = (initialState = false) => {
  // Use the `initialState` argument to initialize the state
  const [state, setState] = useState(initialState);
 
  // Perform an animation each time the state changes
  useEffect(() => {
    performToggleAnimation(state);
  }, [state])
 
  // Create an easy-to-use toggle function
  const toggle = () => { setState(state => !state) }
 
  // Return the state value and the toggle function
  return [state, toggle]
}
```

In this example, we create a custom hook called `useToggle()` that:

- uses `useState()` to manage a toggle state value
- uses `useEffect` to run a toggling animmation effect
- creates a `toggle()` function to interact with the `setState()` function

Now we can just import and use `useToggle()` in other areas of the application.

```react
import { useToggle } from './useToggle';
const DarkMode = () => {
  // Get the state and toggle function from useToggle()
  // We'll use an initial value of true
  const [state, toggle] = useToggle(true);
  return (
    <button onClick={toggle}> 
      {state ? 'On' : 'Off'}
    </button>
  )
}
```

Custom hooks present a number of advantages when used properly in an application:

- They allow us to abstract our code, hide complex logic, as well as share stateful logic between multiple components.
- When using a custom hook in two or more components, all state and effects inside are fully isolated. This is true of `useState()` and `useEffect()` as well. This means that any data from one component won't "bleed over" into another.
- By creating a separate file from which we export the custom hook, we can import it into any part of our application.

