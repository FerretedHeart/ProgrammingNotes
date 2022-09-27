# Unit Testing with Jest

When developing and testing in JavaScript, we can use a testing framework called [Jest](https://jestjs.io/docs/getting-started). Jest focuses on simplicity. Jest provides the two key ingredients needed for testing:

1. An assertion library - an API of functions for validating a program's functionality
2. A test runner - a tool that executes tests and provides outputted test summaries

Unlike other testing frameworks which may require you to bundle these separately, Jest provides both, allowing us to begin testing right out of the box without much setup.

## Installing Jest

The `jest` package must first be installed and configured by downloading and installing with `npm`:

```bash
npm install jest@^27 --save-dev
```

At this point, we should see `"jest"` included in the **package.json** file under `"devDependencies"`:

```json
"devDependencies": {
	"jest": "<version number>"
}
```

> Note: If you were to install Jest locally on your machine, you would need to include the `-g` global flag to get access to the command line tools.

The Jest API looks for files that are either inside of a `__tests__/` directory or any file that ends in either **.test.js** or **.specs.js**. It is good practice to match the name of the test file to the file that you are wanting to test. For example, if you were to test a file called **math.js**, you would create a file in the test directory called **math.test.js**.

Jest provides a terminal command to run test files individually: `jest <filepath/filename>`.

Example: `jest __tests__/math.test.js`

We can also run tests on all of the files that have the **.test.js** or **.spec.js** extension or in the test folder by simply running the `jest` command on its own.

## Configuring Jest

Jest allows us to customize this output by using command line flags. Though there are many [command-line flags](https://jestjs.io/docs/cli#reference), one of the most commonly used is the `--coverage` flag:

```bash
jest __tests__/ --coverage
```

 This `--coverage` flag allows us to get a report of which lines of our code were actually tested. In addition to being outputted in the terminal, this report becomes available in a directory named **coverage/** that is created at runtime.

This report can helps us make sure that our code has been thoroughly tested. There are four categories of the code that is analyzed:

- **Statement** coverage analyzes the percentage of the program's statements that have been executed.
- **Branch** coverage analyzes the percentage of the program's edge cases that have been executed.
- **Function** coverage analyzes the percent of the program's functions that have been called during testing.
- **Line** coverage analyzes the percentage of the program's executable lines in the source file that have been executed.

It is good practice to preconfigure test commands in the **package.json** file's `"scripts"` property:

```json
"scripts": {
  // other scripts...
  "test": "jest __tests__/ --coverage"
}
```

We can now run tests on all of our Jest test files while also creating a coverage report by running this terminal command: `npm test`

## Unit Testing

A unit test is designed to test the smallest unit of your code, like a single function. When unit testing, each function should be tested in isolation. In Jest, we do this by creating separate containers for our testing logic using the `test()` function.

The `test()` function takes three arguments:

1. A string describing what is being tested
2. A callback function containing assertions and other testing logic
3. An optional timeout in milliseconds that specifies how long a test should wait before automatically aborting. If unspecified, this defaults to 5000 ms.

```react
//file: __tests__/recipes.test.js
 
// import the functions to test
import { findRecipe, getIngredients } from "./recipes.js"; 
 
test("Get the full recipe for Pesto", async () => {
    // testing logic for findRecipe() omitted...
}, 10000);
 
test("Get only the ingredients list for Pesto", () => {
    // testing logic for getIngredients() omitted...
});
```

- With the first argument of `test()`, we state the purpose of the test - to get the recipe for Pesto. This string shouldn't explain the implementation of the function being tested, only the desired result.
- A callback function is passed as the second argument to `test()`. Inside is where we will write our testing logic. Since the function being tested makes an asynchronous API call, this callback is marked as `async`.
- Lastly, with the third argument, we specify the at we want to see if this operation can be carried out in under 10000ms (10 seconds) since making the API call may take some time.
- The second `test()` follows the same pattern, however, the function being tested is not asynchronous so the `async` keyword is omitted from the callback function. Also, by leaving out the third argument, we are using the default 5000ms timeout.

With a `test()` container set up, it is time to finish the unit test by writing *assertions* to validate the various features of the code. To do this, Jest provides the `expect()` function.

The `expect()` function asserts how we expect our program to run and is used every time that we want to write a test. However, this function is rarely used alone - it can almost always be found in conjunction with *matcher methods*, like `.toBe()` in the example below:

```react
expect(2+2).toBe(4)
```

The value passed to `expect()` should be an expression that you want to test while the matcher method determines how that expression will be validated and what the expected value of that expression is.

```react
//file: __tests__/recipes.test.js
 
// import the function to test
import { getIngredients } from "./recipes.js"; 
 
test("Get only the ingredients list for Pesto", () => {
  //arrange
  const pestoRecipe = {
    'Basil': '2 cups',
    'Pine Nuts': '2 tablespoons',
    'Garlic': '2 cloves',
    'Olive Oil': '0.5 cups',
    'Grated Parmesan': '0.5 cups'
  }
  const expectedIngredients = ["Basil", "Pine Nuts", "Garlic", "Olive Oil", "Grated Parmesan"]
 
  //act
  const actualIngredients = getIngredients(pestoRecipe);
 
  //assertions
  expect(actualIngredients).toEqual(expectedIngredients)
});
```

In this example, we follow the **Arrange, Act, Assert** pattern in the callback passed to `test()`:

- Arrange: We first declare the input (`pestoRecipe`) to be passsed to the function being tested (`getIngredients()`) as well as the expected output (`expectedIngredients`).
- Act: We then pass the input variable into the function being tested and store the result in a new variable (`actualIngredients`).
- Assert: Finally, we use the `expect()` assertion function and the `.toEqual()` matcher to compare the values of the expected output with the actual output.

Multiple `expect()` assertions can be made within a single call to `test()`. Regardless of the number of assertions made within a unit test, in order for the entire test to pass, all assertions must pass.

## Matcher Functions

[Expect Methods Documentation](https://jestjs.io/docs/expect)

```react
//file: __tests__/recipes.test.js
 
// import the function to test
import { getIngredients } from "./recipes.js"; 
 
test("Get only the ingredients list for Pesto", () => {
  //arrange
  const pestoRecipe = {
    'Basil': '2 cups',
    'Pine Nuts': '2 tablespoons',
    'Garlic': '2 cloves',
    'Olive Oil': '0.5 cups',
    'Grated Parmesan': '0.5 cups'
  };
  const expectedIngredients = ["Basil", "Pine Nuts", "Garlic", "Olive Oil", "Grated Parmesan"];
 
  //act
  const actualIngredients = getIngredients(pestoRecipe);
 
  //assertions
  expect(actualIngredients).toBeDefined();
  expect(actualIngredients).toEqual(expectedIngredients);
  expect(actualIngredients.length).toBe(5);
  expect(actualIngredients[0] === "Basil").toBeTruthy();
  expect(actualIngredients).not.toContain("Ice Cream");
});
```

1. `.toBeDefined()` is used to verify that a variable is not `undefined`. This is often the first thing checked.
2. `toEqual()` is used to perform deep equality checks between objects.
3. `toBe()` is similar to .toEqual() but is used to compare primitive values.
4. `toBeTruthy()` is used to verify whether a value is truthy or not.
5. `.not` is used before another matcher to verify that the opposite result is true.
6. `toContain()` is used when we want to verify that an item is in an array. In this case, since the `.not` matcher is used, we are verifying that `"Ice Cream"` is NOT in the array.

## Testing Async Code with Jest

```react
findRecipe('pesto', (recipe) => {
  console.log(recipe);
  /* 
  Prints {
    'Basil': '2 cups',
    'Pine Nuts': '2 tablespoons',
    'Garlic': '2 cloves',
    'Olive Oil': '0.5 cups',
    'Grated Parmesan': '0.5 cups'
  };
  */
});
```

If we wanted to make sure that this function does in fact get the requested data, we may be tempted to put our `expect()` assertion inside the callback function:

```react
test("get the full recipe for pesto", () => {
  //arrange
  const dish = "pesto";
  const expectedRecipe = {
    'Basil': '2 cups',
    'Pine Nuts': '2 tablespoons',
    'Garlic': '2 cloves',
    'Olive Oil': '0.5 cups',
    'Grated Parmesan': '0.5 cups'
  };
 
  //act  
  findRecipe(dish, (actualRecipe) => {
    //assertion
    expect(actualRecipe).toEqual(expectedRecipe);
  });
});
```

When the API call resolves, the provided callback will be executed with the fetched data (`actualRecipe`) which can be compared to `expectedRecipe`.

However, this test would leave us vulnerable to a false positive, meaning it would pass evenif our API call and/or assertion failed. By default, Jest is not aware that it must wait for asynchronous callbacks to resolve before finishing a test. From Jest's perspective, it executed the `findRecipe()` call and then moved on. When it didn't immediately encounter any failing `expect()` assertions, the test passed.

To fix this issue, Jest allows us to add a `done` parameter in the `test()` callback function. The value of `done` is a function and, when included as a parameter, Jest knows that the test should not finish until this `done()` function is called.

```react
// This time we'll use the `done` parameter
test("get the full recipe for pesto", (done) => {
  //arrange
  const dish = "pesto";
  const expectedRecipe = {
    'Basil': '2 cups',
    'Pine Nuts': '2 tablespoons',
    'Garlic': '2 cloves',
    'Olive Oil': '0.5 cups',
    'Grated Parmesan': '0.5 cups'
  };
 
  //act  
  findRecipe(dish, (actualRecipe) => {
    //assertion
    try {
      expect(actualRecipe).toEqual(expectedRecipe);
      done();
    } catch (error) {
      done(error);
    }   
  });
});
```

- In the first line of code, the `done` parameter is added to the callback passed to `test()`. Jest now knows to wait until that function is called before concluding the test.
- The `done()` function is called after the `expect()` assertion is made. This way, the `expect()` is guaranteed to be seen and any false-positives will be caught.

You should notice that the `expect()` and `done()` call are being made in a `try` block. Without this, if the assertion were to fail, `expect()` would throw an error before the `done()` function gets a chance to be called. From Jest's perspective, the reason for the test failure would be a timeout error (since `done()` was never called) rather than the actual error thrown by the failed `expect()` assertion.

By using a `catch` block, we can capture the `error` value thrown and pass it to `done()`, which then displays it in the test output. Though not required, this a best practice and will yield better test outputs.

## Testing Async Functions that Return a Promise

Here we have a function that returns a promise, along with the  `await` keyword:

```react
const recipe = await findRecipe('pesto');
console.log(recipe);
/* 
Prints {
  'Basil': '2 cups',
  'Pine Nuts': '2 tablespoons',
  'Garlic': '2 cloves',
  'Olive Oil': '0.5 cups',
  'Grated Parmesan': '0.5 cups'
}
*/
```

Jest supports the use of `async` and `await` keywords.

```react
test("get the full recipe for pesto", async () => {
  //arrange
  const dish = "Pesto"
  const expectedRecipe = {
    'Basil': '2 cups',
    'Pine Nuts': '2 tablespoons',
    'Garlic': '2 cloves',
    'Olive Oil': '0.5 cups',
    'Grated Parmesan': '0.5 cups'
  }
 
  //act  
  const actualRecipe = await findRecipe(dish);
 
  //assertion
  expect(actualRecipe).toEqual(expectedRecipe);
});
```

The `async` keyword is placed before the function that contains asynchronous code. Then the `await` keyword is placed in front of the asynchronous function call `findRecipe()`. With the inclusion of the `async/await` keywords, Jest will wait for any `await` statement to resolve before continuing on.

## Mocking with Jest

Testing with a real REST API is not ideal for a few reasons:

- We aren't concerned about whether the third-party API works. Instead, we only care about whether or not the function that performs the API call works.
- Incorporating REST API calls into our tests can create fragile tests that may fail simply due to network issues.
- If we were interacting with a production-grade database, we could accidentally alter official data.

A safer and more efficient way to write our tests would be to create a mock function that bypasses the API call and returns values that we control instead. 

Creating the mock function and then replacing the real function with the mocked one requires two separate steps:

1. First we need to create a directory labeled `__mocks__/` in the same directory as the module tha we want to mock.
2. Next, inside the directory, we create a file with the same name as the module that will be mocked.
3. Then, we create a module with the functionality that we want. Functions that we want to mock can be created using `jest.fn()` - the function provided by the Jest library for creating [mock functions](https://jestjs.io/docs/mock-function-api).
4. Lastly, we export the module.

The function that makes the API call is called `apiRequest()` and it is exported from a f ile called **utils/api-request.js**. To mock this file and the `apiRequest()` function, we write something like this:

```react
// file: utils/__mocks__/api-request.js
const apiRequest = jest.fn(() => {
  return Promise.resolve({
    status: "",
    data: {}
  });
});

