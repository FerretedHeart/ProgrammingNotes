# **HTTP**

# What is a REST Service?

- Representational State Transfer

- Web API or HTTP API

- Uses HTTP verbs to specify CRUD operations

- URL conventions address individual as well as collections of resources

- HTTP response codes indicate success/failure of server action


## RESTful CRUD

Create

- POST - http://localhost/api/books
- If successful, returns HTTP 201 Created

Read

- GET - http://localhost/api/books or https://localhost/api/books/5
- If successful, returns HTTP 200 OK

Update

- PUT - http://localhost/api/books/5
- If successful, returns HTTP 204 No Content

Delete

- DELETE - http://localhost/api/books/5
- If successful, returns HTTP 204 No Content

# Reactive Extensions (RxJS)

- Angular apps are dependent on RxJS
- Installed with apps created by the CLI
- Most HttpClieent methods return Observables
- Lots of RxJS operators available to work with Observables
- A library for composing data using observable sequences
- And transforming that data using operators
  - Similar to .NET LINQ operators
- Angular uses Reactive Extensions for working with data
  - Especially asynchronous data

## Operate on an Observable and return an Observable

- May be chained together to perform complex transformations
- Flexibility to transform data into exactly the shape you need

# Subscribing to Observables

```typescript
getAllBooks(): Observable<Book[]> {
  return this.http.get<Book[]>('/api/books');
}

this.dataService.getAllBooks()
	.subscribe(
  	(data: Book[]) => this.allBooks = data,
  	(err: any) => console.log(err),
  	() => console.log('All done getting books.')
);
```



## Observable

A collection of items over time

- Unlike an array, it doesn't retain items
- Emitted items can be observed over time

### What Does an Observable Do?

- Nothing until we subscribe
- next: Next item is emitted
- error: An error occurred and no more items are emitted
- complete: No more items are emitted

### Common Observable Usage

- Start the Observable (subscribe)
- Pipe emitted items through a set of operators
- Process notifications: next, error, complete
- Stop the Observable (unsubscribe)

```typescript
import { Observable, range } from 'rxjs';
import { map, filter } from 'rxjs/operators';

const source$: Observable<number> = range(0, 10);

source$.pipe(
	map(x => x * 3),
	filter(x => x % 2 === 0)
).subscribe(x => console.log(x));
```

# Synchronous vs.  Asynchronous

- Synchronous: real time
- Asynchronous: No immediate response
- HTTP requests are asynchronous: request and response

# Setting Up an HTTP Request

- Define a dependency for the Http Client Service in the constructor
- Create a method for each HTTP request
- Call the desired HTTP method, such as get
- Use generics to specify the returned type

```typescript
// product.service.ts
import { HttpClient } from '@angular/http';

@Injectable({
	providedIn: 'root'
})
export class ProductService {
	private productUrl = 'www.myWebService.com/api/products';
	
	constructor(private http: HttpClient) { }
	
	getProducts(): Observable<IProduct[]> {
		return this.http.get<IProduct[]>(this.productUrl);
	}
}
```

# Registering the HTTP Service Provider

Add HttpClientModule to the imports array of one of the application's Angular Modules.

```typescript
// app.module.ts
import { HttpClientModule } from '@angular/common/http';

@NgModule({
	imports: [
		BrowserModule,
		FormsModule,
		HttpClientModule
	],
	declarations: [
		AppComponent,
		ProductListComponent,
		ConvertToSpacesPipe,
		StarComponent
	],
	bootstrap: [ AppComponent ]
})
export class AppModule { }
```

# Exception Handling

```typescript
// product.service.ts
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { Observable } from 'rxjs';
import { catchError, tap } from 'rxjs/operators';

	getProducts(): Observable<IProduct[]> {
		return this.http.get<IProduct[]>(this.productUrl).pipe(
			tap(data => console.log('All: ', JSON.stringify(data))),
			catchError(this.handleError)
		);
	}
	
	private handleError(err: HttpErrorResponse) {}
```

# Subscribing to an Observable

- Call the subscribe method of the returned observable
- Provide a function to handle an emitted item
- Provide a function to handle any returned errors

```typescript
x.subscribe()

x.subscribe(Observer)

x.subscribe({
	nextFn,
	errorFn,
	completeFn
})

const sub = x.subscribe({
  nextFn,
  errorFn,
  completeFn
})

// product.service.ts
getProducts(): Observable<IProduct[]> {
  return this.http.get<IProduct[]>(this.productUrl).pipe(
  tap(data => console.log(`All: `, JSON.stringify(data))),
  catchError(this.handleError)
  );
}

// product-list.component.ts
ngOnInit(): void {
  this.productService.getProducts().subscribe({
    next: products => this.products = products,
    error: err => this.errorMessage = err
  });
}
```

# Unsubscribe from an Observable

- Store the subscription in a variable
- Implement the OnDestroy lifecycle hook
- Use the subscription variable to unsubscribe

```typescript
ngOnInit(): void {
  this.sub = this.productService.getProducts().subscribe({
    next: products => this.products = products,
    error: err => this.errorMessage = err
  });
}

ngOnDestroy(): void {
  this.sub.unsubscribe();
}
```

