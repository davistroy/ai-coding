# TacoTracker 3000 - UI/UX Design Document

## Enterprise Taco Truck Tracking and Ordering System for Globex Corporation

| Document Information |                                         |
|---------------------|-----------------------------------------|
| Version             | 1.0                                     |
| Status              | Approved                                |
| Author              | Digital Experience Design Team          |
| Last Updated        | January 22, 2026                        |
| Approved By         | VP of Employee Experience - Jan 20, 2026|

## Document History

| Version | Date       | Author           | Changes                              |
|---------|------------|------------------|--------------------------------------|
| 0.5     | Dec 2025   | DX Design Team   | Initial design exploration           |
| 0.8     | Jan 10, 2026| DX Design Team  | Component library complete           |
| 1.0     | Jan 22, 2026| DX Design Team  | Final approved version               |

---

## 1. Executive Summary

### Product Overview

TacoTracker 3000 is a mobile application and vendor web portal enabling Globex Corporation's 2,500 employees to locate, browse, and order from food trucks across three campus buildings. The design prioritizes speed, accessibility, and delight while maintaining enterprise-grade reliability.

### Key Design Decisions

- **Mobile-first, cross-platform**: React Native for iOS/Android ensures consistent experience
- **Map-centric home screen**: Immediate visibility of truck locations upon app launch
- **3-tap ordering**: Repeat orders achievable in 3 taps or fewer
- **WCAG 2.1 AA compliance**: Full accessibility for all employees
- **Dark mode support**: System-preference-aware theming
- **Food-forward brand**: Professional with playful food-themed accents

### Platform Support

| Platform | Version | Framework |
|----------|---------|-----------|
| iOS | 14.0+ | React Native |
| Android | 10+ | React Native |
| Web (Vendor Portal) | Modern browsers | React + TypeScript |

---

## 2. Design Principles

### Principle 1: Speed to Taco

**Statement**: Every design decision prioritizes getting users to their tacos as quickly as possible.

**Rationale**: Employees have limited lunch breaks. The app should never stand between a user and their food.

**Application**:
- Map view as default home screen
- One-tap reordering
- Pre-filled payment from Globex Wallet
- Maximum 3 taps for repeat orders

**Do**: Show truck locations immediately on launch
**Don't**: Require confirmations for non-destructive actions

---

### Principle 2: Clarity Over Cleverness

**Statement**: Information must be immediately understandable without interpretation.

**Rationale**: Users check the app while multitasking. Complex interfaces cause errors.

**Application**:
- Use familiar patterns (map, list, cart)
- Always label icons with text
- Show prices and wait times prominently

**Do**: Use "Order Now" over creative phrasing
**Don't**: Rely on color alone to convey meaning

---

### Principle 3: Inclusive by Default

**Statement**: The app works for everyone regardless of ability or dietary needs.

**Rationale**: Globex values diversity; tools must reflect this commitment.

**Application**:
- WCAG 2.1 AA minimum compliance
- Prominent dietary filters
- One-handed mobile operation
- Support 200% font scaling

**Do**: Provide text alternatives for all images
**Don't**: Require precise gestures for essential functions

---

### Principle 4: Trustworthy Transactions

**Statement**: Users must feel confident in order accuracy and payment security.

**Application**:
- Order summary before payment
- Immediate confirmation notifications
- Visible security indicators

---

## 3. Color Palette

### Primary Colors

| Name | Hex | RGB | Usage | Contrast on White |
|------|-----|-----|-------|-------------------|
| **Taco Orange** | `#E85D04` | 232, 93, 4 | Primary actions, brand accent | 3.4:1 (large text) |
| **Taco Orange Dark** | `#D14D00` | 209, 77, 0 | Hover/pressed states | 4.2:1 |
| **Taco Orange Light** | `#FF7B2E` | 255, 123, 46 | Dark mode primary | - |

### Secondary Colors

| Name | Hex | RGB | Usage |
|------|-----|-----|-------|
| **Corn Yellow** | `#FFB703` | 255, 183, 3 | Highlights, focus indicators |
| **Tortilla Tan** | `#DDB892` | 221, 184, 146 | Subtle warm accents |
| **Lime Zest** | `#76C893` | 118, 200, 147 | Secondary success |

