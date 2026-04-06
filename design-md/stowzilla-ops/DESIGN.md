# DESIGN.md — Stowzilla Ops

> Internal operations dashboard for a valet storage company. Built with React + Vite + AWS Cloudscape Design components. Light-theme-first, data-dense, staff-facing power tool.

---

## 1. Visual Theme & Atmosphere

| Attribute | Value |
|-----------|-------|
| Mood | Professional, utilitarian, quietly confident |
| Density | High — this is an ops tool for trained staff, not a marketing site |
| Philosophy | Cloudscape provides the structural skeleton; custom CSS layers on brand identity. Complexity is acceptable when it serves efficiency. Every pixel earns its place by helping staff move faster through inventory, scheduling, and billing workflows. |
| Light/Dark | Light-theme-first. White surfaces on a cool grey canvas. No dark mode. |
| Personality | Calm teal accents on neutral ground. The interface stays out of the way — it's a tool, not a destination. Think "well-organized warehouse office" not "startup landing page." |

---

## 2. Color Palette & Roles

### Core Palette

| Token | Hex | Role |
|-------|-----|------|
| `--background-color` | `#f3f7f8` | Page canvas, app background |
| `--surface-color` | `#ffffff` | Cards, panels, modals, table rows |
| `--accent-color` | `#1b998b` | Primary actions, active nav, brand teal |
| `--accent-dark` | `#147c70` | Hover/pressed state for primary actions |
| `--text-color` | `#163149` | Headings, body text, primary content |
| `--muted-color` | `#677389` | Secondary text, labels, timestamps, placeholders |
| `--border-color` | `rgba(22, 49, 73, 0.08)` | Card borders, dividers, table rules |

### Login

| Token | Value | Role |
|-------|-------|------|
| Login gradient | `linear-gradient(135deg, #1e4620 0%, #2d5a2e 100%)` | Full-page login background, dark forest green |

### Semantic Colors

| Name | Hex | Role |
|------|-----|------|
| Cloudscape focus | `#0972d3` | Focus rings on Cloudscape components, selected item borders |
| Selected background | `#e9f3ff` | Selected row/item highlight |
| Hover background | `#f5f5f5` | Row/item hover state |
| Drop target green | `#34a853` | Drag-and-drop target border |
| Drop target glow | `rgba(52, 168, 83, 0.2)` | Drag-and-drop target background glow |
| Error | `#dc2626` | Validation errors, destructive actions, alerts |
| Success | `#16a34a` | Success toasts, confirmed states, completed status |

### Input Focus

| Token | Value | Role |
|-------|-------|------|
| Input focus ring | `rgba(74, 121, 48, 0.1)` | Subtle green-tinted focus glow on custom inputs |

---

## 3. Typography Rules

| Property | Value |
|----------|-------|
| Font family | `Inter, Segoe UI, -apple-system, BlinkMacSystemFont, sans-serif` |
| CSS variable | `--font-family` |

### Type Scale

| Element | Size | Weight | Color | Notes |
|---------|------|--------|-------|-------|
| Page title / H1 | 24px | 700 | `--text-color` | Sparse — one per view |
| Section heading / H2 | 18px | 600 | `--text-color` | Card titles, panel headers |
| Subsection / H3 | 16px | 600 | `--text-color` | Table group headers |
| Body text | 14px | 400 | `--text-color` | Default for all content |
| Secondary text | 14px | 400 | `--muted-color` | Timestamps, helper text, metadata |
| Small / Caption | 12px | 400 | `--muted-color` | Table footers, badges, fine print |
| Button label | 14px | 600 | Varies | Primary: white. Secondary: `--accent-color` |
| Monospace / IDs | 13px | 400 | `--text-color` | `font-family: monospace` for order IDs, codes |

### Rules

- Inter is the only brand font. System fallbacks for environments where Inter isn't loaded.
- Cloudscape components inherit `--font-family` via CSS override.
- No italic usage in the UI. Use weight or color to create hierarchy.
- Line height: 1.5 for body, 1.3 for headings.

---

## 4. Component Stylings

### Buttons

| Variant | Background | Border | Text | Radius | Weight | Hover |
|---------|-----------|--------|------|--------|--------|-------|
| Primary | `--accent-color` | none | `#ffffff` | 8px | 600 | `--accent-dark` bg |
| Secondary | transparent | 1px solid `--accent-color` | `--accent-color` | 8px | 600 | `rgba(27, 153, 139, 0.08)` bg |
| Destructive | `#dc2626` | none | `#ffffff` | 8px | 600 | `#b91c1c` bg |
| Disabled | Any variant | — | — | — | — | `opacity: 0.5`, `cursor: not-allowed` |

### Cards

