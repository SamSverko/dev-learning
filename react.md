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

### Why use the React ecosystem?

The problem with basic React
- Effective way to create modular and performant user interfaces.
- Powerful View portion of the MVC organization.
- It's lacking the Model and Controller.
- React is purposefully designed to allow developers to make their own data mangement decisions.
- Most React developers don't put much thought into these decisions.
- The outcome is huge components, for loading and managing their own data.

The name of the game is separation of concerns.

### Meet the React ecosystem

- Redux - Manage and mofidy the state of the components using the Flux architecture.
- Redux Thunk - Separate out the side effects of the application.
- Reselect - Abstract the details of how our data is managed in the state.
- Styled Components - Abstract away the state's structure.

### Creating Your Basic Project

What do we need?
- index.html
- Support for ES6
- webpack
- Root component
- react-hot-loader

### Why Do We Need Redux?

We need to answer: What's the best way to manage state in our application?

These are a few bad ways to manage your state:

A single root state
- If you use a single root state and pass them down to all the children, that's referred to as "props drilling", which is not good or scalable.

Components manage their own state
- What if a lower child state needs to be updated with another component? You would need to do a lot of hoisting.

Global state management
- A single centralized state where all components share that single state.
- It sounds great in theory, but it can be a nightmare.
- An unrestricted global state is not something you want.

### How Does Redux Work?

We have one central source to manage the state.

The Redux store is the single source of truth for the state.
- Such as if the user is logged in, the list of products, etc.

An unrestricted global state can make bad changes to the global state. We then must assume all components must be properly written.

- Redux store - Stores the state globally.
- Redux actions - Objects consisting of an action type and payload. Explicitely define the different actions that can occur in the application.
- Redux reducers - Specifying what should happen to the store when a certain action occurs.

Components can only interact with the state by triggering Redux actions, which use the Redux reducer to change the Redux store. It is a unidirectional data flow:
- UI triggers actions > state is updated > components see updated state.

When working with reducers, it's important to not mutate the state.

### Redux Best Practices

- Export the connected and unconnected versions of a component.
- Connected version of that component is used in the entire application. The unconnected component is used for testing.
- Your tests shouldn't care whether your component is connected or not.
- Keep Redux actions and async operations out of your reducers.
- Think carefully about connecting components to the Redux store.
- Connecting a component can, in practice, make it less reusable.

### Redux Flow

- Start with `actions.js` to create the new action.
- Then build the `reducers.js` switch case.
- Then add it to your connected component (within the dispatch).
- Then add it to the component item.

### Dealing with Side Effects

Without Redux, you get huge components. Redux allows you to keep lean components.

Components still have to contain all the logic for async operations, also known as "side-effects".
- Redux = state management.
- Components = view logic.
- side effect libraries (Redux Thunk, Redux Saga) = side effects.

### How Does Redux Thunk Work?

- Add a logic fork into flow of operations.
- Components can dispatch a thunk async operation, which will then perform the action/reducer flow into the Redux store.

We should focus the components on their core purpose: taking data and rendering it. Not loading external data or fetching data.

To use Thunk, instead of dispatching an object to the Redux store, we dispatch an async function into the flow.

