# CSS Tips and Tricks

## Calculating REM Font Sizes

First, set the root font-size of the HTML element to 62.5%:

```css
html {
  font-size: 62.5%
}
```

Since most browsers render 62.5% font-size at 10px, this means all of your REM font-size values will be easy to calculate on other selectors. For example:

- 1.0rem = 10px

- 1.2rem = 12px

- 1.6rem = 16px

- 2.0rem = 20px

  ```css
  h4 {
    font-size: 2.0rem; /* 20px equivalent */
  }
  
  p {
    font-size: 1.6rem; /* 16px equivalent */
  }
  ```


## Calc()

Performing calculations on CSS property values can be extremely powerful, especially when crafting a responsive web design. With support in all modern browsers, `calc()` is very handy and can be used in more complex scenarios.

```css
.element {
  width: calc(50% - 20px);
  margin-left: calc(5vw + 10px);
  font-size: calc(1rem + 0.5vw);
}

/* Set the base font size, minimum font size, and scaling factor */
:root {
  --base-font-size: 16px;
  --min-font-size: 14px;
  --max-font-size: 24px;
  --scale-factor: 1.5;
}

/* Use the variables with calc() and clamp() to create fluid fonts */
body {
  font-size: calc(var(--min-font-size) + var(--scale-factor) * 1vw);
  font-size: clamp(
    var(--min-font-size),
    var(--base-font-size) + var(--scale-factor) * 1vw,
    var(--max-font-size)
  );
}
```

This more advanced example is using CSS variables to define the base font size, minimum and maximum font sizes, and a scaling factor. The `calc()` function is creating a font size that scales with the viewport width, while the `clamp()` function ensures that the font size stays within the specified minimum and maximum limits.

## System Fonts

Most of us haven't used system fonts in our website projects for years, ever since the advent of web fonts revolutionized typography across the landscape of the internet in the late 2000s. But there are some very real benefits to using system fonts. Let's take a look at some reasons to reconsider our stance on them.

### 1. Faster Loading Times

System fonts are already installed on the user's device, which means they don't need to be downloaded from the web server like web fonts. This can result in faster loading times for web pages, which is especially important for mobile devices as well as SEO.

### 2. Consistency Across Devices

System fonts are consistent across different devices and operating systems. This means that the typography used in web design will appear the same for all users, regardless of the device they are using, creating a more cohesive and consistent design experience for all users.

### 3. Accessibility

Using system fonts can also improve accessibility for users with disabilities. Some users may have custom font settings on their devices that they rely on for better readability. By using system fonts, web designers can ensure that their content is accessible to a wider range of users.

### 4. No More FOUT

A “flash of unstyled text” (FOUT) is the phenomenon in which a web page loads with the type set using system fonts before switching to the intended web font. This delay is caused by the web fonts being downloaded to the user’s device. Using system fonts obviously eliminates this issue.

```css
/* Some system font stock samples */
.charter {
  font-family: Charter, Bitstream Charter, serif;
}
.georgia {
  font-family: Georgia, Times, Times New Roman, serif;
}
.palatino {
  font-family: Palatino, Palatino Linotype, Palatino LT STD, Book Antiqua, Georgia, serif;
}
.system {
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Helvetica, Arial, sans-serif, Apple Color Emoji, Segoe UI Emoji;
  /* Only supported on Chromium-based browsers and Safari */
  font-family: system-ui;
}
```

## Scroll Snap

The CSS scroll snap property allows you to provide a more predictable and controlled user experience by controlling the scrolling behavior of a container element by defining snap points (specific positions) within the scrollable area.

```css
.container {
  scroll-snap-type: y mandatory;
}

.container .item {
  height: 100vh;
  scroll-snap-align: start;
}
```

## Grid Pile

Instead of using `position absolute` you can use a single call CSS grid to "pile" elements inside it on top of each other, then use `justify` and `align` properties to position them.

```css
.single-cell {
  display: grid;
  place-content: center;
}

/* all elements inside .single-cell will pile on top of each other in the center */
.single-cell > * {
  grid-area: 1/1;
}
```

## The `:is()` pseudo-class function

Simplifies complex selector chains and reduces the size of your CSS code.

```css
/* Instead of doing this.. */
.container h1,
.container h2,
.container h3,
.container h4,
.container h5,
.container h6 {
  margin-bottom: 1rem;
}

/* Add the function to achieve the same effect in a more concise way */
.container :is(h1, h2, h3, h4, h5, h6) {
  margin-bottom: 1rem;
}
```

