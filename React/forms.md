# Forms

[React documentation for Forms](https://reactjs.org/docs/forms.html#gatsby-focus-wrapper)

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

## Controlled Components

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

