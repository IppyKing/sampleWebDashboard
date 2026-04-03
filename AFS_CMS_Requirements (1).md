# Access Financial Services Ltd. — Content Management System
## Product Requirements Document (PRD)

**Project:** AFS Asset & Content Management System  
**Version:** 1.0  
**Company:** Access Financial Services Ltd.  
**Date:** March 31, 2026  
**Status:** Draft

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Brand & Design System](#2-brand--design-system)
3. [Layout & Navigation Architecture](#3-layout--navigation-architecture)
4. [Authentication](#4-authentication)
5. [Core CMS Modules](#5-core-cms-modules)
6. [UI Components Library](#6-ui-components-library)
7. [Dark Mode](#7-dark-mode)
8. [Responsive Design Breakpoints](#8-responsive-design-breakpoints)
9. [Data Management & State](#9-data-management--state)
10. [Extensibility & Future Modules](#10-extensibility--future-modules)
11. [Accessibility & Performance](#11-accessibility--performance)
12. [File & Asset References](#12-file--asset-references)

---

## 1. Project Overview

Access Financial Services Ltd. requires a modern, responsive, browser-based **Content Management System (CMS)** to manage internal IT equipment and other organisational assets. The system is built as a single-page application (SPA), currently using vanilla HTML, CSS, and JavaScript with no external framework dependencies beyond Google Fonts.

### 1.1 Goals

- Provide a centralised platform for tracking, assigning, and maintaining IT equipment.
- Deliver a polished, brand-aligned interface that reflects AFS's professional identity.
- Design the system to be **modular and extensible**, allowing new content types (e.g. furniture, vehicles, software licences) to be added in future phases.
- Support all screen sizes, from desktop workstations to mobile devices used in the field.

### 1.2 Tech Stack (Current)

| Layer | Technology |
|---|---|
| Markup | HTML5 |
| Styling | Vanilla CSS with CSS Custom Properties (`:root` variables) |
| Logic | Vanilla JavaScript (ES6+) |
| Fonts | Google Fonts — `Outfit` (display), `DM Sans` (body) |
| Icons | Inline SVG |
| Build | None — single `index.html` file |

> **Note:** Future phases may migrate to a React or Vue front-end with a REST/GraphQL back-end. All CSS variables and component conventions in this document are written to ease that migration.

---

## 2. Brand & Design System

### 2.1 Colour Palette

All colours are defined as CSS Custom Properties on `:root`. Do not hard-code hex values outside of this token sheet.

#### Primary Colours

| Token | Hex | Usage |
|---|---|---|
| `--navy-900` | `#0a1628` | App background, login screen, sidebar, darkest surface |
| `--navy-800` | `#0f1f3d` | Body text, headings |
| `--navy-700` | `#152952` | Hover surfaces, secondary buttons |
| `--navy-600` | `#1b3468` | Avatar background, subtle fills |
| `--navy-500` | `#213e7d` | Primary accent, badge fills |
| `--navy-400` | `#3b5998` | Icons, item icon colour |
| `--navy-300` | `#5a7ab5` | Muted accent |
| `--yellow-500` | `#f5c518` | **Primary action colour** — buttons, active nav, logo, toggles |
| `--yellow-400` | `#ffd747` | Gradient end, hover states |
| `--yellow-300` | `#ffe066` | Softer accent |
| `--yellow-200` | `#fff0a3` | Badge backgrounds, stat icon fills |

#### Semantic / Utility Colours

| Token | Hex | Usage |
|---|---|---|
| `--danger` | `#e74c5e` | Errors, delete actions, overdue badges, alerts (optional red) |
| `--success` | `#27ae60` | Active/completed statuses, positive changes |
| `--info` | `#5dade2` | Informational badges, available status |
| `--warning` | `#f39c12` | Maintenance status, medium priority |

#### Neutral / Surface Colours

| Token | Hex | Usage |
|---|---|---|
| `--white` | `#ffffff` | Primary background (light mode), cards, modals |
| `--off-white` | `#f4f6fb` | Page background (light mode), input fills |
| `--gray-50` | `#f8f9fc` | Row hover |
| `--gray-100` | `#e8ecf4` | Borders, dividers, input borders |
| `--gray-200` | `#d0d7e5` | Scrollbar thumb |
| `--gray-300` | `#9aa5b9` | Placeholder text, muted icons |
| `--gray-400` | `#6b7a94` | Subtext, labels, ghost button text |

### 2.2 Typography

| Role | Family | Weights | Usage |
|---|---|---|---|
| Display | `Outfit` | 400, 500, 600, 700, 800 | Headings, page titles, brand name, buttons |
| Body | `DM Sans` | 300, 400, 500, 600, 700 | All body copy, table data, labels, inputs |

- Base font size: `14.5px` on `html`
- Body line-height: `1.5` (inherited default)
- Letter-spacing on headings: `−0.3px` to `−0.5px`
- All uppercase utility labels: letter-spacing `1.5px`, size `0.62rem`

### 2.3 Spacing & Shape

| Token | Value | Usage |
|---|---|---|
| `--radius` | `12px` | Cards, modals, large containers |
| `--radius-sm` | `8px` | Buttons, inputs, nav items, small chips |
| `--sidebar-w` | `256px` | Fixed sidebar width |
| `--header-h` | `62px` | Top header / sidebar brand bar height |

### 2.4 Shadows

| Token | Value |
|---|---|
| `--shadow-sm` | `0 1px 3px rgba(10,22,40,.07)` |
| `--shadow-md` | `0 4px 16px rgba(10,22,40,.1)` |
| `--shadow-lg` | `0 8px 32px rgba(10,22,40,.14)` |

### 2.5 Transitions

All interactive elements use `--transition: .25s cubic-bezier(.4,0,.2,1)` for smooth, consistent motion.

### 2.6 Logo & Branding

- The AFS logo renders as an abbreviated mark (`AFS`) inside a yellow square with rounded corners (`border-radius: 12px` for login, `8px` for sidebar).
- Logo mark background: `--yellow-500`; text colour: `--navy-900`; font: `Outfit`, weight `800`.
- Full product name: **"Asset Management"** (display) or module-specific titles in context.
- Version tag (e.g. `v2.4`) appears as a small pill badge beside the brand name in the sidebar.

---

## 3. Layout & Navigation Architecture

### 3.1 Overall App Shell

```
┌─────────────────────────────────────────┐
│              LOGIN SCREEN               │
│  (full-viewport overlay, z-index 9999)  │
└─────────────────────────────────────────┘
              ↓ on successful login
┌───────────────────────────────────────────────────────┐
│  .app  (display: flex; min-height: 100vh)             │
│ ┌────────────────┐  ┌─────────────────────────────┐   │
│ │   .sidebar     │  │         .main               │   │
│ │  (fixed left)  │  │  ┌──────────────────────┐   │   │
│ │                │  │  │      .header         │   │   │
│ │  Nav Items     │  │  │  (sticky, z-50)      │   │   │
│ │  Sections      │  │  └──────────────────────┘   │   │
│ │  User Footer   │  │  ┌──────────────────────┐   │   │
│ └────────────────┘  │  │   .content / pages   │   │   │
│                     │  └──────────────────────┘   │   │
│                     └─────────────────────────────┘   │
└───────────────────────────────────────────────────────┘
```

### 3.2 Navigation Bar — Dual Orientation

The navigation supports two orientations. The user can toggle between them via a control in the sidebar footer or settings page.

#### Vertical Mode (Default — Sidebar)

- **Position:** Fixed, left edge, full viewport height.
- **Width:** `256px` (collapses to icon-only rail of `64px` when minimised).
- **Background:** `--navy-900`
- **Structure:**
  - Brand bar (logo + app name + version) — `62px` tall.
  - Grouped nav sections with uppercase section titles.
  - Nav items with icon + label + optional badge.
  - Footer with user avatar, name, role, and sign-out button.
- **Collapse behaviour:** On collapse, labels and section titles hide; icons remain centred. Tooltip appears on hover showing item label.
- **Mobile behaviour (`< 768px`):** Sidebar slides off-screen (`translateX(-100%)`). A hamburger button in the header triggers `sidebar.classList.toggle('open')` to slide it back in.

#### Horizontal Mode (Top Bar)

- **Position:** Fixed, top edge, full viewport width.
- **Height:** `62px`
- **Background:** `--navy-900` (or optionally `--white` with navy text for a lighter variant)
- **Structure:**
  - Left: Logo + app name.
  - Centre: Horizontal nav items (icon + label, active state with yellow underline or pill).
  - Right: Search bar, notification button, help button, dark mode toggle, user avatar.
- **Overflow:** On narrower viewports, nav items collapse into a "More" dropdown or a hamburger drawer.
- **Main content:** Shifts down by `62px` (no left margin in this mode).

#### Toggle Mechanism

- A layout toggle button (two icons: sidebar / top-bar) lives in:
  - The Settings page under "Display Preferences".
  - The sidebar footer (icon-only, tooltip).
- State persists in `localStorage` under the key `afs_nav_orientation` (`"vertical"` | `"horizontal"`).
- On load, the stored preference is read and the correct class (`nav-vertical` | `nav-horizontal`) is applied to `<body>`.

### 3.3 Navigation Items & Routing

Navigation is client-side only. Clicking a nav item sets the active page by toggling `.active` class on `.page` elements and updating the URL hash.

| Section | Nav Item | Icon Theme | Badge |
|---|---|---|---|
| Overview | Dashboard | Grid / squares | — |
| Assets | Equipment | Laptop/monitor | Count (dynamic) |
| Assets | Assignments | User-check | — |
| Assets | Maintenance | Wrench | Pending count |
| Assets | Categories | Four-square grid | — |
| Administration | Users | Group | — |
| Administration | Reports | Bar chart | — |
| Administration | Settings | Gear / cog | — |

**Active state:** Yellow gradient background (`linear-gradient(135deg, --yellow-500, --yellow-400)`), navy text, navy icon. All other items: 55% white text, 65% white icon.

### 3.4 Header Bar

The sticky top header (`height: 62px`, `backdrop-filter: blur(12px)`) contains:

- **Hamburger button** — visible only on mobile (`< 768px`).
- **Page title** — visible only on mobile.
- **Global search bar** — `max-width: 440px`, left-aligned. Live-filters visible table rows on `input` event.
- **Header actions (right-aligned):**
  - Notifications button (with red dot indicator for unread alerts).
  - Help button.
  - Dark mode toggle (moon/sun icon). *(See Section 7.)*
  - *(Horizontal nav mode only)* User avatar with dropdown.

---

## 4. Authentication

### 4.1 Login Screen

The login screen is a full-viewport overlay (`position: fixed; inset: 0; z-index: 9999`) using a two-column layout:

- **Left panel** (`flex: 1`): Dark navy background (`--navy-900`) with radial gradient decorations. Displays the AFS logo, tagline, and four feature callout items (icon + description). Hidden on screens `< 860px`.
- **Right panel** (`width: 460px`): White card (`border-radius: 24px 0 0 24px`) containing the sign-in form.

### 4.2 Login Form Fields

| Field | Type | Validation |
|---|---|---|
| Email address | `email` | Required, valid email format |
| Password | `password` | Required, min 6 characters |
| Remember me | `checkbox` | Optional |
| Forgot password | Link | Navigates to reset flow (future) |

### 4.3 Credential Logic (Demo / Current)

- Demo credentials: `admin@company.com` / `admin123`
- On success: login screen fades out (opacity + visibility transition), `.app` gains class `.show` (`display: flex`).
- On failure: inline error message shown below submit button.
- Logout: clears session, re-shows login screen.

### 4.4 Future Authentication (Phase 2)

- JWT-based authentication against an AFS identity provider.
- Role-based access control (RBAC) enforced server-side.
- Session expiry with silent token refresh.
- SSO integration (Active Directory / OAuth2).

---

## 5. Core CMS Modules

### 5.1 Dashboard

The landing page after login, rendered in `#contentArea`.

**Components on dashboard:**

- **Stats Grid** (`grid-template-columns: repeat(4, 1fr)`) — four KPI cards showing:
  - Total Equipment count
  - Assigned Equipment count
  - Active Maintenance tickets
  - Total Users
  - Each card has a colour-coded 3px top border, an icon, a large numeric value, a label, and a delta/change indicator (up/down with colour).

- **Main Grid** (`grid-template-columns: 1fr 340px`) — two columns:
  - Left: Recent Equipment table (asset tag, name, type icon, status badge, assigned-to, location).
  - Right column (stacked cards):
    - **Asset Distribution** — progress bar breakdown by category (Laptops, Monitors, Servers, etc.).
    - **Recent Activity** — chronological feed of system events with colour-coded dot, description, and timestamp.

### 5.2 Equipment

Full inventory of all IT assets.

**Page Header:**
- Title: "Equipment" + subtext count.
- Actions: Filter tabs (All / Active / Assigned / Maintenance / Retired), "+ Add Equipment" primary button.

**Table Columns:** Asset Tag | Name (with icon) | Type | Status Badge | Assigned To | Location | Purchase Date | Cost | Actions (View / Edit / Delete).

**Add Equipment Modal Fields:**

| Field | Type | Notes |
|---|---|---|
| Name | Text | Required |
| Type | Select | Laptop, Monitor, Server, Phone, Printer, Network |
| Serial Number | Text | Optional |
| Location | Text | Defaults to "IT Storage" |
| Cost | Number | Optional |
| Status | Select | Active, Available, Maintenance, Retired |

Asset tag auto-generated: `AP-{PREFIX}-{###}` (e.g. `AP-LP-001` for laptops).

### 5.3 Assignments

Track which staff member has each piece of equipment.

**Table Columns:** Equipment (name + asset tag) | Assigned To (avatar + name) | Department | Date Assigned | Due Date | Status Badge | Notes | Actions.

**Add Assignment Modal Fields:** Equipment (select from available/active), Assign To (select from users), Department (text), Notes (textarea).

**Status Badges:** Active · Overdue · Completed

On save, the equipment's status updates to `"assigned"`.

### 5.4 Maintenance

Ticketing system for equipment repairs and scheduled maintenance.

**Table Columns:** Ticket ID | Equipment | Issue Description | Priority Badge | Status Badge | Created Date | Assigned Technician | Actions.

**Priority Badges:** Low (info blue) · Medium (warning orange) · High (danger red)

**Status Badges:** Pending · In Progress · Completed

**Add Maintenance Modal Fields:** Equipment (select all), Issue Description (textarea, required), Priority (select), Assign Technician (select from admin/tech-role users).

On save, the equipment's status updates to `"maintenance"`.

### 5.5 Categories

Manage asset type categories for organising equipment.

**Display:** Card grid — each card shows category icon, name, item count, and a coloured accent strip.

**Add Category Modal Fields:** Category Name (text, required), Icon Key (select from predefined SVG icon set).

Colours are cycled from a predefined palette array on each new category.

### 5.6 Users

Internal user directory and access management.

**Table Columns:** Avatar + Name | Email | Department | Role Badge | Equipment Count | Actions.

**Role Badges:** Administrator (yellow) · Technician (navy) · Viewer (gray)

**Add User Modal Fields:** Full Name (required), Email, Department, Role (Viewer / Technician / Administrator).

User initials auto-generated from name for avatar display.

### 5.7 Reports

Analytics and exportable data views.

**Planned Report Types:**
- Equipment by Status (pie / donut chart).
- Equipment by Category (bar chart).
- Assignment History (date-filtered table).
- Maintenance Cost Summary (line chart).
- User Activity Log.

> Charts will be implemented using a lightweight charting library (e.g. Chart.js or a pure SVG/Canvas implementation).

### 5.8 Settings

System and user preference configuration.

**Setting Groups:**

| Group | Settings |
|---|---|
| Display Preferences | Dark Mode toggle, Navigation Orientation (Vertical / Horizontal), Compact Mode |
| Notifications | Email alerts on assignment, Email on maintenance overdue, In-app toast notifications |
| Data Management | Export all data (JSON/CSV), Import data, Reset to demo data |
| Account | Change password, Update profile, Active session info |
| System | App version, About AFS CMS |

---

## 6. UI Components Library

All components use the shared CSS variable tokens. Below is a specification of each reusable component.

### 6.1 Buttons

| Variant | Class | Background | Text | Use Case |
|---|---|---|---|---|
| Primary | `.btn-primary` | Yellow gradient | Navy | Main CTAs (Add, Save, Assign) |
| Secondary | `.btn-secondary` | `--navy-700` | White | Secondary actions |
| Ghost | `.btn-ghost` | Transparent | Gray-400 | Cancel, minor actions |
| Danger | `.btn-danger` | Red 10% | Red | Delete, remove |
| Small | `.btn-sm` | — | — | Modifier; reduces padding |

All buttons: `display: inline-flex`, `gap: 7px`, `border-radius: 10px`, `font-weight: 600`, transition on hover (`translateY(-1px)` + deeper shadow for primary).

### 6.2 Badges / Status Pills

| Class | Colour | Status |
|---|---|---|
| `.badge-active` | Green | Active |
| `.badge-assigned` | Navy | Assigned |
| `.badge-available` | Info blue | Available |
| `.badge-maintenance` | Warning orange | Maintenance |
| `.badge-retired` | Gray | Retired |
| `.badge-pending` | Yellow/gold | Pending |
| `.badge-completed` | Green | Completed |
| `.badge-overdue` | Red | Overdue |
| `.badge-admin` | Yellow-200 / Navy text | Role: Admin |
| `.badge-tech` | Navy 10% | Role: Technician |
| `.badge-viewer` | Gray 10% | Role: Viewer |

Shape: `border-radius: 6px`, padding `3px 9px`, `font-size: .7rem`, `font-weight: 600`.

### 6.3 Cards

Standard card: white background, `--radius` corners, `--shadow-sm`, `1px` border in `--gray-100`.

- `.card-header` — title + optional action, `16px 20px` padding, bottom border.
- `.card-body` — `20px` padding. Modifier `.no-pad` removes padding for flush content.

### 6.4 Tables

- `thead th` — uppercase, `0.7rem`, letter-spacing `0.8px`, gray-300 colour, bottom border.
- `tbody tr` — hover state `--gray-50` background.
- `tbody td` — `12px 16px` padding, `0.84rem`, bottom border on all but last row.
- `.table-wrap` — horizontal scroll container for overflow on small screens.

### 6.5 Forms

- `.form-group` — label + input wrapper, `margin-bottom: 16px`.
- Labels — `0.76rem`, weight `600`, navy-800.
- Inputs / Selects / Textareas — full width, `10px 14px` padding, `1.5px` border `--gray-100`, `border-radius: 10px`.
- Focus state — border turns `--yellow-500`, `box-shadow: 0 0 0 3px rgba(245,197,24,.1)`.
- `.form-row` — two-column grid for side-by-side fields.

### 6.6 Modal

- Overlay: `position: fixed; inset: 0`, semi-transparent navy backdrop with `backdrop-filter: blur(4px)`.
- Modal container: `width: 560px`, `max-width: 94vw`, `border-radius: 16px`, white background, `--shadow-lg`.
- Entry animation: scales from `0.97` + `translateY(20px)` to natural on `.show`.
- Structure: `.modal-header` (title + close X) / `.modal-body` (form fields) / `.modal-footer` (action buttons).

### 6.7 Toast Notifications

- Position: `fixed; bottom: 24px; right: 24px`.
- Style: navy-900 background, white text, yellow left border (`3px`), `border-radius: 10px`.
- Animation: slides in from right (`translateX(120%)` → `translateX(0)`), auto-dismisses after 3 seconds.
- Stacked vertically with `gap: 8px` for multiple simultaneous toasts.

### 6.8 Toggles (On/Off Switches)

- Width `42px`, height `24px`, `border-radius: 12px`.
- Off: `--gray-200` background. On: `--yellow-500` background.
- Thumb slides via `translateX(18px)` CSS transition.

### 6.9 Tabs

- Container: `background: --off-white`, `border-radius: --radius-sm`, `padding: 3px`.
- Active tab: white background, navy text, `--shadow-sm`.
- Inactive: gray text, navy on hover.

### 6.10 Activity Feed

- Each item: flex row with colour-coded dot, description text, and relative timestamp.
- Dot colours: yellow (add/create), navy (update), green (complete), red (alert/remove).

### 6.11 Progress Bars

- Row: label (min-width `90px`) + bar track + percentage value.
- Track: `--gray-100` background, `6px` height, fully rounded.
- Fill: colour varies by category; set via inline `style` attribute.

### 6.12 Stat Cards

- 4-column responsive grid.
- Each card: white, `--radius`, `--shadow-sm`, `20px` padding, coloured `3px` top border (child selectors).
- Contains: icon square, large numeric value (`1.5rem`, weight `700`), label, delta chip (up = green, down = red).

---

## 7. Dark Mode

Dark mode is a first-class feature, toggled via the header action bar (sun/moon icon) and the Settings page.

### 7.1 Toggle Mechanism

- Button in header actions — icon switches between `☀` (light mode) and `🌙` (dark mode).
- Also available as a toggle row in Settings → Display Preferences.
- State stored in `localStorage` under `afs_theme` (`"light"` | `"dark"`).
- On page load: read stored preference and apply immediately (before first paint) to avoid flash.
- Implementation: `document.body.classList.toggle('dark-mode')` — all CSS changes are driven by this class.

### 7.2 Dark Mode CSS Variable Overrides

When `body.dark-mode` is active, the following `:root` overrides apply:

| Token | Light Value | Dark Value |
|---|---|---|
| `--white` | `#ffffff` | `#1a2236` |
| `--off-white` | `#f4f6fb` | `#131c2e` |
| `--gray-50` | `#f8f9fc` | `#1e2a42` |
| `--gray-100` | `#e8ecf4` | `#2a3a54` |
| `--gray-200` | `#d0d7e5` | `#3a4d6a` |
| `--gray-300` | `#9aa5b9` | `#6b7a94` |
| `--gray-400` | `#6b7a94` | `#8a97ad` |
| `--navy-800` (body text) | `#0f1f3d` | `#e8ecf4` |
| `--navy-900` (sidebar) | `#0a1628` | `#0a1628` *(unchanged)* |
| Header background | `rgba(255,255,255,.92)` | `rgba(26,34,54,.92)` |
| Card background | `--white` | `#1a2236` |
| Modal background | `--white` | `#1a2236` |
| Input background | `--gray-50` | `#1e2a42` |
| Input border | `--gray-100` | `#2a3a54` |

> Primary colours (yellow, navy brand tones), semantic colours (danger, success, info, warning), and sidebar chrome remain unchanged in dark mode — they are already dark-optimised.

### 7.3 Components Requiring Special Dark Mode Attention

- **Login screen** — already dark; no changes needed.
- **Tables** — row hover changes to `#1e2a42` instead of `--gray-50`.
- **Scrollbar** — thumb changes to `#2a3a54`.
- **Badges** — background tint opacity may need slight increase for visibility.
- **Stat card top borders** — unchanged (brand colours).

---

## 8. Responsive Design Breakpoints

| Breakpoint | Width | Behaviour |
|---|---|---|
| Desktop | `> 1100px` | Full two-column main grid, 3-column sub-grids |
| Tablet Landscape | `≤ 1100px` | Main grid collapses to single column; 3-col → 2-col |
| Tablet Portrait | `≤ 900px` | Stats grid: 4-col → 2-col |
| Mobile Large | `≤ 768px` | Sidebar hides off-screen; hamburger visible; content padding reduces; all grids → 1-col; page title appears in header |
| Mobile Small | `≤ 520px` | Stats grid: 2-col → 1-col; page header stacks vertically |
| Login (Mobile) | `≤ 860px` | Left panel hidden; right panel full-width, no border radius |

### 8.1 Horizontal Nav Responsive Rules

When horizontal nav mode is active:

- `≤ 1024px`: Nav labels truncate; icons only with tooltips.
- `≤ 768px`: Nav collapses into a hamburger drawer sliding from the top.

---

## 9. Data Management & State

### 9.1 In-Memory Data Store (`DB`)

The current implementation uses a JavaScript object (`const DB`) as a local data store with the following collections:

| Collection | Fields |
|---|---|
| `DB.equipment` | id, name, asset (tag), type, status, assignedTo, location, purchaseDate, warranty, cost, serial |
| `DB.users` | id, name, email, role, dept, initials, color |
| `DB.assignments` | id, equipmentId, user, dept, date, dueDate, status, notes |
| `DB.maintenance` | id, equipmentId, issue, priority, status, created, tech |
| `DB.categories` | id, name, icon, count, color |

### 9.2 CRUD Operations

All four operations (Create, Read, Update, Delete) are supported for each collection via modal forms. Operations mutate `DB` directly and re-render the active page view.

### 9.3 Global Search

A single `input` listener on `#globalSearch` filters visible `tbody tr` elements by text content match, hiding non-matching rows with `display: none`.

### 9.4 Phase 2 — Backend Integration

When a server-side API is introduced, `DB` will be replaced by API calls. All data operations should be refactored into a service layer (`/services/*.js`) to minimise changes to view logic.

---

## 10. Extensibility & Future Modules

The CMS is architected to grow beyond IT equipment. New modules follow the same conventions:

### 10.1 Adding a New Module

1. Add a nav item entry in the sidebar HTML (or nav config object if data-driven).
2. Create a `renderModuleName()` function following the same page-render pattern.
3. Add a collection to `DB` (or a new API endpoint in Phase 2).
4. Register the page in the `navigate()` router function.
5. Add the corresponding modal open/save functions.

### 10.2 Planned Future Modules

| Module | Description |
|---|---|
| Furniture & Office Assets | Desks, chairs, filing cabinets |
| Software Licences | Licence keys, expiry dates, seat counts |
| Vehicles | Company vehicles, insurance, servicing |
| Documents | Policies, contracts, certifications |
| Consumables | Office supplies with stock levels and reorder alerts |

---

## 11. Accessibility & Performance

### 11.1 Accessibility

- All interactive elements must have accessible labels (`aria-label`, `title`, or visible text).
- Keyboard navigation: tab order follows visual order; modal focus is trapped while open.
- Colour contrast: all text/background combinations must meet WCAG AA (minimum 4.5:1 for body text).
- Icons used as sole affordance must include `aria-hidden="true"` with an accompanying visually-hidden label or `title` attribute.
- Form inputs must have associated `<label>` elements.

### 11.2 Performance

- Fonts loaded via `<link rel="preconnect">` + `display=swap` to avoid FOIT.
- CSS custom properties eliminate redundant style rules.
- Animations use `transform` and `opacity` only (GPU-composited; no layout reflow).
- Page renders are synchronous DOM mutations — no async data fetching in the current phase (all data is local).
- `will-change` should not be applied broadly; only on actively animating elements.

---

## 12. File & Asset References

### 12.1 Source File

| File | Description |
|---|---|
| `index.html` | Single-file SPA — all HTML, CSS, and JavaScript |

### 12.2 External Resources

| Resource | URL | Purpose |
|---|---|---|
| Google Fonts | `fonts.googleapis.com` | `Outfit` + `DM Sans` typefaces |
| Cloudflare CDN | `/cdn-cgi/...` | Email obfuscation script (demo artifact) |

### 12.3 Icon System

All icons are inline SVG (`viewBox="0 0 24 24"`, `stroke="currentColor"`, `stroke-width="2"`, `fill="none"`). They are stored in an `IC` object (icon dictionary) in the JavaScript and injected via `innerHTML`. This avoids external icon font requests.

Icons used:

| Key | Visual | Context |
|---|---|---|
| `laptop` | Laptop outline | Equipment type |
| `monitor` | Monitor with stand | Equipment type |
| `server` | Two server rack units | Equipment type |
| `phone` | Mobile phone | Equipment type |
| `printer` | Printer | Equipment type |
| `network` | Network topology | Equipment type |
| `edit` | Pencil on paper | Edit action |
| `trash` | Dustbin | Delete action |
| `eye` | Eye | View action |
| `check` | Checkmark | Confirm / success |
| `plus` | Cross / plus | Add / create |
| `close` | ✕ | Dismiss modal |

---

*End of Requirements Document*

---

> **Document Owner:** IT Department, Access Financial Services Ltd.  
> **Next Review Date:** June 30, 2026  
> **Change requests** should be submitted via the internal IT service desk and approved before implementation begins.
