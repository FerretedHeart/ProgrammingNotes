# Forms

[React documentation for Forms](https://reactjs.org/docs/forms.html#gatsby-focus-wrapper)

## State Management in HTML Forms

1. **Form elements have internal state** - Different types of form elements keep various types of form state
2. **DOM handles the state data** - The user input is stored on the DOM itself

## State Management in React Forms

1. **Controlled forms** - React component maintains state
2. **Uncontrolled forms** - DOM maintains state

## Controlled Forms

1. React component maintains the state of the form
2. The component sets the state of the form element during rendering using its own state
3. User input is sent back to the component using callbacks on form elements
4. The component updates itself on these callbacks and re-renders the form to update the UI

## Controlling `<input>` Form Element

- State is set using the value/checked attribute on the tag
- onChange callback allows listening for changes in user inputs and keeps state in sync
- Input type radio and checkbox us the checked attribute and all other input tags use the value attribute
- Setting the value attribute on an input tag prevents user from changing the input. It can now only be done via the onChange callback

## Controlling `<textarea>` Form Element

- State is set using the value attribute like input tag
- textarea tag in React need not have any children unlike in HTML
- onChange callback received on every user input

## Controlling `<select>` Form Element

- No selected attribute needed in React
- Selection is determined by value property on the select tag
- onChange callback received when user changes selection
- The option whose value attribute matches the value set on the select tag will be selected

## Event Flow for a Controlled Form

- **Component initialization** - Initial form state provided in the state property
- **Component rendering** - Value for the form elements is picked from the component state and set using the value property
- **User input** - onChange callback on form element triggered with new value
- **Change callback** - Update the component state with new value using setState
- Component re-renders setState with new value triggers component re-render and form updated with new value

## Advantages of Controlled Forms

The recommended way of handling forms in React is to create Controlled forms.

- **Instant feedback** - Show in-place errors to users as they type
- **Disable UI conditionally** - Disable portions on UI based on current user input
- **Format user input** - Show user input in specific formats e.g. credit cards and phone numbers

## Data Validation

- Input
  - The validation logic is executed in the onChange callbacks
  - Set an error object for the input in the component's state using setState
  - Conditionally render the error message based on the state
- On Submit
  - The validation logic is executed in the onSubmit callback for the form
  - Set an error object for all invalid inputs in the component's state
  - Conditionally render the error message based on the state and prevent form submission

## Controlled Forms with Functions

To keep all form field data maintained, we `useState` and contain the elements' data in an object.

```react
const [formData, setFormData] = React.useState(
        {firstName: "", lastName: ""}
    )
    
    console.log(formData)
    
    function handleChange(event) {
        setFormData(prevFormData => {
            return {
                ...prevFormData,
                [event.target.name]: event.target.value
            }
        })
    }
    
    return (
        <form>
            <input
                type="text"
                placeholder="First Name"
                onChange={handleChange}
                name="firstName"
            />
            <input
                type="text"
                placeholder="Last Name"
                onChange={handleChange}
                name="lastName"
            />
        </form>
    )
}
```

Where the React state is what drives the values within the fields since fields can keep their own states.

```react
return (
        <form>
            <input
                type="text"
                placeholder="First Name"
                onChange={handleChange}
                name="firstName"
                value={formData.firstName} // update value with state value
            />
            <input
                type="text"
                placeholder="Last Name"
                onChange={handleChange}
                name="lastName"
                value={formData.lastName}
            />
            <input
                type="email"
                placeholder="Email"
                onChange={handleChange}
                name="email"
                value={formData.email}
            />
        </form>
    )
```

`<input>` and `<textarea>` work the same as self-closing tags. `checkbox` which is an input type, will use `checked` instead of `value`.

```react
function handleChange(event) {
        const {name, value, type, checked} = event.target //destructure object for event.target
        setFormData(prevFormData => {
            return {
                ...prevFormData,
                [name]: type === "checkbox" ? checked : value // ternary operator to verify type
            }
        })
    }
    
......

<textarea 
                value={formData.comments} // this is value
                placeholder="Comments"
                onChange={handleChange}
                name="comments"
            />
            <input 
                type="checkbox" 
                id="isFriendly" 
                checked={formData.isFriendly} // while this is checked
                onChange={handleChange}
                name="isFriendly"
            />
            <label htmlFor="isFriendly">Are you friendly?</label>
```

`radio` input type will work similiarly to `checkbox` , only the difference is that all buttons will have the same name, while `checked` will maintain control and the `value` will be hard-coded.

```react
<fieldset>
  <legend>Current employment status</legend>

  <input 
    type="radio"
    id="unemployed" // id and htmlFor on label need to match
    name="employment"
    value="unemployed"
    checked={formData.employment === "unemployed"}
    onChange={handleChange}
    />
  <label htmlFor="unemployed">Unemployed</label>
  <br />

  <input 
    type="radio"
    id="part-time"
    name="employment"
    value="part-time"
    checked={formData.employment === "part-time"}
    onChange={handleChange}
    />
  <label htmlFor="part-time">Part-time</label>
  <br />

  <input 
    type="radio"
    id="full-time"
    name="employment"
    value="full-time"
    checked={formData.employment === "full-time"}
    onChange={handleChange}
    />
  <label htmlFor="full-time">Full-time</label>
  <br />

</fieldset>
```

Dropdown lists are layed out a little differently but the `name` , `value`, and `onChange` still work the same.

```react
<label htmlFor="favColor">What is your favorite color?</label>
<br />
<select 
  id="favColor"
  value={formData.favColor}
  onChange={handleChange}
  name="favColor"
  >
  <option value="">-- Choose --</option>
  <option value="red">Red</option>
  <option value="orange">Orange</option>
  <option value="yellow">Yellow</option>
  <option value="green">Green</option>
  <option value="blue">Blue</option>
  <option value="indigo">Indigo</option>
  <option value="violet">Violet</option>
</select>
```

To submit the form, put a button within the form element and it will be treated as a submit type by default. The form element is then updated to handle a submit function.

```react
function handleSubmit(event) {
        event.preventDefault() //prevents the data from resetting and retains the values in state
        console.log(formData)
    }
    
    return (
        <form onSubmit={handleSubmit}> //onSubmit with function
```

# Formik

[Formik Documentation](https://formik.org/docs/overview)

Formik is a library that solves all vanilla React problems with writing controlled React forms.

## Problems with Using Vanilla React for Forms

- Very verbose
- Repetitive code for maintaining state and handling callbacks
- Error prone

## Features of Formik

- Automatically keeps track of values, errors, and visited fields
- Automatically hooks up appropriate callback functions on form elements
- Provides helpers for sync and async validations and showing error messages
- Provides sensible defaults for common functionalities while allowing maximum customization

## Setting up Formik

- To install formik run: `npm install formik --save`
- You don't need to migrate all forms in your app at once
- Replace the form and form element tags in your components with respective Formik components

## Key Formik Components

`<Formik>`

- This is the top-level component which controls all other components
- Responsible for keeping track of state for the field components using their `name` attribute as key
- Responsible for injecting and handling callbacks for field components
- Provides properties to set initial values, validation function and handle callbacks like submit and reset
- Allows rendering via children and custom components
- Maintains three maps to automatically track the state of all the `<Field>` tags it contains
  - **values** - Map of the `<Field>` tags' name to its user input value
  - **errors** - Map of the `<Field>`  tags' name to the error string returned by validation functions
  - **touched** - Map of the `<Field>` tags' name to a boolean indicating whether it was ever focused or not

 `<Form>`

- Wrapper around HTML `<form>` tag
- Automatically hooks parent Formik's on submit and reset callbacks into the HTML form
- Extra props are passed to the DOM directly

 `<Field>`

- By default it renders a HTML `<input>` tag
- Uses the name attribute on the `<Field>` to match up with the Formik state
- `<Formik>` will automatically inject onChange, onBlue, and value attributes for all `<Field>` tags
- To render a non-input tag set the `as` attribute of the `<Field>` tag to textarea, select, or any custom component
- Allows for field level validation via the validate attribute

`<ErrorMessage>`

- Represents error message for the `<Field>` tag whose `name` attribute matches with its own `name` attribute
- The message to render is always expected to be a string
- Fully custom rendering supported via children, component and render properties

```react
// Anatomy of a Formik Form

<Formik
  initialValues={{ username: 'jared' }}
  validate={(values, props) => {
    // ..validation logic
  }}
  onSubmit={(values, actions) => {
    const uname = values.username
  }}>
  
  {props => (
    <Form>
    	<label htmlFor="username">First Name</label>
    	<Field name="username" type="text" />
    	<ErrorMessage name="username" />
    	<button type="submit">Submit</button>
    </Form>
  )}
</Formik>
```

Even if your form is empty by default, you must initialize all fields with initial values. Otherwise React will throw an error saying that you have changed an input from uncontrolled to controlled.

## Key `<Formik>` Helper Props

1. **isSubmitting** - Indicates if form is under submission
2. **setSubmitting(bool)** - Set a form's submission phase
3. **isValid** - Indicates if form input is valid
4. **isValidating** - Indicates if form input is being validated
5. **resetForm(values)** - Reset form using given values

## Handling Form Submission

- On form submission, the onSubmit callback on the `<Formik>` tag is invoked with the field values and validations are run
- The onSubmit can be sync or it can be async and return a Promise
- In sync onSubmit call setSubmitting(false) to indicate end of submission

