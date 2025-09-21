# Future Matrix — Admin Dashboard (ROSETTA)

**README.md** — Deep technical & product description for the dashboard template (Bootstrap, core HTML/CSS/JS)

---

## Project overview (one short paragraph)

This repository contains a modern, fully-templated **admin / CRM dashboard** UI built with **HTML, Bootstrap 5, core CSS and JavaScript**. It’s a frontend-first template (static assets) that includes a rich set of pages and components needed by enterprise web apps: dashboards, tables & datatables, forms, auth pages, notifications, charts, user & role management, billing, reports, and more. The layout and asset patterns are designed to be dropped into a Laravel Blade project or used as a standalone static site.

---

## Purpose & audience

* Purpose: provide a production-ready frontend scaffold for SaaS / enterprise CRM / ERP projects so backend teams can plug in APIs quickly.
* Audience: frontend engineers, backend developers integrating UI, product owners, designers who want a ready UI system for document management, CRM, billing and operations.

---

# Dashboard features — deep detail

Below is a section-by-section explanation of the dashboard’s features, UX intent, and how each part should behave when hooked to a real backend.

## 1. Global shell & layout

* **Sidebar (collapsible)** — multi-level navigation with grouped sections, icons, badges and active state. Supports:

  * nested `dropdown` menus (submenus),
  * section headings (e.g., “Application”, “UI Elements”),
  * mobile-friendly collapse + persistent state (remember collapsed/expanded via localStorage).
* **Topbar** — quick search, notifications, user avatar/dropdown, quick links (settings, theme switcher).
* **Main content area** — responsive container; cards & panels layout using Bootstrap grid; supports fluid or fixed width.
* **Utility elements** — floating action button (optional), progress circle, global search overlay.

**Behavioural notes**

* Sidebar should be keyboard accessible (focusable links), support ARIA attributes for expanded state.
* Keep `aria-current="page"` on the active link for screen readers.
* Toggle animations should be CSS-driven for best performance.

## 2. High-level dashboards (KPI & Trends)

* **KPI Cards** — Total Users, Subscriptions, Income, Expense, etc. Each card supports:

  * current value, small delta (▲/▼ with color), short sparkline thumbnail or icon,
  * click to open a detailed drill-down.
* **Time-series charts** — sales, generated content, signups by day/month. Implemented using charting libraries (Chart.js, ApexCharts or similar). Charts must support:

  * time range selector (day / week / month / year),
  * tooltips, series toggles, export to PNG/SVG.
* **Bar / Column charts** — subscribers by day, revenue by plan.
* **Pie / Donut charts** — users by status or region.
* **Map widget** — top countries heatmap (world map), showing regional concentration.

**Data UX**

* Charts should fetch data via an API with `from`/`to` params. Client caches last selection for performance.

## 3. Tables & DataTables (core of CRM)

There are many table templates in the UI; they’re intended to be used for lists (users, orders, invoices, logs).

### Features:

* **Server-side pagination, filtering & sorting** (recommended for large datasets)
* **Client-side search** for small datasets
* **Row selection & bulk actions** (delete, export, change status)
* **Column visibility & column reorder**
* **Inline editing & modal edit patterns**
* **Row-level actions** (view, edit, archive, impersonate)
* **Export** to CSV / Excel / PDF
* **Virtual scrolling** for huge lists

### Example table types:

* **Basic table:** simple lists, small datasets
* **Data table (with DataTables plugin or custom implementation):** search, pagination, length menu, server-side processing
* **Table with avatars & meta:** user list with profile pics, role tags
* **Transaction table:** supporting currency formatting, expandable rows for details

**Backend contract (example)**
`GET /api/users?search=&page=2&per_page=25&sort=created_at:desc` → standard paginated payload `{ data: [], meta: { total, per_page, page } }`.

## 4. Forms & Form Patterns

A comprehensive forms system is included:

### Form types:

* **Basic input forms** (text, email, tel, textarea)
* **Advanced controls**: datepickers, timepickers, select2/multiselect, tags, file upload with progress, rich text editor (Quill/TinyMCE)
* **Validation**: client-side validation using HTML5 + optional JS validation library; patterns for server-side validation mapping
* **Form Wizard**: multi-step forms (useful for onboarding & complex data capture)
* **Form layouts**: horizontal, vertical, inline, grid
* **Dynamic forms**: repeatable field groups (addresses, contacts)
* **File upload & preview**: drag-and-drop area + server chunked upload support

