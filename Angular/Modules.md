# **Modules**

# What is an Angular Module?

A class with an NgModule decorator.

Its purpose:

- Organize the pieces of our application
- Arrange them into blocks
- Extend our application with capabilities from external libraries
- Provide a template resolution environment
- Aggregate and re-export

# Bootstrap Array Truths

1. Every application must bootstrap at least one component, 
2. The bootstrap array should only be used in the root application module, AppModule.

# Declarations Array Truths

1. Every component, directive, and pipe we create must belong to one an only one Angular module.
2. Only declare components, directives, and pipes. 
3. All declared components, directives, and pipes are private by default. They are only accessible to other components, directives, and pipes declared in the same module.
4. The Angular module provides the template resolution environment for its component templates.

# Exports Array Truths

1. Export any component, directive, or pipe if other components need it.
2. Re-export modules to re-export their components, directives, and pipes.
3. We can export something without including it in the imports array.

# Imports Array Truths

1. Adding a module to the imports array makes available any components, directives, and pipes defined in that module's exports array.
2. Only import what this module needs. For each component declared in the module, add to the imports array what is needed by the component's template.
3. Importing a module does NOT provide access to its imported modules. (Imports are not inherited).
4. Use the imports array to register services provided by Angular ot third-party modules. Import the module in the AppModule to ensure its services are registered one time.