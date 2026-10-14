# CampusSphere - Design System & UI/UX Guidelines

## 1. Design Philosophy & Aesthetic

CampusSphere embodies a **collegiate, modern, and high-clarity** interface. It balances an energetic, youthful campus vibe with the structure and legibility demanded by academic institutions.

- **Clarity Over Clutter**: Generous whitespace, distinct visual hierarchy, and intuitive grouped cards.
- **Accessible & Inclusive**: High-contrast ratios meeting WCAG 2.1 AA standards, responsive tap targets, and comprehensive keyboard focus rings.
- **Theme Versatility**: First-class support for both Light and Dark modes with zero flash of unstyled content (FOUC).

---

## 2. Color Palette & Theme Tokens

### 2.1 Primary Brand Colors
- **Campus Blue (Primary)**: Symbolizes institutional trust, intellect, and connectivity.
- **Electric Indigo (Accent)**: Sparks energy for events, clubs, and highlighted actions.
- **Amber Gold (Highlight)**: Emphasizes deadlines, pinned announcements, and badges.

### 2.2 Semantic Color Tokens

| Token | Light Mode (HEX) | Dark Mode (HEX) | Purpose |
| :--- | :--- | :--- | :--- |
| **`--primary`** | `#2563EB` (Blue 600) | `#3B82F6` (Blue 500) | Primary CTAs, active nav links, brand accents |
| **`--primary-foreground`** | `#FFFFFF` | `#FFFFFF` | Text on primary elements |
| **`--secondary`** | `#F1F5F9` (Slate 100) | `#1E293B` (Slate 800) | Secondary buttons, chips, tab backgrounds |
| **`--secondary-foreground`**| `#0F172A` (Slate 900) | `#F8FAFC` (Slate 50) | Text on secondary elements |
| **`--accent`** | `#6366F1` (Indigo 500) | `#818CF8` (Indigo 400) | Club badges, interactive highlights |
| **`--background`** | `#F8FAFC` (Slate 50) | `#0B0F19` (Deep Charcoal) | Application canvas background |
| **`--surface` / `--card`** | `#FFFFFF` | `#131B2E` (Navy Slate) | Cards, modals, dropdown menus |
| **`--muted`** | `#F1F5F9` (Slate 100) | `#1E293B` (Slate 800) | Inactive toggles, subtle backgrounds |
| **`--muted-foreground`** | `#64748B` (Slate 500) | `#94A3B8` (Slate 400) | Secondary metadata, timestamps, captions |
| **`--border`** | `#E2E8F0` (Slate 200) | `#243049` (Border Slate) | Card borders, table dividers |
| **`--success`** | `#16A34A` (Green 600) | `#22C55E` (Green 500) | RSVP confirmed, item resolved, verified badge |
| **`--warning`** | `#D97706` (Amber 600) | `#F59E0B` (Amber 500) | Pending requests, capacity warnings |
| **`--destructive`** | `#DC2626` (Red 600) | `#EF4444` (Red 500) | Delete actions, lost item alerts, errors |

---

## 3. Typography & Hierarchy

### 3.1 Font Families
- **Primary Headings**: `Plus Jakarta Sans`, `Inter`, sans-serif (Clean, modern geometry with friendly humanist undertones).
- **Body & Interface**: `Inter`, system-ui, sans-serif (Engineered for optimal digital screen readability).
- **Monospace / Code**: `JetBrains Mono`, `Fira Code`, monospace (For course codes, timestamps, ticket numbers).

### 3.2 Type Scale

| Style | Size | Line Height | Weight | Tailwind Class | Usage |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Display** | 36px (2.25rem) | 40px (1.1) | 800 (Extrabold) | `text-4xl font-extrabold tracking-tight` | Landing hero title |
| **Heading 1** | 30px (1.875rem)| 36px (1.2) | 700 (Bold) | `text-3xl font-bold tracking-tight` | Major section headers (Clubs, Events) |
| **Heading 2** | 24px (1.5rem)  | 32px (1.3) | 600 (Semibold) | `text-2xl font-semibold` | Page subheadings, modal titles |
| **Heading 3** | 20px (1.25rem) | 28px (1.4) | 600 (Semibold) | `text-xl font-semibold` | Card headers, group titles |
| **Heading 4** | 16px (1.0rem)  | 24px (1.5) | 600 (Semibold) | `text-base font-semibold` | Sub-card headers, list item titles |
| **Body Large**| 16px (1.0rem)  | 24px (1.5) | 400 (Regular) | `text-base font-normal` | Lead paragraphs, announcement text |
| **Body Base** | 14px (0.875rem)| 20px (1.5) | 400 / 500 | `text-sm font-normal` | Default body copy, post text, descriptions |
| **Caption**   | 12px (0.75rem) | 16px (1.3) | 500 (Medium) | `text-xs font-medium text-muted-foreground` | Timestamps, badge labels, metadata |

---

## 4. Spacing, Elevation & Layout Grid

### 4.1 Spacing & Radius
- **Grid Baseline**: 4px base increment (8px, 12px, 16px, 24px, 32px, 48px, 64px).
- **Standard Border Radii**:
  - `rounded-md`: Form inputs, buttons (`6px` / `0.375rem`).
  - `rounded-lg`: Standard cards, modals (`8px` / `0.5rem`).
  - `rounded-xl`: Featured promotional cards, hero banners (`12px` / `0.75rem`).
  - `rounded-full`: Avatars, status pills, reaction chips.

### 4.2 Elevation & Shadows
- **Card Default**: `shadow-sm border border-border` (Subtle boundary, non-distracting).
- **Card Hover**: `hover:shadow-md hover:border-primary/30 transition-all duration-200`.
- **Dropdown & Popover**: `shadow-lg border border-border`.
- **Modal Overlay**: `shadow-2xl` with backdrop blur `backdrop-blur-sm bg-black/40`.

---

## 5. Key Component Specifications

### 5.1 Buttons
- **Primary**: High-contrast blue background with white text, subtle hover lift (`bg-primary hover:bg-primary/90 text-primary-foreground`).
- **Secondary**: Soft slate surface with crisp text (`bg-secondary hover:bg-secondary/80 text-secondary-foreground`).
- **Outline**: Clean 1px border with background hover wash (`border border-border hover:bg-muted text-foreground`).
- **Ghost**: Zero border, subtle hover tint for navigation and icon buttons (`hover:bg-muted text-foreground`).

### 5.2 Status Badges & Pills
- Small, rounded-full chips with icon support:
  - **Verified Faculty**: Blue border and text with subtle blue background.
  - **Club Lead**: Purple/Indigo tint.
  - **Urgent / Exam Notice**: Amber/Red accent.
  - **Marketplace Available**: Emerald green dot + text.

### 5.3 Form Inputs
- 40px height for standard touch friendliness.
- Focus state: `focus-visible:ring-2 focus-visible:ring-primary focus-visible:ring-offset-2`.
- Error state: `border-destructive focus-visible:ring-destructive` with inline validation text beneath.

---

## 6. Accessibility & Responsiveness Checklist

- [x] Minimum 4.5:1 contrast ratio for normal body text against backgrounds.
- [x] Minimum 3:1 contrast ratio for large text ($> 18\text{pt}$ or bold $14\text{pt}$).
- [x] Clear focus ring indicators on all interactive elements (buttons, links, inputs).
- [x] Semantic HTML tags (`<main>`, `<nav>`, `<aside>`, `<article>`, `<header>`).
- [x] Touch target minimum dimensions $\ge 44 \times 44\text{px}$ on mobile screens.
