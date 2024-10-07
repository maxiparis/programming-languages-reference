## Images
To insert images in HTML, you can use the `<img>` tag. Here’s the basic syntax:

```html
<img src="image-path" alt="description" width="width-value" height="height-value">
```

 
### Key Attributes:
1. **`src` (source)**: This is the URL or path to the image you want to display.
2. **`alt` (alternative text)**: This provides a description of the image for accessibility and if the image fails to load.
3. **`width`** and **`height`**: These specify the image's dimensions (optional).

### Example:
```html
<img src="images/picture.jpg" alt="A beautiful sunset" width="500" height="300">
```

- **External Image**: Use an absolute URL for images hosted online.
  ```html
  <img src="https://example.com/image.jpg" alt="Example image">
  ```
  
- **Local Image**: Use a relative path for images stored locally in your project.
  ```html
  <img src="assets/image.jpg" alt="Local image">
  ```

Let me know if you need help with anything specific!

## Header, Nav, Main, Footer
Here’s an explanation of the **`<body>`**, **`<nav>`**, **`<main>`**, **`<header>`**, and **`<footer>`** HTML tags, along with an example of how they are used:

### 1. **`<body>`**:
- **Definition**: Contains the entire content of the HTML document (text, images, videos, links, etc.) that appears on the page.
- **Example**:
  ```html
  <body>
    <h1>Welcome to My Website</h1>
    <p>This is the main content of the page.</p>
  </body>
  ```

### 2. **`<nav>`**:
- **Definition**: Represents a section of the page that contains links to navigate within the website (like a menu or table of contents).
- **Example**:
  ```html
  <nav>
    <ul>
      <li><a href="#home">Home</a></li>
      <li><a href="#about">About</a></li>
      <li><a href="#contact">Contact</a></li>
    </ul>
  </nav>
  ```

### 3. **`<main>`**:
- **Definition**: Contains the dominant content of the `<body>` of a document. It should only contain content unique to that page and be used once per page.
- **Example**:
  ```html
  <main>
    <h2>About Us</h2>
    <p>This section contains information about our company.</p>
  </main>
  ```

### 4. **`<header>`**:
- **Definition**: Represents introductory content or a group of navigation links, typically containing the site’s branding, title, or logo.
- **Example**:
  ```html
  <header>
    <h1>My Awesome Website</h1>
    <p>Welcome to my personal blog!</p>
  </header>
  ```

### 5. **`<footer>`**:
- **Definition**: Represents the footer of a document or section. Usually contains metadata, copyright, contact info, or related links.
- **Example**:
  ```html
  <footer>
    <p>&copy; 2024 My Awesome Website. All rights reserved.</p>
  </footer>
  ```

### Complete Example:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Basic Web Page Structure</title>
</head>
<body>

  <header>
    <h1>My Awesome Website</h1>
    <p>Your go-to site for tech news</p>
  </header>

  <nav>
    <ul>
      <li><a href="#home">Home</a></li>
      <li><a href="#about">About Us</a></li>
      <li><a href="#contact">Contact</a></li>
    </ul>
  </nav>

  <main>
    <h2>Welcome!</h2>
    <p>This is the main content of the website where you can find great articles.</p>
  </main>

  <footer>
    <p>&copy; 2024 My Awesome Website</p>
    <p>Contact us at: info@mywebsite.com</p>
  </footer>

</body>
</html>
```

### Explanation:
- **`<header>`**: Contains the title and a welcoming message.
- **`<nav>`**: Provides a navigation menu with links.
- **`<main>`**: Contains the core content of the page.
- **`<footer>`**: Includes copyright information and contact details.

These tags help structure a webpage and make it more semantically meaningful.

### Summary

``` html
<header>
... could contain the nav here or after
	<nav>
		list
	</nav>
</header>

<main>
	my main content
</main>

<footer>
	footer stuff
</footer>
```
