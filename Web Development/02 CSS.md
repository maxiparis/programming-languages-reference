## Units
Here’s a table summarizing the most commonly used units in CSS:

| **Unit** | **Type** | **Description**                                 | **Example**          |
| -------- | -------- | ----------------------------------------------- | -------------------- |
| `px`     | Absolute | Pixels (1px = 1/96th of 1 inch)                 | `width: 200px;`      |
| `in`     | Absolute | Inches (1in = 96px)                             | `width: 2in;`        |
| `cm`     | Absolute | Centimeters (1cm ≈ 37.8px)                      | `width: 5cm;`        |
| `mm`     | Absolute | Millimeters (1mm ≈ 3.78px)                      | `width: 10mm;`       |
| `pt`     | Absolute | Points (1pt = 1/72 of 1 inch)                   | `font-size: 12pt;`   |
| `pc`     | Absolute | Picas (1pc = 12pt = 1/6 of 1 inch)              | `width: 2pc;`        |
| `%`      | Relative | Percentage of the parent element                | `width: 50%;`        |
| `em`     | Relative | Relative to the font size of the parent element | `padding: 2em;`      |
| `rem`    | Relative | Relative to the root element's font size        | `font-size: 1.5rem;` |
| `vw`     | Relative | Relative to 1% of the viewport's width          | `width: 50vw;`       |
| **`vh`** | Relative | Relative to 1% of the viewport's height         | `height: 75vh;`      |
| `vmin`   | Relative | 1% of the viewport’s smaller dimension          | `font-size: 10vmin;` |
| `vmax`   | Relative | 1% of the viewport’s larger dimension           | `width: 80vmax;`     |
| `ch`     | Relative | Width of the "0" character in the current font  | `width: 40ch;`       |
| `ex`     | Relative | Height of the lowercase "x" in the current font | `font-size: 1ex;`    |

# Associating CSS with HTML

There are three ways that you can associate CSS with HTML. 
## The **first way** is to use the style attribute of an HTML element and explicitly assign one or more declarations.

``` html
<p style="color:green">CSS</p>
```

## Second way 
The next way to associate CSS is to use the HTML style element to define CSS rules within the HTML document. The style element should appear in the head element of the document so that the rules apply to all elements of the document.

``` html
<head>
  <style>
    p {
      color: green;
    }
  </style>
</head>
<body>
  <p>CSS</p>
</body>
```

## Third way
The final way to associate CSS is to use the HTML link element to create a hyperlink reference to an external file containing CSS rules. The link element must appear in the head element of the document.

``` html
<link rel="stylesheet" href="styles.css" />
```

``` css
styles.css

p {
  color: green;
}
```

***All of the above examples are equivalent, but using the ==link== element usually is the preferred way to define CSS.***

# Combinators

Next we want to change the color of the second level headings (`h2`), but we only want to do that within the sections for each department. To make that happen we can provide a `descendant combinator` that is defined with a space delimited list of values where each item in the list is a descendant of the previous item. So our selector would be all `h2`elements that are descendants of `section` elements.

``` css
section h2 {
  color: #004400;
}
```
There are other types of combinators that you can use. These include the following.

| Combinator       | Meaning                    | Example        | Description                                |
| ---------------- | -------------------------- | -------------- | ------------------------------------------ |
| Descendant       | A list of descendants      | `body section` | Any section that is a descendant of a body |
| Child            | A list of direct children  | `section > p`  | Any p that is a direct child of a section  |
| General sibling  | A list of siblings         | `div ~ p`      | Any p that has a div sibling               |
| Adjacent sibling | A list of adjacent sibling | `div + p`      | Any p that has an adjacent div sibling     |

We can use the general sibling combinator to increase the whitespace padding on the left of paragraphs that are siblings of a level two heading.

``` css
h2 ~ p {
  padding-left: 0.5em;
}
```

# Class selector

The next selector we will use is the class selector. Remember that any element can have zero or more classifications applied to it. For example, our document has a class of `introduction` applied to the first paragraph, and a class of `summary` applied to the final paragraph of each section. If we want to bold the summary paragraphs we would supply the class name summary prefixed with a period (`.summary`).

``` css
.summary {
  font-weight: bold;
}
```

You can also combine the element name and class selectors to select all paragraphs with a class of summary.

