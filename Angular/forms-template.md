# **Forms - Template Driven**

# Form Technologies

| Template-Driven Forms      | Reactive Forms                                               |
| -------------------------- | ------------------------------------------------------------ |
| Use a component's template | Use a component's template                                   |
| Unit Test against DOM      | Create a form model in TypeScript (this mus tbe in sync with the template) |
|                            | Unit Test against form model                                 |
|                            | Validation in form model                                     |

# Form Basics

- Use Angular's FormsModule
  - Import in App Module
- Create a Form Component
- Bootstrap for Styling
  - Add npm package
  - Add styles section of angular.json
- Add checkboxes, radios, select control, options
- Other form controls: dates, colors, passwords, and any input type

# Data Binding

- Add #form="ngForm" to the form directive
- Add Interface for the field names as the data model
- Add [(NgModel)]="<interface>.<name>" to the form fields

# Form Validation

## Attributes

- required - field needs a value
- pattern - regular expression - ex. B.\* - asterisk is a wildcard
- minlength/maxlength - minimum or maximum length of a string
- min/max - used for number values

## Classes

- ng-untouched / ng-touched - whether the field has been typed/clicked/mouseover, etc
- ng-pristine / ng-dirty - value has changed
- ng-valid / ng-invalid - based on attributes

## NgModel Properties

- Based on classes without "ng-"

## Styling and Submitting Forms

- (ngSubmit)="onSubmit()";
- form.submitted flag

## Handling Form Control Events

```html
(blur)="onBlur(nameField)
```

```typescript
onBlur(field: NgModel) {
    console.log('in onBlur: ', field.valid);
  }
```

# HTTP Form Posting and Data Access

- Create a Data Service
  - @Injectable()
  - providedIn: 'root'
- Post Forms using Observables
  - observable.subscribe(success, error)
- HTTP Access Using HttpClient
  - Uses HttpClientModule
  - Inject HttpClient in data service
- Post a Form using HttpClient
  - http.post(url, data)
- Handling POST Errors
  - Inform user
- Retrieve Data for Select Elements
  - Use async pipe
  - \*ngFor="let item of items | async"
  - items must be an Observable

# Third-Party Form Controls

Form Resources at angular.io

- Angular Material
- Prime Faces
- ngx-bootstrap

Installing and Using ngx-bootstrap

- ng add
- Import Module

Buttons

Dates and Date Ranges

Time Control

Rating Control