# [react](https://reactjs.org/)

## React.js Essential Training

JSX means JavaScript as XML.

All components should be capitalized.

Access component props with `this.props.propertyName`.

You can use a component prop as a styling value:
```javascript
<MyComponent color='blue' />
<h1 style={{ color: this.props.color }}>My text is blue</h1>
```

You can destructure things:
```javascript
const books = this.props.books
// becomes
const { books } = this.props
```

Setting state is asynchronous, so you can do this:
```javascript
this.setState(prevState => ({
  open: !prevState.open
}))
```

It's a good convention to keep state in the root of the tree. Sometimes this is called "the source of truth".

For function components, if you are only returning one thing, you can omit the curly braces: `const Hiring = () => <div>Hello</div>`.

The React lifecycle:
- Constructor - Called before the component is mounted. Good for setting state and binding methods.
- componentDidMount - Added to the DOM.
- ComponentWillUnmount - Removed from the DOM. Good for cleaning things up like timers.

You cannot use lifecycle methods in function components, only the render.
The only required component lifecycle method is render.

Always a good idea to supply default fallback values should props error out and not pass through.

When using JSX in any file, be sure to `import React from 'react'` at the top.

---

## Learning React.js

React is a JavaScript used for creating user interfaces.

Part of the functional paradigm.

For function components, you can write them multiple ways:
```javascript
const Hello = () => <p>Hello there</p>
// or
const Hello = () => {
  return (
    <p>Hello there</p>
  )
}
```

You don't need to set state in constructor:
```javascript
class MyClass extends React.Component {
  state = {
    myValue: true
  }
}
```

---

## Building Modern Projects with React