``` css
p.summary {
  font-weight: bold;
}
```

# ID selector

ID selectors reference the ID of an element. All IDs should be unique within an HTML document and so this select targets a specific element. To use the ID selector you prefix the ID with the hash symbol (`#`). We would like to showcase our physics department by putting a thin purple border along the left side of the physics section.

``` css
#physics {
  border-left: solid 1em purple;
}
```

# Pseudo selector

CSS also defines a significant list of pseudo selectors which select based on positional relationships, mouse interactions, hyperlink visitation states, and attributes. We will give just one example. Suppose we want our purple highlight bar to appear only when the mouse hovers over the text. To accomplish this we can change our ID selector to select whenever a section is hovered over.

``` css
section:hover {
  border-left: solid 1em purple;
}
```


## Example: 
``` css
body {
  font-family: sans-serif;
}

h1 {
  border-bottom: thin black solid;
}

section {
  background: #eeeeee;
  padding: 0.25em;
  margin-bottom: 0.5em;
  border-left: solid 1em #eeeeee;  
}

/* h2 descendent of section */
section h2 {
  color: #0044EE;
}

/* Sibling of h2 */
h2 ~ p {
  padding-left: 0.5em;
}


/* paragraph with summary class */
p.summary {
  font-weight: bold;
}

/** on section mouse hover */
section:hover {
  border-left: solid 1em purple;
}


```

``` html
<body>
  <h1>Departments</h1>
  <p>welcome message<p>
  <section id="physics">
    <h2>Physics</h2>
    <p class="introduction">Introduction</p>
    <p>Text</p>
    <p class="summary">Summary</p>
  </section>
  <section id="chemistry">
    <h2>Chemistry</h2>
    <p class="introduction">Introduction</p>
    <p>Text</p>
    <p class="summary">Summary</p>
  </section>
</body>

```

# CSS Declarations
| Property           | Value                              | Example             | Discussion                                                                     |
| ------------------ | ---------------------------------- | ------------------- | ------------------------------------------------------------------------------ |
| background-color   | color                              | `red`               | Fill the background color                                                      |
| border             | color width style                  | `#fad solid medium` | Sets the border using shorthand where any or all of the values may be provided |
| border-radius      | unit                               | `50%`               | The size of the border radius                                                  |
| box-shadow         | x-offset y-offset blu-radius color | `2px 2px 2px gray`  | Creates a shadow                                                               |
| columns            | number                             | `3`                 | Number of textual columns                                                      |
| column-rule        | color width style                  | `solid thin black`  | Sets the border used between columns using border shorthand                    |
| color              | color                              | `rgb(128, 0, 0)`    | Sets the text color                                                            |
| cursor             | type                               | `grab`              | Sets the cursor to display when hovering over the element                      |
| display            | type                               | `none`              | Defines how to display the element and its children                            |
| filter             | filter-function                    | `grayscale(30%)`    | Applies a visual filter                                                        |
| float              | direction                          | `right`             | Places the element to the left or right in the flow                            |
| flex               |                                    |                     | Flex layout. Used for responsive design                                        |
| font               | family size style                  | `Arial 1.2em bold`  | Defines the text font using shorthand                                          |
| grid               |                                    |                     | Grid layout. Used for responsive design                                        |
| height             | unit                               | `.25em`             | Sets the height of the box                                                     |
| margin             | unit                               | `5px 5px 0 0`       | Sets the margin spacing                                                        |
| max-[width/height] | unit                               | `20%`               | Restricts the width or height to no more than the unit                         |
| min-[width/height] | unit                               | `10vh`              | Restricts the width or height to no less than the unit                         |
| opacity            | number                             | `.9`                | Sets how opaque the element is                                                 |
| overflow           | [visible/hidden/scroll/auto]       | `scroll`            | Defines what happens when the content does not fix in its box                  |
| position           | [static/relative/absolute/sticky]  | `absolute`          | Defines how the element is positioned in the document                          |
| padding            | unit                               | `1em 2em`           | Sets the padding spacing                                                       |
| left               | unit                               | `10rem`             | The horizontal value of a positioned element                                   |
| text-align         | [start/end/center/justify]         | `end`               | Defines how the text is aligned in the element                                 |
| top                | unit                               | `50px`              | The vertical value of a positioned element                                     |
| transform          | transform-function                 | `rotate(0.5turn)`   | Applies a transformation to the element                                        |
| width              | unit                               | `25vmin`            | Sets the width of the box                                                      |
| z-index            | number                             | `100`               | Controls the positioning of the element on the z axis                          |
# Units

