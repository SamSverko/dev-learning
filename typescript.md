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
