# PRD: UR RES: the resume builder (Minimal — 30 min build)

## Goal
A single-page web app where a user fills a form, sees a live resume preview, and downloads it as a PDF. Supports light/dark theme toggle.

## Stack
Single HTML file — HTML + CSS + vanilla JS. Use `window.print()` for PDF export (no libraries needed). No backend, no database.

## Features (in scope only)

### 1. Form (left side)
Fields:
- Full Name, Job Title, Email, Phone, Location
- Summary (textarea)
- Experience: repeatable block — Company, Role, Dates, Description. "+ Add" button.
- Education: repeatable block — School, Degree, Dates. "+ Add" button.
- Skills: single text input, comma-separated

### 2. Live Preview (right side)
- Updates instantly on every keystroke (no submit button).
- Clean single resume layout — name/title at top, then Summary, Experience, Education, Skills sections.
- Always renders with white background + dark text (regardless of app theme), since it's the print output.

### 3. Download PDF
- One button: "Download PDF".
- Use `window.print()` with print CSS (`@media print`) that hides the form and only shows the preview, no margins/buttons.

### 4. Light / Dark Theme
- Toggle button (sun/moon icon) in top corner, switches app UI only (not the resume preview).
- Use CSS variables + `data-theme` attribute on `<body>`.
- Save choice in `localStorage` so it persists on reload.
- Dark = dark gray bg (`#1a1a1a`) + light text. Light = white bg + dark text.

### 5. Layout
- Desktop: form left, preview right, side-by-side.
- Mobile: stack vertically (form on top, preview below).

## Out of scope (skip entirely)
No login, no templates/theme picker, no cloud save, no AI content, no multiple pages, no PNG export — just one template, one PDF export method, local-only.

## Done when
- [ ] Typing in the form updates the preview live
- [ ] Download PDF produces a clean printable resume
- [ ] Light/dark toggle works and persists on refresh
- [ ] Works on mobile and desktop