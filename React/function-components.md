# Stateless Functional Components

We can define components as JS functions - *function components* rather than class components. They are more concise than class components.

```react
// A component class written in the usual way:
export class MyComponentClass extends React.Component {
  render() {
    return <h1>Hello world</h1>;
  }
}

// The same component class, written as a stateless functional component:
export const MyComponentClass = () => {
  return <h1>Hello world</h1>;
}

// Works the same either way:
ReactDOM.render(
	<MyComponentClass />,
	document.getElementById('app')
);
```



## Components and Props

Give the function component a parameter named `props`. Within the function body, you can access the props  using this pattern: `props.propertyName` . You don't need to use the `this` keyword.

```react
export function YesNoQuestion(props) {
	return (
  	<div>
    	<p>{props.prompt}</p>
      <input value="Yes" />
      <input value="No" />
    </div>
  );
}

ReactDOM.render(
	<YesNoQuestion prompt="Have you eaten an apple today?" />,
  document.getElementById('app');
);
```

