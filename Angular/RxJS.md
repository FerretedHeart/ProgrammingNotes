# **RxJS**

## What is RxJS?

Reactive Extensions for JavaScript

Reactive Extensions were originally developed by Microsoft as Rx.NET

Reactive Extensions has been extended for use for other languages such as RxJava, RxPy (Python), Rx.rb (Ruby), RxJS (JavaScript)

Frameworks such as Angular, React, Vue, etc.

Languages such as JavaScript and TypeScript

### RxJS is a library for composing asynchronous and event-based programs by using observable sequences.

Observe and React to Data as it flows through Time.

Emit items

React to each emitted item

- Transform
- Filter
- Modify

Combine

Cache

RxJS is a library for observing and reacting to data and events by using observable sequences.

## Why RxJS?

One technique to rule them all

Compositional

Watchful - multiple values over time

Lazy - evaluation doesn't start until subscription

Handles errors - built-in error handling

Cancellable

### How is RxJS Used in Angular?

Routing - watch for changes in parameters

Reactive Forms - user changes values

HttpClient - responses

## What is Reactive Development?

A declarative programming paradigm concerned with data streams and the propagation of change.

Code is reactive when an input change leads to an automatic change in output.

# RxJS Terms and Syntax

## Conveyer -> Observable

| Apple Factory                               | RxJS                              |
| ------------------------------------------- | --------------------------------- |
| **Start**                                   | subscribe()                       |
| Emits items                                 | Emits items                       |
| **Item passes through a set of operations** | pipe() through a set of operators |
| **As an observer**                          | Observer                          |
| Next item, process it                       | next()                            |
| Error occurred, handle it                   | error()                           |
| Complete, you're done                       | complete()                        |
| **Stop**                                    | unsubscribe()                     |

**Observer** - a collection of callbacks that knows how to listen to values delivered by the Observable; a consumer of values delivered by an Observable; an interface with next, error, and complete methods

**Subscriber** - an Observer that can unsubscribe from an Observable

**Observable** - a collection of events or values emittd over time

- User actions (key presses)
- Application events
- Response from an HTTP request
- Internal structures
- Can Emit
  - Primitives: numbers, strings, dates
  - Events: mouse, key, valueChanges, routing
  - Objects: customers, products
  - Arrays
  - HTTP response

```typescript
const apples$ = new Observable(appleSubscriber => {
  appleSubscriber.next(`Apple 1`);
  appleSubscriber.next(`Apple 2`);
  appleSubscriber.complete();
});

const observer = {
  next: apple => console.log(`Apple emitted ${apple}`),
  error: err => console.log(`Error occured: ${err}`),
  complete: () => console.log(`No more apples, go home`)
};
```

**Subscribing**

```typescript
const sub = apples$.subscribe(observer);

//Instead use...
const sub = apples$.subscribe({
  next: apple => console.log(`Apple emitted ${apple}`),
  error: err => console.log(`Error occured: ${err}`),
  complete: () => console.log(`No more apples, go home`)
});
```

**Unsubscribing**

- Call complete() on the subscriber
- Use an operator that automatically completes
- Throw an error
- Call unsubscribe() on the subscription

```typescript
sub.unsubscribe();
```

Properly unsubscribing from each Observable helps avoid memory leads.

In Angular, we often work with Observables that Angular creates for us.

**Creating an Observable**

```typescript
const apples$ = of(`Apple1`, `Apple2`); //strings

const apples$ = from([`Apple1`, `Apple2`]); //arrays
```

# RxJS Operators

List of operators: https://rxjs.dev

- Each emitted item can be piped through a set of operators.
  - Transform, filter, process, ...
  - Delay, timeout, ...
- Fashioned after .NET LINQ operators
- Similar to array methods such as filter and map.

An operator is a **function** used to **transform** and **manipulate** emitted items and apply operators in sequence using the Observable's *pipe()* method.

```typescript
of(2, 4, 6) // source observable
	.pipe(
		map(item => item * 2),
		tap(item => console.log(item)),
		take(3)
).subscribe(item => console.log(item));
```

## Operator: map

