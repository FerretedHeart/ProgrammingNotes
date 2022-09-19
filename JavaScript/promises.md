# Promises

Promises are objects that represent the eventual outcome of an asynchronous operation. A `Promise` object can be in one of three states:

- **Pending**: The initial state - the operation has not completed yet.
- **Fulfilled**: The operation has completed successfully and the promise now has a resolved value. For example, a request's promise might resolve with a JSON object as its value.
- **Rejected**: The operation has failed and the promise has a reason for the failure. This reason is usually an `Error` of some kind.

We refer to a promise as *settled* if it is no longer pending - it is either fulfulled or rejected.

## Constructing a Promise Object

To create a new `Promise` object, we use the `new` keyword and the `Promise` constructor method:

```javascript
const executorFunction = (resolve, reject) => {
  if (someCondition) {
      resolve('I resolved!');
  } else {
      reject('I rejected!'); 
  }
}
const myFirstPromise = new Promise(executorFunction);
```

The `Promise` constructor method takes a function parameter called the *executor function* which runs automatically when the constructor is called. The exeecutor function generally starts an asynchronous operation and dictates how the promise should be settled.

The executor function has two function parameters, usually referred to as the `resolve()` and `reject()` functions. The `resolve()` and `reject()` functions aren't defined by the programmer. When the `Promise` constructor runs, JavaScript will pass its own `resolve()` and `reject()` functions into the executor function.

- `resolve` is a function with one argument. Under the hood, if invoked, `resolve()` will change the promise's status from `pending` to `fulfilled`, and the promise's resolved value will be set to the argument passed into `resolve()`.
- `reject` is a function that takes a reason or error as an argument. Under the hood, if invoked, `reject()` will change the promise's status from `pending` to `rejected`, and the promise's rejection reason will be set to the argument passed into `reject()`.

## The Node setTimeout() Function

`setTimeout()` is a Node API that uses callback functions to schedule tasks to be performed after a delay. `setTimeout()` has two parameters: a callback function and a delay in milliseconds.

```javascript
const delayedHello = () => {
  console.log('Hi! This is an asynchronous greeting!');
};
 
setTimeout(delayedHello, 2000);
```

We invoke `setTimeout()` with the callback function `delayedHello()` and `2000`. In at least two seconds `delayedHello()` will be invoked. The delay is performed asynchronously - the rest of our program won't stop executing during the delay. Asynchronous JavaScript uses something called *the event loop*. After two seconds, `delayedHello()` is added to a line of code waiting to be run. Before it can run, any synchronous code from the program will run. Next, any code in front of it in the line will run. This means it might be more than two seconds before `delayedHello()` is actually executed.

```javascript
const returnPromiseFunction = () => {
  return new Promise((resolve, reject) => {
    setTimeout(( ) => {resolve('I resolved!')}, 1000);
  });
};
 
const prom = returnPromiseFunction();
```

## Consuming Promises

Promise objects come with an aptly named `.then()` method. It allows us to say, "I have a promise, when it settles, **then** here's what I want to happen...".

`.then()` is a higher-order function - it takes two callback functions as arguments. We refer to these callbacks as *handlers*. When the promise settles, the apropriate handler will  be invoked with that settled value.

- The first handler, sometimes called `onFulfilled`, is a *success handler*, and it should contain the logic for the promise resolving.
- The second handler, sometimes called `onRejected`, is a *failure handler*, and it should contain the logic for the promise rejecting.

We can invoke `.then()` with one, both, or neither handler. One important feature of `.then()` is that it always returns a promise.

## Success and Failure Callback Functions

To handle a resolved or rejected promise, we invoke `.then()` on the promise, passing in a success or failed handler callback function:

```javascript
let prom = new Promise((resolve, reject) => {
  let num = Math.random();
  if (num < .5 ){
    resolve('Yay!');
  } else {
    reject('Ohhh noooo!');
  }
});
 
const handleSuccess = (resolvedValue) => {
  console.log(resolvedValue);
};
 
const handleFailure = (rejectionReason) => {
  console.log(rejectionReason);
};
 
prom.then(handleSuccess, handleFailure);
```