### Semantic Colors

| Name | Hex | Purpose | Contrast Ratio |
|------|-----|---------|----------------|
| **Success (Guacamole)** | `#2D6A4F` | Confirmations, complete | 5.4:1 AA |
| **Warning** | `#9A6700` | Cautions, wait states | 4.6:1 AA |
| **Error (Salsa Red)** | `#C1121F` | Errors, required fields | 6.2:1 AA |
| **Info** | `#0077B6` | Informational messages | 4.5:1 AA |

### Neutral Colors

| Name | Hex | Usage |
|------|-----|-------|
| **Charcoal** | `#212529` | Primary text |
| **Slate** | `#495057` | Secondary text |
| **Pewter** | `#6C757D` | Placeholder, disabled |
| **Silver** | `#ADB5BD` | Borders, dividers |
| **Cloud** | `#DEE2E6` | Card backgrounds |
| **Mist** | `#F8F9FA` | Page backgrounds |
| **White** | `#FFFFFF` | Cards, inputs |

### Dark Mode Mapping

| Light Mode Token | Dark Mode Value |
|-----------------|-----------------|
| Background (Mist) | `#212529` |
| Surface (White) | `#2D3238` |
| Text Primary (Charcoal) | `#FFFFFF` |
| Text Secondary (Slate) | `#ADB5BD` |
| Primary (Taco Orange) | `#FF7B2E` |

---

## 4. Typography

### Font Families

**Primary**: Inter
- Weights: 400 (Regular), 500 (Medium), 600 (SemiBold), 700 (Bold)
- Fallback: `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`

**Monospace**: JetBrains Mono
- Weights: 400 (Regular), 500 (Medium)
- Use: Order numbers, prices
- Fallback: `"SF Mono", Consolas, monospace`

### Type Scale

| Style | Size | Weight | Line Height | Letter Spacing | Usage |
|-------|------|--------|-------------|----------------|-------|
| Display | 32px | 700 | 40px | -0.5px | Hero headlines |
| H1 | 28px | 700 | 36px | -0.3px | Screen titles |
| H2 | 24px | 600 | 32px | -0.2px | Section headers |
| H3 | 20px | 600 | 28px | 0 | Card titles |
| H4 | 18px | 600 | 24px | 0 | Subsection headers |
| Body Large | 16px | 400 | 24px | 0 | Primary content |
| Body | 14px | 400 | 20px | 0 | Default text |
| Body Small | 12px | 400 | 16px | 0.1px | Captions |
| Label | 14px | 500 | 20px | 0.1px | Form labels |
| Button | 14px | 600 | 20px | 0.3px | Button text |
| Caption | 12px | 400 | 16px | 0.2px | Helper text |
| Overline | 10px | 600 | 14px | 1px | Category labels |

---

## 5. Spacing System

**Base Unit**: 4px (8-point grid alignment)

| Token | Value | Usage |
|-------|-------|-------|
| `space-0` | 0px | No spacing |
| `space-1` | 4px | Tight inline spacing |
| `space-2` | 8px | Related elements, icon padding |
| `space-3` | 12px | Default element spacing |
| `space-4` | 16px | Card padding, margins |
| `space-5` | 20px | Between components |
| `space-6` | 24px | Section spacing |
| `space-8` | 32px | Large gaps |
| `space-10` | 40px | Major sections |
| `space-12` | 48px | Screen sections |
| `space-16` | 64px | Page-level spacing |

### Border Radius

| Token | Value | Usage |
|-------|-------|-------|
| `radius-none` | 0px | Sharp corners |
| `radius-sm` | 4px | Subtle rounding |
| `radius-md` | 8px | Buttons, inputs |
| `radius-lg` | 12px | Cards |
| `radius-xl` | 16px | Bottom sheets |
| `radius-full` | 9999px | Pills, avatars |

### Elevation (Shadows)

| Level | Value | Usage |
|-------|-------|-------|
| `elevation-0` | none | Flat elements |
| `elevation-1` | `0 1px 2px rgba(0,0,0,0.05)` | Subtle lift |
| `elevation-2` | `0 2px 8px rgba(0,0,0,0.08)` | Cards |
| `elevation-3` | `0 4px 16px rgba(0,0,0,0.12)` | Floating elements |
| `elevation-4` | `0 8px 24px rgba(0,0,0,0.16)` | Modals, overlays |

