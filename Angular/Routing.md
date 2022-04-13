# **Routing**

# Displaying Components

- Nest-able components

  - Define a selector

  - ```typescript
    @Component({
    	selector: 'pm-products',
    	templateUrl: './product-list.component.html'
    })
    ```

  - Nest in another component

  - ```html
    <div><h1>{{pageTitle}}</h1>
    	<pm-products></pm-products>
    </div>
    ```

- Routed components

  - No selector or nesting

  - Configure routes

  - ```typescript
    [
    	{path: 'products', component: ProductListComponent}
    ]
    ```

  - Tie routes to actions

# How Routing Works

- Configure a route for each component
- Define options/actions
- Tie a route to each option/action
- Activate the route based on user action
- Activating a route displays the component's view

# Configuring Routes

- Define the base element

  ```html
  <head>
    ...
    <base href="/">
  </head>
  ```

- Add RouterModule

  ```typescript
  @NgModule({
  	imports: [
  		RouterModule.forRoot([])
  	],
  	declarations: [...],
  	bootstrap: [ AppComponent ]
  })
  export class AppModule{ }
  ```

- Add each route to forRoot array

  - Order matters

- path: a string that matches the URL in the browser address bar

  - No leading slash
  - '' for default route
  - '**' for wildcard route

- component: the component that the router should create when navigating to this route

  - Not string name; not enclosed in quotes

  ```typescript
  RouterModule.forRoot([
  	{path: 'products', component: ProductListComponent},
  	{path: '', redirectTo: 'welcome', pathMatch: 'full'},
  	{path: '**', redirectTo: 'welcome', pathMatch: 'full'}
  ])
  ```

# Navigating the Application Routes

- Menu option, link, image or button that activates a route
- Typing the Url in the address bar / bookmark
- The browser's forward or back buttons

# Tying Routes to Actions

- Add the RouterLink directive to an attribute

  - Clickable element
  - Enclose in square brackets

- Bind to a link parameters array

  - First element is the path
  - All other elements are route parameters

  ```html
  <ul>
  	<li><a [routerLink]="['/welcome']">Home</a></li>
  	<li><a [routerLink]="['/products']">Product List</a></li>
  </ul>
  ```

# Placing the View

- Add the RouterOutlet directive

  - Identifies where to display the routed component's view
  - Specified in the host component's template

  ```html
  	...
  	<li><a [routerLink]="['/products']">Product List</a></li>
  </ul>
  <router-outlet></router-outlet>
  ```

# Passing Parameters to a Route

```html
// product-list.component.html
<td>
  <a [routerLink]="['/products', product.productId]">
  	{{product.productName}}
  </a>
</td>
```

```typescript
// app.module.ts
{path: 'products/:id', component: ProductDetailComponent}
```

# Reading Parameters from a Route

```typescript
// product-detail.component.ts
import { ActivatedRoute } from '@angular/router';
	constructor(private route: ActivatedRoute) { }

// app.module.ts
{path: 'products/:id', component: ProductDetailComponent}
```

Snapshot: A static image of the route information shortly after the component was created

ParamMap: A dictionary of route parameter values extracted from the URL. The 'id' key returns the id of the object to fetch

Route parameters are always strings. Use JavaScript Number function to convert the string to a number

```typescript
this.route.snapshot.paramMap.get('id');
```

Observable: Read emitted parameters as they change

```typescript
this.route.paramMap.subscribe(
	params => console.log(params.get('id'))
);
```

Specified string is the route parameter name

```typescript
{path: 'products/:id', component: ProductDetailComponent}
```

# Activating a Route with Code

- Use the Router service
  - Define it as a dependency
- Create a method that calls the navigate method of the Router service
  - Pass in the link parameters array

```typescript
// product-detail.component.ts
import { Router } from '@angular/router';
...
	constructor(private router: Router) { }

	onBack(): void {
    this.router.navigate(['/products']);
  }
```

- Add a user interface element

  - Use event binding to bind to the created method

  - ```html
    <button (click)='onBack()'>Back</button>
    ```

# Protecting Routes with Guards

- LImit access to a route
- Restrict access to only certain users
- Require confirmation before navigating away

- CanActivate
  - Guard navigation to a route
- CanDeactivate
  - Guard navigation from a route
- Resolve
  - Pre-fetch data before activating a route
- CanLoad
  - Prevent asynchronous routing

# Building a Guard

- Build a guard service
  - Implement the guard type (CanActivate)
  - Create the method (canActivate())
- Register the guard service provider
  - Use the providedIn property

```typescript
// product-detail.guard.ts
import { Injectable } from '@angular/core';
import { CanActivate } from '@angular/router';

@Injectable({
  providedIn: 'root'
})
export class ProductDetailGuard implements CanActivate {
  canActivate(): boolean { }
}
```

- Add the guard to the desired route

- ```typescript
  {path: 'products/:id', canActivate: [ ProductDetailGuard ],
  	component: ProductDetailComponent}
  ```

# Using a Guard

```typescript
// app.module.ts
@NgModule({
  imports: [
    ...,
    RouterModule.forRoot([
    {path: 'products', component: ProductListComponent},
    {path: 'products/:id',
    	canActivate: [ ProductDetailGuard ]
    	component: ProductDetailComponent}
  ])
  ],
    declarations: [...],
    bootstrap: [ AppComponent ]
})
export class AppModule { }
```

# Null and Undefined

- Prevent null or undefined errors in a template

  - Safe navigation operator (?)

  - ```html
    {{product?.productName}}
    {{product?.supplier?.companyName}}
    ```

  - \*ngIf directive

  - ```html
    <div *ngIf='product'>
      <div>Name:</div>
      <div>{{product.productName}}</div>
      <div>Description</div>
      <div>{{product.description}}</div>
    </div>
    ```

    