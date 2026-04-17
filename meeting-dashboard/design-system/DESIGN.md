# Meeting Intelligence — Design System

> Extracted from `outputs/meeting-dashboard.html`  
> Brand: Advisor AI Partners — blue / teal / gold  
> Mode: Light only (never dark)

---

## Font Stack

| Role | Family | Google Fonts CDN |
|------|--------|-----------------|
| Body / UI | Geist | `family=Geist:wght@300;400;500;600;700` |
| Mono / Labels | Geist Mono | `family=Geist+Mono:wght@300;400;500;600` |

**CDN link:**
```html
<link href="https://fonts.googleapis.com/css2?family=Geist:wght@300;400;500;600;700&family=Geist+Mono:wght@300;400;500;600&display=swap" rel="stylesheet" />
```

---

## Color Palette

| Token | Hex | Usage |
|-------|-----|-------|
| `--base` | `#F0F4F8` | Page background — cool off-white |
| `--surface` | `#FFFFFF` | Cards, nav bar, detail pane |
| `--surface-alt` | `#F8FAFC` | Secondary surfaces, inline stat blocks |
| `--ink` | `#0F172A` | Primary text |
| `--ink-dim` | `#64748B` | Secondary text, summaries |
| `--ink-faint` | `#94A3B8` | Labels, timestamps, placeholders |
| `--border` | `rgba(15,23,42,0.08)` | Default borders — subtle |
| `--border-hover` | `rgba(15,23,42,0.18)` | Hover / focus borders |
| `--blue` | `#1D4ED8` | Primary brand — active states, CTAs |
| `--blue-light` | `#EFF6FF` | Fireflies badge background |
| `--blue-mid` | `#BFDBFE` | Fireflies badge border |
| `--teal` | `#0D9488` | Secondary brand — Notion badges, other-party actions |
| `--teal-light` | `#F0FDFA` | Notion badge background |
| `--teal-mid` | `#99F6E4` | Notion badge border |
| `--gold` | `#B45309` | Tertiary brand — manual entries, warnings |
| `--gold-light` | `#FFFBEB` | Manual badge background |
| `--gold-mid` | `#FDE68A` | Manual badge border |

---

## Component Inventory

| Class | Description | Usage |
|-------|-------------|-------|
| `.section-header` | Mono label + horizontal rule extending right | Section titles throughout (Overview, Meetings, Summary, Action Items) |
| `.hairline` | 1px full-width ink divider at 7% opacity | Visual breaks between major sections |
| `.stat-card` | White card with border radius 14px, hover shadow + border lift | KPI stat cards in overview row |
| `.meeting-row` | Clickable white card with left blue bar on active, hover lift | Each meeting entry in the list |
| `.badge` | Pill badge with source-specific color scheme | Fireflies / Notion / Manual source labels |
| `.badge-fireflies` | Blue color scheme | Fireflies.ai meetings |
| `.badge-notion` | Teal color scheme | Notion meetings |
| `.badge-manual` | Gold color scheme | Manually entered meetings |
| `.filter-pill` | Toggle button — white default, solid blue when active | All / Fireflies / Notion filter tabs |
| `.search-input` | Mono font, icon-padded, blue focus border | Meeting search field |
| `.detail-pane` | Fixed right panel, spring animation, 420px wide | Meeting detail view |
| `.overlay` | Blurred semi-transparent backdrop | Appears behind detail pane |
| `.duration-bar` | 3px gradient bar (blue → teal) | Relative duration indicator on each meeting row |
| `.action-item` | Flex row with colored dot + owner label + text | Individual action items in detail pane |
| `.action-dot` | 6px circle — blue for David, teal for others | Action item owner indicator |
| `.reveal` | Opacity + translateY(12px) entrance | Page load stagger animation on all sections |
| `.nav-bar` | Sticky top nav with logo mark and status | Global navigation |
| `.pulse-ring` | Ping animation in teal | "IntelOS Live" status indicator |

---

## Design Rules

### DO
- Use Geist Mono for ALL labels, badges, metadata, timestamps, and stat sub-labels — never Geist for these
- Keep all borders at `rgba(15,23,42,0.08)` in rest state — opacity-based, not color-based
- Blue (#1D4ED8) = David's actions + primary active states
- Teal (#0D9488) = other parties' actions + secondary indicators
- Gold (#B45309) = warnings, manual data, third-tier state only
- Stagger reveals by 40–80ms per element for perceived performance
- Use spring easing (`cubic-bezier(0.16, 1, 0.3, 1)`) for panel animations only — standard ease for everything else
- Duration bars use the relative max of all visible meetings (not a fixed scale)
- Light mode only — `--base` is never dark

### DON'T
- Never use Inter, Roboto, Arial, or system-ui directly — Geist is the only sans-serif
- Don't mix gold with blue/teal on the same element — each accent stands alone
- Don't add drop shadows to badges or pills — borders only
- Don't use border-radius > 14px on cards or > 100px on pills
- Don't use opacity < 0.07 for hairlines (too invisible) or > 0.15 (too heavy)
- Don't animate background-color on hover — use box-shadow and border-color instead

---

## Iconography

- Custom SVG inline icons only (no icon library loaded)
- Default stroke: `currentColor`, stroke-width: 2, stroke-linecap: round
- Logo mark: 14×14 white SVG on `--blue` 28×28 rounded square (border-radius: 6px)
- Search icon: 12×12, color `#94A3B8` (ink-faint)
- Close icon: 12×12 ×-shape, color `currentColor`

---

## Animation Reference

| Animation | Target | Duration | Easing |
|-----------|--------|----------|--------|
| Page reveal | `.reveal` elements | 550ms | `cubic-bezier(0.16, 1, 0.3, 1)` |
| Stagger delay | Per `.reveal` | +80ms each | — |
| Detail pane slide | `.detail-pane` | 500ms | `cubic-bezier(0.16, 1, 0.3, 1)` |
| Overlay fade | `.overlay` | 300ms | `ease` |
| Count-up | Stat numbers | ~300ms | rAF loop |
| Meeting row hover | `.meeting-row` | 200ms | `ease` |
| Pulse ring | `.pulse-ring` | 1500ms | `ease-in-out`, infinite |
