---
name: Sr. Diente Clinical OS
colors:
  surface: '#faf8ff'
  surface-dim: '#d2d9f4'
  surface-bright: '#faf8ff'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f2f3ff'
  surface-container: '#eaedff'
  surface-container-high: '#e2e7ff'
  surface-container-highest: '#dae2fd'
  on-surface: '#131b2e'
  on-surface-variant: '#3f4850'
  inverse-surface: '#283044'
  inverse-on-surface: '#eef0ff'
  outline: '#707881'
  outline-variant: '#bfc7d2'
  surface-tint: '#006398'
  primary: '#006194'
  on-primary: '#ffffff'
  primary-container: '#007bb9'
  on-primary-container: '#fdfcff'
  inverse-primary: '#93ccff'
  secondary: '#006a61'
  on-secondary: '#ffffff'
  secondary-container: '#86f2e4'
  on-secondary-container: '#006f66'
  tertiary: '#006b2c'
  on-tertiary: '#ffffff'
  tertiary-container: '#00873a'
  on-tertiary-container: '#f7fff2'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#cce5ff'
  primary-fixed-dim: '#93ccff'
  on-primary-fixed: '#001d31'
  on-primary-fixed-variant: '#004b73'
  secondary-fixed: '#89f5e7'
  secondary-fixed-dim: '#6bd8cb'
  on-secondary-fixed: '#00201d'
  on-secondary-fixed-variant: '#005049'
  tertiary-fixed: '#7ffc97'
  tertiary-fixed-dim: '#62df7d'
  on-tertiary-fixed: '#002109'
  on-tertiary-fixed-variant: '#005320'
  background: '#faf8ff'
  on-background: '#131b2e'
  surface-variant: '#dae2fd'
typography:
  headline-xl:
    fontFamily: Outfit
    fontSize: 36px
    fontWeight: '700'
    lineHeight: 44px
    letterSpacing: -0.02em
  headline-xl-mobile:
    fontFamily: Outfit
    fontSize: 28px
    fontWeight: '700'
    lineHeight: 36px
    letterSpacing: -0.01em
  headline-lg:
    fontFamily: Outfit
    fontSize: 28px
    fontWeight: '600'
    lineHeight: 36px
    letterSpacing: -0.015em
  headline-lg-mobile:
    fontFamily: Outfit
    fontSize: 24px
    fontWeight: '600'
    lineHeight: 32px
    letterSpacing: -0.01em
  headline-md:
    fontFamily: Outfit
    fontSize: 22px
    fontWeight: '600'
    lineHeight: 28px
  headline-sm:
    fontFamily: Outfit
    fontSize: 18px
    fontWeight: '600'
    lineHeight: 24px
  body-lg:
    fontFamily: Manrope
    fontSize: 16px
    fontWeight: '400'
    lineHeight: 24px
  body-md:
    fontFamily: Manrope
    fontSize: 14px
    fontWeight: '400'
    lineHeight: 20px
  body-sm:
    fontFamily: Manrope
    fontSize: 12px
    fontWeight: '400'
    lineHeight: 16px
  label-lg:
    fontFamily: Manrope
    fontSize: 14px
    fontWeight: '600'
    lineHeight: 20px
  label-md:
    fontFamily: Manrope
    fontSize: 12px
    fontWeight: '600'
    lineHeight: 16px
    letterSpacing: 0.02em
  label-sm:
    fontFamily: Manrope
    fontSize: 10px
    fontWeight: '700'
    lineHeight: 14px
    letterSpacing: 0.04em
  data-mono:
    fontFamily: Manrope
    fontSize: 14px
    fontWeight: '600'
    lineHeight: 20px
rounded:
  sm: 0.25rem
  DEFAULT: 0.5rem
  md: 0.75rem
  lg: 1rem
  xl: 1.5rem
  full: 9999px
spacing:
  gutter: 1rem
  gutter-desktop: 1.5rem
  margin: 1rem
  margin-tablet: 1.5rem
  margin-desktop: 2rem
  space-xs: 0.25rem
  space-sm: 0.5rem
  space-md: 1rem
  space-lg: 1.5rem
  space-xl: 2rem
---

## Brand & Style

This design system establishes a clinical workspace tailored for modern Mexican dental practices, balancing medical precision with human empathy. The UI serves three primary profiles with diverging workflow pressures: Receptionists operating under rapid telephone intake and patient arrival flow; Dentists requiring sterilized visual focus, instant tooth-level chart readability, and medical alert priority; and Practice Owners needing immediate financial visibility in MXN and multi-chair resource distribution.

The visual style is **Clinical Modernism**—an evolution of functional minimalism that integrates clean white surfaces, subtle structural borders, and purposeful chromatic signaling. It departs from sterile, intimidating medical software through open geometry, warm micro-accents, and legible typography that reduces cognitive load during 10-hour clinic operations.

