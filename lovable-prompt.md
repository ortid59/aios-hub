# Lovable Build Prompt — AIOS Installer Portal
**Advisor AI Partners · david@advisoraipartners.com**

---

## What I'm Building

A white-labeled client SaaS portal called the **AIOS Installer** for my AI consulting practice, Advisor AI Partners. Clients log in and get a personalized, interactive step-by-step installation wizard that walks them through setting up their AI Operating System (AIOS). I use this to onboard and track every client.

**Backend:** Supabase (auth, database, file storage)
**Deploy:** Netlify

---

## Brand & Design

**Product name:** AIOS Installer — Advisor AI Partners

**Color tokens (use exactly these):**
- `--blue: #1A3A6B` (primary, nav backgrounds, sidebar)
- `--blue-mid: #2A5298` (hover states)
- `--blue-light: #EEF3FF` (selected states, highlights)
- `--teal: #0C7A6E` (accent, Home Services vertical)
- `--teal-light: #E6F6F5`
- `--gold: #B8860B` (CTA buttons, progress fills, badges)
- `--gold-light: #FDF8EC`
- `--gray-50: #F5F4F0` (app background)
- `--gray-800: #1E2535` (body text)
- `--green: #059669` (completed steps)

**Logo mark:** Square with rounded corners (14px), gradient from `--blue` to `--teal`, bold "AP" in white. Beside it: "Advisor AI Partners" in bold, small "AIOS Installer Platform" in gray below.

**Gold dot accent:** Small 8px gold circle used in all nav bars beside the brand name — it's our signature brand mark.

**Font:** System UI stack (`-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`)

**Style:** Light mode always. Clean, professional, not flashy. Lots of white space. Cards with subtle borders. Sticky sidebars. No dark mode.

---

## Supabase Schema

Create these tables:

```sql
-- Clients (managed by admin — David)
create table clients (
  id uuid primary key default gen_random_uuid(),
  email text unique not null,
  name text,
  vertical text check (vertical in ('hvac', 'attorney', 'cpa', 'dental', 'medical', 'consultant')),
  created_at timestamptz default now(),
  last_active timestamptz,
  invited_at timestamptz,
  status text default 'invited' check (status in ('invited', 'active', 'complete')
);

-- Step progress (one row per client per step)
create table step_progress (
  id uuid primary key default gen_random_uuid(),
  client_id uuid references clients(id) on delete cascade,
  step_index integer not null,
  completed_at timestamptz default now(),
  unique(client_id, step_index)
);

-- Module downloads (track what clients download)
create table module_downloads (
  id uuid primary key default gen_random_uuid(),
  client_id uuid references clients(id) on delete cascade,
  module_slug text not null,
  downloaded_at timestamptz default now()
);
```

**Auth:** Supabase Magic Link auth. Admin (david@advisoraipartners.com) invites clients by email. Client receives magic link → logs in → lands on vertical selector.

**Row-level security:** Clients can only read/write their own rows. Admin email bypasses RLS for full access.

**File storage:** Supabase Storage bucket called `modules` — stores ZIP files (context-os.zip, infra-os.zip, data-os.zip, intel-os.zip, command-os.zip, productivity-os.zip). Download buttons generate signed URLs.

---

## App Screens & Flows

### Screen 1: Login
**Route:** `/`

Full-screen login. Background: `linear-gradient(135deg, #1A3A6B 0%, #0D2548 60%, #0C7A6E 100%)`. Centered white card (460px wide, 20px border-radius, generous padding).

Card contents top to bottom:
- Logo mark (AP) + "Advisor AI Partners / AIOS Installer Platform"
- H2: "Welcome back"
- P: "Sign in to access your personalized AIOS installation guide."
- Email input (label: "Email address", placeholder: "you@yourcompany.com")
- "Sign In →" button (full width, blue gradient, bold)
- Footer link: "New client? **Contact your advisor** to get access." (links to mailto:david@advisoraipartners.com)

**Auth:** Supabase magic link. User enters email, clicks Sign In, receives email with magic link. No password. On click, show: "Check your email — we sent you a sign-in link."

**Admin detection:** If the logged-in email is `david@advisoraipartners.com`, redirect to `/admin` instead of `/select`.

---

### Screen 2: Vertical Selector
**Route:** `/select`

Only shown if client has not yet chosen a vertical. Once selected, save to `clients.vertical` and redirect to `/install`.

**Header nav (dark blue bar):**
- Left: gold dot + "Advisor AI Partners — AIOS Installer"
- Right: user avatar circle (initials) + email

**Body (max-width 960px, centered, generous padding):**
- H1: "What kind of business do you **run?**" (the word "run?" in teal)
- Subtitle: "Choose your industry and your AIOS installation guide, module recommendations, and dashboard examples will be customized for you."
- 3-column grid of 6 industry cards:

