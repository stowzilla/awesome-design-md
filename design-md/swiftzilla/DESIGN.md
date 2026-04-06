# Design System — Swiftzilla

## 1. Visual Theme & Atmosphere

Swiftzilla is the Stowzilla customer-facing iOS app — a self-storage companion that should feel like it barely exists. The design philosophy is **Invisible Simplicity**: zero cognitive load, no feature discovery required, no learning curve. Every feature should feel like a non-feature. If a customer has to think about the interface, the interface has failed.

This is the deliberate inverse of the ops-ios app. Where the ops app is "Dr Who's screwdriver" — a dense, powerful multi-tool for warehouse staff — Swiftzilla is the calm, quiet surface that the ops app's complexity enables. The customer never sees the machinery. They see a clean, warm, nature-inspired interface that lets them schedule a pickup, check their items, and get on with their life.

The visual language draws from the Stowzilla web brand: warm forest greens, golden yellows, and earthy oranges that evoke a sense of natural reliability — your stuff is safe, stored like acorns in a well-organized tree. The palette avoids the cold, clinical blues of traditional storage apps. Instead, it feels like a friendly park ranger's cabin: organized, trustworthy, and warm.

Built with Swift + SwiftUI + TCA (The Composable Architecture) v1.10.0, targeting iOS 17+. Images via Nuke 12.6.0. The architecture mirrors ops-ios (Core → Networking → Domain → UI via SPM) but the feature surface is radically smaller. AI-powered features like auto-naming items via Foundation Models should work invisibly — the customer never knows AI is involved, they just notice that things are easier than expected.

**Key Characteristics:**
- Nature-inspired warmth: Forest Green (`#4A7930`), Golden Yellow (`#F7B32B`), Warm Orange (`#FF8C42`) — not corporate, not cold
- iOS-native to the bone: system backgrounds, Dynamic Type, SF Pro, system label colors — never fight the platform
- Five-tab architecture: Dashboard, Inventory, Pickups, Returns, Profile — flat, no nested navigation mazes
- Invisible AI: Foundation Models auto-name items, suggest categories — the customer never sees the word "AI"
- Liquid Glass: inherit system defaults, never customize — let Apple handle the visual evolution
- Extreme restraint: if a feature requires explanation, it's too complex for this app
- TCA state management ensures every screen is a pure function of state — no hidden side effects, no surprise behaviors

## 2. Color Palette & Roles

### Brand — Nature-Inspired Warmth

- **Forest Green** (`#4A7930`): Primary brand accent. Matches the web app's `--accent-color`. Used for primary action buttons, active tab icons, success confirmations, and brand moments. This is the color customers associate with Stowzilla. Maps to SwiftUI `.green` variants where semantic meaning aligns.
- **Golden Yellow** (`#F7B32B`): Secondary accent. Scheduling highlights, pickup confirmations, "items ready" badges, and celebratory moments. Warm and optimistic — the color of "your stuff is on its way." Use sparingly to maintain signal strength.
- **Warm Orange** (`#FF8C42`): Tertiary accent. Scheduling urgency, upcoming pickup reminders, and gentle attention-getters. Never alarming — this is "hey, heads up" not "something's wrong." Maps to SwiftUI `.orange` for warnings.
- **Teal** (`#2D9B9B`): Informational accent. Tips, onboarding hints, and contextual help. Cool complement to the warm palette — used when the app needs to teach without lecturing.
- **Deep Navy** (`#0B2C4A`): Call-to-action backgrounds on light surfaces. "Schedule Pickup" and "Request Return" primary CTAs. Authoritative without being aggressive — the color of a trusted uniform.

### System Semantic — iOS Native

- **SwiftUI `.green`**: Brand-aligned success states, active indicators, completed pickups. Harmonizes with Forest Green across light/dark modes.
- **SwiftUI `.blue`**: Links, interactive text, navigation chevrons, "Learn more" actions. The universal iOS "this is tappable" signal.
- **SwiftUI `.orange`**: Warnings, scheduling conflicts, items approaching storage limits. Gentle urgency.
- **SwiftUI `.red`**: Errors, destructive actions (cancel pickup, remove item), overdue payments. Used with confirmation dialogs — never a surprise.
- **SwiftUI `.gray`**: Secondary text, disabled controls, placeholder content, dividers. The workhorse neutral.

