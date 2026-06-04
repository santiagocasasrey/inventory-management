# Skill: Redesign Vue 3 App to SaaS Sidebar Layout

Transform a Vue 3 application from a horizontal top-navigation layout to a modern SaaS-style interface with a vertical sidebar on the left.

## When to use
Invoke this skill when the user asks to redesign the UI, convert top-nav to sidebar, modernize the layout, or apply a SaaS-style interface to a Vue 3 app.

## Overview of changes
1. Replace the sticky top-nav with a fixed 240px sidebar (dark background, icons + labels)
2. Move ProfileMenu, user controls, and LanguageSwitcher to the sidebar footer
3. Shift FilterBar sticky offset from `top: 70px` to `top: 0`
4. Add CSS design tokens (`:root` variables) for sidebar theming
5. Remove all old top-nav CSS

**IMPORTANT:** Per project rules, ALL `.vue` file edits MUST be delegated to the **vue-expert** subagent.

---

## Phase 1 — Audit (read-only)

Before making any changes, use Explore or Read to gather:

1. **`App.vue`** — extract:
   - All `<router-link>` entries: their `to` path, display label, and active-link logic (`:class` conditions)
   - Where `ProfileMenu`, `LanguageSwitcher`, and any user-facing controls are rendered
   - The current CSS class names on the nav wrapper (e.g. `.top-nav`, `.nav-tabs`)
   - Current `.main-content` padding-top value

2. **`FilterBar.vue`** — find the scoped `top:` value in its sticky/position CSS

3. **`main.js` or router file** — confirm all route paths and their component mappings

Record these before writing any code.

---

## Phase 2 — Design tokens

Add the following CSS variables to the `:root` block in `App.vue`'s `<style>` section (create the block if it doesn't exist):

```css
:root {
  --sidebar-width: 240px;
  --sidebar-bg: #0f172a;
  --sidebar-text: #94a3b8;
  --sidebar-text-active: #f8fafc;
  --sidebar-active-bg: #1e293b;
  --sidebar-hover-bg: #1e293b;
  --sidebar-border: rgba(255, 255, 255, 0.06);
  --brand-primary: #2563eb;
}
```

---

## Phase 3 — Layout transformation in `App.vue`

### Template changes

Replace the top-nav `<header>` block with a `.sidebar` element. The content area wraps FilterBar + `<router-view>`.

**Target structure:**
```html
<div class="app-layout">
  <!-- Sidebar -->
  <aside class="sidebar">
    <div class="sidebar-brand">
      <!-- Logo SVG or initials + App name -->
      <span class="sidebar-brand-name">{{ appName }}</span>
    </div>

    <nav class="sidebar-nav">
      <router-link
        v-for="item in navItems"
        :key="item.path"
        :to="item.path"
        class="sidebar-nav-item"
        :class="{ active: isActive(item.path) }"
      >
        <span class="nav-icon" v-html="item.icon"></span>
        <span class="nav-label">{{ item.label }}</span>
      </router-link>
    </nav>

    <div class="sidebar-footer">
      <LanguageSwitcher />
      <ProfileMenu />
    </div>
  </aside>

  <!-- Main content -->
  <div class="content-area">
    <FilterBar />
    <main class="main-content">
      <router-view />
    </main>
  </div>
</div>
```

### navItems data (adapt labels/icons to match the app's actual routes)

Define `navItems` as a `const` in the `<script setup>` section using the routes found in Phase 1. Use inline SVG strings for icons (no icon library needed). Example icons to use:

```javascript
const navItems = [
  {
    path: '/',
    label: 'Overview',
    icon: `<svg width="20" height="20" fill="none" stroke="currentColor" stroke-width="1.75" viewBox="0 0 24 24">
      <rect x="3" y="3" width="7" height="7" rx="1"/><rect x="14" y="3" width="7" height="7" rx="1"/>
      <rect x="3" y="14" width="7" height="7" rx="1"/><rect x="14" y="14" width="7" height="7" rx="1"/>
    </svg>`
  },
  {
    path: '/inventory',
    label: 'Inventory',
    icon: `<svg width="20" height="20" fill="none" stroke="currentColor" stroke-width="1.75" viewBox="0 0 24 24">
      <path d="M20 7l-8-4-8 4m16 0l-8 4m8-4v10l-8 4m0-10L4 7m8 4v10M4 7v10l8 4"/>
    </svg>`
  },
  {
    path: '/orders',
    label: 'Orders',
    icon: `<svg width="20" height="20" fill="none" stroke="currentColor" stroke-width="1.75" viewBox="0 0 24 24">
      <path d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-6 9l2 2 4-4"/>
    </svg>`
  },
  {
    path: '/spending',
    label: 'Finance',
    icon: `<svg width="20" height="20" fill="none" stroke="currentColor" stroke-width="1.75" viewBox="0 0 24 24">
      <path d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z"/>
    </svg>`
  },
  {
    path: '/demand',
    label: 'Demand',
    icon: `<svg width="20" height="20" fill="none" stroke="currentColor" stroke-width="1.75" viewBox="0 0 24 24">
      <polyline points="22 7 13.5 15.5 8.5 10.5 2 17"/><polyline points="16 7 22 7 22 13"/>
    </svg>`
  },
  {
    path: '/reports',
    label: 'Reports',
    icon: `<svg width="20" height="20" fill="none" stroke="currentColor" stroke-width="1.75" viewBox="0 0 24 24">
      <path d="M9 17v-2m3 2v-4m3 4v-6m2 10H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"/>
    </svg>`
  }
]
```

