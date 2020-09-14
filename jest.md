# [Jest](https://jestjs.io/)

---

## [Jest Crash Course - Unit Testing in JavaScript](https://www.youtube.com/watch?v=7r4xVDI2vho)

Once you have Jest installed, you can run tests with `jest`.

You can watch for test changes with `jest --watchAll`.

### Matchers

#### toBe

`toBe` tests primitive types (`numbers`, `strings`, etc.) to equal a value:

```js
test("Adds 2 + 2 to equal 4", () => {
  const myValue = 2 + 2;
  expect(myValue).toBe(4);
});
```

#### not

`.not` tests if value should not be a certain value:

```js
test("Adds 2 + 2 to not equal 5", () => {
  const myValue = 2 + 2;
  expect(myValue).not.toBe(5);
});
```

#### toBeNull

`toBeNull` tests if value should equal `null`:

```js
test("Should be null", () => {
  const myValue = null;
  expect(myValue).toBeNull();
});
```

#### toBeUndefined

`toBeUndefined` tests if value should equal `undefined`:

```js
test("Should be undefined", () => {
  const myValue = undefined;
  expect(myValue).toBeUndefined();
});
```

#### toBeDefined

`toBeDefined` tests if value should equal `toBeUndefined`.

```js
test("Should be defined", () => {
  const myValue = "hello";
  expect(myValue).toBeDefined();
});
```

#### toBeTruthy

`toBeTruthy` tests if value should equal `true`.

```js
test("Should be truthy", () => {
  const myValueBoolean = true;
  const myValueString = "hello";
  const myValueNumber = 1;
  expect(myValueBoolean).toBeTruthy();
  expect(myValueString).toBeTruthy();
  expect(myValueNumber).toBeTruthy();
});
```

#### toBeFalsy

`toBeFalsy` tests if value should equal `false`.

```js
test("Should be falsy", () => {
  const myValueBoolean = false;
  const myValueString = null;
  const myValueNumber = 0;
  expect(myValueBoolean).toBeFalsy();
  expect(myValueString).toBeFalsy();
  expect(myValueNumber).toBeFalsy();
});
```

#### toEqual

`toEqual` tests composite data types (`objects`, `functions`, etc.) to equal a value:

```js
test("User should be Sam Sverko object", () => {
  const myValue = {
    firstName: "Sam",
    lastName: "Sverko",
  };
  expect(myValue).toEqual({
    firstName: "Sam",
    lastName: "Sverko",
  });
});

test("myFunction function exists", () => {
  const myFunction = () => {
    return "hello";
  };
  expect(typeof myFunction).toEqual("function");
});
```

#### toBeLessThan & toBeLessThanOrEqual

`toBeLessThan` or `toBeLessThanOrEqual` tests if value is less than or less than or equal to.

```js
test("Should be less than 1000", () => {
  const value = 900;
  expect(value).toBeLessThan(1000);
});

test("Should be less than or equal to 1000", () => {
  const value = 1000;
  expect(value).toBeLessThanOrEqual(1000);
});
```

#### toBeGreaterThan & toBeGreaterThanOrEqual

`toBeGreaterThan` or `toBeGreaterThanOrEqual` tests if value is greater than or greater than or equal to.

```js
test("Should be under 1600", () => {
  const value1 = 800;
  const value2 = 700;
  expect(value1 + value2).toBeGreaterThan(1600);
});

test("Should be under 1600", () => {
  const value1 = 800;
  const value2 = 800;
  expect(value1 + value2).toBeGreaterThanOrEqual(1600);
});
```

#### regex

`.toMatch` tests regular expressions.

```js
test("There is no I in team", () => {
  expect("team").not.toMatch(/i/i);
});
```

#### arrays

`.toContain` tests if value exists inside array.

```js
test("Admin should be in usernames", () => {
  const usernames = ["john", "sam", "admin"];
  expect(usernames).toContain("admin");
});
```

---

### Async data

You must use `expect.assertions(numberOfAssertions)` when working with async data in tests. Ensure to also `return` the value from the async function in the test.

#### Promise

```js
test("User fetched name should be Leanne Graham", () => {
  const fetchUser = () => {
    axios
      .get("https://jsonplaceholder.typicode.com/users/1")
      .then((response) => response.data)
      .catch((error) => "error");
  };

  expect.assertions(1);
  functions.fetchUser().then((data) => {
    return expect(data.name).toEqual("Leanne Graham 1");
  });
});
```

#### Async/Await

```js
test("User fetched name should be Leanne Graham", async () => {
  const fetchUser = () => {
    axios
      .get("https://jsonplaceholder.typicode.com/users/1")
      .then((response) => response.data)
      .catch((error) => "error");
  };

  expect.assertions(1);
  const data = await fetchUser();
  expect(data.name).toEqual("Leanne Graham");
});
```

---

### Execute methods before/after tests

#### Before/after each tests