---

## 6. Grid System

### Mobile Grid (375px base)

```
┌─────────────────────────────────────┐
│←16px→│      Content (343px)    │←16px→│
│      │  ┌─────┐ 12px ┌─────┐  │      │
│  M   │  │ COL │      │ COL │  │  M   │
│      │  └─────┘      └─────┘  │      │
└─────────────────────────────────────┘

Margins: 16px
Gutters: 12px
Columns: 4 (flexible)
```

### Tablet Grid (768px)

- Margins: 24px
- Gutters: 16px
- Columns: 8

### Desktop Grid (1440px max)

- Margins: 64px
- Gutters: 24px
- Columns: 12 (85px each)

### Breakpoints

| Name | Range | Target |
|------|-------|--------|
| `xs` | 0-374px | Small phones |
| `sm` | 375-767px | Standard phones |
| `md` | 768-1023px | Tablets |
| `lg` | 1024-1439px | Small desktops |
| `xl` | 1440px+ | Large desktops |

---

## 7. Component Specifications

### 7.1 Primary Button

**Purpose**: Main call-to-action

```
Primary Button States
┌──────────────────────────────────────────────────┐
│ Default      Hover        Active       Disabled  │
│ ┌────────┐   ┌────────┐   ┌────────┐   ┌────────┐│
│ │ Order  │   │ Order  │   │ Order  │   │ Order  ││
│ │  Now   │   │  Now   │   │  Now   │   │  Now   ││
│ └────────┘   └────────┘   └────────┘   └────────┘│
│ #E85D04      #D14D00      #B84300      50% opacity│
└──────────────────────────────────────────────────┘
```

**Specifications**:

| Property | Value |
|----------|-------|
| Height | 48px (touch target) |
| Min Width | 120px |
| Padding | 16px horizontal |
| Background | `#E85D04` |
| Text | Inter SemiBold 14px, `#FFFFFF` |
| Border Radius | 8px |
| Shadow | `0 2px 4px rgba(0,0,0,0.1)` |

**States**:

| State | Background | Additional |
|-------|------------|------------|
| Default | `#E85D04` | - |
| Hover | `#D14D00` | - |
| Pressed | `#B84300` | Scale 0.98 |
| Focused | `#E85D04` | 2px `#FFB703` outline |
| Disabled | `#E85D04` 50% | Cursor not-allowed |
| Loading | `#E85D04` | Spinner replaces text |

**Accessibility**:
- Role: `button`
- Keyboard: Enter/Space to activate
- Focus visible with 2px yellow outline
- Minimum contrast: 3:1 for large text

---

### 7.2 Text Input

```
Input States

Default:    ┌─────────────────────────────┐
            │  Search trucks...           │
            └─────────────────────────────┘

Focused:    ┌─────────────────────────────┐
            │  │                          │  ← 2px orange border
            └─────────────────────────────┘

Error:      ┌─────────────────────────────┐
            │  abc@                       │  ← 2px red border
            └─────────────────────────────┘
            ⚠ Please enter a valid email
```

**Specifications**:

| Property | Value |
|----------|-------|
| Height | 48px |
| Padding | 12px 16px |
| Border | 1px solid `#ADB5BD` |
| Border Radius | 8px |
| Background | `#FFFFFF` |
| Font | Inter Regular 16px |
| Placeholder | `#6C757D` |

**States**:

| State | Border Color | Background |
|-------|--------------|------------|
| Default | `#ADB5BD` | `#FFFFFF` |
| Focused | `#E85D04` (2px) | `#FFFFFF` |
| Error | `#C1121F` (2px) | `#FFF5F5` |
| Disabled | `#DEE2E6` | `#F8F9FA` |

---

### 7.3 Truck Card

```
┌─────────────────────────────────────────┐
│  ┌───────────────────────────────────┐  │
│  │                                   │  │
│  │         [Truck Photo]            │  │
│  │            200px                  │  │
│  │    ┌──────┐                      │  │
│  │    │ OPEN │  ← Status Badge      │  │
│  └───────────────────────────────────┘  │
│                                         │
│   Taco El Rey                      ♡    │
│   ★★★★☆ 4.2 (128 reviews)              │
│                                         │
│   📍 Building A - North Lot            │
│   ⏱  10-15 min wait                     │
│                                         │
│   🌱 🔥 Vegetarian, Spicy              │
│                                         │
│   ┌───────────────────────────────────┐ │
│   │          View Menu                │ │
│   └───────────────────────────────────┘ │
└─────────────────────────────────────────┘
```

