# **Tips and Hints**

| Code | Output          |
| ---- | --------------- |
| \\'  | single quote    |
| \\"  | double quote    |
| \\\  | backslash       |
| \\n  | new line        |
| \\r  | carriage return |
| \\t  | tab             |
| \\b  | backspace       |
| \\f  | form feed       |

**Template Literal** – Insert variables in a string rather than concatenating bits of string with variables; use backticks to construct and ${} surrounding the variables or expressions.
	const jeanine = `I'm ${firstName} and I live on ${address}.`
	
For multi-lined strings, as of ES6, you no longer need to use \n for new lines. Just return to start the new line.

Type conversion (manually converted) and coercion (JS converts automatically)

- Number function for numbers
- NaN = Not a number (invalid)
- String function for strings
- Coercion is done with every operator except + as it will string together the values instead.	

Falsy values:

1. 0
2. ' '
3. undefined
4. null
5. NaN

Nullish values:

1. null
2. undefined

An expression produces a value. A statement is an action.
A ternary operation, since it returns a value, is an expression.

```javascript
// Function declaration
function calcAge1(birthYear) {
return 2037 - birthYear;
}

// Function expression – needs to be called first
const calcAge2 = function(birthYear) {
return 2037 - birthYear;
}
```

Arrow functions are similar to ternary operators where simple expressions are made into one line expressions. Arrow functions don't get 'this'.

```javascript
const calcAge3 = birthYear => 2037 - birthYear;
```

## 4 Ways to Solve Any Problem

1. Make sure you 100% understand the problem. Ask the right questions to get a clear picture of the problem.
   a. Example: Project Manager: "We need a function that reverses whatever we pass into it."
   	i. What does "whatever" even mean in this context? What should be reversed? Answer: Only strings, numbers, and arrays make sense to reverse.
   	ii. What to do if something else is passed in?
   	iii. What should be returned? Should it always be a string, or should the type be the same as passed in?
   	iv. How to recognize whether the argument is a number, a string, or an array?
   	v. How to reverse a number, a string, and an array?
2. Divide and conquer: Break a big problem into smaller sub-problems.
   a. Sub-Problems:
   	i. Check if argument is a number, a string, or an array
   	ii. Implement reversing a number
   	iii. Implement reversing a string
   	iv. Implement reversing an array
   	v. Return reversed value
3. Don't be afraid to do as much research as you have to
   a. Google
   b. StackOverflow
   c. MDN Web Docs
   d. Questions to ask:
   	i. How to check if a value is a number/string/array in JavaScript?
   	ii. How to reverse a number/string/array in JavaScript?
4. For bigger problems, write pseudo-code before writing the actual code
   Function reverse (value)
   If value type !string && !number && !array
     Return value
   If value type == string
     Reverse string
   If value type == number
     Reverse number
   If value type == array
     Reverse array
   Return reversed value

## The Debugging Process

Identify

- During development
- Testing software
- User reports during production
- Context: browsers, users, etc.	

Find

- Developer console (simple code)
- Debugger (complex code)

Fix

- Replace wrong solution with new solution

Prevent

- Searching for the same bug in similar code
- Writing tests using testing software