| Vertical | Emoji | Title | Description | Color | Badge text |
|----------|-------|-------|-------------|-------|-----------|
| hvac | 🔧 | Home Services | HVAC, plumbing, electrical, roofing, cleaning, landscaping | teal | ServiceTitan · Google · Stripe |
| attorney | ⚖️ | Attorney / Law Firm | Personal injury, family, estate, business, criminal defense | blue | Clio · LawPay · Google |
| cpa | 📊 | CPA / Accounting | Tax preparation, bookkeeping, advisory, financial planning | gold | QBO · Karbon · Stripe |
| dental | 🦷 | Dental Practice | General dentistry, orthodontics, cosmetic, oral surgery | #7C3AED | Dentrix · Eaglesoft · PatientPop |
| medical | 🏥 | Medical Practice | Primary care, specialty, concierge, med spa, urgent care | #DC2626 | athenahealth · Healthie · Stripe |
| consultant | 💼 | Consultant / Coach | Business strategy, marketing, executive coaching, fractional roles | #0891B2 | HubSpot · Stripe · Calendly |

Each card: white background, 2px border (gray default, blue on hover/selected), top 4px color stripe, icon, title, description, badge pill. Click to select (highlighted border + soft shadow). 

Below grid: centered "Start My AIOS Setup →" button in gold. Disabled until a card is selected.

On click: save vertical to Supabase `clients.vertical`, redirect to `/install`.

---

### Screen 3: Installation Wizard
**Route:** `/install`

Two-column layout: fixed left sidebar (280px) + scrollable main content area.

**Sidebar (sticky, full height, dark blue header):**
- Header: gold dot + "Advisor AI Partners" brand, gold progress bar, "X of 13 steps complete" label
- Steps list grouped by phase with phase labels:
  - **FOUNDATION** (gray dot)
    - 1. Download & Install Claude Code
    - 2. Set Up Your AIOS Workspace
    - 3. Choose Your Path
  - **LAYER 1 — CONTEXT** (blue dot)
    - 4. Install ContextOS
    - 5. Install InfraOS
  - **LAYER 2 — DATA** (teal dot)
    - 6. Install DataOS
  - **LAYER 3 — INTELLIGENCE** (gold dot)
    - 7. Install IntelOS + Daily Brief
    - 8. Set Up CommandOS (Telegram)
  - **LAYER 4 — AUTOMATE** (purple dot)
    - 9. Run Your Task Audit
    - 10. Install ProductivityOS
  - **LAYER 5 — BUILD** (green dot)
    - 11. Browse the Module Library
    - 12. Build Your First Custom Automation
    - 13. 🎉 AIOS Installation Complete
- Each step has a number circle (turns blue when active, green with ✓ when completed)
- Footer: vertical badge (e.g., "⚖️ Attorney") + "Need help? Contact David" mailto link

**Top bar (sticky):**
- Left: phase label (e.g., "LAYER 1 — CONTEXT") + step title
- Right: "📦 Module Library" outline button + "Next Step →" blue button

**Main content area** (padded, max-width 860px):
Each step renders:
- Small eyebrow: "Layer 1 — Context · Step 4 of 13"
- Large bold title
- Subtitle description paragraph
- Numbered instruction list (items with blue number squares)
- Download button(s) where applicable — generates signed URL from Supabase Storage, tracks download in `module_downloads` table
- Industry-specific tip box (teal background) when relevant — content varies by vertical
- General tip box (gold background) for universal tips
- "✓ Mark as Complete" green button — on click, writes to `step_progress` table, updates sidebar
- "← Previous step" text link below

**Progress persistence:** On load, fetch all completed steps from `step_progress` for the current client. Restore the sidebar state. Continue where they left off (go to first incomplete step).

**Vertical-specific content** — the following changes by vertical:

For the **DataOS step**, the industry note shows:
- hvac: "Connect: ServiceTitan, Stripe, Google Business. Track: jobs booked, revenue/week, technician utilization"
- attorney: "Connect: Clio, LawPay, Google Business. Track: active matters, billable hours, realization rate, AR aging"
- cpa: "Connect: QuickBooks, Karbon, Stripe. Track: clients under management, revenue/client, capacity %, upcoming deadlines"
- dental: "Connect: Dentrix, Eaglesoft, PatientPop. Track: production/day, recall rate, new patients, treatment acceptance %"
- medical: "Connect: athenahealth, Healthie, Stripe. Track: patients seen/week, revenue cycle health, no-show rate"
- consultant: "Connect: HubSpot, Stripe, Calendly. Track: MRR, pipeline value, utilization %, client NPS"

For the **ContextOS step**, the industry note shows the practice area / context seeds for that vertical.

---

### Screen 4: Module Library
**Route:** `/modules`

Accessible from wizard via nav button. Back button returns to `/install`.

Nav bar same style as wizard (dark blue).

