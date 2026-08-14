# Implementation Tasks: UR RES: the resume builder

This document provides a detailed, phase-by-phase execution plan for building the single-page UR RES: the resume builder web app specified in [prd.md](file:///c:/Users/CC4/Desktop/sample/prd.md). Follow each phase sequentially.

---

## Project Overview & Technical Constraints
- **Stack**: Pure HTML5, Vanilla CSS3, and Vanilla JavaScript (ES6+). Single file `index.html` (or modular `index.html`, `styles.css`, `script.js`).
- **Dependencies**: None (no external libraries, frameworks, or databases).
- **Target File**: `index.html`

---

## Phase 1: Core HTML Structure & Semantic Layout

### Goal
Build the semantic HTML shell of the application including the App Header, Form Controls (left side), and Resume Preview Document (right side).

- [x] **1.1 HTML Shell & Header Setup**
  - Create `index.html` with standard boilerplate (`<!DOCTYPE html>`, meta viewport for mobile, title "UR RES: the resume builder").
  - Add an app header/navbar containing:
    - App title ("UR RES: the resume builder")
    - Light/Dark theme toggle button (`#themeToggle`) with sun/moon icon.
    - Download PDF button (`#downloadPdfBtn`) with printable icon/label.

- [x] **1.2 Left Column: Resume Form Container (`#formContainer`)**
  - Create `<form id="resumeForm">` containing organized section cards:
    - **Personal Information**: Inputs for `#fullName` (text), `#jobTitle` (text), `#email` (email), `#phone` (tel), `#location` (text).
    - **Professional Summary**: Textarea for `#summary` (4-5 rows).
    - **Work Experience**: Container `#experienceList` + button `#addExperienceBtn` ("+ Add Experience").
    - **Education**: Container `#educationList` + button `#addEducationBtn` ("+ Add Education").
    - **Skills**: Input `#skills` (text input with placeholder "e.g. JavaScript, HTML, CSS, Git").

- [x] **1.3 Right Column: Resume Live Preview Container (`#previewContainer`)**
  - Create `<div id="resumePreview" class="resume-sheet">` styled like an A4 paper page:
    - **Header Block**: `#previewName` (h1), `#previewJobTitle` (h2), contact info line (`#previewContact`).
    - **Summary Section**: Section heading "Professional Summary", content body `#previewSummary`.
    - **Experience Section**: Section heading "Work Experience", dynamic list container `#previewExperience`.
    - **Education Section**: Section heading "Education", dynamic list container `#previewEducation`.
    - **Skills Section**: Section heading "Skills", dynamic container `#previewSkills`.

**Phase 1 Verification**: Open `index.html` in browser. Verify all form fields, buttons, and preview placeholders are visible on the page.

---

## Phase 2: Design System, Layout & Theme Engine

### Goal
Implement CSS styling, split-screen desktop layout, responsive mobile layout, and persistent Light/Dark theme toggle.

- [x] **2.1 Global CSS Variables & Base Styles**
  - Define CSS custom properties on `:root` and `[data-theme="dark"]`:
    - `--bg-primary`: App main background (`#f8f9fa` light / `#1a1a1a` dark).
    - `--bg-card`: Form/Header card background (`#ffffff` light / `#242424` dark).
    - `--text-primary`: Text color (`#212529` light / `#e0e0e0` dark).
    - `--text-muted`: Secondary text color (`#6c757d` light / `#a0a0a0` dark).
    - `--accent-color`: Primary action color (`#2563eb` light / `#3b82f6` dark).
    - `--border-color`: Border color (`#e5e7eb` light / `#374151` dark).
    - `--font-sans`: Clean modern font stack (`system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif`).

- [x] **2.2 App Layout & Responsive Grid**
  - Main app wrapper: Flexbox/Grid side-by-side layout (Desktop: Form 50%, Preview 50% or fixed preview aspect ratio).
  - Media query (`@media (max-width: 768px)`): Stack form vertically above preview.
  - Custom scrollbars & form input styling (rounded borders, smooth focus rings).

- [x] **2.3 Resume Sheet Styling (Isolated)**
  - `.resume-sheet` MUST ALWAYS render with:
    - `background-color: #ffffff !important;`
    - `color: #111827 !important;`
    - High readability typography, subtle dividers (`border-bottom: 1px solid #e5e7eb`), margin/padding resembling real printed document.

- [x] **2.4 Light/Dark Theme Switcher Logic**
  - Write theme toggling function in JavaScript:
    - Reads saved theme from `localStorage.getItem('resume_app_theme')`.
    - Defaults to system preference or `'light'`.
    - Sets `data-theme` attribute on `document.body`.
    - Toggles sun/moon icon state on button click.
    - Saves selection to `localStorage` on change.

**Phase 2 Verification**: Click theme toggle button. The app UI toggles between dark/light mode seamlessly, while the resume preview sheet remains crisp white with dark text. Refresh page to confirm theme persists.

---

## Phase 3: JavaScript State Management & Dynamic Form Blocks

### Goal
Implement dynamic item addition/removal for Experience & Education blocks and reactive state storage.

- [x] **3.1 Central Data Structure**
  - Define state object template (`resumeData`).

- [x] **3.2 Dynamic Experience Block Manager**
  - Implement `addExperienceItem(data = {})`:
    - Appends a fieldset/card inside `#experienceList` containing inputs for Company, Role, Dates, Description, and Remove button.
  - Bind remove button click event to remove item DOM node and re-render.

- [x] **3.3 Dynamic Education Block Manager**
  - Implement `addEducationItem(data = {})`:
    - Appends a card inside `#educationList` containing inputs for School, Degree, Dates, and Remove button.
  - Bind remove button click to remove node and re-render.

- [x] **3.4 Initial Sample Data Setup**
  - Pre-populate form state on first page load with clean default values (1-2 default Experience & Education blocks) so preview renders immediately.

**Phase 3 Verification**: Click "+ Add Experience" and "+ Add Education". Confirm new dynamic input cards appear in the form. Click "Remove" to delete cards.

---

## Phase 4: Real-Time Live Preview Synchronization Engine

### Goal
Bind form input events to instantly update the resume preview DOM without requiring a submit button.

- [x] **4.1 Event Delegation & Input Binding**
  - Attach an `'input'` event listener to `#resumeForm` using event delegation to catch all text typing and dynamic block changes instantly.
  - Implement `updatePreview()` function:
    - Reads values from personal fields (`#fullName`, `#jobTitle`, `#email`, `#phone`, `#location`, `#summary`).
    - Formats contact line (`Email | Phone | Location`) filtering out empty fields cleanly.
    - Updates `#previewName`, `#previewJobTitle`, `#previewContact`, and `#previewSummary`.

- [x] **4.2 Dynamic Lists Renderer**
  - **Render Experience**: Loop over `#experienceList` item cards, build HTML structure for each role, insert into `#previewExperience`. Show section heading only if items exist.
  - **Render Education**: Loop over `#educationList` item cards, build HTML structure for each education entry, insert into `#previewEducation`. Show section heading only if items exist.
  - **Render Skills**: Split `#skills` value by comma, trim whitespace, render as skill badges in `#previewSkills`.

- [x] **4.3 Edge Case & Empty State Handling**
  - Ensure empty fields do not leave stray bullets or pipe `|` separators in the preview.
  - Ensure HTML tags entered by user are escaped/rendered safely via `escapeHtml()` to prevent XSS.

**Phase 4 Verification**: Type into any form input field or dynamic block. Observe instant (<16ms) matching updates on the right-hand preview panel.

---

## Phase 5: PDF Export & Print Styling (`@media print`)

### Goal
Configure `window.print()` export with custom print CSS to produce a clean, professional single-page PDF output without app UI clutter.

- [x] **5.1 PDF Trigger Action**
  - Attach click handler to `#downloadPdfBtn` to execute `window.print()`.

- [x] **5.2 Print Stylesheet Rules (`@media print`)**
  - Add `@media print` rules in CSS:
    - Hide all non-resume elements: `#formContainer`, header/navbar, buttons, theme toggle (`display: none !important;`).
    - Remove body background, padding, margins, shadows, scrollbars, and borders.
    - Set `.resume-sheet` to position absolute, full width, 0 margins, no box shadows.
    - Set page margins via `@page`: `@page { size: A4; margin: 12mm 15mm; }`.
    - Ensure exact color reproduction: `print-color-adjust: exact;`.
    - Prevent section page breaks: `page-break-inside: avoid; break-inside: avoid;`.

**Phase 5 Verification**: Click "Download PDF". Verify print preview dialog shows ONLY the resume sheet, perfectly positioned, without form inputs, header controls, or background colors.

---

## Phase 6: Final Verification & QA Checklist

### Goal
Ensure all acceptance criteria from [prd.md](file:///c:/Users/CC4/Desktop/sample/prd.md) are met.

- [x] **6.1 Functional Acceptance Tests**
  - [x] Typing in form updates live preview on every keystroke.
  - [x] Dynamic Experience blocks add and delete accurately.
  - [x] Dynamic Education blocks add and delete accurately.
  - [x] Comma-separated skills format correctly in preview.
  - [x] Light/Dark theme toggle switches app UI and persists across page reloads.
  - [x] Resume sheet maintains white background & dark text in both dark & light app modes.
  - [x] Download PDF opens native print window and generates clean print output.
  - [x] Mobile responsive layout stacks form above preview smoothly.

- [x] **6.2 Browser Compatibility & Code Quality**
  - Validate clean HTML structure (valid tags, proper IDs/classes).
  - Verify script execution without console errors or warnings.