Key brand attributes:
- **Hygienic & Luminous:** Dominated by pristine clinical whites, air, and precise light-blue tints that evoke a sterilized, contemporary dental operatory.
- **Systematic & Trustworthy:** Dense tabular data, chair schedules, and currency notations are organized with absolute vertical alignment and geometric clarity.
- **Empathetic & Accessible:** Non-punitive alert patterns, legible forms for quick record updates, and humanized touchpoints for patient communication.

## Colors

The palette relies on clinical cleanliness with high-contrast functional accents:

- **Primary (`#0284C7` / Sky 600):** Drives central actions, active appointment statuses, and navigational anchors. Paired with `#0369A1` (Hover/Active states) and `#E0F2FE` (Soft selection fills, focus rings, and badge backgrounds).
- **Secondary (`#0D9488` / Teal 600, `#14B8A6` / Teal 500):** Applied to restorative treatments, preventative care tags, hygiene indicators, and clinical wellness metrics. Accompanied by `#CCFBF1` for restorative badges and calm banner backgrounds.
- **Neutrals:**
  - Canvas / Operatory Base: `#FFFFFF` (Pure white cards and elevated panels) and `#F8FAFC` (Slate 50 app background).
  - Outlines & Dividers: `#E2E8F0` (Slate 200 structural borders).
  - High-Emphasis Typography: `#0F172A` (Slate 900 carbon for primary headers, patient names, and currency figures).
  - Medium-Emphasis Typography: `#475569` (Slate 600 for metadata, table column headers, and secondary labels).
- **Operational & Clinical Semantics:**
  - **Success / Healthy (`#16A34A`):** Confirmed appointments, paid ledger balances, available operatory chairs, and active Google Calendar sync. Tint: `#DCFCE7`.
  - **Warning / Ongoing (`#D97706`):** Pending approvals, patient currently in chair ("En atención"), pending cloud sync, and partial payments. Tint: `#FEF3C7`.
  - **Critical / Medical Alert (`#DC2626`):** Allergies, systemic conditions, canceled appointments, sync errors, and overdue patient debts. Tint: `#FEE2E2`.
  - **Improvement / Innovation Tag (`#6366F1`):** "Propuesta de mejora" features and workflow suggestions. Tint: `#EEF2FF`.

All text combinations against their complementary backgrounds satisfy WCAG AA 4.5:1 minimum contrast ratios for desktop and tablet screens.

## Typography

The pairing combines **Outfit** for interface navigation, identity, and section framing with **Manrope** for clinical forms, odontograms, schedule grids, and ledger ledgers:

- **Outfit (Display & Headers):** Geometric yet open and friendly curves reduce anxiety. Used exclusively in application titles, patient overview names, and primary modal titles.
- **Manrope (Data, Forms, Tables):** Engineered with legible numerals and robust kerning. Numerical figures for Mexican Pesos (`$1,850.00 MXN`) and military/standard appointment times (`09:30 - 10:15`) must employ tabular figures (`font-variant-numeric: tabular-nums`) to ensure zero-jitter alignment down financial tables.
- **Hierarchy Rules:**
  - Page titles: `headline-lg` (Desktop) scaling to `headline-lg-mobile` (Mobile).
  - Patient chart tabs, chair selectors, and table headers: `label-md` uppercase.
  - Odontogram tooth numbering and billing line entries: `data-mono` using Manrope semi-bold.

## Layout & Spacing

This design system uses a fluid 12-column grid system bounded inside desktop canvases with compact gutters to optimize dense horizontal dental workflows (such as parallel multi-chair agendas).

### Breakpoints & Fluid Grid
- **Mobile (< 768px):** Single-column layout. Margin `1rem`, component gap `0.75rem`. Tab bars become sticky bottom sheets; horizontal scrolling panels house the chair selector.
- **Tablet (768px - 1024px):** 6-column grid. Margin `1.5rem`, gutter `1rem`. Optimized for iPad/tablet operatory mounts with touch-friendly 44px tap targets.
- **Desktop (> 1024px):** 12-column grid. Max content width `1600px` centered. Margin `2rem`, gutter `1.5rem`. Side-by-side patient clinical file with simultaneous calendar and treatment plan ledger.

### Layout Philosophy
- **Split-Screen Ergonomics:** Desktop clinical views pin the Patient Identity & Medical Alert Bar at the top, allocating 65% width to the Odontogram/Agenda and 35% to the contextual Action Ledger (MXN balances, consent signatures, Google Sync status).
- **Time Slots:** Calendar grid rows snap to a strict 15-minute operational rhythm with 1rem minimum height per time increment.

## Elevation & Depth

Visual hierarchy avoids heavy drop shadows, which can feel dirty in a clinical context. Depth is achieved via crisp borders, pure white surface elevation, and soft blue-tinted ambient glows:

