[TOC]

# Angular CLI Overview

A command line interface for building Angular applications.

Purpose:

- Build an Angular application
- Generate Angular files
- Build and serve the application
- Run tests
- Prepare the application for deployment

# Command Syntax

ng <command> <args> --<options>

# Installing the Angular CLI

```bash
npm install -g @angular/cli
```

# New Project

```
ng new <project name> --flag(s)							// Format
ng new my-app 															// Generate a new app in /my-app
ng new my-app --dry-run											// Don't write the files, but report them
ng new my-app --skip-install								// Generate without running npm install
ng new my-app --defaults										// Use the defaults (no prompts)
ng new -- help															// Displays all of the options
```

# Serving the Application (ng serve)

- Compiles the application
- Generates application bundles
- Starts a local web server
- Serves the application from memory
- Rebuilds on file changes

# Setting the Angular CLI's Configuration

- Use ng config

- Indicate the json path

- The --global (or -g) flag sets your global angular.json file

  ```
  ng config schematics.@schematics/angular:component.styleext scss
  ```

# Linting

```
ng lint my-app --help												// Show the help for linting a project
ng lint my-app --format stylish							// Lint and format the output
ng lint my-app --fix												// Lint and attempt to fix all problems
```

# Schematic/Blueprint

A template-based code generator that supports complex logic.

It is a set of instructions for transforming a software project by generating or modifying code.

- Class (ng g cl <name>)
- Components (ng g c <name>)
- Directives (ng g d <name>)
- Enum (ng g e <name>)
- Interfaces (ng g i <name>)
- Modules (ng g m <name>)
- Pipes (ng g p <name>)
- Route guards (ng g g <name>)
- Services (ng g s <name>)

You can also use folders: ng g c <folder>/<name> and option --flat if you want to generate new ones into the folder indicated and not a new <name> folder

Angular documentation: https://angular.io/guide/schematics

# Testing

```
ng test   			// Runs unit tests
ng e2e    			// Runs end-to-end test  (requires adding an external package)
ng test --help	// Display all of the options for testing
```

## Common ng test Options

| Options         | Description                                   |
| --------------- | --------------------------------------------- |
| --code-coverage | Generate code coverage report (default false) |
| --progress      | Log profess to console (default true)         |
| --sourcemaps    | Generate source maps (default true)           |
| --watch         | Run test once, and watch (default true)       |

### Continuous Testing vs Single Test Cycle

- Tests execute after a build is executed via Karma
- Automatically watch your files for changes, by default with ng test
- Run a test a single time with: ng test --watch false

### Test Code Coverage

- Calculate and report test code coverage with ng test --code-coverage
- Report is generated in the /coverage folder
- Report folder is onfigurable in karm.conf.js

# Building

```bash
ng build   // Compiles into an output directory
ng deploy  // Deployes the application (requires adding an external package)
```

Once you've built the application, the files are stored in /dist/<app name>.

| File         | Description                    |
| ------------ | ------------------------------ |
| runtime.js   | WebPack runtime                |
| main.js      | App code                       |
| polyfills.js | Platform polyfills             |
| styles.s     | Styles                         |
| vendor.js    | Angular and other vendor files |

## Exploring the Source in the Output

```
npm install webpack-bundle-analyzer --save-dev
ng build --stats-json // https://github.com/webpack-contrib/webpack-bundle-analyzer
npx webpack-bundle-analyzer dist/my-app/stats.json

npm install source-map-explorer --save-dev
ng build // https://github.com/danvk/source-map-explorer/
npx source-map-explorer dist/my-app/main.js
```

## Comparing Dev and Prod Build Targets

|                   | ng build                      | ng build --prod     |
| ----------------- | ----------------------------- | ------------------- |
| **Environment**   | environment.ts                | environment.prod.ts |
| **Cache-busting** | only images referenced in css | all build files     |
| **Source maps**   | generated                     | not generated       |
| **Extracted CSS** | global CSS output to .js      | yes, to css file(s) |
| **Uglification**  | no                            | yes                 |
| **Tree-Shaking**  | no                            | yes                 |
| **AOT**           | no                            | yes                 |
| **Bundling**      | yes                           | yes                 |

## Common ng build Options

| Options      | Alias | Description                      |
| ------------ | ----- | -------------------------------- |
| --source-map |       | Generated a source map           |
| --aot        |       | Ahead of Time compilation        |
| --watch      | -w    | Watch and rebuild                |
| --prod       | -e    | Shortcut for prod env and target |

## Common ng serve Options

| Options        | Alias | Description                    |
| -------------- | ----- | ------------------------------ |
| --open         | -o    | Opens in the default browser   |
| --port         |       | Port to listen to when serving |
| --live-reload  |       | Reload when changes occur      |
| --ssl          |       | Serve using HTTPS              |
| --proxy-config |       | Proxy configuration file       |

# Commands

- ng help - Displays available commands
- ng new - Creates a new Angular application
- ng server - Builds the app and launches a server
- ng generate - Generates code
- ng add - Apps support for an external library to the app
- ng test - Runs unit tests
- ng e2e - Runs end-to-end tests
- ng build - Compiles into an output directory
- ng deploy - Deploys the application
- ng update - Updates the Angular version for the app

# Update Your Angular App

## Common ng update Options

| Options | Description                                                  |
| ------- | ------------------------------------------------------------ |
| --all   | Whether to update all packages in package.json               |
| --force | Force updates, or error if installed packages are incompatible |

Angular Update Guide: https://update.angular.io