- came out in 2012
- JS with type enforcement
- less error prone than JS

# Primitive types
```ts
let count: number = 10;            // Number type
let username: string = "Alice";    // String type
let isActive: boolean = false;     // Boolean type
let nothingHere: null = null;      // Null type
let noValueYet: undefined;         // Undefined type
let symbolId: symbol = Symbol();   // Symbol type
let bigNumber: bigint = 100n;      // BigInt type
```

![[Pasted image 20250111185259.png]]

### Unknown
Cant use it until I have checked its type:
```ts
let a: unknown = 30         // unknown
let b = a === 123           // boolean
let c = a + 10              // Error TS2571: Object is of type 'unknown'.
if (typeof a === 'number') {
  let d = a + 10            // number
}
```

### Null, undefined, void and never
- **`null`**: Represents an intentional absence of any object value.
- **`undefined`**: Indicates a variable has been declared but not assigned a value.
- **`void`**: Denotes a function returns no value.
- **`never`**: Represents a value that will never occur (e.g., a function that always throws an error or infinite loop).
# Functions

## Function Types (first class)
```ts
(name: string, value: number) => string
//a function that takes as paremeter a string a number and return a string

//I can pass function types as part of a function call
function apply(arr: string[], func: (s: string) => void) : void {
	arr.forEach((s: string) => func(s))
}

```

## Named functions
```ts
//Function with return type
function add(first: number, second: number): number {
	return first + second
}

//Function that doesn't return anything
function noReturn(): void { ... }

//Function that return `any`
function parseInput(input: any): any { 
	if (typeof input === 'string') { 
		return parseInt(input, 10); 
	} else if (typeof input === 'number') { 
		return input.toString(); 
	} 
}
```

## Other ways
```ts
function greet(name: string) {
  return 'hello ' + name
}

// Function expression
let greet2 = function(name: string) {
  return 'hello ' + name
}

// Arrow function expression
let greet3 = (name: string) => {
  return 'hello ' + name
}

// Shorthand arrow function expression
let greet4 = (name: string) =>
  'hello ' + name

// Function constructor
let greet5 = new Function('name', 'return "hello " + name')
```

## Passing Rest parameters
```ts
function sumVariadicSafe(...numbers: number[]): number {
  return numbers.reduce((total, n) => total + n, 0)
}

sumVariadicSafe(1, 2, 3) // evaluates to 6
```

## `call`, `apply`, and `bind`
```ts



//Call
const person = {
  name: 'Alice',
  greet: function(greeting) {
    console.log(`${greeting}, I'm ${this.name}`);
  }
};
const anotherPerson = { name: 'Bob' };
person.greet.call(anotherPerson, 'Hello'); // "Hello, I'm Bob"
/// This anotherPerson to call the greet function even though they dont have that method defined. 




//Apply
const person = {
  name: 'Alice',
  greet: function(greeting, punctuation) {
    console.log(`${greeting}, I'm ${this.name}${punctuation}`);
  }
};

const anotherPerson = { name: 'Bob' };

// Use `apply` to call `greet` with `anotherPerson` as `this` and arguments as an array
person.greet.apply(anotherPerson, ['Hello', '!']); // "Hello, I'm Bob!"





//Bind
const person = {
  name: 'Alice',
  greet: function(greeting) {
    console.log(`${greeting}, I'm ${this.name}`);
  }
};

const anotherPerson = { name: 'Bob' };

// Create a new function with `this` bound to `anotherPerson`
const boundGreet = person.greet.bind(anotherPerson, 'Hello');

// Invoke later
boundGreet(); // "Hello, I'm Bob"

```

## Generator functions
![[Pasted image 20250111193624.png]]

- The function runs until it hits the `yield` value. The `next` method, resumes the function until it hits `yield` again.
- It stops once it hits the `return` value. 
- If there's no `return` then it runs forever, or as long as the function's `next` is called.




# Objects
```ts

//Declaring an object and its properties types.
let stock: { sym: string, price?: number } = { name: 'AAPL', price: 137.23 }

let c: {
  firstName: string
  lastName: string
} = {
  firstName: 'john',
  lastName: 'barrowman'
}
```
# Optionals
In TypeScript, **optional properties** and **optional parameters** allow you to define elements that may or may not be present. This adds flexibility to your code and makes it possible to represent more complex data structures.

### 1. **Optional Properties in Interfaces**
You can use the `?` symbol to mark properties in an interface as optional. This means the property may or may not be present in an object of that interface type.

```typescript
interface User {
  id: number;
  name: string;
  email?: string; // Optional property
}

const user1: User = { id: 1, name: "Alice" };
const user2: User = { id: 2, name: "Bob", email: "bob@example.com" };
```

- **`email?: string`** means the `email` property is optional.

### 2. **Optional Parameters in Functions**
You can also use `?` to define optional parameters in functions. This allows the function to be called with or without that parameter.

```typescript
function greet(name: string, greeting?: string): string {
  return `${greeting || 'Hello'}, ${name}!`;
}

