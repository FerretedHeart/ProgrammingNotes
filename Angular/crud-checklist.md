# **CRUD Checklist**

# Steps to Building a Data Access Service

## Import the Angular HTTP Service

Add HttpClientModule to the imports array of an Angular Module

```typescript
...
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  imports: [ HttpClientModule ],
  ...
})
  export class AppModule {}
```

## Data Access Service

Import what we need

```typescript
...
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { catchError, tap } from 'rxjs/operators';
```

Define a dependency for the http client service

```typescript
// Use a constructor parameter
...
export class ProductService {
  constructor(private http: HttpClient) {}
}
```

Create a method for each http request

```typescript
...
getProduct ...
createProduct ...
updateProduct ...
deleteProduct ...
```

Call the desired http method, such as get

```typescript
// Pass in the Url

const url = `${this.baseUrl}/${id}`;
return this.http.get(url);
```

Add error handling

```typescript
const url = `${this.baseUrl}/${id}`;

return this.http.get<Product>(url).pipe(.catchError(this.handleError));
```



## Using the Service

Inject the Data Access Service

```typescript
constuctor(private ps: ProductService) {}
```

Call the subscribe method of the returned observable

```typescript
this.ps.getProduct(id).subscribe();
```

Provide a function to handle an emitted item

```typescript
this.ps.getProduct(id).subscribe({
	next: (product: Product) =>
  this.onRetrieved(product)
});
```

Provide an error function to handle any returned errors

```typescript
this.ps.getProduct(id).subscribe({
	next: (product: Product) => this.onRetrieved(product),
  error: err => this.errorMessage = err
});
```



## 