Body:
- H1: "Module Library"
- Subtitle: "Pre-built systems for your AIOS. Download, drop into your workspace, run `/install`."

**Core Modules section** (3-column grid, left border colored by layer):

| Module | Icon | Description | Layer | Border color |
|--------|------|-------------|-------|-------------|
| ContextOS | 🧠 | Guided interview that teaches your AI your business | Layer 1 · Required | blue |
| InfraOS | 🔒 | Git version control, GitHub backup, auto-documentation, changelog | Layer 1 · Required | blue |
| DataOS | 📊 | Daily data collection into local SQLite + auto dashboard | Layer 2 · Required | teal |
| IntelOS | 🎧 | Meeting intelligence — transcripts, action items, insights | Layer 3 · Required | teal |
| CommandOS | 📱 | Claude on your phone via Telegram. Voice notes → responses. | Layer 3 · Required | gold |
| ProductivityOS | ⚡ | Task audit + automation workflow. Maps and scores everything. | Layer 4 · Required | gold |

**Add-On Modules section** (same grid):

| Module | Icon | Description | Size |
|--------|------|-------------|------|
| Content Pipeline | 🎬 | YouTube, LinkedIn, newsletter on autopilot | 14 KB |
| Thumbnail Generator | 🖼️ | AI-generated YouTube thumbnails from your photo catalog | 8 KB |
| Diagram Engine | 🗺️ | Architecture diagrams, D2 language, PNG output | 6 KB |
| Deep Research | 🔍 | Multi-agent research across 7 platforms simultaneously | 22 KB |
| Anti-AI-Slop Writing | ✍️ | 40+ banned words, 12 rules. Every output audited. | 5 KB |
| OpenClaw AI Employee | 🤖 | Autonomous agent on $6/mo server — runs 24/7 | 18 KB |

Download buttons hit Supabase Storage signed URLs. Log to `module_downloads` table.

---

### Screen 5: Admin Dashboard
**Route:** `/admin` (only accessible to david@advisoraipartners.com — redirect all others to `/install`)

Nav: same dark blue bar. Links: "Clients" (active) | "Settings". Right: "DA" avatar.

**Stats row (4 cards):**
- Total Clients (blue top border)
- Fully Installed (green top border) — count where status = 'complete'
- In Progress (gold top border) — count where status = 'active'
- Invited / Pending (gray top border) — count where status = 'invited'

All stat counts are live from Supabase.

**Client table:**
Columns: Client (name + email), Industry (colored chip), Progress (bar + %), Modules (X / 6), Last Active, Status pill

Status pills:
- Active: green background
- Needs Nudge: gold background (last_active > 5 days ago and not complete)
- Invited: blue background (never logged in)
- Complete: green

Each row is clickable → opens client detail modal (future: show full step breakdown per client)

**"+ Add Client" button:**
Opens a modal: enter client name + email + vertical → creates row in `clients` table + triggers Supabase magic link email to the client.

**Search input** filters the table client-side by name or email.

---

## Key Interactions

- **Progress saves in real time.** Every "Mark as Complete" click writes to Supabase immediately. Sidebar and progress bar update instantly.
- **Download tracking.** Every module download is logged by client_id + module_slug + timestamp.
- **Last active tracking.** Update `clients.last_active` on every page load after login.
- **Completion detection.** When all 13 steps are marked complete, update `clients.status` to 'complete'. Show a celebration banner on the final step.
- **Admin shortcut.** Keyboard shortcut `Cmd+A` on any screen jumps to admin (for demos).
- **Mobile responsive.** The wizard collapses the sidebar into a top progress bar on mobile. Vertical cards go to 1-column.

---

## File & Folder Conventions

- Use React + TypeScript
- Supabase client in `src/lib/supabase.ts`
- Auth context in `src/contexts/AuthContext.tsx`
- Pages in `src/pages/`: `Login.tsx`, `Select.tsx`, `Install.tsx`, `Modules.tsx`, `Admin.tsx`
- Shared components in `src/components/`: `Sidebar.tsx`, `StepCard.tsx`, `ModuleCard.tsx`, `NavBar.tsx`, `ProgressBar.tsx`
- Types in `src/types/index.ts`

---

## Environment Variables

```
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
```

---

## What This Is NOT

- Not a course platform (no video playback)
- Not a CRM
- Not a public-facing marketing site
- The wizard content is static (hard-coded) — no CMS needed
- No payment integration needed in this build

---

## Done State

The app is complete when:
1. A client can receive a magic link, log in, pick their vertical, and work through all 13 wizard steps
2. Progress persists across sessions (close browser, come back, pick up where they left off)
3. Download buttons serve real files from Supabase Storage
4. David can log in at `/admin`, see all clients, their progress, and invite new ones
5. The design matches the brand tokens above exactly — light mode, gold dots, blue nav, clean cards
