# Spring

Spring is an open-source Java framework that is useful for building RESTful web applications. Like any web framework, Spring comes with an established set of code conventions and patters, such as predefined templates, databases, and functions. These generic code conventions provide reusable infrastructural support, enabling developers to easily build a variety of applications without reinventing the wheel at the stat of every project. Spring focuses on project "scaffolding" so that developers can concentrate on the core logic of applications.

The term REST stands for REpresentational State Transfer, and it describes the architectural technique used to facilitate communication between different systems via the web. Stateless requests are sent from a client to either retrieve or make changes to a resource stored on a server. The server provides a response based on the information received in the request.

[REST architecture](https://www.codecademy.com/article/what-is-rest)

## Mapping Requests in Spring

To provide a request, the server maps the request to an endpoint. In Spring, a class is marked as a controller and each of its methods is associated with an endpoint. The method is called to formulate a response to each request. This is facilitated by two important annotations:

- The `@RestController` annotation is used to declare a class as a controller that can provide application-specific types of data as part of an HTTP response.
- The `@RequestMapping` annotation is used to wire request types and endpoints to specific class methods.

The `@RestController` annotation should be added to each class that will handle responses to HTTP requests. The `@RestController` combines the functionality of two separate annotations, `@Controller` and `@ResponseBody`.

- `@Controller` is used to make a class identifiable to Spring as a component of the web application.
- `@ResponseBody` tells Spring to convert the return values of a controller's methods to JSON and bind that JSON to the HTTP response body. 

`@RequestMapping` accepts several arguments. The first, and perhaps most important, is the `path`. The path argument is used to determine where requests are mapped. For example, if a user makes a request to get a "book" resource, the server will send the request to a method with the annotation `@RequestMapping(path = "/book")` and that method would provide the response.

The `@RequestMapping` annotation also accepts several other arguments including `method`, `params`, and `consumes`.

- `method`: allows the developer to specify which HTTP method should be used when accessing the controller method. The most common are `RequestMethod.GET`, `...POST`, `...PUT`, and `...DELETE`. Since this is an optional argument, if we do not specify an HTTP method it will be defaulted to GET.

- `params`: filters requests based on given parameters.

- `consumes`: used to specify which media type can be consumed; some common media types are `"text/plain"`, `"application/JSON"`, etc.

  ```java
  @Controller
  public class VirtualLibrary{
  	@RequestMapping("/books/all", RequestMethod.GET)
  	public Book getAllBooks() {
  		return getBook();
  	}
  }
  ```

When the `@RequestMapping` data annotation is used at the class level, the specified path argument will become the base path. For example, a class with the data annotation `@RequestMapping("/books")` will map all appropriate requests to this class, and in this case, "/books" becomes the base path. Methods in this class can be further mapped.

```java
@RestController
@RequestMapping("/books")
public class VirtualLibrary {
	@RequestMapping(value = "/thumbnails", method = RequestMethod.GET)
	public String[] getBookThumbnails() {
		//returns thumbnails for all available titles
	}
}
```

> Note: When specifying a selection from the `RequestMethod` enum, be sure to import the annotation using:
>
> `org.springframework.web.bind.annotation.RequestMethod`

Helper methods and the requests to which they map:

| HTTP Method Annotation | Usage                                                        |
| ---------------------- | ------------------------------------------------------------ |
| `@GetMapping`          | Used to specify GET requests to retrieve resources           |
| `@PostMapping`         | Used to specify POST requests to create new resources        |
| `@PutMapping`          | Used to specify PUT requests to update existing resources    |
| `@DeleteMapping`       | Used to specify DELETE requests to remove specific resources |

`@RequestParam` is an annotation that can be used at the method parameter level. It allows us to parse query parameters and capture those parameters as method arguments. This is helpful because we can take values passed in from the HTTP request, parse them, and then bind the values to a controller method for further processing. We also have the option of defining a default value for the method argument in case a value is not received from the client.

Let's say the user submits their request for a book with 2 authors and a publishing year of 1995:

```bash
libraryapp.com/book?author_count=2&published_year=1995
```

The `author_count` and `published_year` values would map to the method parameters as follows:

```java
@GetMapping("/book")
public Book isBookAvailable(@RequestParam int author_count, @RequestParam int published_year) {
	return books.find(author_count, published_year);
}
```

`@PathVariable` maps template variables in the request URI directly to a method's parameters. For example, we could define a template path

```bash
/books/{id}
```

and use the URI

```bash
localhost:4001/books/28937
```

to pass the path variable "28937" to a method's `id` parameter. On the server side, we would have an endpoint that looks up books by ID as follows:

```java
@GetMapping("/id")
public Book isBookAvailable(@PathVariable int id) {
	return book.findByID(id);
}
```

In the above example, use of the `@PathVariable` at the method parameter level allows us to take a variable received from the request URI and pass it into a method as a parameter.

`@RequestBody` is an annotation used to allow Spring to automatically deserialize the HTTP request body into a Java object which can be bound to the method and further processed. The objects van be created by matching attributes with values in JSON.

For example, we can rewrite the previous book example with `@RequestBody` if we define a `Book` object:

```java
class Book {
	public int authorCount;
	public int publishedYear;
}
```

The new controller will have a single `Book` parameter instead of separate `authorCount` and `publishedYear` parameters:

```java
@GetMapping("/book")
public Book isBookAvailable(@RequestBody Book book) {
	return book.find(book.authorCount, book.publishedYear);
}
```

The `@ResponseStatus` annotation can be applied to methods to help with fine-tuning HTTP responses. The annotation accepts parameters for the status `code` and `reason`.

This can be used to customize messages in both failure and success responses. Consider the scenario when an admin adds a new book to the collection. When they enter all of the required information and click submit, we can use `@ResponseStatus` to return an HTTP status showing the request was successful and a reason indicating the entry was created.

```java
@PutMapping(path="/book")
@ResponseStatus(code = HttpStatus.CREATED, reason = "Book was successfully added")
public string AddNewBook(@RequestParam string title) {

}
```

## Clarifying with HTTP Status Codes

Anytime an HTTP response is transmitted, a status code is included in the response. [HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) are used to determine if the request was successful or if some type of error occurred and every code has a specific meaning.

The Spring framework uses an `HttpStatus` enumeration ([enum](https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html)) to represent different HTTP status codes. 

The actual constants in the `HttpStatus` enum loosely match the status code name, e.g. a 201 Created code corresponds to `HttpStatus.CREATED` and a 400 Bad Request code corresponds to `HttpStatus.BAD_REQUEST`. [Full list here](https://github.com/spring-projects/spring-framework/blob/main/spring-web/src/main/java/org/springframework/http/HttpStatus.java).

| Range     | Description            |
| --------- | ---------------------- |
| 100 - 199 | Informational Response |
| 200 - 299 | Successful Response    |
| 300 - 399 | Redirect Response      |
| 400 - 499 | Client Error Response  |
| 500 - 599 | Server Error Response  |

## Exception Handling in Spring

If an error is encountered, we can use `ResponseStatusException` to exert even more control over the exception handling process.

`ResponseStatusException` accepts up to 3 arguments:

1. `HttpStatus code`
2. `String reason`
3. `Throwable cause`

In the following example, we are accepting the ID from the client as a string and parsing it into an integer. If the parse fails, a `NumberFormatException` will be thrown. This type of exception may not be so helpful if it is returned to the client. Therefore, we are catching this exception and throwing a new `ResponseStatusException` that will contain more detailed information. In this way, we have exercised more control over the exception handling process and we can let the user know why the error occurred and/or what they can do to resolve the issue.

```java
@GetMapping("/{id}")
public Book isBookAvailable(@PathVariable string id) {
	if(id.isNumeric()) {
		int id_int = Integer.parseInt(id)
		return book.findByID(id_int)
	} else {
		throw new ResponseStatusException(HttpStatus.BAD_REQUEST, "The ID contained a non-numerical value.");
	}
}
```



## Additional Info

 [Spring Foundations](Spring/foundations.md)

 [Framework Fundamentals](Spring/fundamentals.md)