You can use a variety of units when defining the size of a CSS property. For example, the width of an element can be defined using absolute units such as the number of pixels (`px`) or inches (`in`). You can also use relative units such as a percentage of the parent element (`50%`), a percentage of a minimum viewport dimension (`25vmin`), or a multiplier of the size of the letter m (`1.5rem`) as defined by the root element.

``` css
p {
  width: 25%;
  height: 30vh;
}
```

Here is a list of the most commonly used units. All of the units are prefixed with a number when used as a property value.

| Unit | Description                                                      |
| ---- | ---------------------------------------------------------------- |
| px   | The number of pixels                                             |
| pt   | The number of points (1/72 of an inch)                           |
| in   | The number of inches                                             |
| cm   | The number of centimeters                                        |
| %    | A percentage of the parent element                               |
| em   | A multiplier of the width of the letter `m` in the parent's font |
| rem  | A multiplier of the width of the letter `m` in the root's font   |
| ex   | A multiplier of the height of the element's font                 |
| vw   | A percentage of the viewport's width                             |
| vh   | A percentage of the viewport's height                            |
| vmin | A percentage of the viewport's smaller dimension                 |
| vmax | A percentage of the viewport's larger dimension                  |

# Color

CSS defines multiple ways to describe color, ranging from representations familiar to programmers and ones familiar to layout designers and artists.

| Method       | Example                   | Description                                                                                                                                                                                                       |
| ------------ | ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| keyword      | `red`                     | A set of predefined colors (e.g. white, cornflowerblue, darkslateblue)                                                                                                                                            |
| RGB hex      | `#00FFAA22`or `#0FA2`     | Red, green, and blue as a hexadecimal number, with an optional alpha opacity                                                                                                                                      |
| RGB function | `rgb(128, 255, 128, 0.5)` | Red, green, and blue as a percentage or number between 0 and 255, with an optional alpha opacity percentage                                                                                                       |
| HSL          | `hsl(180, 30%, 90%, 0.5)` | Hue, saturation, and light, with an optional opacity percentage. Hue is the position on the 365 degree color wheel (red is 0 and 255). Saturation is how gray the color is, and light is how bright the color is. |
# Importing fonts

In addition to referencing standard fonts found on the user's computer you can specify a font that you provide with your application. That way your application is guaranteed to always look the same. In order to have the browser load a font you use the `@font-face` rule and provide the font name and source location.

```
@font-face {
  font-family: 'Quicksand';
  src: url('https://cs260.click/fonts/quicksand.ttf');
}

p {
  font-family: Quicksand;
}
```


