# DESIGN.md — Stowzilla Ops (Android)

> Native Android operations app for warehouse staff. Kotlin · Material Design 3 (Material You) · Jetpack Compose. A power tool for trained users — complex workflows are acceptable; speed and clarity under pressure are paramount.

---

## 1. Visual Theme & Atmosphere

| Attribute | Value |
|-----------|-------|
| Mood | Industrial-functional, high-trust, task-driven |
| Density | High — warehouse staff need maximum information per screen |
| Design philosophy | Material You foundations with ops-grade density; every pixel earns its place |
| Platform | Android-native, `Theme.Material3.DayNight.NoActionBar` |
| Motion | Compose shared-element transitions, spring-based animations; keep durations ≤300 ms so scanning workflows feel instant |
| Iconography | Material Symbols Rounded, weight 400, optical size 24 dp; filled variants for active nav items |

The app lives on ruggedized handhelds and warehouse-floor phones. Assume bright overhead lighting (light theme default) and occasional dim-dock conditions (dark theme auto-switch). Touch targets must survive gloved hands.

**Identity signal:** Cool blue primary palette — immediately distinguishable from the warm green/cream/orange customer apps. The temperature shift is the cue: cool = ops, warm = customer.

---

## 2. Color Palette & Roles

### Light Theme

| Token | Hex | Role |
|-------|-----|------|
| `colorPrimary` | `#1976D2` | Primary actions, FABs, top app bar tint |
| `colorPrimaryVariant` | `#1565C0` | Pressed/focused state of primary elements |
| `colorOnPrimary` | `#FFFFFF` | Text/icons on primary surfaces |
| `colorPrimaryContainer` | `#BBDEFB` | Tonal container behind primary content (chips, badges) |
| `colorOnPrimaryContainer` | `#0D47A1` | Text/icons on primary container |
| `colorSecondary` | `#43A047` | Success actions, confirmations, "complete" affordances |
| `colorSecondaryVariant` | `#388E3C` | Pressed/focused state of secondary elements |
| `colorOnSecondary` | `#FFFFFF` | Text/icons on secondary surfaces |
| `colorTertiary` | `#FB8C00` | Warnings, attention-required badges, urgent counts |
| `colorOnTertiary` | `#FFFFFF` | Text/icons on tertiary surfaces |
| `colorBackground` | `#FAFAFA` | App canvas |
| `colorOnBackground` | `#212121` | Primary text on background |
| `colorSurface` | `#FFFFFF` | Cards, sheets, dialogs |
| `colorOnSurface` | `#212121` | Primary text on surfaces |
| `colorSurfaceVariant` | `#E0E0E0` | Dividers, disabled fills, secondary containers |
| `colorOnSurfaceVariant` | `#616161` | Secondary/caption text |

### Dark Theme

| Token | Hex | Role |
|-------|-----|------|
| `colorPrimary` | `#64B5F6` | Primary actions |
| `colorPrimaryVariant` | `#42A5F5` | Pressed/focused primary |
| `colorOnPrimary` | `#000000` | Text/icons on primary |
| `colorPrimaryContainer` | `#0D47A1` | Tonal container |
| `colorOnPrimaryContainer` | `#BBDEFB` | Text on primary container |
| `colorSecondary` | `#66BB6A` | Success actions |
| `colorSecondaryVariant` | `#4CAF50` | Pressed/focused secondary |
| `colorOnSecondary` | `#000000` | Text/icons on secondary |
| `colorTertiary` | `#FFB74D` | Warnings, attention badges |
| `colorOnTertiary` | `#000000` | Text/icons on tertiary |
| `colorBackground` | `#121212` | App canvas |
| `colorOnBackground` | `#E0E0E0` | Primary text |
| `colorSurface` | `#1E1E1E` | Cards, sheets, dialogs |
| `colorOnSurface` | `#E0E0E0` | Primary text on surfaces |
| `colorSurfaceVariant` | `#424242` | Dividers, disabled fills |
| `colorOnSurfaceVariant` | `#BDBDBD` | Secondary/caption text |

