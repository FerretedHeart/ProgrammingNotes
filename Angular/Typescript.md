# **TypeScript**

Git repo for reference: https://github.com/bricewilson/TypeScript-Getting-Started

Install with npm: npm install -g typescript

# TypeScript Project Files

- Simple JSON text file named tsconfig.json
- Stores compiler options used with the project
- Specifies files to be included or excluded in compilation
- Supports configuration inheritance

## Adding Compiler Options to tsconfig.json

Comprised of keys and values

```json
{
  "compilerOptions": {
    "target": "es5", //which version of JS to compile with
    "noUnusedLocals": true,
    "outFile": "output.js"
  },
  "files": [
    "app.ts" //array of files included in compilation
  ]
}
```

Compiler Options: https://www.typescriptlang.org/docs/handbook/compiler-options.html

Use a terminal to create a default tsconfig.json file by running: tsc --init

**Built-in Types**: string, number, boolean, array. Can be sed on variables and parameters following the *variableName: type* pattern.

Additional Built-in Types: undefined, null, any, void. In cases where a variable can be one or more types a union type can be used. Ex. *variable: string | undefined | null*

**Enum** respresents a set of name constants. They are not a "type-level" extension of JavaScript. They generate JavaScript code that is used at runtime.

```typescript
enum ProductType {
  Sports,
  HomeGoods,
  Groceries
}

// Select from a set of values
let productType = ProductType.Sports;
```

**Type Inference** - If a type is not assigned, TS will infer that type based on the value that was assigned at initialization.