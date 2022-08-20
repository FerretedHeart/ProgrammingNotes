# CSS

[Cascading Style Sheets](https://developer.mozilla.org/en-US/docs/Web/CSS) for describing how elements should look on screen.

![css-syntax](css-syntax.png)

`.class`- when appending using a class, a period must be appended to the class's name.

`#id`- when appending using an id, a hashtag must be appended to the id's name.

## [HTML Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes)

`[href]`- when targetting an attribute, the attribute needs to be surrounded by square brackets.

`type[attribute*=value]` - selects an element where the attribute contains any instance of the specified value.

`p:hover`- pseudo-class selectors such as `:focus`, `:visited`, `:disabled`, and `:active` give elements a different state.

## Chaining

`h1.special`- combining multiple selectors, so this would select only the `<h1>` elements with a class of `special`.

## Descendents

`.main-list li`- nested elements are descendents of the `<ul>` element and can be selected with the *descendent combinator*, so this would select the `.main-lis` class (the `<ul>` element) and then adding `li` separated by a space.

## Multiple Selectors

It's possible to add CSS styles to multiple CSS selectors all at once. This prevents writing repetitive code.

```css
h1 {
	font-family: Georgia;
}

.menu {
	font-family: Georgia;
}

/* Combine the selectors, separated by commas */
h1,
.menu {
  font-family-Georgia;
}
```

