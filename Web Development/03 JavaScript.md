
# Declaring variables

Variables are declared using either the `let` or `const` keyword. `let` allows you to change the value of the variable while `const` will cause an error if you attempt to change it.
```js
let x = 1; //can change
const y = 2; //can't change, constant
```

# Features
- is for manipulating the DOM.
- it started as client side scriptiing languge, but thanks to `Node.js` we can know run it as a backend service. 

### Example

```javascript
function sayHello( {
	console.log("hello")
})
```

```html
<body>
	<script>
		function sayGoodbye() {
			alert('Goodbye')
		}
	</script>
	
	<button onclick="sayHello()">Say Hello </button> //METHOD1
	<button onclick="sayGoodbye()">Say Goodbye </button> //METHOD2
	<button onclick="let i=1;i++;console.log(i)">Press me </button> //METHOD3
	
</body>
```

# debugger

`debugger;` to trigger a breakpoint in the code, ex:

```javascript
function trigger() {
	debugger;
}
```

https://htmlpreview.github.io/?https://github.com/webprogramming260/.github/blob/main/profile/javascript/introduction/jsDemo.html

# `Null` / `undefined`

??

# Types
## Objects
```javascript
x = {};
console.log('type changed: ', typeof x, x);
x = { v: 2, z: 'fish' };
console.log('type changed: ', typeof x, x);

```

```js
const obj = new Object({ a: 3 });
obj['b'] = 'fish';
obj.c = [1, 2, 3];
obj.hello = function () {
  console.log('hello');
};

console.log(obj);
// OUTPUT: {a: 3, b: 'fish', c: [1,2,3], hello: func}
```

### Object literals

You can also declare a variable of object type with the `object-literal` syntax. This syntax allows you to provide the initial composition of the object.

```js
const obj = {
  a: 3,
  b: 'fish',
};
```

### Object functions

| Function | Meaning                             |
| -------- | ----------------------------------- |
| entries  | Returns an array of key value pairs |
| keys     | Returns an array of keys            |
| values   | Returns an array of values          |

```js
const obj = {
  a: 3,
  b: 'fish',
};

console.log(Object.entries(obj));
// OUTPUT: [['a', 3], ['b', 'fish']]
console.log(Object.keys(obj));
// OUTPUT: ['a', 'b']
console.log(Object.values(obj));
// OUTPUT: [3, 'fish']
```

### Constructor

Any function that returns an object is considered a `constructor` and can be invoked with the `new` operator.

```js
function Person(name) {
  return {
    name: name,
  };
}

const p = new Person('Eich');
console.log(p);
// OUTPUT: {name: 'Eich'}
```

### Classes

You can use classes to define objects. Using a class clarifies the intent to create a **reusable** component rather than a one-off object. Class declarations look similar to declaring an object, but classes have an explicit constructor and assumed function declarations. The person object from above would look like the following when converted to a class.

```js
class Person {
  constructor(name) {
    this.name = name;
  }

  log() {
    console.log('My name is ' + this.name);
  }
}

const p = new Person('Eich');
p.log();
// OUTPUT: My name is Eich
```

#### Private class properties
You can make properties and functions of classes private by prefixing them with a `#`.

```js
class Person {
  #name;

  constructor(name) {
    this.#name = name;
  }
}

const p = new Person('Eich');
p.#name = 'Lie';
// OUTPUT: Uncaught SyntaxError: Private field '#name' must be declared in an enclosing class
```


## Arrays
```javascript
x = [1, 2];
console.log('type changed: ', typeof x, x);
console.log(Array.isArray(x));
console.log(x instanceof Array);
```

## Functions
functions are a type in JavaScript

```javascript
  x = function () {
    return 3;
  };
  console.log('type changed: ', typeof x, x);
```

### Chat
In JavaScript, functions are considered **first-class citizens**, meaning they are treated as a **type of object**. This allows functions to be assigned to variables, passed as arguments to other functions, and returned from functions, much like any other data type (e.g., numbers, strings).

Here's why functions are treated as a type in JavaScript:

##### 1. Function Objects
   - Functions in JavaScript are technically objects of the `Function` type. This means ==they can have properties and methods==, just like any other object.
   - You can use functions like objects, even adding properties to them.

   ```javascript
   function greet() {
       console.log("Hello!");
   }
   
   greet.language = "English"; // Adding a property to the function
   console.log(greet.language); // Output: English
   ```

##### 2. Storing Functions in Variables
   - Functions can be assigned to variables, making the variable reference that function.

   ```javascript
   const sayHello = function() {
       console.log("Hello, world!");
   };
   
   sayHello(); // Output: Hello, world!
   ```

##### 3. Passing Functions as Arguments
   - Since functions are a type, they can be passed as arguments to other functions, making callbacks and higher-order functions possible.

   ```javascript
   function executeFunction(fn) {
       fn(); // Calling the passed function
   }
   
   executeFunction(() => console.log("Function executed!")); // Output: Function executed!
   ```

##### 4. Returning Functions from Functions
   - Functions can also be returned from other functions, allowing for more dynamic and flexible code.

   ```javascript
   function createMultiplier(factor) {
       return function(number) {
           return number * factor;
       };
   }
   
   const double = createMultiplier(2);
   console.log(double(5)); // Output: 10
   ```

##### 5. Functions are callable objects
   - Unlike normal objects, functions can be "called" (invoked) using parentheses. However, they still maintain the behavior of objects in every other way.

In short, functions in JavaScript are treated as a special type of object, allowing them to behave dynamically and flexibly in the language.

###  Function syntax

```js

function withNoReturn(parameter) {
	//do something
}

function withReturn(parameter) {
	return something
}

```

- **parameter** is not defined with a type
- **return** type is not defined in the function.


### Function parameters
When a function is called, the caller may choose what parameters to provide. If a parameter is not provided then the value of the parameter is `undefined` when the function executes.

In addition to explicitly passing the value of a parameter to a function, the function can define a default value. This is done by assigning a value to the parameter in the function declaration.

```js
function labeler(value, title = 'title') {
  console.log(`${title}=${value}`);
}

labeler();
// OUTPUT: title=undefined

labeler('fish');
// OUTPUT: title=fish

labeler('fish', 'animal');
// OUTPUT: animal=fish
```

### Anonymous functions

