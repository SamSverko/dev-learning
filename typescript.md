# [typescript](https://www.typescriptlang.org)

---

## General

* `tsc [filename.ts]` Compile TypeScript file.
* **Type annotations:** Are ways to record the intended contract of the function or variable: `function greeter(person: string) {}`.
	* Boolean: `let isDone: boolean = false;`
	* Number: `let myNumber: number = 6;`
	* String: `let color: string = "blue";`
	* Array: `let list: number[] = [1, 2, 3];` or `let list: Array<number> = [1, 2, 3];`
	* Tuple (array with fixed number of known elements): `let x: [string, number];`
	* Enum: `enum Color {Red, Green, Blue}`
	* Any: `let notSure: any = 4;`
	* Void (functions that do not return values): `function warnUser(): void { console.log('Warning!'); }`
	* Null and Undefined: `let u: undefined = undefined;` and `let n: null = null;`
* **Interfaces:** checking focuses on the shape that values have and must match.
```typescript
interface Person {}
function greeter(person: Person) {}
```

## [TypeScript Tutorial](https://www.tutorialspoint.com/typescript/index.htm)

### Overview

#### What is TypeScript

- TypeScript is a strongly typed, object oriented, compiled language.
- TypeScript is both a language and a set of tools.
- TypeScript is a typed superset of JavaScript compiled to JavaScript.

#### Features of TypeScript

- TypeScript is just JavaScript. TypeScript starts with JavaScript and ends with JavaScript. Typescript adopts the basic building blocks of your program from JavaScript.
- TypeScript supports other JS libraries. Compiled TypeScript can be consumed from any JavaScript code.
- JavaScript is TypeScript. This means that any valid .js file can be renamed to .ts and compiled with other TypeScript files.
- TypeScript is portable. TypeScript is portable across browsers, devices, and operating systems. It can run on any environment that JavaScript runs on.

#### Why Use TypeScript?

- Compilation − JavaScript is an interpreted language. TypeScript will compile the code and generate compilation errors, if it finds some sort of syntax errors.
- Strong Static Typing − JavaScript is not strongly typed.
- TypeScript supports type definitions for existing JavaScript libraries.
- TypeScript supports Object Oriented Programming concepts like classes, interfaces, inheritance, etc.

#### Components of TypeScript

- At its heart, TypeScript has the following three components:
  - Language − It comprises of the syntax, keywords, and type annotations.
  - The TypeScript Compiler − The TypeScript compiler (tsc) converts the instructions written in TypeScript to its JavaScript equivalent.
  - The TypeScript Language Service − The "Language Service" exposes an additional layer around the core compiler pipeline that are editor-like applications.

### Environment Setup

```typescript
var num:number = 12
console.log(num)
```
Becomes
```javascript
var num = 12;
console.log(num);
```

#### Local Environment Setup

- You will need the following tools to write and test a Typescript program:
  - A Text Editor.
  - The TypeScript Compiler.

### Basic Syntax

- To compile a file, run `tsc file.ts`.
- Multiple files can be compiled at once, run `tsc file1.ts, file2.ts, file3.ts`

#### Compiler Flags

- `--help` Displays the help manual.
- `--module` Load external modules.
- `--target` Set the target ECMA version.
- `--declaration` Generates an additional .d.ts file.
- `--removeComments` Removes all comments from the output file.
- `--out` Compile multiple files into a single output file.
- `--sourcemap` Generate a sourcemap (.map) files.
- `--module noImplicitAny` Disallows the compiler from inferring the any type.
- `--watch` Watch for file changes and recompile them on the fly.

#### Identifiers in TypeScript

- Identifiers are names given to elements in a program like variables, functions etc. The rules for identifiers are:
  - Identifiers can include both, characters and digits. However, the identifier cannot begin with a digit.
  - Identifiers cannot include special symbols except for underscore (_) or a dollar sign ($)
  - Identifiers cannot be keywords.
  - They must be unique.
  - Identifiers are case-sensitive.
  - Identifiers cannot contain spaces.

#### TypeScript ─ Keywords

