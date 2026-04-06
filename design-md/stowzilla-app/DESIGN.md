# DESIGN.md — Stowzilla Customer App

> Consumer marketplace for valet storage. React + Vite + AWS Cloudscape Design + Inter font.

---

## 1. Visual Theme & Atmosphere

| Attribute | Value |
|-----------|-------|
| Mood | Warm, friendly, trustworthy — like handing your belongings to a neighbor you trust |
| Density | Spacious. Generous whitespace, breathing room around every element |
| Design philosophy | **Invisible simplicity** — zero cognitive load, no feature discovery required |
| Palette inspiration | Nature-inspired: forest greens, golden yellows, warm oranges against a cream canvas |
| Surface feel | Soft, paper-like warmth. Never cold or clinical |
| Imagery | Photography-driven for inventory items. Real objects, natural light, no stock-photo sterility |
| Motion | Subtle and purposeful. Cubic-bezier easing on transitions, never bouncy or playful |
| Overall | The app should feel effortless — a customer should be able to schedule a pickup without thinking |

---

## 2. Color Palette & Roles

### Core Palette

| Token | Hex | Role |
|-------|-----|------|
| `--background-color` | `#FFFDF5` | Page canvas. Warm cream — never pure white |
| `--surface-color` | `#FFFFFF` | Cards, modals, header. Clean white against the cream canvas |
| `--accent-color` | `#4A7930` | Primary interactive: buttons, links, active states. Forest green |
| `--accent-dark` | `#5A9642` | Accent hover/emphasis. Slightly brighter green |
| `--accent-light` | `#A4D17C` | Accent backgrounds, tags, soft highlights |
| `--secondary-color` | `#F7B32B` | Attention/highlight: badges, promotions, star ratings. Golden yellow |
| `--secondary-dark` | `#E09B0D` | Secondary hover state |
| `--tertiary-color` | `#FF8C42` | Warm orange. Alerts, urgency indicators, promotional callouts |
| `--teal-accent` | `#2D9B9B` | Informational: tooltips, help text, secondary links |

### Text & UI Chrome

| Token | Hex | Role |
|-------|-----|------|
| `--text-color` | `#1F2937` | Primary body text. Near-black with warmth |
| `--muted-color` | `#6B7280` | Secondary text, captions, placeholders |
| `--cta-color` | `#0B2C4A` | Deep navy. High-commitment CTAs: "Schedule Pickup", "Confirm Order" |
| `--cta-text` | `#FFFFFF` | Text on CTA buttons |
| `--cta-hover` | `#093040` | CTA hover state |
| `--border-color` | `rgba(31, 41, 55, 0.1)` | Subtle dividers and card borders |

### Semantic States

| State | Foreground | Background |
|-------|-----------|------------|
| Error | `#DC2626` | `rgba(220, 38, 38, 0.1)` |
| Success | `#16A34A` | `rgba(34, 197, 94, 0.1)` |

---

## 3. Typography Rules

| Attribute | Value |
|-----------|-------|
| Font family | `Inter, Segoe UI, -apple-system, BlinkMacSystemFont, sans-serif` |
| Variable | `--font-family` |
| Rendering | `-webkit-font-smoothing: antialiased` |

### Type Scale

| Level | Size | Weight | Use |
|-------|------|--------|-----|
| Page title | `1.75rem` | 700 | Page headings: "My Items", "Schedule Pickup" |
| Section heading | `1.25rem` | 600 | Card group titles, section labels |
| Card title | `1rem` | 600 | Item names, card headers |
| Body | `0.9375rem` (15px) | 400 | Paragraphs, descriptions, nav links |
| Nav link | `0.9375rem` | 500 | Header navigation items |
| Caption | `0.8125rem` | 400 | Timestamps, metadata, helper text |
| Small / Badge | `0.75rem` | 600 | Status badges, calendar labels |
| Button | `0.875rem` | 600 | All button text |

### Rules

- Line height: `1.5` for body, `1.2` for headings
- Letter spacing: default (Inter handles this well at all sizes)
- Never use font weights below 400 or above 700
- Muted text uses `--muted-color` (`#6B7280`), never opacity tricks

---

## 4. Component Stylings

### Buttons

**Primary (accent)**
```
background: var(--accent-color)        /* #4A7930 */
color: #FFFFFF
border-radius: 8px
font-weight: 600
font-size: 0.875rem
padding: 10px 20px
border: none
transition: background 0.2s ease
hover: var(--accent-dark)              /* #5A9642 */
```

