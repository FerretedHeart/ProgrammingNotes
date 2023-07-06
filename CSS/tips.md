# Tips and Tricks

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


# Calc()

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