- Keywords have a special meaning in the context of a language. The following [link](https://www.tutorialspoint.com/typescript/typescript_basic_syntax.htm) lists some keywords in TypeScript.

- Each line of instruction is called a statement. Semicolons are optional in TypeScript.

#### Comments in TypeScript

- Single-line comments ( `//` ) − Any text between a // and the end of a line is treated as a comment.
- Multi-line comments (`/* */`) − These comments may span multiple lines.

#### TypeScript and Object Orientation

- TypeScript is Object-Oriented JavaScript. Object Orientation is a software development paradigm that follows real-world modelling.
- Object − An object is a real time representation of any entity. According to Grady Brooch, every object must have three features:
  - State − described by the attributes of an object.
  - Behavior − describes how the object will act.
  - Identity − a unique value that distinguishes an object from a set of similar such objects.
- Class − A class in terms of OOP is a blueprint for creating objects. A class encapsulates data for the object.
- Method − Methods facilitate communication between objects.
- Example:
```typescript
class Greeting {
   greet():void {
      console.log("Hello World!!!")
   }
}
var obj = new Greeting();
obj.greet();
```

### Types

#### The Any type

- The `any` data type is the super type of all types in TypeScript. It denotes a dynamic type. Using the `any` type is equivalent to opting out of type checking for a variable.

#### Built-in types

| Data type | Keyword | Description |
|---|---|---|---|---|
| Number | number | Double precision 64-bit floating point values. It can be used to represent both, integers and fractions. |
| String | string | Represents a sequence of Unicode characters |
| Boolean | boolean | Represents logical values, true and false |
| Void | void | Used on function return types to represent non-returning functions |
| Null | null | Represents an intentional absence of an object value. |
| Undefined | undefined | Denotes value given to all uninitialized variables |

- There is no integer type in TypeScript and JavaScript.

#### Null and undefined ─ Are they the same?

- The `null` and the `undefined` datatypes are often a source of confusion. The `null` and `undefined` cannot be used to reference the data type of a variable. They can only be assigned as values to a variable.
- However, `null` and `undefined` are not the same. A variable initialized with `undefined` means that the variable has no value or object assigned to it while `null` means that the variable has been set to an object whose value is undefined.

### Variables

- A variable, by definition, is “a named space in the memory” that stores values. In other words, it acts as a container for values in a program. TypeScript variables must follow the JavaScript naming rules:
  - Variable names can contain alphabets and numeric digits.
  - They cannot contain spaces and special characters, except the underscore (_) and the dollar ($) sign.
  - Variable names cannot begin with a digit.

#### Variable Declaration in TypeScript

- The type syntax for declaring a variable in TypeScript is to include a colon (:) after the variable name, followed by its type. Just as in JavaScript, we use the `var` keyword to declare a variable.
- When you declare a variable, you have four options:
  - Declare its type and value in one statement: `var [identifier]:[type-annotation] = value ;`
  - Declare its type but no value. In this case, the variable will be set to undefined: `var [identifier]:[type-annotation] =  ;`
  - Declare its value but no type. The variable type will be set to the data type of the assigned value: `var [identifier] = value;`
  - Declare neither value not type. In this case, the data type of the variable will be any and will be initialized to undefined: `var [identifier];`

- Examples:
```typescript
var myName:string = "Sam"
var myScore:number = 50.5
```

#### Type Assertion in TypeScript

- TypeScript allows changing a variable from one type to another. TypeScript refers to this process as Type Assertion. The syntax is to put the target type between < > symbols and place it in front of the variable or expression. The following example explains this concept:
```typescript
var str = '1'
var str2:number = <number> <any> str // str is now of type number
```

#### Inferred Typing in TypeScript

- Typescript is strongly typed, this feature is optional. TypeScript also encourages dynamic typing of variables: `var num = 2;    // data type inferred as  number `

### Functions

- Functions may also return value along with control, back to the caller. Such functions are called as returning functions:
```typescript
function function_name():return_type {
   //statements
   return value;
}

function greet():string { //the function returns a string
   return "Hello World"
}
```

### Arrays

- To declare an initialize an array in Typescript use the following syntax:
```typescript
var array_name[:datatype];        //declaration
array_name = [val1,val2,valn..]   //initialization

var alphas:string[];
alphas = ["1","2","3","4"]
```

### Tuples

- At times, there might be a need to store a collection of values of varied types. Arrays will not serve this purpose. TypeScript gives us a data type called tuple that helps to achieve such a purpose.
```typescript
var tuple_name = [value1, value2, value3, …value n]
var mytuple = [10, "Hello"];
```

### Union

- Union types are a powerful way to express a value that can be one of the several types. Two or more data types are combined using the pipe symbol (|) to denote a Union Type.
```typescript
Type1|Type2|Type3
var val:string|number
```

#### Union Type and Arrays

-Union types can also be applied to arrays, properties and interfaces. The following illustrates the use of union type with an array:
```typescript
var arr:number[]|string[];
var i:number;
arr = [1,2,4]
```

### Interfaces

- An interface is a syntactical contract that an entity should conform to. In other words, an interface defines the syntax that any entity must adhere to.
```typescript
interface IPerson {
  firstName:string,
  lastName:string,
  sayHi: ()=>string
}

var customer:IPerson = {
  firstName:"Tom",
  lastName:"Hanks",
  sayHi: ():string =>{return "Hi there"}
}
```

#### Union Type and Interface

- The following example shows the use of Union Type and Interface:
```typescript
interface RunOptions {
  program:string;
  commandline:string[]|string|(()=>string);
}
```

#### Interfaces and Arrays

- Interface can define both the kind of key an array uses and the type of entry it contains. Index can be of type string or type number:
```typescript
interface namelist {
  [index:number]:string
}
```

---

## [Understanding TypeScript - 2021 Edition](https://www.udemy.com/course/understanding-typescript/learn/lecture/16949812#overview)

### TypeScript (TS) basics & basic types

#### What is TS?

- A JavaScript (JS) superset.
- A language building up on JS.
- Adds new features and advantages to JS.
- Browsers can't execute TS.
- TS compiles to JS.
- Features are compiled to JS "workarounds", possible errors are thrown.

#### Why use TS?

- Unwanted behaviour at runtime in JS, such as using strings of numbers instead of actual numbers.

#### Installing and using TS

- Installing TS using NPM: `sudo npm i -g typescript`.

#### TS advantages - Overview

- TS adds types.
- Next-gen JS features (compiled down for older browsers).
- Non-JS features like Interfaces or Generics.
- Meta-programming features like Decorators.
- Rich configuration options.
- Modern tooling that helps even in non-TS projects.

#### Using types

Core types:
- `number` - All numbers, no differentiation between integers or floats (`1`, `5.3`, `-10`)
- `string` - All text values ("hello", 'hello', ``hello``)
- `boolean` - Just `true` and `false`, no "truthy" or "falsy".

TS's type system only helps you during development (i.e. before the code gets compiled).

#### TS types versus JS types

- JS is dynamically typed. Meaning you can change the type of the variable at any point of the application. They become resolved at runtime.
- TS uses static types. Meaning they are set during development.

#### Type casing

- The core primitive types in TS are all lowercase: `string`, `number`, etc.

#### Working with numbers, string, and booleans

- All numbers are floats by default.

#### Type assignment and type inference

- TS has type inference, meaning TS will know what type a constant will have for primitive types.
- Assigning a type is actually redundant and not a best practice because TS will already know the type and actual value if you use a constant.
- You tell TS the type when you create the variable in an unassigned way.

#### Object types

- TS object types are written with key: type; values.
- TS can infer object type.
```ts
const person: object = {
  name: 'Sam',
}
```
- Using `const person: object = {}` won't have TS infer any of the properties.
- The snippet below isn't needed because TS will infer the types and values.
```ts
const person: {
  name: string;
  age: number;
} = {
  name: 'Sam',
  age: '100'
}
```
- Say you have this JS object:
```js
const product = {
  id: 'abc1',
  price: 12.99,
  tags: ['great-offer', 'hot-and-new'],
  details: {
    title: 'Red Carpet',
    description: 'A great carpet - almost brand-new!'
  }
}
```
- The TS inferred type would be:
```ts
{
  id: string;
  price: number;
  tags: string[],
  details: {
    title: string;
    description: string;
  }
}
```

#### Array types

- Arrays can be flexible or strict.
- An array of strings would be `string[]`.
- To support a mixed array, you can use `any[]`. Not that recommended.

#### Tuples

- `[1, 'hello']` = A fixed-length and type array.
- The type would be: `[number, string]`.

#### Working with enums

- `enum { NEW, OLD }`
- Automatically enumerated global constant identifiers.
- Often, you'll see enums with all-uppercase values, but that's not a "must do". You can go with any value naming convention.
```ts
enum Role { ADMIN, READ_ONLY, AUTHOR }

const person = {
  name: 'Sam',
  age: 100,
  hobbies: ['Cooking', 'Games'],
  role: Role.ADMIN
}
```
- You can set your own enum values and types: `enum Role { ADMIN = 5, READ_ONLY = 100, AUTHOR = 'AUTHOR' }`


#### The any type

- You can store any type of value, no specific type assignment

#### Union types

- Accept multiple type assignments:
```ts
function combine(input1: number | string, input2: number | string) {
  let result
  if (typeof input1 === 'number' && typeof input2 === 'number') {
    result = input1 + input2;
  } else {
    result = input1.toString() + input2.toString()
  }
  return result;
}
```

#### Literal types

- Exact value of the type: `const number = 2.8`. The literal type would be `2.8`.

#### Type aliases / custom types

- You can define custom type using aliases.
```ts
// before
input1: string | number

// using an alias
type Combinable = string | number
```

#### Function return types & void

- TS does infer basic function returns, such as if all inputs are numbers, and it returns a number.
- If you have no reason for setting the type, don't set it, and let TS infer the type.
- Use the `void` type when not returning anything from a function (e.g. the function logs the value instead of returning it).

#### Functions as types

- `let combineValues: Function;`
- `let combineValues: (a: number, b: number) => number;` Should accept any functions that take two parameters that are both numbers and the function returns a number.

#### Function types & callbacks

```ts
function addAndHandle(n1: number, n2: number, cb: (num: number) => void) {
  const result = n1 + n2;
  cb(result)
}

addAndHandle(10, 20, (result) => {
  console.log(result);
})
```
- Add void as a function return type, it tells TS to not worry about what you might return.

#### The "unknown" type

- `let userInput: unknown;`
- Using the `any` type removes all type validations.
- You need to use a type check in order to assign an unknown type to a known type.
```ts
let userInput: unknown;
let userName: string;

userInput = 5;
userInput = 'Max';

if (typeof userInput === 'string') {
  userName = userInput;
}
```

#### The 'never' type

- Use `never` if the function will never return anything.
```ts
function generateError(message: string, code: number): never {
  throw { message: message, errorCode: code }
}
```

### The TS compiler (and its configuration)

#### Using "watch mode"

- Run TS compiler in watch mode: `tsc app.ts -w` or `tsc app.ts --watch`.

#### Compiling the entire project / multiple files

- Initial project as a TS project: `tsc --init`.
- With a project having a `tsconfig.json`, you can watch the entire project using `tsc --watch`.

#### Including and excluding files

- Using the `tsconfig.json` file:
```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "exclude": [
    "*.dev.ts", /* ignore all files that end with .dev.ts */
    "**/*.dev.ts", /* ignore all files that end with .dev.ts in any folder */
    "node_modules",
  ],
  "include": [
    "analytics.ts"
  ],
  "files": [
    "app.ts" /* to point only at files, not directories */
  ]
}
```
- If you do not provide the `exclude` key in the `tsconfig.json`, `node_modules` is excluded automatically.
- If you do provide the `exlude` key, make sure to include the `node_modules` folder.
- There is an `include` key, to tell TS to only compile that's in the `include` array.
- Compilation does include minus exclude. So if you include `app.ts` but also exlude `app.ts`, `app.ts` will not be compiled.

#### Setting a compilation target

- Default target is `ES3`.
- Using `ctrl + space` on an empty value in the `tsconfig.json` will show a dropdown of the options you can select.

#### Understanding TS core libs

- If `lib` is not set in `tsconfig.js`, it will infer default libraries from the target (i.e. `ES6` that have the `DOM` APIs).
- If you manually write `lib` values, you must specificy what you want.
