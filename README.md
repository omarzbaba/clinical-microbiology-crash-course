# Clinical Microbiology — One-Day Crash Course

An immersive, single-page educational **dashboard** that teaches clinical microbiology to medical students on a one-day rotation, built around four pillars:

1. **Specimens & the diagnostic toolbox**
2. **Reading the organisms**
3. **Susceptibility, resistance & stewardship**
4. **The syndrome-based approach**

It is a working teaching tool — interactive cases, a scored quiz center, a searchable glossary, decision trees and pattern cards — not a static study guide.

> **Live site:** see the **GitHub Pages** URL in the repository’s **About** panel (or the deploy section below).

---

## Highlights

- **Four pillar sections** with clinical-language-first teaching cards (Why it matters · Clinical pattern · What to do next · Student pearl · Advanced pearl · Pitfall · Do not confuse).
- **Quiz Center** — filterable by pillar, with instant feedback: correct answer, why each distractor is wrong, and a teaching pearl. In-session scoring with a live progress ring.
- **Case Lab** — progressive-disclosure cases drawn from real laboratory calls (the 2 a.m. positive blood culture, the confused patient with a "positive" urine, the CSF that cannot wait).
- **Searchable glossary / cheat sheet** — from Gram stain to carbapenemase; the must-know set is flagged.
- **Decision trees** — the Gram stain shortlist, and the empiric → targeted stewardship pathway.
- **Final recap** — take-home points, "how to sound smart on rounds," and good questions to ask on service.
- **Light & dark themes** (true-black dark, clean-white light), scroll-spy navigation, reading-progress bar, restrained ambient background.
- **Accessible**: semantic HTML, keyboard navigable, `prefers-reduced-motion` respected, focus-visible states.

---

## Run it locally

It is a single static file — no build step.

```bash
python3 -m http.server 8123
```

Then visit `http://localhost:8123`. (Opening `index.html` directly also works.)

---

## File structure

```
.
├── index.html      # the entire app — HTML, CSS, and JS in one file
├── PLANNING.md     # Phase 1: learner profile, objectives, content map, IA
├── README.md       # this file (usage + implementation notes)
├── .gitignore
└── .claude/
    └── launch.json # local static-server config for previewing
```

Everything lives in **`index.html`**, organized top-to-bottom as:

1. `<style>` — design tokens (light/dark), layout shell, and every component style.
2. `<body>` — the app shell (sidebar, header, main) and the long-form teaching content as semantic HTML.
3. `<script>` — the data (questions, cases, glossary) and the engine (quiz, cases, glossary search, theme, scroll-spy).

---

## Libraries used

- **None at all.** All interactivity is vanilla JavaScript; every visual is pure CSS. No framework, no bundler, no runtime dependencies.
- **No web fonts.** It uses the native system UI stack (SF Pro on Apple devices, Segoe/Helvetica elsewhere) for an Apple-grade, dependency-free, instant-loading feel.

This keeps it fast, portable, and trivially hostable on GitHub Pages.

---

## How the animations work

- **Reveal-on-scroll:** elements with the `.reveal` class start at `opacity:0` and translate up 12px. An `IntersectionObserver` (rooted on the scrolling `<main>`) adds `.in` to fade them in once, then unobserves. Disabled under `prefers-reduced-motion`.
- **Ambient background:** a single soft pair of radial washes on a fixed `body::before` layer (opacity ~0.05) that drifts a few pixels over 26s, plus a barely-visible SVG grain overlay. No particles, no canvas.
- **UI motion:** CSS transitions only, on compositor-friendly properties (`opacity`, `transform`), ~200–400 ms with an ease-out curve.

---

## How to customize the content later

The repetitive teaching content is **data-driven** — edit plain arrays near the top of the `<script>` in `index.html`:

| What | Where | Shape |
|---|---|---|
| Quiz questions | `const QUESTIONS = [ … ]` | `{ id, pillar:'dx'\|'bug'\|'abx'\|'syn', diff:'easy'\|'moderate'\|'integrative', stem, opts:[…], answer:<index>, explain, wrong, pearl }` |
| Inline checkpoints | `const CHECKPOINTS = [ … ]` | same shape as a question |
| Cases | `const CASES = [ … ]` | `{ slot:'dx'\|'bug'\|'abx'\|'syn'\|'integrated', tagClass, tagLabel, title, blurb, labs?:[{k,v,s}], steps:[{label, prompt, reveal}] }` |
| Glossary | `const GLOSSARY = [ … ]` | `{ term, abbr?, def, must?:true }` |

Add an object to an array and it renders automatically — the hero stat counters update too.

**Long-form teaching content** is semantic HTML inside each `<section>`; edit the markup directly. The labeled callouts use `class="callout callout--why|pattern|next|pearl|adv|pitfall|dnc"` paired with a matching `<span class="tag tag--…">`.

**Personalize it for a learner:** add the learner’s name to the hero lede, the sidebar chip (`.learner-meta`), and the recap — see the sibling transfusion-medicine dashboards for the pattern.

**Re-theme:** all colors live in the `:root` / `[data-theme]` blocks at the top of `<style>`. The pillar hues are `--dx` (teal), `--bug` (ochre), `--abx` (rose), `--syn` (green); the chrome accent is `--accent`.

---

## Deploy (GitHub Pages)

Published from the `main` branch root:

1. **Settings → Pages → Build and deployment → Source: Deploy from a branch → `main` / `root`.**
2. Wait ~1 minute; the live URL appears at the top of the Pages settings and in the repo **About** panel.

---

## A note on accuracy

The content is an educational synthesis for medical-student teaching, written and then adversarially fact-checked for accuracy, unsafe simplification, and fabrication. It is **not** a clinical protocol and contains **no fabricated citations or invented breakpoint values**.

Antimicrobial selection, susceptibility interpretation, and dosing are institution- and time-dependent. Always confirm against your local antibiogram, current **CLSI**/**EUCAST** breakpoints, current **IDSA** guidance, and your institution’s policies before acting on a patient. See [`PLANNING.md`](PLANNING.md) for the source bodies consulted.
