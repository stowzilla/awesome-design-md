# DESIGN.md — StowzillaOps iOS

> Native iOS operations app for warehouse staff. Swift + SwiftUI + TCA v1.10.0 + Nuke 12.6.0. iOS 17+.

---

## 1. Visual Theme & Atmosphere

| Attribute | Value |
|-----------|-------|
| Mood | Industrial-utilitarian, tool-first, zero-vanity |
| Density | High — dense lists, compact cards, multi-action toolbars |
| Design philosophy | "Dr Who's screwdriver" — a power tool for trained users. Complex and feature-heavy is acceptable. Learning curve is fine. |
| Platform | Native iOS, full system integration. Follows Human Interface Guidelines as baseline, then layers operational density on top. |
| Motion | System-default SwiftUI transitions. No custom choreography. Speed over spectacle. |
| Dark mode | Full support via SwiftUI semantic colors. All custom colors must resolve in both appearances. |
| iOS feature adoption | Liquid Glass for visual hierarchy and layered surfaces. App Intents for Siri/Shortcuts integration. Foundation Models framework for on-device AI intake assistance. Visual Intelligence for camera-based item identification. |
| Identity signal | **Cool blue palette** — system blues, no warm tones. Immediately distinguishable from the warm green/cream/orange customer app (Swiftzilla). The temperature shift is the cue: cool = ops, warm = customer. |

The app is not a consumer product. It is a warehouse floor instrument. Every pixel earns its place by helping staff move faster through intake, scanning, labeling, and container management workflows.

---

## 2. Color Palette & Roles

### Brand Accent

| Token | Hex | RGB | Role |
|-------|-----|-----|------|
| `AccentColor` | `#0066CC` | `rgb(0, 102, 204)` | Primary brand accent from Assets.xcassets. App tint, navigation bar tint, default button color. |

### System Semantic Colors

The app uses SwiftUI system colors exclusively for semantic meaning. No custom hex values beyond the accent.

| SwiftUI Color | Hex (Light) | Role |
|---------------|-------------|------|
| `.blue` | `#007AFF` | Interactive elements, links, selected states, tappable text |
| `.green` | `#34C759` | Success, completion, QR scanner overlay, confirmed status |
| `.orange` | `#FF9500` | Warnings, pending states, needs-attention badges |
| `.purple` | `#AF52DE` | Processing status, in-progress indicators |
| `.red` | `#FF3B30` | Errors, destructive actions, delete confirmations |
| `.gray` | `#8E8E93` | Disabled controls, secondary text, background fills |
| `.black` | `#000000` | QR scanner background, full-bleed camera views |
| `.primary` | System | Primary text — adapts light/dark automatically |
| `.secondary` | System | Secondary text — adapts light/dark automatically |

### Opacity Variants

| Token | Usage |
|-------|-------|
| `Color.gray.opacity(0.1)` | Card backgrounds, list row fills |
| `Color.gray.opacity(0.3)` | Placeholder image backgrounds, empty-state fills |
| `Color.blue.opacity(0.2)` | Selected item highlight background |
| `Color.green.opacity(0.2)` | Success state background tint |
| `Color.orange.opacity(0.2)` | Warning state background tint |

### Status Color Map

```swift
enum Status {
    case pending    // .orange + Color.orange.opacity(0.2) bg
    case processing // .purple
    case completed  // .green + Color.green.opacity(0.2) bg
    case error      // .red
    case selected   // .blue + Color.blue.opacity(0.2) bg
}
```

### Environment Indicators

Non-production builds display a persistent capsule badge in the top-right corner of the navigation bar so staff always know which environment they are using. Production builds show nothing — the absence of a badge is the signal.

| Environment | Badge Background | Badge Text | Text Color |
|-------------|-----------------|------------|------------|
| Production | *none — no badge* | — | — |
| UAT | `#F59E0B` (amber) | `UAT` | `#78350F` |
| Dev | `#8B5CF6` (violet) | `DEV` | `#FFFFFF` |

Rules:
- Badge uses `.caption2` font, `.bold` weight, `Capsule()` clip shape, 4pt vertical / 8pt horizontal padding
- Injected via build configuration (`#if DEBUG` / environment variable), never hardcoded as visible in release builds
- Badge is non-interactive and does not interfere with navigation

---

## 3. Typography Rules