### UX & security:

* Use proper `name` attributes and `autocomplete` hints.
* Protect file uploads (scan, size limits) on the backend.
* Add CSRF tokens for form submissions when integrated with Laravel.

## 5. Authentication & User Flows

Auth pages are included:

* Sign In, Sign Up (with email confirmation flow), Forgot Password, Reset Password, Lock Screen, Two-Factor setup.

### Recommended auth patterns:

* **Session-based (Laravel)**: classic sessions + CSRF.
* **Token-based (SPA)**: JWT with refresh token; secure HttpOnly refresh cookie recommended.
* **Multi-factor auth (MFA)**: TOTP via authenticator apps or SMS as fallback.
* **SSO**: OAuth2 / SAML connector points (enterprise customers).

### Security notes:

* Strong password policy, rate limiting, account lockout, session timeout, device listing.
* Audit log for auth events (logins, password resets, permission changes).

## 6. Roles, Permissions & Access Control (RBAC)

The template includes pages for role & access management. A suggested model:

* **Entities**: `users`, `roles`, `permissions`, `teams`.
* **Role examples**: `super-admin`, `admin`, `manager`, `agent`, `viewer`.
* **Permission granularity**: `users.create`, `users.edit`, `reports.view`, `billing.manage`.
* Use middleware on server to enforce rules. For client-side, show/hide UI actions based on permissions.

## 7. Notifications, Alerts & Activity

* **Real-time toast notifications** (for user actions) and persistent notification center (bell icon).
* **In-app alerts** for scheduled maintenance, system health.
* **Activity feed / audit trail**: user actions, system events, document changes.

## 8. Messaging, Chat & Collaboration

* Chat UI (one-to-one and group), attachments, read receipts.
* Useful for helpdesk or internal agent collaboration.

## 9. Calendar & Scheduling

* Monthly / weekly calendar, event CRUD, drag-and-drop event repositioning, reminders and deep links to bookings.

## 10. Kanban & Project Management

* Boards with columns, card details, task assignments, attachments, due dates — useful for internal workflows.

## 11. Billing & Plans

* Pricing pages, subscription plan management, trial flows, coupon codes, invoices list and invoice PDF preview/download.
* Payment integrations: Stripe / PayPal / local gateways (pluggable).

## 12. AI Tools / Generator Pages

* Example pages for text/code/image/video generation — each with input form, live preview and result listing.

## 13. Reports, Exports & Scheduled Jobs

* CSV/Excel export, scheduled email reports, PDF generation for invoices and reports.
* Background jobs queue (server side) for heavy exports.

## 14. Settings & Tenant Configuration

* Company profile, themes, language, currencies, notification preferences, API keys.
* Multi-tenant considerations: isolate data per tenant, tenant admin pages.

## 15. Error Pages & Maintenance Mode

* Styled 404, 500, 503, forbidden pages with helpful CTAs.

---

# Component inventory (detailed)

* Buttons: primary, secondary, outline, icon buttons, FAB
* Inputs: text, email, password, number, tel, date, time
* Selects: single, multi, searchable
* Switches & radios, checkboxes, badges, chips/tags
* Cards: compact, media card, stat card
* Modals: confirmation, form modal, preview modal
* Tooltips & popovers (Bootstrap)
* Breadcrumbs & pagination
* Avatars & user chips
* Toasts & in-page alerts
* Progress bars & loaders
* Charts: line, bar, donut, sparkline
* Tables: basic, dataTable, virtual list
* File upload with progress
* Date-range picker
* Rich text editor (optional)
* Icons: Iconify + SVG sprite support

---

# Folder structure (recommended)

```
/project-root
├─ index.html
├─ assets/
│  ├─ css/
│  │  ├─ bootstrap.min.css
│  │  ├─ style.css
│  ├─ js/
│  │  ├─ vendor/
│  │  ├─ main.js
│  ├─ images/
│  └─ libs/
├─ pages/            # optional standalone pages or blade partials
├─ components/       # HTML partials for cards, tables, modals
└─ README.md
```