**Specifications**:

| Property | Value |
|----------|-------|
| Width | 100% (container) |
| Padding | 16px |
| Background | `#FFFFFF` |
| Border Radius | 12px |
| Shadow | `elevation-2` |
| Image Height | 200px |

---

### 7.4 Bottom Tab Bar

```
┌─────────────────────────────────────────┐
│                                         │
│    🏠        📋        🛒        👤    │
│   Home     Browse     Cart    Profile   │
│   (●)                  (3)              │
│                                         │
└─────────────────────────────────────────┘

Height: 56px + safe area
Background: #FFFFFF
Border Top: 1px solid #DEE2E6
Active: #E85D04
Inactive: #6C757D
Badge: #E85D04 with white text
```

---

### 7.5 Bottom Sheet

```
Order Confirmation Bottom Sheet
┌─────────────────────────────────────────┐
│                ═══                      │ ← Drag handle
├─────────────────────────────────────────┤
│                                         │
│          ✓ Order Confirmed!             │
│                                         │
│   Order #TT-2026-0847                   │
│                                         │
│   1x Carnitas Taco            $3.50     │
│   2x Al Pastor Taco           $7.00     │
│   ─────────────────────────────         │
│   Total                      $10.50     │
│                                         │
│   Estimated pickup: 12:45 PM            │
│                                         │
│   ┌─────────────────────────────────┐   │
│   │        Track My Order           │   │
│   └─────────────────────────────────┘   │
│                                         │
└─────────────────────────────────────────┘

Border Radius: 16px (top corners)
Drag Handle: 36px x 4px, #DEE2E6, radius-full
Max Height: 90% of screen
Overlay: #000000 50%
```

---

### 7.6 Toast Notifications

```
Success:  ┌───────────────────────────────────┐
          │  ✓  Item added to cart         ✕ │
          └───────────────────────────────────┘
          Background: #2D6A4F, Text: #FFFFFF

Error:    ┌───────────────────────────────────┐
          │  ⚠  Payment failed. Try again. ✕ │
          └───────────────────────────────────┘
          Background: #C1121F, Text: #FFFFFF
```

**Specifications**:

| Property | Value |
|----------|-------|
| Height | 48px |
| Border Radius | 8px |
| Padding | 12px 16px |
| Position | Bottom, 16px from edge |
| Duration | 4000ms auto-dismiss |
| Animation | Slide up 200ms, fade out 200ms |

---

## 8. Screen Specifications

### 8.1 Home Screen (Map View)

```
┌─────────────────────────────────────────┐
│  ≡  TacoTracker 3000            🔔  👤 │
├─────────────────────────────────────────┤
│  ┌─────────────────────────────────┐    │
│  │  🔍 Search trucks, tacos...     │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │                                 │    │
│  │     ┌───┐                       │    │
│  │     │ B │  Building B           │    │
│  │     └───┘       🌮              │    │
│  │            Taco El Rey          │    │
│  │                                 │    │
│  │  ┌───┐                  ┌───┐   │    │
│  │  │ A │  Building A  🌮  │ C │   │    │
│  │  └───┘               ↑  └───┘   │    │
│  │              La Taqueria        │    │
│  │                                 │    │
│  │                    📍 You       │    │
│  └─────────────────────────────────┘    │
│                                         │
│  [ Building A ][ Building B ][ C ]      │
│                                         │
│  Nearby Now                   View All ▶│
│  ┌────────────────┐ ┌────────────────┐  │
│  │ ┌────────────┐ │ │ ┌────────────┐ │  │
│  │ │  [Photo]   │ │ │ │  [Photo]   │ │  │
│  │ │   OPEN     │ │ │ │   OPEN     │ │  │
│  │ └────────────┘ │ │ └────────────┘ │  │
│  │ Taco El Rey    │ │ La Taqueria    │  │
│  │ ★★★★☆ 4.2      │ │ ★★★★★ 4.8      │  │
│  │ 📍 Bldg A ⏱10m │ │ 📍 Bldg A ⏱5m │  │
│  └────────────────┘ └────────────────┘  │
│                                         │
├─────────────────────────────────────────┤
│   🏠      📋       🛒      👤          │
│  Home   Browse    Cart   Profile        │
│  (●)               (2)                  │
└─────────────────────────────────────────┘
```