### Status Colors (theme-independent)

| Token | Hex | Meaning |
|-------|-----|---------|
| `status_pending` | `#9E9E9E` | Awaiting action — grey neutral |
| `status_approved` | `#2196F3` | Approved / ready to proceed |
| `status_on_the_way` | `#FF9800` | In transit — amber urgency |
| `status_processing` | `#9C27B0` | Actively being worked — purple |
| `status_complete` | `#4CAF50` | Done — green confirmation |
| `status_cancelled` | `#F44336` | Cancelled / error — red alert |
| `status_unassigned` | `#E65100` | Needs assignment — deep orange |

Status colors are used as-is in both themes. Pair with white (`#FFFFFF`) text/icons when used as chip/badge backgrounds. In outline-only contexts, use the status color for stroke and text on the current surface.

### Environment Indicators

Non-production builds display a persistent top-edge banner (32dp tall, full width, above the top app bar) so staff always know which environment they are using. Production has no banner — the absence of a banner is the signal.

| Environment | Banner Background | Banner Text | Text Color |
|-------------|------------------|-------------|------------|
| Production | *none — no banner* | — | — |
| UAT | `#F59E0B` (amber) | `UAT` | `#78350F` |
| Dev | `#8B5CF6` (violet) | `DEV` | `#FFFFFF` |

Rules:
- Banner uses `labelMedium` typography, centered, uppercase, letter-spacing 2sp
- Injected via `BuildConfig` field set by the build variant, never hardcoded as visible in release builds
- Banner is non-scrollable — sits above the top app bar in the root `Scaffold`

---

## 3. Typography Rules

| Style | Font | Weight | Size | Line Height | Tracking | Usage |
|-------|------|--------|------|-------------|----------|-------|
| Display Large | Roboto | 400 | 57 sp | 64 sp | −0.25 sp | — reserved, not used in-app |
| Display Medium | Roboto | 400 | 45 sp | 52 sp | 0 sp | — reserved |
| Display Small | Roboto | 400 | 36 sp | 44 sp | 0 sp | — reserved |
| Headline Large | Roboto | 400 | 32 sp | 40 sp | 0 sp | Screen titles (rare) |
| Headline Medium | Roboto | 400 | 28 sp | 36 sp | 0 sp | Section headers |
| Headline Small | Roboto | 400 | 24 sp | 32 sp | 0 sp | Card titles |
| Title Large | Roboto Medium | 500 | 22 sp | 28 sp | 0 sp | Top app bar title |
| Title Medium | Roboto Medium | 500 | 16 sp | 24 sp | 0.15 sp | List item primary text |
| Title Small | Roboto Medium | 500 | 14 sp | 20 sp | 0.1 sp | Tab labels, chip text |
| Body Large | Roboto | 400 | 16 sp | 24 sp | 0.5 sp | Primary body text |
| Body Medium | Roboto | 400 | 14 sp | 20 sp | 0.25 sp | Default body, form fields |
| Body Small | Roboto | 400 | 12 sp | 16 sp | 0.4 sp | Captions, timestamps |
| Label Large | Roboto Medium | 500 | 14 sp | 20 sp | 0.1 sp | Button text |
| Label Medium | Roboto Medium | 500 | 12 sp | 16 sp | 0.5 sp | Badges, small labels |
| Label Small | Roboto Medium | 500 | 11 sp | 16 sp | 0.5 sp | Overlines, micro-labels |

Font stack: `Roboto` (system default on Android). No custom fonts — keeps APK lean and respects system accessibility scaling.

---

## 4. Component Stylings

### Buttons