### Surfaces & Text — System Default

- **Background**: `Color(.systemBackground)` — pure white in light mode, true black in dark mode. Never override.
- **Secondary Background**: `Color(.secondarySystemBackground)` — grouped table backgrounds, card surfaces.
- **Tertiary Background**: `Color(.tertiarySystemBackground)` — nested card surfaces, input field backgrounds.
- **Primary Label**: `Color(.label)` — primary text. Adapts automatically to light/dark.
- **Secondary Label**: `Color(.secondaryLabel)` — subtitles, metadata, timestamps.
- **Tertiary Label**: `Color(.tertiaryLabel)` — placeholder text, disabled labels.
- **Separator**: `Color(.separator)` — list dividers, section borders.

### Keychain & API

- Keychain service identifier: `com.stowzilla.customer`
- API namespace: `/customer/*` endpoints
- User model: simplified — no roles, no permissions, no admin flags

## 3. Typography Rules

### Font Family

- **All text**: SF Pro via SwiftUI's native `.font()` modifiers. Never import custom fonts. SF Pro's optical sizing handles everything — Display variants for large titles, Text variants for body, Rounded variants where the system applies them.
- **Monospace** (item IDs, tracking codes): `Font.system(.caption, design: .monospaced)` — SF Mono, system-managed.

### Hierarchy

| Role | SwiftUI Style | Size (Default) | Weight | Design | Usage |
|------|--------------|----------------|--------|--------|-------|
| Large Title | `.largeTitle` | 34pt | `.bold` | Default | Tab root view navigation titles |
| Title | `.title` | 28pt | `.bold` | Default | Section headers on Dashboard |
| Title 2 | `.title2` | 22pt | `.bold` | Default | Card group headings, "My Items" |
| Title 3 | `.title3` | 20pt | `.semibold` | Default | Card titles, item names |
| Headline | `.headline` | 17pt | `.semibold` | Default | List row primary text, button labels |
| Body | `.body` | 17pt | `.regular` | Default | Descriptions, detail text, form labels |
| Callout | `.callout` | 16pt | `.regular` | Default | Supporting text, inline help |
| Subheadline | `.subheadline` | 15pt | `.regular` | Default | Secondary list row text, metadata |
| Footnote | `.footnote` | 13pt | `.regular` | Default | Timestamps, tertiary info, legal |
| Caption | `.caption` | 12pt | `.regular` | Default | Badges, tags, smallest readable text |
| Caption 2 | `.caption2` | 11pt | `.regular` | Default | Fine print, version numbers |
| Mono Code | `.caption` | 12pt | `.regular` | `.monospaced` | Item IDs, tracking numbers, codes |

### Principles

- **Dynamic Type always**: every text element must use SwiftUI's built-in text styles. Never hardcode point sizes. The app must remain usable from `xSmall` to `AX5` accessibility sizes.
- **No custom fonts**: SF Pro is the only typeface. Custom fonts add bundle size, break Dynamic Type, and fight the platform. The ops app uses SF Pro; the customer app uses SF Pro. Brand consistency comes from color and layout, not typography.
- **Weight as hierarchy**: `.bold` for titles, `.semibold` for headings and emphasis, `.regular` for everything else. Never use `.light` or `.ultraLight` — they collapse at small Dynamic Type sizes and fail accessibility contrast.
- **Relative sizing via SwiftUI**: use `@ScaledMetric` for any dimension that should scale with Dynamic Type (icon sizes, spacing near text, minimum tap targets).

## 4. Component Stylings

### Buttons

**Primary CTA (Schedule Pickup, Request Return)**
- Background: Deep Navy `#0B2C4A`
- Text: `.white`, `.headline` weight
- Corner radius: 12pt (`.cornerRadius(12)`)
- Padding: 16pt vertical, full width (`.frame(maxWidth: .infinity)`)
- Min height: 50pt (accessible tap target)
- Pressed state: opacity 0.85
- Disabled state: `.opacity(0.4)`

**Secondary Action (View Details, Edit Item)**
- Background: Forest Green `#4A7930`
- Text: `.white`, `.headline` weight
- Corner radius: 12pt
- Padding: 12pt vertical, horizontal padding 24pt
- Pressed state: opacity 0.85

