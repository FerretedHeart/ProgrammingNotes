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

  
