# 🛒 TP — Smart Inventory & Order Platform

> Full-Stack **Angular 18+** & **Spring Boot 3.x** — Project-Driven Learning

[![Angular](https://img.shields.io/badge/Angular-18+-DD0031?logo=angular&logoColor=white)](https://angular.dev)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-6DB33F?logo=springboot&logoColor=white)](https://spring.io/projects/spring-boot)
[![Java](https://img.shields.io/badge/Java-21+-007396?logo=openjdk&logoColor=white)](https://www.oracle.com/java/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16+-4169E1?logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](#license)

---

**Institution:** Harvard University — School of Engineering and Applied Sciences  
**Course:** CS-E4200 — Modern Full-Stack Web Engineering  
**Professor:** Dr. Y. Elkhamlichi  
**Semester:** Fall 2026  

---

## 📖 About This TP

This repository is a **hands-on practical work (TP)** designed to take a student from *"I know the syntax"* to *"I can build and reason about a real full-stack application."*

It follows the **70/30 Golden Rule** of Project-Driven Learning:

- **30%** — reading documentation & watching focused concept videos
- **70%** — actual hands-on coding inside a single connected repository

You build **one real product** — a **Smart Inventory & Order Platform** — and every concept (forms, security, pagination, signals, JWT…) is learned *because the project needs it*, not in isolation.

---

## 🎯 What You Will Learn

### 🔧 Spring Boot 3.x (Backend)

| Concept | Description |
|---|---|
| **Layered Architecture** | Controller → Service → Repository — strict separation of concerns |
| **JPA / Hibernate** | Entity mapping, relationships, `@EntityGraph`, DTO Projections |
| **REST API Design** | Versioned endpoints, proper HTTP verbs, status codes |
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
| **Clean Project Structure** | Industry-standard folder naming (`core/`, `features/`, `shared/`) |
| **Full-Stack Integration** | Frontend ↔ Backend communication with proper CORS |
| **Docker Compose** | Multi-container deployment (PostgreSQL + API + Nginx) |
| **Debugging Skills** | Network Tab, SQL logs, error tracing |

---

## 🧱 Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Angular 18+ (Standalone Components, Signals, Reactive Forms) |
| Backend | Spring Boot 3.x, Spring Data JPA, Spring Security |
| Database | PostgreSQL 16+ |
| Auth | JWT (Stateless) with jjwt |
| DevOps | Docker & Docker Compose (multi-stage builds, Nginx) |

---

## 🛒 Project Overview

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

## 🗂️ Architecture

This project uses **Layered Architecture** (N-Tier), organized with a **"Package by Layer"** structure.

### Backend — Spring Boot

```
smart-inventory-api/
├── src/main/java/com/smartinventory/
│   ├── SmartInventoryApplication.java
│   ├── config/            # CORS, Security config
│   ├── controller/        # REST endpoints (Auth, Product, Category, Cart, Admin)
│   ├── dto/
│   │   ├── request/       # LoginRequest, RegisterRequest, ProductRequest, CartItemRequest
│   │   └── response/      # AuthResponse, ProductResponse, PageResponse, ErrorResponse
│   ├── exception/         # GlobalExceptionHandler, ResourceNotFoundException, BusinessException
│   ├── model/             # JPA Entities (Product, Category, CartItem, Order, User)
│   ├── repository/        # JPA Repositories + JpaSpecificationExecutor
│   ├── security/          # JwtService, JwtAuthenticationFilter, UserDetailsServiceImpl
│   └── service/           # Service interfaces + impl/
├── src/main/resources/
│   └── application.yml
└── pom.xml
```

### Frontend — Angular

```
smart-inventory-ui/
├── src/app/
│   ├── core/
│   │   ├── interceptors/   # auth.interceptor.ts, error.interceptor.ts
│   │   ├── guards/         # auth.guard.ts, admin.guard.ts
│   │   └── services/       # auth.service.ts, storage.service.ts
│   ├── features/
│   │   ├── auth/           # login/, register/, models/, auth.routes.ts
│   │   ├── products/       # product-list/, product-form/, models/, services/
│   │   ├── cart/           # cart-page/, models/, services/
│   │   └── admin/          # dashboard/, admin.routes.ts
│   ├── shared/
│   │   ├── components/     # navbar/, pagination/
│   │   └── pipes/          # currency-format.pipe.ts
│   ├── app.component.ts
│   ├── app.config.ts
│   └── app.routes.ts
├── src/environments/       # environment.ts, environment.development.ts
├── angular.json
└── package.json
```

> **Why this structure?**  
> `core/` → Singleton services, guards, interceptors (instantiated ONCE).  
> `features/` → Domain modules, each self-contained with its own models, services, and components.  
> `shared/` → Reusable components, pipes, and directives used across multiple features.

---

## 🧩 The 5-Phase Learning Path

### Phase 1 — Data Contract & Basic Rendering *(4 days, 2h/day)*

| # | Exercise | Backend | Frontend |
|---|---|---|---|
| 1.1 | Spring Boot Init | Spring Initializr, `application.yml`, PostgreSQL | — |
| 1.2 | Data Model | `Product` & `Category` entities, JPA mapping | — |
| 1.3 | Repository Layer | `JpaRepository`, derived query methods | — |
| 1.4 | DTO Layer | Java `record` DTOs (request/response) | — |
| 1.5 | Service Layer | Interface + Impl, constructor injection, entity↔DTO mapping | — |
| 1.6 | REST Controller | `GET`, `POST` endpoints, `ResponseEntity`, HTTP 201 | — |
| 1.7 | CORS Config | `WebMvcConfigurer`, cross-origin setup | — |
| 1.8 | Angular Init | — | `ng new --standalone`, `provideHttpClient(withFetch())` |
| 1.9 | TypeScript Models | — | `interface Product`, `interface ProductRequest` |
| 1.10 | Product Service | — | `HttpClient`, `Observable<T>`, strongly-typed API calls |
| 1.11 | Product List | — | `@for`, `@if`, `@empty`, `OnInit`, `subscribe()` |
| 1.12 | Navigation & Layout | — | `NavbarComponent`, `routerLink`, `router-outlet` |

---

### Phase 2 — Reactive Forms & Error Handling *(4 days, 2h/day)*

| # | Exercise | Backend | Frontend |
|---|---|---|---|
| 2.1 | Bean Validation | `@NotBlank`, `@Min`, `@Valid` on `@RequestBody` | — |
| 2.2 | Global Error Handler | `@RestControllerAdvice`, structured `ErrorResponse` | — |
| 2.3 | Product Form | — | `ReactiveFormsModule`, `FormGroup`, `Validators` |
| 2.4 | Server-Side Errors | — | Display field-level errors from 400 responses |
| 2.5 | Custom Validator | — | Price decimal validator, `ValidationErrors` |

---

### Phase 3 — Signals & RxJS Pipeline *(4 days, 2h/day)*

| # | Exercise | Backend | Frontend |
|---|---|---|---|
| 3.1 | Pagination & Sorting | `Pageable`, `Page<T>`, `PageResponse<T>` DTO | — |
| 3.2 | Dynamic Search | `JpaSpecificationExecutor`, combinable `Specification<T>` | — |
| 3.3 | Signals | — | `signal()`, `computed()`, `effect()`, `.set()`, `.update()` |
| 3.4 | Live Search | — | RxJS: `debounceTime` → `distinctUntilChanged` → `switchMap` |
| 3.5 | Pagination Component | — | Reusable `PaginationComponent` in `shared/` |

---

### Phase 4 — JWT Security & Interceptors *(5 days, 2h/day)*

| # | Exercise | Backend | Frontend |
|---|---|---|---|
| 4.1 | User Entity & Auth DTOs | `User` entity, `Role` enum, `LoginRequest`, `AuthResponse` | — |
| 4.2 | JWT Service | `jjwt` library, token generation/validation, HMAC signing | — |
| 4.3 | Security Config | `SecurityFilterChain`, `JwtAuthenticationFilter`, `STATELESS` | — |
| 4.4 | Auth Controller | `POST /auth/register`, `POST /auth/login`, `BCrypt` hashing | — |
| 4.5 | Auth Service | — | `StorageService`, JWT decode, `localStorage` token management |
| 4.6 | HTTP Interceptor | — | `HttpInterceptorFn`, auto `Bearer` token, error handler |
| 4.7 | Route Guards | — | `CanActivateFn`, `authGuard`, `adminGuard` |
| 4.8 | Login & Register Pages | — | Auth forms, cross-field validator, lazy-loaded routes |

---

### Phase 5 — Production Readiness *(3 days, 2h/day)*

| # | Exercise | Backend | Frontend |
|---|---|---|---|
| 5.1 | N+1 Problem | `@EntityGraph`, DTO Projections, JPQL, SQL log analysis | — |
| 5.2 | Deferred Loading | — | `@defer (on viewport)`, skeleton loaders, lazy triggers |
| 5.3 | Docker Compose | Multi-stage Dockerfile (Maven → JRE) | Multi-stage Dockerfile (Node → Nginx), `nginx.conf` proxy |
| 5.4 | Admin Panel | `@PreAuthorize("hasRole('ADMIN')")`, CRUD endpoints | Editable table, inline editing, confirmation dialogs |

---

## 📑 Repository Contents

| File | Description |
|---|---|
| [`TP_Angular_SpringBoot.md`](./TP_Angular_SpringBoot.md) | 📝 Full TP document — exercises, solutions, and explanations |
| [`TP_Angular_SpringBoot.tex`](./TP_Angular_SpringBoot.tex) | 📄 LaTeX source for the PDF version |

---

## 🛠️ Prerequisites

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

## 🚀 Getting Started

### Run Backend

```bash
cd smart-inventory-api
./mvnw spring-boot:run
```

### Run Frontend

```bash
cd smart-inventory-ui
npm install
ng serve
```

### Or run everything with Docker (Phase 5)

```bash
docker-compose up --build
```

App will be available at `http://localhost` (Nginx → Angular + API proxy).

---

## 📅 Suggested Weekly Schedule (2h/day)

| Days | Focus |
|---|---|
| Day 1–4 | 30 min theory + 90 min hands-on coding (2 days backend, 2 days frontend, per feature) |
| Day 5 | Refactoring & debugging — Network tab, SQL logs, DTO/interface cleanup |
| Day 6 | Free architectural exercise — add a small feature using only official docs, no tutorials |

---

## 🤝 Contributing

This TP is a learning resource — feel free to fork it, adapt it, or submit improvements via pull request.

## 📜 License

This project is licensed under the MIT License — see the [LICENSE](./LICENSE) file for details.

---

<p align="center">Built for students who want to actually <b>understand</b> Angular & Spring Boot — not just copy-paste.</p>