| Property | Value |
|----------|-------|
| Background | `--surface-color` |
| Border | 1px solid `--border-color` |
| Border radius | 12px |
| Padding | 20px–24px |
| Hover shadow | `--shadow-soft` (`0 20px 40px rgba(16, 42, 67, 0.08)`) |
| Resting shadow | none |

### Inputs

| Property | Value |
|----------|-------|
| Background | `--surface-color` |
| Border | 1px solid `--border-color` |
| Border radius | 8px |
| Focus | Border `--accent-color`, box-shadow `0 0 0 3px rgba(74, 121, 48, 0.1)` |
| Error | Border `#dc2626` |

### Tables (Cloudscape)

Tables are the primary data display. Cloudscape `<Table>` component with custom overrides:

| State | Background | Border |
|-------|-----------|--------|
| Default row | `--surface-color` | bottom 1px `--border-color` |
| Hover row | `#f5f5f5` | — |
| Selected row | `#e9f3ff` | left 2px `#0972d3` |

### Login Card

| Property | Value |
|----------|-------|
| Max width | 440px |
| Padding | 3rem (48px) |
| Border radius | 16px |
| Background | `--surface-color` |
| Page background | `linear-gradient(135deg, #1e4620 0%, #2d5a2e 100%)` |

### Drag & Drop Targets

| State | Border | Background |
|-------|--------|-----------|
| Idle | 2px dashed `--border-color` | transparent |
| Active / dragover | 2px solid `#34a853` | `rgba(52, 168, 83, 0.2)` |

### Status Badges

| Status | Background | Text |
|--------|-----------|------|
| Success / Complete | `rgba(22, 163, 74, 0.1)` | `#16a34a` |
| Error / Overdue | `rgba(220, 38, 38, 0.1)` | `#dc2626` |
| Pending / Info | `rgba(9, 114, 211, 0.1)` | `#0972d3` |
| Neutral | `rgba(103, 115, 137, 0.1)` | `--muted-color` |

### Navigation

Sidebar or top-nav powered by Cloudscape `<SideNavigation>`. Active item uses `--accent-color` indicator. Text inherits `--text-color` / `--muted-color` hierarchy.

---

## 5. Layout Principles

| Property | Value |
|----------|-------|
| Max content width | 1800px (`--max-width`) |
| Page padding | 24px horizontal, 20px vertical |
| Card gap | 16px |
| Section gap | 24px |
| Component internal padding | 16px–24px |

### Spacing Scale

| Token | Value | Usage |
|-------|-------|-------|
| xs | 4px | Inline icon gaps, badge padding |
| sm | 8px | Button padding-x, input padding, tight groups |
| md | 16px | Card gap, form field spacing, card padding |
| lg | 24px | Section spacing, page padding |
| xl | 32px | Major section breaks |
| 2xl | 48px | Login card padding, hero spacing |

### Layout Patterns

- **Master-detail (dual-pane)**: Left list panel (40–50% width) + right detail panel. Used for inventory browsing, order management.
- **Full-width table**: Single Cloudscape table spanning the content area. Used for billing, scheduling lists.
- **Calendar view**: Grid-based calendar for scheduling pickups/returns. Cells are interactive.
- **Modal overlays**: Cloudscape `<Modal>` for create/edit forms. Centered, max-width 600px on desktop.

### Grid

No rigid column grid. Layout is flex/CSS-grid driven, adapting to content:
- Sidebar (fixed 240px) + main content (fluid).
- Dual-pane splits use `1fr 1fr` or `2fr 3fr` depending on context.

---

## 6. Depth & Elevation

| Level | Shadow | Usage |
|-------|--------|-------|
| 0 — Flat | none | Default cards, table rows, inputs |
| 1 — Soft hover | `0 20px 40px rgba(16, 42, 67, 0.08)` | Card hover, elevated panels (`--shadow-soft`) |
| 2 — Modal | `0 24px 48px rgba(16, 42, 67, 0.16)` | Modals, dropdowns, popovers |
| 3 — Toast | `0 8px 24px rgba(16, 42, 67, 0.12)` | Flash messages, notification toasts |

### Surface Hierarchy

```
Background (#f3f7f8)
  └─ Surface (#ffffff) — cards, panels
       └─ Elevated surface — modals, popovers (white + Level 2 shadow)
            └─ Overlay — backdrop rgba(0, 0, 0, 0.5)
```

### Rules

- Borders do the heavy lifting at rest. Shadows appear on interaction (hover, focus, open).
- No colored shadows. All shadows use `rgba(16, 42, 67, ...)` — a desaturated navy.
- Cloudscape components bring their own elevation for dropdowns/selects — don't fight them.

---

## 7. Do's and Don'ts

### Do