**Tertiary / Text Button (Cancel, Skip, Learn More)**
- Background: `.clear`
- Text: `.blue`, `.body` weight
- No border, no fill
- Pressed state: opacity 0.6

**Destructive (Cancel Pickup, Remove Item)**
- Background: `.clear`
- Text: `.red`, `.body` weight
- Always paired with a confirmation dialog — never immediate

### Cards

**Item Card (Inventory list)**
- Background: `Color(.secondarySystemBackground)`
- Corner radius: 16pt
- Padding: 16pt all sides
- Shadow: none in light mode; none in dark mode — elevation via background color only
- Image: 80×80pt thumbnail via Nuke, corner radius 8pt, `.fill` aspect ratio
- Layout: `HStack` — image left, text stack right, chevron trailing
- Primary text: `.headline` — item name
- Secondary text: `.subheadline` + `.secondary` color — category, date stored

**Dashboard Summary Card**
- Background: `Color(.secondarySystemBackground)`
- Corner radius: 16pt
- Padding: 20pt all sides
- Layout: `VStack(alignment: .leading, spacing: 8)`
- Metric number: `.title` + `.bold` + Forest Green color
- Label: `.subheadline` + `.secondary` color
- Optional icon: SF Symbol, 24pt, Forest Green

**Pickup/Return Status Card**
- Background: `Color(.secondarySystemBackground)`
- Corner radius: 16pt
- Padding: 16pt all sides
- Status badge: pill shape, 6pt vertical / 12pt horizontal padding, caption text
  - Scheduled: Golden Yellow `#F7B32B` background, Deep Navy text
  - In Transit: Teal `#2D9B9B` background, white text
  - Completed: Forest Green `#4A7930` background, white text
  - Cancelled: `.gray` background, `.secondary` text

### Navigation

**Tab Bar (5 tabs)**
- Style: system default `TabView`
- Icons: SF Symbols, regular weight
  - Dashboard: `house`
  - Inventory: `shippingbox`
  - Pickups: `truck.box`
  - Returns: `arrow.uturn.left.circle`
  - Profile: `person.circle`
- Active tint: Forest Green `#4A7930`
- Inactive tint: `.gray`
- Labels: always visible (no icon-only tabs)

**Navigation Bar**
- Style: `.navigationBarTitleDisplayMode(.large)` on root views, `.inline` on pushed views
- Liquid Glass: inherit system defaults — do not customize `UINavigationBarAppearance`
- Back button: system default chevron + previous title

### Inputs

**Text Field (Item name, search)**
- Background: `Color(.tertiarySystemBackground)`
- Corner radius: 10pt
- Padding: 12pt horizontal, 14pt vertical
- Placeholder: `.tertiaryLabel` color
- Border: none at rest; 2pt Forest Green on focus
- Font: `.body`

**Search Bar**
- Use SwiftUI `.searchable()` modifier — system search bar, no custom implementation
- Placement: `.navigationBarDrawer(displayMode: .always)` on Inventory tab

### Lists

- Use native SwiftUI `List` with `.listStyle(.insetGrouped)`
- Section headers: `.subheadline` + `.secondary` color, uppercase
- Swipe actions: `.red` for delete, Forest Green for primary action
- Pull-to-refresh: native `.refreshable {}` — no custom spinners

### Empty States

- Centered `VStack`, 60% vertical offset from top
- SF Symbol: 48pt, `.gray` color, relevant to context
- Title: `.title3` + `.secondary` color
- Subtitle: `.subheadline` + `.tertiaryLabel` color
- Optional CTA button: Primary style

## 5. Layout Principles

### Spacing Scale

| Token | Value | Usage |
|-------|-------|-------|
| `xs` | 4pt | Inline icon-to-text gap, tight badge padding |
| `sm` | 8pt | Between related elements within a component |
| `md` | 12pt | Between components within a section |
| `base` | 16pt | Standard card padding, list row insets, section gaps |
| `lg` | 20pt | Between sections, dashboard card padding |
| `xl` | 24pt | Major section separation |
| `2xl` | 32pt | Screen-level top/bottom padding |
| `3xl` | 48pt | Empty state vertical centering offset |

### Grid & Layout