| Element | Font | Size | Weight | Notes |
|---------|------|------|--------|-------|
| Large title | SF Pro Display | 34pt | `.bold` | Navigation large titles |
| Title | SF Pro Display | 28pt | `.bold` | Section headers |
| Title 2 | SF Pro Display | 22pt | `.bold` | Card titles, detail view headers |
| Title 3 | SF Pro Display | 20pt | `.semibold` | Sub-section headers |
| Headline | SF Pro Text | 17pt | `.semibold` | List row primary text, button labels |
| Body | SF Pro Text | 17pt | `.regular` | Default readable text |
| Callout | SF Pro Text | 16pt | `.regular` | Supporting descriptions |
| Subheadline | SF Pro Text | 15pt | `.regular` | Secondary list row text, metadata |
| Footnote | SF Pro Text | 13pt | `.regular` | Timestamps, tertiary info |
| Caption | SF Pro Text | 12pt | `.regular` | Badges, status labels |
| Caption 2 | SF Pro Text | 11pt | `.regular` | Fine print, counts |
| Monospaced | SF Mono | 15pt | `.medium` | QR codes, container IDs, reference numbers |

All typography uses the SwiftUI `Font` API (`.title`, `.headline`, `.body`, etc.) — never hardcoded point sizes. This ensures Dynamic Type support out of the box.

### Rules

- Container IDs and reference codes always render in `.monospaced` or `Font.system(.body, design: .monospaced)`.
- Status labels use `.caption` weight `.semibold` with colored background capsules.
- Warehouse staff read at arm's length — minimum touch-target text is `.subheadline` (15pt).

---

## 4. Component Stylings

### Buttons

| Variant | Style | Usage |
|---------|-------|-------|
| Primary action | `.borderedProminent` with `AccentColor` tint | "Start Intake", "Save", "Confirm" |
| Secondary action | `.bordered` | "Cancel", "Edit", "Filter" |
| Destructive | `.bordered` + `.red` tint | "Delete Item", "Remove Photo" |
| Toolbar action | `.plain` with SF Symbol icon | Top-bar actions, contextual menus |
| Floating action | Custom circle, 56pt, shadow, `AccentColor` fill | Camera trigger in photoshoot mode |

```swift
// Primary
Button("Start Intake") { }
    .buttonStyle(.borderedProminent)

// Destructive
Button("Delete", role: .destructive) { }
    .buttonStyle(.bordered)
```

### Cards

```
┌─────────────────────────────────┐
│  Color.gray.opacity(0.1) fill   │
│  12pt corner radius             │
│  No border stroke               │
│  16pt internal padding          │
│                                 │
│  [Thumbnail]  Title (.headline) │
│               Subtitle (.sub…)  │
│               Status badge      │
└─────────────────────────────────┘
```

- Background: `Color.gray.opacity(0.1)` — works in both light and dark mode.
- Corner radius: 12pt via `.clipShape(RoundedRectangle(cornerRadius: 12))`.
- Internal padding: 16pt.
- No explicit border. Separation comes from background contrast.
- Selected state: background shifts to `Color.blue.opacity(0.2)` with `.blue` leading accent bar (3pt wide).

### Image Loading (Nuke 12.6.0)

```swift
LazyImage(url: imageURL) { state in
    if let image = state.image {
        image.resizable().aspectRatio(contentMode: .fill)
    } else if state.error != nil {
        Image(systemName: "photo")
            .foregroundStyle(.gray)
    } else {
        Color.gray.opacity(0.3) // placeholder
    }
}
```

- All remote images load via Nuke `LazyImage`.
- Placeholder: `Color.gray.opacity(0.3)` rectangle.
- Error state: SF Symbol `photo` in `.gray`.
- Thumbnails clip to `RoundedRectangle(cornerRadius: 8)`.

### QR Scanner Overlay

- Full-screen camera feed on `.black` background.
- Center cutout: rounded rectangle with `Color.green` border (3pt stroke).
- Corner brackets in `.green` at cutout corners.
- Instruction text in `.white` below cutout.
- On successful scan: brief `Color.green.opacity(0.2)` flash fill + haptic `.success`.

### Status Badges

```swift
Text(status.label)
    .font(.caption)
    .fontWeight(.semibold)
    .foregroundStyle(status.color)
    .padding(.horizontal, 8)
    .padding(.vertical, 4)
    .background(status.color.opacity(0.2))
    .clipShape(Capsule())
```

### Navigation

