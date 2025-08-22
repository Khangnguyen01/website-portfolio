# 🚀 Modern Portfolio (Single-File HTML)

A sleek, mobile-first portfolio site built with **vanilla HTML/JS** and **Tailwind CSS (CDN)**. It includes smart mobile optimizations, a dark mode toggle, filterable projects, animated hero text (desktop only), accessibility helpers, and a tiny client-side “log capture” utility to help you debug.

> This README explains how the file is organized, how to customize it, and tips for deployment & performance.

---

## ✨ Highlights

- **Device-aware UI:** Early device detection adds `.is-mobile` or `.is-desktop` to `<html>` to tailor styles, fonts, and animations.
- **Mobile performance optimizations:** Lighter gradients, no heavy animations, and reduced effects on small screens.
- **Dark mode:** Uses the `dark` class with a persistent preference in `localStorage`, plus `prefers-color-scheme` fallback.
- **Animated hero name (desktop-only):** Fancy per-letter color cycling and “bubbles” with a reduced-motion-safe path.
- **Filterable project gallery:** Click any skill badge to filter the projects copied into a “filtered” grid; live counts on badges update automatically from project metadata.
- **Accessible navigation & controls:** Skip link, focus rings, ARIA states, and keyboard-friendly interactions.
- **Debug logs helper:** Capture console logs/errors, auto-persist a recent ring buffer, and export logs with one shortcut.

---

## 🧱 Tech Stack

- **HTML5** (single file)
- **Tailwind CSS (CDN)** with custom theme extension (brand colors, shadows, animations)
- **Google Fonts** (Inter; plus ornamental desktop-only fonts)
- **(Conditional) Font Awesome** icons for **desktop** only
- **Vanilla JavaScript** (no framework)

---

## 📂 File Structure (Single File)

```
web-portfolio_test.html
```

> All markup, styles, and scripts live in this one file. There is no build step required—just open it in a browser or deploy it as a static page.

---

## 🧭 Sections Overview

- **Header / Navigation**
  - Glassy nav shell that reveals/hides on scroll, with a hamburger **mobile menu**.
  - “Back to Top” brand link; skip-to-content link for accessibility.

- **Hero**
  - Intro headline with the animated **name** (desktop only), short bio, and CTA buttons.
  - Card with profile image, current role, and optional CV link.

- **About**
  - Narrative paragraph with background, interests, and working style.

- **Skills**
  - Row of **skill badges**. Each badge has a `data-skill` value that maps to projects (see “Projects” section). Counts auto-fill based on what’s on your Featured Projects grid.

- **Projects**
  - **Featured Projects**: cards use a `data-skills` string (comma-separated) to describe the technologies used.
  - **Projects by Skill**: a hidden section that shows when you click a skill badge; it **clones** matching project cards into a filtered grid; includes a **Clear** button.

- **Experience**
  - Timeline-style entries with organization links, roles, timespans, and bullet points.

- **Education**
  - Single card with degree info, chips (GPA, achievements), and relevant coursework.

- **Contact**
  - Email, LinkedIn, GitHub, and phone links. (Icons are **desktop-only** by default to save weight on mobile—see “Icons on Mobile” note.)

- **Footer**
  - Dynamic year, social links, and the **Log Capture** helper (see “Debugging” below).

---

## 🔧 Customization Guide

### 1) Branding & Metadata
- **Title & description**: Update `<title>` and meta description.
- **Open Graph**: Update `og:title`, `og:description`, and `og:image` for richer shares.
- **Favicon**: Replace `/favicon.ico` (and the OG image) with your assets.

### 2) Hero
- **Name**: Replace the text inside `#heroName` (e.g., “Khang Phuc Nguyen”).
- **Tagline & bio**: Edit the `<h1>` and the following paragraph.
- **Profile image**: Replace the `<img>` `src` with your own URL/asset.
- **CV link**: Point “Detailed CV” to your resume or remove it.

