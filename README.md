# CommandCenter Dashboard

Minimal, functional admin dashboard. Pure HTML + Tailwind CSS. Zero build step. Backend-ready.

---

## Stack

| Layer | Tech |
|---|---|
| Markup | HTML5 |
| Style | Tailwind CSS (Play CDN) |
| Logic | Vanilla JS |
| Backend (planned) | Django or Flask |
| Charts (planned) | Chart.js via CDN |

---

## File Structure (current)

```
dashboard.html        ← single file, all phases merged
README.md
```

## File Structure (target)

```
/
├── dashboard.html
├── analytics.html
├── users.html
├── reports.html
├── settings.html
├── assets/
│   └── (icons, logo if any)
└── README.md
```

---

## Pages & Status

| Page | File | Status | Key Features |
|---|---|---|---|
| Dashboard | dashboard.html | ✅ Done | stats, table, activity, modal |
| Analytics | analytics.html | 🔲 Planned | charts, KPIs, date range |
| Users | users.html | 🔲 Planned | user table, roles, invite |
| Reports | reports.html | 🔲 Planned | builder, filters, export |
| Settings | settings.html | 🔲 Planned | profile, prefs, danger zone |

---

## Components Inventory

| Component | Location | Notes |
|---|---|---|
| Navbar | All pages | fixed, 56px, dark toggle, bell, avatar |
| Sidebar | All pages | 240px / 64px, mobile slide-in |
| Stat Cards | Dashboard | 4-col grid, trend badge, sparkline (planned) |
| Data Table | Dashboard, Reports, Users | sort, filter tabs, row actions, pagination |
| Dropdowns | Navbar, Header | mutual-exclusion, outside-click close |
| Modal | Dashboard | scale+opacity transition, backdrop close |
| Notification Panel | Navbar | 3 items, "view all" link |
| User Dropdown | Navbar | profile, settings, sign out |
| Filter Dropdown | Header | checkboxes, apply button |
| Tab Switcher | Table section | pill style, JS active toggle |
| Search Input | Table section | CSS width expand on focus |
| Dark Mode Toggle | Navbar | persisted to localStorage |

---

## Planned Visuals

| Visual | Page | Library |
|---|---|---|
| Line chart | Analytics | Chart.js |
| Bar chart | Analytics | Chart.js |
| Donut chart | Analytics | Chart.js |
| Area chart | Analytics | Chart.js |
| Sparklines | Dashboard cards | Chart.js inline |
| Heatmap calendar | Analytics | custom HTML/CSS |
| Progress bars | Dashboard, Reports | pure CSS |
| Gauge/radial | Dashboard | Chart.js |
| Timeline/feed | Dashboard, Users | pure HTML |
| Map placeholder | Analytics | placeholder → Leaflet later |

---

## Component Selector (planned)

- Gear/grid icon button on Dashboard opens a "Customize" panel
- Toggle switches per widget: Stat Cards, Activity Feed, Chart, Table, etc.
- Visibility state saved to localStorage key: `dashboard_widgets`
- Backend swap: POST to `/api/user/preferences` when auth is added

---

## Backend Integration Plan

### API Endpoints (to build)

| Method | Endpoint | Returns |
|---|---|---|
| GET | `/api/stats` | 4 stat card values + trends |
| GET | `/api/activity` | recent activity feed items |
| GET | `/api/reports` | table rows |
| GET | `/api/users` | user list |
| GET | `/api/analytics` | chart datasets |
| POST | `/api/reports` | create report |
| POST | `/api/user/preferences` | save widget layout |

### Data Contract (current static → future API)

All static data in the HTML is written as plain JS objects.
Swap pattern per page:

```js
// Current
const stats = { revenue: "$48,295", users: 3842 }

// After backend
const stats = await fetch("/api/stats").then(r => r.json())
```

### Django Notes
- Add `{% csrf_token %}` inside all `<form>` tags
- Use `data-csrf="{{ csrf_token }}"` on body for JS fetch calls
- Templates go in `/templates/`, extend a `base.html`
- Static files: `{% load static %}` + `{% static '...' %}`

### Flask Notes
- Use `render_template()` for each page
- CSRF via Flask-WTF: `{{ form.hidden_tag() }}`
- Pass data as `render_template("dashboard.html", stats=stats_dict)`

---

## localStorage Keys

| Key | Value | Used by |
|---|---|---|
| `theme` | `"light"` or `"dark"` | dark mode toggle |
| `sidebar` | `"open"` or `"closed"` | sidebar state |
| `dashboard_widgets` | JSON object | component selector (planned) |

---

## Design Tokens

| Token | Light | Dark |
|---|---|---|
| Background | gray-50 | gray-950 |
| Surface | white | gray-900 |
| Text primary | gray-800 | gray-100 |
| Text muted | gray-500 | gray-400 |
| Accent | indigo-600 | indigo-400 |
| Border | gray-200 | gray-700 |
| Danger | red-600 | red-400 |

---

## Phase Log

| Phase | Scope | Status |
|---|---|---|
| 1 | Navbar, sidebar, dark mode | ✅ |
| 2 | Stat cards, layout, activity feed | ✅ |
| 3 | Dropdowns, notifications, modal | ✅ |
| 4 | Data table, sort, row actions, pagination | ✅ |
| 5 | Mobile, hamburger, polish | 🔄 Running |
| 6 | Analytics page + charts | 🔲 |
| 7 | Users page | 🔲 |
| 8 | Reports page | 🔲 |
| 9 | Settings page | 🔲 |
| 10 | Component selector / widget customizer | 🔲 |
| 11 | Backend wiring (Django or Flask) | 🔲 |

---

## Conventions

- One dropdown open at a time — enforced globally
- All JS in single `<script>` block at bottom of file
- No inline `onclick` — all `addEventListener`
- Dark mode via `dark` class on `<html>`, not `prefers-color-scheme` alone
- `data-*` attributes drive JS behavior, not CSS classes