- `NavigationStack` with large titles on root views.
- `TabView` for top-level sections (if applicable).
- Toolbar items use SF Symbols at default weight.
- Liquid Glass material applied to navigation bars and tab bars for layered depth on iOS 26+.

### Lists

- `List` with `.insetGrouped` style as default.
- Swipe actions for quick operations (delete, archive, flag).
- Pull-to-refresh on all data-driven lists.
- Search via `.searchable` modifier on list views.

---

## 5. Layout Principles

### Spacing Scale

| Token | Value | Usage |
|-------|-------|-------|
| `xs` | 4pt | Tight internal gaps, icon-to-label |
| `sm` | 8pt | Between related elements, badge padding |
| `md` | 12pt | Card internal sections |
| `base` | 16pt | Standard padding, card insets, list row padding |
| `lg` | 20pt | Between card groups |
| `xl` | 24pt | Section spacing |
| `xxl` | 32pt | Major section breaks |

### Grid

- Single-column layouts for most operational views — warehouse staff use one hand.
- Two-column `LazyVGrid` for photo galleries and item thumbnails.
- `LazyVStack(spacing: 12)` for scrollable card lists.

### Whitespace Philosophy

Minimal. This is a dense operational tool. Whitespace exists to separate semantic groups, not for aesthetic breathing room. Every screen should maximize information density while remaining tappable at glove-friendly sizes.

### Safe Areas

- Respect all safe areas by default.
- QR scanner and photoshoot mode go `.ignoresSafeArea()` for full-bleed camera.
- Floating action buttons position via `.safeAreaInset(edge: .bottom)`.

---

## 6. Depth & Elevation

| Level | Surface | Treatment |
|-------|---------|-----------|
| 0 — Base | System background | `Color(.systemBackground)` |
| 1 — Card | Raised content | `Color.gray.opacity(0.1)` fill, no shadow |
| 2 — Floating | Action buttons, modals | `shadow(color: .black.opacity(0.15), radius: 8, y: 4)` |
| 3 — Overlay | QR scanner UI, camera HUD | Solid `.black` or `.ultraThinMaterial` blur |
| 4 — Sheet | Bottom sheets, detail panels | System `.sheet` presentation with default material |

### Liquid Glass (iOS 26+)

- Navigation bars and tab bars use Liquid Glass material for translucent layered depth.
- Toolbar buttons gain the glassy, refractive treatment automatically.
- Custom floating elements can opt into `.glassEffect()` where appropriate.
- Fallback: standard `.ultraThinMaterial` on iOS 17–25.

### Shadows

Shadows are used sparingly — only on floating action buttons and modal overlays. Cards rely on background color contrast, not shadow, for separation. This keeps the UI fast to render on older devices.

---

## 7. Do's and Don'ts

### Do

- ✅ Use SwiftUI semantic colors (`.primary`, `.secondary`, `.blue`, `.red`) — they adapt to light/dark automatically.
- ✅ Use SF Symbols for all icons. Prefer `.medium` weight to match SF Pro Text.
- ✅ Use `NavigationStack` with value-based navigation (TCA `StackState`).
- ✅ Make touch targets minimum 44×44pt — warehouse staff wear gloves.
- ✅ Use `.monospaced` for all machine-readable strings (container IDs, QR values, reference codes).
- ✅ Show loading states with `ProgressView()` — never leave the user staring at a blank screen.
- ✅ Use haptics for scan success (`.success`), errors (`.error`), and destructive confirmations (`.warning`).
- ✅ Keep feature density high — trained users prefer fewer taps over simpler screens.
- ✅ Use TCA `@Reducer` for all feature state management. One reducer per feature module.
- ✅ Load images exclusively through Nuke `LazyImage`.

### Don't

- ❌ Don't use custom hex colors — stick to system colors + the single `AccentColor` asset.
- ❌ Don't add decorative illustrations or empty-state artwork. A text label and SF Symbol suffice.
- ❌ Don't use `.plain` list style — always `.insetGrouped` for consistency.
- ❌ Don't hide functionality behind long-press or obscure gestures. Swipe actions and toolbar buttons only.
- ❌ Don't animate transitions beyond system defaults. No spring animations, no custom page transitions.
- ❌ Don't use `UIKit` wrappers when a SwiftUI equivalent exists.
- ❌ Don't sacrifice information density for whitespace. This is not a consumer app.
- ❌ Don't hardcode font sizes — always use the `Font` semantic API for Dynamic Type.
- ❌ Don't use third-party UI component libraries. SwiftUI native components only.
- ❌ Don't put business logic in views. All logic lives in TCA reducers.

