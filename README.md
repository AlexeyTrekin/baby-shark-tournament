# HEMA Tournament Series

A website for HEMA (Historical European Martial Arts) tournament series, built with Hugo and the Hugo Book theme.

## 🚀 Quick Start

```bash
git clone --recurse-submodules https://github.com/AlexeyTrekin/dukescup.git
cd dukescup/landing
hugo server -D --port 1314
```

Visit: **http://localhost:1314**

## 📝 Content Management

### Structure
```
content/
├── _index.md                      # Homepage
└── docs/
    ├── current-event/
    │   └── spring-2025.md         # Current tournament
    ├── past-events/
    │   └── winter-2024.md         # Past tournaments
    └── future-events/
        └── autumn-2025.md         # Upcoming tournaments
```

### Add New Event

Copy an existing event and modify the front matter:

```bash
cp content/docs/current-event/spring-2025.md content/docs/future-events/summer-2025.md
```

**Front matter fields:**
- `event_status`: `"current"`, `"past"`, or `"future"`
- `application_url`: Registration link (for current events)
- `results_url`: Results link (for past events)
- `application_content`: Registration info (markdown)
- `rules_content`: Tournament rules (markdown)
- `timetable_content`: Schedule (markdown)
- `venue_content`: Location & travel info (markdown)

### Images & Assets

Place files in `static/` directory:
- Images: `static/filename.jpg` → access as `/filename.jpg`
- Sponsors: `static/sponsors/logo.png` → access as `/sponsors/logo.png`

## 🎨 Customization

**Custom CSS:** `layouts/partials/docs/inject/head.html`

**Event Template:** `layouts/_default/event.html`

**Config:** `hugo.toml`

## 🔄 Deployment

```bash
cd landing
hugo --gc --minify
```

Output in `public/` - deploy to GitHub Pages via Actions on push to `main`.
