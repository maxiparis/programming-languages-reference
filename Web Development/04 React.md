
# Intro 

React abstracts HTML into a JavaScript variant called [JSX](https://reactjs.org/docs/introducing-jsx.html). JSX is converted into valid HTML and JavaScript using a preprocessor called [Babel](https://babeljs.io/). For example, the following is a JSX file. Notice that it mixes both HTML and JavaScript into a single representation.
```js
const i = 3;
const list = (
  <ol class='big'>
    <li>Item {i}</li>
    <li>Item {3 + i}</li>
  </ol>
);
```

Babel will convert that into valid JavaScript:
```js
const i = 3;
const list = React.createElement(
  'ol',
  { class: 'big' },
  React.createElement('li', null, 'Item ', i),
  React.createElement('li', null, 'Item ', 3 + i)
);
```

## Declarative vs Imperative
```js
//Imperative: building the DOM using JS (the how its built)
const target = document.getElementById("target");
const wrapper = document.createElement("div");
const headline = document.createElement("h1");

wrapper.id = "welcome";
headline.innerText = "Hello World";

wrapper.appendChild(headline);
target.appendChild(wrapper);


//Declarative: building the same thing with React (focusing on the what we want to accomplish)
const { render } = ReactDOM;

const Welcome = () => (
  <div id="welcome">
    <h1>Hello World</h1>
  </div>
);

render(<Welcome />, document.getElementById("target"));
```

## JSX
JSX (JavaScript XML) is a syntax extension for JavaScript commonly used with React to describe what the UI should look like. It allows developers to write HTML elements and components within JavaScript code, providing a declarative way to define the structure and content of the user interface.

### Key Features of JSX:

1. **HTML-like Syntax**: JSX allows writing elements and components in a syntax similar to HTML, making it easier to visualize the UI structure.
   ```jsx
   const element = <h1>Hello, world!</h1>;
   ```

2. **Embedding Expressions**: You can embed JavaScript expressions inside JSX using curly braces `{}`.
   ```jsx
   const name = 'Maximiliano';
   const element = <h1>Hello, {name}!</h1>;
   ```

3. **Components**: JSX can include user-defined components, which are typically capitalized.
   ```jsx
   function Welcome(props) {
     return <h1>Hello, {props.name}</h1>;
   }
   ```

4. **Attributes**: JSX supports passing attributes to elements, similar to HTML, but uses camelCase for event handlers and properties.
   ```jsx
   const element = <button onClick={handleClick}>Click me</button>;
   ```

5. **Child Elements**: JSX can have child elements nested within other elements.
   ```jsx
   const element = (
     <div>
       <h1>Hello!</h1>
       <p>Welcome to the site.</p>
     </div>
   );
   ```

6. **Compilation**: JSX is not valid JavaScript; it needs to be transformed into standard JavaScript using a tool like Babel before it can be executed by the browser.

### Benefits of JSX:
- **Improved Readability**: The syntax closely resembles HTML, making it easier to understand the component structure.
- **Seamless Integration with React**: JSX integrates well with React, making it intuitive to create and manage components.
- **Dynamic UI**: Embedding JavaScript expressions within JSX allows for dynamic content and responsive UIs.

Overall, JSX enhances the development experience in React by making the code more declarative and readable, while still maintaining the flexibility of JavaScript.

# Creating a Project
## Using Vite
1. 

# Components
## The render function

One of the primary purposes of a component is to generate the user interface. This is done with the component's `render` function. Whatever is returned from the `render` function is inserted into the component HTML element.

As a simple example, a JSX file containing a React component element named `Demo` would cause React to load the `Demo` component, call the `render` function, and insert the result into the place of the `Demo` element.

**JSX**
```js
<div>
  Component: <Demo />
</div>
```

Notice that `Demo` is not a valid HTML element. The transpiler will replace this tag with the resulting rendered HTML.

**React component**
```js
function Demo() {
  const who = 'world';
  return <b>Hello {who}</b>;
}
```

**Resulting HTML**

`<div>Component: <b>Hello world</b></div>`

## Properties or `Props`

React components also allow you to pass information to them in the form of element properties. The component receives the properties in its constructor and then can display them when it renders.

**JSX**
```html
<div>Component: <Demo who="Walke" /><div>
```

**React component**
```js
function Demo(props) {
  return <b>Hello {props.who}</b>;
}
```

**Resulting HTML**:

`<div>Component: <b>Hello Walke</b></div>`

## State
The `React.useState` hook is used to add state to functional components in React. When you call `useState`, it returns an array with two elements:

1. **The current state value**.
2. **A function to update the state**.

Here's how it works step-by-step:

1. **Import `useState`:**
   ```javascript
   import React, { useState } from 'react';
   ```

2. **Initialize State**:
   You call `useState` inside your component to declare a state variable. You can give it an initial value (number, string, array, object, etc.).
   
   ```javascript
   function Counter() {
       // Declare a state variable called "count" with an initial value of 0
       const [count, setCount] = useState(0);
       
       // count is the state value
       // setCount is the function we call to update it 
   }
   ```

   Here, `count` is the current state, and `setCount` is the function to update it. `useState(0)` means `count` starts at 0.

3. **Update the State**:
   Use the `setCount` function to update `count` when needed. Calling `setCount` tells React to re-render the component with the new state value.
   
   ```javascript
   function Counter() {
       const [count, setCount] = useState(0);
       
       return (
           <div>
               <p>You clicked {count} times</p>
               <button onClick={() => setCount(count + 1)}>
                   Click me
               </button>
           </div>
       );
   }
   ```

   In this example, every time you click the button, `count` increases by 1 because `setCount(count + 1)` is called, updating the state.

4. **Lazy Initialization (Optional)**:
   You can pass a function to `useState` to calculate the initial state only once, which is useful for expensive operations:
   
   ```javascript
   const [value, setValue] = useState(() => {
       return computeExpensiveValue();
   });
   ```

### Summary
- `useState` helps you manage state in functional components.
- `useState(initialValue)` returns `[state, setStateFunction]`.
- Use `setStateFunction(newValue)` to update the state, which re-renders the component.

## Reactivity

A component's properties and state are used by the React framework to determine the reactivity of the interface. Reactivity controls how a component reacts to actions taken by the user or events that happen within the application. Whenever a component's state or properties change, the `render` function for the component and all of its dependent component `render` functions are called.


# Vite

**Vite** is a modern build tool and development server designed to provide a faster and more efficient development experience for JavaScript applications, especially when working with frameworks like React. Here are some key features and benefits of using Vite with React:

### What is Vite?

- **Next Generation Build Tool**: Vite (French for "fast") was created by Evan You, the creator of Vue.js, to improve the development workflow for modern web applications.
- **Native ES Module Support**: Vite leverages the native ES modules in the browser for development, allowing it to serve files over native ESM. This means that you can load modules in the browser without needing to bundle them first, leading to faster startup times.
- **Hot Module Replacement (HMR)**: Vite supports HMR out of the box, which means changes in your code reflect immediately in the browser without needing a full page reload, providing a smoother development experience.
- **Build Optimizations**: For production builds, Vite uses Rollup under the hood, which efficiently bundles and optimizes your code for deployment.

### Benefits of Using Vite with React

1. **Fast Development Server**: The use of native ES modules enables Vite to start up almost instantly and serve your app quickly, which is especially beneficial for large projects.

2. **Instant Feedback**: With HMR, you can see changes in real-time as you edit your code, enhancing your productivity by eliminating wait times for page reloads.

3. **Simple Configuration**: Vite offers sensible defaults and a straightforward configuration process, allowing you to get started quickly without extensive setup.

4. **Optimized Build**: When you’re ready to deploy your application, Vite generates optimized production builds using Rollup, which efficiently bundles your code, reducing the size of your application.

5. **Rich Ecosystem**: Vite supports a wide range of plugins, making it easy to extend its functionality (e.g., for TypeScript, JSX, and more).

6. **Framework Agnostic**: While Vite is well-suited for React, it also works seamlessly with other frameworks like Vue, Svelte, and Preact, allowing you to choose the best tools for your project.

### Getting Started with Vite and React

To create a new React project using Vite, you can run the following command:

```bash
npm create vite@latest my-react-app --template react
```

This command sets up a new React project with Vite configured, allowing you to start coding right away.

### Conclusion

Vite provides a modern, efficient, and enjoyable development experience for React applications by offering fast builds, instant updates, and easy configuration. It's particularly valuable for developers looking for a streamlined workflow without sacrificing performance.

# Router

React does not have a standard router package, and there are many that you can choose from. We will use [react-router-dom](https://www.npmjs.com/package/react-router-dom) Version 6. The simplified routing functionality of React-router-dom derives from the project [react-router](https://www.npmjs.com/package/react-router) for its core functionality. Do not confuse the two, or versions of react-router-dom before version 6, when reading tutorials and documentation.

![React Router](https://raw.githubusercontent.com/webprogramming260/.github/main/profile/webFrameworks/react/router/routerExample.gif)

A basic implementation of the router consists of a `BrowserRouter` component that encapsulates the entire application and controls the routing action. The `Link`, or `NavLink`, component captures user navigation events and modifies what is rendered by the `Routes` component by matching up the `to` and `path` attributes.

```jsx
// Inject the router into the application root DOM element
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  // BrowserRouter component that controls what is rendered
  // NavLink component captures user navigation requests
  // Routes component defines what component is routed to
  <BrowserRouter>
    <div className='app'>
      <nav>
        <NavLink to='/'>Home</Link>
        <NavLink to='/about'>About</Link>
        <NavLink to='/users'>Users</Link>
      </nav>

      <main>
        <Routes>
          <Route path='/' element={<Home />} exact />
          <Route path='/about' element={<About />} />
          <Route path='/users' element={<Users />} />
          <Route path='*' element={<Navigate to='/' replace />} />
        </Routes>
      </main>
    </div>
  </BrowserRouter>
);
```



# Working on TicTacToe

## How app works


### Index.html
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
  <meta charset="UTF-8">  
  <meta name="viewport" content="width=device-width, initial-scale=1.0">  
  <title>Document</title>  
</head>  
<body>  
  <div id="root"></div>  
</body>  
</html>
```

### Index.js
```jsx
import React, { StrictMode } from "react";  
import { createRoot } from "react-dom/client";  
import "./styles.css";  
  
import App from "./App";  
  
const root = createRoot(document.getElementById("root"));  
root.render(  
  <StrictMode>    
	  <App />  
  </StrictMode>  
);
```

### App.js
```jsx
import { useState } from "react";  
  

function Board() {  
    const [squares, setSquares] = useState(Array(9).fill(null));  
    const [xIsNext, setXIsNext] = useState(true);  
  
    function handleClick(i) {  
        if (squares[i] || calculateWinner(squares)) {  
            return;  
        }  
  
        const nextSquares = squares.slice();  
        if (xIsNext) {  
            nextSquares[i] = "X";  
        } else {  
            nextSquares[i] = "O";  
        }  
  
        setSquares(nextSquares);  
        setXIsNext(!xIsNext);  
    }  
  
    const winner = calculateWinner(squares);  
    let status;  
    if (winner) {  
        status = 'Winner: ' + winner;  
    } else {  
        status = 'Next player: ' + (xIsNext ? 'X' : 'O');  
    }  
  
    return (  
        <>  
            <div className="status">{status}</div>  
            <div className="board-row">  
                <Square value={squares[0]} onSquareClick={() => handleClick(0)}/>  
                <Square value={squares[1]} onSquareClick={() => handleClick(1)}/>  
                <Square value={squares[2]} onSquareClick={() => handleClick(2)}/>  
            </div>  
            <div className="board-row">  
                <Square value={squares[3]} onSquareClick={() => handleClick(3)}/>  
                <Square value={squares[4]} onSquareClick={() => handleClick(4)}/>  
                <Square value={squares[5]} onSquareClick={() => handleClick(5)}/>  
            </div>  
            <div className="board-row">  
                <Square value={squares[6]} onSquareClick={() => handleClick(6)}/>  
                <Square value={squares[7]} onSquareClick={() => handleClick(7)}/>  
                <Square value={squares[8]} onSquareClick={() => handleClick(8)}/>  
            </div>  
        </>  
  )  
}  
  
  
export default function Game() {  
    return (  
        <div className="game">  
            <div className="game-board">  
                <Board />  
            </div>  
            <div className="game-info">  
                <ol>{/*TODO*/}</ol>  
            </div>  
        </div>  
    );  
}  
  
function calculateWinner(squares) {  
    const lines = [  
        [0, 1, 2],  
        [3, 4, 5],  
        [6, 7, 8],  
        [0, 3, 6],  
        [1, 4, 7],  
        [2, 5, 8],  
        [0, 4, 8],  
        [2, 4, 6]  
    ];  
    for (let i = 0; i < lines.length; i++) {  
        const [a, b, c] = lines[i];  
        if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {  
            return squares[a];  
        }  
    }  
    return null;  
}
```

## Export default function

Allows me to declare which component from a specific file will be exported and used, when that file is called.

### Example: 

**App.jsx**:
```jsx
export default function Game() {
	...
}
```


**index.js**
```jsx
import App from "./App";  
  
const root = createRoot(document.getElementById("root"));  
root.render(  
  <StrictMode>    
	  <App />  //this will render Game()
  </StrictMode>  
);
```

In this case, when `index.js` renders `App`,  it will actually be rendering the `Game()` component. 


## Installed packages

### Summary of Installed Packages

The setup includes:

- **`react`**: The core React library.
- **`react-dom`**: Provides DOM-specific methods for rendering React components.
- **`react-scripts`**: Handles the build process, development server, and configuration out of the box.


# Hooks
> Functions that start with the word `use` and return a value, object, or array. 

- They can only be accessed through React components. 

### Basic Hooks
1. **`useState`**
    - Manages state in functional components.
    - Example: `const [count, setCount] = useState(0);`
    
1. **`useEffect`**
    - Handles side effects such as fetching data, subscriptions, or manually changing the DOM.
    - Example:
```js
	useEffect(() => {   
		console.log("Component mounted"); 
	}, []);
```

- Last parameter is an optional dependency:
	- missing = invoke one every render
	- empty [] = invoke only on the initial render
	- comma separated list  = invoked when any of those variables changes



3. **`useContext`**
    
    - Accesses context values defined by `React.createContext`.
    - Example:
        `const value = useContext(MyContext);`
        
### Additional Hooks

4. **`useReducer`**
    
    - A more advanced state management alternative to `useState`, especially for complex state logic.
    - Example:
        `const [state, dispatch] = useReducer(reducer, initialState);`
        
5. **`useCallback`**
    
    - Memoizes a function to avoid re-creating it on every render.
    - Example:
        `const memoizedCallback = useCallback(() => {   doSomething(); }, [dependencies]);`
        
6. **`useMemo`**
    
    - Memoizes a value to optimize performance.
    - Example:
        `const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);`
        
7. **`useRef`**
    
    - Holds a mutable reference to a DOM element or a value that persists across renders.
    - Example:
        `const inputRef = useRef(null);`
        
8. **`useLayoutEffect`**
    
    - Similar to `useEffect`, but fires synchronously after all DOM mutations.
    - Example:
        `useLayoutEffect(() => {   console.log("Synchronous side-effect"); }, []);`
        
9. **`useImperativeHandle`**
    
    - Customizes the instance value exposed when using `React.forwardRef`.
    - Example:
        `useImperativeHandle(ref, () => ({   focus: () => {     inputRef.current.focus();   }, }));`












# How to inject a React app into HTML

To render an app we are going to need 3 basic files:
- **index.html**: this file will create the main html file that will be served to the user. It will contain an element `<div id="root"></div>` which is where `index.jsx` file will inject the `app.jsx`
- **index.jsx**
- **app.jsx**

## Index.html
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <title>GateKeeper 🏰| Index</title>

  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
    <script type="module" src="/index.jsx"></script>
  </body>
</html>
```

## Index.jsx
```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './src/app';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```
## App.jsx
```js
import React from 'react';
import 'bootstrap/dist/css/bootstrap.min.css';
import './app.css';

export default function App() {
    return <div className='body bg-dark text-light'>App will display here</div>;
}
```


# Router
**React Router** is a standard library for routing in React applications. It enables the navigation among views of various components in a React application, allows changing the browser URL, and keeps the UI in sync with the URL.

### Key Features of React Router:
1. **Declarative Routing**: Routes are declared using a declarative syntax, making the code easy to read and understand.
   ```jsx
   <Route path="/home" component={Home} />
   ```

2. **Dynamic Routing**: Unlike traditional routing, which is configured in a static file, React Router allows dynamic routing that can be updated based on user interaction or API data.

3. **Nested Routes**: React Router supports nesting of routes to create a hierarchical structure of components, which mirrors the URL path structure.
   ```jsx
   <Route path="/dashboard">
     <Route path="settings" component={Settings} />
   </Route>
   ```

4. **URL Parameters**: It allows passing dynamic segments in the URL, which can be accessed within the components to fetch or display specific data.
   ```jsx
   <Route path="/user/:id" component={User} />
   ```

5. **Programmatic Navigation**: You can navigate programmatically using the `useNavigate` hook or `history` object, which allows redirecting users based on logic.
   ```jsx
   const navigate = useNavigate();
   navigate('/profile');
   ```

6. **Route Guards**: Implement logic to protect certain routes, allowing access only to authenticated users or based on certain conditions.

### Basic Components of React Router:
1. **`<BrowserRouter>`**: The main component that wraps the application and enables routing.
   ```jsx
   import { BrowserRouter } from 'react-router-dom';
   <BrowserRouter>
     <App />
   </BrowserRouter>
   ```

2. **`<Routes>` and `<Route>`**: `<Routes>` contains multiple `<Route>` elements, each defining a path and the component to render.
   ```jsx
   <Routes>
     <Route path="/" element={<Home />} />
     <Route path="/about" element={<About />} />
   </Routes>
   ```

3. **`<Link>`**: Used for navigation between routes without refreshing the page.
   ```jsx
   <Link to="/about">About</Link>
   ```

4. **`useNavigate`**: A hook that provides a function to navigate to different routes programmatically.
   ```jsx
   const navigate = useNavigate();
   ```

5. **`useParams`**: A hook that extracts parameters from the URL.
   ```jsx
   const { id } = useParams();
   ```

### Example:
```jsx
import { BrowserRouter as Router, Routes, Route, Link } from 'react-router-dom';

function App() {
  return (
    <Router>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Router>
  );
}

function Home() {
  return <h2>Home Page</h2>;
}

function About() {
  return <h2>About Page</h2>;
}
```

### Benefits of React Router:
- **SPA Navigation**: Helps in building Single Page Applications with seamless navigation without full page reloads.
- **Flexible and Modular**: Highly customizable and works well with complex routing needs.
- **URL Management**: Keeps the browser history in sync with the UI, supporting back and forward navigation.

React Router is a powerful and flexible tool for managing routing in React applications, enhancing user experience through smooth navigation and dynamic content loading.

# Things learned from GateKeeper's React phase 
### Passing states to children components
```js
<Login
	userName={userName}
	authState={authState}
	onAuthChange={(userName, authState) => {
	  setAuthState(authState);
	  setUserName(userName);
	}}
/>
```

What is going on here?
- `onAuthChange` is a function that we are defining in the `App` component and we are passing to the `Login` component. 
	- we are defining what the function does, and which parameters it has
	- `Login` is free to call `onAuthChange` whenever it wants, and with whatever arguments it wants. 
	- 

### Using `localStorage`

```js
localStorage.getItem('userName') || ''
localStorage.setItem('userName', userName);
localStorage.removeItem('userName');

//JSON
const scoresText = localStorage.getItem('scores');  
if (scoresText) {  
  scores = JSON.parse(scoresText);  
}
localStorage.setItem('scores', JSON.stringify(scores));
```


# Good stuff for portfolio

https://aaronshaf.github.io/react-toggle/
https://blog.logrocket.com/dark-mode-react-in-depth-guide/


