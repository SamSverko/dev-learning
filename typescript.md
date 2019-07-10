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