export default apiRequest;
```

- Since we are mocking the **utils/api-request.js** file, we created a file called `utils/__mocks__/api-request.js`.
- Inside, we declared an `apiRequest()` function that is assigned a value of `jest.fn()`.
- By passing a callback function to `jest.fn()`, we can define the behavior of the mocked function. In this case, we have the mocked function return a custom Promise that matches the structure expected by our application (an object with `status` and `data` properties).
- Lastly, we export `apiRequest` as the default export.

The steps to use a function that we've created in our `__mocks__/` directory in our tests:

1. First, in the test file, we import the real function. This allows the test to work as it would otherwise if no mock existed.
2. Then use the `jest.mock()` function provided by Jest to override the value with the mocked one defined in the `__mocks__/` folder. `jest.mock()` accepts a string as an argument that should match the file path to the file being mocked.

> Note: The second step is only required when mocking lock modules. When mocking modules installed into the `node_modules` directory, the module will automatically be mocked when it is imported in step 1.

```react
// import the actual module
import apiRequest from './api-request.js';

// then tell Jest to use the mocked version
jest.mock('./api-request.js')
```

Following these steps will cause any `apiRequest()` function calls made in the Jest tests to use the mocked version instead.

We can also further manipulate how specific methods in the mocked module behave by using special methods attached to functions mocked with `jest.fn()`. One of these methods is `mockFunction.mockResolvedValueOnce()`, which accepts a value that the next call to `mockFunction()` will resolve to.

```react
// file: __tests__/recipes.js
 
import { findRecipe } from './recipes.js'; 
 
// import the actual module
import apiRequest from './api-request.js';
 
// then tell Jest to use the mocked version!
jest.mock('./api-request.js');
 
test("get the full recipe for a dish", async () => {
  // arrange  
  const dish = "Pesto";
  const expectedValue = { "Magical Deliciousness": "3 cups" };
 
  // set the resolved value for the next call to apiRequest  
  const mockResponse = {
    status: "mock",
    data: { "Magical Deliciousness": "3 cups" }
  }
  apiRequest.mockResolvedValueOnce(mockResponse);
 
  // act  
  const actualRecipe = await findRecipe(dish);
 
  // assertion
  expect(actualRecipe).toEqual(expectedValue);
});
```

The `.mockResolvedValueOnce()` method is used to determine what the next call to the `apiRequest()` function will resolve to. This method may be called any number of times in a test and may be useful if you'd like to finely control what each call to your mocked function resolves to.