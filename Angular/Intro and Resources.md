[TOC]

# Anatomy of an Angular Application

- Application
  - Components
    - Template - The view or UI
    - Class - Contains properties and methods (code behind interface)
    - Metadata - Additional info about the component (extra component data)
  - Services

# Resources

- Angular - https://angular.io
- Angular CLI - https://cli.angular.io
- Angular (GitHub) - https://github.com/angular/angular-cli
- PutsReq - https://putsreq.com/ - used for making sample/fake requests for responses
- TypeScript Playground - https://www.typescriptlang.org/play
- Bootstrap - https://getbootstrap.com/
- Font Awesome - https://fontawesome.com

Bootstrap and Font Awesome can be installed through npm:

```bash
npm install bootstrap font-awesome
```

# Tools

Editor - VS Code or Webstorm

NPM - Version 6 or higher

package.json - Includes dependencies that Angular needs to install

- dependencies - Packages required for development and deployment
- devDependencies - Packages only required for development

# Starting from Scratch

Use a terminal and CD into the desired project directory and add CLI so that all other applications get CLI as well with the following command:

```bash
npm install -g @angular/cli
```

Then create a new project with the following command:

```bash
ng new apm-new --prefix pm
```

- <u>apm-new</u> is the name of the project
- <u>pm</u> is the name of the application

It will prompt for adding routing and to select stylesheet format. It will then create and install all necessary components and files, including a package.json.