If you do not want to host font files on your server, then you can load them from a font provider. For example, Google provides a large selection of [open source fonts](https://fonts.google.com/) that you can use without paying any royalties. The easiest way to use Google fonts is to use a CSS import statement to reference the Google Font Service. This will automatically generate the CSS for importing the font.

```
@import url('https://fonts.googleapis.com/css2?family=Rubik Microbe&display=swap');

p {
  font-family: 'Rubik Microbe';
}
```

# Animation

``` css
p {
  text-align: center;
  font-size: 20vh;

  animation-name: demo;
  animation-duration: 3s;
}

@keyframes demo {
  from {
    font-size: 0vh;
  }

  95% {
    font-size: 21vh;
  }

  to {
    font-size: 20vh;
  }
}

```

# Difference between padding, border and margin
In CSS, **padding**, **border**, and **margin** are properties used to control the space around an element. Here's how they differ:

![[Pasted image 20241001145338.png]]
### 1. **Padding**
- **Definition**: Padding is the space **inside** the element, between the content and the element's border.
- **Effect**: It increases the size of the element without affecting the position relative to other elements.
- **Example**:
  ``` css
  .box {
    padding: 20px;
  }
  ```
  This adds 20 pixels of space between the content and the inside edges of the element.

### 2. **Border**
- **Definition**: The border is a line that wraps around the element, outside the padding, separating the element from the margin.
- **Effect**: It creates a visible outline around the element, but can also affect the size of the element.
- **Example**:
  ```css
  .box {
    border: 2px solid black;
  }
  ```
  This adds a 2-pixel solid black line **around** the element.

### 3. **Margin**
- **Definition**: Margin is the space **outside** the element, separating it from neighboring elements.
- **Effect**: It adds space around the element, pushing it away from other elements, without affecting the element’s size.
- **Example**:
  ```css
  .box {
    margin: 20px;
  }
  ```
  This adds 20 pixels of space around the outside of the element, between it and other elements.

### Summary:
- **Padding**: Space **Inside** the border, creates space between content and border.
- **Border**: The visible **outline** around an element.
- **Margin**: Space **Outside** the border, creates space between elements.

These properties are used to manage layout and spacing in CSS effectively.

---
# Display

The CSS display property allows you to change how an HTML element is displayed by the browser. The common options for the display property include the following.

| Value    | Meaning                                                                                                                      |
| -------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `none`   | Don't display this element. The element still exists, but the browser will not render it.                                    |
| `block`  | Display this element with a width that fills its parent element. A `p` or `div` element has block display by default.        |
| `inline` | Display this element with a width that is only as big as its content. A `b` or `span` element has inline display by default. |
| `flex`   | Display this element's children in a flexible orientation.                                                                   |
| `grid`   | Display this element's children in a grid orientation.                                                                       |
```html
<div class="none">None</div>
<div class="block">Block</div>
<div class="inline">Inline1</div>
<div class="inline">Inline2</div>
<div class="flex">
  <div>FlexA</div>
  <div>FlexB</div>
  <div>FlexC</div>
  <div>FlexD</div>
</div>
<div class="grid">
  <div>GridA</div>
  <div>GridB</div>
  <div>GridC</div>
  <div>GridD</div>
</div>
```

By default: 
![[Pasted image 20241003212220.png]]

If we modify the display property associated with each element with the following CSS, then we get a totally different rendering.

```css
.none {
  display: none;
}

.block {
  display: block;
}

.inline {
  display: inline;
}

.flex {
  display: flex;
  flex-direction: row;
}

.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
```

![[Pasted image 20241003212310.png]]

# Flex

Flex is like a Stack in Swift.
It could be vertical (`VStack`)
```css
.container {
	display: flex;
	flex-direction: column;
}
```

or horizontal (`HStack`)
```css
.container {
	display: flex;
	flex-direction: row;
}
```

## Gap

Gap adds even space between the elements inside a `flex` container. 

see [documentation](https://developer.mozilla.org/en-US/docs/Web/CSS/gap)

# Frameworks
## Tailwind

Tailwind takes a different approach than traditional CSS frameworks. Instead of using large, rich, CSS rulesets to compartmentalize styling and functionality, it uses smaller definitions that are applied specifically to individual HTML elements. This moves much of the CSS representation out of the CSS file and directly into the HTML.

```html
<div class="pt-6 md:p-8 text-center md:text-left space-y-4">
  <img class="w-24 h-24 md:w-48 md:h-auto md:rounded-none rounded-full mx-auto" src="profile.png" />
  <p class="text-lg font-medium">“Tailwind CSS”</p>
</div>
```
## Bootstrap

### Getting bootstrap

[Some documentation](https://getbootstrap.com/docs/5.2/getting-started/introduction/)

You can integrate Bootstrap into your web applications simply by referencing the Bootstrap CSS files from their [content delivery network](https://getbootstrap.com/docs/5.2/getting-started/introduction/#cdn-links) (CDN). You then add the HTML link elements to your head element like this.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65"
      crossorigin="anonymous"
    />
  </head>
  <body>
    ...
  </body>
</html>
```
If you are going to use Bootstrap components that require JavaScript (carousel, buttons, and more), you will also need to include Bootstrap's JavaScript module. You add this by putting the following at **the end** of your HTML body element.

```html
<body>
  ...

  <script
    src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js"
    integrity="sha384-kenU1KFdBIe4zVF0s0G1M5b4hcpxyD9F7jL+jjXkk+Q2h455rYXK/7HAuoJl+0I4"
    crossorigin="anonymous"
  ></script>
</body>
```

## Example in CodePen

[Link 🔗](https://codepen.io/leesjensen/pen/JjZavjW)

