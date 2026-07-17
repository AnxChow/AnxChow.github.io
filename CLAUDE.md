# ankitachowdhry.com

Ankita's personal website. Not a portfolio — **a little space on the internet**, meant to be
explored through play. This file is the guide to how we build and think about it.

## North star / vibe

- **Personal, not resume-first.** Career exists, but the site is about what Ankita enjoys
  (books, games, music/dancing, travel, etc.) as much as her work.
- **Minimal + lots of white space**, but warm and **scrapbooky** — polaroids, tape, tilt,
  hand-drawn touches, playful hovers. Restraint over clutter.
- **Soft pastel palette.** Nothing harsh.
- **Must NOT look AI-generated.** No generic bento grids, no stock-y hero, no buzzword copy.
  When in doubt, make it feel handmade and specific.
- **Early-2000s web energy** (think MyScene / old Disney.com), done tastefully — the snake-game
  career page is the flagship example. Whimsy in small, deliberate doses.

## Hard-won content rules

- **Copy is lowercase** across the site (intentional style choice).
- **Never fabricate personal specifics.** Don't invent book takes, favorite songs, movie lists,
  career embellishments. Use only what Ankita actually provides. A "few of my favorite things"
  card section was built and cut for feeling inauthentic — the lesson stuck.
- **Career copy is matter-of-fact and resume-grounded** (source: `Ankita_Chowdhry_2026.pdf`,
  gitignored). Terse over flowery. The personal voice comes through in one deliberate line
  (e.g. the AI/ML "feels like magic" note), not in embellishing every bullet.

## Tech stack

- **[Astro](https://astro.build)** static site. Every page is hand-authored HTML/CSS/JS — Astro
  just removes the boring plumbing (shared `<head>`/nav, scoped CSS, image optimization). It does
  **not** impose a uniform look; each page is free to be its own thing.
- No UI framework. Vanilla `<script>` for interactivity (snake game, book modals).
- Builds to `dist/` (gitignored). Deployed to **GitHub Pages via GitHub Actions** (see Deploy).

## Project structure

```
src/
  layouts/Base.astro        # <head>, analytics, sticky nav, footer — every page wraps in this
  components/NavLinks.astro  # the nav icons; `bar` variant (top nav) + `launcher` variant (unused)
  pages/
    index.astro             # home
    career.astro            # DEFAULT career = the Snake game
    career-timeline.astro   # the calm timeline view (opt-in)
    books.astro             # the bookshelf
  styles/global.css         # palette vars, fonts, shared helpers (.hand, a.effect, etc.)
  assets/                   # images imported by pages -> optimized by Astro (use these!)
public/                     # copied as-is to site root (favicon, CNAME, /logos/*)
```

Legacy hand-written site (`index.html`, `blog.html`, `work.html`, `css/`, `js/`, old `assets/`,
`vc/`, etc.) still sits in the repo root. It is **no longer served** once Pages deploys via
Actions (only `dist/` is published). Fine to delete in a future cleanup.

## Design system (`src/styles/global.css`)

- **Palette** (CSS vars): `--paper` `--ink` `--muted` `--line`, pastels `--sage --blue --blush
  --butter --lavender`.
- **Fonts:** `--serif` Fraunces (headings), `--sans` Karla (body), `--accent` Fredoka (rounded,
  friendly labels/asides — replaced a cursive font that felt AI-ish). `Press Start 2P` is loaded
  only on the career/snake page for the arcade HUD.
- Helpers: `.hand` (Fredoka aside), `a.effect` (pastel gradient-highlight link, carried from v1).

## Page notes

- **Home (`/`):** intro blurb + polaroid photo pile. Deliberately sparse. The two columns are
  vertically centered against each other (`.hero { align-items: center }`).
- **Career (`/career`) = Snake game.** Neon-green pixel snake on a CRT-style screen; eat 7 food
  dots to unlock the 7 career stops in order. Crash keeps progress. Arrow keys / WASD / on-screen
  d-pad / swipe. Progressive enhancement: without JS the stops render unlocked (accessible). Links
  to the timeline via a pill button.
- **Career timeline (`/career-timeline`):** the same stops on a winding dashed SVG "walking path".
  Stop positions are fixed `x`/`y` in the frontmatter and the `path` string is hand-authored to
  connect them — **if you change the number of stops or their spacing, update both the `y` values
  and the `path`.** Cross-links back to the snake.
- **Books (`/books`):** a real sideways bookshelf — you see only spines (CSS, vertical text) on a
  wooden ledge, centered. Hover pops a spine up; click "pulls it out" and opens a `<dialog>` modal
  with cover + details. Data-driven from the `books` array; `dateRead`/`rating`/`quote` are
  optional and show gentle "coming soon / add me" placeholders. Ratings are intentionally omitted
  (the whole shelf is her 4–5★ selection).

## Editing content

- **Add a book:** append to the `books` array in `books.astro` (title, author, `spine` color, `h`/`w`
  for the spine size, `added`, optional `take`/`quote`). Vary colors/heights so it reads like a real
  shelf.
- **Add/adjust a career stop:** edit the `stops` array in **both** `career.astro` and
  `career-timeline.astro` (they intentionally share copy). For the timeline, also mind `x`/`y`/`path`.
- **Images:** put new photos in `src/assets/` and import them so Astro optimizes/resizes them
  automatically (the homepage photos went from ~7 MB to ~100 KB this way). `public/` is only for
  files that must be served verbatim.

## Cross-page polish (don't regress)

- All top-level pages align their first heading at the same offset (`main` padding-top `4rem`, ~117px)
  so navigating between pages doesn't jump.
- `scrollbar-gutter: stable` is set on `html` so short vs. tall pages don't shift sideways.

## Deploy

- Push to `master` → `.github/workflows/deploy.yml` builds Astro and deploys to GitHub Pages.
- **One-time setup:** in GitHub repo **Settings → Pages → Build and deployment → Source**, choose
  **"GitHub Actions"** (not "Deploy from a branch"). Until this is set, the deploy step fails.
- Custom domain `ankitachowdhry.com` is preserved via `public/CNAME` (shipped in the build).
- Local dev: `npm run dev` (http://localhost:4321). Prod build check: `npm run build`.

## Future work / backlog

- **New sections:** Spotify playlists ("icons" / favorite songs), a travel map with photos,
  research / things Ankita finds interesting.
- Seed more **book takes** (only a few are written; rest show placeholders).
- Give career stops a more **personal, narrative voice** when ready (currently factual placeholders).
- Decide whether snake stays the default career page long-term.
- Clean up the **legacy root HTML** once fully migrated.