| Variant | Background | Text | Corner | Height | Usage |
|---------|-----------|------|--------|--------|-------|
| Filled (primary) | `colorPrimary` | `colorOnPrimary` | 12 dp (full-rounded pill) | 48 dp min | Primary CTA — "Scan", "Confirm", "Assign" |
| Filled (secondary) | `colorSecondary` | `colorOnSecondary` | 12 dp | 48 dp min | Success actions — "Complete", "Approve" |
| Filled (tertiary) | `colorTertiary` | `colorOnTertiary` | 12 dp | 48 dp min | Warning actions — "Flag", "Escalate" |
| Outlined | transparent, 1 dp `colorPrimary` stroke | `colorPrimary` | 12 dp | 48 dp min | Secondary actions — "Cancel", "Back" |
| Text | transparent | `colorPrimary` | 12 dp | 40 dp min | Tertiary actions — "Skip", "Details" |
| FAB | `colorPrimaryContainer` | `colorOnPrimaryContainer` | 16 dp | 56 dp | Floating scan trigger |

States: `pressed` → 12% `colorOnSurface` overlay · `focused` → 12% `colorPrimary` overlay · `disabled` → 38% opacity on content, 12% `colorOnSurface` fill.

Minimum touch target: **48 × 48 dp** on all interactive elements (gloved-hand requirement).

### Cards

| Variant | Background | Elevation | Corner | Padding | Usage |
|---------|-----------|-----------|--------|---------|-------|
| Filled | `colorSurface` | Level 1 (1 dp) | 12 dp | 16 dp | Order cards, container cards |
| Outlined | `colorSurface`, 1 dp `colorSurfaceVariant` stroke | Level 0 | 12 dp | 16 dp | List items, detail sections |

Cards display a status indicator — a 4 dp-wide vertical bar on the leading edge using the appropriate `status_*` color.

### Status Chips

| State | Background | Text | Border |
|-------|-----------|------|--------|
| Default | status color at 15% opacity | status color | none |
| Emphasized | status color at 100% | `#FFFFFF` | none |

Corner radius: 8 dp. Height: 32 dp. Use `Label Medium` typography.

### Bottom Navigation Bar

| Attribute | Value |
|-----------|-------|
| Background | `colorSurface` |
| Elevation | Level 2 (3 dp) |
| Active icon | `colorPrimary`, filled variant |
| Active label | `colorPrimary`, `Label Medium` |
| Inactive icon | `colorOnSurfaceVariant`, outlined variant |
| Inactive label | `colorOnSurfaceVariant`, `Label Medium` |
| Item count | 4–5 max |
| Indicator | pill-shaped `colorPrimaryContainer` behind active icon |

### Text Fields

| Attribute | Value |
|-----------|-------|
| Variant | Outlined (`OutlinedTextField`) |
| Corner | 8 dp top, 8 dp bottom |
| Border idle | 1 dp `colorSurfaceVariant` |
| Border focused | 2 dp `colorPrimary` |
| Border error | 2 dp `status_cancelled` (`#F44336`) |
| Label | `Body Small`, `colorOnSurfaceVariant` |
| Input text | `Body Medium`, `colorOnSurface` |
| Helper/error text | `Body Small` |
| Height | 56 dp |

### Dialogs & Bottom Sheets

| Attribute | Value |
|-----------|-------|
| Surface | `colorSurface` |
| Corner | 28 dp (dialog), 28 dp top (bottom sheet) |
| Scrim | `#000000` at 32% opacity |
| Elevation | Level 3 (6 dp) |
| Title | `Headline Small` |
| Body | `Body Medium` |
| Actions | right-aligned, `Text` or `Filled` buttons |

### QR / Barcode Scanner Overlay

| Attribute | Value |
|-----------|-------|
| Viewfinder | 240 × 240 dp rounded rect, 16 dp corner, 3 dp `colorPrimary` stroke |
| Background scrim | `#000000` at 60% outside viewfinder |
| Instruction text | `Body Large`, `#FFFFFF`, centered below viewfinder |
| Flash toggle | `IconButton` top-right, `#FFFFFF` icon |

