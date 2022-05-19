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

Use the tools to create a default tsconfig.json file by typing in: tsc --init