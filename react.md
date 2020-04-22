# react

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