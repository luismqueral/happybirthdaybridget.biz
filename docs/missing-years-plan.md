# Missing Years Recovery Plan

## Year-by-Year Status

| Year | Status | What Exists | Action Needed |
|------|--------|-------------|---------------|
| 2014 | ACTIVE | Marquee/Hey Arnold audio site | Done |
| 2015 | ACTIVE | YouTube video + photo essay | Done |
| 2016 | EMPTY | Empty directory | Search Drive |
| 2017 | ACTIVE | Wedding year, Sass-based | Done |
| 2018 | SKELETON | scss file + empty js | Search Drive |
| 2019 | MISSING | Nothing | Search Drive |
| 2020 | CONTENT ONLY | 1.1GB video (screen recordings, TikToks) | Build HTML wrapper |
| 2021 | MISSING | Nothing | Search Drive |
| 2022 | CONTENT ONLY | Numbered images + gifs (no HTML) | Build HTML wrapper |
| 2023 | MISSING | Nothing | Search Drive / may not exist |
| 2024 | MISSING | Nothing | Search Drive / may not exist |
| 2025 | ACTIVE | Next.js static export | Done |
| 2026 | ACTIVE | MySpace-themed vanilla HTML | Done |

## What We Have Locally

### 2020/
- Large video files (~1.1GB total)
- Screen recordings, TikTok downloads
- No index.html or structure
- Likely a pandemic-year video compilation site

### 2022/
- Numbered image files (1.jpg, 2.jpg, etc.)
- GIF files
- No index.html
- Likely a photo gallery site similar to 2015

### 2018/
- `scss/` directory with a stylesheet
- Empty `js/` file
- Suggests a site was started but never finished, OR the HTML was hosted elsewhere

## Recovery Strategy

### Step 1: Search Personal Google Drive

The old site files are likely in personal Google Drive. Google's web search is unreliable for finding old files. Better options:

**Option A: Google Drive Desktop + local search**
- Install Google Drive for Desktop (if not already)
- Mount Drive as local filesystem
- Use `find` / `rg` to search for `.html` files mentioning "bridget" or "birthday"
- Search for folders named "2016", "2018", "2019", "2021", "2023", "2024" near birthday-related content

**Option B: Google Takeout**
- Export all Drive data via takeout.google.com
- Search the downloaded archive locally
- Slower but gives a complete snapshot

**Option C: Google Drive API via rclone**
```bash
# Install rclone
brew install rclone

# Configure Google Drive remote
rclone config
# (follow prompts to auth with personal Google account)

# Search for birthday site files
rclone lsf --recursive --include "*.html" gdrive: | grep -i "birthday\|bridget\|happybirthday"

# Search for year-named folders
rclone lsf gdrive: | grep -E "^20(1[6-9]|2[0-4])"

# Download specific folders once found
rclone copy gdrive:path/to/folder ./recovered/
```

**Option D: Google Apps Script**
- Create a script in script.google.com
- Search Drive programmatically with DriveApp.searchFiles()
- Query: `title contains 'birthday' or title contains 'bridget' or fullText contains 'happybirthdaybridget'`

### Step 2: Triage Recovered Files

For each recovered year:
1. Check if it's a complete site or just assets
2. Move assets to `/{year}/assets/` and rename per convention
3. Add dev nav bar
4. Create manifest.json
5. Add to .gitignore exclusion list (un-ignore it)
6. Update vercel.json if needed
7. Update nav bar links in all active years

### Step 3: Build Sites for Content-Only Years

For 2020 and 2022 (and any others with assets but no HTML):
- Create a simple index.html using the aesthetic of that era
- 2020: Video gallery (COVID year vibes)
- 2022: Photo gallery with gifs

### Step 4: Handle Truly Missing Years

Some years may simply not have had a site made. Options:
- **Skip them** — nav only links to years that exist
- **Create retrospective pages** — simple "this year we did X" with a few photos pulled from camera roll
- **Placeholder pages** — "no site was made this year, here's a gif"

## Search Queries to Try

When searching Drive, try these terms:
- `happybirthdaybridget`
- `happy birthday bridget`
- `bridget birthday site`
- `index.html` (in folders from 2016-2024)
- Filenames like `main.css`, `style.css` near birthday content
- Look in Trash — old files may have been deleted

## Priority Order

1. **2020** — has video content, just needs an HTML shell
2. **2022** — has images/gifs, just needs an HTML shell
3. **2018** — has scss, might be recoverable from Drive
4. **2016** — first missing year, likely exists somewhere
5. **2019, 2021** — may or may not exist
6. **2023, 2024** — most recent gaps, likely exist in Drive