**Interactions**:
- Map: Pinch to zoom, pan to move
- Truck pins: Tap to see preview card
- Building chips: Filter trucks by location
- Pull down: Refresh truck locations

---

### 8.2 Menu Screen

```
┌─────────────────────────────────────────┐
│  ←  Taco El Rey Menu              🔍   │
├─────────────────────────────────────────┤
│  [All] [Tacos] [Burritos] [Sides]      │
│                                         │
│  Filter: 🌱 Vegetarian  ✕               │
│                                         │
│  ─────────────────────────────────      │
│  TACOS                                  │
│  ─────────────────────────────────      │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ ┌──────┐  Carnitas Taco   $3.50│    │
│  │ │[img] │                        │    │
│  │ │ 80px │  Slow-roasted pork    │    │
│  │ └──────┘  with cilantro...     │    │
│  │           🔥                    │    │
│  │           ┌──────────────────┐ │    │
│  │           │  Add to Cart     │ │    │
│  │           └──────────────────┘ │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ ┌──────┐  Veggie Taco     $3.00│    │
│  │ │[img] │                        │    │
│  │ │ 80px │  Grilled peppers,     │    │
│  │ └──────┘  mushrooms, cheese    │    │
│  │           🌱                    │    │
│  │           ┌──────────────────┐ │    │
│  │           │  Add to Cart     │ │    │
│  │           └──────────────────┘ │    │
│  └─────────────────────────────────┘    │
│                                         │
├─────────────────────────────────────────┤
│  ┌─────────────────────────────────┐    │
│  │  🛒 View Cart (3)       $10.50  │    │
│  └─────────────────────────────────┘    │
└─────────────────────────────────────────┘
```

---

### 8.3 Cart & Checkout

```
┌─────────────────────────────────────────┐
│  ←  Your Cart                 Clear All │
├─────────────────────────────────────────┤
│                                         │
│  From: Taco El Rey                      │
│  📍 Building A - North Lot              │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ Carnitas Taco             $3.50 │    │
│  │ 🔥 Extra salsa                  │    │
│  │ ┌─────────────────────┐         │    │
│  │ │  [ − ]    1    [ + ]│         │    │
│  │ └─────────────────────┘         │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │ Al Pastor Taco (2)        $7.00 │    │
│  │ ┌─────────────────────┐         │    │
│  │ │  [ − ]    2    [ + ]│         │    │
│  │ └─────────────────────┘         │    │
│  └─────────────────────────────────┘    │
│                                         │
│  Special Instructions                   │
│  ┌─────────────────────────────────┐    │
│  │ No onions please                │    │
│  └─────────────────────────────────┘    │
│                                         │
│  ─────────────────────────────────      │
│  Subtotal                      $10.50   │
│  Service Fee                    $0.50   │
│  ─────────────────────────────────      │
│  Total                         $11.00   │
│                                         │
│  Payment                                │
│  ┌─────────────────────────────────┐    │
│  │ 💳 Globex Wallet        $142.50 │ ▶  │
│  └─────────────────────────────────┘    │
│                                         │
│  Add Tip                                │
│  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐   │
│  │ None │ │  15% │ │  20% │ │Custom│   │
│  └──────┘ └──────┘ └──────┘ └──────┘   │
│             (●)                         │
│                                         │
│  ┌─────────────────────────────────┐    │
│  │      Place Order - $12.65       │    │
│  └─────────────────────────────────┘    │
└─────────────────────────────────────────┘
```

---

## 9. User Flows

### Primary Flow: Quick Order

