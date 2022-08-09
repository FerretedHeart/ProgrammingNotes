# Lifecycle Methods

### Lifecycle Phases

There are three categories of lifecycle methods:

1. Mounting - when the component is being initialized and put into the DOM for the first time
   1. `constructor()`
   2. `render()`
   3. `componentDidMount()`
2. Updating -when the component updates as a result of changed state or changed props
   1. `render()`
   2. `componentDidUpdate()`
3. Unmounting - when the component is removed from the DOM
   1. `componentWillUnmount()`

### Component Mounting Phase

A component "mounts" when it renders for the first time. When a component mounts, it automatically calls these three methods, in the order of:

1. `constructor()`
2. `render()`
3. `componentDidUpdate()`

### Unmounting Lifecycle Method

`componentWillUnmount()` is used to do any necessary cleanup (cancelling any times or intervals) before the component disappears.

`this.setState()` method should not be called inside this method because the component will not be re-rendered.

[Lifecycle Diagram](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)