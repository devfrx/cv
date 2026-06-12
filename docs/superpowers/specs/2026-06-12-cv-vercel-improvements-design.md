# CV — Vercel readiness & improvements

Date: 2026-06-12 · Status: approved by user in chat

## Goal

Make the public CV (single static `index.html`) deployable on Vercel and improve hygiene, SEO, UX and maintainability. No build step, no framework — stays a single static file.

## Scope

### 1. Immediate fixes
- Remove stray `size` text after the name div (uncommitted typo).
- Add the missing `data-t="title-tag"` to the header subtitle so the language toggle translates it; align its static text with the current EN copy ("IT Technician Apprentice · Backend/Frontend Developer · AI Enthusiast").
- Remove the dead `portfolio` link (`href="#"`); contacts show GitHub only (user decision: no public email).
- Add `rel="noopener noreferrer"` to all `target="_blank"` links.

### 2. Vercel
- Add minimal `vercel.json`: `cleanUrls: true` plus security headers (`X-Content-Type-Options: nosniff`, `X-Frame-Options: DENY`, `Referrer-Policy: strict-origin-when-cross-origin`). Static deploy, no build.

### 3. SEO / sharing
- `<title>Francesco Idato — CV</title>`, meta description, Open Graph + Twitter card tags.
- Favicon as inline SVG data URI (no extra file).
- JSON-LD `schema.org/Person` block.
- Excluded (user chose not to include extras): og:image asset, Vercel Analytics.

### 4. UX
- Persist theme and language in `localStorage`, but only when the user explicitly clicks a toggle; first visit auto-detects via `prefers-color-scheme` and `navigator.language` (it → IT, else EN).
- Early inline script right after `<body>` applies the dark class before content paints (no theme flash).
- New PDF button in the fixed controls calling `window.print()` (print CSS already exists); tooltip translated per language.

### 5. Content deduplication — approach B (chosen)
- The static EN HTML is the canonical EN content. On load, the script captures `innerHTML` of every `[data-t]` element into `T.en`; the ~50-line hardcoded `T.en` object is deleted. Only `T.it` stays hand-written.
- Rationale: content lives in one place, no-JS/SEO rendering stays intact, EN/IT drift becomes impossible. Rejected approach A (render everything from JS) because the CV would disappear without JavaScript.
- Drop the unused `coming-soon` key.

### 6. Repo cleanup
- Delete untracked `professional_cv.html` (superseded duplicate; deletion explicitly confirmed by user).

## Verification
- Load the page locally: EN/IT toggle translates everything including the subtitle; dark/light persists across reload only after an explicit click; PDF button opens the print dialog; no stray text in header; valid HTML/JS (no console errors).