### Drag-and-Drop Surfaces

| State | Visual |
|-------|--------|
| Idle | standard card styling |
| Dragging | elevation → Level 3, 8% `colorPrimary` overlay, slight scale (1.02×) |
| Drop target active | 2 dp dashed `colorPrimary` border, `colorPrimaryContainer` at 20% fill |
| Drop complete | brief `colorSecondary` flash (150 ms) |

---

## 5. Layout Principles

### Spacing Scale

| Token | Value | Usage |
|-------|-------|-------|
| `space-xs` | 4 dp | Inline icon-to-text gap |
| `space-sm` | 8 dp | Tight list item internal padding |
| `space-md` | 16 dp | Standard card padding, section gaps |
| `space-lg` | 24 dp | Between major sections |
| `space-xl` | 32 dp | Screen-level top/bottom padding |

### Grid & Layout

- Single-column layout on phones (default). Two-column on tablets (≥600 dp width).
- Content margins: 16 dp horizontal on compact, 24 dp on medium/expanded.
- `LazyColumn` for all scrollable lists — mandatory for warehouse-scale data sets.
- Bottom navigation is persistent; content scrolls behind it with appropriate `contentPadding`.

### Whitespace Philosophy

Dense but not cramped. Warehouse staff scan lists rapidly — use consistent 8 dp vertical gaps between list items and 16 dp between sections. Never add decorative whitespace; every gap must create a scannable rhythm.

---

## 6. Depth & Elevation

Material 3 tonal elevation system. Shadow + tonal surface color shift in dark theme.

| Level | Elevation | Shadow | Usage |
|-------|-----------|--------|-------|
| 0 | 0 dp | none | Flat backgrounds, outlined cards |
| 1 | 1 dp | subtle | Filled cards, list items |
| 2 | 3 dp | medium | Bottom nav bar, top app bar (scrolled) |
| 3 | 6 dp | pronounced | Dialogs, bottom sheets, dragged items |
| 4 | 8 dp | strong | Menus, autocomplete popups |
| 5 | 12 dp | maximum | — reserved, not used |

In dark theme, higher elevation = lighter tonal surface (Material 3 automatic tonal elevation). Do not manually lighten surfaces — let `Surface` composable handle it.

---

## 7. Do's and Don'ts

### Do

- Use status colors consistently — the same hex always means the same thing across every screen.
- Maintain 48 dp minimum touch targets everywhere. Warehouse gloves are real.
- Show loading states (shimmer or `CircularProgressIndicator`) for any network call >300 ms.
- Use `Snackbar` for transient confirmations ("Container moved", "Label printed").
- Provide haptic feedback (`HapticFeedbackType.LongPress`) on successful scans and drag-drop completions.
- Support both camera-based and hardware scanner input for QR/barcode fields.
- Respect system font scaling up to 200% — layouts must not break.
- Use `colorPrimary` for the single most important action on each screen.

### Don't

- Don't use status colors for decoration — they are semantic, not aesthetic.
- Don't put destructive actions (cancel order, delete container) in easy-tap zones. Require confirmation dialogs.
- Don't use Display typography styles — screens are too task-dense for display type.
- Don't rely on color alone to convey status — always pair with an icon or text label.
- Don't auto-dismiss critical error messages. Use dialogs, not snackbars, for errors that block workflow.
- Don't disable the back gesture or system navigation — warehouse staff switch apps constantly.
- Don't use custom splash screens — rely on the Android 12+ `SplashScreen` API with `colorPrimary` background and app icon.
- Don't nest scrollable containers — one `LazyColumn` per screen maximum.

---

## 8. Responsive Behavior

### Window Size Classes (Material 3)

| Class | Width | Layout | Nav |
|-------|-------|--------|-----|
| Compact | < 600 dp | Single column | Bottom navigation bar |
| Medium | 600–839 dp | Two columns where beneficial (list-detail) | Navigation rail |
| Expanded | ≥ 840 dp | Full list-detail, side panels | Navigation rail + persistent detail pane |

