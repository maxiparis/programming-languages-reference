
## My code
### HTML
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <title>Maximiliano Paris</title>
    <link rel="stylesheet" href="styles.css" />
    <!-- Add any additional meta tags or links here -->
  </head>

  <body>
    <input
      type="text"
      id="arrayNumbers"
      name="arrayNumbers"
      placeholder="Enter some numbers"
      required
    />
    <button id="myButton">Click Me</button>
    <p>Count is <span id="count">bluhh</span></p>
    <script src="index.js"></script>
  </body>
</html>

```

### JavaScript
```javascript
// Select the button element using its ID
const button = document.getElementById("myButton");

// Add an event listener to the button
button.addEventListener("click", handleClick);

function handleClick() {
  console.log("Button was clicked!");
  const numbers = document.getElementById("arrayNumbers").value;
  const result = divisibleBy3(numbers.split(","));
  document.getElementById("count").innerHTML = result;
}

function divisibleBy3(array) {
  let count = 0;
  for (let i = 0; i < array.length; i++) {
    if (array[i] % 3 == 0) {
      count++;
    }
  }
  return count;
}

```

## How to connect HTML with JavaScript

```html
<html>
	<body>
	...
	    <script src="index.js"></script>
	...
	<body>
</html>
```

## How to add a listener to JavaScript
```js
// Select the button element using its ID
const button = document.getElementById("myButton");

// Add an event listener to the button
button.addEventListener("click", handleClick);
```

