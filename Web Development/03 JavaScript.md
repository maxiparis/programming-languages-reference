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

#### Chat
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
