# HTTP Requests

HTTP stands for *Hypertext Transfer Protocol* and is used to structure requests and responses over the internet. 

The transfer of resources happens using TCP (Transmission Control Protocol). TCP is used to manage many types of internet connections in which one computer or device wants to send something to another. HTTP is the command language that the devices on both sides of the connection must follow in order to communicate.

The four most commonly used types of HTTP requests are GET, POST, PUT, and DELETE.

## GET Requests using Fetch

The `fetch()` function:

- Creates a request object that contains relevant information that an API needs.
- Sends that request object to the API endpoint provided.
- Returns a promise that ultimately resolves to a response object, which contains the status of the promise with information the API sent back.

```javascript
fetch('http://api-to-call.com/endpoint').then(response => { // sends request
  if (response.ok) {
    return response.json();
  } // converts response object to JSON
  throw new Error('Request failed!');
}, networkError => console.log(networkError.message) // handles errors
).then(jsonResponse => { // will not execute if there was an error
  // Code to execute with jsonResponse
}); // handles success
```

## POST Requests using Fetch

The `fetch()` call takes two arguments: an endpoint and an object that contains information needed for the POST request.

The object passed to the `fetch()` function as its second argument contains two properties: `method`, with a value of `'POST'`, and `body`, with a value of `JSON.stringify({id: '200'})`. This second argument determines that this request is a POST request and what information will be sent to the API.

```javascript
fetch('http://api-to-call.com/endpoint', {
  method: 'POST',
  body: JSON.stringify({id: '200'}) // sends request
}).then(response => {
  if(response.ok) {
    return response.json();
  } // converts response object to JSON
  throw new Error('Request failed!');
}, networkError => console.log(networkError.message) // handles errors
).then(jsonResponse => {
  // Code to execute with jsonResponse
}) // handles success
```

## async GET Requests

- The `async` keyword is used to declare an `async` function that returns a promise.
- The `await` keyword can ony be used within an `async` function. `await` suspends the program while waiting for a promise to resolve.
- In a `try...catch` statement, code in the `try` block will be run and in the event of an exception, the code in the `catch` statement will run.

```javascript
const getData = async () => {
  try {
    const response = await fetch('https://api-to-call.com/endpoint'); // sends request
    if (response.ok) {
      const jsonResponse = await response.json();
      // Code to execute with jsonResponse
    } // handles response if successfuly
  }
  throw new Error('Request failed!');
} catch (error) {
  console.log(error);
} // handles response if unsuccessful
```

## async POST Requests

We still have the same structure of using `try` and `catch` as the `async` GET request but in the `fetch()` call, we now have to include an additional argument that contains more information like `method` and `body`.

```javascript
const getData = async () => {
  try {
    const response = await fetch('https://api-to-call.com/endpoint', {
      method: 'POST',
      body: JSON.stringify({id: 200})
    }) // sends request
    if(response.ok) {
      const jsonResponse = await response.json();
      // Code to execute with jsonResponse
    } // handles response if successful
    throw new Error('Request failed!');
  } catch (error) {
    console.log(error);
  } // handles response if unsuccessful
}
```

