# Context

**Prop drilling** is the term for when a piece of data is passed as a prop through a large number of components in a React application. The prop is "drilled" from a high-level component down through middle-level components down to a low-level component.

**Context API** is a common approach to get around unnecessary prop drilling. Context is a feature of React that allows to create a piece of state that any component within an area of your application can subscribe to.

## Creating and Consuming Context

The React Context API uses a **Provider** and **Consumer** pattern to share data throughout an application. The provider role is played by a React component that makes data available to its descendant components. When one of those descendants accesses the shared data, it becomes a consumer.

To use the React Context API, start by creating a React context object, a named object created by the `React.createContext()` function.

```react
const MyContext = React.createContext();
```

Context objects include a `.Provider` property that is a React component. It takes in a `value` prop to be stored in the context.

```react
<MyContext.Provider value="Hello world!">
	<ChildComponent />
</MyContext.Provider>
```

That value - in this case, the string `"Hello world!"` - is available to all its descendent components. Descendent components - in this case, `ChildComponent` - can then retrieve the context's value with React's `useContext()` hook.

```react
import { useContext } from 'react';
import { MyContext } from './MyContext.js'

const ChildComponent = () => {
  const value = useContext(MyContext);
  return <p>{value}</p>
}
// Renders <p>Hello world!</p>
```

The `useContext()` hook accepts the context object as an argument and returns the current value of the context. Prop drilling that value is no longer needed.

> Note: If a component attempts to use a context that isn't provided by one of its ancestors, `useContext()` will return `null`.

> Note: In some older React applications, you might instead see `SomeContext.Consumer` used to subscribe to a Context. That alternative is generally considered bad practice and avoided for being overly verbose and difficult to work with.

## Multiple Providers

A `.Provider` component may be reused with the same context multiple times in an application with different values. This is particularly useful if the context is rendered by a component that is used multiple times throughout the application. Each instance of the component might want to give a different value to the context.

```react
const GreetingContext = React.createContext();

const ChildComponent = () => {
  const greeting = useContext(GreetingContext);
  return <h2>{greeting}</h2>
}

const MyComponent = () => {
  return (
  <>
  	<GreetingContext.Provider value="bonjour le monde!">
    	<ChildComponent />
    </GreetingContext.Provider>
    <GreetingContext.Provider vallue="hallo welt!">
      <ChildComponent />
    </GreetingContext.Provider>
    </>
  );
};
```

## Provider Wrappers

It's common for React applications to create a "wrapper" component around a `.Provider` component. The wrapper component can provide a value of its choosing, often one of its props or a string literal, to the `.Provider` component.

Here is a `ThemeMessage` component that returns the `ThemeContext.Provider` component wrapped around any `children` components it receives. `ThemesMessage` also assigns a value using a `theme` prop:

```react
const ThemeContext = React.createContext();

const ThemedMessage = ({ children, theme }) => {
  return (
  	<ThemeContext.Provider value={theme}>
    	This content is in {theme} mode!
      {children}
    </ThemeContext.Provider>
  );
};
```

Wrapper components like `ThemedMessage` can then be used in place of a `.Provider` component to wrap children:

```react
// Renders:
// This content is in dark mode! Hooray!
const MyComponent = () => {
  return (
  	<ThemedMessage theme="dark">
    	Hooray!
    </ThemedMessage>
  )
};
```

## Updating Context

Many React applications use prop drilling to pass down two values: a piece of state and the state updater function to update that state. Child components may then use the state updater function to change the state of their ancestors.

```react
const CounterApp = () => {
  const [count, setCount] = useState(0);
  
  return (
  	<Counter count={count} setCount={setCount} />
  )
}
```

React Contexts can also be used to provide state and state updater functions. One common pattern is to have the context provide an object containing both of those values. Child components that consume the context can then use both (or either of) the state and the state updater function.

In this example, `CounterArea` provides the `count` value and the `setCount()` function to its descendants using context. The `Counter` component extracts both values from the provided context.

```react
const CounterContext = React.createContext();

const CounterArea = ({ children }) => {
  const [count, setCount] = useState(0);
  
  return (
    <CounterContext.Provider value={{ count, setCount }}>
    	{children}
    </CounterContext.Provider>
  );
};

const Counter = () => {
  const { count, setCount } = useContext(CounterContext);
  
  return (
    <button onClick={() => setCount(count => count+1)}>
    	{count}
    </button>
  );
};

const CounterApp = () => {
  return (
  	<CounterArea>
    	<Counter />
    </CounterArea>
  )
}
```

## Nested Providers

A Context Provider may be a child in the application tree undernearth an earlier Context Provider. Components subscribing to a Context's Provider will receive the value for the `.Provider` component closest to them in the application tree. This pattern is sometimes referred to as "nesting".

```react
<GreeterContext.Provider value="Salut!">
  <HighLevelComponent> {// GreeterContext's value is "Salut!" here}
 
    <GreeterContext.Provider value="Hallo!">  
      <LowLevelComponent /> {// GreeterContext's value is "Hallo!" here}
    </GreeterContext.Provider>
 
  </RootComponent>
</GreeterContext.Provider>
```

In this example, we have two components that are used within the `GreeterContext.Provider`: `HighLevelComponent` and `LowLevelComponent`. Each component will use the value of the nearest `GreeterContext.Provider`. In this case, `HighLevelComponent` will have the value `"Salut!"` when using GreeterContext while the `LowLevelComponent` will have the value `"Hallo!"`.