### isActive helper (adapt to the app's existing active-link logic)
```javascript
import { useRoute } from 'vue-router'
const route = useRoute()

function isActive(path) {
  if (path === '/') return route.path === '/'
  return route.path.startsWith(path)
}
```

---

## Phase 4 — CSS for `App.vue`

### Remove these selectors entirely
`.top-nav`, `.nav-container`, `.nav-logo`, `.nav-brand`, `.nav-tabs`, `.nav-tab`, `.nav-tab.active`, `.nav-actions`, any top-nav hover/media rules.

### Add these new selectors

```css
/* Layout shell */
.app-layout {
  display: flex;
  height: 100vh;
  overflow: hidden;
}

/* Sidebar */
.sidebar {
  width: var(--sidebar-width);
  min-width: var(--sidebar-width);
  height: 100vh;
  background: var(--sidebar-bg);
  display: flex;
  flex-direction: column;
  position: fixed;
  left: 0;
  top: 0;
  z-index: 100;
  border-right: 1px solid var(--sidebar-border);
}

.sidebar-brand {
  display: flex;
  align-items: center;
  gap: 0.625rem;
  padding: 1.25rem 1rem;
  border-bottom: 1px solid var(--sidebar-border);
  min-height: 64px;
}

.sidebar-brand-logo {
  width: 32px;
  height: 32px;
  background: var(--brand-primary);
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-weight: 700;
  font-size: 0.875rem;
  flex-shrink: 0;
}

.sidebar-brand-name {
  font-size: 0.9375rem;
  font-weight: 700;
  color: var(--sidebar-text-active);
  letter-spacing: -0.01em;
}

/* Nav links */
.sidebar-nav {
  flex: 1;
  padding: 0.75rem 0.625rem;
  display: flex;
  flex-direction: column;
  gap: 0.125rem;
  overflow-y: auto;
}

.sidebar-nav-item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.5625rem 0.75rem;
  border-radius: 6px;
  color: var(--sidebar-text);
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 500;
  transition: background 0.15s ease, color 0.15s ease;
  cursor: pointer;
}

.sidebar-nav-item:hover {
  background: var(--sidebar-hover-bg);
  color: var(--sidebar-text-active);
}

.sidebar-nav-item.active {
  background: var(--sidebar-active-bg);
  color: var(--sidebar-text-active);
}

.sidebar-nav-item.active .nav-icon {
  color: var(--brand-primary);
}

.nav-icon {
  display: flex;
  align-items: center;
  flex-shrink: 0;
  color: inherit;
  transition: color 0.15s ease;
}

.nav-label {
  flex: 1;
}

/* Sidebar footer */
.sidebar-footer {
  padding: 0.75rem 0.625rem;
  border-top: 1px solid var(--sidebar-border);
  display: flex;
  align-items: center;
  justify-content: space-between;
}

/* Content area */
.content-area {
  margin-left: var(--sidebar-width);
  flex: 1;
  height: 100vh;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  background: #f8fafc;
}

/* Main content padding */
.main-content {
  flex: 1;
  padding: 1.5rem 2rem;
  max-width: 1600px;
  width: 100%;
  box-sizing: border-box;
}
```

---

## Phase 5 — FilterBar sticky offset fix

In `client/src/components/FilterBar.vue`, change the sticky position from `top: 70px` to `top: 0`:

```css
/* Before */
.filter-bar { position: sticky; top: 70px; z-index: 90; }

/* After */
.filter-bar { position: sticky; top: 0; z-index: 90; }
```

---

## Phase 6 — Delegate to vue-expert

Collect the full spec from Phases 1–5 and hand it off to the **vue-expert** subagent with:
- The complete template structure (Phase 3)
- The navItems array with icons (Phase 3) adapted to this app's actual routes
- The full CSS block (Phase 4)
- The FilterBar fix (Phase 5)
- Instruction to remove old top-nav CSS entirely

Tell the vue-expert to preserve ALL existing scoped styles in each view and ALL modal/composable logic unchanged.

---

## Phase 7 — Verification

After changes are applied:

1. Start the dev server: `npm run dev` in `client/`
2. Use Playwright (`mcp__playwright__*`) to:
   - Navigate to `http://localhost:3000`
   - Take a screenshot — sidebar must be visible on the left, no top-nav
   - Click each nav link and confirm active state highlights in the sidebar
   - Scroll down on a long page (e.g. `/inventory`) — FilterBar must stay sticky at the top of the content area
   - Open a modal (e.g. click an inventory item) — confirm modal still renders correctly
3. Check browser console for errors

---

## Common pitfalls
- **Overflow hidden on `.app-layout`** breaks sticky FilterBar inside `.content-area`. Set `overflow: hidden` only on `.app-layout`, and `overflow-y: auto` on `.content-area` — sticky works relative to the scrolling ancestor.
- **z-index conflicts**: Sidebar at z-100, FilterBar at z-90, modals (Teleport to body) at z-1000+. Keep this order.
- **`router-link` active class**: Vue Router adds `.router-link-active` and `.router-link-exact-active` automatically. You can use these instead of a manual `isActive()` helper if simpler.
- **ProfileMenu and LanguageSwitcher** may have positioning assumptions (e.g. `position: absolute` dropdowns that expand upward). Test that their dropdowns are visible after moving them to the sidebar footer.
