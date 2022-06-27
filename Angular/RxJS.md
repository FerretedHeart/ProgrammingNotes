# **RxJS**

[TOC]



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

RxJS is a library for **observing** and **reacting** to data and events by using observable sequences.

## Why RxJS?

One technique to rule them all - works with all types of data and data sources

Compositional

Watchful - multiple values over time

Lazy - evaluation doesn't start until subscription

Handles errors - built-in error handling

Cancellable

### How is RxJS Used in Angular?

Routing - watch for changes in parameters

Reactive Forms - user changes values

HttpClient - responses from backend servers

## What is Reactive Development?

A **declarative** programming paradigm concerned with **data streams** and the propagation of change.

Code is **reactive** when an **input change** leads to an **automatic change in output**.

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

**Observer** - a collection of **callbacks** that knows how to listen to values delivered by the Observable; a **consumer** of values delivered by an Observable; an **interface** with next, error, and complete methods

**Subscriber** - an Observer that can unsubscribe from an Observable

**Observable** - a collection of events or values emitted over time

- User actions (key presses)
- Application events (routing, forms)
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

Properly unsubscribing from each Observable helps avoid memory leaks.

In Angular, we often work with Observables that Angular creates for us.

**Creating an Observable**

```typescript
const apples$ = of(`Apple1`, `Apple2`); //individual items i.e. strings or numbers

const apples$ = from([`Apple1`, `Apple2`]); //arrays

@ViewChild('para') par: ElementRef;
ngAfterViewInit() {
  const parStream = fromEvent(this.par.nativeElement, 'click').subscribe(console.log)
}

const num = interval(1000).subscribe(console.log);
```

### Cold vs. Hot Observables

| Cold Observables                             | Hot Observables                                              |
| -------------------------------------------- | ------------------------------------------------------------ |
| Value producer created inside the observable | Value producer exists outside the observable                 |
| One observer per execution                   | Shared producer allows for multiple observers                |
| Unicast                                      | Multicast                                                    |
| Examples include interval(), ajax()          | Examples include Observables that wrap DOM events, Websockets |



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

Used for debugging and performing actions outside of the flow of data (side effects) as it doesn't affect the data.

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
  - When it equals the specified number, it completes

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

## Multicasting Operators

### multicast()

- Takes a Subject as a parameter
- Must call connect() to begin execution

### refCount()

- Executes when observers > 0

### publish()

- Thin wrapper about multicast()
- Not required to pass it a Subject

### share()

- Executes when observers > 0
- Re-subscribes as necessary

# Going Reactive

Work with Observables directly

## Async Pipe

- Automatically subscribes to the Observable when component is initialized
- Returns each emitted value
- When a new item is emitted, component is marked to be checked for changes
- Unsubscribes when component is destroyed

Benefits of an Async Pipe

- No need to subscribe
- No need to unsubscribe
- Improve change detection

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

#### catchError: Catches any errors that occur on an Observable

```typescript
catchError(this.handleError)
```

Must be after any operator that could generate an error.

Used for catching errors

In the error handler:

- Rethrow the error
- Replace the errored Observable to continue after an error occurs

#### throwError: Creates an Observable that emits no items

Immediately emits an error notification

```typescript
throwError(() => new Error('Could not retrieve'));
```

Used for: Propagating an error

Or use the throw statement

#### EMPTY: Is an Observable that emits no items

```typescript
return EMPTY;
```

Immediately emits a complete notification

Used for: Returning an empty Observable

### Replacing an Errored Observable

An Observable created from hard-coded or local data

An Observable that emits an empty value or empty array

The EMPTY RxJS constant

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

### Benefits of a Declarative Approach

- Leverages the power of RxJS Observables and operators
- Combine data streams
- React to user actions

# Mapping Returned Data

When retrieving a set of data, HTTP get returns an Observable that emits an array

```typescript
products$ = this.http.get<Product[]>(this.productsUrl);
```

Mapping the Emitted Array

Mapping Array Elements

Transforming Array Elements

```typescript
products$ = this.http.get<Product[]>(this.productsUrl)
	.pipe(
		map(products => 
       products.map(product => ({
      	...product,
      	price: product.price * 1.5
    } as Product))),
	catchError(this.handleError)
);
```

# Combining Data Streams

Why Combine Data Streams?

- Map id to a string
- Work with multiple data sources
- React to actions
- Simplify template code

Types of Combination Operators/Functions

- Combine to a single Observable result (merge, concat)
- Flatten higher-order Observables (mergeAll)
- Emit a combined value (combineLatest, forkJoin, withLatestFrom)

## combineLatest

Creates an Observable whose vallues are defined:

- Using the **latest** values from each input Observable

- ```typescript
  combineLatest([a$, b$, c$])
  ```

Static creation function, not a pipeable operator

combineLatest is a **combination function**

- Takes in a set of Observables, subscribes
- Creates an output Observable

When an item is emitted from any Observable

- If all Observables have emitted at least once
- Emits a value to the output Observable

Emitted value combines the **latest** emitted value from each input Observable into an array

Completes when all input Observables complete

Use combineLatest

- To work with multiple data sets
- To reevaluate state when an action occurs

## forkJoin

Creates an Observable whose value is defined:

- Using the **last** value from each input Observable

- ```typescript
  forkJoin([a$, b$, c$])
  ```

Only emits one time, when all input Observables complete

Static creation function, not a pipeable operator

forkJoin is a **combination function**

- Takes in a set of Observables, subscribes
- Creates an output Observable

When an item is emitted from any Observable

- If all Observables have emitted at least once
- Emits a value to the output Observable

Emitted value combines the **last** emitted value from each input Observable into an array

Use forkJoin

- To wait to process any results until all Observables are complete
- When emitting individual items you'd likek emitted in a single array
- Don't use when working with Observables that don't complete such as action streams

## withLatestFrom

Creates an Observable whose vallues are defined:

- Using the **latest** values from each input Observable

- But only when the **source** Observable emits

- ```typescript
  a$.pipe(withLatestFrom(b$, c$))
  ```

Pipeable operator

withLatestForm is a **combination function**

- Takes in a set of Observables, subscribes
- Creates an output Observable

When an item is emitted from the **source** Observable

- If all Observables have emitted at least once
- Emits a value to the output Observable

Emitted value combines the **latest** emitted value from each input Observable into an array

Completes when the source Observables completes

Use withLatestForm

- To react to changes in only one Observable
- To regulate the output of the other Observables

## 