- ✅ Use Cloudscape components as the foundation — `<Table>`, `<Modal>`, `<FormField>`, `<SideNavigation>`, `<Button>`, `<Select>`, etc.
- ✅ Override Cloudscape CSS variables for brand colors (`--accent-color` teal, not Cloudscape default blue) where appropriate.
- ✅ Keep data density high. Staff process hundreds of items daily — show more, scroll less.
- ✅ Use `--muted-color` for secondary information to create clear visual hierarchy.
- ✅ Use the teal accent sparingly — primary actions, active states, key indicators only.
- ✅ Maintain 8px radius on interactive elements (buttons, inputs) and 12px on containers (cards).
- ✅ Provide clear focus indicators — Cloudscape's `#0972d3` focus ring is correct.
- ✅ Use status badges with semantic color backgrounds for at-a-glance scanning.

### Don't

- ❌ Don't introduce a dark mode. This is a light-theme-only operational tool.
- ❌ Don't use the login gradient green (`#1e4620`) anywhere except the login page.
- ❌ Don't add decorative illustrations, mascots, or marketing-style hero sections.
- ❌ Don't override Cloudscape's focus management or keyboard navigation.
- ❌ Don't use shadows at rest — they appear on hover/interaction only.
- ❌ Don't mix border-radius values. 8px for small elements, 12px for cards, 16px for login card only.
- ❌ Don't use color alone to convey status — pair with text labels or icons.
- ❌ Don't reduce font size below 12px. Staff use this all day — readability matters.
- ❌ Don't fight Cloudscape's internal spacing. Override colors and borders, not padding/margins inside components.

---

## 8. Responsive Behavior

| Breakpoint | Behavior |
|------------|----------|
| ≥ 1200px | Full layout: sidebar + dual-pane master-detail. Max width 1800px centered. |
| 768px–1199px | Sidebar collapses to icon-only or hamburger. Dual-pane stacks to single column. |
| < 768px | Single column. Modals go full-screen (mobile modal fix). Tables scroll horizontally. |

### Rules

- **Max width**: Content never exceeds `1800px`. Centered with auto margins on ultrawide screens.
- **Modals at ≤ 768px**: Full-viewport takeover. No side margins. Close button always visible top-right.
- **Tables**: Horizontal scroll with sticky first column on narrow viewports.
- **Touch targets**: Minimum 44×44px tap area on mobile for buttons and interactive elements.
- **Sidebar**: 240px fixed on desktop. Collapsible on tablet. Hidden behind hamburger on mobile.
- **Calendar views**: Switch from grid to list layout below 768px.
- **Drag-and-drop**: Disabled on touch devices. Provide tap-based alternatives for container management.

---

## 9. Agent Prompt Guide

### Quick Color Reference

```
Background:    #f3f7f8
Surface:       #ffffff
Accent (teal): #1b998b
Accent dark:   #147c70
Text:          #163149
Muted:         #677389
Border:        rgba(22, 49, 73, 0.08)
Focus blue:    #0972d3
Error red:     #dc2626
Success green: #16a34a
Drop green:    #34a853
Selected bg:   #e9f3ff
Hover bg:      #f5f5f5
Login green:   linear-gradient(135deg, #1e4620, #2d5a2e)
```

### Ready-to-Use Prompts

**"Build a new data table view"**
> Use Cloudscape `<Table>` with `--surface-color` background. Rows have bottom `--border-color` border. Hover state `#f5f5f5`. Selected state `#e9f3ff` bg with `#0972d3` left border. Header row uses `--text-color` at 600 weight, 12px uppercase. Wrap in a card with 12px radius.

**"Build a form modal"**
> Use Cloudscape `<Modal>` max-width 600px. Form fields use Cloudscape `<FormField>` + `<Input>`. Primary submit button: `--accent-color` bg, white text, 8px radius, 600 weight. Cancel button: secondary variant. Full-screen on viewports ≤ 768px.

**"Build a status dashboard card"**
> White card, 12px radius, 1px `--border-color` border, 24px padding. Title in 18px/600 `--text-color`. Metric value in 32px/700 `--accent-color`. Secondary text in 14px/400 `--muted-color`. Hover adds `--shadow-soft`.

**"Build a master-detail layout"**
> Two-pane flex layout. Left panel 40% width with scrollable list. Right panel 60% with detail view. Both panels are white cards with 12px radius. List items: 14px body, `--muted-color` metadata. Selected item: `#e9f3ff` bg, `#0972d3` left border. Stacks to single column below 1200px.

**"Build a drag-and-drop container grid"**
> Grid of cards representing storage containers. Idle drop zone: 2px dashed `--border-color`. Active drop zone: 2px solid `#34a853`, `rgba(52, 168, 83, 0.2)` background glow. Cards use 12px radius, `--shadow-soft` on drag. Disable drag on touch — provide tap alternative.
