[TOC]

# What is a Component?

- Template
  - View layout
  - Created with HTML
  - Including binding and directives
- Class
  - Code supporting the view
  - Created with TypeScript
  - Properties: data
  - Methods: logic
- Metadata
  - Extra data for Angular
  - Defined with a decorator
    - Decorator is a function that adds metadata to a class, its members, or method arguments
    - It is prefixed with a @ and is above the class

# Importing What We Need

Before we use an external function or class, we define where to find it.

Allows us to use exported members from:

- Other files in our application
- Angular framework
- External JavaScript libraries

## import keyword

- Defines where to find the members that this component needs
- Member name
  - Correct spelling/casing
- Path
  - Enclose in quotes
  - Correct spelling/casing

```typescript
import { Component } from '@angular/core';
```

## export keyword

- Use PascalCasing
- Append "Component" to the  name

### Data in properties

- Appropriate data type
- Appropriate default value
- camelCase with first letter lowercase

### Logic in methods

- camelCase with first letter lowercase

```typescript
export class AppComponent {
	pageTitle: string = 'Acme Product Management';
  showImage: boolean = false;	// property
  toggleImage() : void {	// method
    this.showImage = true;
  }
}
```

## Metadata

### Component decorator

- Prefix with @
- Suffix with ( )

selector: Component name in HTML

- Prefix for clarity

template: View's HTML

- Correct HTML syntax

```typescript
@Component({
	selector: 'pm-root',
	template: `
		<div><h1>{{pageTitle}}</h1></div>
		`
})
```

# Improving Our Components

- Strong typing and interfaces
- Encapsulating styles
- Lifecycle hooks
- Custom pipes
- Nested components

An interface is a specification identifying a related set of properties and methods.

## Two Ways to Use an Interface

### Interface as a Data Type

- interface keyword
- Properties and their types
- Export it
- Use the interface as a data type

```typescript
export interface IProduct {
	productId: number;
	productName: string;
	productCode: string;
	releaseDate: string;
	price: number;
	description: string;
	starRating: number;
	imageUrl: string;
}
```

```html
products: IProduct[] = [];
```

### Interface as a Feature Type

- implements keyword and interface name
- Write code for each property and method

```typescript
export interface DoTiming {
	count: number;
	start(index: number): void;
	stop(): void;
}

export class myComponent implements DoTiming {
  count: number = 0;
  start(index: number): void {
    ...
  }
  stop() : void {
    ...
  }
}
```

# Handling Unique Component Styles

- Templates sometimes require unique styles
- We can inline the styles directly into the HTML
- We can build an external stylesheet and link it in index.html

# Encapsulating Component Styles

styles

```typescript
@Component({
	selector: 'pm-products',
	templateUrl: './product-list.component.html',
	styles: ['thead {color: #337AB7}']
})
```

styleUrls

```typescript
@Component({
	selector: 'pm-products',
	templateUrl: './product-list.component.html',
	styleUrls: ['./product-list.component.css']
})
```

You can add more than one stylesheet, separated by commas.

# Component Lifecycle

Create -> Render -> Create and render children -> Process changes -> Destroy

A lifecycle hook is an interface we implement to write code when a component lifecycle event occurs.

## Lifecycle Hooks

- **OnInit**: Perform component initialization, retrieve data
- **OnChanges**: Perform action after change to input properties
- **OnDestroy**: Perform cleanup 

# What Makes a Component Nest-able?

- Its template manages a fragment of a larger view
- It has a selector
- It optionally communicates with its container

## Input Properties

- Input decorator

- Attach to a property of any type

- Prefix with @; Suffix with ()

- ```typescript
  export class StarComponent {
  	@Input() rating: number;
  }
  ```

## Output Properties

- Output decorator

- Attached to a property declared as an EventEmitter

- Use the generic argument to define the event data type

- Use the new keyword to create an instance of the EventEmitter

- Prefix with @; Suffix with ()

- ```typescript
  export class StarComponent {
  	@Output() notify: EventEmitter<string> = new EventEmitter<string>();
  }
  ```

## Container Component

- Use the directive

  - Directive name -> nested component's selector

- Use property binding to pass data to the nested component

- Use event binding to respond to events from the nested component

  - Use $event to access the event data passed from the nested component

- ```html
  <pm-star [rating]='product.starRating'
  				 (notify)='onNotify($event)'>
  </pm-star>
  ```

  