### 3) Skills & Project Filtering
- **Badges**: Each button in **Skills** has `data-skill="..."`. Use lowercase slugs, e.g., `data-skill="pandas"`.
- **Projects**: Each project card has `data-skills="comma,separated,slugs"`. Make sure these **match** the badge slugs or they won’t filter in.
- **Counts**: Badge counts are computed at runtime by scanning project cards—no manual updates needed.
- **Add projects**: Duplicate a card in Featured Projects and adjust the link, description, and `data-skills` list.

### 4) Experience & Education
- Replace organization names, roles, timespans, and bullet points.
- Update coursework, GPA chips, and achievements.

### 5) Contact
- Update `mailto:`, LinkedIn, GitHub, and `tel:` to your contacts.
- **Icons on Mobile**: Font Awesome is loaded **only on desktop** by default (for performance). If you want icons on mobile:
  - Option A: Load Font Awesome for all devices (remove the device check).
  - Option B: Replace FA icons with **inline SVGs** (lightweight and reliable).

---

## 🎛️ Behavior & Settings

### Device Detection
- Runs **very early** to add `.is-mobile` or `.is-desktop` on `<html>`, which the CSS uses to:
  - Toggle desktop-only fonts/effects.
  - Swap heavy gradients for lighter ones on mobile.
  - Disable complex animations and `will-change` on mobile.

### Dark Mode
- Uses Tailwind `dark` class.
- Reads `localStorage.theme`, otherwise falls back to `prefers-color-scheme`.
- Toggle buttons update localStorage to persist preference.

### Header Reveal & Mobile Menu
- The header appears when you **scroll down** and hides when you scroll back up near the top.
- **Hamburger** toggles a glassy dropdown menu on mobile; it auto-closes when a menu link is clicked.

### Hero Name Animation (Desktop Only)
- On desktop, the script splits your name into **spans per character**, cycles a highlight, and runs subtle “bubble” particles.
- Honors `prefers-reduced-motion` to keep things calm when requested.

### Skills → Filtered Projects
- Clicking a skill badge **clones** matching project cards (by `data-skills`) into the filtered section.
- The **Clear** button resets the view and scrolls back to the Featured Projects grid.

---

## 🪲 Debugging (Log Capture Helper)

- Console logs/errors are captured in a **ring buffer** (configurable size).
- The most recent entries are periodically synced to `localStorage` (last ~200 by default).
- **Shortcut**: Press **Ctrl/Cmd + Shift + L** to download a `debug-logs.json` snapshot.
- You can also call `window.saveLogs('my-logs.json')` from the console.

Use this when debugging on a user’s device—you get their browser/URL, timestamps, and messages in one file.

---

## ⚙️ Performance Notes

- **Font Awesome** is loaded **only on desktop** to save bandwidth on mobile.
- **Mobile**: Gradients use smaller radii; heavy animations are disabled; `background-attachment` uses `scroll` to reduce work.
- **Images**: Use `loading="lazy"` on large images. Replace remote images with compressed, local assets if needed.
- **Tailwind (CDN)**: For production, consider prebuilding your CSS to ship only the classes you use (smaller, faster).

---

## ♿ Accessibility

- **Skip to content** link for keyboard users.
- **Focus rings** and `aria-pressed` states on interactive elements.
- **Reduced motion** handling for hero animation.
- Descriptive **aria-labels** on social links and toggles.

---

## 🧪 Known Issues / Quick Fixes

- **Invalid Tailwind class**: One project badge uses `bg--400` (double dash). Replace with a valid class, e.g., `bg-sky-400` or `bg-blue-400`.
- **Icons on mobile**: If you keep FA desktop-only, the contact/footer icons won’t render on phones. See “Icons on Mobile” above.
- **Empty links**: Replace any placeholder `href=""` with your real URLs so users aren’t led nowhere.

---

## 🚀 Run & Deploy

- **Local**: Double-click the HTML file to open in your browser.
- **GitHub Pages**: Put the file in a repo, enable Pages, and set the root as the source.
- **Netlify / Vercel**: Drag-and-drop (Netlify) or import the repo (Vercel). No build step is required.
