# [Jest](https://jestjs.io/)

---

## [Jest Crash Course - Unit Testing in JavaScript](https://www.youtube.com/watch?v=7r4xVDI2vho)

Once you have Jest installed, you can run tests with `jest`.

You can watch for test changes with `jest --watchAll`.

### Matchers

#### toBe

`toBe` is used for primitive types (`numbers`, `strings`, etc.):

```js
test("Adds 2 + 2 to equal 4", () => {
  const myValue = 2 + 2;
  expect(myValue).toBe(4);
});
```

#### not

`.not` tests if something should not be a value:

```js
test("Adds 2 + 2 to not equal 5", () => {
  const myValue = 2 + 2;
  expect(myValue).not.toBe(5);
});
```

#### toBeNull

`toBeNull` matches only `null`:

```js
test("Should be null", () => {
  const myValue = null;
  expect(myValue).toBeNull();
});
```

#### toBeUndefined

`toBeUndefined` matches only `undefined`.

```js
test("Should be undefined", () => {
  const myValue = undefined;
  expect(myValue).toBeUndefined();
});
```

#### toBeDefined

`toBeDefined` is the opposite of `toBeUndefined`.

```js
test("Should be defined", () => {
  const myValue = "hello";
  expect(myValue).toBeDefined();
});
```

#### toBeTruthy

`toBeTruthy` matches anything that an if statement treats as `true`.

```js
test("Should be truth", () => {
  const myValue = true;
  expect(myValue).toBeTruthy();
});
```

#### toBeFalsy

`toBeFalsy` matches anything that an if statement treats as `false`:

```js
test("Should be falsy", () => {
  const myValue = false;
  expect(myValue).toBeFalsy();
});
```

#### toEqual

`toEqual` is used for objects, functions, etc..

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
test("Should be under 1600", () => {
  const value1 = 800;
  const value2 = 700;
  expect(value1 + value2).toBeLessThan(1600);
});

test("Should be under 1600", () => {
  const value1 = 800;
  const value2 = 800;
  expect(value1 + value2).toBeLessThanOrEqual(1600);
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
  usernames = ["john", "sam", "admin"];
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
