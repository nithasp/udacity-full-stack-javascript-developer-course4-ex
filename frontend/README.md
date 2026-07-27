# MyStore — Angular Frontend

E-commerce SPA built with Angular 18. Authenticate, browse products, manage a cart, and complete checkout.

## Prerequisites

- Node.js v18+
- Angular CLI v18

## Setup

```bash
npm install
ng serve          # http://localhost:4200
ng test           # Karma + Jasmine (13 spec files)
ng build          # production build
```

> **Note:** product and cart data are fetched from the backend REST API. The UI loads with `ng serve` alone, but the backend must also be running (see [root README](../README.md)) for the catalog and cart to populate.

## Features

| Area | Description |
|------|-------------|
| **Auth** | Register / Login / Logout with JWT (access + refresh tokens). Interceptor auto-attaches token and handles 401 refresh. |
| **Products** | Product list and detail pages. Products have type variants (color/price/stock) and reviews. |
| **Cart** | Add/remove items, debounced quantity updates, grouped by shop. Synced to backend REST API. |
| **Checkout** | Address dialog (add/edit/select), order confirmation page. |
| **Shared** | Navbar, confirm dialog, loading spinner, reusable form controls, `truncate` pipe, toast notifications (`ngx-toastr`). |

## Project Structure

```
src/
├── app/
│   ├── core/
│   │   ├── config/          # API base URL
│   │   ├── guards/          # Auth route guard
│   │   ├── interceptors/    # JWT attach + token refresh
│   │   ├── models/          # AuthUser, CartApiItem, ConfirmDialogConfig, QuantityUpdate
│   │   └── services/
│   │       ├── auth/        # AuthService, AuthApiService
│   │       ├── cart/        # CartService, CartApiService
│   │       └── ui/          # NotificationService, ConfirmDialogService
│   ├── features/
│   │   ├── auth/            # Login, Register components (lazy-loaded)
│   │   ├── products/        # ProductList, ProductDetail, ProductCard (lazy-loaded)
│   │   └── cart/            # CartPage, OrderConfirmation, AddressDialog (lazy-loaded)
│   └── shared/
│       ├── components/      # Navbar, LoadingSpinner, DialogConfirm, InputField, QuantityInput
│       └── pipes/           # TruncatePipe
├── environments/            # environment.ts / environment.production.ts
└── styles/                  # SCSS partials (variables, mixins, base, auth, animations)
```

## Key Patterns

- **Lazy-loaded feature modules**: `auth`, `products`, `cart`
- **JWT auth flow**: `AuthService` → `AuthInterceptor` → auto-refresh on 401
- **Cart state**: `CartService` fetches/resets on auth state change; debounced quantity updates
- **Form controls**: `InputField` implements `ControlValueAccessor`; `QuantityInput` emits `QuantityUpdate`
- **BEM + SCSS partials** for all component styles