- **No custom grid**: use SwiftUI's native `List`, `LazyVStack`, `LazyVGrid` — the system handles safe areas, keyboard avoidance, and scroll behavior.
- **Content width**: full screen width with 16pt horizontal padding (`.padding(.horizontal, 16)`). No max-width constraints — iOS handles this natively.
- **Card grids** (Dashboard): 2-column `LazyVGrid` with `GridItem(.flexible())`, spacing 12pt.
- **List views** (Inventory, Pickups, Returns): full-width `List` with `.insetGrouped` style.

### Whitespace Philosophy

- **Breathing room over density**: this is not a power-user tool. Every screen should feel like it has 30% fewer elements than you'd expect. If a screen feels "full," remove something.
- **Vertical rhythm**: consistent 16pt spacing between list rows, 20pt between sections, 32pt between major screen regions.
- **Safe area respect**: never extend content into safe areas. Let the system manage notch, Dynamic Island, and home indicator spacing.

### Navigation Architecture

```
TabView
├── Dashboard (NavigationStack)
│   ├── DashboardView (root)
│   ├── ItemDetailView (push)
│   └── PickupDetailView (push)
├── Inventory (NavigationStack)
│   ├── InventoryListView (root, searchable)
│   ├── ItemDetailView (push)
│   └── AddItemView (sheet)
├── Pickups (NavigationStack)
│   ├── PickupListView (root)
│   ├── PickupDetailView (push)
│   └── SchedulePickupView (sheet)
├── Returns (NavigationStack)
│   ├── ReturnListView (root)
│   ├── ReturnDetailView (push)
│   └── RequestReturnView (sheet)
└── Profile (NavigationStack)
    ├── ProfileView (root)
    ├── AccountSettingsView (push)
    ├── PaymentMethodsView (push)
    └── SupportView (push)
```

- **Sheets for creation**: adding items, scheduling pickups, requesting returns — modal sheets with `.presentationDetents([.medium, .large])`.
- **Push for detail**: viewing item details, pickup status, return tracking — standard navigation push.
- **No deep nesting**: maximum 2 levels deep from any tab root. If you need a third level, rethink the information architecture.

## 6. Depth & Elevation

### Philosophy

Depth in Swiftzilla is communicated through **background color layering**, not shadows. This is the iOS-native approach and it works perfectly in both light and dark mode without any custom shadow management.

### Surface Hierarchy

| Level | Surface | SwiftUI Color | Usage |
|-------|---------|---------------|-------|
| 0 (Base) | Screen background | `.systemBackground` | Root view canvas |
| 1 (Grouped) | Section background | `.secondarySystemBackground` | Cards, grouped list sections |
| 2 (Nested) | Nested surface | `.tertiarySystemBackground` | Input fields inside cards, nested containers |
| 3 (Overlay) | Sheet / Modal | `.systemBackground` (new context) | Sheets reset to Level 0 — they are new surfaces |

### Shadow Usage

- **None**: cards, buttons, and list rows use zero shadow. Background color differentiation provides sufficient depth.
- **Exception**: `.shadow(color: .black.opacity(0.1), radius: 8, y: 4)` only on floating action buttons if ever introduced (currently none in the design).
- **System sheets**: inherit iOS sheet shadow behavior — never customize.

### Material & Blur

- **Liquid Glass**: inherit all system defaults for navigation bars, tab bars, and sheets. Do not apply custom `.background(.ultraThinMaterial)` unless the system doesn't provide it automatically.
- **No custom blur layers**: the ops app uses blur for complex overlay states. The customer app has no overlays complex enough to warrant blur.

## 7. Do's and Don'ts

### Do

- ✅ Use system colors (`Color(.label)`, `Color(.systemBackground)`) for all text and backgrounds — they adapt to light/dark/high-contrast automatically
- ✅ Support Dynamic Type at every size — test at `AX5` (largest accessibility size) before shipping
- ✅ Use SF Symbols for all icons — they scale with Dynamic Type and support accessibility traits
- ✅ Let AI features work invisibly — auto-name items via Foundation Models without showing "AI-powered" badges
- ✅ Use native SwiftUI components (`List`, `.searchable`, `.refreshable`, `TabView`) — don't rebuild what Apple provides
- ✅ Keep navigation flat — 5 tabs, max 2 push levels deep
- ✅ Use TCA `@Reducer` for every feature — state is always a pure function, effects are always explicit
- ✅ Load images with Nuke's `LazyImage` — it handles caching, placeholders, and progressive loading
- ✅ Store auth tokens in Keychain (`com.stowzilla.customer`) — never UserDefaults
- ✅ Confirm destructive actions with `.confirmationDialog` — never delete/cancel on a single tap
- ✅ Use `.sensoryFeedback(.impact, trigger:)` for meaningful state changes (pickup scheduled, item added)
- ✅ Respect the 2-action App Intents limit: "Schedule Pickup" and "View My Items" for Control Center

