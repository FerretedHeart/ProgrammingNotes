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