```
┌────────────┐    ┌────────────┐    ┌────────────┐
│            │    │            │    │            │
│  Open App  │───▶│  View Map  │───▶│   Select   │
│            │    │   (Home)   │    │   Truck    │
└────────────┘    └────────────┘    └────────────┘
                                          │
                                          ▼
┌────────────┐    ┌────────────┐    ┌────────────┐
│            │    │            │    │            │
│  Receive   │◀───│  Confirm   │◀───│  Add to    │
│  Confirm.  │    │   & Pay    │    │   Cart     │
└────────────┘    └────────────┘    └────────────┘

Target: 3 taps for repeat order
Target: < 60 seconds for new order
```

### Reorder Flow

```
┌────────────┐    ┌────────────┐    ┌────────────┐
│            │    │            │    │            │
│  Open App  │───▶│ Tap "Order │───▶│  Confirm   │
│            │    │   Again"   │    │   & Pay    │
└────────────┘    └────────────┘    └────────────┘

Target: 2 taps to reorder
```

---

## 10. Animation Specifications

### Transition Timing

| Transition | Duration | Easing | Description |
|------------|----------|--------|-------------|
| Page push | 300ms | `ease-out` | Slide from right |
| Page pop | 250ms | `ease-in` | Slide to right |
| Modal open | 350ms | `ease-out` | Slide from bottom |
| Modal close | 300ms | `ease-in` | Slide to bottom |
| Toast appear | 200ms | `ease-out` | Slide up + fade |
| Toast dismiss | 200ms | `ease-in` | Fade out |
| State change | 150ms | `ease` | Color/opacity changes |

### Microinteractions

**Favorite Heart**:
```
Frame 1: ♡ (outline, scale 1.0)
Frame 2: ♥ (filled, scale 1.3) - 100ms, spring easing
Frame 3: ♥ (filled, scale 1.0) - 150ms
Color: #E85D04
```

**Add to Cart**:
```
┌──────────┐  →  ┌──────────┐  →  ┌──────────┐
│ + Add    │     │   ✓ 1    │     │  − 1 +   │
└──────────┘     └──────────┘     └──────────┘
                 (flash green)    (final state)
Duration: 200ms
```

**Loading Spinner**:
- Taco icon rotating 360 degrees
- Duration: 800ms per rotation
- Easing: linear
- Loop: infinite

### Reduced Motion

When `prefers-reduced-motion` is enabled:
- Disable decorative animations
- Use instant transitions (duration: 0)
- Keep essential feedback (loading states)
- Replace slide transitions with fades

---

## 11. Error States

### Network Error

```
┌─────────────────────────────────────────┐
│                                         │
│              📡                         │
│                                         │
│       Connection Lost                   │
│                                         │
│   We can't reach the taco trucks.       │
│   Check your connection and try again.  │
│                                         │
│   ┌─────────────────────────────────┐   │
│   │           Try Again             │   │
│   └─────────────────────────────────┘   │
│                                         │
└─────────────────────────────────────────┘
```

### Payment Error

```
┌─────────────────────────────────────────┐
│                                         │
│              ⚠                          │
│                                         │
│       Payment Declined                  │
│                                         │
│   Your Globex Wallet has insufficient   │
│   funds for this order.                 │
│                                         │
│   ┌─────────────────────────────────┐   │
│   │       Add Funds to Wallet       │   │
│   └─────────────────────────────────┘   │
│                                         │
│   ┌─────────────────────────────────┐   │
│   │     Try Different Payment       │   │
│   └─────────────────────────────────┘   │
│                                         │
└─────────────────────────────────────────┘
```

### Error Message Guidelines

| Error Type | Pattern |
|------------|---------|
| Network | "[What happened]. Check your connection and try again." |
| Validation | "Please enter a valid [field name]" |
| Payment | "Payment didn't go through. [Reason if known]" |
| Sold out | "This item just sold out." |
| Timeout | "This is taking too long. Please try again." |

---

## 12. Empty States

### No Nearby Trucks

```
┌─────────────────────────────────────────┐
│                                         │
│                 🌮                      │
│                zzz                      │
│                                         │
│        No Trucks Nearby                 │
│                                         │
│   All taco trucks are at other          │
│   buildings right now. Check back       │
│   around lunch time!                    │
│                                         │
│   ┌─────────────────────────────────┐   │
│   │        View All Trucks          │   │
│   └─────────────────────────────────┘   │
│                                         │
│   ┌─────────────────────────────────┐   │
│   │    Notify When Trucks Arrive    │   │
│   └─────────────────────────────────┘   │
│                                         │
└─────────────────────────────────────────┘
```

