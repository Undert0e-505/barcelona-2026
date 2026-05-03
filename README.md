# 🇪🇸 Barcelona 2026 — Itinerary Site

Public-facing holiday itinerary for the Barcelona trip (Wed 27 – Sat 30 May 2026). Dark-mode calendar grid with clickable activity detail pages.

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
├── index.html          ← Calendar grid (4-day view + mobile tabs)
├── flight-out.html     ← Outbound flight detail page
├── arrive-bcn.html     ← Airport transfer detail page
├── checkin.html        ← Check-in detail page
├── casa-batllo.html    ← Casa Batlló detail page
├── flight-home.html    ← Return flight detail page
├── img/
│   ├── casa-batllo.jpg ← Activity images (from voting repo)
│   ├── gothic.jpg
│   ├── barceloneta.jpg
│   └── sagrada.jpg
└── README.md           ← This file
```

### Two page types

1. **Calendar grid** (`index.html`) — dual view:
   - **Desktop:** 4-column grid with hourly rows (06:00–23:00), time labels on left, photo headers per day, events positioned absolutely by time
   - **Mobile (≤900px):** Swipeable day-by-day slider with the same hourly time grid, photo header, dot indicators, and tab navigation
2. **Detail pages** (`<id>.html`) — one per activity, with hero, info card, fun facts, trivia, jokes, and map

---

## Adding a New Activity

### 1. Add the event to `index.html`

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

### 2. Add a photo for the day header (if new day)

In the `DAYS` array, add an entry for any new day:

```javascript
{date:"2026-05-29", name:"FRI", num:"29", month:"May", full:"Friday", photo:"img/beach.jpg"}
```

Copy the image to `img/`. Use images from the voting repo (`barcelona-trip/voting/img/`) when available, or source royalty-free images.

### 3. Create the detail page `<id>.html`

Copy an existing detail page (e.g. `casa-batllo.html`) and modify. Each detail page follows this structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <!-- Standard meta, Poppins font -->
  <style>
    /* CSS variables — change --accent to match the event colour */
    :root{--bg:#0a0e1a;--surface:#131829;--surface2:#1a2035;
           --border:#252d45;--text:#e8eaf0;--text2:#8892b0;
           --accent:#E040FB}  ← change this per activity
    /* ... shared dark-mode styles ... */
  </style>
</head>
<body>
  <!-- Hero section with gradient background -->
  <div class="hero" style="background:linear-gradient(135deg,#COLOUR,#DARKER)">
    <img class="hero-img" src="img/IMAGE.jpg" onerror="this.style.display='none'">
    <div class="hero-content">
      <h1>Activity Name</h1>
      <div class="sub">Day Date · Start–End · <span class="tag">tag</span></div>
    </div>
  </div>

  <div class="content">
    <a class="back" href="index.html">← Back to itinerary</a>

    <!-- 📋 Info card — key details -->
    <div class="card">
      <h2>📋 Visit Details</h2>
      <!-- info-row pairs for each detail -->
    </div>

    <!-- 💡 Fun Facts card — 3-5 facts with emojis -->
    <div class="card">
      <h2>💡 Fun Facts</h2>
      <!-- fact divs with emoji + paragraph -->
    </div>

    <!-- 🧠 Trivia card — interactive reveal (optional) -->
    <div class="card">
      <h2>🧠 Trivia</h2>
      <div class="trivia-box">
        <h3>🇪🇸 Question</h3>
        <p class="question">Question text?</p>
        <button onclick="document.getElementById('t1').style.display='block'">Reveal Answer</button>
        <div class="answer" id="t1">Answer text</div>
      </div>
    </div>

    <!-- 😂 Jokes card -->
    <div class="card">
      <h2>😂 Jokes</h2>
      <div class="joke">
        <div class="q">Setup?</div>
        <div class="a">Punchline! 🎨</div>
      </div>
    </div>

    <!-- 📍 Map card -->
    <div class="card">
      <h2>📍 Location</h2>
      <div class="map-wrap">
        <iframe src="https://maps.google.com/maps?q=ENCODED+QUERY&output=embed" 
                allowfullscreen loading="lazy"></iframe>
      </div>
      <a class="open-maps" href="https://www.google.com/maps/search/PLAIN+QUERY" 
         target="_blank" rel="noopener">📍 Open in Google Maps</a>
    </div>
  </div>
</body>
</html>
```

### 4. Commit and push

```bash
git add -A && git commit -m "✨ Add Park Güell — Fri 29 May" && git push
```

GitHub Pages auto-deploys from `main`.

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
| general | Grey | `#78909C` |

### Hero Gradients

Each detail page uses a two-stop gradient from the accent colour to a darker shade:

| Activity | Gradient |
|----------|----------|
| Flight out | `#1E88E5` → `#1565C0` |
| Arrival | `#43A047` → `#2E7D32` |
| Check-in | `#FF7043` → `#E64A19` |
| Casa Batlló | `#E040FB` → `#7B1FA2` |
| Flight home | `#1E88E5` → `#0D47A1` |

### Calendar Grid

- **Desktop (>900px):** 4 columns (one per day), time labels on left (06:00–23:00), 60px per hour
- **Mobile (≤900px):** Swipeable slider — one day at a time with the same hourly time grid, photo header, dot indicators, and tab buttons for quick jumps
- **Events** positioned absolutely within their hour cell using `top` offset for minute precision and `height` based on duration
- **Photo headers** per day column (100px tall, opacity 0.4, gradient fade to background)
- **Hover effects**: events scale up with shadow, photo headers brighten to 0.6 opacity

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

### Responsive Breakpoints

| Breakpoint | Layout |
|-----------|--------|
| > 900px | 4-column grid with photo headers |
| ≤ 900px | Swipeable day slider with hourly time grid, photo header, dots + tabs |

---

## Event JSON Schema

The `EVENTS` array in `index.html` uses this schema:

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
}
```

The `DAYS` array:

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