---

# API contract (example endpoints)

These are suggested backend endpoints to integrate the UI:

**Auth**

* `POST /api/auth/login` → `{ token, user }`
* `POST /api/auth/logout`
* `POST /api/auth/register`
* `POST /api/auth/forgot-password`
* `POST /api/auth/reset-password`

**Users**

* `GET /api/users` (filter, sort, paginate)
* `GET /api/users/{id}`
* `POST /api/users`
* `PUT /api/users/{id}`
* `DELETE /api/users/{id}`

**Dashboard / Metrics**

* `GET /api/metrics/overview?range=30d`
* `GET /api/metrics/sales?from=&to=`

**Invoices & Billing**

* `GET /api/invoices`
* `POST /api/invoices/{id}/send`
* `GET /api/plans`

**Files**

* `POST /api/uploads` (supports chunked upload)
* `GET /api/uploads/{id}`

**Notifications**

* `GET /api/notifications`
* `POST /api/notifications/mark-read`

Design responses to follow JSON API or a consistent schema (data/meta/links).

---

# Theming & customization

* CSS variables for primary/secondary colors, border radius, base fonts.
* Theme files (light/dark) that swap root variables.
* Use utility-first pattern for spacing/tokens in `:root` CSS variables.
* For brand updates, centralize fonts & colors in `variables.css`.

---

# Accessibility & internationalization

* Provide `lang` attribute on `<html>`.
* Ensure keyboard focus states visible.
* Add `aria` attributes for dynamic components (sidebar, modals, accordions).
* Use semantic elements — `<main>`, `<nav>`, `<header>`, `<footer>`.
* Support i18n by storing strings in JSON files and switch via `data-i18n` attributes or via server templating.

---

# Security & best practices

* Sanitize & escape all user inputs server-side.
* Content Security Policy (CSP) headers for script/style sources.
* Store sensitive tokens on server or use secure cookies (HttpOnly, Secure).
* Validate file upload types & size; scan for malware.
* Enforce rate-limiting and login throttling.

---

# Performance & optimization

* Defer non-critical JS. Use `defer` or load at end of body.
* Use tree-shaken builds for JS libraries.
* Compress images (WebP), use `loading="lazy"`.
* Bundle & minify CSS/JS for production.
* Use CDN for vendor libs if possible.
* HTTP caching headers for static assets.

---

# Testing & QA

* Unit test critical JS functions (if you have a build & test setup).
* Visual regression tests (Percy / Storybook snapshots).
* Cross-browser checks (Chrome, Firefox, Edge, Safari).
* Accessibility checks (axe-core).
* Load testing for API endpoints feeding large tables/exports.

---

# Integration notes (Laravel)

* Convert pages to Blade templates (`resources/views/layouts/app.blade.php`).
* Use `{{ asset('assets/...') }}` for static assets.
* Protect forms with `@csrf` and use `@can` for permission UI gating.
* Create API controllers for endpoints above and use Laravel policies for RBAC.

---

# Developer workflow & build

* Keep a `dev` branch for UI changes and `main` for production build.
* Use npm for dev tools (optional): bundling, minification, linting.
* Example npm tasks:

  * `npm run dev` — start watcher, compile SCSS/JS
  * `npm run build` — production build & minify

---

# Recommended next steps (priority list)

1. **Standardize design tokens** (colors, fonts) into a single CSS variables file.
2. **Connect tables to real APIs** with server-side pagination.
3. **Implement RBAC** and hide UI actions based on permissions.
4. **Add accessibility fixes** (aria attributes, labels).
5. **Add automated tests** for regression and accessibility.
6. **Add analytics & monitoring** for user behavior and errors.

---

# License & credits

* Choose a license that fits your use (MIT for templates is common).
* Credit third-party libs you use (Bootstrap, Iconify, Chart library, DataTables, etc).

---

## Quick copyable summary (for README top)

> Modern admin dashboard template for enterprise CRMs — built with HTML, Bootstrap 5, core CSS & JS. Includes dashboards, charts, datatables, forms, auth, role-based access pages, billing, reports and many reusable UI components — designed to be integrated into Laravel or used as a static frontend scaffold.

---