Functions in JavaScript are commonly assigned to a variable so that they can be passed as a parameter to some other function or stored as an object property. To easily support this common use **you can define a function anonymously and assign it to a variable.**

```js
// Function that takes a function as a parameter
function doMath(operation, a, b) {
  return operation(a, b);
}

// Anonymous function assigned to a variable
const add = function (a, b) {
  return a + b;
};

console.log(doMath(add, 5, 3));
// OUTPUT: 8

// Anonymous function assigned to a parameter
console.log(
  doMath(
    function (a, b) {
      return a - b;
    },
    5,
    3
  )
);
// OUTPUT: 2
```


### Arrow functions

Because functions are first order objects in JavaScript they can be declared anywhere and passed as parameters. This results in code with lots of anonymous functions cluttering things up. To make the code more compact the `arrow` syntax was created. This syntax replaces the need for the `function` keyword with the symbols `=>` placed after the parameter declaration. The enclosing curly braces are also optional.

This is a function in arrow syntax that takes no parameters and always returns 3.

`() => 3;`

The following two invocations of sort are equivalent.

```js
const a = [1, 2, 3, 4];

// standard function syntax
a.sort(function (v1, v2) {
  return v1 - v2;
});

// arrow function syntax
a.sort((v1, v2) => v1 - v2);

// arrow function with 2 parameters, one return
const lordify = (firstName, land) => `${firstName} of ${land}`;


// arrow function with 2 parameters, 2+ return statements, surround with { .. }
const lordify = (firstName, land) => {
  if (!firstName) {
    throw new Error("A firstName is required to lordify");
  }

  if (!land) {
    throw new Error("A lord must have a land");
  }

  return `${firstName} of ${land}`;
};


// arrow function returning a object: surround object with parenthesis
const person = (firstName, lastName) => ({
  first: firstName,
  last: lastName
});


```

Besides being compact, the `arrow` function syntax has some important semantic differences from the standard function syntax. This includes restrictions that arrow functions cannot be used for constructors or iterator generators.

#### Return values

Arrow functions also have special rules for the `return` keyword. The return keyword is optional if no curly braces are provided for the function and it contains a single expression. In that case the result of the expression is automatically returned. If curly braces are provided then the arrow function behaves just like a standard function.

```js
() => 3;
// RETURNS: 3

() => {
  3;
};
// RETURNS: undefined

() => {
  return 3;
};
// RETURNS: 3
```

# Loops

JavaScript supports many common programming language looping constructs. This includes `for`, `for in`, `for of`, `while`, `do while`, and `switch`. Here are some examples.

### for

Note the introduction of the common post increment operation (`i++`) for adding one to a number.
```js
for (let i = 0; i < 2; i++) {
  console.log(i);
}
// OUTPUT: 0 1
```


### do while

```js
let i = 0;
do {
  console.log(i);
  i++;
} while (i < 2);
// OUTPUT: 0 1
```


### while

```js
let i = 0;
while (i < 2) {
  console.log(i);
  i++;
}
// OUTPUT: 0 1
```


### for in

The `for in` statement iterates over an object's property names.

```js
const obj = { a: 1, b: 'fish' };
for (const name in obj) {
  console.log(name);
}
// OUTPUT: a
// OUTPUT: b
```


For **arrays** the object's name is the array **index**.

```js
const arr = ['a', 'b'];
for (const name in arr) {
  console.log(name);
}
// OUTPUT: 0
// OUTPUT: 1
```


### for of

The `for of` statement iterates over an iterable's (Array, Map, Set, ...) property values.

```js
const arr = ['a', 'b'];
for (const val of arr) {
  console.log(val);
}
// OUTPUT: 'a'
// OUTPUT: 'b'
```


### Break and continue

All of the looping constructs demonstrated above allow for either a `break` or `continue` statement to abort or advance the loop.

```js
let i = 0;
while (true) {
  console.log(i);
  if (i === 0) {
    i++;
    continue;
  } else {
    break;
  }
}
// OUTPUT: 0 1
```

# Strings

