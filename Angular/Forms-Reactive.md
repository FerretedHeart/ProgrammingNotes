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

  