### Empty Cart

```
┌─────────────────────────────────────────┐
│                                         │
│                 🛒                      │
│                                         │
│         Your Cart is Empty              │
│                                         │
│   Find a truck and add some             │
│   delicious tacos to get started!       │
│                                         │
│   ┌─────────────────────────────────┐   │
│   │          Find Trucks            │   │
│   └─────────────────────────────────┘   │
│                                         │
└─────────────────────────────────────────┘
```

### No Search Results

```
┌─────────────────────────────────────────┐
│                                         │
│                 🔍                      │
│                                         │
│         No Results Found                │
│                                         │
│   We couldn't find anything matching    │
│   "burritozzz". Try another search.     │
│                                         │
│   ┌─────────────────────────────────┐   │
│   │          Clear Search           │   │
│   └─────────────────────────────────┘   │
│                                         │
└─────────────────────────────────────────┘
```

---

## 13. Loading States

### Skeleton Screen (Truck List)

```
┌─────────────────────────────────────────┐
│  ┌───────────────────────────────────┐  │
│  │░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░│  │
│  │░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░│  │
│  │░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░│  │
│  └───────────────────────────────────┘  │
│                                         │
│   ░░░░░░░░░░░░░░░░░                     │
│   ░░░░░░░░░░░░░░░░░░░░░░░░             │
│                                         │
│   ░░░░░░░░░░                            │
│                                         │
└─────────────────────────────────────────┘

Skeleton color: #DEE2E6
Shimmer highlight: #F8F9FA
Animation: Shimmer left-to-right, 1500ms, infinite
```

### Inline Loading

- Buttons: Replace text with spinner, maintain width
- Pull-to-refresh: Native spinner at top
- Pagination: Spinner below last item

---

## 14. Accessibility Checklist

### WCAG 2.1 AA Compliance

| Criterion | Status | Notes |
|-----------|--------|-------|
| 1.1.1 Non-text Content | ✓ | Alt text on all images |
| 1.3.1 Info and Relationships | ✓ | Proper heading hierarchy |
| 1.4.3 Contrast (Minimum) | ✓ | All text 4.5:1+ |
| 1.4.11 Non-text Contrast | ✓ | UI components 3:1+ |
| 2.1.1 Keyboard | ✓ | Full keyboard navigation |
| 2.4.3 Focus Order | ✓ | Logical tab order |
| 2.4.7 Focus Visible | ✓ | 2px yellow outline |
| 4.1.2 Name, Role, Value | ✓ | ARIA labels on all controls |

### Screen Reader Announcements

| Element | Announcement |
|---------|--------------|
| Truck card | "[Truck name], [rating] stars, [wait time]" |
| Favorite button | "Add [truck] to favorites" / "Remove from favorites" |
| Cart badge | "[count] items in cart" |
| Order status | "Order status: [status]" (live region) |

### Touch Targets

- Minimum: 48x48dp for all interactive elements
- Spacing: 8px minimum between targets

---

## 15. Platform Adaptations

### iOS vs Android

| Feature | iOS | Android |
|---------|-----|---------|
| Navigation | Native nav bar | Material top bar |
| Back gesture | Swipe from edge | System back button |
| Share | iOS Share Sheet | Android Share Sheet |
| Haptics | UIImpactFeedback | VibrationEffect |
| Maps | Apple Maps | Google Maps |
| Bottom nav | 56px + safe area | 56px |

### iOS-Specific

- Support Dynamic Type up to 200%
- Use SF Symbols for system icons
- Handle notch/Dynamic Island
- Safe area insets on bottom sheet

### Android-Specific

- Support Material You color extraction
- Handle system navigation bar
- Edge-to-edge display
- Predictive back gesture (Android 14+)

---

## 16. Haptic Feedback

| Trigger | iOS | Android |
|---------|-----|---------|
| Button tap | `.light` impact | `EFFECT_CLICK` |
| Order confirmed | `.success` notification | `EFFECT_HEAVY_CLICK` |
| Error | `.error` notification | `EFFECT_DOUBLE_CLICK` |
| Pull-to-refresh | `.medium` impact | `EFFECT_TICK` |
| Long press | `.rigid` impact | `EFFECT_HEAVY_CLICK` |

