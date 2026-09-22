# ScrapSetu

ScrapSetu is a marketplace where sellers publish recyclable materials by kilogram and nearby customers request pickup orders.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/scrap-marketplace/src/App.tsx` — storefront, cart, order history, and seller desk UI
- `artifacts/api-server/src/routes/` — scrap inventory, categories, orders, sell requests, and seller dashboard API
- `lib/api-spec/openapi.yaml` — source of truth for the marketplace API contract
- `lib/db/src/schema/` — PostgreSQL tables for categories, inventory, orders, order items, and scrap sell requests

## Architecture decisions

- Listings and customer orders are stored in PostgreSQL so inventory changes survive refreshes and are visible across sessions.
- Customers buy by kilogram; placing an order reserves stock immediately and leaves the request in a pending state for seller confirmation.
- People can submit the scrap they want to sell with contact details and an expected price; admins review and accept or reject those requests.
- Seller tools live behind Clerk sign-in; role-specific owner restrictions and payment collection remain planned follow-ups.
- The React client and server share an OpenAPI contract and generated request/response validation.

## Product

- Browse active scrap listings by category or search
- Add quantities in kilograms to a pickup cart
- Submit customer name, phone, and address to create an order request
- Review previous requests and their current status
- Sign up and sign in with Clerk
- Add, edit, hide, and delete scrap listings from the seller desk
- Create categories and review stock, pending requests, and sales totals
- Review incoming scrap sell requests and accept or reject them

## User preferences

- User asked for a scrap marketplace where they can add scrap options and customers can buy them.

## Gotchas

- Run API codegen after changing `lib/api-spec/openapi.yaml`.
- Restart the managed API and web workflows after backend or frontend changes.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