- **Level 0 (App Canvas):** `#F8FAFC`. Base layer for global application navigation and workspace background.
- **Level 1 (Cards & Workbenches):** `#FFFFFF` background with a 1px solid border in `#E2E8F0`. Shadow: `0 1px 3px 0 rgba(15, 23, 42, 0.04)`.
- **Level 2 (Dropdowns, Role Selectors, Active Chair Cards):** `#FFFFFF` background with a 1px solid border in `#CBD5E1`. Shadow: `0 4px 6px -1px rgba(15, 23, 42, 0.07), 0 2px 4px -2px rgba(15, 23, 42, 0.04)`.
- **Level 3 (Modals, Clinical Alerts, Floating Speed-Dials):** `#FFFFFF` with a 1px solid border in `#E2E8F0`. Shadow: `0 20px 25px -5px rgba(2, 132, 199, 0.08), 0 8px 10px -6px rgba(15, 23, 42, 0.04)`.
- **Emergency / Medical Alert Elevation:** Sticky persistent banner at screen top using `#FEE2E2` fill, `#DC2626` crisp 1px border, and an ambient red pulse indicator (`box-shadow: 0 0 0 3px rgba(220, 38, 38, 0.15)`).

## Shapes

The design system uses **Rounded (Level 2)** geometry:
- Standard inputs, buttons, and chair chips: `0.5rem` (8px).
- Surface cards, clinical tables, and modal containers: `1rem` (16px).
- Toast messages, medical warning tags, and floating role pickers: `1.5rem` (24px).
- Full Pill (`9999px`): Reserved strictly for dynamic status badges (e.g., "Confirmada", "Sincronizado", "En atención").

This level softens clinical interactions without compromising the architectural order needed for high-density tables and medical charts.

## Components

### Buttons
- **Primary:** Solid `#0284C7` with white text, font `label-lg`. Hover: `#0369A1`. Active scale down to 98%. Focus ring: 3px `#E0F2FE`.
- **Secondary (Health/Action):** Solid `#0D9488` with white text. Hover: `#0F766E`. Used for "Completar Tratamiento" or "Registrar Cobro".
- **Outline / Neutral:** 1px `#E2E8F0` border, `#FFFFFF` background, `#0F172A` text. Hover: `#F8FAFC`.

### Medical Alert Badges (Badges de Alerta Médica)
- **Visuals:** High-urgency pills. Red background (`#FEE2E2`), deep red text (`#991B1B`), left-bordered by a 2px solid `#DC2626`.
- **Behavior:** Always include an alert icon (e.g., triangle or pulse). Clicking opens a non-dismissible flyout detailing allergies (e.g., "Alergia a la Penicilina"), hypertension, or systemic conditions.

### Google Calendar Sync Indicator
- **States:**
  - *Sincronizado:* Icon check in `#16A34A`, background `#DCFCE7`, label "Sync OK (CDMX)".
  - *Sync Pendiente:* Icon refresh with rotational indicator in `#D97706`, background `#FEF3C7`, label "Sincronizando...".
  - *Error de Sync:* Icon alert in `#DC2626`, background `#FEE2E2`, label "Reconectar Google". Emits an immediate toast warning.

### Role Selector (Recepción, Doctor, Dueño/Admin)
- **Visuals:** Segmented control or persistent header dropdown.
- **Styling:** Pill container with `#F1F5F9` background. Selected role animates with a `#FFFFFF` card, subtle shadow, and color tag:
  - *Recepción:* Sky Blue icon (`#0284C7`)
  - *Doctor:* Teal health icon (`#0D9488`)
  - *Dueño/Admin:* Violet/Indigo crown icon (`#6366F1`) with global revenue visibility toggles.

### Chair Selector (Sillón 1, Sillón 2, Sillón 3)
- **Visuals:** Horizontal segment bar displayed at the calendar summit.
- **State Details:** Each tab highlights current occupancy:
  - Empty: `#F8FAFC` background with green dot (`#16A34A`, "Disponible").
  - Occupied: `#E0F2FE` background with sky blue indicator ("En uso - Dr. Morales").
  - Muted: Disabled if chair is closed or in maintenance.

### Financial Breakdown (Desglose de Saldos en MXN)
- **Dual Display Card:**
  - *Total General:* Large display figure (`headline-md`, `#0F172A`), formatted strictly as `$XX,XXX.XX MXN`.
  - *Por Tratamiento:* Collapsible accordion row showing treatment name (e.g., "Endodoncia Molar 36"), unit cost, insurance/copay, amount paid, and outstanding balance in bold `#DC2626` (if overdue) or `#16A34A` (if settled).

### "Propuesta de Mejora" Badges
- **Visuals:** Soft indigo pill (`#EEF2FF` fill, `#4F46E5` text, 1px `#C7D2FE` border).
- **Placement:** Placed alongside newly integrated operational workflows—such as automated WhatsApp appointment confirmations, automatic chair reassignment, and smart ledger reconciliation.

### Inputs & Form Fields
- Height 40px, font `body-md`. Background `#FFFFFF`, 1px border `#CBD5E1`.
- Focus state: Border `#0284C7` with 3px `#E0F2FE` outer glow.
- Labels: `label-md` in `#475569`, positioned above input with 4px margin.