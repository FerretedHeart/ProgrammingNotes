# Forms

The `<form>` element is a great tool for collecting information, but then we need to send that information somewhere else for processing. We need to supply the `<form>` element with both the location of where the `<form>`'s information goes and what HTTP request to make.

```html
<form action="/example.html" <!-- determines where the information is sent -->
      method="POST"> <!-- is assigned a HTTP verb that is included in the HTTP request -->
	<h1>Creating a form</h1>
	<p>Looks like you want to learn how to create an HTML form. Well, the best way to learn is to play around with it.</p>
	
	<label for="meal">What do you want to eat?</label>
	<input type="text" name="food" id="meal" value="already-prefilled" />
</form>
```

The `<input>` element has a `type` attribute which determines how it renders on the web page and what kind of data it can accept.

- `type` refers to the kind of information being entered
  - `text` renders a single line text field that users can type into
  - `password` will replace input text with another character like an asterisk or dot to hide sensitive information
  - `number` renders a field that users that restricts users to entering only numbers
  - `range` creates a glider, along with attributes `min` and `max`
  - `checkbox` for multiple choices and displays a box with assigned `value`
  - `radio` for selecting only one option
  - `submit` which renders a button for sending the form
  - `date` will render a date picker
  - `time` will render a time picker
  - `color` will render a color palette
  - `file` will allow to upload a file by providing a button to be able to choose a file from your local device storage
- `name` - name of the input field, which is required
- `value` - what is typed into the text field, which can start with a default value that displays within the input field
- `step` - for number types, creates arrows inside the field to increase or decrease the value

The `<label` element has an opening and closing tag and displays text that is written between the opening and closing tags. To associate a `<label>` and an `<input>` , the `<input>` need an `id` attribute. We then assign the `for` attribute of the `<label>` element with the value of the `id` attribute of `<input>`.



## Dropdown List

If we want users to pick one option from a few visible options, dropdown list is a great alternative to radio and check buttons.

```html
<label for="lunch">What's for lunch?</label>
  <select id="lunch" name="lunch">
    <option value="pizza">Pizza</option>
    <option value="curry">Curry</option>
    <option value="salad">Salad</option>
    <option value="ramen">Ramen</option>
    <option value="tacos">Tacos</option>
  </select>
```



## Datalist Input

If the list has a lot of options, it can be tedious for users to scroll through the entire list to locate one option. The `<datalist>` is used with an `<input type="text">` element. The `<input>` creates a text field that user can type into and filter options from the `<datalist>`.

```html
<form>
  <label for="city">Ideal city to visit?</label>
  <input type="text" list="cities" id="city" name="city">
 
  <datalist id="cities">
    <option value="New York City"></option>
    <option value="Tokyo"></option>
    <option value="Barcelona"></option>
    <option value="Mexico City"></option>
    <option value="Melbourne"></option>
    <option value="Other"></option>  
  </datalist>
</form>
```



## Textarea Element

The `<textarea>` element is used to create a bigger text field for users to write more text. We can add the attributes `rows` and `cols` to determine the amount of rows and columns for the field.

```html
<form>
  <label for="blog">New Blog Post: </label>
  <br>
  <textarea id="blog" name="blog" rows="5" cols="30">
    Adding default text
  </textarea>
</form>
```



## Validations

Validation is the concept of checking user provided data against the required data. *Client-side validation* is used to check the data on the browser (the client). This validation occurs before data is sent to the server for *server-side validation*. Different browsers display messages differently but basically work the same.

Attributes:

- `required` to enforce the rule that information must be provided before submitted
- `min/max` for number type fields
- `minlength/maxlength` for text type fields
- `pattern` for text type fields using *regular expression* which is a sequence of characters that make up a search pattern