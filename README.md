# 🌙 Ramadan Lockscreen Generator

Automatically generates a beautiful phone lockscreen wallpaper (1080×2400) with daily prayer times for Ramadan 2026.

Supports two mosques:
- **Masjid Faizul Islam** (Perry Barr, Birmingham)
- **Masjid Quba Trust** (Handsworth, Birmingham)

## How It Works

1. A GitHub Actions workflow runs every day at **2:00am GMT** during Ramadan
2. It reads the prayer times from the CSV timetables
3. Renders a styled HTML template into a high-resolution PNG
4. Commits the image to `output/` and a stable-URL copy to `latest/`

## Stable URLs for Automation

Once the repo has GitHub Pages enabled (or you just fetch raw files), the latest lockscreen is always at:

```
latest/ramadan_lockscreen_faizul_latest.png
latest/ramadan_lockscreen_quba_latest.png
```

Use these URLs in your phone automation (iOS Shortcuts / Android Tasker) to always fetch today's image.

## Setup

### 1. Create the GitHub repo

```bash
gh repo create ramadan-lockscreen --private --source=. --push
```

### 2. Enable GitHub Actions

Actions should be enabled by default. The workflow will start running automatically on schedule.

### 3. Manual trigger

You can also generate any day manually from the **Actions** tab:
- Click "Generate Daily Ramadan Lockscreen"
- Click "Run workflow"
- Optionally set a specific date and mosque

### 4. Phone automation

**iPhone (Shortcuts):**
1. Create a new Shortcut automation triggered at e.g. 3:00am daily
2. Add "Get Contents of URL" → point to the raw GitHub URL of your `latest/` PNG
3. Add "Set Wallpaper" → set as Lock Screen
4. Done — your lockscreen updates itself every morning

**Android (Tasker):**
1. Create a profile triggered at 3:00am
2. Task: HTTP Get → download the `latest/` PNG URL
3. Task: Set Wallpaper → apply to lock screen

## Running Locally

```bash
pip install playwright Pillow
playwright install --with-deps chromium

# Generate for both mosques, today's date
python generate.py

# Generate for one mosque on a specific date
python generate.py faizul 2026-02-22
python generate.py quba 2026-03-01
```

## Project Structure

```
├── .github/workflows/generate.yml   # Daily GitHub Actions workflow
├── data/
│   ├── masjid_faizul_islam_ramadan_2026.csv
│   └── masjid_quba_ramadan_2026.csv
├── templates/
│   └── lockscreen.html               # HTML template with placeholders
├── output/                            # Generated PNGs (date-stamped)
├── latest/                            # Stable-URL copies (always today's)
├── generate.py                        # Main generation script
└── requirements.txt
```