### Breakpoint Behaviors

- Bottom nav collapses to navigation rail at 600 dp.
- Order list → order detail becomes side-by-side at 600 dp.
- Camera/scanner views always remain full-width regardless of window class.
- Dialogs become full-screen on compact, remain centered on medium/expanded.

### Touch Targets

| Context | Minimum Size |
|---------|-------------|
| Standard interactive | 48 × 48 dp |
| Dense list actions (trained users) | 40 × 40 dp with 48 dp row height |
| FAB | 56 × 56 dp |
| Bottom nav item | 48 dp height, equal-width distribution |

### Orientation

- Portrait is primary. Landscape supported but not optimized — scanner overlay and lists reflow naturally.
- Lock to portrait for camera-intake screens to avoid disorienting rotation mid-capture.

### Accessibility

- Minimum contrast ratio: 4.5:1 for body text, 3:1 for large text (all token pairings above meet this).
- All status chips include icon + text, never color alone.
- `contentDescription` required on all icons and status indicators.
- Support TalkBack navigation with logical focus order: top bar → content → bottom nav.

---

## 9. Agent Prompt Guide

### Quick Color Reference

```
Primary Blue:    Light #1976D2 / Dark #64B5F6
Success Green:   Light #43A047 / Dark #66BB6A
Warning Orange:  Light #FB8C00 / Dark #FFB74D
Background:      Light #FAFAFA / Dark #121212
Surface:         Light #FFFFFF / Dark #1E1E1E
On-Surface:      Light #212121 / Dark #E0E0E0

Status: pending=#9E9E9E approved=#2196F3 on_the_way=#FF9800
        processing=#9C27B0 complete=#4CAF50 cancelled=#F44336
        unassigned=#E65100
```

### Ready-to-Use Prompts

**Scaffold a new screen:**
> Build a Jetpack Compose screen for [feature]. Use `Theme.Material3.DayNight.NoActionBar` theming. Surface background is `MaterialTheme.colorScheme.background`. Cards use `MaterialTheme.colorScheme.surface` with 12 dp rounded corners and Level 1 elevation. Primary action button is filled with `colorPrimary`. Bottom navigation bar with 4 tabs. Follow the spacing scale: 4/8/16/24/32 dp.

**Add a status-aware list:**
> Create a `LazyColumn` of order cards. Each card has a 4 dp vertical leading-edge bar colored by status: pending=#9E9E9E, approved=#2196F3, on_the_way=#FF9800, processing=#9C27B0, complete=#4CAF50, cancelled=#F44336, unassigned=#E65100. Card padding 16 dp, corner radius 12 dp, 8 dp gap between items. Title in `titleMedium`, subtitle in `bodySmall` with `colorOnSurfaceVariant`.

**Build a scanner overlay:**
> Create a camera preview composable with a centered 240×240 dp viewfinder. Viewfinder has 16 dp rounded corners and 3 dp `colorPrimary` stroke. Area outside viewfinder is scrimmed at 60% black. Instruction text below viewfinder in `bodyLarge`, white. Flash toggle icon button top-right corner, white.

**Create a drag-and-drop container grid:**
> Build a reorderable grid of container cards using Compose drag-and-drop. Idle state: standard filled card. Dragging: elevate to Level 3, apply 1.02× scale and 8% `colorPrimary` overlay. Drop target: 2 dp dashed `colorPrimary` border with 20% `colorPrimaryContainer` fill. On drop: flash `colorSecondary` for 150 ms. Minimum touch target 48×48 dp.

**Bluetooth print confirmation:**
> Show a `Snackbar` with "Label printed successfully" using `colorSecondary` as the container color and `colorOnSecondary` for text. If printing fails, show a dialog (not snackbar) with error details, `Headline Small` title, `Body Medium` message, and a filled primary "Retry" button.
