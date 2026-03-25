# Workspace

## Overview

pnpm workspace monorepo using TypeScript. Each package manages its own dependencies.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM (api-server) + Firebase Firestore (shop)
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)

## Structure

```text
artifacts-monorepo/
├── artifacts/              # Deployable applications
│   ├── api-server/         # Express API server
│   └── shop/               # ShopEasy - Firebase-powered shopping website
├── lib/                    # Shared libraries
│   ├── api-spec/           # OpenAPI spec + Orval codegen config
│   ├── api-client-react/   # Generated React Query hooks
│   ├── api-zod/            # Generated Zod schemas from OpenAPI
│   └── db/                 # Drizzle ORM schema + DB connection
├── scripts/                # Utility scripts (single workspace package)
│   └── src/                # Individual .ts scripts
├── pnpm-workspace.yaml     # pnpm workspace
├── tsconfig.base.json      # Shared TS options
├── tsconfig.json           # Root TS project references
└── package.json            # Root package with hoisted devDeps
```

## ShopEasy - Shopping Website (`artifacts/shop`)

A full-stack shopping website powered by Firebase Authentication and Firestore.

### Features
- **Customer side**: Signup/login, auto login, product listing, place orders (COD, Hardoi only), track orders, submit reviews after delivery
- **Admin side**: Separate admin login (`admin@shopeasy.com`), product management (add/edit/delete), order management, mark delivered, print order slips

### Firebase Config
- Project ID: `my-shopping-site-f801f`
- Auth Domain: `my-shopping-site-f801f.firebaseapp.com`
- API Key: stored in `GOOGLE_API_KEY` secret, injected as `VITE_FIREBASE_API_KEY`

### Firestore Collections
- `users` — customer profiles (uid, name, email, createdAt)
- `products` — product catalog (name, price, imageUrl, description, createdAt)
- `orders` — customer orders (productId, userId, customerName, address, city, phone, paymentMethod, status, reviewed, createdAt)
- `reviews` — product reviews (orderId, userId, productName, rating, comment, createdAt)

### Admin Setup
1. Go to Firebase Console → Authentication → Add user
2. Email: `admin@shopeasy.com`, set a secure password
3. Admin logs in at `/admin/login`

### Pages
- `/` — Customer product listing (requires login)
- `/login` — Customer signup/login
- `/order/:id` — Place an order
- `/my-orders` — Customer order history + reviews
- `/admin` — Admin dashboard (products + orders tabs)
- `/admin/login` — Admin login
- `/admin/print/:id` — Printable order slip

## Packages

### `artifacts/api-server` (`@workspace/api-server`)
Express 5 API server. Routes live in `src/routes/`.

### `lib/db` (`@workspace/db`)
Database layer using Drizzle ORM with PostgreSQL.

### `lib/api-spec` (`@workspace/api-spec`)
Owns the OpenAPI 3.1 spec and Orval config.

### `lib/api-zod` (`@workspace/api-zod`)
Generated Zod schemas from the OpenAPI spec.

### `lib/api-client-react` (`@workspace/api-client-react`)
Generated React Query hooks and fetch client.

### `scripts` (`@workspace/scripts`)
Utility scripts package.