---

## 17. Voice and Tone

### UI Copy Guidelines

| Context | Tone | Example |
|---------|------|---------|
| Success | Celebratory, brief | "You're all set!" |
| Errors | Apologetic, helpful | "Sorry, we hit a snag..." |
| Empty states | Lighthearted | "No tacos here... yet!" |
| Waiting | Reassuring | "Hang tight, almost there..." |

### Button Labels

| Do | Don't |
|----|-------|
| Order Now | Submit |
| Add to Cart | Add |
| View Menu | Menu |
| Try Again | Retry |

---

## 18. Design Tokens (JSON)

```json
{
  "color": {
    "primary": { "value": "#E85D04" },
    "primaryDark": { "value": "#D14D00" },
    "primaryLight": { "value": "#FF7B2E" },
    "secondary": { "value": "#FFB703" },
    "success": { "value": "#2D6A4F" },
    "warning": { "value": "#9A6700" },
    "error": { "value": "#C1121F" },
    "info": { "value": "#0077B6" },
    "neutral900": { "value": "#212529" },
    "neutral700": { "value": "#495057" },
    "neutral500": { "value": "#6C757D" },
    "neutral300": { "value": "#ADB5BD" },
    "neutral200": { "value": "#DEE2E6" },
    "neutral100": { "value": "#F8F9FA" },
    "neutral0": { "value": "#FFFFFF" }
  },
  "typography": {
    "fontFamily": {
      "primary": { "value": "Inter" },
      "mono": { "value": "JetBrains Mono" }
    },
    "fontSize": {
      "display": { "value": "32px" },
      "h1": { "value": "28px" },
      "h2": { "value": "24px" },
      "h3": { "value": "20px" },
      "body": { "value": "14px" },
      "caption": { "value": "12px" }
    }
  },
  "spacing": {
    "1": { "value": "4px" },
    "2": { "value": "8px" },
    "3": { "value": "12px" },
    "4": { "value": "16px" },
    "6": { "value": "24px" },
    "8": { "value": "32px" }
  },
  "borderRadius": {
    "sm": { "value": "4px" },
    "md": { "value": "8px" },
    "lg": { "value": "12px" },
    "xl": { "value": "16px" },
    "full": { "value": "9999px" }
  },
  "animation": {
    "fast": { "value": "150ms" },
    "normal": { "value": "300ms" },
    "slow": { "value": "500ms" }
  }
}
```

---

## 19. Usability Testing Plan

### Test Scenarios

| Scenario | Task | Success Criteria |
|----------|------|------------------|
| Find truck | Locate Taco El Rey on map | < 10 seconds |
| Browse menu | Find vegetarian options | < 15 seconds, correct filter |
| Place order | Order 2 tacos and pay | < 60 seconds, order confirmed |
| Reorder | Repeat last order | < 3 taps, < 20 seconds |
| Track order | Check order status | Status visible immediately |

### Accessibility Testing

- VoiceOver (iOS): Complete ordering flow
- TalkBack (Android): Complete ordering flow
- Keyboard only: All functions accessible
- 200% zoom: All content readable
- Color blindness simulation: All info perceivable

---

## Appendix A: Glossary

| Term | Definition |
|------|------------|
| BEM | Block Element Modifier - CSS naming convention |
| Design Token | Named entity storing visual design attributes |
| WCAG | Web Content Accessibility Guidelines |
| Touch Target | Interactive area responding to touch (min 48x48dp) |
| Semantic Color | Color with assigned meaning (success, error, etc.) |
| Safe Area | Screen region not obscured by device features |
| Skeleton Screen | Loading placeholder mimicking content shape |

---

## Appendix B: Resources

**Design Files**:
- Figma: `figma.com/file/tacotracker-3000`
- Component Library: `github.com/globex/tacotracker-ui`

**External References**:
- Apple HIG: `developer.apple.com/design`
- Material Design: `material.io/design`
- WCAG 2.1: `w3.org/WAI/WCAG21`

---

*This UI/UX Design Document is maintained by the Globex Digital Experience Team.*

**Document Classification**: Internal Use Only
**Last Review**: January 22, 2026
**Next Review**: April 2026