**CTA (high-commitment)**
```
background: var(--cta-color)           /* #0B2C4A */
color: var(--cta-text)                 /* #FFFFFF */
border-radius: 8px
font-weight: 600
hover: var(--cta-hover)                /* #093040 */
```

**Secondary / outline**
```
background: transparent
color: var(--accent-color)
border: 1px solid var(--accent-color)
border-radius: 8px
hover-background: rgba(74, 121, 48, 0.1)
```

### Cards

```
background: var(--surface-color)       /* #FFFFFF */
border: 1px solid var(--border-color)  /* rgba(31, 41, 55, 0.1) */
border-radius: 12px
padding: 16px–24px
transition: box-shadow 0.2s ease
hover: var(--shadow-soft)              /* 0 20px 40px rgba(31, 41, 55, 0.08) */
```

- Item cards are photography-driven: image fills top portion, details below
- No card has a colored background — always white on cream

### Header / Navigation

```
height: 80px
background: var(--surface-color)       /* #FFFFFF */
box-shadow: var(--shadow-soft)
position: sticky (mobile)
z-index: 1000
```

**Nav links:**
```
font-size: 0.9375rem
font-weight: 500
border-radius: 6px
padding: 8px 12px
hover-background: rgba(74, 121, 48, 0.1)
```

**Active nav link:**
```
color: var(--accent-color)             /* #4A7930 */
border-bottom: 3px solid var(--accent-color)
```

### Calendar Badge

```
background: var(--accent-color)        /* #4A7930 */
color: #FFFFFF
border-radius: 9px
font-size: 0.75rem
font-weight: 600
padding: 2px 8px
```

### Form Inputs

- Uses AWS Cloudscape Design input components as base
- Override focus ring to `var(--accent-color)` with `0 0 0 2px rgba(74, 121, 48, 0.25)`
- Error state: border `#DC2626`, background `rgba(220, 38, 38, 0.1)`
- Success state: border `#16A34A`, background `rgba(34, 197, 94, 0.1)`
- Border radius: `8px` to match button system

### Status Badges

| Status | Background | Text | Border |
|--------|-----------|------|--------|
| Stored | `rgba(34, 197, 94, 0.1)` | `#16A34A` | none |
| Pending | `rgba(247, 179, 43, 0.15)` | `#E09B0D` | none |
| In Transit | `rgba(45, 155, 155, 0.1)` | `#2D9B9B` | none |
| Error | `rgba(220, 38, 38, 0.1)` | `#DC2626` | none |

---

## 5. Layout Principles

| Property | Value |
|----------|-------|
| Max width | `1800px` (`--max-width`) |
| Page padding | `24px` horizontal, `32px` vertical |
| Card gap | `16px` (grid), `24px` (section spacing) |
| Section spacing | `48px` between major sections |
| Content centering | `margin: 0 auto` within max-width |

### Spacing Scale

| Token | Value | Use |
|-------|-------|-----|
| `xs` | `4px` | Inline icon gaps, badge padding |
| `sm` | `8px` | Input padding, tight element spacing |
| `md` | `16px` | Card padding, grid gap, standard spacing |
| `lg` | `24px` | Section padding, card internal spacing |
| `xl` | `32px` | Page vertical padding |
| `2xl` | `48px` | Between major page sections |

### Grid

- Item grids: CSS Grid, `repeat(auto-fill, minmax(280px, 1fr))`
- Dashboard widgets: 2–3 column grid on desktop, single column on mobile
- Always left-aligned content. No centered body text

---

## 6. Depth & Elevation

| Level | Shadow | Use |
|-------|--------|-----|
| Flat | none | Default card resting state, inline elements |
| Soft | `0 20px 40px rgba(31, 41, 55, 0.08)` | Header, card hover, floating elements |
| Modal | `0 25px 50px rgba(31, 41, 55, 0.15)` | Modals, mobile drawer overlay |

### Surface Hierarchy

```
z-0  Background (#FFFDF5)     — page canvas
z-1  Surface (#FFFFFF)         — cards, content areas (1px border, no shadow)
z-2  Elevated surface          — card hover state (shadow-soft)
z-3  Header / sticky elements  — shadow-soft, z-index: 1000
z-4  Drawer / modal overlay    — modal shadow, z-index: 1100+
```

- No colored shadows. All shadows use `rgba(31, 41, 55, ...)` — warm gray derived from `--text-color`
- Elevation is communicated through shadow intensity, never background color changes
- Borders (`--border-color`) do the heavy lifting at rest; shadows appear on interaction

---

## 7. Do's and Don'ts

### Do