### Don't

- ❌ Don't hardcode colors — no hex literals in view code. Define all colors in Asset Catalog or as `Color` extensions
- ❌ Don't hardcode font sizes — no `.font(.system(size: 17))`. Always use text styles (`.body`, `.headline`)
- ❌ Don't customize Liquid Glass — no `UINavigationBarAppearance` overrides, no custom tab bar backgrounds
- ❌ Don't add biometric auth — not in scope yet. Don't pre-build it, don't stub it
- ❌ Don't show AI/ML labels — no "Powered by AI" badges, no "Smart suggestions" headers. It just works.
- ❌ Don't use custom tab bars — the system `TabView` handles safe areas, accessibility, and Liquid Glass
- ❌ Don't nest navigation deeper than 2 levels — if you need depth, use sheets instead
- ❌ Don't use `.shadow()` on cards — use background color layering for depth
- ❌ Don't build custom pull-to-refresh — use `.refreshable {}`
- ❌ Don't use `UIKit` wrappers unless SwiftUI has no equivalent — every `UIViewRepresentable` is tech debt
- ❌ Don't add features that require explanation — if it needs a tooltip, it's too complex for this app
- ❌ Don't use more than 2 App Intents — restraint is the design
- ❌ Don't use Visual Intelligence unless it removes measurable friction (e.g., scan a label to auto-fill item details)

## 8. Responsive Behavior

### Device Support

- **iPhone only** (iOS 17+): iPhone SE 3rd gen through iPhone 16 Pro Max
- **No iPad layout**: if iPad support is added later, use a sidebar-based `NavigationSplitView` — but don't pre-build it
- **No landscape**: lock to portrait (`.supportedInterfaceOrientations(.portrait)`) — storage management doesn't need landscape

### Dynamic Type Scaling

| Size Category | Behavior |
|--------------|----------|
| `xSmall` → `xxxLarge` | All layouts flex naturally via SwiftUI text styles |
| `AX1` → `AX5` | Cards stack vertically (`HStack` → `VStack` via `@Environment(\.dynamicTypeSize)`), images shrink, buttons remain full-width and ≥50pt tall |

### Touch Targets

- **Minimum 44×44pt** for all interactive elements — Apple HIG requirement
- **Primary CTAs**: 50pt tall, full width — oversized for easy thumb reach
- **List rows**: minimum 44pt tall, system default insets
- **Tab bar icons**: system-managed sizing — never override

### Keyboard Behavior

- **Automatic avoidance**: SwiftUI handles keyboard avoidance natively. Never manually offset views.
- **Dismiss on scroll**: `.scrollDismissesKeyboard(.interactively)` on all scrollable views with text inputs
- **Submit actions**: `.onSubmit {}` for search fields and single-field forms

### Safe Areas

- **Never ignore safe areas** unless displaying full-bleed images (item photos in detail view)
- **Home indicator**: system-managed — the tab bar handles this
- **Dynamic Island**: system-managed — navigation bar handles this
- **Keyboard**: system-managed — SwiftUI handles this

### Image Handling (Nuke 12.6.0)

- **Thumbnails** (list rows): 80×80pt, `ContentMode.fill`, corner radius 8pt, `ImageRequest` with `.thumbnail(.init(size: CGSize(width: 160, height: 160)))` for 2x resolution
- **Detail images**: full-width, aspect ratio `.fit`, progressive loading enabled
- **Placeholders**: `Color(.tertiarySystemBackground)` with SF Symbol `photo` in `.gray`
- **Failure state**: same placeholder with SF Symbol `exclamationmark.triangle` in `.orange`

## 9. Agent Prompt Guide

### Quick Color Reference