greet('Alice'); // "Hello, Alice!"
greet('Alice', 'Hi'); // "Hi, Alice!"
```

- **`greeting?: string`** indicates that the `greeting` parameter is optional.

### 3. **Default Values with Optional Parameters**
You can combine optional parameters with default values to ensure a value is used even if the parameter is not provided.

```typescript
function greet(name: string, greeting: string = 'Hello'): string {
  return `${greeting}, ${name}!`;
}

greet('Alice'); // "Hello, Alice!"
greet('Alice', 'Hi'); // "Hi, Alice!"
```

### 4. **Optional Chaining**
Optional chaining (`?.`) is used to access properties or call methods on objects that may be `null` or `undefined`, without throwing an error.

```typescript
let user: User | null = null;
console.log(user?.email); // undefined, no error
```

### 5. **Use in Classes**
Optional properties can also be used in class definitions.

```typescript
class Product {
  name: string;
  price?: number; // Optional property

  constructor(name: string, price?: number) {
    this.name = name;
    this.price = price;
  }
}

const product1 = new Product("Laptop");
const product2 = new Product("Phone", 699);
```

### Key Points:
- **Optional properties** allow properties to be present or absent in an object.
- **Optional parameters** allow flexibility in function arguments.
- **Default values** ensure a fallback value when an optional parameter is not provided.
- **Optional chaining** safely accesses properties of potentially `null` or `undefined` objects.

This feature is useful for creating flexible and robust TypeScript applications.



# Type Aliases

```ts
type Age = number
type Stock = {
	sym: string,
	price: number
};

// Example:
let stock: Stock = { sym: '', price: 0 }  
console.log(typeof stock); //prints: object

```

# Arrays
In TypeScript, arrays are collections of elements that can be of any type, but they are typically of a single type. TypeScript provides strong typing for arrays, ensuring type safety and preventing errors. Here's how you can work with arrays in TypeScript:

### 1. **Declaring Arrays**
You can declare arrays in two main ways:

- **Using square brackets (`[]`):**

```typescript
let numbers: number[] = [1, 2, 3, 4, 5];
```

- **Using `Array<Type>`:**

```typescript
let fruits: Array<string> = ['apple', 'banana', 'cherry'];
```

### 2. **Accessing Elements**
You can access elements using their index, which starts at `0`.

```typescript
let firstFruit = fruits[0]; // 'apple'
```

### 3. **Adding Elements**
You can add elements using methods like `push` or by directly assigning to an index.

```typescript
numbers.push(6); // Adds 6 to the end
fruits[3] = 'date'; // Adds 'date' at index 3
```

### 4. **Removing Elements**
Use methods like `pop`, `shift`, or `splice` to remove elements.

```typescript
numbers.pop(); // Removes the last element
fruits.shift(); // Removes the first element
```

### 5. **Iterating Over Arrays**
You can use loops to iterate over array elements:

- **For loop:**

```typescript
for (let i = 0; i < numbers.length; i++) {
  console.log(numbers[i]);
}
```

- **For...of loop:**

```typescript
for (let fruit of fruits) {
  console.log(fruit);
}
```

- **ForEach method:**

```typescript
fruits.forEach((fruit) => console.log(fruit));
```

### 6. **Array Methods**
TypeScript supports all JavaScript array methods with type safety.

- **`map`**: Creates a new array by applying a function to each element.

```typescript
let doubledNumbers = numbers.map(n => n * 2);
```

- **`filter`**: Creates a new array with elements that pass a test.

```typescript
let evenNumbers = numbers.filter(n => n % 2 === 0);
```

- **`reduce`**: Applies a function against an accumulator and each element to reduce it to a single value.

```typescript
let sum = numbers.reduce((total, n) => total + n, 0);
```

### 7. **Multidimensional Arrays**
Arrays can have multiple dimensions by nesting arrays.

```typescript
let matrix: number[][] = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];
```

### 8. **Tuples**
TypeScript allows defining arrays with a fixed number of elements of specific types, called tuples.

```typescript
let tuple: [string, number] = ['Alice', 25];
```

### 9. **Readonly Arrays**
You can define an array that cannot be modified by using the `readonly` modifier.

```typescript
let readonlyNumbers: readonly number[] = [1, 2, 3];
// readonlyNumbers.push(4); // Error: push does not exist on type 'readonly number[]'
```

### 10. **Type Inference**
TypeScript can infer the type of an array from its initial values.

```typescript
let inferredArray = [1, 2, 3]; // inferred as number[]
```

### Examples

```typescript
let names: string[] = ['Alice', 'Bob', 'Charlie'];
let mixedArray: (number | string)[] = [1, 'two', 3];

