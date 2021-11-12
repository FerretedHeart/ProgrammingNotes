# **Templates and Directives**

# Defining a Template in a Component

## Inline Template (quotes)

```typescript
template: "<h1>{{pageTitle}}</h1>"
```

## Inline Template (back ticks)

```typescript
template: `
	<div>
		<h1>{{pageTitle}}</h1>
  	<div>
  		My First Component
  	</div>
  </div>
  `
```

- Specify the template property
- Use the ES 2015 back ticks for multiple lines
- Watch syntax

## Linked Template

```typescript
templateUrl: './product-list.component.html'
```

- Specify the templateUrl property
- Define the path to the HTML file

# Binding

Coordinates communication between the component's class and its template and often involves passing data.

# Interpolation

- One way binding
  - From component class property to an element property

- Defined with double curly braces
  - Contains a template expression
  - No quotes needed

Class - Define the property

```typescript
export class AppComponent {
	pageTitle: string = 'Acme Product Management';
}
```

Template - Use the property

```html
<h1>{{pageTitle}}</h1>
{{'Title: ' + pageTitle}}
{{2*20+1}}
<h1 innerText={{pageTitle}}></h1>
```

## Expression Restrictions

- Assignments (=, +=, ++, etc)
- new Keyword
- Expression Chaining with ;
- Global Namespace

## Expression Recommendations

- No Side-Effects
- Fast
- Simple
- Idempotent

# Property Bindings

Property is within brackets and the expression within quotes.

```html
<img [src]="user.imageUrl"/>
```



# Event Bindings

Events for when an action is taken such as clicking on a button.

Event is within parenthesis and the action is within quotes.

```html
<button (click)="doSomething()">
</button>
```

## Statement Restrictions

- Assignments Except = (+=, ++, etc)
- new Keyword
- Global Namespace

# Directive

Custom HTML element or attribute used to power up and extend our HTML.

- Custom

- Built-in

  - Structural Directives

    - ### *ngIf: If logic

    - Show or hide based on the condition

    - ```html
      <div class='table-responsive'>
      	<table class='table' *ngIf='products.length'>
      		<thead></thead>
      		<tbody></tbody>
      	</table>
      </div>
      ```

      

    - ### *ngFor: For loops

    - Loop through elements to display

    - ```html
      <tr *ngFor='let product of products'>
      	<td></td>
      	<td>{{ product.productName }}</td>
      </tr>
      ```

    - #### for...of

      - Iterates over iterable objects, such as an array

      - ```typescript
        let nicknames=['di', 'boo', 'punkeye'];
        for (let nickname of nicknames) {
        	console.log(nickname);
        }
        ```

      - Result: di, boo, punkeye

    - #### for...in

      - Iterates over the <u>properties</u> of an object

      - ```typescript
        let nicknames=['di', 'boo', 'punkeye'];
        for (let nickname in nicknames) {
        	console.log(nickname);
        }
        ```

      - Result: 0, 1, 2

