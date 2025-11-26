# HEMA Tournament Series

A website for HEMA (Historical European Martial Arts) tournament series, built with Hugo and the Hugo Book theme.

## 🚀 Quick Start

```bash
git clone --recurse-submodules https://github.com/AlexeyTrekin/baby-shark-tournament.git
cd baby-shark/landing
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

### Gallery Images (External Storage)

Gallery images are stored externally on S3 (`baby-shark-media` bucket) to keep the repository lightweight.

**Bucket structure:**
```
baby-shark-media/
└── longsword-2025/           # Event folder (same name as content)
    ├── photo1.jpeg           # Full-size images
    ├── photo2.jpeg
    └── thumbnails/           # 200x200 thumbnails
        ├── photo1.jpeg
        └── photo2.jpeg
```

#### 1. Generate Thumbnails

Requires [ImageMagick](https://imagemagick.org/):

```bash
# Install (if needed)
brew install imagemagick  # macOS
# apt install imagemagick  # Linux
# choco install imagemagick  # Windows

# In folder with original images, generate 200x200 thumbnails
mkdir -p thumbnails
for img in *.jpg *.jpeg *.png; do
  [ -f "$img" ] && magick "$img" -resize 200x200^ -gravity center -extent 200x200 -quality 85 "thumbnails/$img"
done
```

#### 2. Upload to S3

```bash
# Set your bucket and event name
BUCKET="baby-shark-media"
EVENT="longsword-2025"

# Upload originals
aws s3 sync . s3://$BUCKET/$EVENT/ --exclude "thumbnails/*" --exclude "*.sh"

# Upload thumbnails
aws s3 sync thumbnails/ s3://$BUCKET/$EVENT/thumbnails/
```

#### 3. Reference in Front Matter

```yaml
gallery_images:
  - url: https://baby-shark-media.s3.amazonaws.com/longsword-2025/photo1.jpeg
    thumb: https://baby-shark-media.s3.amazonaws.com/longsword-2025/thumbnails/photo1.jpeg
  - url: https://baby-shark-media.s3.amazonaws.com/longsword-2025/photo2.jpeg
    thumb: https://baby-shark-media.s3.amazonaws.com/longsword-2025/thumbnails/photo2.jpeg
```

#### Local Development (Optional)

For offline preview, place images in `media/` folder (gitignored) and use local paths:

```yaml
gallery_images:
  - url: /media/longsword-2025/photo1.jpeg
    thumb: /media/longsword-2025/thumbnails/photo1.jpeg
```

Hugo mounts `../media/` to `/media/` during local development.

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
