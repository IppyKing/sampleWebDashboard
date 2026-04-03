# Access Financial Services Ltd.
## Content Management System — Project Requirements Document

**Version:** 1.0  
**Date:** March 31, 2026  
**Prepared for:** Access Financial Services Ltd.  
**Document Type:** Frontend & System Requirements Specification

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Brand & Design System](#2-brand--design-system)
3. [Layout & Navigation](#3-layout--navigation)
4. [Theme & Color Modes](#4-theme--color-modes)
5. [Pages & Features](#5-pages--features)
6. [Responsiveness Requirements](#6-responsiveness-requirements)
7. [Accessibility & Performance](#7-accessibility--performance)
8. [Security Requirements](#8-security-requirements)
9. [Future Modules](#9-future-modules)
10. [Technical Stack Recommendations](#10-technical-stack-recommendations)
11. [Deliverables Checklist](#11-deliverables-checklist)

---

## 1. Project Overview

Access Financial Services Ltd. requires a modern, scalable **Content Management System (CMS)** to manage internal assets, starting with IT equipment. The system must support multiple user roles, maintain full audit trails, and provide a clean, professional interface that reflects the company's brand identity.

The CMS is designed to grow — IT equipment is the first module, but the architecture must allow for seamless addition of other item categories in the future (e.g., office furniture, vehicles, software licenses).

### Goals

- Centralize the tracking and lifecycle management of IT equipment and future assets
- Enforce role-based access control across all modules
- Maintain tamper-evident audit logs for compliance and accountability
- Deliver a responsive experience across desktop, tablet, and mobile devices
- Reflect the Access Financial Services brand with a polished, professional UI

---

## 2. Brand & Design System

### 2.1 Company Identity

| Property     | Value                          |
|-------------|-------------------------------|
| Company Name | Access Financial Services Ltd. |
| Abbreviation | AFS                           |
| Industry     | Financial Services             |
| Tone         | Professional, trustworthy, modern |

### 2.2 Color Palette

| Role              | Color Name   | Hex       | Usage                                      |
|-------------------|--------------|-----------|--------------------------------------------|
| **Primary**       | Navy Blue    | `#0A1F44` | Headers, sidebar, primary buttons, logo    |
| **Accent**        | AFS Yellow   | `#F5C518` | CTAs, highlights, active states, badges    |
| **Alert / Error** | AFS Red      | `#C0392B` | Destructive actions, error states, alerts  |
| **Background**    | White        | `#FFFFFF` | Main content background (light mode)       |
| **Surface**       | Light Gray   | `#F4F6F9` | Card backgrounds, table rows               |
| **Text Primary**  | Charcoal     | `#1A1A2E` | Body text                                  |
| **Text Secondary**| Slate Gray   | `#6B7280` | Subtitles, metadata, placeholders          |
| **Border**        | Cool Gray    | `#E2E8F0` | Dividers, input borders                    |

> **Note:** Red is optional and should be used sparingly — reserved for destructive actions (delete, disable), critical alerts, and error feedback only.

### 2.3 Typography

| Role            | Font Family              | Weight       | Size Range   |
|-----------------|--------------------------|--------------|--------------|
| Display / H1    | `Playfair Display`       | 700          | 32–48px      |
| Headings H2–H4  | `Outfit` or `DM Sans`    | 600          | 18–28px      |
| Body            | `Inter` or `DM Sans`     | 400 / 500    | 14–16px      |
| Monospace/Code  | `JetBrains Mono`         | 400          | 12–14px      |
| Labels / Badges | `Outfit`                 | 600          | 11–13px      |

### 2.4 Iconography

- Use **Lucide Icons** or **Heroicons** (outline style) for consistency
- Icon sizes: 16px (inline), 20px (nav), 24px (dashboard cards)
- Icons must support color inheritance for dark mode compatibility

### 2.5 Spacing & Border Radius

| Property         | Value   |
|-----------------|---------|
| Base unit        | 4px     |
| Component radius | 8px     |
| Card radius      | 12px    |
| Button radius    | 6px     |
| Modal radius     | 16px    |

---

## 3. Layout & Navigation

### 3.1 Navigation Bar — Core Requirements

The navigation must be **collapsible** and support switching between **horizontal (top bar)** and **vertical (sidebar)** orientations. The user's preference must persist across sessions (via `localStorage` or user profile settings).

#### Horizontal Mode (Top Bar)
- Fixed at the top of the viewport
- Contains: AFS logo (left), primary nav links (center/left), user avatar + notifications (right)
- Collapses into a **hamburger menu** on tablets and mobile
- Dropdown menus for grouped sections (e.g., "Assets" > IT Equipment, Other Items)

#### Vertical Mode (Sidebar)
- Fixed left sidebar, default width: **240px**
- Collapses to icon-only rail at **64px** on user toggle or at viewports under 1024px
- Contains: AFS logo/monogram at top, nav items with icons + labels, user info at bottom
- Active item highlighted with yellow left-border accent and navy background tint
- Sections separated by subtle dividers with group labels (e.g., "MANAGEMENT", "SYSTEM")

#### Nav Toggle Control
- A visible toggle button (arrow icon or hamburger) allows switching between modes
- Transition: smooth CSS animation (300ms ease)
- Mode preference saved to user session

### 3.2 Navigation Structure

| Label             | Route              | Roles Allowed          |
|-------------------|--------------------|------------------------|
| Dashboard         | `/dashboard`       | All                    |
| IT Equipment      | `/equipment`       | All                    |
| User Management   | `/users`           | Admin, Super Admin     |
| Audit Log         | `/audit`           | Admin, Super Admin     |
| Inventory         | `/inventory`       | All (future module)    |
| Settings          | `/settings`        | Admin, Super Admin     |
| Logout            | (action)           | All                    |

### 3.3 Breadcrumbs

- All inner pages must display a breadcrumb trail below the top bar or sidebar header
- Example: `Dashboard > IT Equipment > Edit Item`
- Breadcrumbs are clickable links except for the current page (displayed as plain text)

---

## 4. Theme & Color Modes

### 4.1 Light Mode (Default)

| Element           | Value                                          |
|------------------|------------------------------------------------|
| Page background  | `#FFFFFF`                                      |
| Card / surface   | `#F4F6F9`                                      |
| Sidebar bg       | `#0A1F44` (Navy)                               |
| Sidebar text     | `#FFFFFF`                                      |
| Primary text     | `#1A1A2E`                                      |
| Border           | `#E2E8F0`                                      |
| Active nav item  | Yellow `#F5C518` left border + navy bg tint    |

### 4.2 Dark Mode

| Element           | Value                                          |
|------------------|------------------------------------------------|
| Page background  | `#0D1117`                                      |
| Card / surface   | `#161B22`                                      |
| Sidebar bg       | `#0A1220`                                      |
| Primary text     | `#E6EDF3`                                      |
| Secondary text   | `#8B949E`                                      |
| Border           | `#30363D`                                      |
| Accent (yellow)  | `#F5C518` (unchanged)                          |
| Active nav item  | Yellow `#F5C518` left border + dark surface tint|

### 4.3 Theme Toggle

- Toggle button (sun/moon icon) accessible in the top-right of the nav bar
- System preference auto-detected on first load (`prefers-color-scheme` media query)
- Preference stored in `localStorage` and/or user profile
- Transition: smooth 300ms color fade using CSS custom properties

---

## 5. Pages & Features

### 5.1 Login Page

**Route:** `/` or `/login`  
**Access:** Public (unauthenticated)

#### Layout
- Split-panel design: left panel (brand visual/illustration), right panel (form)
- On mobile: full-screen form with AFS logo at top
- AFS logo prominently displayed in navy + yellow

#### Components
- Email / Username field with validation
- Password field with show/hide toggle
- Remember Me checkbox
- Login button (navy background, yellow hover state)
- Forgot Password link — triggers email reset flow
- Error state: red inline message beneath affected field(s) on failure
- Loading state: spinner inside login button during API call

#### Security & Feedback
- Rate limiting message (e.g., "Too many attempts. Try again in 30 seconds.")
- CSRF protection (handled server-side)
- Redirect to `/dashboard` on successful authentication

---

### 5.2 Dashboard

**Route:** `/dashboard`  
**Access:** All authenticated roles

#### Summary Cards (Top Row)
- Total IT Equipment
- Equipment Checked Out
- Equipment Under Maintenance
- Total Users (visible to Admin / Super Admin only)

Each card displays: icon, metric value, descriptive label, and a subtle trend indicator (up/down arrow with percentage).

#### Quick Actions Panel
- Add New Equipment
- View Recent Audit Logs
- Manage Users (Admin only)

#### Recent Activity Feed
- Last 10 audit events showing who did what and when
- Filterable by event type

#### Equipment Status Chart
- Doughnut or bar chart showing equipment count by status: Active, Maintenance, Retired, Checked Out

---

### 5.3 IT Equipment Management

**Route:** `/equipment`  
**Access:** All authenticated roles (write access gated by role)

#### Equipment List View

Responsive data table with the following columns:

| Column        | Description                                         |
|--------------|-----------------------------------------------------|
| Asset Tag     | Auto-generated or manually assigned identifier     |
| Name          | Item name and short description                    |
| Category      | Laptop, Desktop, Monitor, Peripheral, Network, etc.|
| Assigned To   | User name or department                            |
| Status        | Active, Maintenance, Retired, Checked Out          |
| Location      | Physical or office location                        |
| Last Updated  | Timestamp of most recent change                    |
| Actions       | View, Edit, Delete (role-gated)                    |

Additional table features:
- Real-time search bar
- Filter dropdowns: Category, Status, Department, Location
- Sortable column headers
- Pagination: 10 / 25 / 50 records per page
- Bulk actions: Assign, Export, Delete (Admin only)
- Export button: CSV and PDF

#### Equipment Detail View
**Route:** `/equipment/:id`

- Full record with all fields displayed
- Assignment history timeline
- Maintenance log entries
- Attached documents or images (optional)
- Edit and Delete buttons (role-gated)

#### Add / Edit Equipment Form
**Routes:** `/equipment/new` and `/equipment/:id/edit`

Form fields:

| Field              | Type                   | Required |
|--------------------|------------------------|----------|
| Asset Tag          | Text (auto or manual)  | Yes      |
| Item Name          | Text                   | Yes      |
| Category           | Dropdown               | Yes      |
| Brand / Manufacturer| Text                  | Yes      |
| Model              | Text                   | Yes      |
| Serial Number      | Text                   | Yes      |
| Purchase Date      | Date picker            | Yes      |
| Purchase Price     | Currency input         | No       |
| Warranty Expiry    | Date picker            | No       |
| Current Status     | Dropdown               | Yes      |
| Assigned To        | User search + select   | No       |
| Department         | Dropdown               | No       |
| Location           | Dropdown               | No       |
| Condition          | Dropdown (New/Good/Fair/Poor) | Yes |
| Notes              | Textarea               | No       |
| Attachments        | File upload            | No       |

- All required fields validated before submission
- Cancel and Save buttons
- Confirmation modal displayed before delete

---

### 5.4 User Management Page

**Route:** `/users`  
**Access:** Admin, Super Admin

#### User List Table

| Column       | Description                              |
|-------------|------------------------------------------|
| Avatar       | User initials or uploaded photo          |
| Full Name    | First and last name                      |
| Email        | Login email address                      |
| Role         | Viewer, Editor, Admin, Super Admin       |
| Department   | Assigned department                      |
| Status       | Active or Inactive                       |
| Last Login   | Timestamp of most recent session         |
| Actions      | View, Edit, Deactivate                   |

- Search by name or email
- Filter by Role, Department, Status
- Pagination

#### Add / Edit User Form

| Field        | Notes                                              |
|-------------|---------------------------------------------------|
| First Name   | Required                                          |
| Last Name    | Required                                          |
| Email        | Required, unique, validated format                |
| Role         | Dropdown — cannot change own role                 |
| Department   | Dropdown                                          |
| Phone        | Optional                                          |
| Password     | Required on create only, hidden on edit           |
| Status       | Active / Inactive toggle                          |

#### User Detail View
- Profile summary card
- Last 20 activity events pulled from audit log
- List of equipment currently assigned to the user

#### Role Permissions Reference

| Permission               | Viewer | Editor | Admin | Super Admin |
|--------------------------|--------|--------|-------|-------------|
| View equipment           | Yes    | Yes    | Yes   | Yes         |
| Add / Edit equipment     | No     | Yes    | Yes   | Yes         |
| Delete equipment         | No     | No     | Yes   | Yes         |
| View users               | No     | No     | Yes   | Yes         |
| Add / Edit users         | No     | No     | Yes   | Yes         |
| Delete users             | No     | No     | No    | Yes         |
| View audit logs          | No     | No     | Yes   | Yes         |
| Export data              | No     | Yes    | Yes   | Yes         |
| Manage system settings   | No     | No     | No    | Yes         |

---

### 5.5 Audit Log Page

**Route:** `/audit`  
**Access:** Admin, Super Admin

#### Purpose
Provides a tamper-evident, read-only record of all actions performed within the system, supporting compliance requirements and accountability investigations.

#### Log Table Columns

| Column          | Description                                          |
|----------------|------------------------------------------------------|
| Timestamp       | Date and time with timezone                         |
| User            | Name and avatar of the acting user                  |
| Action          | CREATED, UPDATED, DELETED, LOGIN, LOGOUT, EXPORT    |
| Module          | Equipment, Users, Settings, Auth                    |
| Record Affected | Linked reference to the affected item               |
| IP Address      | Originating IP of the request                       |
| Details         | Expandable row: field-level before/after diff       |

#### Filter Controls
- Date range picker (start and end date)
- User filter (search by name)
- Action type (multi-select checkboxes)
- Module filter (dropdown)
- IP address search (text input)

#### Constraints
- All audit log entries are **read-only** — no user, including Super Admin, may edit or delete them
- Paginated: 25 / 50 / 100 records per page
- Export to CSV or PDF of current filtered view

---

### 5.6 Settings Page

**Route:** `/settings`  
**Access:** Super Admin

#### General Settings
- Company display name
- Company logo upload (replaces default AFS branding)
- System timezone selection
- Date and time format preference

#### Email & Notification Settings
- SMTP server configuration (host, port, credentials)
- Email template management (welcome email, password reset, alert emails)
- Notification triggers (e.g., warranty expiring within 30 days)

#### Security Settings
- Session idle timeout duration (configurable, default 30 minutes)
- Password policy: minimum length, complexity requirements, expiry interval
- Two-factor authentication: enable/disable system-wide
- IP allowlist: restrict access to specified IP ranges (optional)

#### System Configuration
- Asset tag prefix and auto-increment format
- Department list management (add, rename, deactivate)
- Location list management (add, rename, deactivate)
- Equipment category list management (add, rename, deactivate)

#### Backup & Maintenance
- Trigger manual database backup
- Display current system version and changelog

---

### 5.7 Error Pages

| Page | Route      | Description                                                  |
|------|-----------|--------------------------------------------------------------|
| 404  | `*`       | Friendly "Page not found" with logo and link to Dashboard    |
| 403  | `/403`    | "Access denied" with role explanation and support contact    |
| 500  | `/500`    | "Something went wrong" with technical reference and contact  |

All error pages maintain full branding (navy, yellow, AFS logo) and responsive layout.

---

## 6. Responsiveness Requirements

| Breakpoint     | Width        | Navigation Behavior                       | Content Layout         |
|----------------|--------------|-------------------------------------------|------------------------|
| Mobile         | under 640px  | Hamburger menu, full-screen drawer        | Single column, stacked |
| Tablet         | 640–1023px   | Sidebar collapses to 64px icon rail       | 2-column cards         |
| Laptop         | 1024–1279px  | Full sidebar or top nav (user preference) | 3-column cards         |
| Desktop        | 1280px+      | Full 240px sidebar                        | 4-column cards         |

Additional responsiveness rules:
- Data tables scroll horizontally on mobile — no columns are hidden
- Forms collapse to single column on mobile
- Modals render full-screen on mobile viewports
- All interactive touch targets must be at minimum 44x44px (WCAG 2.1 AA)

---

## 7. Accessibility & Performance

### Accessibility Standards — WCAG 2.1 AA

- All interactive elements fully keyboard navigable (Tab, Enter, Escape, Arrow keys)
- Color contrast ratio: minimum 4.5:1 for body text, 3:1 for large text
- All images carry descriptive `alt` attributes
- Form inputs are associated with visible `<label>` elements
- ARIA roles and `aria-label` attributes applied to modals, dropdowns, and dynamic alerts
- Focus indicators must be clearly visible (recommended: yellow outline, 2px solid)
- Compatible with major screen readers (NVDA on Windows, VoiceOver on macOS/iOS)

### Performance Targets

| Metric                         | Target              |
|-------------------------------|---------------------|
| First Contentful Paint (FCP)  | Under 1.5 seconds   |
| Largest Contentful Paint (LCP)| Under 2.5 seconds   |
| Time to Interactive (TTI)     | Under 3.5 seconds   |
| Cumulative Layout Shift (CLS) | Under 0.1           |
| Lighthouse Score              | 90 or above (all)   |

Performance implementation:
- Lazy loading for images and below-the-fold components
- Route-based code splitting
- Asset minification and Gzip / Brotli compression
- API response caching for read-heavy endpoints

---

## 8. Security Requirements

| Area                | Requirement                                                             |
|--------------------|-------------------------------------------------------------------------|
| Authentication      | JWT with refresh token rotation, or server-side sessions               |
| Authorization       | RBAC enforced on both client and server                                 |
| Input Validation    | All inputs sanitized and validated frontend and backend                |
| XSS Protection      | Dynamic DOM content escaped at all render points                       |
| CSRF Protection     | CSRF tokens on all state-mutating HTTP requests                        |
| Transport Security  | HTTPS enforced; HTTP automatically redirects to HTTPS                  |
| Password Storage    | Bcrypt hashing, minimum 12 rounds; plaintext never stored              |
| Audit Trail         | All login attempts (success and failure) logged with IP and timestamp  |
| Session Management  | Configurable idle timeout with warning modal before expiry             |
| Rate Limiting       | Login endpoint rate-limited to prevent brute-force attacks             |
| Data Masking        | Serial numbers and PII partially masked in Viewer-role views           |

---

## 9. Future Modules

The architecture must support adding new asset modules without significant refactoring. Modules should be implemented as feature-flagged plugins, toggleable from the Settings page.

| Module                 | Description                                            |
|-----------------------|--------------------------------------------------------|
| Office Furniture       | Desks, chairs, cabinets, and fixtures                 |
| Software Licenses      | License keys, seat counts, renewal tracking           |
| Vehicle Fleet          | Company vehicles, insurance, service schedules        |
| Facilities             | Office locations, lease terms, utility accounts       |
| Consumables            | Toner, paper, supplies with stock level tracking      |
| Procurement            | Purchase requests, approvals, vendor management       |
| Maintenance Scheduling | Service reminders and work order management           |
| Reporting & Analytics  | Custom report builder and exportable dashboards       |

---

## 10. Technical Stack Recommendations

These recommendations may be adapted based on the development team's existing infrastructure and expertise.

### Frontend

| Layer          | Recommended Technology               |
|---------------|--------------------------------------|
| Framework      | React 18+ with TypeScript            |
| Styling        | Tailwind CSS + CSS custom properties |
| State Mgmt     | Zustand or Redux Toolkit             |
| Routing        | React Router v6                      |
| Forms          | React Hook Form + Zod validation     |
| Data Fetching  | TanStack Query (React Query)         |
| Charts         | Recharts or Chart.js                 |
| Icons          | Lucide React                         |
| Date Picker    | React Day Picker                     |
| Data Tables    | TanStack Table                       |

### Backend (Reference)

| Layer          | Recommended Technology               |
|---------------|--------------------------------------|
| Runtime        | Node.js with Express or Laravel (PHP)|
| Database       | PostgreSQL                           |
| ORM            | Prisma (Node) or Eloquent (PHP)      |
| Authentication | JWT with refresh token rotation      |
| File Storage   | AWS S3 or local filesystem with CDN  |
| Email          | Nodemailer or SendGrid               |

### DevOps (Reference)
- CI/CD pipeline: GitHub Actions
- Containerization: Docker and Docker Compose
- Hosting: AWS, DigitalOcean, or Azure
- SSL certificates: Let's Encrypt via Certbot

---

## 11. Deliverables Checklist
 
### Design Deliverables
- [ ] Design system and style guide (color tokens, typography, component library)
- [ ] Wireframes for all pages — mobile and desktop viewports
- [ ] High-fidelity mockups (Figma recommended)
- [ ] Interactive prototype covering key navigation and user flows

### Development Deliverables
- [ ] Login page with forgot password flow
- [ ] Dashboard with summary cards and recent activity feed
- [ ] IT Equipment — list view, detail view, add form, edit form, delete flow
- [ ] User Management — list, add, edit, deactivate
- [ ] Audit Log — filterable table, expandable rows, export
- [ ] Settings page (Super Admin access)
- [ ] Responsive navigation: horizontal and vertical modes, collapsible
- [ ] Dark mode toggle with session persistence
- [ ] 404, 403, and 500 error pages
- [ ] Role-based route protection (client and server)
- [ ] API integration layer with error handling
- [ ] Client-side form validation
- [ ] CSV and PDF export functionality

### QA & Testing
- [ ] Unit tests for utility functions and reusable components
- [ ] Integration tests for authentication flow and CRUD operations
- [ ] Cross-browser testing: Chrome, Firefox, Safari, Edge
- [ ] Mobile device testing: iOS Safari, Android Chrome
- [ ] Accessibility audit against WCAG 2.1 AA
- [ ] Lighthouse performance audit (target score >= 90)
- [ ] Security review against OWASP Top 10

---

*This document is subject to revision based on feedback from Access Financial Services Ltd. stakeholders. All measurements, colors, and component specifications should be reviewed and confirmed before development begins.*

---

**Document maintained by:** Project Team  
**Last Updated:** March 31, 2026  
**Status:** Draft — Pending Stakeholder Review
