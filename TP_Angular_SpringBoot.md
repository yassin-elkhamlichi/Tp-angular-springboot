# 🎓 TP — Smart Inventory & Order Platform

## Full-Stack Angular 18+ & Spring Boot 3.x — Project-Driven Learning

---

**Author:** Yassine Elkhamlichi  

---

> **Pedagogical Approach — The 70/30 Golden Rule:**  
> 30% reading documentation & watching concepts → 70% hands-on coding inside a single repository.  
> Every concept is immediately applied in the project. No isolated toy examples.

---

## 📑 Table of Contents

- [What You Will Learn](#what-you-will-learn)
- [Prerequisites](#prerequisites)
- [Project Overview](#project-overview)
- [Architecture Blueprint](#architecture-blueprint)
- **Exercises**
  - [Phase 1 — Data Contract & Basic Rendering](#phase-1--data-contract--basic-rendering)
  - [Phase 2 — Reactive Forms & Error Handling](#phase-2--reactive-forms--error-handling)
  - [Phase 3 — Signals & RxJS Pipeline](#phase-3--signals--rxjs-pipeline)
  - [Phase 4 — JWT Security & Interceptors](#phase-4--jwt-security--interceptors)
  - [Phase 5 — Production Readiness](#phase-5--production-readiness)
- **Solutions**
  - [Solution — Phase 1](#solution--phase-1)
  - [Solution — Phase 2](#solution--phase-2)
  - [Solution — Phase 3](#solution--phase-3)
  - [Solution — Phase 4](#solution--phase-4)
  - [Solution — Phase 5](#solution--phase-5)

---

## What You Will Learn

By the end of this TP, you will have built a **complete, production-grade** full-stack application and mastered the following:

### 🔧 Spring Boot 3.x (Backend)

| Concept | Description |
|---|---|
| **Layered Architecture** | Controller → Service → Repository — strict separation of concerns |
| **JPA / Hibernate** | Entity mapping, relationships, `@EntityGraph`, DTO Projections |
| **REST API Design** | Versioned endpoints, proper HTTP verbs, status codes, HATEOAS awareness |
| **Bean Validation** | `@Valid`, `@NotBlank`, `@Min`, custom constraints |
| **Global Error Handling** | `@RestControllerAdvice`, `@ExceptionHandler`, structured error maps |
| **Pagination & Sorting** | `Pageable`, `Page<T>`, dynamic search with JPA Specifications |
| **Spring Security + JWT** | Stateless authentication, `JwtAuthenticationFilter`, role-based access |
| **Performance** | N+1 problem diagnosis, `@EntityGraph`, DTO Projections |

### 🅰️ Angular 18+ (Frontend)

| Concept | Description |
|---|---|
| **Standalone Architecture** | No `NgModule` — `provideHttpClient()`, `provideRouter()` |
| **Strongly-Typed Services** | `HttpClient` with TypeScript interfaces |
| **Modern Control Flow** | `@for`, `@if`, `@switch`, `@defer` |
| **Reactive Forms** | `FormGroup`, `FormControl`, `Validators`, custom validators |
| **Angular Signals** | `signal()`, `computed()`, `effect()` for reactive state management |
| **RxJS Operators** | `debounceTime`, `distinctUntilChanged`, `switchMap` for live search |
| **HTTP Interceptors** | `HttpInterceptorFn` for token injection and error handling |
| **Route Guards** | `CanActivateFn` for protecting routes |
| **Lazy Loading** | `@defer (on viewport)` for performance optimization |

### 🏗️ Architecture & DevOps

| Concept | Description |
|---|---|
| **Clean Project Structure** | Industry-standard folder naming used by top tech companies |
| **Full-Stack Integration** | Frontend ↔ Backend communication with proper CORS |
| **Docker Compose** | Multi-container deployment (DB + API + UI) |
| **Debugging Skills** | Network Tab, SQL logs, error tracing |

---

## Prerequisites

Before starting this TP, ensure you have:

- [ ] **Java 21+** installed (`java --version`)
- [ ] **Node.js 20+** and **npm 10+** installed (`node -v && npm -v`)
- [ ] **Angular CLI 18+** installed globally (`npm install -g @angular/cli && ng version`)
- [ ] **Maven 3.9+** or use the Maven Wrapper (`./mvnw`)
- [ ] **PostgreSQL 16+** running locally (or Docker: `docker run -d -p 5432:5432 -e POSTGRES_PASSWORD=secret postgres:16`)
- [ ] **An IDE:** IntelliJ IDEA (backend) + VS Code (frontend)
- [ ] **Postman** or **curl** for API testing
- [ ] **Docker & Docker Compose** (Phase 5 only)

---

## Project Overview

### 🛒 Smart Inventory & Order Platform

You are building a **professional inventory management system** with three main modules:

```
┌─────────────────────────────────────────────────────────┐
│                  SMART INVENTORY PLATFORM                │
├─────────────────┬──────────────────┬────────────────────┤
│  📦 Products    │  🛒 Cart         │  🔐 Admin Panel    │
│                 │                  │                    │
│ • Live search   │ • Add/Remove     │ • Role-based       │
│ • Category      │ • Update qty     │ • Price management │
│ • Pagination    │ • Server sync    │ • Stock control    │
│ • Sort & Filter │ • Total calc     │ • Protected routes │
└─────────────────┴──────────────────┴────────────────────┘
```

---

## Architecture Blueprint

### Backend — Spring Boot Project Structure

```
smart-inventory-api/
├── src/main/java/com/smartinventory/
│   ├── SmartInventoryApplication.java
│   ├── config/
│   │   ├── CorsConfig.java
│   │   └── SecurityConfig.java
│   ├── controller/
│   │   ├── AuthController.java
│   │   ├── ProductController.java
│   │   ├── CategoryController.java
│   │   ├── CartController.java
│   │   └── AdminController.java
│   ├── dto/
│   │   ├── request/
│   │   │   ├── LoginRequest.java
│   │   │   ├── RegisterRequest.java
│   │   │   ├── ProductRequest.java
│   │   │   └── CartItemRequest.java
│   │   └── response/
│   │       ├── AuthResponse.java
│   │       ├── ProductResponse.java
│   │       ├── PageResponse.java
│   │       └── ErrorResponse.java
│   ├── exception/
│   │   ├── GlobalExceptionHandler.java
│   │   ├── ResourceNotFoundException.java
│   │   └── BusinessException.java
│   ├── model/
│   │   ├── Product.java
│   │   ├── Category.java
│   │   ├── CartItem.java
│   │   ├── Order.java
│   │   └── User.java
│   ├── repository/
│   │   ├── ProductRepository.java
│   │   ├── CategoryRepository.java
│   │   ├── CartItemRepository.java
│   │   └── UserRepository.java
│   ├── security/
│   │   ├── JwtService.java
│   │   ├── JwtAuthenticationFilter.java
│   │   └── UserDetailsServiceImpl.java
│   └── service/
│       ├── ProductService.java
│       ├── CategoryService.java
│       ├── CartService.java
│       ├── AuthService.java
│       └── impl/
│           ├── ProductServiceImpl.java
│           ├── CategoryServiceImpl.java
│           ├── CartServiceImpl.java
│           └── AuthServiceImpl.java
├── src/main/resources/
│   └── application.yml
└── pom.xml
```

### Frontend — Angular Project Structure

```
smart-inventory-ui/
├── src/
│   ├── app/
│   │   ├── core/
│   │   │   ├── interceptors/
│   │   │   │   ├── auth.interceptor.ts
│   │   │   │   └── error.interceptor.ts
│   │   │   ├── guards/
│   │   │   │   ├── auth.guard.ts
│   │   │   │   └── admin.guard.ts
│   │   │   └── services/
│   │   │       ├── auth.service.ts
│   │   │       └── storage.service.ts
│   │   ├── features/
│   │   │   ├── auth/
│   │   │   │   ├── login/
│   │   │   │   │   ├── login.component.ts
│   │   │   │   │   └── login.component.html
│   │   │   │   ├── register/
│   │   │   │   │   ├── register.component.ts
│   │   │   │   │   └── register.component.html
│   │   │   │   ├── models/
│   │   │   │   │   └── auth.model.ts
│   │   │   │   └── auth.routes.ts
│   │   │   ├── products/
│   │   │   │   ├── product-list/
│   │   │   │   │   ├── product-list.component.ts
│   │   │   │   │   └── product-list.component.html
│   │   │   │   ├── product-form/
│   │   │   │   │   ├── product-form.component.ts
│   │   │   │   │   └── product-form.component.html
│   │   │   │   ├── models/
│   │   │   │   │   └── product.model.ts
│   │   │   │   ├── services/
│   │   │   │   │   └── product.service.ts
│   │   │   │   └── products.routes.ts
│   │   │   ├── cart/
│   │   │   │   ├── cart-page/
│   │   │   │   │   ├── cart-page.component.ts
│   │   │   │   │   └── cart-page.component.html
│   │   │   │   ├── models/
│   │   │   │   │   └── cart.model.ts
│   │   │   │   ├── services/
│   │   │   │   │   └── cart.service.ts
│   │   │   │   └── cart.routes.ts
│   │   │   └── admin/
│   │   │       ├── dashboard/
│   │   │       │   ├── dashboard.component.ts
│   │   │       │   └── dashboard.component.html
│   │   │       └── admin.routes.ts
│   │   ├── shared/
│   │   │   ├── components/
│   │   │   │   ├── navbar/
│   │   │   │   │   ├── navbar.component.ts
│   │   │   │   │   └── navbar.component.html
│   │   │   │   └── pagination/
│   │   │   │       ├── pagination.component.ts
│   │   │   │       └── pagination.component.html
│   │   │   └── pipes/
│   │   │       └── currency-format.pipe.ts
│   │   ├── app.component.ts
│   │   ├── app.component.html
│   │   ├── app.config.ts
│   │   └── app.routes.ts
│   ├── environments/
│   │   ├── environment.ts
│   │   └── environment.development.ts
│   ├── styles.css
│   └── index.html
├── angular.json
├── tsconfig.json
└── package.json
```

> **🧠 Why this structure?**  
> This is the standard used at companies like Google, Apple, and Microsoft.  
> - `core/` → Singleton services, guards, interceptors (instantiated ONCE).  
> - `features/` → Domain modules, each self-contained with its own models, services, and components.  
> - `shared/` → Reusable components, pipes, and directives used across multiple features.

---

# 📝 EXERCISES

---

## Phase 1 — Data Contract & Basic Rendering

**🎯 Objective:** Establish the full-stack connection. By the end of this phase, your Angular app will fetch and display a list of products from your Spring Boot API.

**⏱ Estimated Time:** 4 days (2h/day)

---

### Exercise 1.1 — Initialize the Spring Boot Project

**Context:** Every professional project starts with a well-configured foundation. You will use Spring Initializr to scaffold your backend.

**Tasks:**

1. Go to [start.spring.io](https://start.spring.io) and generate a project with these settings:
   - **Group:** `com.smartinventory`
   - **Artifact:** `smart-inventory-api`
   - **Java:** 21
   - **Dependencies:** Spring Web, Spring Data JPA, PostgreSQL Driver, Lombok, Validation

2. Extract and open the project in IntelliJ IDEA.

3. Configure your database connection. Think about what properties you need in `application.yml`:
   - What is the JDBC URL format for PostgreSQL?
   - What property controls table auto-creation in JPA? What value should you use during development?
   - How do you enable SQL logging to see what Hibernate generates?

4. Create ALL the package folders shown in the [Architecture Blueprint](#backend--spring-boot-project-structure). Do not create any classes yet — just the empty packages.

> **🧠 Think About It:**  
> Why do we separate `dto/request` and `dto/response` into sub-packages? What problem does this solve compared to putting all DTOs in a single package?

**✅ Checkpoint:** Your Spring Boot application starts without errors, connects to PostgreSQL, and the console shows the Hibernate dialect being used.

---

### Exercise 1.2 — Design the Data Model

**Context:** The `model/` package contains your JPA entities — the objects that map directly to database tables. Start with two entities: `Product` and `Category`.

**Tasks:**

1. Create the `Category` entity with the following fields:
   - `id` (Long, auto-generated)
   - `name` (String, unique, not null)
   - `description` (String)
   - A relationship: one category can have **many** products

2. Create the `Product` entity with:
   - `id` (Long, auto-generated)
   - `name` (String, not null)
   - `description` (String)
   - `price` (BigDecimal, not null)
   - `stockQuantity` (Integer, not null)
   - `imageUrl` (String)
   - `createdAt` (LocalDateTime, auto-set on creation)
   - `updatedAt` (LocalDateTime, auto-set on update)
   - A relationship: each product belongs to **one** category

> **🧠 Think About It:**  
> - Why do we use `BigDecimal` for price instead of `Double`?  
> - What JPA annotation makes `createdAt` auto-populate when the entity is first saved?  
> - What is the difference between `@ManyToOne(fetch = FetchType.LAZY)` and `FetchType.EAGER`? Which one should you choose by default, and why?

**Hints:**
- Look at `@Entity`, `@Table`, `@Id`, `@GeneratedValue`
- For timestamps: research `@CreationTimestamp` and `@UpdateTimestamp` (Hibernate) or `@PrePersist` / `@PreUpdate` (JPA lifecycle callbacks)
- For relationships: `@ManyToOne`, `@OneToMany(mappedBy = "...")`

**✅ Checkpoint:** Restart the app. Hibernate should auto-create `products` and `categories` tables in your database. Verify with a SQL client.

---

### Exercise 1.3 — Build the Repository Layer

**Context:** Repositories are the data-access layer. In Spring Data JPA, you don't write SQL — you extend an interface.

**Tasks:**

1. Create `CategoryRepository` that extends the appropriate Spring Data interface. Think about:
   - Which base interface provides CRUD operations?
   - Which one adds pagination and sorting support?

2. Create `ProductRepository` extending the same base interface. Add a custom query method:
   - A method that finds all products **by category id** — Spring Data can derive this from the method name alone. What would that method signature look like?

> **🧠 Think About It:**  
> Spring Data JPA uses "Derived Query Methods." If your entity has a field `category` with a sub-field `id`, what method name would automatically generate `WHERE category_id = ?`?  
> Research the naming conventions at [Spring Data JPA Reference](https://docs.spring.io/spring-data/jpa/reference/jpa/query-methods.html).

**✅ Checkpoint:** The application compiles. No repository methods need implementation — Spring Data generates them.

---

### Exercise 1.4 — Create the DTO Layer

**Context:** **Never expose your entities directly in API responses.** DTOs (Data Transfer Objects) act as a shield between your database model and the outside world. This prevents leaking internal details (like Hibernate proxies, lazy-loaded collections, or sensitive fields).

**Tasks:**

1. In `dto/response/`, create a `ProductResponse` record (Java 16+ records are perfect for DTOs):
   - `id` (Long)
   - `name` (String)
   - `price` (BigDecimal)
   - `stockQuantity` (Integer)
   - `imageUrl` (String)
   - `categoryName` (String) — notice: we send the category **name**, not the entire category object

2. In `dto/response/`, create a `CategoryResponse` record with `id` and `name`.

3. In `dto/request/`, create a `ProductRequest` record with `name`, `description`, `price`, `stockQuantity`, `imageUrl`, and `categoryId` (Long).

> **🧠 Think About It:**  
> - Why do we use Java `record` instead of a regular class for DTOs?  
> - Why does `ProductResponse` contain `categoryName` (String) instead of `categoryId` (Long)?  
> - What would happen if we returned the `Product` entity directly and the `category` field was lazily loaded?

**Hints:**
- Java records: `public record ProductResponse(Long id, String name, ...) {}`
- Records automatically generate constructor, getters, `equals()`, `hashCode()`, and `toString()`

**✅ Checkpoint:** All DTOs compile. They have no annotations yet (validation comes in Phase 2).

---

### Exercise 1.5 — Implement the Service Layer

**Context:** The Service layer contains **business logic**. It receives DTOs from controllers, interacts with repositories, and returns DTOs back. The service interface + implementation pattern allows for testability and future flexibility.

**Tasks:**

1. Create a `ProductService` **interface** with these method signatures:
   - `List<ProductResponse> getAllProducts()`
   - `ProductResponse getProductById(Long id)`
   - `ProductResponse createProduct(ProductRequest request)`

2. Create `ProductServiceImpl` in `service/impl/` that **implements** `ProductService`:
   - Annotate it so Spring registers it as a service bean
   - Inject `ProductRepository` and `CategoryRepository` via **constructor injection**
   - Implement `getAllProducts()`: fetch all products, convert each entity to a `ProductResponse`
   - Implement `getProductById()`: find by ID, throw a meaningful exception if not found
   - Implement `createProduct()`: lookup the category by `categoryId`, create a new `Product` entity, save it, return the response

> **🧠 Think About It:**  
> - Why do we define a `ProductService` interface separate from `ProductServiceImpl`? Why not just have one class?  
> - Why do we use **constructor injection** instead of `@Autowired` on fields?  
> - The entity-to-DTO conversion logic — should it live in the service, or should you create a separate mapper class? What are the trade-offs?

**Hints:**
- For the "not found" case, create a `ResourceNotFoundException` in the `exception/` package. Extend `RuntimeException`. Pass a descriptive message like `"Product not found with id: " + id`.
- Conversion can be done with a private helper method: `private ProductResponse mapToResponse(Product product) { ... }`

**✅ Checkpoint:** The service class compiles. You can't test it yet without a controller — that's next.

---

### Exercise 1.6 — Build the REST Controller

**Context:** Controllers are the entry point of your API. They receive HTTP requests, delegate to services, and return HTTP responses. Controllers should be **thin** — no business logic here.

**Tasks:**

1. Create `ProductController` in `controller/`:
   - Map it to the base path `/api/v1/products`
   - Inject `ProductService` via constructor injection

2. Create three endpoints:
   - `GET /api/v1/products` → returns a list of all products (HTTP 200)
   - `GET /api/v1/products/{id}` → returns one product (HTTP 200) or 404 if not found
   - `POST /api/v1/products` → creates a new product, returns the created product (HTTP 201)

3. For the POST endpoint, think about:
   - What annotation makes Spring read the JSON body into your DTO?
   - How do you return HTTP 201 (Created) instead of 200?

> **🧠 Think About It:**  
> - Why do we version the API with `/api/v1/`? What problem does this solve in real-world applications?  
> - Should the controller return `ResponseEntity<T>` or just `T`? What's the difference?  
> - What HTTP status code should `DELETE` return? 200? 204? Why?

**Hints:**
- `@RestController` combines `@Controller` and `@ResponseBody`
- `@RequestMapping("/api/v1/products")` sets the base path
- `@GetMapping`, `@PostMapping`, `@PathVariable`, `@RequestBody`
- `ResponseEntity.status(HttpStatus.CREATED).body(response)` for 201

**✅ Checkpoint:** Test with Postman:
- First, manually insert a category in the DB: `INSERT INTO categories (name, description) VALUES ('Electronics', 'Electronic devices');`
- Send a POST to create a product with the correct `categoryId`
- Send a GET to retrieve all products
- You should receive proper JSON responses

---

### Exercise 1.7 — Configure CORS

**Context:** Your Angular app runs on `http://localhost:4200`, but your API runs on `http://localhost:8080`. Browsers block cross-origin requests by default (Same-Origin Policy). You need to configure CORS (Cross-Origin Resource Sharing) on the server.

**Tasks:**

1. Create a `CorsConfig` class in `config/`:
   - Implement `WebMvcConfigurer`
   - Override the method that registers CORS mappings
   - Allow requests from `http://localhost:4200`
   - Allow methods: GET, POST, PUT, DELETE, OPTIONS
   - Allow all headers
   - Allow credentials

> **🧠 Think About It:**  
> - Why is CORS a server-side configuration, not a client-side one?  
> - In production, should you allow `*` (all origins)? What's the security risk?  
> - What is the `OPTIONS` preflight request, and why must you allow it?

**✅ Checkpoint:** Using Postman, set the `Origin` header to `http://localhost:4200` and verify the response includes `Access-Control-Allow-Origin`.

---

### Exercise 1.8 — Initialize the Angular Project

**Context:** Now we switch to the frontend. You will create an Angular 18+ project using the modern standalone architecture (no `NgModule`).

**Tasks:**

1. Open a new terminal. Navigate to your workspace root and generate a new Angular project:
   ```bash
   ng new smart-inventory-ui --standalone --style=css --routing --ssr=false
   ```

2. Create ALL the folders shown in the [Angular Architecture Blueprint](#frontend--angular-project-structure). Do not create any files yet — just the directory structure.

3. Create the `environment.ts` and `environment.development.ts` files in `src/environments/`:
   - Define an `apiUrl` property. What should its value be for development?

4. Open `app.config.ts` and configure the application:
   - Add `provideHttpClient(withFetch())` — what does `withFetch()` do compared to the default XMLHttpRequest?
   - Add `provideRouter(routes)` — import routes from `app.routes.ts`

> **🧠 Think About It:**  
> - What is the difference between the old `NgModule`-based architecture and the new `Standalone` architecture?  
> - Why do we use `environment.ts` files instead of hardcoding URLs?  
> - What does `--ssr=false` mean? When would you want SSR?

**✅ Checkpoint:** Run `ng serve`. The default Angular welcome page loads at `http://localhost:4200`.

---

### Exercise 1.9 — Create TypeScript Models

**Context:** TypeScript interfaces define the **shape** of data flowing between your frontend and backend. They act as a contract, ensuring type safety at compile time.

**Tasks:**

1. In `features/products/models/product.model.ts`, create:
   - An interface `Product` that matches the `ProductResponse` fields from your backend
   - An interface `ProductRequest` that matches the `ProductRequest` DTO from your backend

2. Think about the types carefully:
   - What TypeScript type should `price` be? (Hint: JSON numbers become...)
   - Should `id` be required or optional in `ProductRequest`?

> **🧠 Think About It:**  
> - Why do we use `interface` and not `class` for these models?  
> - What happens if the backend adds a new field to the response but you don't update the interface?  
> - How does TypeScript's structural typing differ from Java's nominal typing?

**✅ Checkpoint:** The interfaces compile without errors. They mirror the backend DTOs exactly.

---

### Exercise 1.10 — Build the Product Service

**Context:** Angular services are **singleton** classes that handle data fetching and business logic. They are injected into components via Angular's dependency injection system.

**Tasks:**

1. Create `product.service.ts` in `features/products/services/`:
   - Use the `@Injectable({ providedIn: 'root' })` decorator — what does `providedIn: 'root'` mean?
   - Inject `HttpClient` via constructor injection
   - Use the `environment.apiUrl` as the base URL

2. Implement these methods (all must be **strongly typed** with generics):
   - `getAll(): Observable<Product[]>` → GET request to `/api/v1/products`
   - `getById(id: number): Observable<Product>` → GET request to `/api/v1/products/{id}`
   - `create(request: ProductRequest): Observable<Product>` → POST request

> **🧠 Think About It:**  
> - Why does `HttpClient.get<Product[]>(...)` return an `Observable` instead of a `Promise`? What advantage does this provide?  
> - What is the difference between `providedIn: 'root'` and registering a service in a component's `providers` array?  
> - Should this service know about the UI? Should it ever reference a component?

**Hints:**
- `HttpClient` methods: `this.http.get<T>(url)`, `this.http.post<T>(url, body)`
- Template literals for URLs: `` `${this.apiUrl}/products/${id}` ``

**✅ Checkpoint:** The service compiles. You can't test it until you create a component that uses it.

---

### Exercise 1.11 — Create the Product List Component

**Context:** Components are the building blocks of Angular UIs. Each component has a TypeScript class (logic), an HTML template (view), and optionally CSS (styling).

**Tasks:**

1. Generate the component:
   ```bash
   ng generate component features/products/product-list --standalone
   ```

2. In the component TypeScript file:
   - Inject `ProductService`
   - Create a property to hold the list of products. What type should it be?
   - Fetch the products when the component initializes. Research the Angular lifecycle hooks — which hook is called after the component is constructed and its inputs are set?
   - Subscribe to the observable and assign the result to your property

3. In the component HTML template:
   - Display a loading indicator while data is being fetched
   - Use the `@for` block syntax to iterate over products (NOT `*ngFor`)
   - For each product, display: name, price (formatted as currency), stock quantity, and category name
   - Use the `@if` block to conditionally show "Out of Stock" when `stockQuantity === 0`
   - Don't forget the `@empty` block inside `@for` for when there are no products

4. Set up routing: In `app.routes.ts`, add a route that maps `/products` to `ProductListComponent`

> **🧠 Think About It:**  
> - What is the `track` expression in `@for`? Why is it required? What should you track by?  
> - What is the difference between `@for` (new syntax) and `*ngFor` (old syntax)? Why did Angular introduce the new syntax?  
> - You're using `subscribe()` in `ngOnInit()`. Is there a memory leak risk? What happens if the component is destroyed while the HTTP request is still pending?

**Hints:**
- New control flow: `@for (item of items; track item.id) { ... } @empty { ... }`
- Lifecycle: `implements OnInit` → `ngOnInit()`
- Currency formatting: Angular has a built-in `CurrencyPipe`, or you can create a shared pipe

**✅ Checkpoint:** Navigate to `http://localhost:4200/products`. You should see the product list fetched from your Spring Boot API. Both servers must be running simultaneously.

---

### Exercise 1.12 — Navigation & Layout

**Context:** A real application needs a navigation bar and proper routing between pages.

**Tasks:**

1. Create a `NavbarComponent` in `shared/components/navbar/`:
   - Display the application name: "Smart Inventory"
   - Add navigation links: Products, Cart (for later), Login (for later)
   - Use `routerLink` for navigation (NOT `href`)
   - Apply `routerLinkActive` to highlight the current page

2. Update `app.component.html`:
   - Include the navbar
   - Add a `<router-outlet>` for page content

3. Add basic styling to `styles.css`:
   - A clean, modern navbar (flexbox layout, dark background)
   - Responsive card layout for the product list
   - Think about using CSS custom properties (variables) for your color palette

> **🧠 Think About It:**  
> - Why do we use `routerLink` instead of `href`? What happens to the page when you use `href`?  
> - What is `<router-outlet>` and how does Angular know which component to render inside it?

**✅ Checkpoint:** Your app has a navigation bar, clicking "Products" loads the product list, and the active link is visually highlighted.

---

## Phase 2 — Reactive Forms & Error Handling

**🎯 Objective:** Build a product creation form with client-side and server-side validation. Learn how to handle errors gracefully across the full stack.

**⏱ Estimated Time:** 4 days (2h/day)

---

### Exercise 2.1 — Backend Validation with Bean Validation

**Context:** Never trust client input. The server must validate ALL incoming data, even if the frontend also validates. This is your last line of defense.

**Tasks:**

1. Add validation annotations to `ProductRequest`:
   - `name`: cannot be blank, maximum 100 characters
   - `price`: must be greater than 0
   - `stockQuantity`: must be ≥ 0
   - `categoryId`: cannot be null
   - `description`: maximum 500 characters (but can be empty)

2. In `ProductController`, activate validation on the POST endpoint. What single annotation on the `@RequestBody` parameter triggers Bean Validation?

3. Test with Postman: send a POST with an empty `name`. What happens? What HTTP status code do you get? Is the error message helpful?

> **🧠 Think About It:**  
> - What is the difference between `@NotNull`, `@NotEmpty`, and `@NotBlank`?  
> - What happens if you forget the activation annotation on the controller parameter?  
> - The default error response from Spring is messy and exposes internal details. How can we improve this?

**✅ Checkpoint:** Sending invalid data returns HTTP 400 — but the error format is Spring's default (ugly and verbose). We'll fix this next.

---

### Exercise 2.2 — Global Error Handler

**Context:** A professional API returns **consistent, clean** error responses. We need a centralized error handler that catches all exceptions and formats them uniformly.

**Tasks:**

1. Create `ErrorResponse` record in `dto/response/`:
   - `timestamp` (LocalDateTime)
   - `status` (int)
   - `error` (String)
   - `errors` (Map<String, String>) — field-level errors for validation failures

2. Create `GlobalExceptionHandler` in `exception/`:
   - Annotate with `@RestControllerAdvice` — research what this annotation does
   - Handle `MethodArgumentNotValidException` (thrown by `@Valid`):
     - Extract field errors from `BindingResult`
     - Map them to `fieldName → errorMessage` pairs
     - Return HTTP 400 with your `ErrorResponse`
   - Handle `ResourceNotFoundException`:
     - Return HTTP 404 with a descriptive message
   - Handle generic `Exception`:
     - Return HTTP 500 — but **never expose stack traces** to the client

> **🧠 Think About It:**  
> - Why is `@RestControllerAdvice` better than putting try-catch in every controller method?  
> - Should the error handler log the full exception? What's the difference between what you log (server-side) and what you return (client-side)?  
> - What is the principle of "fail fast, fail informatively"?

**Hints:**
- `MethodArgumentNotValidException` has `getBindingResult().getFieldErrors()`
- Each `FieldError` has `getField()` and `getDefaultMessage()`

**✅ Checkpoint:** Send invalid data via Postman. The response should now look like:
```json
{
  "timestamp": "2026-09-22T14:30:00",
  "status": 400,
  "error": "Validation Failed",
  "errors": {
    "name": "must not be blank",
    "price": "must be greater than 0"
  }
}
```

---

### Exercise 2.3 — Build the Product Form (Angular)

**Context:** Angular offers two approaches for forms: **Template-Driven** (simple, less control) and **Reactive Forms** (programmatic, full control). In enterprise applications, we always use **Reactive Forms**.

**Tasks:**

1. Generate a `ProductFormComponent` in `features/products/product-form/`

2. In the component TypeScript:
   - Import `ReactiveFormsModule`
   - Build a `FormGroup` with `FormControl` for each field:
     - `name`: required, minLength(2), maxLength(100)
     - `description`: maxLength(500)
     - `price`: required, min(0.01)
     - `stockQuantity`: required, min(0)
     - `categoryId`: required
   - Create a `submit()` method that:
     - Marks all fields as **touched** (to show errors on submit)
     - If valid, calls `ProductService.create()` with the form value
     - On success: navigates to the product list
     - On error: extracts the error map from the server response and displays it

3. In the HTML template:
   - Create a `<form>` bound to your `FormGroup`
   - For each field, add an `<input>` bound to its `FormControl`
   - Below each input, add error messages that appear only when the field is **invalid AND touched**
   - Style the invalid fields with a red border

> **🧠 Think About It:**  
> - What is the difference between a field being "touched" vs "dirty"?  
> - Why do we `markAllAsTouched()` on submit instead of just checking `form.valid`?  
> - What would happen if a user submits the form, gets a server error, edits a field, and submits again — how do you clear previous server errors?

**Hints:**
- `FormGroup` builder: `this.form = new FormGroup({ name: new FormControl('', [Validators.required, ...]) })`
- Access errors in template: `form.get('name')?.errors?.['required']`
- Check if touched: `form.get('name')?.touched`

**✅ Checkpoint:** The form renders, client-side validation works, and submitting valid data creates a product on the server.

---

### Exercise 2.4 — Display Server-Side Errors

**Context:** Client-side validation is not enough. The server might reject data for reasons the client can't predict (duplicate names, business rules). You need to display server errors under the appropriate fields.

**Tasks:**

1. In `ProductFormComponent`:
   - Create a `serverErrors` object of type `{ [key: string]: string }` initialized to `{}`
   - In the HTTP error handler (the `error` callback of `subscribe`):
     - Check if the error status is 400
     - Extract the `errors` map from the response body
     - Assign it to `serverErrors`
   - Clear `serverErrors` when the user starts typing (listen to form `valueChanges`)

2. In the HTML template:
   - Below each field, add a conditional block that shows the server error for that field
   - Example: `@if (serverErrors['name']) { <span class="error">{{ serverErrors['name'] }}</span> }`

3. Add a general error message at the top of the form for non-field errors (e.g., HTTP 500)

> **🧠 Think About It:**  
> - Why do we clear server errors when the user starts typing?  
> - How would you handle a situation where the server returns an error for a field that doesn't exist in your form (e.g., a computed field)?  
> - Could you use Angular's `setErrors()` on individual `FormControl`s to integrate server errors into the form's built-in error system?

**✅ Checkpoint:** Submit a product with a duplicate name (if you added a unique constraint). The server error appears under the name field.

---

### Exercise 2.5 — Custom Validator

**Context:** Sometimes built-in validators aren't enough. You need to create custom validators for business-specific rules.

**Tasks:**

1. Create a custom validator function that validates price format (max 2 decimal places):
   - A validator is a function that receives a `AbstractControl` and returns `ValidationErrors | null`
   - Return `{ invalidPrice: true }` if the value has more than 2 decimal places
   - Return `null` if valid

2. Apply this validator to the `price` FormControl

3. Display the custom error message in the template

> **🧠 Think About It:**  
> - What is the difference between a **synchronous** validator and an **async** validator?  
> - When would you need an async validator? (Hint: checking if a username is already taken)  
> - Can a single FormControl have multiple validators? How are they composed?

**✅ Checkpoint:** Entering `9.999` in the price field shows "Price can have at most 2 decimal places."

---

## Phase 3 — Signals & RxJS Pipeline

**🎯 Objective:** Implement live search, pagination, and sorting. Master Angular Signals for state management and RxJS operators for complex async flows.

**⏱ Estimated Time:** 4 days (2h/day)

---

### Exercise 3.1 — Backend Pagination & Sorting

**Context:** Loading 10,000 products at once kills performance. Pagination sends data in chunks. Spring Data JPA makes this almost effortless.

**Tasks:**

1. Modify `ProductService.getAllProducts()` to accept pagination parameters:
   - Change the return type to `Page<ProductResponse>`
   - Accept a `Pageable` parameter
   - Use the repository's built-in pagination support

2. Create a `PageResponse<T>` generic DTO in `dto/response/`:
   - `content` (List<T>)
   - `pageNumber` (int)
   - `pageSize` (int)
   - `totalElements` (long)
   - `totalPages` (int)
   - `last` (boolean)

3. Update `ProductController`:
   - The GET endpoint should accept `page`, `size`, and `sort` as query parameters
   - Spring automatically binds these to a `Pageable` object — how? Research `@PageableDefault`

4. Add a method for **sorting**: allow sorting by `name`, `price`, or `createdAt` in ascending or descending order

> **🧠 Think About It:**  
> - Why does Spring Data's `Page<T>` interface include `totalElements`? How does the frontend use this number?  
> - What SQL query does `Pageable` generate under the hood? (Enable SQL logging and observe)  
> - What is the difference between `Slice<T>` and `Page<T>`? When would you prefer one over the other?

**✅ Checkpoint:** `GET /api/v1/products?page=0&size=5&sort=price,desc` returns only 5 products sorted by price descending, with pagination metadata.

---

### Exercise 3.2 — Backend Dynamic Search with JPA Specifications

**Context:** Users want to search products by name AND filter by category AND price range — all dynamically. Hard-coding every combination of query parameters is unmaintainable. JPA Specifications let you build dynamic queries programmatically.

**Tasks:**

1. Make `ProductRepository` extend `JpaSpecificationExecutor<Product>`

2. Create a `ProductSpecification` class (you can put it in `repository/` or a new `specification/` package):
   - Static method `hasName(String name)` → returns a `Specification<Product>` that filters by name containing the search term (case-insensitive)
   - Static method `hasCategory(Long categoryId)` → filters by category
   - Static method `hasPriceRange(BigDecimal min, BigDecimal max)` → filters by price range
   - Static method `hasMinStock(Integer minStock)` → filters products with stock ≥ minStock

3. Update `ProductService` to accept search parameters and **combine** specifications using `.and()`

4. Update `ProductController` to accept optional query parameters: `search`, `categoryId`, `minPrice`, `maxPrice`

> **🧠 Think About It:**  
> - How does the Specification pattern implement the **Strategy** design pattern?  
> - Why is building dynamic queries with Specifications better than writing native SQL with `@Query` and lots of conditional logic?  
> - What would happen if all search parameters are null? What should the Specification return?

**Hints:**
- `Specification<Product>` is a functional interface: `(root, query, cb) -> cb.like(cb.lower(root.get("name")), "%" + name.toLowerCase() + "%")`
- Combine: `Specification.where(hasName(name)).and(hasCategory(categoryId))`

**✅ Checkpoint:** `GET /api/v1/products?search=phone&minPrice=100&maxPrice=500&page=0&size=10` returns filtered, paginated results.

---

### Exercise 3.3 — Angular Signals for State Management

**Context:** Angular Signals (introduced in Angular 16+) provide a **synchronous, fine-grained reactive** state management system. Unlike RxJS Observables, Signals are simpler for UI state.

**Tasks:**

1. Refactor `ProductListComponent` to use Signals:
   - Replace `products: Product[] = []` with `products = signal<Product[]>([])`
   - Replace `loading: boolean = true` with `loading = signal(true)`
   - Replace `currentPage: number = 0` with `currentPage = signal(0)`
   - Create a `computed()` signal: `isEmpty = computed(() => this.products().length === 0)`

2. Update the template:
   - Access signal values with `()`: `@for (product of products(); track product.id)`
   - Use `loading()` instead of `loading` in `@if` blocks

3. Create methods that update signals:
   - `nextPage()` → `this.currentPage.update(p => p + 1)` and refetch
   - `previousPage()` → similar
   - `updateProducts(data: Product[])` → `this.products.set(data)`

> **🧠 Think About It:**  
> - What is the difference between `signal.set()` and `signal.update()`? When do you use each?  
> - What is a `computed()` signal? How is it different from a regular `signal()`?  
> - How do Signals compare to React's `useState()`? What are the similarities and differences?  
> - What is `effect()` and when would you use it? (Hint: logging, side effects)

**✅ Checkpoint:** The product list works the same as before, but now uses Signals for all state. Pagination controls change pages and refetch data.

---

### Exercise 3.4 — Live Search with RxJS Operators

**Context:** When a user types in a search box, you don't want to fire an API request on every keystroke. RxJS operators let you build a pipeline that **debounces** input, **deduplicates** identical queries, and **cancels** previous pending requests.

**Tasks:**

1. Add a search input to `ProductListComponent`:
   - Create a `searchControl = new FormControl('')` (import `ReactiveFormsModule`)
   - Bind it to an `<input>` in the template

2. Build the RxJS pipeline in `ngOnInit()`:
   ```
   searchControl.valueChanges
     → debounceTime(300)      // Wait 300ms after the user stops typing
     → distinctUntilChanged() // Don't re-search if the value hasn't changed
     → switchMap(term => ...) // Cancel the previous request, fire a new one
     → subscribe(results => ...) // Update the signal
   ```

3. Update `ProductService` to accept a search query parameter:
   - `search(query: string, page: number, size: number): Observable<PageResponse<Product>>`

4. Create the `PageResponse<T>` interface in `models/product.model.ts` (or a shared model file):
   - Mirror the backend's `PageResponse` DTO

> **🧠 Think About It:**  
> - What problem does `debounceTime(300)` solve? What would happen without it?  
> - Why is `switchMap` the correct operator here, not `mergeMap` or `concatMap`? What does "cancel the previous request" mean at the HTTP level?  
> - What happens if the user types "ph", waits 300ms (request fires), then types "phone" before the first request completes?

**Hints:**
- `switchMap` automatically unsubscribes from the previous inner Observable
- Import from `rxjs/operators`
- Update your signal inside the subscribe callback: `this.products.set(response.content)`

**✅ Checkpoint:** Type "ph" in the search box, wait — results filter. Type "phone" — the previous request is cancelled, new results appear. The Network tab should show cancelled requests.

---

### Exercise 3.5 — Reusable Pagination Component

**Context:** Pagination is used across multiple features. Build a reusable component that can be dropped into any list page.

**Tasks:**

1. Create `PaginationComponent` in `shared/components/pagination/`:
   - **Inputs** (use the `input()` function or `@Input()`):
     - `currentPage: number`
     - `totalPages: number`
     - `pageSize: number`
   - **Outputs** (use `output()` or `@Output()`):
     - `pageChange: EventEmitter<number>` — emits the new page number when clicked

2. The template should display:
   - "Previous" button (disabled on first page)
   - Page numbers (show at most 5 pages around the current page)
   - "Next" button (disabled on last page)
   - "Page X of Y" indicator

3. Use this component in `ProductListComponent` to replace your manual pagination

> **🧠 Think About It:**  
> - Why do we put this component in `shared/` instead of `features/products/`?  
> - What is the difference between a "smart" component and a "dumb/presentational" component? Which is the PaginationComponent?  
> - Why do we use `output()` to communicate with the parent instead of injecting the parent's service?

**✅ Checkpoint:** The pagination component renders below the product list, and clicking page numbers fetches the correct page.

---

## Phase 4 — JWT Security & Interceptors

**🎯 Objective:** Implement a complete authentication and authorization system. Users log in, receive a JWT token, and that token is automatically attached to every request.

**⏱ Estimated Time:** 5 days (2h/day)

---

### Exercise 4.1 — User Entity & Auth DTOs

**Context:** Before implementing security, you need a `User` entity and the DTOs for login/register.

**Tasks:**

1. Create the `User` entity in `model/`:
   - `id` (Long, auto-generated)
   - `firstName` (String)
   - `lastName` (String)
   - `email` (String, unique)
   - `password` (String) — will store the **hashed** password
   - `role` (enum: `USER`, `ADMIN`)
   - `createdAt` (LocalDateTime)

2. Create an enum `Role` (you can put it in `model/` or a sub-package)

3. Create DTOs:
   - `LoginRequest(String email, String password)`
   - `RegisterRequest(String firstName, String lastName, String email, String password, String confirmPassword)`
   - `AuthResponse(String token, String email, String role)`

4. Create `UserRepository` extending the appropriate interface:
   - Add a method to find a user by email
   - Add a method to check if an email already exists

> **🧠 Think About It:**  
> - Why do we store hashed passwords instead of plain text? What hashing algorithm should we use?  
> - Should `Role` be an entity with its own table, or an enum stored as a column? What are the trade-offs?  
> - Why does `RegisterRequest` include `confirmPassword`? Where should the password match check happen — in the DTO, the service, or the controller?

**✅ Checkpoint:** The application starts, and a `users` table is created in the database.

---

### Exercise 4.2 — JWT Service

**Context:** JWT (JSON Web Token) is a compact, self-contained token that carries user claims. The server generates it on login, and the client sends it with every subsequent request.

**Tasks:**

1. Add the JWT dependency to `pom.xml`:
   - You need `io.jsonwebtoken:jjwt-api`, `jjwt-impl`, and `jjwt-jackson`

2. Create `JwtService` in `security/`:
   - Define a secret key (store it in `application.yml`, not hardcoded)
   - Define token expiration time (e.g., 24 hours)
   - Implement:
     - `generateToken(UserDetails userDetails): String` — creates a JWT with claims (email, role)
     - `extractUsername(String token): String` — extracts the email from the token
     - `isTokenValid(String token, UserDetails userDetails): boolean` — validates the token
     - `extractClaim(String token, Function<Claims, T> resolver): T` — generic claim extractor
   - Private helper: `extractAllClaims(String token): Claims`

> **🧠 Think About It:**  
> - What are the three parts of a JWT? What information does each part carry?  
> - Why is the secret key critical to security? What happens if it's leaked?  
> - What is the difference between **symmetric** (HMAC) and **asymmetric** (RSA) signing? Which is simpler for a single-server setup?  
> - Why does the token have an expiration time? What should happen when it expires?

**Hints:**
- Use `Keys.hmacShaKeyFor(secret.getBytes())` to create the signing key
- `Jwts.builder().subject(email).claim("role", role).issuedAt(...).expiration(...).signWith(key).compact()`
- `Jwts.parser().verifyWith(key).build().parseSignedClaims(token).getPayload()`

**✅ Checkpoint:** Write a simple test or a temporary endpoint that generates a token and prints it. Decode it at [jwt.io](https://jwt.io) to verify the claims.

---

### Exercise 4.3 — Spring Security Configuration

**Context:** Spring Security intercepts ALL HTTP requests by default and requires authentication. We need to configure it to:
- Allow unauthenticated access to login/register endpoints
- Require authentication for everything else
- Use JWT (stateless) instead of sessions

**Tasks:**

1. Create `UserDetailsServiceImpl` in `security/`:
   - Implement `UserDetailsService`
   - Override `loadUserByUsername(String email)` to find the user in the database
   - Return a `UserDetails` object with the user's email, hashed password, and authorities

2. Create `JwtAuthenticationFilter` in `security/`:
   - Extend `OncePerRequestFilter`
   - Override `doFilterInternal()`:
     - Extract the `Authorization` header
     - Check if it starts with "Bearer "
     - Extract the token and validate it using `JwtService`
     - If valid, create an `Authentication` object and set it in `SecurityContextHolder`
     - Call `filterChain.doFilter()` to continue the chain

3. Create `SecurityConfig` in `config/`:
   - Define a `SecurityFilterChain` bean
   - Disable CSRF (why is this safe for a stateless API?)
   - Set session management to `STATELESS`
   - Configure URL authorization:
     - `/api/v1/auth/**` → permit all
     - `/api/v1/admin/**` → require role ADMIN
     - Everything else → authenticated
   - Add your `JwtAuthenticationFilter` before `UsernamePasswordAuthenticationFilter`
   - Define `PasswordEncoder` bean (BCrypt)
   - Define `AuthenticationManager` bean

> **🧠 Think About It:**  
> - Why do we disable CSRF for a REST API but not for a traditional web app?  
> - What does "stateless" mean in the context of Spring Security? Where is the user's identity stored?  
> - Why must `JwtAuthenticationFilter` run **before** `UsernamePasswordAuthenticationFilter`?  
> - What is the `SecurityContextHolder` and why does setting the authentication object there make subsequent security checks work?

**✅ Checkpoint:** All endpoints now return 403 Forbidden except `/api/v1/auth/**`. The product list no longer works — that's expected.

---

### Exercise 4.4 — Auth Service & Controller

**Context:** Now build the actual login and register functionality.

**Tasks:**

1. Create `AuthService` interface and `AuthServiceImpl`:
   - `register(RegisterRequest request): AuthResponse`
     - Check if email already exists → throw exception
     - Validate passwords match
     - Hash the password with `PasswordEncoder`
     - Save the user
     - Generate a JWT
     - Return `AuthResponse`
   - `login(LoginRequest request): AuthResponse`
     - Authenticate with `AuthenticationManager`
     - If successful, generate a JWT
     - Return `AuthResponse`

2. Create `AuthController`:
   - `POST /api/v1/auth/register` → register a new user
   - `POST /api/v1/auth/login` → log in an existing user

3. Add proper validation to `RegisterRequest`:
   - Email format validation (`@Email`)
   - Password minimum length
   - All fields required

> **🧠 Think About It:**  
> - Why do we use `AuthenticationManager.authenticate()` instead of manually checking the password?  
> - What happens inside `authenticate()`? How does it use `UserDetailsService` and `PasswordEncoder`?  
> - Should the register endpoint return a token immediately, or should the user log in separately?

**✅ Checkpoint:** 
- Register: `POST /api/v1/auth/register` with valid data → get a JWT token
- Login: `POST /api/v1/auth/login` with the same credentials → get a JWT token
- Use the token: `GET /api/v1/products` with `Authorization: Bearer <token>` → products returned

---

### Exercise 4.5 — Angular Auth Service & Token Storage

**Context:** The Angular side needs to store the JWT token securely and send it with every request.

**Tasks:**

1. Create `auth.model.ts` in `features/auth/models/`:
   - `LoginRequest`, `RegisterRequest`, `AuthResponse` interfaces

2. Create `StorageService` in `core/services/`:
   - Methods: `setToken(token: string)`, `getToken(): string | null`, `removeToken()`, `isLoggedIn(): boolean`
   - Think about where to store the token: `localStorage`, `sessionStorage`, or in-memory?
   - What are the security trade-offs of each approach?

3. Create `AuthService` in `core/services/`:
   - Inject `HttpClient` and `StorageService`
   - `login(request: LoginRequest): Observable<AuthResponse>`
   - `register(request: RegisterRequest): Observable<AuthResponse>`
   - `logout(): void` — clear the token and navigate to login
   - `isLoggedIn(): boolean` — delegate to `StorageService`
   - `getUserRole(): string | null` — decode the JWT to extract the role (you can use a simple base64 decode on the payload)

> **🧠 Think About It:**  
> - Why is storing the token in `localStorage` considered a security risk (XSS attacks)?  
> - What is the alternative (HttpOnly cookies)? Why didn't we use it in this TP?  
> - Can the frontend decode a JWT without the secret key? Why or why not?

**✅ Checkpoint:** Login from the console with `authService.login({...}).subscribe(res => console.log(res))`. The token is stored and `isLoggedIn()` returns true.

---

### Exercise 4.6 — HTTP Interceptor for Token Injection

**Context:** You don't want to manually add `Authorization: Bearer <token>` to every HTTP call. An interceptor does it automatically for ALL requests.

**Tasks:**

1. Create `auth.interceptor.ts` in `core/interceptors/`:
   - Export a function of type `HttpInterceptorFn`
   - The function receives `(req: HttpRequest, next: HttpHandlerFn)` and returns `Observable<HttpEvent>`
   - Clone the request and add the Authorization header if a token exists
   - Do NOT add the header for auth endpoints (login/register)

2. Create `error.interceptor.ts` in `core/interceptors/`:
   - Catch HTTP errors in the response pipeline
   - If 401 Unauthorized → clear the token and redirect to `/auth/login`
   - If 403 Forbidden → redirect to an "Access Denied" page (or show a toast)

3. Register both interceptors in `app.config.ts`:
   - Use `provideHttpClient(withInterceptors([authInterceptor, errorInterceptor]))`

> **🧠 Think About It:**  
> - Why do we `clone` the request instead of modifying it directly?  
> - What is the order of interceptors? Does it matter which one runs first?  
> - What is the difference between the old class-based `HttpInterceptor` and the new functional `HttpInterceptorFn`?  
> - Why do we skip adding the token for auth endpoints?

**Hints:**
- Clone: `req.clone({ setHeaders: { Authorization: \`Bearer ${token}\` } })`
- Catch errors: `return next(req).pipe(catchError(error => { ... }))`
- Check URL: `req.url.includes('/auth/')` to skip auth endpoints

**✅ Checkpoint:** After logging in, navigate to the product list. Open the Network tab — every request should have an `Authorization` header automatically. No changes to `ProductService` were needed.

---

### Exercise 4.7 — Route Guards

**Context:** Even with interceptors, the user could manually type a URL to access a protected page. Route guards prevent navigation to certain routes based on conditions.

**Tasks:**

1. Create `auth.guard.ts` in `core/guards/`:
   - Export a `CanActivateFn` function
   - Check if the user is logged in (using `AuthService`)
   - If not, redirect to `/auth/login` and return `false`
   - If yes, return `true`

2. Create `admin.guard.ts` in `core/guards/`:
   - Check if the user has the ADMIN role
   - If not, redirect to `/products` (or an "unauthorized" page)

3. Apply guards to routes in `app.routes.ts`:
   - `/products` → protected by `authGuard`
   - `/cart` → protected by `authGuard`
   - `/admin/**` → protected by `authGuard` AND `adminGuard`
   - `/auth/**` → no guard (public)

> **🧠 Think About It:**  
> - Route guards are client-side. Can a malicious user bypass them? If so, why are they still valuable?  
> - What is the difference between `CanActivateFn` and `CanMatchFn`?  
> - Can you apply multiple guards to the same route? In what order do they execute?

**Hints:**
- `inject(AuthService)` inside the function to get the service
- `inject(Router).createUrlTree(['/auth/login'])` to redirect
- In routes: `{ path: 'products', ..., canActivate: [authGuard] }`

**✅ Checkpoint:** Without logging in, navigating to `/products` redirects to `/auth/login`. After login, navigation works normally. An ADMIN user can access `/admin`, but a regular USER cannot.

---

### Exercise 4.8 — Login & Register Pages

**Context:** Build the actual authentication UI components.

**Tasks:**

1. Create `LoginComponent` in `features/auth/login/`:
   - Reactive form with email and password fields
   - On submit, call `AuthService.login()`
   - On success, store the token and navigate to `/products`
   - On error, display the error message (e.g., "Invalid credentials")
   - Add a link: "Don't have an account? Register"

2. Create `RegisterComponent` in `features/auth/register/`:
   - Reactive form with firstName, lastName, email, password, confirmPassword
   - Create a **cross-field validator** that checks if password === confirmPassword
   - Display validation errors
   - On success, navigate to login (or auto-login)

3. Create `auth.routes.ts`:
   - `/auth/login` → `LoginComponent`
   - `/auth/register` → `RegisterComponent`

4. Add lazy loading for auth routes in `app.routes.ts`:
   - Use `loadChildren` to lazy-load the auth module

> **🧠 Think About It:**  
> - What is a **cross-field validator**? How does it differ from a field-level validator?  
> - What is `loadChildren` and how does it achieve lazy loading?  
> - Should you auto-login after registration? What are the pros and cons?

**✅ Checkpoint:** Complete auth flow works: Register → Login → Protected pages are accessible → Logout → Protected pages redirect to login.

---

## Phase 5 — Production Readiness

**🎯 Objective:** Optimize performance, solve the N+1 query problem, add lazy loading on the frontend, and containerize the entire application with Docker.

**⏱ Estimated Time:** 3 days (2h/day)

---

### Exercise 5.1 — Diagnosing & Fixing the N+1 Problem

**Context:** The N+1 problem is a silent performance killer. When you load a list of products and each product lazy-loads its category, Hibernate executes 1 query for all products + N queries for N categories = N+1 queries total.

**Tasks:**

1. Enable SQL logging in `application.yml`:
   ```yaml
   spring:
     jpa:
       show-sql: true
       properties:
         hibernate:
           format_sql: true
   ```

2. Fetch the product list and **count the SQL queries** in the terminal. How many queries are executed for 20 products?

3. Fix it using `@EntityGraph`:
   - In `ProductRepository`, create a custom query method annotated with `@EntityGraph(attributePaths = {"category"})`
   - This tells Hibernate to fetch the category in a **single JOIN query**

4. Verify: count the queries again. You should see a single query with a JOIN.

5. **Alternative Fix:** Use a DTO Projection:
   - Create a JPQL query that directly selects into a DTO: `SELECT new com.smartinventory.dto.response.ProductResponse(p.id, p.name, ..., c.name) FROM Product p JOIN p.category c`
   - This is even more efficient — no entity hydration at all

> **🧠 Think About It:**  
> - Why is `FetchType.EAGER` not a good solution to the N+1 problem?  
> - What is the difference between `@EntityGraph` and a `JOIN FETCH` in JPQL?  
> - When is a DTO Projection better than an `@EntityGraph`? When is it worse?
> - How do you detect N+1 problems in production? (Hint: look at Hibernate statistics)

**✅ Checkpoint:** The product list endpoint executes exactly 1 SQL query regardless of how many products exist.

---

### Exercise 5.2 — Angular Deferred Loading

**Context:** Not all UI components need to load immediately. Heavy components like data tables, charts, or admin panels can be loaded **when they're needed** (e.g., when they scroll into view).

**Tasks:**

1. In `ProductListComponent`, wrap the product table/grid inside a `@defer` block:
   ```html
   @defer (on viewport) {
     <!-- The heavy product table component -->
   } @loading {
     <div class="skeleton-loader">Loading products...</div>
   } @placeholder {
     <div class="placeholder">Scroll down to see products</div>
   } @error {
     <div class="error">Failed to load products</div>
   }
   ```

2. Create a skeleton loader CSS animation for the loading state:
   - Gray pulsing boxes that mimic the layout of product cards
   - This provides better UX than a simple "Loading..." text

3. Experiment with other `@defer` triggers:
   - `(on idle)` — load when the browser is idle
   - `(on timer(2s))` — load after 2 seconds
   - `(when condition)` — load when a boolean condition is true

> **🧠 Think About It:**  
> - How does `@defer` differ from lazy loading with `loadComponent`?  
> - What is a "skeleton loader" and why is it better UX than a spinner?  
> - Can you defer-load a component that depends on data from a service? What challenges arise?

**✅ Checkpoint:** The product grid only loads when scrolled into view. The loading state shows skeleton placeholders.

---

### Exercise 5.3 — Docker Compose Deployment

**Context:** For production (and consistent development environments), we containerize everything: the database, the API, and the frontend.

**Tasks:**

1. Create a `Dockerfile` for the Spring Boot API:
   - Use a **multi-stage build**:
     - Stage 1: Build with Maven (use `maven:3.9-eclipse-temurin-21` image)
     - Stage 2: Run with a minimal JRE (use `eclipse-temurin:21-jre-alpine`)
   - The final image should only contain the JAR file and the JRE

2. Create a `Dockerfile` for the Angular app:
   - Stage 1: Build with Node (use `node:20-alpine`)
   - Stage 2: Serve with Nginx (use `nginx:alpine`)
   - Create an `nginx.conf` that:
     - Serves the Angular static files
     - Proxies `/api/**` requests to the Spring Boot container
     - Handles client-side routing (returns `index.html` for all non-file routes)

3. Create a `docker-compose.yml` at the project root:
   - **db** service: PostgreSQL 16
     - Persistent volume for data
     - Environment variables for credentials
   - **api** service: Spring Boot
     - Depends on `db`
     - Environment variables override `application.yml` settings
   - **ui** service: Angular (Nginx)
     - Depends on `api`
     - Maps port 80 to the Nginx container

4. Create a `.env` file for sensitive values (database password, JWT secret)

> **🧠 Think About It:**  
> - Why do we use multi-stage builds? What's the benefit in terms of image size?  
> - Why does Nginx need to proxy API requests instead of the Angular app calling the API directly?  
> - What is a Docker volume and why is it critical for the database container?  
> - What happens if the API container starts before the database is ready? How does `depends_on` handle this? (Hint: it doesn't fully solve the problem — research `healthcheck`)

**✅ Checkpoint:** Run `docker-compose up --build`. Open `http://localhost` — the full application works: login, browse products, add to cart, admin panel.

---

### Exercise 5.4 — Admin Panel (Putting It All Together)

**Context:** This final exercise combines everything you've learned. Build an admin panel where ADMIN users can manage products and categories.

**Tasks:**

1. **Backend:**
   - Create `AdminController` mapped to `/api/v1/admin/`:
     - `PUT /admin/products/{id}` → update product (price, stock)
     - `DELETE /admin/products/{id}` → delete product
     - `POST /admin/categories` → create category
   - Protect all endpoints with `@PreAuthorize("hasRole('ADMIN')")`

2. **Frontend:**
   - Create a `DashboardComponent` in `features/admin/`:
     - Display all products in an editable table
     - Inline editing for price and stock quantity
     - Delete button with confirmation dialog
     - "Add Category" form
   - Protect the `/admin` route with the `adminGuard`

3. Style the admin panel differently from the public pages (e.g., sidebar navigation, different color scheme)

> **🧠 Think About It:**  
> - Why do we need BOTH the `@PreAuthorize` annotation (backend) AND the route guard (frontend)?  
> - What would happen if you only protected the frontend and forgot the backend?  
> - How would you implement a soft delete (marking as deleted instead of actually deleting)?

**✅ Checkpoint:** An ADMIN user sees the admin panel with full CRUD capabilities. A regular USER gets redirected when trying to access `/admin`.

---

# 📋 SOLUTIONS

---

## Solution — Phase 1

### Solution 1.1 — Spring Boot Initialization

**`application.yml`**:

```yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/smart_inventory
    username: postgres
    password: secret
    driver-class-name: org.postgresql.Driver
  
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    properties:
      hibernate:
        format_sql: true
        dialect: org.hibernate.dialect.PostgreSQLDialect
```

> **Why `ddl-auto: update`?**  
> During development, Hibernate automatically creates/updates tables based on your entities. In production, you'd use `validate` or `none` and handle migrations with Flyway or Liquibase.

---

### Solution 1.2 — Data Model

**`model/Category.java`**:

```java
package com.smartinventory.model;

import jakarta.persistence.*;
import lombok.*;
import java.util.List;

@Entity
@Table(name = "categories")
@Getter @Setter
@NoArgsConstructor @AllArgsConstructor
@Builder
public class Category {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String name;

    private String description;

    @OneToMany(mappedBy = "category", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private List<Product> products;
}
```

**`model/Product.java`**:

```java
package com.smartinventory.model;

import jakarta.persistence.*;
import lombok.*;
import org.hibernate.annotations.CreationTimestamp;
import org.hibernate.annotations.UpdateTimestamp;
import java.math.BigDecimal;
import java.time.LocalDateTime;

@Entity
@Table(name = "products")
@Getter @Setter
@NoArgsConstructor @AllArgsConstructor
@Builder
public class Product {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    private String description;

    @Column(nullable = false, precision = 10, scale = 2)
    private BigDecimal price;

    @Column(name = "stock_quantity", nullable = false)
    private Integer stockQuantity;

    @Column(name = "image_url")
    private String imageUrl;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "category_id", nullable = false)
    private Category category;

    @CreationTimestamp
    @Column(name = "created_at", updatable = false)
    private LocalDateTime createdAt;

    @UpdateTimestamp
    @Column(name = "updated_at")
    private LocalDateTime updatedAt;
}
```

> **Why `BigDecimal` and not `Double`?**  
> `Double` uses floating-point arithmetic: `0.1 + 0.2 = 0.30000000000000004`. For financial calculations, `BigDecimal` provides exact precision. This is a standard across all financial software.

> **Why `FetchType.LAZY`?**  
> With `EAGER`, loading 100 products would also load 100 categories immediately, even if you don't need them. `LAZY` loads the category only when you access `product.getCategory()`. This is the performance-safe default.

---

### Solution 1.3 — Repositories

**`repository/CategoryRepository.java`**:

```java
package com.smartinventory.repository;

import com.smartinventory.model.Category;
import org.springframework.data.jpa.repository.JpaRepository;

public interface CategoryRepository extends JpaRepository<Category, Long> {
}
```

**`repository/ProductRepository.java`**:

```java
package com.smartinventory.repository;

import com.smartinventory.model.Product;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.List;

public interface ProductRepository extends JpaRepository<Product, Long> {
    List<Product> findByCategoryId(Long categoryId);
}
```

> **How does `findByCategoryId` work?**  
> Spring Data JPA parses the method name: `findBy` (query prefix) + `Category` (entity field) + `Id` (sub-field) → generates: `SELECT * FROM products WHERE category_id = ?`. No implementation needed.

---

### Solution 1.4 — DTOs

**`dto/response/ProductResponse.java`**:

```java
package com.smartinventory.dto.response;

import java.math.BigDecimal;

public record ProductResponse(
    Long id,
    String name,
    String description,
    BigDecimal price,
    Integer stockQuantity,
    String imageUrl,
    String categoryName
) {}
```

**`dto/response/CategoryResponse.java`**:

```java
package com.smartinventory.dto.response;

public record CategoryResponse(
    Long id,
    String name
) {}
```

**`dto/request/ProductRequest.java`**:

```java
package com.smartinventory.dto.request;

import java.math.BigDecimal;

public record ProductRequest(
    String name,
    String description,
    BigDecimal price,
    Integer stockQuantity,
    String imageUrl,
    Long categoryId
) {}
```

---

### Solution 1.5 — Service Layer

**`exception/ResourceNotFoundException.java`**:

```java
package com.smartinventory.exception;

public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String message) {
        super(message);
    }
}
```

**`service/ProductService.java`**:

```java
package com.smartinventory.service;

import com.smartinventory.dto.request.ProductRequest;
import com.smartinventory.dto.response.ProductResponse;
import java.util.List;

public interface ProductService {
    List<ProductResponse> getAllProducts();
    ProductResponse getProductById(Long id);
    ProductResponse createProduct(ProductRequest request);
}
```

**`service/impl/ProductServiceImpl.java`**:

```java
package com.smartinventory.service.impl;

import com.smartinventory.dto.request.ProductRequest;
import com.smartinventory.dto.response.ProductResponse;
import com.smartinventory.exception.ResourceNotFoundException;
import com.smartinventory.model.Category;
import com.smartinventory.model.Product;
import com.smartinventory.repository.CategoryRepository;
import com.smartinventory.repository.ProductRepository;
import com.smartinventory.service.ProductService;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import java.util.List;
import java.util.stream.Collectors;

@Service
@RequiredArgsConstructor
public class ProductServiceImpl implements ProductService {

    private final ProductRepository productRepository;
    private final CategoryRepository categoryRepository;

    @Override
    public List<ProductResponse> getAllProducts() {
        return productRepository.findAll()
                .stream()
                .map(this::mapToResponse)
                .collect(Collectors.toList());
    }

    @Override
    public ProductResponse getProductById(Long id) {
        Product product = productRepository.findById(id)
                .orElseThrow(() -> new ResourceNotFoundException(
                    "Product not found with id: " + id));
        return mapToResponse(product);
    }

    @Override
    public ProductResponse createProduct(ProductRequest request) {
        Category category = categoryRepository.findById(request.categoryId())
                .orElseThrow(() -> new ResourceNotFoundException(
                    "Category not found with id: " + request.categoryId()));

        Product product = Product.builder()
                .name(request.name())
                .description(request.description())
                .price(request.price())
                .stockQuantity(request.stockQuantity())
                .imageUrl(request.imageUrl())
                .category(category)
                .build();

        Product savedProduct = productRepository.save(product);
        return mapToResponse(savedProduct);
    }

    private ProductResponse mapToResponse(Product product) {
        return new ProductResponse(
                product.getId(),
                product.getName(),
                product.getDescription(),
                product.getPrice(),
                product.getStockQuantity(),
                product.getImageUrl(),
                product.getCategory().getName()
        );
    }
}
```

> **Why interface + implementation?**  
> 1. **Testability:** In unit tests, you can mock the interface.  
> 2. **Flexibility:** You can swap implementations without changing dependent code.  
> 3. **Industry standard:** This is the pattern used at Google, Apple, and Microsoft.

---

### Solution 1.6 — REST Controller

**`controller/ProductController.java`**:

```java
package com.smartinventory.controller;

import com.smartinventory.dto.request.ProductRequest;
import com.smartinventory.dto.response.ProductResponse;
import com.smartinventory.service.ProductService;
import lombok.RequiredArgsConstructor;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/api/v1/products")
@RequiredArgsConstructor
public class ProductController {

    private final ProductService productService;

    @GetMapping
    public ResponseEntity<List<ProductResponse>> getAllProducts() {
        return ResponseEntity.ok(productService.getAllProducts());
    }

    @GetMapping("/{id}")
    public ResponseEntity<ProductResponse> getProductById(@PathVariable Long id) {
        return ResponseEntity.ok(productService.getProductById(id));
    }

    @PostMapping
    public ResponseEntity<ProductResponse> createProduct(
            @RequestBody ProductRequest request) {
        ProductResponse response = productService.createProduct(request);
        return ResponseEntity.status(HttpStatus.CREATED).body(response);
    }
}
```

---

### Solution 1.7 — CORS Configuration

**`config/CorsConfig.java`**:

```java
package com.smartinventory.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class CorsConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
                .allowedOrigins("http://localhost:4200")
                .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                .allowedHeaders("*")
                .allowCredentials(true)
                .maxAge(3600);
    }
}
```

---

### Solution 1.8 — Angular Initialization

**`src/environments/environment.development.ts`**:

```typescript
export const environment = {
  production: false,
  apiUrl: 'http://localhost:8080/api/v1'
};
```

**`src/environments/environment.ts`**:

```typescript
export const environment = {
  production: true,
  apiUrl: '/api/v1'
};
```

**`src/app/app.config.ts`**:

```typescript
import { ApplicationConfig, provideZoneChangeDetection } from '@angular/core';
import { provideRouter } from '@angular/router';
import { provideHttpClient, withFetch } from '@angular/common/http';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [
    provideZoneChangeDetection({ eventCoalescing: true }),
    provideRouter(routes),
    provideHttpClient(withFetch())
  ]
};
```

> **Why `withFetch()`?**  
> By default, Angular's `HttpClient` uses `XMLHttpRequest`. `withFetch()` switches to the modern Fetch API, which supports streaming, better cancellation, and is required for SSR compatibility.

---

### Solution 1.9 — TypeScript Models

**`src/app/features/products/models/product.model.ts`**:

```typescript
export interface Product {
  id: number;
  name: string;
  description: string;
  price: number;
  stockQuantity: number;
  imageUrl: string;
  categoryName: string;
}

export interface ProductRequest {
  name: string;
  description: string;
  price: number;
  stockQuantity: number;
  imageUrl: string;
  categoryId: number;
}
```

---

### Solution 1.10 — Product Service

**`src/app/features/products/services/product.service.ts`**:

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Product, ProductRequest } from '../models/product.model';
import { environment } from '../../../../environments/environment';

@Injectable({
  providedIn: 'root'
})
export class ProductService {

  private readonly apiUrl = `${environment.apiUrl}/products`;

  constructor(private http: HttpClient) {}

  getAll(): Observable<Product[]> {
    return this.http.get<Product[]>(this.apiUrl);
  }

  getById(id: number): Observable<Product> {
    return this.http.get<Product>(`${this.apiUrl}/${id}`);
  }

  create(request: ProductRequest): Observable<Product> {
    return this.http.post<Product>(this.apiUrl, request);
  }
}
```

---

### Solution 1.11 — Product List Component

**`src/app/features/products/product-list/product-list.component.ts`**:

```typescript
import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { RouterLink } from '@angular/router';
import { ProductService } from '../services/product.service';
import { Product } from '../models/product.model';

@Component({
  selector: 'app-product-list',
  standalone: true,
  imports: [CommonModule, RouterLink],
  templateUrl: './product-list.component.html',
  styleUrl: './product-list.component.css'
})
export class ProductListComponent implements OnInit {
  
  products: Product[] = [];
  loading = true;
  error = '';

  constructor(private productService: ProductService) {}

  ngOnInit(): void {
    this.productService.getAll().subscribe({
      next: (data) => {
        this.products = data;
        this.loading = false;
      },
      error: (err) => {
        this.error = 'Failed to load products';
        this.loading = false;
        console.error('Error loading products:', err);
      }
    });
  }
}
```

**`src/app/features/products/product-list/product-list.component.html`**:

```html
<div class="product-list-container">
  <h1>Product Catalog</h1>

  @if (loading) {
    <div class="loading-spinner">
      <p>Loading products...</p>
    </div>
  }

  @if (error) {
    <div class="error-message">
      <p>{{ error }}</p>
    </div>
  }

  @if (!loading && !error) {
    <div class="product-grid">
      @for (product of products; track product.id) {
        <div class="product-card">
          <div class="product-image">
            @if (product.imageUrl) {
              <img [src]="product.imageUrl" [alt]="product.name" />
            } @else {
              <div class="no-image">No Image</div>
            }
          </div>
          <div class="product-info">
            <h3>{{ product.name }}</h3>
            <p class="category">{{ product.categoryName }}</p>
            <p class="price">{{ product.price | currency:'USD' }}</p>
            <p class="stock" [class.out-of-stock]="product.stockQuantity === 0">
              @if (product.stockQuantity > 0) {
                In Stock: {{ product.stockQuantity }}
              } @else {
                Out of Stock
              }
            </p>
          </div>
        </div>
      } @empty {
        <div class="empty-state">
          <p>No products found. Add your first product!</p>
        </div>
      }
    </div>
  }
</div>
```

---

### Solution 1.12 — Navigation & Layout

**`src/app/shared/components/navbar/navbar.component.ts`**:

```typescript
import { Component } from '@angular/core';
import { RouterLink, RouterLinkActive } from '@angular/router';

@Component({
  selector: 'app-navbar',
  standalone: true,
  imports: [RouterLink, RouterLinkActive],
  templateUrl: './navbar.component.html',
  styleUrl: './navbar.component.css'
})
export class NavbarComponent {}
```

**`src/app/shared/components/navbar/navbar.component.html`**:

```html
<nav class="navbar">
  <div class="navbar-brand">
    <a routerLink="/">🛒 Smart Inventory</a>
  </div>
  <ul class="navbar-links">
    <li>
      <a routerLink="/products" routerLinkActive="active">Products</a>
    </li>
    <li>
      <a routerLink="/cart" routerLinkActive="active">Cart</a>
    </li>
    <li>
      <a routerLink="/auth/login" routerLinkActive="active">Login</a>
    </li>
  </ul>
</nav>
```

**`src/app/app.component.ts`**:

```typescript
import { Component } from '@angular/core';
import { RouterOutlet } from '@angular/router';
import { NavbarComponent } from './shared/components/navbar/navbar.component';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet, NavbarComponent],
  template: `
    <app-navbar />
    <main class="main-content">
      <router-outlet />
    </main>
  `,
  styles: [`
    .main-content {
      max-width: 1200px;
      margin: 0 auto;
      padding: 2rem;
    }
  `]
})
export class AppComponent {}
```

**`src/app/app.routes.ts`**:

```typescript
import { Routes } from '@angular/router';
import { ProductListComponent } from './features/products/product-list/product-list.component';

export const routes: Routes = [
  { path: '', redirectTo: '/products', pathMatch: 'full' },
  { path: 'products', component: ProductListComponent },
];
```

---

## Solution — Phase 2

### Solution 2.1 — Bean Validation

**`dto/request/ProductRequest.java`** (updated):

```java
package com.smartinventory.dto.request;

import jakarta.validation.constraints.*;
import java.math.BigDecimal;

public record ProductRequest(
    @NotBlank(message = "Product name is required")
    @Size(max = 100, message = "Name must be at most 100 characters")
    String name,

    @Size(max = 500, message = "Description must be at most 500 characters")
    String description,

    @NotNull(message = "Price is required")
    @DecimalMin(value = "0.01", message = "Price must be greater than 0")
    BigDecimal price,

    @NotNull(message = "Stock quantity is required")
    @Min(value = 0, message = "Stock quantity cannot be negative")
    Integer stockQuantity,

    String imageUrl,

    @NotNull(message = "Category is required")
    Long categoryId
) {}
```

**Controller update** — add `@Valid`:

```java
@PostMapping
public ResponseEntity<ProductResponse> createProduct(
        @Valid @RequestBody ProductRequest request) {
    // ...
}
```

---

### Solution 2.2 — Global Error Handler

**`dto/response/ErrorResponse.java`**:

```java
package com.smartinventory.dto.response;

import java.time.LocalDateTime;
import java.util.Map;

public record ErrorResponse(
    LocalDateTime timestamp,
    int status,
    String error,
    Map<String, String> errors
) {}
```

**`exception/GlobalExceptionHandler.java`**:

```java
package com.smartinventory.exception;

import com.smartinventory.dto.response.ErrorResponse;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.FieldError;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import java.time.LocalDateTime;
import java.util.HashMap;
import java.util.Map;

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidationErrors(
            MethodArgumentNotValidException ex) {
        
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getFieldErrors().forEach((FieldError fieldError) -> {
            errors.put(fieldError.getField(), fieldError.getDefaultMessage());
        });

        ErrorResponse response = new ErrorResponse(
                LocalDateTime.now(),
                HttpStatus.BAD_REQUEST.value(),
                "Validation Failed",
                errors
        );

        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(response);
    }

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleResourceNotFound(
            ResourceNotFoundException ex) {
        
        ErrorResponse response = new ErrorResponse(
                LocalDateTime.now(),
                HttpStatus.NOT_FOUND.value(),
                ex.getMessage(),
                null
        );

        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(response);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGenericException(Exception ex) {
        // Log the full exception server-side
        ex.printStackTrace();

        ErrorResponse response = new ErrorResponse(
                LocalDateTime.now(),
                HttpStatus.INTERNAL_SERVER_ERROR.value(),
                "An unexpected error occurred",
                null
        );

        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(response);
    }
}
```

---

### Solution 2.3 — Product Form Component

**`src/app/features/products/product-form/product-form.component.ts`**:

```typescript
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ReactiveFormsModule, FormGroup, FormControl, Validators } from '@angular/forms';
import { Router } from '@angular/router';
import { ProductService } from '../services/product.service';
import { HttpErrorResponse } from '@angular/common/http';

@Component({
  selector: 'app-product-form',
  standalone: true,
  imports: [CommonModule, ReactiveFormsModule],
  templateUrl: './product-form.component.html',
  styleUrl: './product-form.component.css'
})
export class ProductFormComponent {

  serverErrors: { [key: string]: string } = {};
  generalError = '';
  submitting = false;

  form = new FormGroup({
    name: new FormControl('', [
      Validators.required,
      Validators.minLength(2),
      Validators.maxLength(100)
    ]),
    description: new FormControl('', [
      Validators.maxLength(500)
    ]),
    price: new FormControl<number | null>(null, [
      Validators.required,
      Validators.min(0.01)
    ]),
    stockQuantity: new FormControl<number | null>(null, [
      Validators.required,
      Validators.min(0)
    ]),
    imageUrl: new FormControl(''),
    categoryId: new FormControl<number | null>(null, [
      Validators.required
    ])
  });

  constructor(
    private productService: ProductService,
    private router: Router
  ) {
    // Clear server errors when user starts typing
    this.form.valueChanges.subscribe(() => {
      this.serverErrors = {};
      this.generalError = '';
    });
  }

  onSubmit(): void {
    // Mark all fields as touched to show validation errors
    this.form.markAllAsTouched();

    if (this.form.invalid) {
      return;
    }

    this.submitting = true;

    this.productService.create(this.form.value as any).subscribe({
      next: () => {
        this.router.navigate(['/products']);
      },
      error: (err: HttpErrorResponse) => {
        this.submitting = false;
        if (err.status === 400 && err.error?.errors) {
          this.serverErrors = err.error.errors;
        } else {
          this.generalError = err.error?.error || 'An unexpected error occurred';
        }
      }
    });
  }

  // Helper to check field errors in template
  hasError(field: string, errorType: string): boolean {
    const control = this.form.get(field);
    return !!(control?.hasError(errorType) && control?.touched);
  }
}
```

**`src/app/features/products/product-form/product-form.component.html`**:

```html
<div class="form-container">
  <h2>Add New Product</h2>

  @if (generalError) {
    <div class="alert alert-danger">{{ generalError }}</div>
  }

  <form [formGroup]="form" (ngSubmit)="onSubmit()">
    
    <div class="form-group">
      <label for="name">Product Name *</label>
      <input id="name" type="text" formControlName="name"
             [class.invalid]="hasError('name', 'required') || serverErrors['name']" />
      @if (hasError('name', 'required')) {
        <span class="error">Product name is required</span>
      }
      @if (hasError('name', 'minlength')) {
        <span class="error">Name must be at least 2 characters</span>
      }
      @if (serverErrors['name']) {
        <span class="error server-error">{{ serverErrors['name'] }}</span>
      }
    </div>

    <div class="form-group">
      <label for="description">Description</label>
      <textarea id="description" formControlName="description"></textarea>
      @if (hasError('description', 'maxlength')) {
        <span class="error">Description must be at most 500 characters</span>
      }
    </div>

    <div class="form-group">
      <label for="price">Price *</label>
      <input id="price" type="number" step="0.01" formControlName="price"
             [class.invalid]="hasError('price', 'required')" />
      @if (hasError('price', 'required')) {
        <span class="error">Price is required</span>
      }
      @if (hasError('price', 'min')) {
        <span class="error">Price must be greater than 0</span>
      }
      @if (serverErrors['price']) {
        <span class="error server-error">{{ serverErrors['price'] }}</span>
      }
    </div>

    <div class="form-group">
      <label for="stockQuantity">Stock Quantity *</label>
      <input id="stockQuantity" type="number" formControlName="stockQuantity"
             [class.invalid]="hasError('stockQuantity', 'required')" />
      @if (hasError('stockQuantity', 'required')) {
        <span class="error">Stock quantity is required</span>
      }
      @if (hasError('stockQuantity', 'min')) {
        <span class="error">Stock quantity cannot be negative</span>
      }
    </div>

    <div class="form-group">
      <label for="categoryId">Category *</label>
      <select id="categoryId" formControlName="categoryId">
        <option [ngValue]="null" disabled>Select a category</option>
        <!-- Categories would be loaded from the server -->
      </select>
      @if (hasError('categoryId', 'required')) {
        <span class="error">Category is required</span>
      }
    </div>

    <button type="submit" [disabled]="submitting" class="btn-primary">
      @if (submitting) {
        Creating...
      } @else {
        Create Product
      }
    </button>
  </form>
</div>
```

---

### Solution 2.5 — Custom Validator

**`src/app/features/products/validators/price.validator.ts`**:

```typescript
import { AbstractControl, ValidationErrors } from '@angular/forms';

export function priceFormatValidator(control: AbstractControl): ValidationErrors | null {
  const value = control.value;
  
  if (value === null || value === undefined || value === '') {
    return null; // Let 'required' validator handle empty values
  }

  const stringValue = value.toString();
  const decimalIndex = stringValue.indexOf('.');
  
  if (decimalIndex !== -1) {
    const decimals = stringValue.length - decimalIndex - 1;
    if (decimals > 2) {
      return { invalidPrice: true };
    }
  }

  return null;
}
```

Usage in FormControl:

```typescript
price: new FormControl<number | null>(null, [
  Validators.required,
  Validators.min(0.01),
  priceFormatValidator
]),
```

---

## Solution — Phase 3

### Solution 3.1 — Backend Pagination

**`dto/response/PageResponse.java`**:

```java
package com.smartinventory.dto.response;

import java.util.List;

public record PageResponse<T>(
    List<T> content,
    int pageNumber,
    int pageSize,
    long totalElements,
    int totalPages,
    boolean last
) {}
```

**`service/ProductService.java`** (updated):

```java
package com.smartinventory.service;

import com.smartinventory.dto.request.ProductRequest;
import com.smartinventory.dto.response.PageResponse;
import com.smartinventory.dto.response.ProductResponse;
import org.springframework.data.domain.Pageable;

public interface ProductService {
    PageResponse<ProductResponse> getAllProducts(Pageable pageable);
    ProductResponse getProductById(Long id);
    ProductResponse createProduct(ProductRequest request);
}
```

**`service/impl/ProductServiceImpl.java`** (updated `getAllProducts`):

```java
@Override
public PageResponse<ProductResponse> getAllProducts(Pageable pageable) {
    Page<Product> page = productRepository.findAll(pageable);

    List<ProductResponse> content = page.getContent()
            .stream()
            .map(this::mapToResponse)
            .collect(Collectors.toList());

    return new PageResponse<>(
            content,
            page.getNumber(),
            page.getSize(),
            page.getTotalElements(),
            page.getTotalPages(),
            page.isLast()
    );
}
```

**`controller/ProductController.java`** (updated):

```java
@GetMapping
public ResponseEntity<PageResponse<ProductResponse>> getAllProducts(
        @PageableDefault(size = 10, sort = "createdAt", direction = Sort.Direction.DESC)
        Pageable pageable) {
    return ResponseEntity.ok(productService.getAllProducts(pageable));
}
```

---

### Solution 3.2 — JPA Specifications

**`repository/ProductSpecification.java`**:

```java
package com.smartinventory.repository;

import com.smartinventory.model.Product;
import org.springframework.data.jpa.domain.Specification;
import java.math.BigDecimal;

public class ProductSpecification {

    public static Specification<Product> hasName(String name) {
        return (root, query, cb) -> {
            if (name == null || name.isBlank()) return null;
            return cb.like(
                cb.lower(root.get("name")),
                "%" + name.toLowerCase() + "%"
            );
        };
    }

    public static Specification<Product> hasCategory(Long categoryId) {
        return (root, query, cb) -> {
            if (categoryId == null) return null;
            return cb.equal(root.get("category").get("id"), categoryId);
        };
    }

    public static Specification<Product> hasPriceRange(BigDecimal min, BigDecimal max) {
        return (root, query, cb) -> {
            if (min == null && max == null) return null;
            if (min != null && max != null) {
                return cb.between(root.get("price"), min, max);
            } else if (min != null) {
                return cb.greaterThanOrEqualTo(root.get("price"), min);
            } else {
                return cb.lessThanOrEqualTo(root.get("price"), max);
            }
        };
    }

    public static Specification<Product> hasMinStock(Integer minStock) {
        return (root, query, cb) -> {
            if (minStock == null) return null;
            return cb.greaterThanOrEqualTo(root.get("stockQuantity"), minStock);
        };
    }
}
```

**Updated `ProductRepository`**:

```java
package com.smartinventory.repository;

import com.smartinventory.model.Product;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.JpaSpecificationExecutor;

public interface ProductRepository extends JpaRepository<Product, Long>,
                                           JpaSpecificationExecutor<Product> {
    // ...
}
```

**Updated service method:**

```java
@Override
public PageResponse<ProductResponse> searchProducts(
        String search, Long categoryId,
        BigDecimal minPrice, BigDecimal maxPrice,
        Pageable pageable) {

    Specification<Product> spec = Specification
            .where(ProductSpecification.hasName(search))
            .and(ProductSpecification.hasCategory(categoryId))
            .and(ProductSpecification.hasPriceRange(minPrice, maxPrice));

    Page<Product> page = productRepository.findAll(spec, pageable);

    List<ProductResponse> content = page.getContent()
            .stream()
            .map(this::mapToResponse)
            .collect(Collectors.toList());

    return new PageResponse<>(
            content, page.getNumber(), page.getSize(),
            page.getTotalElements(), page.getTotalPages(), page.isLast()
    );
}
```

---

### Solution 3.3 — Angular Signals

**`product-list.component.ts`** (refactored with Signals):

```typescript
import { Component, OnInit, signal, computed } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ProductService } from '../services/product.service';
import { Product } from '../models/product.model';
import { PageResponse } from '../models/product.model';

@Component({
  selector: 'app-product-list',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './product-list.component.html'
})
export class ProductListComponent implements OnInit {

  // Signals for reactive state
  products = signal<Product[]>([]);
  loading = signal(true);
  currentPage = signal(0);
  totalPages = signal(0);
  totalElements = signal(0);
  pageSize = signal(10);
  error = signal('');

  // Computed signals (derived state)
  isEmpty = computed(() => this.products().length === 0 && !this.loading());
  isFirstPage = computed(() => this.currentPage() === 0);
  isLastPage = computed(() => this.currentPage() >= this.totalPages() - 1);

  constructor(private productService: ProductService) {}

  ngOnInit(): void {
    this.loadProducts();
  }

  loadProducts(): void {
    this.loading.set(true);
    this.productService.search('', this.currentPage(), this.pageSize())
      .subscribe({
        next: (response: PageResponse<Product>) => {
          this.products.set(response.content);
          this.totalPages.set(response.totalPages);
          this.totalElements.set(response.totalElements);
          this.loading.set(false);
        },
        error: (err) => {
          this.error.set('Failed to load products');
          this.loading.set(false);
        }
      });
  }

  nextPage(): void {
    if (!this.isLastPage()) {
      this.currentPage.update(p => p + 1);
      this.loadProducts();
    }
  }

  previousPage(): void {
    if (!this.isFirstPage()) {
      this.currentPage.update(p => p - 1);
      this.loadProducts();
    }
  }

  goToPage(page: number): void {
    this.currentPage.set(page);
    this.loadProducts();
  }
}
```

---

### Solution 3.4 — Live Search with RxJS

**Updated `product-list.component.ts`** (with search):

```typescript
import { Component, OnInit, OnDestroy, signal, computed } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ReactiveFormsModule, FormControl } from '@angular/forms';
import { Subject, takeUntil } from 'rxjs';
import { debounceTime, distinctUntilChanged, switchMap, tap } from 'rxjs/operators';
import { ProductService } from '../services/product.service';
import { Product, PageResponse } from '../models/product.model';

@Component({
  selector: 'app-product-list',
  standalone: true,
  imports: [CommonModule, ReactiveFormsModule],
  templateUrl: './product-list.component.html'
})
export class ProductListComponent implements OnInit, OnDestroy {

  // Search control
  searchControl = new FormControl('');

  // Signals
  products = signal<Product[]>([]);
  loading = signal(true);
  currentPage = signal(0);
  totalPages = signal(0);
  pageSize = signal(10);

  // Computed
  isEmpty = computed(() => this.products().length === 0 && !this.loading());

  // Cleanup
  private destroy$ = new Subject<void>();

  constructor(private productService: ProductService) {}

  ngOnInit(): void {
    // Initial load
    this.loadProducts('');

    // Live search pipeline
    this.searchControl.valueChanges.pipe(
      debounceTime(300),              // Wait 300ms after last keystroke
      distinctUntilChanged(),         // Only if value actually changed
      tap(() => {
        this.loading.set(true);       // Show loading indicator
        this.currentPage.set(0);      // Reset to first page
      }),
      switchMap(term =>               // Cancel previous request
        this.productService.search(
          term || '', 
          0, 
          this.pageSize()
        )
      ),
      takeUntil(this.destroy$)        // Clean up on destroy
    ).subscribe({
      next: (response: PageResponse<Product>) => {
        this.products.set(response.content);
        this.totalPages.set(response.totalPages);
        this.loading.set(false);
      },
      error: () => {
        this.loading.set(false);
      }
    });
  }

  loadProducts(search: string): void {
    this.loading.set(true);
    this.productService.search(search, this.currentPage(), this.pageSize())
      .pipe(takeUntil(this.destroy$))
      .subscribe({
        next: (response) => {
          this.products.set(response.content);
          this.totalPages.set(response.totalPages);
          this.loading.set(false);
        },
        error: () => this.loading.set(false)
      });
  }

  ngOnDestroy(): void {
    this.destroy$.next();
    this.destroy$.complete();
  }
}
```

**Updated `product.service.ts`**:

```typescript
import { Injectable } from '@angular/core';
import { HttpClient, HttpParams } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Product, ProductRequest, PageResponse } from '../models/product.model';
import { environment } from '../../../../environments/environment';

@Injectable({ providedIn: 'root' })
export class ProductService {

  private readonly apiUrl = `${environment.apiUrl}/products`;

  constructor(private http: HttpClient) {}

  search(query: string, page: number, size: number): Observable<PageResponse<Product>> {
    let params = new HttpParams()
      .set('page', page.toString())
      .set('size', size.toString());
    
    if (query) {
      params = params.set('search', query);
    }

    return this.http.get<PageResponse<Product>>(this.apiUrl, { params });
  }

  getById(id: number): Observable<Product> {
    return this.http.get<Product>(`${this.apiUrl}/${id}`);
  }

  create(request: ProductRequest): Observable<Product> {
    return this.http.post<Product>(this.apiUrl, request);
  }
}
```

**`models/product.model.ts`** (add `PageResponse`):

```typescript
// ... existing interfaces ...

export interface PageResponse<T> {
  content: T[];
  pageNumber: number;
  pageSize: number;
  totalElements: number;
  totalPages: number;
  last: boolean;
}
```

---

### Solution 3.5 — Pagination Component

**`src/app/shared/components/pagination/pagination.component.ts`**:

```typescript
import { Component, input, output } from '@angular/core';

@Component({
  selector: 'app-pagination',
  standalone: true,
  templateUrl: './pagination.component.html',
  styleUrl: './pagination.component.css'
})
export class PaginationComponent {

  currentPage = input.required<number>();
  totalPages = input.required<number>();

  pageChange = output<number>();

  get visiblePages(): number[] {
    const pages: number[] = [];
    const current = this.currentPage();
    const total = this.totalPages();
    
    let start = Math.max(0, current - 2);
    let end = Math.min(total - 1, current + 2);

    // Adjust to always show 5 pages if possible
    if (end - start < 4) {
      if (start === 0) {
        end = Math.min(total - 1, start + 4);
      } else {
        start = Math.max(0, end - 4);
      }
    }

    for (let i = start; i <= end; i++) {
      pages.push(i);
    }

    return pages;
  }

  goToPage(page: number): void {
    if (page >= 0 && page < this.totalPages()) {
      this.pageChange.emit(page);
    }
  }
}
```

**`pagination.component.html`**:

```html
<div class="pagination">
  <button 
    class="page-btn" 
    [disabled]="currentPage() === 0"
    (click)="goToPage(currentPage() - 1)">
    ← Previous
  </button>

  @for (page of visiblePages; track page) {
    <button 
      class="page-btn"
      [class.active]="page === currentPage()"
      (click)="goToPage(page)">
      {{ page + 1 }}
    </button>
  }

  <button 
    class="page-btn"
    [disabled]="currentPage() >= totalPages() - 1"
    (click)="goToPage(currentPage() + 1)">
    Next →
  </button>

  <span class="page-info">
    Page {{ currentPage() + 1 }} of {{ totalPages() }}
  </span>
</div>
```

---

## Solution — Phase 4

### Solution 4.1 — User Entity

**`model/Role.java`**:

```java
package com.smartinventory.model;

public enum Role {
    USER,
    ADMIN
}
```

**`model/User.java`**:

```java
package com.smartinventory.model;

import jakarta.persistence.*;
import lombok.*;
import org.hibernate.annotations.CreationTimestamp;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;
import java.time.LocalDateTime;
import java.util.Collection;
import java.util.List;

@Entity
@Table(name = "users")
@Getter @Setter
@NoArgsConstructor @AllArgsConstructor
@Builder
public class User implements UserDetails {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "first_name", nullable = false)
    private String firstName;

    @Column(name = "last_name", nullable = false)
    private String lastName;

    @Column(nullable = false, unique = true)
    private String email;

    @Column(nullable = false)
    private String password;

    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private Role role;

    @CreationTimestamp
    @Column(name = "created_at", updatable = false)
    private LocalDateTime createdAt;

    // --- UserDetails implementation ---

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return List.of(new SimpleGrantedAuthority("ROLE_" + role.name()));
    }

    @Override
    public String getUsername() {
        return email;
    }

    @Override
    public boolean isAccountNonExpired() { return true; }

    @Override
    public boolean isAccountNonLocked() { return true; }

    @Override
    public boolean isCredentialsNonExpired() { return true; }

    @Override
    public boolean isEnabled() { return true; }
}
```

**`repository/UserRepository.java`**:

```java
package com.smartinventory.repository;

import com.smartinventory.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.Optional;

public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
    boolean existsByEmail(String email);
}
```

---

### Solution 4.2 — JWT Service

**Add to `pom.xml`**:

```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.12.5</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.12.5</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.12.5</version>
    <scope>runtime</scope>
</dependency>
```

**`application.yml`** (add):

```yaml
application:
  security:
    jwt:
      secret-key: "my-super-secret-key-that-is-at-least-256-bits-long-for-hs256"
      expiration: 86400000  # 24 hours in milliseconds
```

**`security/JwtService.java`**:

```java
package com.smartinventory.security;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.io.Decoders;
import io.jsonwebtoken.security.Keys;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.stereotype.Service;

import javax.crypto.SecretKey;
import java.util.Date;
import java.util.Map;
import java.util.function.Function;

@Service
public class JwtService {

    @Value("${application.security.jwt.secret-key}")
    private String secretKey;

    @Value("${application.security.jwt.expiration}")
    private long jwtExpiration;

    public String generateToken(UserDetails userDetails) {
        return generateToken(Map.of(), userDetails);
    }

    public String generateToken(Map<String, Object> extraClaims, UserDetails userDetails) {
        return Jwts.builder()
                .claims(extraClaims)
                .subject(userDetails.getUsername())
                .issuedAt(new Date())
                .expiration(new Date(System.currentTimeMillis() + jwtExpiration))
                .signWith(getSigningKey())
                .compact();
    }

    public String extractUsername(String token) {
        return extractClaim(token, Claims::getSubject);
    }

    public <T> T extractClaim(String token, Function<Claims, T> claimsResolver) {
        final Claims claims = extractAllClaims(token);
        return claimsResolver.apply(claims);
    }

    public boolean isTokenValid(String token, UserDetails userDetails) {
        final String username = extractUsername(token);
        return username.equals(userDetails.getUsername()) && !isTokenExpired(token);
    }

    private boolean isTokenExpired(String token) {
        return extractClaim(token, Claims::getExpiration).before(new Date());
    }

    private Claims extractAllClaims(String token) {
        return Jwts.parser()
                .verifyWith(getSigningKey())
                .build()
                .parseSignedClaims(token)
                .getPayload();
    }

    private SecretKey getSigningKey() {
        byte[] keyBytes = Decoders.BASE64.decode(secretKey);
        return Keys.hmacShaKeyFor(keyBytes);
    }
}
```

---

### Solution 4.3 — Security Configuration

**`security/JwtAuthenticationFilter.java`**:

```java
package com.smartinventory.security;

import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import lombok.RequiredArgsConstructor;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;
import org.springframework.web.filter.OncePerRequestFilter;

import java.io.IOException;

@Component
@RequiredArgsConstructor
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    private final JwtService jwtService;
    private final UserDetailsService userDetailsService;

    @Override
    protected void doFilterInternal(
            HttpServletRequest request,
            HttpServletResponse response,
            FilterChain filterChain
    ) throws ServletException, IOException {

        final String authHeader = request.getHeader("Authorization");

        if (authHeader == null || !authHeader.startsWith("Bearer ")) {
            filterChain.doFilter(request, response);
            return;
        }

        final String jwt = authHeader.substring(7);
        final String userEmail = jwtService.extractUsername(jwt);

        if (userEmail != null && SecurityContextHolder.getContext().getAuthentication() == null) {
            UserDetails userDetails = userDetailsService.loadUserByUsername(userEmail);

            if (jwtService.isTokenValid(jwt, userDetails)) {
                UsernamePasswordAuthenticationToken authToken =
                        new UsernamePasswordAuthenticationToken(
                                userDetails,
                                null,
                                userDetails.getAuthorities()
                        );
                authToken.setDetails(
                        new WebAuthenticationDetailsSource().buildDetails(request)
                );
                SecurityContextHolder.getContext().setAuthentication(authToken);
            }
        }

        filterChain.doFilter(request, response);
    }
}
```

**`security/UserDetailsServiceImpl.java`**:

```java
package com.smartinventory.security;

import com.smartinventory.repository.UserRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

@Service
@RequiredArgsConstructor
public class UserDetailsServiceImpl implements UserDetailsService {

    private final UserRepository userRepository;

    @Override
    public UserDetails loadUserByUsername(String email) throws UsernameNotFoundException {
        return userRepository.findByEmail(email)
                .orElseThrow(() -> new UsernameNotFoundException(
                        "User not found with email: " + email));
    }
}
```

**`config/SecurityConfig.java`**:

```java
package com.smartinventory.config;

import com.smartinventory.security.JwtAuthenticationFilter;
import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.dao.DaoAuthenticationProvider;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configurers.AbstractHttpConfigurer;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
@EnableWebSecurity
@EnableMethodSecurity
@RequiredArgsConstructor
public class SecurityConfig {

    private final JwtAuthenticationFilter jwtAuthFilter;
    private final UserDetailsService userDetailsService;

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(AbstractHttpConfigurer::disable)
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/v1/auth/**").permitAll()
                .requestMatchers("/api/v1/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            )
            .authenticationProvider(authenticationProvider())
            .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }

    @Bean
    public AuthenticationProvider authenticationProvider() {
        DaoAuthenticationProvider provider = new DaoAuthenticationProvider();
        provider.setUserDetailsService(userDetailsService);
        provider.setPasswordEncoder(passwordEncoder());
        return provider;
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public AuthenticationManager authenticationManager(
            AuthenticationConfiguration config) throws Exception {
        return config.getAuthenticationManager();
    }
}
```

---

### Solution 4.4 — Auth Service & Controller

**`service/AuthService.java`**:

```java
package com.smartinventory.service;

import com.smartinventory.dto.request.LoginRequest;
import com.smartinventory.dto.request.RegisterRequest;
import com.smartinventory.dto.response.AuthResponse;

public interface AuthService {
    AuthResponse register(RegisterRequest request);
    AuthResponse login(LoginRequest request);
}
```

**`service/impl/AuthServiceImpl.java`**:

```java
package com.smartinventory.service.impl;

import com.smartinventory.dto.request.LoginRequest;
import com.smartinventory.dto.request.RegisterRequest;
import com.smartinventory.dto.response.AuthResponse;
import com.smartinventory.exception.BusinessException;
import com.smartinventory.model.Role;
import com.smartinventory.model.User;
import com.smartinventory.repository.UserRepository;
import com.smartinventory.security.JwtService;
import com.smartinventory.service.AuthService;
import lombok.RequiredArgsConstructor;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.stereotype.Service;

@Service
@RequiredArgsConstructor
public class AuthServiceImpl implements AuthService {

    private final UserRepository userRepository;
    private final PasswordEncoder passwordEncoder;
    private final JwtService jwtService;
    private final AuthenticationManager authenticationManager;

    @Override
    public AuthResponse register(RegisterRequest request) {
        if (userRepository.existsByEmail(request.email())) {
            throw new BusinessException("Email already registered");
        }

        if (!request.password().equals(request.confirmPassword())) {
            throw new BusinessException("Passwords do not match");
        }

        User user = User.builder()
                .firstName(request.firstName())
                .lastName(request.lastName())
                .email(request.email())
                .password(passwordEncoder.encode(request.password()))
                .role(Role.USER)
                .build();

        userRepository.save(user);

        String token = jwtService.generateToken(user);

        return new AuthResponse(token, user.getEmail(), user.getRole().name());
    }

    @Override
    public AuthResponse login(LoginRequest request) {
        authenticationManager.authenticate(
                new UsernamePasswordAuthenticationToken(
                        request.email(),
                        request.password()
                )
        );

        User user = userRepository.findByEmail(request.email())
                .orElseThrow(() -> new BusinessException("User not found"));

        String token = jwtService.generateToken(user);

        return new AuthResponse(token, user.getEmail(), user.getRole().name());
    }
}
```

**`exception/BusinessException.java`**:

```java
package com.smartinventory.exception;

public class BusinessException extends RuntimeException {
    public BusinessException(String message) {
        super(message);
    }
}
```

**`controller/AuthController.java`**:

```java
package com.smartinventory.controller;

import com.smartinventory.dto.request.LoginRequest;
import com.smartinventory.dto.request.RegisterRequest;
import com.smartinventory.dto.response.AuthResponse;
import com.smartinventory.service.AuthService;
import jakarta.validation.Valid;
import lombok.RequiredArgsConstructor;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/v1/auth")
@RequiredArgsConstructor
public class AuthController {

    private final AuthService authService;

    @PostMapping("/register")
    public ResponseEntity<AuthResponse> register(
            @Valid @RequestBody RegisterRequest request) {
        return ResponseEntity.status(HttpStatus.CREATED)
                .body(authService.register(request));
    }

    @PostMapping("/login")
    public ResponseEntity<AuthResponse> login(
            @Valid @RequestBody LoginRequest request) {
        return ResponseEntity.ok(authService.login(request));
    }
}
```

---

### Solution 4.5–4.6 — Angular Auth & Interceptors

**`src/app/core/services/storage.service.ts`**:

```typescript
import { Injectable } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class StorageService {

  private readonly TOKEN_KEY = 'auth_token';

  setToken(token: string): void {
    localStorage.setItem(this.TOKEN_KEY, token);
  }

  getToken(): string | null {
    return localStorage.getItem(this.TOKEN_KEY);
  }

  removeToken(): void {
    localStorage.removeItem(this.TOKEN_KEY);
  }

  isLoggedIn(): boolean {
    const token = this.getToken();
    if (!token) return false;
    // Check if token is expired
    try {
      const payload = JSON.parse(atob(token.split('.')[1]));
      return payload.exp * 1000 > Date.now();
    } catch {
      return false;
    }
  }
}
```

**`src/app/core/services/auth.service.ts`**:

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Router } from '@angular/router';
import { Observable, tap } from 'rxjs';
import { StorageService } from './storage.service';
import { environment } from '../../../environments/environment';

export interface LoginRequest {
  email: string;
  password: string;
}

export interface RegisterRequest {
  firstName: string;
  lastName: string;
  email: string;
  password: string;
  confirmPassword: string;
}

export interface AuthResponse {
  token: string;
  email: string;
  role: string;
}

@Injectable({ providedIn: 'root' })
export class AuthService {

  private readonly apiUrl = `${environment.apiUrl}/auth`;

  constructor(
    private http: HttpClient,
    private storage: StorageService,
    private router: Router
  ) {}

  login(request: LoginRequest): Observable<AuthResponse> {
    return this.http.post<AuthResponse>(`${this.apiUrl}/login`, request)
      .pipe(
        tap(response => this.storage.setToken(response.token))
      );
  }

  register(request: RegisterRequest): Observable<AuthResponse> {
    return this.http.post<AuthResponse>(`${this.apiUrl}/register`, request);
  }

  logout(): void {
    this.storage.removeToken();
    this.router.navigate(['/auth/login']);
  }

  isLoggedIn(): boolean {
    return this.storage.isLoggedIn();
  }

  getUserRole(): string | null {
    const token = this.storage.getToken();
    if (!token) return null;
    try {
      const payload = JSON.parse(atob(token.split('.')[1]));
      return payload.role || null;
    } catch {
      return null;
    }
  }
}
```

**`src/app/core/interceptors/auth.interceptor.ts`**:

```typescript
import { HttpInterceptorFn } from '@angular/common/http';
import { inject } from '@angular/core';
import { StorageService } from '../services/storage.service';

export const authInterceptor: HttpInterceptorFn = (req, next) => {
  // Skip auth endpoints
  if (req.url.includes('/auth/')) {
    return next(req);
  }

  const storage = inject(StorageService);
  const token = storage.getToken();

  if (token) {
    const clonedReq = req.clone({
      setHeaders: {
        Authorization: `Bearer ${token}`
      }
    });
    return next(clonedReq);
  }

  return next(req);
};
```

**`src/app/core/interceptors/error.interceptor.ts`**:

```typescript
import { HttpInterceptorFn } from '@angular/common/http';
import { inject } from '@angular/core';
import { Router } from '@angular/router';
import { catchError, throwError } from 'rxjs';
import { StorageService } from '../services/storage.service';

export const errorInterceptor: HttpInterceptorFn = (req, next) => {
  const router = inject(Router);
  const storage = inject(StorageService);

  return next(req).pipe(
    catchError(error => {
      if (error.status === 401) {
        storage.removeToken();
        router.navigate(['/auth/login']);
      }

      if (error.status === 403) {
        router.navigate(['/products']);
        // Optionally show a toast: "Access denied"
      }

      return throwError(() => error);
    })
  );
};
```

**Updated `app.config.ts`**:

```typescript
import { ApplicationConfig, provideZoneChangeDetection } from '@angular/core';
import { provideRouter } from '@angular/router';
import { provideHttpClient, withFetch, withInterceptors } from '@angular/common/http';
import { routes } from './app.routes';
import { authInterceptor } from './core/interceptors/auth.interceptor';
import { errorInterceptor } from './core/interceptors/error.interceptor';

export const appConfig: ApplicationConfig = {
  providers: [
    provideZoneChangeDetection({ eventCoalescing: true }),
    provideRouter(routes),
    provideHttpClient(
      withFetch(),
      withInterceptors([authInterceptor, errorInterceptor])
    )
  ]
};
```

---

### Solution 4.7 — Route Guards

**`src/app/core/guards/auth.guard.ts`**:

```typescript
import { CanActivateFn, Router } from '@angular/router';
import { inject } from '@angular/core';
import { AuthService } from '../services/auth.service';

export const authGuard: CanActivateFn = () => {
  const authService = inject(AuthService);
  const router = inject(Router);

  if (authService.isLoggedIn()) {
    return true;
  }

  return router.createUrlTree(['/auth/login']);
};
```

**`src/app/core/guards/admin.guard.ts`**:

```typescript
import { CanActivateFn, Router } from '@angular/router';
import { inject } from '@angular/core';
import { AuthService } from '../services/auth.service';

export const adminGuard: CanActivateFn = () => {
  const authService = inject(AuthService);
  const router = inject(Router);

  if (authService.getUserRole() === 'ADMIN') {
    return true;
  }

  return router.createUrlTree(['/products']);
};
```

**Updated `app.routes.ts`**:

```typescript
import { Routes } from '@angular/router';
import { authGuard } from './core/guards/auth.guard';
import { adminGuard } from './core/guards/admin.guard';

export const routes: Routes = [
  { path: '', redirectTo: '/products', pathMatch: 'full' },
  {
    path: 'auth',
    loadChildren: () => import('./features/auth/auth.routes')
      .then(m => m.AUTH_ROUTES)
  },
  {
    path: 'products',
    canActivate: [authGuard],
    loadComponent: () => import('./features/products/product-list/product-list.component')
      .then(m => m.ProductListComponent)
  },
  {
    path: 'products/new',
    canActivate: [authGuard],
    loadComponent: () => import('./features/products/product-form/product-form.component')
      .then(m => m.ProductFormComponent)
  },
  {
    path: 'cart',
    canActivate: [authGuard],
    loadComponent: () => import('./features/cart/cart-page/cart-page.component')
      .then(m => m.CartPageComponent)
  },
  {
    path: 'admin',
    canActivate: [authGuard, adminGuard],
    loadComponent: () => import('./features/admin/dashboard/dashboard.component')
      .then(m => m.DashboardComponent)
  },
  { path: '**', redirectTo: '/products' }
];
```

**`src/app/features/auth/auth.routes.ts`**:

```typescript
import { Routes } from '@angular/router';

export const AUTH_ROUTES: Routes = [
  {
    path: 'login',
    loadComponent: () => import('./login/login.component')
      .then(m => m.LoginComponent)
  },
  {
    path: 'register',
    loadComponent: () => import('./register/register.component')
      .then(m => m.RegisterComponent)
  }
];
```

---

## Solution — Phase 5

### Solution 5.1 — N+1 Fix with @EntityGraph

**Updated `ProductRepository`**:

```java
package com.smartinventory.repository;

import com.smartinventory.model.Product;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.domain.Specification;
import org.springframework.data.jpa.repository.*;
import java.util.Optional;

public interface ProductRepository extends JpaRepository<Product, Long>,
                                           JpaSpecificationExecutor<Product> {

    @EntityGraph(attributePaths = {"category"})
    Page<Product> findAll(Specification<Product> spec, Pageable pageable);

    @EntityGraph(attributePaths = {"category"})
    Page<Product> findAll(Pageable pageable);

    @EntityGraph(attributePaths = {"category"})
    Optional<Product> findById(Long id);
}
```

> **Before `@EntityGraph`:**  
> 1 query for products + N queries for categories = N+1 queries  
> **After `@EntityGraph`:**  
> 1 query with a LEFT JOIN = 1 query total

---

### Solution 5.3 — Docker Deployment

**`smart-inventory-api/Dockerfile`**:

```dockerfile
# Stage 1: Build
FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Run
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

**`smart-inventory-ui/Dockerfile`**:

```dockerfile
# Stage 1: Build
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build -- --configuration=production

# Stage 2: Serve
FROM nginx:alpine
COPY --from=build /app/dist/smart-inventory-ui/browser /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
```

**`smart-inventory-ui/nginx.conf`**:

```nginx
server {
    listen 80;
    server_name localhost;
    root /usr/share/nginx/html;
    index index.html;

    # Angular routing — serve index.html for all non-file routes
    location / {
        try_files $uri $uri/ /index.html;
    }

    # Proxy API requests to the Spring Boot container
    location /api/ {
        proxy_pass http://api:8080/api/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

**`docker-compose.yml`** (at project root):

```yaml
version: '3.8'

services:
  db:
    image: postgres:16-alpine
    container_name: smart-inventory-db
    environment:
      POSTGRES_DB: smart_inventory
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5

  api:
    build: ./smart-inventory-api
    container_name: smart-inventory-api
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/smart_inventory
      SPRING_DATASOURCE_USERNAME: postgres
      SPRING_DATASOURCE_PASSWORD: ${DB_PASSWORD}
      APPLICATION_SECURITY_JWT_SECRET_KEY: ${JWT_SECRET}
    ports:
      - "8080:8080"
    depends_on:
      db:
        condition: service_healthy

  ui:
    build: ./smart-inventory-ui
    container_name: smart-inventory-ui
    ports:
      - "80:80"
    depends_on:
      - api

volumes:
  postgres_data:
```

**`.env`**:

```env
DB_PASSWORD=your_secure_password_here
JWT_SECRET=your_base64_encoded_secret_key_here
```

---

# 🗓️ Weekly Time Distribution (2 hours/day)

| Day | Focus | Activities |
|-----|-------|------------|
| **Day 1** | 📖 Learn + 💻 Backend | 30min: Read docs/watch video on the concept. 90min: Implement the API endpoint and test with Postman |
| **Day 2** | 💻 Backend (continued) | 30min: Read docs. 90min: Complete backend logic, add validation, handle errors |
| **Day 3** | 📖 Learn + 🅰️ Frontend | 30min: Read Angular docs for the concept. 90min: Build the Angular service and component |
| **Day 4** | 🅰️ Frontend (continued) | 30min: Read docs. 90min: Complete UI, connect to backend, test the full flow |
| **Day 5** | 🔍 Refactoring & Debugging | Inspect Network Tab, review SQL logs, clean up code, unify naming |
| **Day 6** | 🧪 Free Architecture Exercise | Add a small feature using ONLY official docs and error messages — no tutorials |

---

# 📚 Essential Resources

| Resource | Link |
|---|---|
| Spring Boot Reference | https://docs.spring.io/spring-boot/reference/ |
| Spring Data JPA | https://docs.spring.io/spring-data/jpa/reference/ |
| Angular Documentation | https://angular.dev |
| RxJS Documentation | https://rxjs.dev |
| JWT Introduction | https://jwt.io/introduction |
| Docker Compose | https://docs.docker.com/compose/ |

---

*End of TP — Smart Inventory & Order Platform*  
*© 2026 Yassine Elkhamlichi*