Strings are a primitive type in JavaScript. A string variable is specified by surrounding a sequence of characters with **single quotes (`'`), double quotes (`"`), or backticks (`` ` ``)**. 

The meaning of single or double quotes are equivalent, but the **backtick defines a string literal that may contain JavaScript that is evaluated in place and concatenated into the string**. 

A string literal replacement specifier is declared with a dollar sign followed by a curly brace pair. Anything inside the curly braces is evaluated as JavaScript. You can also use backticks to create multiline strings without having to explicitly escape the newline characters using `\n`.

```js
'quoted text'; // " also works

const l = 'literal';
console.log(`string ${l + (1 + 1)} text`);
// OUTPUT: string literal2 text
```
## String functions

The string object has several interesting functions associated with it. Here are some of the commonly used ones.

| Function      | Meaning                                                      |
| ------------- | ------------------------------------------------------------ |
| length        | The number of characters in the string                       |
| indexOf()     | The starting index of a given substring                      |
| split()       | Split the string into an array on the given delimiter string |
| startsWith()  | True if the string has a given prefix                        |
| endsWith()    | True if the string has a given suffix                        |
| toLowerCase() | Converts all characters to lowercase                         |

```js
const s = 'Example:조선글';

console.log(s.length);
// OUTPUT: 11

console.log(s.indexOf('조선글'));
// OUTPUT: 8

console.log(s.split(':'));
// OUTPUT: ['Example', '조선글']

console.log(s.startsWith('Ex'));
// OUTPUT: true

console.log(s.endsWith('조선글'));
// OUTPUT: true

console.log(s.toLowerCase());
// OUTPUT: example:조선글
```

# JSON and JS

You can convert JSON to, and from, JavaScript using the `JSON.parse` and `JSON.stringify` functions.

```js
const obj = { a: 2, b: 'crockford', c: undefined };
const json = JSON.stringify(obj);
const objFromJson = JSON.parse(json);

console.log(obj, json, objFromJson);

// OUTPUT:
// {a: 2, b: 'crockford', c: undefined}
// {"a":2, "b":"crockford"}
// {a: 2, b: 'crockford'} < pretty much the same thing
```

Note that in this example, JSON cannot represent the JavaScript `undefined` object and so it gets dropped when converting from JavaScript to JSON.

# Rest

Sometimes you want a function to take an unknown number of parameters. For example, if you wanted to write a function that checks to see if some number in a list is equal to a given number, you could write this using an array.

```js
function hasNumber(test, numbers) {
  return numbers.some((i) => i === test);
}

const a = [1, 2, 3];
hasNumber(2, a);
// RETURNS: true
```

However sometimes you don't have an array to work with. In this case you could create one on the fly.

```js
function hasTwo(a, b, c) {
  return hasNumber(2, [a, b, c]);
}
```

But JavaScript provides the `rest` syntax to make this easier. Think of it as a parameter that contains the `rest` of the parameters. To turn the last parameter of any function into a `rest` parameter you prefix it with three periods. You can then call it with any number of parameters and they are all automatically combined into an array.

```js
function hasNumber(test, ...numbers) {
  return numbers.some((i) => i === test);
}

hasNumber(2, 1, 2, 3);
// RETURNS: true
```

Note that you can only make the last parameter a `rest` parameter. Otherwise JavaScript would not know which parameters to combine into the array.

Technically speaking, `rest` allows JavaScript to provide what is called variadic functions.

## Spread

Spread does the opposite of rest. It take an object that is iterable (e.g. array or string) and expands it into a function's parameters. Consider the following.

```js
function person(firstName, lastName) {
  return { first: firstName, last: lastName };
}

const p = person(...['Ryan', 'Dahl']);
console.log(p);
// OUTPUT: {first: 'Ryan', last: 'Dahl'}
```

# Exceptions

JavaScript supports exception handling using the `try catch` and `throw` syntax. An exception can be triggered whenever your code generates an exception using the `throw` keyword, or whenever an exception is generated by the JavaScript runtime, for example, when an undefined variable is used.

To catch a thrown exception, you wrap a code block with the `try` keyword, and follow the try block with a `catch` block. If within the try block, including any functions that block calls, an exception is thrown, then all of the code after the throw is ignored, the call stack is unwound, and the catch block is called.

In addition to a catch block, you can specify a `finally` block that is always called whenever the `try` block is exited regardless if an exception was ever thrown.

The basic syntax looks like the following.

## Syntax
```js
try {
  // normal execution code
} catch (err) {
  // exception handling code
} finally {
  // always called code
}
```

## For example:

```js
function connectDatabase() {
  throw new Error('connection error');
}

try {
  connectDatabase();
  console.log('never executed');
} catch (err) {
  console.log(err);
} finally {
  console.log('always executed');
}

// OUTPUT: Error: connection error
//         always executed
```



# DOM (Document Object Model)

## What is DOM?
> In JavaScript, the Document Object Model (DOM) is a programming interface that represents the structure of an HTML or XML document as a tree of objects. Each element, attribute, and piece of text in the document is represented as a node in this tree, with properties and methods that JavaScript can interact with to manipulate the page.

Here are some of the most commonly used DOM functions in JavaScript:

## **`document.getElementById(id)`**
Selects an element by its ID.

   ```javascript
   const element = document.getElementById('myElement');
	//selects an element like <div id="myElement"> </div>>
	
   ```

## **`document.querySelector(selector)`**
Selects the first element that matches a CSS selector.

   ```javascript
   const element = document.querySelector('.myClass');
   ```

Example: 

```html
<!DOCTYPE html>
<html>
<head>
    <title>Example</title>
</head>
<body>
    <div class="myClass">Hello, World!</div>
    <script>
        const element = document.querySelector('.myClass');
        element.textContent = 'Hello, JavaScript!'; // Changes the text inside the div
    </script>
</body>
</html>

```

## **`document.querySelectorAll(selector)`**
Selects all elements that match a CSS selector, returning a NodeList.

   ```javascript
   const elements = document.querySelectorAll('div');
   ```

## **`document.createElement(tagName)`**
Creates a new HTML element.

   ```javascript
   const newDiv = document.createElement('div');  
   //creates an <div></div> element

	const table = document.createElement('table'); 
	//<table></table>
   ```

## **`element.appendChild(child)`**
Appends a child element to a specified parent element.

   ```javascript
   const parent = document.getElementById('parent');
	//<div class="parent">
	// ...
	//</div>
	

   parent.appendChild(newDiv);
	//<div class="parent">
		// <div></div>
	//</div>
   ```

## **`element.removeChild(child)`**
Removes a specified child element from a parent.

   ```javascript
   parent.removeChild(childElement);
   ```

## **`element.setAttribute(name, value)`**
 Sets the value of an attribute on the specified element.

### Syntax:
```js
	element.setAttribute(name, value);
```

### Parameters

1. **`name`**: A string that specifies the name of the attribute you want to set. This can be any valid HTML attribute (like `class`, `id`, `src`, `href`, etc.).
    
2. **`value`**: A string that defines the value you want to assign to the specified attribute. If the attribute does not exist, it will be created.

### Example
   ```javascript
   //before: element = <h1>Title</h1>
   element.setAttribute('class', 'newClass');

	//after: element = <h1 class="newClass">Title</h1>
   ```

### Better Example
```html
    <img id="myImage" src="image.jpg" alt="A sample image">
    <script>
        const image = document.getElementById('myImage');

        // Change the 'src' attribute
        image.setAttribute('src', 'new-image.jpg');

        // Change the 'alt' attribute
        image.setAttribute('alt', 'A new sample image');

        // Add a 'title' attribute
        image.setAttribute('title', 'This is a new image');
    </script>
```
## **`element.getAttribute(name)`**
Gets the value of an attribute from an element.

   ```javascript
   const className = element.getAttribute('class');
   ```

### Example: 
```html
    <a id="myLink" href="https://example.com">Click here</a>
    <script>
        const link = document.getElementById('myLink');

        const hrefValue = link.getAttribute('href');
        // href = "https://example.com"
    </script>
```

## **element.classList**
The `classList` property is a convenient way to access and manipulate the list of classes on an HTML element. It provides a simple interface for adding, removing, and toggling classes without directly manipulating the `class` attribute as a string.

The `classList` property is a convenient way to access and manipulate the list of classes on an HTML element. It provides a simple interface for adding, removing, and toggling classes without directly manipulating the `class` attribute as a string.

### How to Access `classList`

To access the `classList` of an element, you can use the following syntax:

```javascript
const element = document.getElementById('myElement');
const classList = element.classList;
```

### Methods Available on `classList`

Here are some commonly used methods of `classList`:

1. **`add(className)`**: Adds one or more classes to the element.

   ```javascript
   element.classList.add('newClass');
   ```

2. **`remove(className)`**: Removes one or more classes from the element.

   ```javascript
   element.classList.remove('oldClass');
   ```

3. **`toggle(className)`**: Toggles a class; if the class exists, it removes it, and if it doesn't, it adds it.

   ```javascript
   element.classList.toggle('active');
   ```

4. **`contains(className)`**: Checks if the element has a specific class and returns `true` or `false`.

   ```javascript
   const hasClass = element.classList.contains('myClass');
   ```

5. **`replace(oldClass, newClass)`**: Replaces an existing class with a new one.

   ```javascript
   element.classList.replace('oldClass', 'newClass');
   ```

### Example

Here’s a simple example demonstrating how to use `classList`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>classList Example</title>
</head>
<body>
    <div id="myElement" class="box active">Hello World!</div>
    <button id="toggleButton">Toggle Class</button>
    <script>
        const element = document.getElementById('myElement');
        const button = document.getElementById('toggleButton');

        // Log the current classList
        console.log('Initial classList:', element.classList);

        // Toggle the 'active' class on button click
        button.addEventListener('click', () => {
            element.classList.toggle('active');
            console.log('Updated classList:', element.classList);
        });
    </script>
</body>
</html>
```

### In this example:

1. **Initial Class List**: When the page loads, the initial class list of the `<div>` is logged to the console.

2. **Toggle Class**: When the button is clicked, the `active` class is toggled on the `<div>`, and the updated class list is logged.

## Manipulating HTML

Sure! Let's break down `outerHTML`, `innerHTML`, and `textContent` in simple terms, along with straightforward examples.

### 1. `innerHTML`

- **What it is**: `innerHTML` gets or sets the HTML inside an element. It includes any HTML tags inside that element.

#### Example

```html
<div id="myDiv">Hello, <strong>world!</strong></div>
```

- **Using `innerHTML` to get content**:

```javascript
const div = document.getElementById('myDiv');
const content = div.innerHTML; // "Hello, <strong>world!</strong>"
```

- **Using `innerHTML` to set new content**:

```javascript
div.innerHTML = '<p>New content!</p>'; // Replaces the existing content
```

### 2. `outerHTML`

- **What it is**: `outerHTML` gets or sets the entire HTML of an element, including the element itself and its attributes.

#### Example

```html
<div id="myDiv">Hello, <strong>world!</strong></div>
```

- **Using `outerHTML` to get content**:

```javascript
const div = document.getElementById('myDiv');
const fullContent = div.outerHTML; // "<div id="myDiv">Hello, <strong>world!</strong></div>"
```

- **Using `outerHTML` to replace the element**:

```javascript
div.outerHTML = '<section>New section!</section>'; 
// Replaces the whole <div> with a <section>
```

### 3. `textContent`

- **What it is**: `textContent` gets or sets the plain text inside an element, ignoring any HTML tags.

#### Example

```html
<div id="myDiv">Hello, <strong>world!</strong></div>
```

- **Using `textContent` to get content**:

```javascript
const div = document.getElementById('myDiv');
const text = div.textContent; // "Hello, world!" (only text, no tags)
```

- **Using `textContent` to set new text**:

```javascript
div.textContent = 'New plain text!'; // Replaces the text, no HTML allowed
```

### Summary

- **`innerHTML`**: Works with HTML inside an element. It can include tags.
- **`outerHTML`**: Works with the entire element, including its tags and attributes.
- **`textContent`**: Works only with text, ignoring any HTML tags.

## Event Listeners //TODO



# Promises

The state of the promise execution is always in one of three possible states.

1. **pending** - Currently running asynchronously
2. **fulfilled** - Completed successfully
3. **rejected** - Failed to complete


## Resolving and rejecting

Now that we know how to use a promise to execute asynchronously, we need to be able to set the state to `fulfilled` when things complete correctly, or to `rejected` when an error happens. The promise executor function takes two functions as parameters, `resolve` and `reject`. Calling `resolve` sets the promise to the `fulfilled` state, and calling `reject` sets the promise to the `rejected` state.

Consider the following "coin toss" promise that waits ten seconds and then has a fifty percent chance of resolving or rejecting.

```js
const coinToss = new Promise((resolve, reject) => {
  setTimeout(() => {
    if (Math.random() > 0.5) {
      resolve('success');
    } else {
      reject('error');
    }
  }, 10000);
});
```

If you log the coinToss promise object to the console immediately after calling the constructor, it will display that it is in the `pending` state.

```js
console.log(coinToss);
// OUTPUT: Promise {<pending>}
```

If you wait ten seconds and then log the coinToss promise object again, the state will either show as `fulfilled` or `rejected` depending upon the way the coin landed.

```js
console.log(coinToss);
// OUTPUT: Promise {<fulfilled>}
```

## Then, catch, finally

With the ability to asynchronously execute and set the resulting state, we now need a way to generically do something with the result of a promise after it resolves. This is done with functionality similar to exception handling. The promise object has three functions: `then`, `catch`, and `finally`. The `then` function is called if the promise is fulfilled, `catch` is called if the promise is `rejected`, and `finally` is always called after all the processing is completed.

We can rework our coinToss example and make it so 10 percent of the time the coin falls off the table and resolves to the rejected state. Otherwise the promise resolves to fulfilled with a result of either `heads` or `tails`.

```js
const coinToss = new Promise((resolve, reject) => {
  setTimeout(() => {
    if (Math.random() > 0.1) {
      resolve(Math.random() > 0.5 ? 'heads' : 'tails');
    } else {
      reject('fell off table');
    }
  }, 10000);
});
```

We then chain the `then`, `catch` and `finally` functions to the coinToss object in order to handle each of the possible results.
```js
coinToss
  .then((result) => console.log(`Coin toss result: ${result}`))
  .catch((err) => console.log(`Error: ${err}`))
  .finally(() => console.log('Toss completed'));

// OUTPUT:
//    Coin toss result: tails
//    Toss completed
```

### Explanation

In the Promise declaration we can set the conditions upon what is considered successful or rejected. 
For example, I can make an rest api call to a weather service. If I get a code 200 then I will `resolve` with a specific value (maybe the temperature.) Otherwise, (i get a code 404) I `reject`. 

Then, when I do this API call in my website, I can choose to do something according to the state of the `Promise`. 
- I use `then` when I want to work after the state was set to `fulfilled`. I can display the weather.
- I use `catch` when I detect an error in the weather api call and I want to display an error to the user. 
- `finally` will run regarding the result of the `Promise`. 


![[Pasted image 20241019133517.png]]

- [ ] This [CodePen](https://codepen.io/leesjensen/pen/RwJJKwj) uses promises to order pizzas. Create a fork of the pen and take some time to experiment with it. Modify the CodePen to include a **new function** that **makes the pizza** and **include it in the promise chain**.

# Async/Await
## Why do we use it? 
`async` and `await` are used to make working with promises more **readable** and **manageable**, especially when dealing with complex asynchronous code. While you can work with plain promises using `.then()`, `async`/`await` provides several benefits that make asynchronous code easier to write and understand.
### Summary of `async`/`await` Benefits:

- **Readability**: Makes asynchronous code look more like synchronous code, improving readability and maintainability.
- **Error handling**: Uses familiar `try/catch` blocks instead of chaining `.catch()`.
- **Sequential execution**: Easier to handle sequential asynchronous tasks, especially in loops or with complex logic.
- **Less nesting**: Reduces promise chaining and avoids "promise hell."

**`async`/`await`** in JavaScript is a simpler and cleaner way to handle **asynchronous** operations, like making network requests or reading files, which take some time to complete.

## Definitions and how to use 
### What does **asynchronous** mean?
When you do something asynchronously, the task doesn't block your program from doing other things. Instead, your program continues running, and the task completes in the background. When the task finishes, you get the result.

### What is **`async`**?
- **`async`** is a keyword used to declare a function that will handle asynchronous operations.
- An `async` function always returns a **promise**. Even if you don’t explicitly return a promise, JavaScript automatically wraps the return value in a promise.

```javascript
async function myFunction() {
  return "Hello";
}

myFunction().then(console.log);  // Output: Hello
```

### What is **`await`**?
- **`await`** is used inside an `async` function to wait for a promise to resolve (or finish). It pauses the function execution until the asynchronous task is done, making the code look **synchronous** (step-by-step).

```javascript
async function fetchData() {
  const result = await fetch('https://api.example.com/data');  // Wait for the fetch operation
  console.log(result);  // This runs after the fetch is done
}
```

### Why is it useful?
Without `async`/`await`, you'd have to handle promises with `.then()`, which can get messy and harder to read when you have multiple asynchronous tasks. `async`/`await` makes your code look more like normal, step-by-step code while still being non-blocking.

### Simple analogy:
- **Without `async`/`await` (using promises)**: Imagine you’re ordering food online. You place the order (a promise) and keep checking every few minutes if the food has arrived (using `.then()`), which can be annoying.
- **With `async`/`await`**: You place the order (a promise) and go about your day. When the food arrives (the promise resolves), you handle it immediately. You don’t need to keep checking!

In short, `async`/`await` makes working with asynchronous code in JavaScript much easier to read and understand!

### Example from class:

**then/catch chain version**
```js
coinToss()
  .then((result) => console.log(`Toss result ${result}`))
  .catch((err) => console.error(`Error: ${err}`))
  .finally(() => console.log(`Toss completed`));
```


**async, try/catch version**
```js
try {
  const result = await coinToss();
  console.log(`Toss result ${result}`);
} catch (err) {
  console.error(`Error: ${err}`);
} finally {
  console.log(`Toss completed`);
}
```


# Function parameters: Value vs Reference

> [!TLDR]
> - **Primitive values** are passed by value (a copy is made).
> - **Objects (including arrays)** are passed by reference, meaning changes to the object inside the function will be reflected in the original object.
> 

In JavaScript, whether arguments are passed **by value** or **by reference** depends on the **data type** of the argument being passed.

### 1. **Primitive types** (pass-by-value):
   - **Primitive types** in JavaScript include `number`, `string`, `boolean`, `null`, `undefined`, `symbol`, and `bigint`.
   - When you pass a primitive value to a function, it is **copied** into the function parameter. Any changes made to the parameter inside the function do not affect the original value.

   Example with primitives (pass-by-value):
   ```javascript
   function changeValue(x) {
     x = 10;  // This change won't affect the original value
   }

   let num = 5;
   changeValue(num);
   console.log(num);  // Output: 5 (not affected by the change)
   ```

   In this case, `num` remains `5` because the value `5` is copied into the function, and any changes to `x` inside the function don’t affect the original variable.

### 2. **Objects and Arrays** (pass-by-reference):
   - **Objects** (including arrays) are passed **by reference**. This means that when you pass an object to a function, the function receives a reference to the same object in memory, not a copy. Any changes made to the object's properties inside the function will affect the original object.

   Example with objects (pass-by-reference):
   ```javascript
   function changeObject(obj) {
     obj.name = "Max";  // This will change the original object
   }

   let person = { name: "John" };
   changeObject(person);
   console.log(person.name);  // Output: Max (the original object is modified)
   ```

   In this case, `person.name` changes to `"Max"` because the function manipulates the same object reference.

### Important distinction:
- **Primitive values** are passed by value (a copy is made).
- **Objects (including arrays)** are passed by reference, meaning changes to the object inside the function will be reflected in the original object.
  
However, note that while objects are passed by reference, if you reassign the object inside the function, it **won’t affect the original reference** outside the function:

```javascript
function reassignObject(obj) {
  obj = { name: "New Person" };  // This reassignment doesn't affect the original object
}

let person = { name: "John" };
reassignObject(person);
console.log(person.name);  // Output: John (not affected by reassignment)
```

In this case, the reassignment to `obj` inside the function only changes the local reference inside the function, so the original `person` remains unchanged.


# Fetch
In JavaScript, `fetch` is a powerful and easy-to-use function that allows you to make HTTP requests (like GET, POST, PUT, DELETE) to retrieve or send data to servers. It’s a modern alternative to the older `XMLHttpRequest` and is built on **Promises**, making it simpler and more intuitive for handling asynchronous operations.

### Basic Usage of `fetch`

Here’s the general syntax:

```javascript
fetch(url, options)
    .then(response => {
        // Handle the response
    })
    .catch(error => {
        // Handle any errors
    });
```

- **`url`**: The URL endpoint to which the request is made.
- **`options`**: An optional object to configure the request, like setting the method, headers, or body.

### Example: Basic GET Request

Let’s start with a simple GET request:

```javascript
fetch('https://api.example.com/data')
    .then(response => {
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        return response.json();
    })
    .then(data => {
        console.log(data); // The JSON data returned from the server
    })
    .catch(error => {
        console.error('There was a problem with the fetch operation:', error);
    });
```

1. **`fetch`** sends a GET request to `https://api.example.com/data`.
2. **`.then(response => response.json())`** parses the response into JSON, making it easy to work with the data.
3. **`.catch(error => {...})`** handles any network or parsing errors.

### Making POST Requests with `fetch`

In a POST request, you send data to the server. Let’s say we want to send a new user to an API endpoint:

```javascript
fetch('https://api.example.com/users', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'  // Specifies that we’re sending JSON data
    },
    body: JSON.stringify({
        name: 'Alice',
        age: 30
    })
})
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error('Error:', error));
```

- **`method: 'POST'`** tells `fetch` to use the POST method.
- **`headers`** defines the request headers; here, we specify `Content-Type: application/json` since we're sending JSON data.
- **`body`** is the actual data we’re sending, converted to JSON using `JSON.stringify()`.

### Handling Errors with `fetch`

A common mistake is assuming `fetch` automatically throws errors for HTTP response codes like 404 or 500. Instead, it only rejects the promise if there’s a **network error**. If the server responds with an error status (e.g., 404), you’ll need to handle it manually:

```javascript
fetch('https://api.example.com/data')
    .then(response => {
        if (!response.ok) {  // Check if status is not 2xx
            throw new Error(`HTTP error! Status: ${response.status}`);
        }
        return response.json();
    })
    .then(data => console.log(data))
    .catch(error => console.error('Fetch error:', error));
```

### Adding Headers

Sometimes, you need to include additional headers in your request. For example, when using an API that requires an **Authorization token**:

```javascript
fetch('https://api.example.com/secure-data', {
    method: 'GET',
    headers: {
        'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
        'Accept': 'application/json'
    }
})
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error('Fetch error:', error));
```

### Using `async` and `await` with `fetch`

`async` and `await` make `fetch` even easier to use. Here’s the same example as above using `async/await`:

```javascript
async function fetchData() {
    try {
        const response = await fetch('https://api.example.com/data');
        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Fetch error:', error);
    }
}

fetchData();
```

### Common Options in `fetch`

Here are some commonly used `fetch` options:

- **method**: HTTP method (e.g., `GET`, `POST`, `PUT`, `DELETE`).
- **headers**: HTTP headers (like `Content-Type` and `Authorization`).
- **body**: The data to be sent with the request, used mostly in POST and PUT requests.
- **credentials**: To include cookies in a cross-origin request, set to `'include'`.

### Example: Sending Form Data with `fetch`

If you want to send data from a form, `fetch` can work with `FormData` objects:

```javascript
const formData = new FormData();
formData.append('username', 'john_doe');
formData.append('age', 25);

fetch('https://api.example.com/user', {
    method: 'POST',
    body: formData
})
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error('Fetch error:', error));
```

### Example: File Upload with `fetch`

You can also use `fetch` to upload files by appending them to a `FormData` object:

```javascript
const fileInput = document.querySelector('input[type="file"]');
const formData = new FormData();
formData.append('file', fileInput.files[0]);

fetch('https://api.example.com/upload', {
    method: 'POST',
    body: formData
})
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error('Fetch error:', error));
```

### Summary

- **GET Requests**: Simply provide the URL and handle the response.
- **POST Requests**: Use the `method`, `headers`, and `body` options.
- **Error Handling**: Check the `response.ok` property.
- **Async/Await**: Makes handling asynchronous code more readable.
- **Sending Form Data or Files**: Use `FormData` objects for simple handling. 

With `fetch`, you can interact with APIs, send and receive data, and handle all kinds of HTTP requests in a streamlined, modern way. It’s essential for any web developer working with APIs!


# Express

Express is a fast, minimal, and flexible web framework for Node.js that simplifies building server-side applications and APIs. It provides a set of robust features for building web and mobile applications, making it one of the most popular server frameworks for Node.js.

Here’s a breakdown of what makes Express so valuable and widely used:

### Key Features of Express
1. **Minimalist Core**: Express is intentionally lightweight, offering just the core features needed to handle HTTP requests and route them to various endpoints in your app. This means you can add only the features you need, making it highly flexible.

2. **Middleware**: Middleware functions in Express let you handle requests in stages. For example, you can use middleware for logging, handling JSON data, authentication, or error handling, all organized in a simple, sequential way.

3. **Routing**: Express has a robust and intuitive routing system for handling various endpoints and HTTP methods (GET, POST, PUT, DELETE). This allows you to build RESTful APIs or other web services effectively.

4. **Template Engines**: Express supports many template engines like EJS, Pug, and Handlebars, which allow you to render HTML pages dynamically, making it easier to build web apps.

5. **Static File Serving**: Express can serve static files such as images, CSS, and JavaScript files with ease, allowing it to handle both the front end and back end of simple applications.

6. **Full Control**: Unlike frameworks that come with built-in databases, structure, and conventions, Express leaves choices like database selection and app structure up to you. It’s highly unopinionated, so you can choose how to organize your code and what components to use.

### Basic Express App Example

Here's a simple Express app setup:

```javascript
// Import Express
const express = require('express');
const app = express();

// Define a simple route
app.get('/', (req, res) => {
    res.send('Hello, world!');
});

// Define a dynamic route
app.get('/user/:name', (req, res) => {
    res.send(`Hello, ${req.params.name}`);
});

// Start the server on port 3000
app.listen(3000, () => {
    console.log('Server is running on http://localhost:3000');
});
```

- **`app.get()`** defines a route for handling GET requests to `'/'` or `'/user/:name'`.
- **`app.listen()`** starts the server on a specified port (3000 in this example), allowing it to listen for incoming requests.

### Middleware Example

Adding middleware functions is straightforward:

```javascript
// Logging middleware
app.use((req, res, next) => {
    console.log(`${req.method} ${req.url}`);
    next();  // Move to the next middleware or route
});

// Parsing JSON body data
app.use(express.json());

// Define routes
app.post('/data', (req, res) => {
    res.json({ receivedData: req.body });
});
```

### Why Use Express?

Express is versatile and can be used for:
- **Single Page Applications**: Serve a React or Vue app while handling backend operations and APIs.
- **RESTful APIs**: Easily build RESTful services to interact with a frontend or mobile application.
- **Real-Time Applications**: Integrate with WebSockets for chat apps, notifications, etc.
- **Server-Side Rendering**: For rendering dynamic HTML pages (useful for SEO) using template engines.

### Summary

Express simplifies Node.js development by providing essential tools for web development while remaining highly customizable. It’s great for projects of any scale, from quick prototypes to large-scale production apps, due to its flexibility, extensive middleware, and compatibility with the entire JavaScript ecosystem.


## Things I learned from the React service phase

### `Express.json()`

```jsx
const app = express();
app.use(express.json());
```

This line tells app to parse all the requests received as json

If a POST request sends JSON data to an endpoint:

```json
{"username": "max",   "password": "securepass" }
```

Without `express.json()`, `req.body` would be `undefined` because Express wouldn't know how to handle the JSON data. With `express.json()`, Express will parse the JSON data, and you can access it like this:

```jsx
app.post('/login', (req, res) => {
  const { username, password } = req.body;
  console.log(username); // 'max'
  console.log(password); // 'securepass'
});

```

## Router

The code snippet:

```javascript
var apiRouter = express.Router();
app.use('/api', apiRouter);
```

sets up a **sub-router** in an Express.js application. This structure helps organize routes and adds a **prefix** (`/api` in this case) to all routes handled by `apiRouter`. Here’s a breakdown of its purpose and how it works:
### 3. **Example of Usage**
   Suppose you add routes to `apiRouter` like this:

   ```javascript
   apiRouter.get('/users', (req, res) => {
     res.send('List of users');
   });

   apiRouter.post('/users', (req, res) => {
     res.send('Create a new user');
   });
   ```

   With the `app.use('/api', apiRouter);` line, these routes will now be accessible at:

   - `GET /api/users` for retrieving users
   - `POST /api/users` for creating a new user

## 


##

---


# Destructuring

Destructuring in JavaScript is a syntax that lets you unpack values from arrays or properties from objects into distinct variables in a concise way. This feature, introduced in ES6, makes it easier to work with data by extracting values directly without manually accessing each one. Here’s a quick summary of how it works:

### 1. Array Destructuring
With arrays, you can unpack values by position:
```javascript
const colors = ['red', 'green', 'blue'];
const [first, second, third] = colors;
console.log(first); // 'red'
console.log(second); // 'green'
```

You can skip elements with commas or use default values:
```javascript
const [, , last = 'default'] = colors;
console.log(last); // 'blue'
```

### 2. Object Destructuring
With objects, you can unpack properties by their names:
```javascript
// Example 1
const person = { name: 'Alice', age: 25 };
const { name, age } = person;
console.log(name); // 'Alice'
console.log(age); // 25


// Example 2: passing the whole object as parameter
const lordify = ({ firstname }) => {
  console.log(`${firstname} of Canterbury`);
};

const regularPerson = {
  firstname: "Bill",
  lastname: "Wilson"
};

lordify(regularPerson); // Bill of Canterbury
```

You can also rename variables or set defaults:
```javascript
const { name: firstName, age = 30 } = person;
console.log(firstName); // 'Alice'
```

### 4. Function Parameters
You can destructure directly in function arguments:
```javascript
function greet({ name, age }) {
  console.log(`Hello ${name}, you are ${age}`);
}
greet({ name: 'Charlie', age: 28 }); // 'Hello Charlie, you are 28'
```

#### Function parameters:
Yes, destructuring in function arguments is generally recommended when it simplifies your code, especially for functions that take in many parameters or require specific properties from an object. Here are some pros and cons to consider:

##### Advantages of Destructuring in Function Arguments
1. **Improves Readability**: Destructuring makes it immediately clear which properties or elements the function uses, reducing the need to look up parameter details elsewhere.
   ```javascript
   // Instead of this:
   function greet(user) {
     const name = user.name;
     const age = user.age;
     console.log(`Hello ${name}, you are ${age}`);
   }

   // You can do this:
   function greet({ name, age }) {
     console.log(`Hello ${name}, you are ${age}`);
   }
   ```

2. **Handles Default Values Easily**: You can set default values within the destructuring syntax, making it easier to handle optional parameters without extra checks.
   ```javascript
   function greet({ name = 'Guest', age = 18 }) {
     console.log(`Hello ${name}, you are ${age}`);
   }
   ```

3. **Prevents Unused Properties**: Destructuring in function arguments allows you to pick only the properties you need from an object, avoiding clutter and unused data.

4. **Supports Named Parameters**: Destructuring helps emulate named parameters, making it easier to pass arguments without relying on position.
   ```javascript
   // With destructuring:
   greet({ age: 30, name: 'Alice' }); // Order doesn't matter
   ```

##### Considerations and Potential Drawbacks
1. **Readability in Complex Cases**: For deeply nested objects, destructuring directly in function arguments can get complex and harder to read. In such cases, it might be better to destructure in the function body or pass in simpler objects.
   ```javascript
   function handleData({ user: { profile: { name } } }) { ... } // Can get confusing
   ```

##### When to Use It
Destructuring in function arguments is ideal when:
- The function only needs specific properties from an object.
- The argument count is high and the order may vary.
- Default values are needed for some parameters.

However, if the data structure is very complex or deeply nested, it might be better to destructure within the function body for clarity.
##### Summary
Destructuring helps make code shorter and more readable, especially when working with complex data structures or needing to pass data into functions.



# Object Literal Enhancement
_Object literal enhancement_ is the opposite of destructuring. It’s the process of restructuring or putting the object back together. With object literal enhancement, we can grab variables from the global scope and add them to an object:

```js
const name = "Tallac";
const elevation = 9738;

const funHike = { name, elevation };

console.log(funHike); // {name: "Tallac", elevation: 9738}



// With methods
const name = "Tallac";
const elevation = 9738;
const print = function() {
  console.log(`Mt. ${this.name} is ${this.elevation} feet tall`);
};

const funHike = { name, elevation, print };

funHike.print(); // Mt. Tallac is 9738 feet tall
```
# Falsy values

The following values evaluate to `false` (also known as [Falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) values):

- `false`
- `undefined`
- `null`
- `0`
- `NaN`
- the empty string (`""`)

All other values—including all objects—evaluate to `true` when passed to a conditional statement.

# `For in` / `for of`

## For in

Loops through all the `properties` of an `Object`. 
### Example with Object

```js
const person = { name: "Carl", age: 45 }

for (i in person) {
	console.log(i)
}

//Console:
//name
//age

```

### Example with Array

```js
const brands = ["Ford", "Honda", "Kawasaki"]

for (index in brands) {
	console.log(index)
}

//Output:
//0
//1
//2

```

## For of 

Loop that traverse through all the `iterable` properties of an object. Especially suited for Arrays, Sets, Maps, etc.

### Why is better to use `for ... of` with Arrays, instead of `for ... in`

```js
const arr = [3, 5, 7];
arr.foo = "hello";

for (const i in arr) {
  console.log(i);
}
// "0" "1" "2" "foo"

for (const i of arr) {
  console.log(i);
}


// Logs: 3 5 7
```

### Using `destructuring` with `for ... of`

```js
const obj = { foo: 1, bar: 2 };

for (const [key, val] of Object.entries(obj)) {
  console.log(key, val);
}

//Console:
// "foo" 1
// "bar" 2
```


# Working with env variables

The backend and the frontend will have differences. The backend will be running with Node.js, so it will have access to all the files in the server, in this case, it will have access to the .env file. The front end will be running JS through the users browser, so it wont have access to the .env file in the root folder. 
## In the front-end
- The frontend runs **JavaScript in the user’s browser**, which cannot directly access server files, including `.env` files.
- Environment variables for the frontend must be **injected at build time** by tools like **Vite**, **Webpack**, or **Create React App (CRA)**, and then bundled into the frontend code.
- These environment variables are typically exposed as constants in the code (e.g., `process.env.REACT_APP_*` or `import.meta.env.VITE_*`), but **only values explicitly injected during the build are accessible**.
- Since the `.env` file contains potentially sensitive data (e.g., private API keys), it should **never be sent to the frontend or bundled into the client-side code** unless the data is safe for public access.

### Using Vite
1. Make sure I have a .env file in the root of the project. The .env should contain key-value pairs where each key should START with the word `VITE`:
```sh
VITE_BACKEND_URL=https://maxiparis.com/api
```

```js
const backendUrl = import.meta.env.VITE_BACKEND_URL
```

## In the backend

- The backend runs in a **Node.js environment** on a server (e.g., AWS, Heroku, or your local machine during development).
- In a backend context, the server has direct access to all files in its environment, including the `.env` file.
- Using packages like `dotenv`, you can load the contents of the `.env` file into **`process.env`** variables at runtime, enabling the backend to securely access sensitive information like API keys, database URLs, or secrets.

### Using `dotenv`
**1. Install dotenv**
`npm install dotenv`

**2. Create .env file**
```sh
PORT=3000
VITE_BACKEND_URL=https://maxiparis.com/api
JWT_SECRET=supersecretkey
```

3. **Configure dotenv in server**
```js
import dotenv from 'dotenv';
// Load environment variables from .env file
dotenv.config();
```

## Key Differences:

1. **Access to `.env` Files:**
    - **Backend**: Direct access to `.env` at runtime.
    - **Frontend**: No direct access; values from `.env` must be injected during build time.
2. **Security Considerations:**
    - Backend `.env` files can contain sensitive information (e.g., database credentials, API secrets).
    - Frontend `.env` variables must only include non-sensitive values that can safely be exposed to users (e.g., public API URLs).
3. **Execution Environment:**
    - Backend: Runs on the server (Node.js).
    - Frontend: Runs in the browser (JavaScript).


# Spread Operator `...`

The spread operator is three dots (`...`) that perform several different tasks. First, the spread operator allows us to combine the contents of arrays. For example, if we had two arrays, we could make a third array that combines the two arrays into one:

```js
const peaks = ["Tallac", "Ralston", "Rose"];
const canyons = ["Ward", "Blackwood"];
const tahoe = [...peaks, ...canyons];


// Create copy of array
const peaks = ["Tallac", "Ralston", "Rose"];
const [last] = [...peaks].reverse();

console.log(last); // Rose
console.log(peaks.join(", ")); // Tallac, Ralston, Rose



// The spread operator can also be used to get the remaining items in the array:
const lakes = ["Donner", "Marlette", "Fallen Leaf", "Cascade"];

const [first, ...others] = lakes;

console.log(others.join(", ")); // Marlette, Fallen Leaf, Cascade



// Using the spread operator as part of the function parameters. 
function directions(...args) {
  let [start, ...remaining] = args;
  let [finish, ...stops] = remaining.reverse();

  console.log(`drive through ${args.length} towns`);
  console.log(`start in ${start}`);
  console.log(`the destination is ${finish}`);
  console.log(`stopping ${stops.length} times in between`);
}

directions("Truckee", "Tahoe City", "Sunnyside", "Homewood", "Tahoma");
```


# Equality vs Stric Equality operator
In JavaScript, `==` and `===` are comparison operators used to compare two values. However, they differ in how they perform the comparison:

### `==` (Equality Operator)

- **Loose Equality**: It compares two values for equality after converting both values to a common type (type coercion).
- **Type Coercion**: Before comparing, JavaScript converts the operands to the same type.
- **Examples**:
    ```javascript
    '5' == 5  // true, because '5' is converted to a number
    false == 0  // true, because false is converted to 0
    null == undefined  // true, these are loosely equal
    ```

### `===` (Strict Equality Operator)

- **Strict Equality**: It compares two values for equality without converting them to a common type.
- **No Type Coercion**: Both the value and the type must be the same for the comparison to return `true`.
- **Examples**:
    
    ```javascript
    '5' === 5  // false, different types (string vs number)
    false === 0  // false, different types (boolean vs number)
    null === undefined  // false, different types (null vs undefined)
    ```
    

### Key Points

- Use `==` when you want to allow type conversion in comparisons.
- Use `===` when you want to ensure both the value and type are exactly the same, which is generally recommended to avoid unexpected results due to type coercion.

### Best Practice

It's typically a good practice to use `===` over `==` to avoid unintended type coercion, making your code more predictable and easier to understand.