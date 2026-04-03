# JWM Attendance System — Google Sheets Edition

Multi-device student attendance system for John Wyatt Montessori School.

## Architecture

```
Browser (index.html) ←→ Google Apps Script Web App ←→ Google Sheets
```

All data is stored in Google Sheets and accessible from any device with the URL.

---

## Setup Guide

### Step 1 — Create the Google Sheet

1. Go to [sheets.google.com](https://sheets.google.com) → create a new blank sheet
2. Name it: **JWM Attendance Database** (or anything you like)
3. Copy the Sheet ID from the URL:
   `https://docs.google.com/spreadsheets/d/**SHEET_ID_HERE**/edit`

### Step 2 — Deploy the Apps Script Backend

1. In your Google Sheet, click **Extensions → Apps Script**
2. Delete the default `Code.gs` content and paste the entire `Code.gs` file from this repo
3. Paste your Sheet ID into the `SHEET_ID` variable at the top:
   ```js
   const SHEET_ID = 'your-sheet-id-here';
   ```
4. Click **Run → `setupSheets`** (first run only — creates all tab headers)
   - You'll be asked to authorise the script — click **Allow**
5. Click **Deploy → New deployment**
   - Type: **Web app**
   - Execute as: **Me**
   - Who has access: **Anyone**
6. Click **Deploy** → copy the Web App URL (looks like `https://script.google.com/macros/s/.../exec`)

### Step 3 — Deploy the Frontend

1. Create a new GitHub repository (public)
2. Push `index.html` + `.github/workflows/deploy.yml` to the `main` branch
3. Go to **Settings → Pages → Source → GitHub Actions**
4. Push to trigger deploy — your URL will be: `https://yourusername.github.io/repo-name/`

### Step 4 — First-Time Connection

1. Open your GitHub Pages URL in any browser
2. A setup screen will appear — paste your Apps Script Web App URL
3. Click **Connect & Load Data** — done!
4. The URL is remembered in the browser. Share the GitHub Pages link with all staff.

---

## Re-deploying the Apps Script (after changes)

> **Important:** Every time you edit `Code.gs`, you must create a **new deployment** (not update the existing one) for changes to take effect. Copy the new URL and update it in the app's setup screen.

---

## Sheets Structure (auto-created)

| Sheet | Columns |
|-------|---------|
| Students | id, name, dob, joinDate, group, mainCampus, notes, photo |
| Attendance | id, studentId, date, campus, checkIn, checkOut, afterSchool, loa, teacherRemark, parentRemark |
| Semesters | id, name, startDate, endDate |
| Holidays | id, semesterId, date, name |
| Settings | key, value |

## Attendance Rules

| Status | Rule |
|--------|------|
| Present | Checked in |
| Half Day | Checked in AND out before 12:00 |
| Late | Check-in after 09:00 |
| Leave of Absence | LOA box ticked |
| Absent | No check-in + no LOA |
