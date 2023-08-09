# HTML Tips and Tricks

## Collapsible Content Sections

Use the `<details>` and `<summary>` elements to create native, collapsible content sections without any special JavaScript or CSS. These elements can be particularly useful for creating FAQs, simple accordions, or other expandable sections.

```html
<details>
  <summary>Click to reveal the answer</summary>
  <p>This is the hidden content that will be displayed.</p>
</details>
```

The `<summary>` element serves as the visible label for the collapsible section. When the user clicks on the `<summary>` element, the hidden content 

## Scroll Indicator

With barebones code, you can make a scroll indicator that will show users what position of the page they're at as they scroll down. This works excellent for longer content, such as blog articles.

HTML:

```html
<progress id="scrollProgress"  value="0" max="100"></progress
```

CSS:

```css
progress#scrollProgress {
  width: 100%;
  height: 5px;
  position: fixed;
  top: 0;
  left: 0;
  -webkit-appearance: none;
  appearance: none;
}
progress#scrollProgress::-webkit-progress-bar { background-color: #eee; }
progress#scrollProgress::-webkit-progress-value { background-color: #f06; }
progress#scrollProgress::-moz-progress-bar { background-color: #f06; }
```

JS:

```javascript
document.addEventListener('DOMContentLoaded', () => {
  const progress = document.getElementById('scrollProgress');
  
  window.addEventListener('scroll', () => {
    const scrollPos = window.pageYOffset || document.documentElement.scrollTop;
    const totalHeight = document.body.scrollHeight - window.innerHeight;
    progress.value = (scrollPos / totalHeight) * 100;
  })
})
```

Final Result:

## Entities

Commonly used HTML entities:

- Non-breaking space: `&nbsp;`
- Less than: `&lt;`
- Greater than: `&gt;`
- Ampersand: `&amp;`
- Quotation mark: `&quot;`
- Apostrophe: `&apos;` (HTML5) or `&#39;` (older HTML versions)
- Copyright: `&copy;`
- Registered trademark: `&reg;`
- Euro sign: `&euro;`
- Pound sign: `&pound;`
- Yen sign: `&yen;`
- Em dash: `&mdash;`
- En dash: `&ndash;`
- Ellipsis: `&hellip;`
- Left single quotation mark: `&lsquo;`
- Right single quotation mark: `&rsquo;`
- Left double quotation mark: `&ldquo;`
- Right double quotation mark: `&rdquo;`
- Multiplication sign: `&times;`
- Right arrow: `&rarr;`

 