Used to pipe emitted items through a sequence of operators

map is a **transformation** operator

- Subscribes to its input Observable
- Creates an output Observable

When an item is emitted

- Item is transformed as specified by the provided function
- Transformed item is emitted to the output Observable

## Operator: tap

Used for debugging and performing actions outside of the flow of data (side effects)

tap is a **utility** operator

- Subscribes to its input Observable
- Creates an output Observable

When an item is emitted

- Performs a side effect as specified by a provided function
- Original item is emitted to the output Observable

## Operator: take

Used for 

- Taking a specified number of items
- Limiting unlimited Observables

take is a **filtering** operator

- Subscribes to its input Observable
- Creates an output Observable

When an item is emitted

- Counts the item
  - If <= specified number, emits item to the output Observable
  - When it equals the specified nuber, it completes

Only emits the defined number of items

### map Operator Internals

```typescript
import { Observable } from 'rxjs';

export function map(fn) { // Function
  return (input) => // Takes an input Observable
  	new Observable(observer => { // Creates an output Observable
    return input.subscribe({ // Subscribes to the input Observable
      next: value => observer.next(fn(value)), // Transforms item using provided function and emits item
      error: err => observer.error(err), // Emits error notification
      complete: () => observer.complete() // Emits complete notification
    });
  });
}
```

# Going Reactive

Work with Observables directly

## Async Pipe

- Automatically subscribes to the Observable when component is initialized
- Returns each emitted value
- When a new item is emitted, component is marked to be checked for changes
- Unsubscribes when component is destroyed

```typescript
Product List Component

//products: Product[] = [];
products$: Observable<Product[]>;

constructor(private productServe: ProductService) {}

ngOnInit(): void {
  //this.productService.getProducts()
  	//.subscribe(products => this.products = products);
  this.products$ = this.productService.getProducts();
} 
```

```html
Product List Template

<!--<div *ngIf="products"> -->
<div *ngIf="products$ | async as products"
  <table>
    <tr *ngFor="let product of products">
    	<td>{{ product.productName }}</td>
 			<td>{{ product.productCode }}</td>
    </tr>
  </table>
</div>
```

## Declare Observable properties

```typescript
products$ = this.http.get<Product[]>(this.productUrl)
	.pipe(
		tap(data => console.log(JSON.stringify(data))),
		catchError(this.handleError)
);
```

## Handling Parameters

### Procedural approach: pass parameters

```typescript
getProducts(pageNumber): Observable<Product[]> {
  return this.http.get<Product[]>(this.productsUrl, {
    params: { page: pageNumber }
  });
}
```

### Declarative approach: react to actions

```typescript
products$ = this.requestedPage$
	.pipe(
		switchMap(pageNumber =>
      this.http.get<Product[]>(this.productsUrl, {
      	params: { page: pageNumber }	
    }))
);
```

## Error Handling

### Procedural approach: errors handled in the subscribe

```typescript
this.productService.getProducts()
	.subscribe({
  	next: products => this.products = products,
  	error: err => tis.errorMessage = err
	});
```

### Declarative approach: errors handled in the pipeline

```typescript
product$ = this.productService.products$
	.pipe(
		catchError(err => {
      this.errorMessage = err;
      return EMPTY;
    })
	);
```

#### catchError: catches an error

```typescript
catchError(this.handleError)
```

#### throwError: Throws an error along the subscriber chain

```typescript
throwError(() => new Error('Could not retrieve'));
```

#### EMPTY: Defines an empty Observable that completes

```typescript
return EMPTY;
```



### Change Detection Strategies

|                               Default | OnPush                                                     |
| ------------------------------------: | ---------------------------------------------------------- |
| Uses the default checkAlways strategy | Improves performance by minimizing change detection cycles |
|      Every component is checked when: | Component is only checked when:                            |
|              - Any change is detected | - @Input properties change                                 |
|                                       | - Event emits                                              |
|                                       | - A bound Observable emits                                 |

```typescript
@Component({
  templateUrl: `./product-list.component.html`,
  changeDetection: ChangeDetectionStrategy.OnPush
})
```

