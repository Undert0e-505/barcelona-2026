# 🇪🇸 Barcelona 2026 — Itinerary Site

Public-facing holiday itinerary for the Barcelona trip (Wed 27 – Sat 30 May 2026). Dark-mode calendar grid with clickable activity detail pages, plus a "Things to Do" page for unscheduled activities.

**Live site:** https://undert0e-505.github.io/barcelona-2026/

---

## ⚠️ Privacy Rules

This repo is **public**. Never commit:

- Full names (use first names only)
- Email addresses
- Booking references
- Phone numbers
- Home addresses
- Payment details
- Exact ticket counts / prices
- Airbnb listing URLs or host names

All personal/booking details live in the **private repo** ([barcelona-trip](https://github.com/Undert0e-505/barcelona-trip)). Public pages show general descriptions only.

---

## Architecture

### Files

```
barcelona-2026/
├── index.html              ← Calendar grid (4-day view + mobile slider)
├── things-to-do.html      ← Unscheduled activities (by category, no time slot)
├── flight-out.html         ← Outbound flight detail page
├── arrive-bcn.html         ← Airport transfer detail page
├── checkin.html            ← Check-in detail page
├── sartoria.html            ← Sartoria Panatieri detail page
├── casa-batllo.html        ← Casa Batlló detail page
├── magic-fountain.html     ← Magic Fountain detail page
├── bogatell.html            ← Bogatell Beach detail page
├── parking.html            ← Airport parking detail page
├── flight-home.html        ← Return flight detail page
├── maps.html               ← Metro & train maps
├── travel-cb-pg.html       ← Metro route: Casa Batlló → Park Güell
├── bunkers.html            ← Bunkers del Carmel detail (unscheduled)
├── montjuic-castle.html    ← Montjuïc Castle detail (unscheduled)
├── picasso.html            ← Museu Picasso detail (unscheduled)
├── park-guell.html         ← Park Güell detail (unscheduled)
├── tibidabo.html            ← Tibidabo detail (unscheduled)
├── boqueria.html            ← Mercat de la Boqueria detail (unscheduled)
├── granja-dulcinea.html    ← Granja Dulcinea detail (unscheduled)
├── img/                    ← Activity images (sourced from voting repo)
└── README.md               ← This file
```

### Three page types

1. **Calendar grid** (`index.html`) — dual view:
   - **Desktop:** 4-column grid with hourly rows (04:00–24:00), time labels on left, photo headers per day, events positioned absolutely by time
   - **Mobile (≤900px):** Swipeable day-by-day slider with the same hourly time grid, photo header, dot indicators, and tab navigation
2. **Things to Do** (`things-to-do.html`) — unscheduled activities:
   - **Desktop:** Category columns side-by-side (CSS grid, auto-fit min 280px)
   - **Mobile (≤900px):** Single column, categories stacked
   - Activities listed by category with image cards, clickable to detail pages
   - Activities here are not tied to a time slot — they may get scheduled later
3. **Detail pages** (`<id>.html`) — one per activity, with hero, info card, fun facts, trivia, jokes, and map
   - Calendar detail pages: back link → `index.html`
   - Things to Do detail pages: back link → `things-to-do.html`

---

## Adding a New Activity

There are two types of activity: **scheduled** (with a time slot on the calendar) and **unscheduled** (floating, no fixed time).

---

### Option A: Scheduled Activity (timed event on the calendar)

#### 1. Add the event to `index.html`

In the `EVENTS` array, add an object:

```javascript
{
  id: "park-guell",          // slug — used as filename and URL
  title: "Park Güell",       // display name
  day: "2026-05-29",         // date in YYYY-MM-DD
  start: "10:00",            // start time HH:MM (24h, local time = CEST)
  duration: 120,             // duration in minutes (default 60)
  icon: "🌳",                // emoji shown on event block
  colour: "#4CAF50",         // hex colour for the event block
  tag: "sights",             // category tag shown on event
  detail: true               // true = has a detail page; false = no detail page
}
```

#### 2. Add a photo for the day header (if new day)

In the `DAYS` array, add an entry for any new day:

```javascript
{date:"2026-05-29", name:"FRI", num:"29", month:"May", full:"Friday", photo:"img/beach.jpg"}
```

Copy the image to `img/`. Use images from the voting repo (`barcelona-trip/voting/img/`) when available, or source royalty-free images.

#### 3. Create the detail page `<id>.html`

Copy an existing scheduled detail page (e.g. `casa-batllo.html`) and modify. Back link → `index.html`.

#### 4. Commit and push

```bash
git add -A && git commit -m "✨ Add Park Güell — Fri 29 May" && git push
```

GitHub Pages auto-deploys from `main`.

---

### Option B: Unscheduled Activity (on Things to Do page)

#### 1. Add the activity to `things-to-do.html`

In the `UNSCHEDULED` array, add an object:

```javascript
{
  id: "park-guell",           // slug — matches <id>.html filename
  title: "Park Güell",        // display name
  desc: "Short description",  // shown on the card (1 line, max ~100 chars)
  cat: "sights",             // category key (see CATS object)
  icon: "🦎",                 // emoji shown on the card
  colour: "#4CAF50",          // hex colour for the card accent
  img: "img/parkguell.jpg"    // path to image
}
```

Categories are defined in the `CATS` object: `sights`, `food`, `beach`, `activities`, `shopping`, `nightlife`, `relax`.

#### 2. Copy the image to `img/`

Use images from the voting repo (`barcelona-birthday/img/`) when available, or download from Wikipedia Commons (use Python `requests` with `User-Agent: JimothyBot/1.0` — `curl` from WSL is blocked by Wikimedia CDN).

#### 3. Create the detail page `<id>.html`

Copy an existing unscheduled detail page (e.g. `bunkers.html` or `granja-dulcinea.html`) and modify. **Back link → `things-to-do.html`** (not `index.html`).

#### 4. Commit and push

```bash
git add -A && git commit -m "🧭 Add Park Güell to Things to Do" && git push
```

---

### Moving an Unscheduled Activity to the Calendar

When you're ready to assign a time:

1. Remove the entry from `UNSCHEDULED` in `things-to-do.html`
2. Add it to the `EVENTS` array in `index.html` with day/start/duration
3. Update the detail page: change back link to `index.html`, add date/time to hero subtitle
4. Commit & push

---

### Detail Page Structure (both types)

All detail pages follow this structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ICON Activity Name</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">
  <style>
    :root{--bg:#0a0e1a;--surface:#131829;--surface2:#1a2035;
           --border:#252d45;--text:#e8eaf0;--text2:#8892b0;
           --accent:#COLOUR}  ← change per activity
  </style>
</head>
<body>
  <div class="hero" style="background:linear-gradient(135deg,#COLOUR,#DARKER)">
    <img class="hero-img" src="img/IMAGE.jpg" onerror="this.style.display='none'">
    <div class="hero-content">
      <h1>Activity Name</h1>
      <!-- Scheduled: "Thu 28 May · 09:00–11:00 · tag" -->
      <!-- Unscheduled: "Tagline · tag" -->
      <div class="sub">Tagline · <span class="tag">tag</span></div>
    </div>
  </div>

  <div class="content">
    <!-- Scheduled: href="index.html" -->
    <!-- Unscheduled: href="things-to-do.html" -->
    <a class="back" href="things-to-do.html">← Back to Things to Do</a>

    <div class="card"><h2>📋 Visit Details</h2><!-- info-row pairs --></div>
    <div class="card"><h2>💡 Fun Facts</h2><!-- fact divs --></div>
    <div class="card"><h2>🧠 Trivia</h2><!-- trivia-box with reveal --></div>
    <div class="card"><!-- jokes --><!-- joke divs --></div>
    <div class="card"><h2>📍 Location</h2><!-- map iframe + open-maps link --></div>
  </div>

  <!-- Transport bar on all pages -->
  <div class="transport-bar">
    <a class="transport-btn" href="things-to-do.html"><span class="tb-icon">🧭</span> Things to Do</a>
    <a class="transport-btn" href="maps.html"><span class="tb-icon">🚇</span> Metro</a>
    <a class="transport-btn" href="maps.html#rodalies"><span class="tb-icon">🚆</span> Trains</a>
  </div>
</body>
</html>
```

---

## Style Guide

### Design System

| Token | Value | Usage |
|-------|-------|-------|
| `--bg` | `#0a0e1a` | Page background |
| `--surface` | `#131829` | Card / panel background |
| `--surface2` | `#1a2035` | Button background |
| `--border` | `#252d45` | Borders, dividers |
| `--text` | `#e8eaf0` | Primary text |
| `--text2` | `#8892b0` | Secondary / muted text |
| `--accent` | Varies per page | Page accent colour |

### Font

- **Poppins** (400, 500, 600, 700, 800, 900)
- Google Fonts CDN

### Colour Palette per Tag

| Tag | Colour | Hex |
|-----|--------|-----|
| travel | Blue | `#1E88E5` |
| sights | Purple | `#E040FB` |
| food | Orange | `#FB923C` |
| stays | Warm orange | `#FF7043` |
| beach | Teal | `#4ECDC4` |
| nightlife | Pink | `#FF6B6B` |
| relax | Sage | `#34D399` |
| shopping | Amber | `#F59E0B` |
| activities | Warm orange | `#FF7043` |
| general | Grey | `#78909C` |

### Hero Gradients

Each detail page uses a two-stop gradient from the accent colour to a darker shade:

| Activity | Type | Gradient |
|----------|------|----------|
| Flight out | scheduled | `#1E88E5` → `#1565C0` |
| Arrival | scheduled | `#43A047` → `#2E7D32` |
| Check-in | scheduled | `#FF7043` → `#E64A19` |
| Sartoria | scheduled | `#FB923C` → `#C2410C` |
| Casa Batlló | scheduled | `#E040FB` → `#7B1FA2` |
| Magic Fountain | scheduled | `#4ECDC4` → `#2A9D8F` |
| Bogatell Beach | scheduled | `#4ECDC4` → `#2A9D8F` |
| Flight home | scheduled | `#1E88E5` → `#0D47A1` |
| Bunkers del Carmel | unscheduled | `#E040FB` → `#7B1FA2` |
| Montjuïc Castle | unscheduled | `#E040FB` → `#7B1FA2` |
| Museu Picasso | unscheduled | `#E040FB` → `#7B1FA2` |
| Park Güell | unscheduled | `#E040FB` → `#7B1FA2` |
| Tibidabo | unscheduled | `#FF7043` → `#BF360C` |
| Granja Dulcinea | unscheduled | `#FB923C` → `#C2410C` |
| Mercat de la Boqueria | unscheduled | `#FB923C` → `#C2410C` |

### Calendar Grid

- **Desktop (>900px):** 4 columns (one per day), time labels on left (04:00–24:00), 60px per hour
- **Mobile (≤900px):** Swipeable slider — one day at a time with the same hourly time grid, photo header, dot indicators, and tab buttons for quick jumps
- **Events** positioned absolutely within their hour cell using `top` offset for minute precision and `height` based on duration
- **Photo headers** per day column (100px tall, opacity 0.4, gradient fade to background)
- **Hover effects**: events scale up with shadow, photo headers brighten to 0.6 opacity

### Things to Do Page

- **Desktop:** Category columns side-by-side (CSS grid, auto-fit min 280px)
- **Mobile (≤900px):** Single column, categories stacked vertically
- Activity cards with image thumbnail, title, category tag, short description
- Cards link to detail pages
- Transport bar at bottom (same as calendar page)
- Link to Things to Do page in the transport bar on `index.html`

### Mobile Slider

- Touch swipe left/right between days (tracks finger in real-time)
- Direction detection: vertical scroll still works; only horizontal swipes change days
- CSS transition on release (0.35s cubic-bezier ease-out)
- Dot indicators at bottom (tap to jump)
- Day tabs at top (tap to jump)
- Same hourly time grid as desktop — scrollable vertically within each day

### Animations

- `fadeUp` — cards slide up on load (staggered 0.1s per card)
- `eventPop` — events scale from 0.92 to 1.0 on load
- `float` — subtle background particles (10, 3-7px, warm colours at 10% opacity)
- Event hover: `scale(1.03)` + glow shadow
- Back button hover: `translateX(-4px)` (slides left)
- Trivia reveal: `display:block` toggle
- Open in Maps hover: `translateY(-2px)` + accent background

### Map Pattern

Every detail page has:
1. Embedded iframe using `https://maps.google.com/maps?q=ENCODED+QUERY&output=embed`
2. "📍 Open in Google Maps" link using `https://www.google.com/maps/search/PLAIN+QUERY` — this opens the native Maps app on mobile

### Transport Bar

Fixed at bottom of every page. Currently:
- 🧭 Things to Do → `things-to-do.html`
- 🚇 Metro → `maps.html`
- 🚆 Trains → `maps.html#rodalies`

### Responsive Breakpoints

| Breakpoint | Layout |
|-----------|--------|
| > 900px | 4-column grid with photo headers |
| ≤ 900px | Swipeable day slider with hourly time grid, photo header, dots + tabs |

---

## JSON Schemas

### `EVENTS` array in `index.html` (scheduled activities)

```typescript
interface Event {
  id:        string;    // Unique slug, matches <id>.html filename
  title:     string;    // Display name
  day:       string;    // "YYYY-MM-DD" — must match a DAYS entry
  start:     string;    // "HH:MM" — 24h, local time (CEST for Barcelona)
  duration:  number;    // Minutes. Default slot is 60.
  icon:      string;    // Emoji for the event block
  colour:    string;    // Hex colour (see palette above)
  tag:       string;    // Category: travel|sights|food|stays|beach|nightlife|relax|shopping|general
  detail:    boolean;   // true = has a <id>.html detail page
  page?:     string;    // Optional: override detail page filename (e.g. parking.html for both entries)
}
```

### `UNSCHEDULED` array in `things-to-do.html` (floating activities)

```typescript
interface UnscheduledActivity {
  id:     string;    // Unique slug, matches <id>.html filename
  title:  string;    // Display name
  desc:   string;    // Short description for card (~100 chars max)
  cat:    string;    // Category key: sights|food|beach|activities|shopping|nightlife|relax
  icon:   string;    // Emoji for the card
  colour: string;    // Hex colour (see palette above)
  img:    string;    // Path to image: "img/slug.jpg"
}
```

### `DAYS` array in `index.html`

```typescript
interface Day {
  date:  string;  // "YYYY-MM-DD"
  name:  string;  // 3-letter day abbreviation: "WED", "THU", etc.
  num:   string;  // Day number: "27", "28", etc.
  month: string;  // Month name: "May"
  full:  string;  // Full day name: "Wednesday"
  photo: string;  // Path to header image: "img/sagrada.jpg"
}
```

---

## Current Scheduled Events

| # | Activity | Day | Start | Dur | Tag | Detail Page |
|---|----------|-----|-------|-----|-----|-------------|
| 1 | Airport Parking | Wed 27 May | 04:00 | 1h | travel | ✅ parking.html |
| 2 | Fly to Barcelona | Wed 27 May | 07:00 | 3h15 | travel | ✅ flight-out.html |
| 3 | Arrive in Barcelona | Wed 27 May | 10:15 | 1h | travel | ✅ arrive-bcn.html |
| 4 | Check In | Wed 27 May | 16:00 | 1h | stays | ✅ checkin.html |
| 5 | Sartoria Panatieri | Wed 27 May | 19:30 | 1h30 | food | ✅ sartoria.html |
| 6 | Casa Batlló | Thu 28 May | 09:00 | 2h | sights | ✅ casa-batllo.html |
| 7 | Metro to Park Güell | Thu 28 May | 11:00 | 1h | travel | ✅ travel-cb-pg.html |
| 8 | Park Güell | Thu 28 May | 12:00 | 2h | sights | ✅ park-guell.html |
| 9 | Magic Fountain | Fri 29 May | 21:00 | 1h | sights | ✅ magic-fountain.html |
| 8 | Bogatell Beach | Fri 29 May | 10:00 | 5h | beach | ✅ bogatell.html |
| 9 | Costa Brava Kayak & Snorkel | Fri 29 May | 09:30 | 6h | activities | ✅ costa-brava-kayak.html |
| 10 | The Venue Steak House | Thu 28 May | 19:00 | 2h | food | ✅ venue-steakhouse.html |
| 11 | Checkout | Sat 30 May | 10:00 | 30m | general | ❌ no detail page |
| 12 | Fly Home | Sat 30 May | 10:50 | 1h30 | travel | ✅ flight-home.html |
| 13 | Collect Car | Sat 30 May | 13:00 | 1h | travel | ✅ parking.html (via page prop) |

## Current Unscheduled Activities

| Activity | Category | Detail Page |
|----------|----------|-------------|
| Bunkers del Carmel | sights | ✅ bunkers.html |
| Montjuïc Castle | sights | ✅ montjuic-castle.html |
| Museu Picasso | sights | ✅ picasso.html |
| Tibidabo | activities | ✅ tibidabo.html |
| Poblenou Street Art Walk | activities | ✅ poblenou-streetart.html |
| Mercat de la Boqueria | food | ✅ boqueria.html |
| Granja Dulcinea | food | ✅ granja-dulcinea.html |
| Sailing, Inflatables & Snorkel | activities | ✅ sailing-barcelona.html |
| Breakfast & Supplies Near the Flat | relax | ✅ (uses boqueria.jpg) |
| CosmoCaixa Science Museum | sights | ✅ cosmocaixa.html |

---

## Workflow

Activities follow a lifecycle across repos:

```
Idea → Things to Do page (public, no booking info)
          ↓
     Offline agreement / booking confirmed
          ↓
     Calendar (public, with time slot) + Private repo .md (booking refs, prices)
```

- **Things to Do** is a staging area — activities there are being *considered*, not necessarily booked
- An activity can appear on the public site **without** a private repo file (it just means it's not confirmed yet)
- The voting site ([barcelona-birthday](https://github.com/Undert0e-505/barcelona-birthday)) is in **archive mode** — it served its purpose for initial preference gathering but is no longer being actively updated

---

## Content Guidelines

### Fun Facts
- 3–5 per activity
- Each starts with an emoji
- Bold the key word/phrase using `<strong>` coloured with `--accent`
- Keep to 1–2 sentences per fact
- Research real facts — don't make them up

### Trivia
- 1–2 per activity (optional)
- Interactive "Reveal Answer" button using `onclick="document.getElementById('tX').style.display='block'"`
- Make them genuinely interesting, not just easy

### Jokes
- 2 per activity
- **Must be genuinely funny** — observational, dry, a bit sharp. No dad jokes, no puns, no "why did the X cross the Y"
- Specific to the activity or travel context is a bonus
- Mildly edgy is fine; no swearing, no offensive content
- Think: what would make a witty 15-year-old actually laugh
- Keep them short — setup 1 line, punchline 1 line

### Info Card
- Use `info-row` pairs (label / value)
- Include: what, when, where, how long, any must-knows
- **Never include** booking refs, full names, prices, or personal data

### Map
- Always include both the iframe embed AND the "Open in Google Maps" button
- Use URL-encoded query for iframe, plain text for the button link

### Images
- ~640px wide JPEG, stored locally in `img/`
- Prefer images from the voting repo (`barcelona-birthday/img/`) — already sourced and verified
- For new images, use Wikipedia Commons via Python `requests` with `User-Agent: JimothyBot/1.0` header (`curl` from WSL is blocked by Wikimedia CDN)
- Verify downloaded images match the activity with `file` command