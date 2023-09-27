# NPM

NPM is a Node Package Manager for the JavaScript programming language maintained by npm, Inc. npm is the default package manager for the JavaScript runtime environment Node.js.

## Common NPM Commands

| Command                 | Description                                                  |
| ----------------------- | ------------------------------------------------------------ |
| npm init                | Initializes a new Node.js project, creating a package.json file. |
| npm install             | Installs all dependencies for the current project listed in the package.json file. |
| npm install <package>   | Installs a specific package and saves it in the package.json file under dependencies. |
| npm uninstall <package> | Removes a specific package from the dependencies in the package.json file and from the node_modules directory. |
| npm start               | Starts the application by running the script specified in the start property of the scripts section in the package.json file. |
| npm update              | Updates all the packages to their latest version according to the semantic versioning notation set in the package.json file. |
| npm update <package>    | Updates a specific package to its latest version according to the semantic versioning notation set in the package.json file. |
| npm run <script>        | Runs a specific script defined in the scripts section of the package.json file. |
| npm list                | Lists all installed packages for the current project.        |
| npm -v                  | Checks the installed version of npm.                         |
| npm audit               | Checks your project for security vulnerabilities in its dependencies and suggests fixes when available. |
| npm ci                  | Installs dependencies directly from package-lock.json and uses exact versions, providing consistent installs and improving build times. |
| npm outdated            | Checks for outdated packages. It will list current version, wanted version, and latest version of each package in your project. |
| npm cache clean --force | Clears the NPM cache forcefully. This is a common troubleshooting step if you're having issues with installing packages. |
| npm pack                | Packs the project into a .tgz file which can be later installed through NPM. This is useful if you want to see what npm publish will send to the registry. |

