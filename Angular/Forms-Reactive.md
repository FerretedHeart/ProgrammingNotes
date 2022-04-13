# **Forms - Reactive**

# Template-Driven vs Reactive Forms

| Template-driven                                   | Reactive                                                     |
| ------------------------------------------------- | ------------------------------------------------------------ |
| Easy to use                                       | More flexible -> more complex scenarios                      |
| Similar to AngularJS                              | Immutable data model                                         |
| Two-way binding -> Minimal component code         | Easier to perform an action on a value change                |
| Automatically tracks form and input element state | Reactive transformations -> DebounceTime or DistinctUntilChanged |
|                                                   | Easily add input elements dynamically                        |
|                                                   | Easier unit testing                                          |

## Resource

Blog Post: http://blogs.msmvps.com/deborahk/angular-2-reactive-forms-problem-solver/

## State

- Value Changed - pristine / dirty
- Validity - valid / errors
- Visited - touched / untouched

## Model

- Retains form state
- Retains form value
- Retains child controls
  - FormControls
  - Nested FormGroups

## Template-Driven Forms

### Template

- Form element
- Input element(s)
- Data binding
- Validation rules (attributes)
- Validation error messages
- Form model automatically generated

### Component Class

- Properties for data binding (data model)
- Methods for form operations, such as submit

## Reactive Forms

### Component Class

- Form model
- Validation rules
- Validation error messages
- Properties for managing data (data model)
- Methods for form operations, such as submit

### Template

- Form element
- Input element(s)
- Binding to form model

## Directive

| Template-driven (FormsModule) | Reactive (ReactiveFormsModule) |
| ----------------------------- | ------------------------------ |
| ngForm                        | formGroup                      |
| ngModel                       | formControl                    |
| ngModelGroup                  | formControlName                |
|                               | formGroupName                  |
|                               | formArrayName                  |

# Building a Reactive Form

## Form Model

- Root FormGroup

- FormControl for each input element

- ```typescript
  ngOnInit(): void {
  	this.customerForm = new FormGroup({
    		firstName: new FormControl(),
        lastName: new FormControl(),
        email: new FormControl(),
        sendCatalog: new FormControl(true)
  	});
  }
  ```

  

- Nested FormGroups as desired

- FormArrays

## FormBuilder

- Creates a form model from a configuration
- Shortens boilerplate code
- Provided as a service

### Component

- Import FormBuilder

- Inject the FormBuilder instance ('private fb: FormBuilder' in constructor)

- Use the instance

- ```typescript
  ngOnInit(): void {
    this.customerForm = this.fb.group({
    	firstName: '',
    	lastName: '',
    	email: '',
    	setCatalog: true
  	});
  }
  ```

### Benefits of a FormGroup

- Match the value of the form model to the data model
- Check touched, dirty, and valid state
- Watch for changes and react
- Perform cross field validation
- Dynamically duplicate the group

### Module

- Import ReactiveFormsModule
- Add ReactiveFormsModule to the imports array

### Template

- Bind the form element to the FormGroup property

- ```html
  <form (ngSubmit)="save()"
        [formGroup]="customerForm">
  </form>
  ```

- Bind each input element to its associated FormControl

- ```html
  <input id="firstNameId"
         type="text"
         placeholder="First Name (required)"
         formControlName="firstName" />
  ```


# Validation

## Setting Built-in Validation Rules

```typescript
this.customerForm = this.fb.group({
  firstName: ['', [Validators.required, Validators.minLength(3)]],
  sendCatalog: true
})
```

- Import Validators into the component
- After declaring the group item:
  - The first array item is the default value. Most times you enter an empty string.
  - The second array item is the validator. You can add more than one validation within brackets.
  - The third array item is for asynchonous validator

## Adjusting Validation Rules

```typescript
setNotification(notifyVia: string): void {
  const p = this.myFrm.get('phone');
if(notifyVia === 'text') {
  p.setValidators(Validators.required);
} else {
  p.clearValidators();
}
p.updateValueAndValidity();
}
```

- Determine when to make the change
- Use setValidators or clearValidators
- Call updateValueAndValidity

## Custom Validator

```typescript
function myValidator(c: AbstractControl): {[key: string]:boolean} | null {
  if(somethingIsWrong) {
    return {`myValidator`: true};
  }
  return null;
}
```

- Build a validator function
- Use it like any other validator

## Custom Validator with Parameters