- `prom` is a promise which will resolve to `'Yay!'`.
- We define a function, `handleSucccess()`, which prints the argument passed to it.
- We invoke `prom`'s `.then()`' function passing in our `handleSuccess()` function.
- Since `prom`  resolves, `handleSuccess()` is invoked with `prom`'s resolved value, `Yay`, so `Yay` is logged to the console.
- If it is rejected, it will log `Ohh noooo!` to the console in the same fashion.

## Using catch() with Promises

Separation of concerns means organizing code into distinct sections each handling a specific task. `.then()` will return a promise with the same settled value as the promise it was called on if no appropriate handler was provided. This implementation allows us to separate our resolved logic from our rejected logic. Instead of passing both handlers into one `.then()`, we can chain a second `.then()` with a failure handler to a first `.then()` with a success handler and both cases will be handled.

```javascript
prom
  .then((resolvedValue) => {
    console.log(resolvedValue);
  })
  .then(null, (rejectionReason) => {
    console.log(rejectionReason);
  });
```

The `.catch()` function takes only one argument, `onRejected`. In the case of a rejected promise, this failure handler will be invoked with the reason for rejection. Using `.catch()` accomplishes the same thing as using a `.then()` with only a failure handler.

```javascript
prom
  .then((resolvedValue) => {
    console.log(resolvedValue);
  })
  .catch((rejectionReason) => {
    console.log(rejectionReason);
  });
```

## Chaining Multiple Promises

One common pattern with asynchronous programming is multiple operations which depend on each other to execute or that must be executed in a certain order. We might make one request to a database and use the data returned to use to make another request.

The process of  chaining promises together is called *composition*. Promises are designed with composition in mind.

```javascript
firstPromiseFunction()
.then((firstResolveVal) => {
  return secondPromiseFunction(firstResolveVal);
})
.then((secondResolveVal) => {
  console.log(secondResolveVal);
});
```

- We invoke a function `firstPromiseFunction()` which returns a promise.
- We invoke `.then()` with an anonymous function as the success handler.
- Inside the success handler we **return** a new promise - the result of invoking a second function, `secondPromiseFunction()` with the first promise's resolved value.
- We invoke a second `.then()` to handle the logic for the second promise setting.
- Inside that `.then()`, we have a success handler which will log the second promise's resolved value to the console.

In order for our chain to work properly, we had to **return** the promise `secondPromiseFunction(firstResolveVal)`. This ensured that the return value of the first `.then()` was our second promise rather than the default return of a new promise with the same settled value as the initial.

## Avoiding Common Mistakes

Two common mistakes with promise composition:

1. Nesting promises instead of chaining them.

   ```javascript
   returnsFirstPromise()
     .then((firstResolveVal) => {
     return returnsSecondValue(firstResolveVal)
       .then((secondResolveVal) => {
       console.log(secondResolveVal);
     })
   })
   ```

   Instead of having a clean chain of promises, we've nested the login for one inside the logic of the other.

2. Forgetting to `return` a promise.

   ```javascript
   returnsFirstPromise()
   .then((firstResolveVal) => {
     returnsSecondValue(firstResolveVal)
   })
   .then((someVal) => {
     console.log(someVal);
   })
   ```

   Since forgetting to return our promise won't throw an error, this can be a really tricky thing to debug.

## Using Promise.all()

When dealing with multiple promises, but we don't care about the order, to maximize efficience we should use *concurrency*, multiple asynchronous operations happening together. With promises, we can do this with the function `Promise.all()`.

`Promise.all()` accepts an array of promises as its argument and returns a single promise. That single promise will settle in one of two ways:

- If every promise in the argument array resolved, the single promise returned from `Promise.all()` will resolve with an array containing the resolve value from each promise in the argument array.
- If any promise from the argument array rejects, the single promise returned from `Promise.all()` will immediately reject with the reason that promise rejected. This behavior is sometimes referred to as *failing fast*.

```javascript
let myPromises = Promise.all([returnsPromOne(), returnsPromTwo(), returnsPromThree()]);
 
myPromises
  .then((arrayOfValues) => {
    console.log(arrayOfValues);
  })
  .catch((rejectionReason) => {
    console.log(rejectionReason);
  });
```

- We declare `myPromises` assigned to invoking `Promise.all()`.
- We invoke `Promise.all()` with an array of three promises - the returned values from functions.
- We invoke `.then()` with a success handler which will print the array of resolved values if each promise resolved successfully.
- We invoke `.catch()` with a failure handler which will print the first rejection message if any promise rejects.

