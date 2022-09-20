# Redux

[Redux](https://redux.js.org/) is a state management library that follows a pattern known as the [Flux architecture](https://facebook.github.io/flux/docs/in-depth-overview/). In the Flux pattern, and in Redux, shared information is not stored in components but in a single object. Components are just given data to render and can request changes using events called *actions*. The state is available throughout the application and updates are made in a predictable manner: components are "notified" whenever a change is made to the state.

[Core Concepts](https://redux.js.org/introduction/core-concepts)

## State

*State* is the current information behind a web application.

With Redux, state can be any JavaScript type, including: number, string, boolean, array, and object.

```react
// Example state for a todo app
const state = [ 'Print trail map', 'Pack snacks', 'Summit the mountain' ];
```

## Actions

In Redux, actions are represented as plain JS objects.

```react
const action = {
  type: 'todos/addTodo',
  payload: 'Take selfies'
};
```

- Every action must have a `type` property with a string value. This describes the action.
- Typically, an action has a `payload` property with an object value. This includes any information related to the action. In this case, the payload is the todo text.
- When an action is generated and notifies other parts of the application, we say that the action is *dispatched*.

```react
// 'Remove all todos'. This requires no payload because no additional information is needed.
const action = {
  type: 'todos/removeAll'
}

// 'Remove the 'Pack snacks' todo'
const action = {
  type: 'todos/removeTodo',
  payload: 'Pack snacks'
}
```

## Reducers

A *reducer*, or reducer function, is a plain JavaScript function that defines how the current state and an action are used in combination to create the new state.

```react
const initialState = ['Print trail map', 'Pack snacks', 'Summit the mountain'];

const todoReducer = (state = initialState, action) => {
  switch(action.type) {
    case 'todos/addTodo': {
      return [...state, action.payload];
    }
    case 'todos/removeAll': {
      return [];
    }
    default: {
      return state;
    }
  }
}
```

There are a few things about this reducer that are true for all reducers:

- It's a plain JavaScript function
- It defines the application's next state given a current state and a specific action
- It returns a default initial state if no action is provided
- It returns the current state if the action is not recognized

## Rules of Reducers

We wrote reducers that returned a new copy of the state rather than editing it directly. We did this to adhere to the [rules of reducers provided by the Redux documentation](https://redux.js.org/tutorials/fundamentals/part-3-state-actions-reducers#rules-of-reducers);

1. They should only calculate the new state value based on the `state` and `action` arguments.
2. They are not allowed to modify the existing state. Instead, they must copy the existing state and make changes to the copied values.
3. They must not do any asynchronous logic or have other "side effects".

By asynchronous logic or "side effects", we mean anything that the function does aside from returning a value, e.g. logging to the console, saving a file, setting a timer, making an HTTP request, generating random numbers.

## Immutable Updates and Pure Functions

Reducers must make *immutable updates* and be *pure functions*.

If a function makes *immutable updates* to its arguments, it does not change the argument but instead makes a copy and changes that copy. It's called updating *immutably* because the function doesn't change, or *mutate*, the arguments.

```react
// This function mutates its argument:
const mutableUpdater = (obj) => {
  obj.completed = !obj.completed;
  return obj;
}

const task = { text: 'do dishes', completed: false };
const updatedTask = mutableUpdater(task);
console.log(updatedTask); // Prints { text: 'do dishes', completed: true }

console.log(task); // Prints { text: 'do dishes', completed true }

----------------------

//This function 'immutably updates' its argument:
const immutableUpdater = (obj) => {
  return {
    ...obj,
    completed: !obj.completed
  }
}

const task = { text: 'iron clothes', completed: false };
const updatedTask = immutableUpdater(task);
console.log(updatedTask); // Prints { text: 'iron clothes', completed: true }

console.log(task); // Prints { text: 'iron clothes', completed: false }
```

If a function is *pure*, then it will always have the same outputs given the same inputs.

This is a combination of rules 1 and 3:

- Reducers should only calculate the new state value based on the state and action arguments.
- Reducers must not do any asynchronous logic or other 'side effects'.

```react
// Function is not a pure function because its returned value depends on the status of a remote endpoint
const addItemToList = (list) => {
  let item;
  fetch('https://anything.com/endpoint')
  	.then(response => {
    	if(!response.ok) {
        item = {};
      }
    
    	item = response.json();
  });
  
  return [...list, item];
};

// Function can be made pure by pulling the fetch() statement outside of the function
let item;
	fetch('https://anything.com/endpoint')
		.then(response => {
    	if(!response.ok) {
        item = {};
      }
    
    	item = response.json();
  });

const addItemToList = (list, item) => {
  return [...list, item];
};
```

## Store

Redux uses a special object called the *store*. The store acts as a container for state, it provides a way to dispatch actions, and it calls the reducer when actions are dispatched. In nearly every Redux application, there will only be one store.

1. The store initializes the state with a default value.
2. The view displays that state.
3. When a user interacts with the view, like clicking a button, an action is dispatched to the store.
4. The dispatched action and the current state are combined in the store's reducer to determine the next state.
5. The view is updated to display the new state.

# Redux API

Installing the Redux package: `npm install redux`

Import the createStore method: `import { createStore } from 'react'`

## Create a Redux Store

Redux exports a valuable helper function for creating this `store` object called `createStore()`. The `createStore()` helper function has a single argument, a reducer function.

```react
import { createStore } from 'redux'
 
const initialState = 'on';
const lightSwitchReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'toggle':
      return state === 'on' ? 'off' : 'on';
    default:
      return state;
  }
}
 
const store = createStore(lightSwitchReducer);
```

## Dispatch Actions to the Store

The `store` object returned by `createStore()` provides a number of useful methods for interacting with its state as well as the reducer function it was created with.

The most commonly used method, `store.dispatch()`, can be used to dispatch an action to the store, indicating that you wish to update the state. Its only argument is an action object, which must have a `type` property describing the desired state change.

```react
const action = { type: 'actionDescriptor' };
store.dispatch(action);
```

Each time `store.dispatch()` is called with an `action` object, the store's reducer function will be executed with the same `action` object. Assumign the the `action.type` is recognized by the reducer, the state will be updated and returned.

```react
// Using the lightswitch application from earlier
console.log(store.getState()); // Prints 'on'
 
store.dispatch({ type: 'toggle' }); 
console.log(store.getState()); // Prints 'off'
 
store.dispatch({ type: 'toggle' });
console.log(store.getState()); // Prints 'on'
```

## Action Creators

In most Redux applications, *action creators* are used to reduce repetition and to provide consistency. An action creator is simply a function that returns an action object with a `type` property. They are typically called and passed directly to the `store.dispatch()` method resulting in fewer errors and an easier-to-read dispatch statement.

```react
const toggle = () => {
  return {type: 'toggle'};
}

store.dispatch(toggle()); // Toggles the light to 'off'
store.dispatch(toggle()); // Toggles the light back to 'on'
store.dispatch(toggle()); // Toggles the light back to 'off'
```

## Respond to State Changes

In a typical web application, user interactions that trigger DOM events can be listened for and responded to using an event listener.

In Redux, actions dispatched to the `store` can be listened for and responded to using the `store.subscribe()` method. This method accepts one argument: a function, often called a *listener*, that is executed in response to chanegs to the `store`'s state.

```react
const reactToChange = () => console.log('change detected!');
store.subscribe(reactToChange);
```

Sometimes it is useful to stop the listener from responding to changes to the `store`, so `store.subscribe()` returns an `unsubscribe` function.

```react
// lightSwitchReducer(), toggle(), and store omitted...
 
const reactToChange = () => {
  console.log(`The light was switched ${store.getState()}!`);
}
const unsubscribe = store.subscribe(reactToChange);
 
store.dispatch(toggle());
// reactToChange() is called, printing:
// 'The light was switched off!'
 
store.dispatch(toggle());
// reactToChange() is called, printing:
// 'The light was switched on!'
 
unsubscribe(); 
// reactToChange() is now unsubscribed
 
store.dispatch(toggle());
// no print statement!
 
console.log(store.getState()); // Prints 'off'
```

- In this example, the listener function `reactToChange()` is subscribed to the `store`
- Each time an action is dispatched, `reactToChange()` is called and prints the current value of the light switch. It is common for callbacks subscribed to the `store` to use `store.getState()` inside them.
- After the first two dispatched actions, `unsubscribe()` is called causing `reactToChange()` to no longer be executed in response to further dispatches made to `store`.

## Connect the Redux Store to a UI

Connecting a Redux store with any UI requires a few consistent steps, regardless of how the UI is implemented:

- Create a Redux store
- Render the initial state of the application
- Subscribe to updates. Inside the subscription callback:
  - Get the current store state
  - Select the data needed by tis piece of UI
  - Update the UI with the data
- Respond to UI events by dispatching Redux actions

## React and Redux

Common steps involved in connecting Redux to a React UI:

- A `render()` function will be subscribed to the `store` to re-render the top-level React Component.
- The top-level React component will receive the current value of `store.getState()` as a `prop` and use that data to render the UI.
- Event listeners attached to React components will dispatch actions to the `store`.
