# Code Structure Overview

This document explains the main pieces of the B2C Storefront codebase and how the application is structured.

## Overview

The project is a **Next.js 15** application using the `app/` directory. Pages are implemented as Server Components and often fetch data directly from the Medusa backend via server-side functions. Client side interactivity (e.g. forms, Stripe, TalkJS) is added through Client Components.

The code follows an *Atomic Design* approach:

- `src/components/atoms` – small, reusable UI elements (buttons, icons)
- `src/components/molecules` – groups of atoms forming a single component (modals, inputs)
- `src/components/organisms` – more complex sections (header, footer, product cards)
- `src/components/sections` – page level sections which compose the organisms

## Application Routing

Routes are located under `src/app`. All pages are nested within `app/[locale]` to support locale and region handling. The `middleware.ts` file automatically detects a visitor’s country and redirects them to the appropriate locale path (e.g. `/us`, `/gb`). It caches region information in cookies and fetches available regions from the Medusa backend.

```
src/app
└── [locale]
    ├── (main)      – storefront pages such as home, categories and sellers
    └── (checkout)  – checkout specific layout and pages
```

Each page fetches data in server components using helpers from `src/lib/data`. For example, `listProducts` queries the Medusa API via the JS SDK and formats the result for the UI.

## Data Layer

The `src/lib` folder provides utilities and data functions:

- `config.ts` configures the Medusa JS SDK (`@medusajs/js-sdk`)
- `data/*.ts` contains server-side functions that call the backend (cart, products, orders, etc.)
- `helpers/` holds helper utilities such as price formatting and SEO metadata generation
- `client.tsx` initializes the Algolia search client

These files are marked with `"use server"` when they are meant to run on the server.

## Styling and UI

Tailwind CSS is configured in `tailwind.config.ts` and global styles reside in `src/app/globals.css`. Component styles are composed using Tailwind utility classes. Icons live under `src/icons` and are imported by components as needed.

## Third‑party Integrations

- **MedusaJS** – Provides commerce backend APIs used throughout `src/lib/data`
- **Algolia** – Optional product search integration (see `src/lib/client.tsx`)
- **Stripe** – Payment integration handled by components in `src/components/organisms/PaymentContainer`
- **TalkJS** – Customer‑to‑seller chat (see `src/components/organisms/Chat`)
- **Storybook** – Configuration under `.storybook/` for component previews

Environment variables listed in `README.md` control API URLs, keys and other runtime options.

## Development Scripts

Key package scripts:

- `npm run dev` – Start Next.js in development mode
- `npm run build` – Build for production
- `npm run start` – Start the production build
- `npm run lint` – Run ESLint

## Further Reading

The root `README.md` contains quick start instructions. For more information about the Medusa platform visit [https://docs.medusajs.com](https://docs.medusajs.com).

