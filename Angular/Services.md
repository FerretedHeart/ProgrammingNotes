# **Services**

A class with a focused purpose.

Used for features that:

- Are independent from any particular component
- Provide shared data or logic across components
- Encapsulate external interactions

# Dependency Injection

A coding pattern in which a class receives the instances of objects it needs (called dependencies) from an external source rather than creating them itself.

# Building a Service

- Create the service class
  - Clear name
  - Use PascalCasing
  - Append "Service" to the name
  - export keyword
- Define the metadata with a decorator
  - Use injectable
  - Prefix with @; Suffix with ()
- Import what we need

```typescript
import { Injectable } from '@angular/core';

@Injectable()
export class ProductService {
	getProducts(): IProduct[] {
	}
}
```

# Registering a Service

| ROOT INJECTOR                                   | COMPONENT INJECTOR                                           |
| ----------------------------------------------- | ------------------------------------------------------------ |
| Service is available throughout the application | Service is available ONLY to that component and its child (nested) components |
| Recommended for most scenarios                  | Isolates a service used by only one component                |
|                                                 | Provides multiple instances of the service                   |

```typescript
// Root
@Injectable({
	providedIn: 'root'
})

// Component
@Component({
  templateUrl: './product-list.component.html',
  providers: [ProductService]
})
export class ProductListComponent { }
```

# Injecting the Service

- Specify the service as a dependency
- Use a constructor parameter
- Service is injected when component is instantiated

```typescript
import { ProductService } from './product.service';

@Component({
	selector: 'pm-products',
	templateUrl: './product-list.component.html'
})
export class ProductListComponent {
	constructor(private productService: ProductService) { }
}
```

