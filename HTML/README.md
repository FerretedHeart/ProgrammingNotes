# HTML

HTML is organized into a family tree structure. HTML elements can have parents, grandparents, siblings, children, grandchildren, etc.

```html
<!--Standard structure -->

<!DOCTYPE html>
<html>
  <head>
    <title>Title of the document</title>
  </head>
  <body>
    <div>
      <h1>Heading 1</h1>
      <h2>Heading 2</h2>
    </div>
  </body>
</html>
```

Blocks: `<div></div>`

Paragraphs: `<p>This is a paragraph.</p>`

Breaks: `<br/>`

Images: `<img src="..." alt=""/>`

Videos: `<video src="..." width="10" height="10" controls></video>`

Links: `<a href="..." target="_blank">This will go to a link.</a>`

Lists:

```html
<!-- Ordered lists -->
<ol>
  <li>One</li>
  <li>Two</li>
</ol>

<!-- Unordered lists -->
<ul>
  <li>One</li>
  <li>Two</li>
</ul>
```



[Forms](HTML/forms.md)



## Semantic HTML

Semantic elements provide information about the content between the opening and closing tags.

- `<header>` - container using for either navigational links or introductory content containing `<h1>` to `<h6>` headings
- `<nav>` - used to define a block of navigation links such as menus and tables of contents and can be used inside of the `<header>` element
- `<main>`- used to encapsulate the dominant content within a webpage
- `<footer>`- content at the bottom of the subject information such as contact and copyright information, terms of use, etc.
- `<section>`- defines elements in a document such as chapters, headings, or any other area of the document with the same theme
- `<article>`- holds content such as articles, blogs, comments, magazines, etc.
- `<aside>`- used to mark additional information that can enhance another element but isn't required to understand the main content
- `<figure>`- used to encapsulate media such as an image, illustration, diagram, code snippet, etc which is referenced in the main flow of the document
- `<figcaption>`- used to describe the media in the `<figure>` tag
- `<audio>`- used to embed audio content into a document, and contain attributes much like `<video>`
  - `controls`: automatically displays the audio controls into the browser such as play and mute
  - `src`: specifies the URL of the audio file
  - `type`: what kind of audio it is
  - [other attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio#attributes)
- `<video>`- add videos to website along with attributes
  - `controls`: play/pause button will be added along with volume
  - `autoplay`: results in a video automatically playing as soon as the page is loaded
  - `loop`: results in the video continuously playing on repeat
- `<embed>`- used to incorporate media content into a page and is a self-closing tag (deprecated)
