[TOC]

# Spring Framework

"is a universal, reusable software environment that provides particular functionality as part of a larger software platform to faciliate development of software applications, products and solutions."

## Core

Foundation which provides a number of different features:

- i18n internationalization support
- Validation support
- Data binding support
- Type conversion support
- and more...

Dependency Injection is about dealing with the way objects fulfill their dependent objects.

1. The object fulfills its own dependencies
   1. Seems easy
   2. Tightly coupled objects
2. The object declares what it depends on and something else fulfills the dependency
   1. More flexible
   2. Loosely coupled objects

Spring Core is a dependency injection container

- Creates and maintains objects and their dependencies
- Less for developer to manage
- It's the glue

## Web

Framework for handling web requests

- Spring Web MVC
  - (Java) Servlet - is an object that receives a request and generates a response based on that request
  - Higher level API
  - Easier to use
  - M(odel) V(iew) C(ontroller)
- Spring Web Webflux
  - A different way of handling web requests
    - Asynchronous execution
    - Doesn't block (wait)
      - Better resource utilization

## AOP

Aspect-Oriented Programming - a programming paradigm that aims to increase modularity by allowing the separation of cross-cutting concerns.

Without AOP, solving concerns that are cross-cutting results in scattered and duplicated code across many parts of an application.

- Used to implement features in Spring
- A valuable tool for developers to handle cross-cutting concerns

## Data Access

Data Access module makes it easier to develop applications that interact with data.

Database transactions with the module are really easy.

## Integration

Integration is all about making different systems and applications work together.

RestTemplate

- Abstracts away tedious details
- Handles
  - Connecting to the web service
  - Sending the command
  - Handling the response

## Testing

Unit Testing - a software development process in which the smallest testable parts of an application, called units, are individually and independently scrutinizedfor proper operation.

Explicit support for unit testing is minimal.

Benefits come from using dependency injection.

Testing and Dependency Injection

- Test the smallest unit of code possible
  - Dependencies cause challenges
- Dependency injection forces the developer to declare dependencies
- Control how dependencies behave
  - ONLY test the code, not the dependencies
  - Mock dependencies

Integration Testing - the phase in software testing in which individual software modules are combined and tested as a group. It occurs after unit testing.