- Use `--background-color` (`#FFFDF5`) as the page canvas — the warm cream is the brand signature
- Let photography carry the visual weight for inventory items
- Use `--cta-color` (deep navy) only for high-commitment actions: "Schedule Pickup", "Confirm", "Place Order"
- Use `--accent-color` (forest green) for standard interactive elements
- Keep forms short — one column, clear labels, immediate validation
- Use Cloudscape components as the structural base, themed with the custom tokens
- Maintain generous whitespace — when in doubt, add more space

### Don't

- Never use pure white (`#FFFFFF`) as a page background — always `#FFFDF5`
- Never use more than one CTA button per view
- Never stack more than 3 colors in a single component
- Don't use `--tertiary-color` (orange) for buttons — it's for callouts and alerts only
- Don't add decorative borders or dividers — use spacing to separate content
- Don't use dark mode — the warm cream palette is the entire brand identity
- Don't animate layout shifts — only opacity and transform transitions
- Don't use icon-only buttons without accessible labels

---

## 8. Responsive Behavior

### Breakpoints

| Name | Width | Behavior |
|------|-------|----------|
| Desktop | `≥1024px` | Full nav, multi-column grids, side panels |
| Tablet | `768px–1023px` | Hamburger nav, 2-column grids, stacked panels |
| Mobile | `481px–767px` | Single column, full-width cards, bottom-anchored CTAs |
| Compact | `≤480px` | Tighter padding (16px), smaller type scale, compressed cards |

### Navigation

- `≥768px`: Horizontal nav links in header
- `<768px`: Hamburger icon → 280px slide-out drawer from left
  - Drawer transition: `transform 0.3s cubic-bezier(0.4, 0, 0.2, 1)`
  - Overlay: `rgba(0, 0, 0, 0.5)` backdrop

### Touch Targets

- Minimum tap target: `44px × 44px`
- Button min-height: `44px` on mobile
- Nav drawer links: `48px` row height
- Spacing between tappable elements: `≥8px`

### Responsive Adjustments

| Element | Desktop | Mobile |
|---------|---------|--------|
| Page padding | `24px` | `16px` |
| Card grid | `auto-fill, minmax(280px, 1fr)` | Single column |
| Header | Static, `80px` | Sticky, `80px` |
| CTA buttons | Inline | Full-width, bottom-anchored |
| Item images | Aspect-ratio preserved | Full-width, 16:9 crop |

---

## 9. Agent Prompt Guide

### Quick Color Reference

```
Cream background:  #FFFDF5
White surface:     #FFFFFF
Forest green:      #4A7930  (primary accent)
Golden yellow:     #F7B32B  (secondary)
Warm orange:       #FF8C42  (tertiary — alerts only)
Teal:              #2D9B9B  (informational)
Deep navy CTA:     #0B2C4A  (high-commitment actions)
Body text:         #1F2937
Muted text:        #6B7280
Error red:         #DC2626
Success green:     #16A34A
```

### Ready-to-Use Prompts

**"Build a Stowzilla item card"**
> White card on cream background. 12px radius, 1px border `rgba(31,41,55,0.1)`. Photo fills top half. Item name in 1rem/600 weight below. Muted caption for date stored. Forest green "Request Delivery" button, 8px radius. Shadow on hover.

**"Build the Stowzilla header"**
> 80px tall, white background, soft shadow. Logo left. Nav links at 0.9375rem/500 weight with 6px radius hover state `rgba(74,121,48,0.1)`. Active link has forest green 3px bottom border. Hamburger at 768px breakpoint, 280px slide-out drawer.

**"Build a Stowzilla scheduling flow"**
> Single-column form on cream canvas. Cloudscape date picker themed with forest green focus ring. Calendar badges: green bg, white text, 9px radius. One deep navy CTA at the bottom: "Confirm Pickup". Full-width on mobile, bottom-anchored.

**"Build a Stowzilla dashboard"**
> Cream background. 2–3 column grid of white cards. Status badges: green for Stored, yellow for Pending, teal for In Transit. Section headings at 1.25rem/600. Golden yellow highlight for promotional banners. Max-width 1800px, centered.

### Key Principles for Agents

1. **Cream, not white** — the page is always `#FFFDF5`
2. **Green is primary** — `#4A7930` for all standard interactions
3. **Navy is commitment** — `#0B2C4A` only for final-step CTAs
4. **Photography first** — item cards lead with real photos, not icons
5. **One CTA per view** — never compete for attention
6. **Cloudscape base** — use AWS Cloudscape components, override with theme tokens
7. **Mobile-first** — design for 375px width, then expand
