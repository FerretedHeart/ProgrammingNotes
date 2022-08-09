# React Components

### Base Class and render() Method

React class components need to inherit from the `React.Component` base class.

React class components must have a `render()` method. This method should return some elements created with JSX.

```react
class MyComponent extends React.Component {
  render() {
    return <h1>Hello from the render method!</h1>;
  }
}
```



### Components

A React component is a reusable piece of code used to define the appearance, behavior, and state of a portion of a web app's interface. Components are defined as functions or as classes. React requires that the first letter of components be capitalized. If the first letter is capitalized, JSX knows it's a component, if not then it's an HTML element.

```react
import React from 'react';
import ReactDOM from 'react-dom';

function MyFunctionComponent() {
  return <h1>Hello from a function component!</h1>;
}

class MyClassComponent extends React.Component {
  render() {
    return <h1>Hello from a class component!</h1>;
  }
}

ReactDOM.render(
  <MyComponent />, // component instance
  document.getElementById('app')
);
```



### Code in render()

A React component can contain JavaScript before any JSX is returned. The JS before the `return` statement informs any logic necessary to render the component.

```react
class Integer extends React.Component {
  render() {
    const value = 3.14;
    const asInteger = Math.round(value);
    return <p>{asInteger}</p>;
  }
}
```

The method can return any JSX, including a mix of HTML elements and custom React components.

```react
class Header extends React.Component {
  render() {
    <div>
    	<Logo /> // component defined elsewhere
      <h1>Codecademy</h1>
    </div>
  }
}
```



### Object Properties as Attribute Values

JSX attribute values can be set through data stored in objects.

```react
const seaAnemones = {
  src: 'https://commons.wikipedia.org/wiki/Category:Images#/media/File:Anemones_0429.jpg',
  alt: 'Sea Anemones',
  width: '300px'
};

class SeaAnemones extends React.Component {
  render() {
    return (
      <div>
      	<h1>Colorful Sea Anemones</h1>
        <img
          src={seaAnemones.src}
          alt={seaAnemones.alt}
          width={seaAnemones.width}
          />
      </div>
    );
  }
}
```



### this.props

Class components can access its properties with the `this.props` object.

```react
class Hello extends React.Component {
  render() {
    return <h1>Hi there, {this.props.firstName}!</h1>;
  }
}

ReactDOM.render(
	<hello firstName="Kim" />,
  document.getElementById('app')
);
```



### defaultProps

The `defaultProps` object contains default values to be used in case props are not passed.

```react
class Profile extends React.Component {
  render() {
    return (
    	<div>
      	<img src={this.props.profilePictureSrc} alt="" />
        <h2>{this.props.name}</h2>
      </div>
    );
  }
}

Profile.defaultProps = {
  profilePictureSrc: 'https://example.com/no-profile-picture.jpg'
};

class MyFriends extends React.Component {
  render() {
    return (
    	<div>
      	<h1>My friends</h1>
        <Profile 
          name="Jane Doe" // passing information to other component
          profilePictureSrc="http://example.com/jane-doe.jpg"
         />
        <Profile name="John Smith" />
      </div>
    );
  }
}
```



### this.props.children

Every component's `props` object has a property named `children`. Using `this.props.children` will return everything in between a component's opening and closing JSX tags.

```react
<List> // opening tag
  <li></li> // child 1
  <li></li> // child 2
  <li></li> // child 3
</List> // closing tag
```



### Binding this keyword

It is common to pass event handler functions to elements in the `render()` method. If those methods update the component state, `this` must be bound so that those methods correctly update the overall component state.

```react
class MyName extends React.Component {
  constructor(props) {
    super(props);  // should call to properly set up their this.props object
    this.state = { name: 'Jane Doe' };
    this.changeName = this.changeName.bind(this);
  }
  
  changeName(newName) {
    this.setState({ name: newName });
  }
  
  render() {
    return (
    	<h1>My name is {this.state.name}</h1>
      <NameChanger handleChange={this.changeName} />
    )
  }
}
```



### this.setState()

Components can change their state with `this.setState()`. It should always be used instead of directly modifying the `this.state` object. This takes an object which it merges with the componet's current state. If there are properties in the current state that aren't part of that object, then those properties are unchanged. The state is an object initialized in the `constructor()`. Never `setState()` in the render function because it automatically re-renders, so it will cause an infinite loop.

```react
class Flavor extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      favorite: 'chocolate'
    };
  }
  
  render() {
    return (
    	<button onClick={(event) => {
          event.preventDefault();
          this.setState({ favorite: 'vanilla' });
        }}>
        No, my favorite is vanilla
      </button>
    );
  }
}
```



### Dynamic Data

Components can receive dynamic information from *props*, or set their own dynamic data with *state*. Props are passed down by parent components, whereas state is created and maintained by the component itself.

```react
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { showPassword: false };
  }
  
  render() {
    let text;
    if (this.state.showPassword) {
      text = `The password is ${this.props.password}`;
    } else {
      text = `The password is a secret`;
    }
    
    return (
    	<div>
      	<p>{text}</p>
        <button
          onClick={(event) => {
            event.preventDefault();
            this.setState((oldState) => {
              showPassword: !oldState.showPassword
            })
          }}>
          Toggle Password
        </button>
      </div>
    )
  }
}
```