```typescript
function myValidator(param: any): ValidatorFn {
  return (c: AbstractControl): {[key: string]: boolean | null => {
    if(somethingIsWrong) {
      return {'myValidator':true};
    }
    return null;
  };
}
```

- Wrap the validator function in a factory function
- Use it like any other validator

## Cross-Field Validation: Nested FormGroup

```typescript
this.customerform = this.fb.group({
  firstName: ['', [Validators.required, Validators.minLength(3)]],
  lastName: ['', [Validators.required, Validators.maxLength(50)]],
  availability: this.fb.group({
    start: ['', Validators.required],
    end: ['', Validators.required]
  })
})
```

- Created a nested FormGroup
- Add FormControls to that FormGroup
- Update the HTML

## Cross-Field Validation: Custom Validator

```typescript
function dateCompare(c: AbstractControl): {[key: string]: boolean} ! null {
  let startControl = c.get('start');
  let endControl = c.get('end');
  if(startControl.value) !== endControl.value) {
    return { 'match': true };
  }
  return null;
}

this.customerform = this.fb.group({
  firstName: ['', [Validators.required, Validators.minLength(3)]],
  lastName: ['', [Validators.required, Validators.maxLength(50)]],
  availability: this.fb.group({
    start: ['', Validators.required],
    end: ['', Validators.required]
  }, {validator: dateCompare}) // add validator to FormGroup
})
```

- Build a custom validator
- Use the validator as an object



# Reacting to Changes

## Watching

Use the valueChanges Observable property

Subscribe to the Observable

```typescript
const phoneControl = this.customerForm.get('phone');
phoneControl.valueChanges.subscribe();
phoneControl.statusChanges.subscribe();
```

(property) AbstractControl.valueChanges: Observable<any>

A multicasting observable that emits an event every time the value of the control changes, in the UI or programmatically.

```typescript
// Allows to observe changes for the individual form items
this.myFormControl.valueChanges.subscribe(value => console.log(value));

// Allows to observe changes for the form group
this.myFormGroup.valueChanges.subscribe(value => console.log(JSON.stringify(value)));

// Allows to observe changes for the entire form
this.customerForm.valueChanges.subscribe(value => console.log(JSON.stringify(value)));
```

## Reacting

You can make adjustments based on the changes:

- Adjust validation rules
- Handle validation messages
- Modify user interface elements
- Provide automatic suggestions

```typescript
this.myFormControl.valueChanges.subscribe(value => this.setNotification(value));
```

## Reactive Transformations

**debounceTime**

- Ignores events until a specific time has passed without another event
- debounceTime(1000) waits for 1000 milliseconds (1 sec) of no events before emitting another event

**throttleTime**

- Emits a value, then ignores subsequent values for a specific amount of time

**distinctUntilChanges**

- Suppresses duplicate consecutive items

Import the operator

```typescript
import { debounceTime } from 'rxjs/operators';
```

Use the operator

```typescript
this.myFormControl.valueChanges.pipe(
	debounceTime(1000)
).subscribe(
	value => console.log(value)
);
```



## Dynamically Duplicate Input Elements

- Define the input element(s) to duplicate
- Define a FormGroup, if needed - nest the elements in the FormGroup
- Refactor to make copies - create a FormGroup in a method
- Create a FormArray
- Loop through the FormArray
- Duplicate the input element(s)

### Creating a FormGroup in a Method

```typescript
buildAddress(): FormGroup {
  return this.fb.group({
    addressType: 'home',
    street1: '',
    street2: '',
    city: '',
    state: '',
    zip: ''
  });
}

this customerForm = this.fb.group({
  ...
  addresses: this.buildAddress()
});
```

### Creating a FormArray

Import FormArray to the component class.

```typescript
this.myArray = new FormArray([...]);

this.myArray = this.fb.array([...]);

addresses: this.fb.array([ this.buildAddress() ])

get addresses(): FormArray{
  return <FormArray>this.customerForm.get('addresses');
}
```

Adjust the template to include the array.

```html
<div formArrayName="addresses" *ngFor="let address of addresses.controls; let i=index">
   <div [formGroupName]="i"> //index
     ...
     <label attr.for="{{ 'street1Id' + i }}"></label> //index
     <input id="{{ 'street1Id' + i }}"> //index
  </div>
</div>
```

### Duplicate the Input Elements

Create a method to add elements to the array.

```typescript
addAddress(): void {
  this.addresses.push(this.buildAddress());
}
```

Add a way to add more elements to the template using the method.

```html
<button class="btn btn-primary" type="button" (click)="addAddress()">
  Add Another Address
</button>
```

