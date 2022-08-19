# Stateless Functional Components

A React component is a function that returns React elements.

We can define components as JS functions - *function components* rather than class components. They are more concise than class components.

Function  names, like classes, need to be written in Pascal case which is similar to camelCase except that the first letter is capilized as well.

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



## Destructuring props in functions

If you destructure the props as an object parameter in the function, you can then use the assigned variable rather than the object.key method.

```react
export default function Contact({img, name, phone, email}) {
    return (
        <div className="contact-card">
            <img src={img}/>
            <h3>{name}</h3>
            <div className="info-group">
                <img src="./images/phone-icon.png" />
                <p>{phone}</p>
            </div>
            <div className="info-group">
                <img src="./images/mail-icon.png" />
                <p>{email}</p>
            </div>
        </div>
    )
}
```