// Using array methods
names.push('David');
let upperCaseNames = names.map(name => name.toUpperCase());
console.log(upperCaseNames); // ['ALICE', 'BOB', 'CHARLIE', 'DAVID']
```

### Summary
- **Declaration**: Use `Type[]` or `Array<Type>`.
- **Access and Modify**: Access elements by index and use methods like `push`, `pop`.
- **Iteration**: Use loops and `forEach` for iteration.
- **Methods**: Use methods like `map`, `filter`, `reduce` for transformations.
- **Tuples and Multidimensional**: Define fixed-size tuples and multidimensional arrays.
- **Readonly**: Use `readonly` for immutable arrays.

Arrays in TypeScript enhance JavaScript arrays by providing type safety and improved developer experience.
## Read-only arrays and tuples
```ts
let as: readonly number[] = [1, 2, 3]     // readonly number[]
let bs: readonly number[] = as.concat(4)  // readonly number[]
let three = bs[2]                         // number
as[4] = 5            // Error TS2542: Index signature in type
                     // 'readonly number[]' only permits reading.
as.push(6)           // Error TS2339: Property 'push' does not
                     // exist on type 'readonly number[]'.
```



# Tuples
```ts
let a: [number] = [1]
let carModelAndYear: [number, string] = [1994, "Honda Civic"]

// A tuple of [first name, last name, birth year]
let b: [string, string, number] = ['malcolm', 'gladwell', 1963]

```

# Enums

```ts
enum Language {
  English,
  Spanish,
  Russian
}

enum Language {
  English = 0,
  Spanish = 1,
  Russian = 2
}

let myFirstLanguage = Language.Russian      // Language
let mySecondLanguage = Language['English']  // Language
```

# Types
## Type Narrowing
```ts
type CalculatorProps = {
	left: number
	// 🦺 limit the operator to be only +, -, *, or /
	operator: Operators
	right: number
}

type Operators = "+" | "-" | "*" | "/"
```

## Derived types
```ts
const operations = {
	'+': (left: number, right: number): number => left + right,
	'-': (left: number, right: number): number => left - right,
	'*': (left: number, right: number): number => left * right,
	'/': (left: number, right: number): number => left / right,
}

type Operators = typeof operations

type CalculatorProps = {
	left: number
	// 🐨 derive these values from the keys of the operations object
	operator: keyof Operators
	right: number
}
```
# Classes and Inheritance
```ts
// Represents a chess game
class Game {}

// A chess piece
class Piece {}

// A set of coordinates for a piece
class Position {}

class King extends Piece {}
class Queen extends Piece {}
class Bishop extends Piece {}
class Knight extends Piece {}
class Rook extends Piece {}
class Pawn extends Piece {}
```

# Operators
## Optional chaining
```js
obj?.prop
obj?.[expr]
obj.method?.(args)
```


# Modules
Any file containing a top level import or export is considered to be a "module" or like a class.
Conversely, anything that does not contain it its is considered a global script (?).

## Syntax
```js
//On definition
export function ...
export class Player

//At the end of file
export { Player, Person }


//Default export
export default function add(a, b) {
  return a + b;
}

```

# Type Declaration Files (`.d.ts`)

used to transpile js libraries into ts. 

# Classes

```ts
class Person {
	// Defining Person properties by declaring constructor parameters.
	costructor(
		public readonly name: string,
		age: number
		(...)
	)
}
```

## Getters and setters
```js
class Person {
  private _name: string;
  private _age: number;

  constructor(name: string, age: number) {
    this._name = name;
    this._age = age;
  }

  // Getter for 'name'
  get name(): string {
    return this._name;
  }

  // Setter for 'name'
  set name(newName: string) {
    if (newName.length > 0) {
      this._name = newName;
    } else {
      throw new Error('Name must be a non-empty string');
    }
  }

  // Getter for 'age'
  get age(): number {
    return this._age;
  }

  // Setter for 'age'
  set age(newAge: number) {
    if (newAge > 0) {
      this._age = newAge;
    } else {
      throw new Error('Age must be a positive number');
    }
  }
}

const person = new Person('Maximiliano', 25);

// Using the getter
console.log(person.name); // Output: "Maximiliano"
console.log(person.age);  // Output: 25

// Using the setter
person.name = 'Max';      // Sets the name to 'Max'
person.age = 26;          // Sets the age to 26

// Using the getter again
console.log(person.name); // Output: "Max"
console.log(person.age);  // Output: 26

```

## Structural typing
Matched by signature and type.

```js
class Dog {
  name: string;
  breed: string;

  constructor(name: string, breed: string) {
    this.name = name;
    this.breed = breed;
  }
}

class Animal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }
}

let dog: Dog = new Dog("Buddy", "Golden Retriever");
let animal: Animal = dog; // This works because Dog has a 'name' property

console.log(animal.name); // Output: "Buddy"

p```

# Interfaces
Always use this instead of using `type` aliases.

```ts
interface Animal {
	eat(...) { ... }
	sleep(...) { ... }
}

class Cat implements Animal { ... }

```