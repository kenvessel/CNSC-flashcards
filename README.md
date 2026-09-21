# NutriDx — CNSC Study PWA

A phone-friendly Progressive Web App with two study tools for CNSC (Certified Nutrition
Support Clinician) board prep, sharing one look, one install, and one home-screen icon.

## What's in this app

### 1. Flashcard Review (`index.html`) — the default/start page
- **684 flashcards** pulled from your CNSC study guide (242 from the "Official" source set,
  442 "Practice" questions), spanning 10 content areas: Nutrition Assessment, Parenteral
  Nutrition, Complications of Parenteral Nutrition, Introduction to Enteral Nutrition,
  Enteral Nutrition Administration & Monitoring, Condition-Specific Nutrition Support, Home
  Nutrition Support, Nutrition Support of the Older Adult, Nutrition Support of the Pediatric
  Patient, and Ethics & Professional Practice.
- Difficulty-tagged (333 easy / 302 medium / 49 hard) and searchable/filterable by part and
  difficulty in the **Browse** tab.
- **Spaced-repetition scheduling**: after answering, you rate the card Again / Hard / Good /
  Easy (Anki-style). Cards you struggle with resurface sooner; cards you know well get pushed
  further out. A **Study** tab shows what's due today plus new cards.
- **Flagging**: star any card to pin it to a dedicated **Flagged** list for quick re-review,
  independent of the spaced-repetition schedule.
- Each card shows the correct answer plus a rationale for every option (why the right answer
  is right and why each distractor is wrong), not just the correct letter.

### 2. Mock Boards exam (`mock-exam.html`) — linked from the flashcard page
- **340 original multiple-choice questions**, written from scratch (not copied from the
  flashcard set or any copyrighted question bank), across 8 domains:

  | Domain | Questions |
  |---|---|
  | Assessment & screening | 35 |
  | Requirements (macro/micro/fluid) | 35 |
  | Enteral nutrition | 40 |
  | Parenteral nutrition (incl. line management & complications) | 60 |
  | Disease states & populations (incl. pediatrics/neonatal) | 80 |
  | Complications, ethics, monitoring | 30 |
  | USP &lt;797&gt; sterile compounding | 40 |
  | Drug-nutrient interactions | 20 |

- **Selectable session length**: 10, 25, 50, 100, or all 340, sampled proportionally across
  domains so even a short session reflects the full content mix.
- **Answer order reshuffles every attempt** — the correct answer isn't reliably in the same
  position and isn't written to be the longest option, so it can't be guessed from formatting.
- **Free navigation + partial grading**: jump between questions in any order before
  submitting, and submit at any point — only answered questions are scored, skipped ones are
  shown separately rather than counted wrong.
- **Results screen**: overall score, a practice pass/fail marker against a **70% threshold**
  (an explicit self-study proxy — the real CBNM cut score is an undisclosed scaled score, not
  a public percentage), a domain-by-domain accuracy breakdown, and a full question-by-question
  review with rationale for every option, filterable to All / Wrong only / Skipped only.

Both tools link to each other ("Switch to Mock Board Exam" from the flashcard page, "← Back
to Flashcards" from the exam page), so it feels like one app.

## What this is NOT
- **Not real, published CNSC exam questions.** Both the flashcards and the mock exam are
  original study material written to mirror the CNSC content outline and ASPEN-style
  guidance; neither is affiliated with, endorsed by, or sourced from ASPEN or CBNM.
- **Not a certified or scored assessment.** The mock exam's 70% "pass" marker is a self-study
  confidence benchmark, not a prediction of a real exam outcome.
- **Not connected to any backend, account system, or analytics.** There's no login and no way
  for you (the site owner) to see how visitors are doing — all progress lives only in each
  visitor's own browser.
- **Not a clinical or diagnostic reference.** This is a study tool; content should not guide
  actual patient care decisions.

## How progress is saved
Both tools use the browser's own **`localStorage`** — flashcard scheduling/flags/streaks, and
mock-exam attempt history (attempts, best score, last score, pass count) — so progress
survives closing the tab or refreshing the page, but it's **per-browser/per-device only**. It
is not synced across devices, and nothing is ever sent to a server. Clearing browser data, or
opening the app in a different browser/device, starts progress over. A later version could add
cloud sync (e.g., via Supabase) for cross-device backup if that's ever wanted.

## Installing as an app (PWA)
This is a full Progressive Web App:
- A **manifest** (`manifest.webmanifest`) lets it be installed to a phone or desktop home
  screen as a standalone app (no browser chrome).
- A **service worker** (`sw.js`) caches all five files after the first load, so the app keeps
  working offline afterward.
- On iPhone Safari: open the site, tap Share → **Add to Home Screen**.
- On Android Chrome / desktop Chrome: look for the **Install** icon in the address bar, or the
  browser's "Install app" menu option.

## Deploying via GitHub Pages
1. Create a GitHub repository (keep it **Private** if you don't want the question content
   public) and upload all five files in this folder to the repository root:
   `index.html`, `mock-exam.html`, `manifest.webmanifest`, `sw.js`, `icon.svg`.
2. In the repo: **Settings → Pages → Deploy from a branch** → choose your main branch and
   `/ (root)` → **Save**.
3. Open the resulting Pages URL — that's your live app.
4. Open it on your phone and "Add to Home Screen" as above.

If you update either `index.html` or `mock-exam.html` later, bump the `CACHE` constant at the
top of `sw.js` (e.g., `nutridx-v5` → `nutridx-v6`) before you redeploy — otherwise returning
visitors' service workers may keep serving the old cached version instead of your update.

## Updating the mock exam's question bank later
All 340 questions live in one JavaScript array (`QUESTIONS`) inside `mock-exam.html`, in this
format:

```js
{ d: "PN", q: "Question text?", o: ["Correct answer", "Wrong 1", "Wrong 2", "Wrong 3"], c: 0, e: "Explanation of why the correct answer is right and the others are wrong." }
```

- `d` is the domain key — must match one of the keys already in `DOMAIN_LABELS`/`DOMAIN_ORDER`
  near the top of the same script block.
- `o[0]` should always be written as the correct answer — the page shuffles option order at
  runtime, so you never need to manually randomize position when adding questions.
- `c` should stay `0` to match that convention.

To add a brand-new domain, add it to both `DOMAIN_LABELS` and `DOMAIN_ORDER`, and update the
domain list and question-count text on the intro screen (`<div class="domainlist">`) to match.