The example below will execute `initDatabase` before every test, and then execute `closeDatabase` after every test.

```js
beforeEach(() => initDatabase());
afterEach(() => closeDatabase());

test("Adds 2 + 2 to equal 4", () => {
  const myValue = 2 + 2;
  expect(myValue).toBe(4);
});

test("Adds 2 + 2 to not equal 5", () => {
  const myValue = 2 + 2;
  expect(myValue).not.toBe(5);
});
```

#### Before/after all tests

The example below will execute `initDatabase` before the first test, and then execute `closeDatabase` after the last test.

```js
beforeAll(() => initDatabase());
afterAll(() => closeDatabase());

test("Adds 2 + 2 to equal 4", () => {
  const myValue = 2 + 2;
  expect(myValue).toBe(4);
});

test("Adds 2 + 2 to not equal 5", () => {
  const myValue = 2 + 2;
  expect(myValue).not.toBe(5);
});
```

#### Before/after certain tests

To only execute methods before/after certain tests, you can group them within the `describe` test method.

The example below will execute `nameCheck` before each test within the describe test method.

```js
const nameCheck = () => console.log("Checking name...");

describe("Checking names", () => {
  beforeEach(() => nameCheck());

  test("User is Sam", () => {
    const user = "Sam";
    expect(user).toBe("Sam");
  });

  test("User is Katlynn", () => {
    const user = "Katlynn";
    expect(user).toBe("Katlynn");
  });
});
```

---

## [Intro to React Testing [Jest and React Testing Library Tutorial]](https://www.youtube.com/watch?v=ZmVBCpefQe8)

### Introduction

#### What is a test?

Code you write to verify the behaviour of your application.

#### Why write tests?

Tests are specifications for how our code should work.

#### Consistency

Verify that engineers are following best practices and conventions for your tema.

#### Comfort and confidence

A strong test suite is like a warm blanket.

#### Productivity

We write test because it allows us to ship quality code faster.

#### Types of tests

- **End to end:** Spin up your app and simulate user behaviour. Kind of like a robot performing a task on your app.
- **Integration:** Verify that multiple units work together.
- **Unit:** Verify the functionality og a single function/component.
- **Static:** Catch types and errors while writing code.

#### React component tests

These are considered integration/unit tests.

Basic Jest test:

```js
const expected = true;
const actual = false;

test("it works", () => {
  expect(actual).toBe(expected);
});
```

- `test('it works', () => {})` is the Jest test runner.
- `expect(actual).toBe(expected)` is the Jest assertion library.

- **Tip:** Execute tests in random order. There might be some improper component clean up, which gives the next test a false positive.

---

### Rendering components for testing

```js
// the component
import React from "react";
export default function Component() {
  return <h1>hello</h1>;
}

// the test
import React from "react";
import ReactDOM from "react-dom";

import TestComponent from "../src/component";

test("renders the correct content", () => {
  const root = document.createElement("div");
  ReactDOM.render(<TestComponent />, root);

  expect(root.querySelector("h1").textContent).toBe("0");
});
```

---

### Use DOM Testing Library for Querying the DOM

```js
// the component
import React from "react";
export default function Component() {
  return <h1>hello</h1>;
}

// the test
import React from "react";
import ReactDOM from "react-dom";
import { getQueriesForElement } from "@testing-library/dom";

import TestComponent from "../src/component";

test("renders the correct content", () => {
  const root = document.createElement("div");
  ReactDOM.render(<TestComponent />, root);

  const { getByLabelText, getByText } = getQueriesForElement(root);

  expect(getByText("hello")).not.toBeNull();
  // you can also omit the expect, because getByText is the assertion
  // getByText('hello') // this works same as line above
});
```

---

## Rendering and Testing with React Testing Library

```js
// the component
import React from "react";
export default function Component() {
  return <h1>hello</h1>;
}

// the test
import React from "react";
import { render } from "@testing-library/react";

import TestComponent from "../src/component";

test("renders the correct content", () => {
  const { getByText } = render(<TestComponent />);

  expect(getByText("0")).not.toBeNull();
});
```

---

### Simulating User Interaction

```js
// the component
import React, { useState } from "react";
export default function Component() {
  const [counter, setCounter] = useState(0);
  return (
    <>
      <h1 data-testid="counter">{counter}</h1>
      <button data-testid="button-up" onClick={() => setCounter(counter + 1)}>
        Up
      </button>
      <button data-testid="button-down" onClick={() => setCounter(counter - 1)}>
        Down
      </button>
    </>
  );
}

// the test
import React from "react";
import { fireEvent, render } from "@testing-library/react";

import TestComponent from "../src/component";

test("allows users to interact with component", () => {
  const { getByText } = render(<TestComponent />);

  const header = getByText("0");
  const button = getByText("Up");

  fireEvent.click(button);

  expect(header.textContent).toBe("1");
});
```