```
Brand:
  Forest Green:   #4A7930  (primary accent, active states, success)
  Golden Yellow:  #F7B32B  (scheduling, confirmations, badges)
  Warm Orange:    #FF8C42  (reminders, gentle urgency)
  Teal:           #2D9B9B  (informational, tips, onboarding)
  Deep Navy:      #0B2C4A  (primary CTA backgrounds)

System Semantic:
  .green   → success, brand-aligned positive states
  .blue    → links, interactive elements
  .orange  → warnings, scheduling urgency
  .red     → errors, destructive actions
  .gray    → secondary, disabled, placeholder

Surfaces:
  .systemBackground           → screen canvas
  .secondarySystemBackground  → cards, grouped sections
  .tertiarySystemBackground   → inputs, nested surfaces

Text:
  .label          → primary text
  .secondaryLabel → subtitles, metadata
  .tertiaryLabel  → placeholders, disabled
```

### Ready-to-Use Prompts

**"Build a new feature screen"**
> Create a SwiftUI view for [feature] in the Swiftzilla customer app. Use TCA `@Reducer` for state management. Background is `Color(.systemBackground)`. Cards use `Color(.secondarySystemBackground)` with 16pt corner radius and 16pt padding. Primary CTA is Deep Navy `#0B2C4A` with white `.headline` text, full width, 50pt tall, 12pt corner radius. All text uses SwiftUI text styles (`.title`, `.headline`, `.body`) — no hardcoded sizes. Support Dynamic Type. Navigation via `NavigationStack` push, creation flows via `.sheet`. Load images with Nuke `LazyImage`.

**"Add an item card to a list"**
> Create an item card as a `List` row. `HStack` layout: 80×80pt Nuke `LazyImage` thumbnail (corner radius 8pt, `.fill`) on the left, `VStack(alignment: .leading, spacing: 4)` with `.headline` item name and `.subheadline` + `.secondary` color metadata on the right, system chevron trailing. Background is `Color(.secondarySystemBackground)`. Tap navigates via `NavigationLink`. Swipe actions: Forest Green `#4A7930` for primary action, `.red` for delete with `.confirmationDialog`.

**"Create a status badge"**
> Build a pill-shaped status badge. `Text` with `.caption` font, `6pt` vertical and `12pt` horizontal padding, `Capsule()` clip shape. Colors by status: Scheduled = Golden Yellow `#F7B32B` background + Deep Navy `#0B2C4A` text. In Transit = Teal `#2D9B9B` background + white text. Completed = Forest Green `#4A7930` background + white text. Cancelled = `.gray` background + `.secondary` text.

**"Implement invisible AI item naming"**
> When a customer adds a new item, use Foundation Models (iOS 26+) to suggest a name from the item photo. Show the suggestion as pre-filled text in the name field — no "AI suggested" label, no sparkle icon, no explanation. If the model is unavailable or confidence is low, leave the field empty. The customer should never know AI was involved. Gate behind `#available(iOS 26, *)` with empty fallback.

**"Add a Control Center App Intent"**
> Create an `AppIntent` for "Schedule Pickup" that opens the app to the Pickups tab with the SchedulePickupView sheet presented. Use `@Parameter` for optional item selection. Register via `AppShortcutsProvider` with a phrase like "Schedule a Stowzilla pickup." Maximum 2 App Intents total (the other is "View My Items"). Keep the intent implementation minimal — it should just set TCA state to present the correct view.

### Architecture Reminders for Agents

- **TCA v1.10.0**: every feature is a `@Reducer` struct with `State`, `Action`, `body: some ReducerOf<Self>`. Use `@Dependency` for API clients, Keychain access, and Nuke image loading.
- **SPM module structure**: `Core` (shared models, extensions) → `Networking` (API client, `/customer/*` endpoints) → `Domain` (TCA reducers, business logic) → `UI` (SwiftUI views).
- **Nuke 12.6.0**: use `LazyImage` in views, `ImagePipeline.shared` configuration in app setup. Thumbnail requests for list views, full requests for detail views.
- **Keychain**: `com.stowzilla.customer` service identifier. Store JWT tokens only. No biometric auth.
- **User model**: simplified — `id`, `email`, `name`, `phone`, `createdAt`. No roles, no permissions, no team membership.