---

## 8. Responsive Behavior

### Device Targets

| Device | Priority | Notes |
|--------|----------|-------|
| iPhone (6.1"–6.7") | Primary | Warehouse floor device. One-handed use. |
| iPad (11"–12.9") | Secondary | Office/desk use for management views. |

### Breakpoints (Size Classes)

| Size Class | Layout |
|------------|--------|
| Compact width | Single-column stack. Full-width cards. Bottom tab navigation. |
| Regular width (iPad) | Two-column `NavigationSplitView`. Sidebar + detail. Photo grids expand to 3–4 columns. |

### Touch Targets

- Minimum 44×44pt for all interactive elements — non-negotiable.
- QR scan button and camera shutter: 56pt diameter minimum.
- Swipe action hit areas use full row height.
- Glove-friendly: prefer larger targets (48–56pt) where layout permits.

### Orientation

- Portrait-locked for iPhone. Warehouse scanning is a portrait activity.
- iPad supports both orientations with adaptive layout via `NavigationSplitView`.

### Dynamic Type

- All text scales with Dynamic Type up to `.accessibility3`.
- Layouts use `ScrollView` to accommodate expanded text without truncation.
- Status badges cap at `.body` equivalent to prevent layout breakage.

---

## 9. Agent Prompt Guide

### Quick Color Reference

```
Accent/Brand:   #0066CC (Assets.xcassets AccentColor)
Interactive:     .blue (#007AFF)
Success:         .green (#34C759)
Warning:         .orange (#FF9500)
Processing:      .purple (#AF52DE)
Error:           .red (#FF3B30)
Disabled:        .gray (#8E8E93)
Scanner BG:      .black (#000000)
Card BG:         Color.gray.opacity(0.1)
Placeholder:     Color.gray.opacity(0.3)
Selected BG:     Color.blue.opacity(0.2)
Success BG:      Color.green.opacity(0.2)
Warning BG:      Color.orange.opacity(0.2)
```

### Architecture Reference

```
StowzillaOps/
├── App/                    # App entry point, root reducer
├── Packages/
│   ├── Core/               # Shared utilities, extensions, constants
│   ├── Networking/          # API client, endpoints, DTOs
│   ├── Domain/              # Business models, shared state
│   └── UI/                  # Shared UI components, styles
├── Features/
│   ├── QRScanner/           # Camera scanning with green overlay
│   ├── QuickIntake/         # Photography-first item intake
│   ├── ContainerManagement/ # Container CRUD, assignment
│   ├── CustomerDetail/      # Customer info, item history
│   ├── Schedule/            # Appointment and pickup scheduling
│   ├── ItemLabeling/        # Label generation and printing
│   └── PhotoshootMode/      # Multi-angle item photography
└── Resources/               # Assets.xcassets, Localizable
```

### Ready-to-Use Prompts

**New feature screen:**
> Build a SwiftUI view for [feature] using TCA v1.10.0. Use NavigationStack, .insetGrouped List style, system colors for status states. Cards use Color.gray.opacity(0.1) background with 12pt corner radius. Load images with Nuke LazyImage. Minimum 44pt touch targets. Follow the StowzillaOps DESIGN.md.

**New TCA reducer:**
> Create a TCA @Reducer for [feature]. Use StackState for navigation. Keep all business logic in the reducer — views are pure render functions. Use @Dependency for API calls via the Networking package.

**Status badge component:**
> Build a reusable StatusBadge view. Capsule shape, .caption font, .semibold weight. Foreground is the status color, background is status color at 0.2 opacity. Statuses: pending (.orange), processing (.purple), completed (.green), error (.red).

**QR scanner screen:**
> Build a full-screen QR scanner. Black background, camera feed centered. Green rounded-rectangle cutout border (3pt stroke). On scan success: green flash overlay + .success haptic. Use AVFoundation for capture, SwiftUI overlay for chrome.

**Item card:**
> Build an item card: Color.gray.opacity(0.1) background, 12pt corner radius, 16pt padding. Left thumbnail (Nuke LazyImage, 8pt radius, Color.gray.opacity(0.3) placeholder). Right side: .headline title, .subheadline subtitle, StatusBadge. Selected state: Color.blue.opacity(0.2) background with 3pt blue leading bar.