## The `:has()` pseudo-class function

The long-awaited `:has()` pseudo-class selects elements that contain other elements that match the selector passed into its arguments. This makes it incredibly useful to write styles for components that may or may not contain certain elements inside, or to select a parent element based on the child elements it contains and apply styles to the parent.

```css
/* Select the .card element when it contains an <img>. */
.card:has(img) {
  flex-direction: row;
}
```

This is also a CSS-only approach for parent selectors. For example, let's say you have a paragraph with the class `.child` ad you want to style the parent DIV:

```css
div:has(>p.child) {
  border: 2px solid #000;
}
```

This basic example will apply a 2px black border around the parent DIV. Keep in mind, full support is not quite there yet in all major browsers to use this trick cautiously.

## 3 Ways to Center a DIV

Here's the HTML for the examples:

```html
<div class="container">
  <div class="content">
    <!-- Your content here -->
  </div>
</div>
```



### Flexbox

With this method, you simply set the container element to display: `flex` and `align-items: center`.

```css
.container {
  display: flex;
  align-items: center;
  justify-content: center; /* optional */
  height: 100vh; /* optional */
}
```

### Position and Transform

Use `position: absolute` and `transform: translate`. This method requires you to set a fixed height on the container element.

```css
.container {
  position: relative;
  height: 500px; /* Set a fixed height */
}

.content {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  /* Your content styles here */
}
```

### Grid

This method involves creating a two-column grid, with the first column set to `auto` and the second column set to `1fr`.

```css
.container {
  display: grid;
  align-items: center;
  height: 100vh; /* Set a fixed height */
  grid-template-columns: auto 1fr;
}

.content {
  /* Your content styles here */
}
```

## Animated Background Gradient

Here's a simple, fast way to make a pure CSS animated background gradient.

```css
body {
  background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
  background-size: 400% 400%;
  animation: bg-gradient 5s ease infinite;
}

@keygrames bg-gradient {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}
```

This code will create a subtle angled color animation in the background of the body as shown below.

![gradient](../Images/gradient.jpg)

## Current Color

An often overlooked trick is using the `currentColor` keyword. It will automatically inherit the current element's color value for other properties like `border-color` or `background-color`.

```css
button {
  color: #f06;
  border: 2px solid currentColor;
  /* border will inherit the color value from the 'color' property */
}
```

This is an easy way to reduce the need to repeat color values in your CSS code and keeps it DRY (Don't Repeat Yourself).

## Selectors

3 powerful options for targeting the exact elements you want to style.

### ~ General Sibling Selector

The ~ selector targets all sibling elements that come after the selected element, regardless of their position or proximity. It applies the styles to all matching siblings that follow the selected element in the HTML structure.

```css
h2 ~ p {
  /* CSS applied to all <p> elements that come after <h2> */
}
```

### + Adjacent Sibling Selector

The + selector targets the immediately adjacent sibling element that follows the selected element. It applies the styles only to the first matching sibling immediately next to the selected element.

```css
h2 + p {
  /* CSS applied to the first <p> element immediately after <h2> */
}
```

### > Child Selector

The > selector targets direct child elements of the selected element. It selects elements that are immediate children and ignores nested elements further down the HTML heirarchy.

```css
ul > li {
  /* CSS applied to <li> elements that are direct childnre of <ul> */
}
```

## Loader Icon

Create a loader icon that helps cut the need for another image and give you more flexibility with styling.

```css
.loader {
  width: 40px;
  height: 40px;
  border: 4px solid #f3f3f3;
  border-top: 4px solid #3498db;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0);
  }
  100% {
    transform: rotate(360deg);
  }
}
```

## Tailwind CSS

### `group-hover`

Use the `group-hover` variant to apply hover styles to child elements within a parent container.

```html
<div class="group">
  <button class="group-hover:text-red-500">Hover me!</button>
  <p class="text-gray-500">Child element</p>
</div>
```

By adding the `group` class to a parent container and using the `group-hover` variant on a child elements, you can create interactive hover effects on specific child elements when the parent is hovered. In the demo above, when the parent container is hovered, the button's text color changes to red. What's cool is this feature allows you to apply hover styles to specific elements within a container without requiring additional JavaScript or complex CSS selectors.

