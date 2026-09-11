# Session Handoff — krishpri-1811.github.io

Context document for a follow-on Claude session. Everything below was completed and
deployed; the last section lists what is deliberately left undone.

- **Repo:** `KRISHPRI-1811/krishpri-1811.github.io` (GitHub Pages, user site)
- **Default branch:** `main` — at `613c10e` at end of session
- **Working branch used:** `claude/github-pages-update-wqv9v8`
- **Session date:** 2026-09-01 (earlier commits dated 2026-08-10 / 08-17 / 08-20)
- **Build:** GitHub Pages default Jekyll (`dynamic/pages/pages-build-deployment`). No
  custom workflow, no tests, no package manifest. Static HTML + one CSS file.

---

## 1. Current file inventory (`main`)

| Path | Size | Notes |
|---|---:|---|
| `index.html` | 7,858 | Homepage: hero, latest updates, selected work, writing, footer |
| `style.css` | 13,562 | Single stylesheet for homepage **and** both article pages |
| `resume.pdf` | 256,335 | Linked from footer as `resume.pdf` |
| `assets/photo.jpg` | 257,898 | Hero photo, **700 × 933 px (3:4 portrait)** |
| `writing/capacitor-deep-dive.html` | 47,609 | Article (pre-existing content, re-added this session) |
| `writing/class-e-tapeout-proposals.html` | 50,289 | Article (new this session) |
| `writing/assets/class-e-relationship.png` | 70,620 | Figure — used twice (intro + Figure 5) |
| `writing/assets/class-e-fig1-funnel.png` | 80,138 | Figure 1 |
| `writing/assets/class-e-fig2-timing.png` | 58,489 | Figure 2 |
| `writing/assets/class-e-fig3-project1.png` | 80,810 | Figure 3 |
| `writing/assets/class-e-fig4-project2.png` | 100,737 | Figure 4 |
| `README.md` | 1,994 | Untouched this session |

---

## 2. Pull requests merged (all into `main`, all Pages deploys green)

| PR | Merge commit | Title |
|---|---|---|
| #1 | `151dc1e` | Add capacitor deep-dive article, update resume |
| #2 | `d34e138` | Remove three work items from selected work section |
| #3 | `0b48029` | Trim work descriptions from latest updates internship entries |
| #4 | `1c20ec3` | Rewrite hero intro |
| #5 | `86a89db` | Enlarge hero photo and rewrite intro |
| #6 | `3504626` | Make hero photo span the full height of the intro text |
| #7 | `cd380e5` | Add Class-E tape-out proposals article |
| #8 | `e6a31e8` | Align article figures to the text column width |
| #9 | `44df973` | Update resume.pdf |
| #10 | `6884813` | Update resume.pdf |
| #11 | `3e1dbfb` | Update resume.pdf |
| #12 | `613c10e` | Update resume.pdf |

Every merge triggered `pages build and deployment`; all 12 concluded **success**.

---

## 3. What changed, by area

### 3.1 Hero section (`index.html` + `style.css`)

The intro was rewritten twice (PR #4, then #5) and now stands as **four**
`<p class="hero-intro">` paragraphs:

1. Name + pronunciation "(Pri-th-vee)", MSEE at UW, BS in ECE with Applied Math minor
2. Husky Robotics / FSAE Electronics, Power Distribution PCBs, concentration
3. Current interests — Signal/Power Integrity, Electro-Magnetic-Mechanical Systems
4. Personal interests, closing invitation to connect

Implementation constraints to preserve:

- `id="about"` lives on the **first** paragraph only — the nav `#about` anchor depends on it.
- "Periodically Updated Here" in paragraph 4 links to `#writing` (the on-page section,
  not a specific article).
- University of Washington links to `https://www.ece.uw.edu/`; "(UW)" sits outside the anchor.

### 3.2 Hero photo — stretches to text height (PR #6)

This is the least obvious part of the CSS. The photo is sized by the **text**, not by a
fixed value:

```css
.avatar {
  flex: none;
  position: relative;
  align-self: stretch;        /* height comes from .hero-text beside it */
  width: min(440px, 44%);     /* capped but fluid */
  min-height: 340px;
  overflow: hidden;
}
.avatar img {
  position: absolute; inset: 0;   /* absolute so the photo never sets the row height */
  width: 100%; height: 100%;
  object-fit: cover;
  object-position: center 60%;    /* subject sits low in frame */
}
```

Measured behaviour (headless Chromium): photo renders **440 × 605** at ≥1100px viewport,
exactly equal to the text column height (delta 0). Side crop ~3% at ≥1100px, rising to
~15% in the narrow 1001–1020px band.

**The stacking breakpoint was moved 600px → 1000px, and that is load-bearing.** Below
~1000px there is no room for a full-height photo beside the text: the text column narrows,
grows several hundred px taller, and the stretched photo crops severely — measured **66%
crop plus horizontal page overflow at a 700px viewport** before the fix. Stacked, the photo
uses `aspect-ratio: 700 / 933` so it is never cropped.

- `@media (max-width: 1000px)` — stacks, `align-self: flex-start`, `width: min(360px, 62%)`, `aspect-ratio: 700/933`, `min-height: 0`
- `@media (max-width: 600px)` — additionally centers, `width: min(300px, 78vw)`

A dead `.avatar { width: 220px }` was removed from the ≤480px block (it was always
overridden by the later 600px rule).

### 3.3 Page width

`--measure` went **960px → 1080px** and `.hero-intro` max-width **58ch → 68ch**, so
paragraph lines run longer. `--measure` drives `.wrap` and `.topnav`, so this widened the
whole homepage, keeping the hero aligned with the sections below.

Note `.article-wrap` is separate and still **720px** (672px of content after padding).

### 3.4 Latest updates (PR #3)

Five internship entries were trimmed so each ends at the company name. Removed clauses:

| Date | Removed |
|---|---|
| Sep 2026 | ", designing and testing servo controllers for Nova, their fully reusable launch vehicle" |
| Jun 2026 | ", working on 48V zonal ECUs for the next-generation EV platform" |
| Jan 2026 | ", working on hardware bring-up and qualification of the V-BAT ViDAR processing module" |
| Jun 2024 | ", working on GPU power and signal validation for DGX/MGX server platforms" |
| Jan 2023 | ", developing a custom Deep Reactive Ion Etching process for MEMS through-silicon vias" |

The two **education** entries (Sep 2025 Master's, Jun 2025 Bachelor's) kept their
"focusing on…" / "concentrating in…" wording — they describe a degree, not internship work.

### 3.5 Selected work (PR #2)

Three items removed at the user's request: GPU-Emulator PCB for NVIDIA GB200 (2024),
V-BAT Battery Management System (2026), Custom DRIE Bosch Process (2023). Two remain:
900 W Isolated Resonant LLC DC–DC Converter and 26 kVA Quad-Motor Inverter (both 2025–26).

Note: V-BAT is still referenced in the Jan 2026 "latest updates" entry — that's a different
section and was intentionally left alone.

### 3.6 Class-E article (PR #7)

Converted from a supplied `.docx` into `writing/class-e-tapeout-proposals.html`, matching
the capacitor article's layout (meta table, TOC, numbered sections, equations, data tables,
figures, references). Mapping decisions worth knowing:

- **Four of the source's 21 "tables" were single-cell emphasis boxes**, not data — rendered
  as `.callout`, not `.data-table`. The other 17 are data tables.
- Five diagrams were extracted from the docx media and committed under `writing/assets/`.
  One (the Project 1/Project 2 relationship) appears **twice** in the source — as the
  opening illustration and again as Figure 5 — and is reused rather than duplicated on disk.
- References keep literal bracketed `[N]` labels (`.ref-num` spans, `list-style: none`) so
  they still match the in-text "[5]", "[7]" citations.
- The kicker line renders lowercase in markup because `.role` applies
  `text-transform: uppercase` in CSS.
- Appendix A URLs display without the `https://` scheme but link to the full address.

Fidelity was verified with a fragment-level diff against the extracted source: 440 text
fragments checked, all present.

### 3.7 New CSS classes (additive only)

Added in PR #7, refined in #8. No existing rules were changed, so the capacitor article was
unaffected:

- `.article-figure` / `.article-figure img` / `figcaption` — figures on a light panel
  (`background: #fbfaf8`), since the diagrams are light-background exports on a dark site
- `.callout` — accent-left-bordered emphasis box
- `.reference-list` + `.ref-num` — hanging-indent reference list with monospace `[N]` gutter

**Do not re-add padding or a border to `.article-figure img`.** PR #8 removed them
specifically: the stylesheet sets `* { box-sizing: border-box }` globally, so 12px padding
+ 1px border came *out of* `width: 100%`, rendering the picture 646px against 672px
paragraphs. Without them the image spans the text column exactly (delta 0 at 1100/800/500px).

### 3.8 Resume

Replaced four times this session (PRs #9–#12). The path is always `resume.pdf` at the repo
root so the footer link and any shared external links keep working.

| Upload | Result |
|---|---|
| 1st (start of session) | **Byte-identical** to what was already committed — nothing to commit |
| 2nd → PR #9 | 255,797 B, `d97702d9…` |
| 3rd → PR #10 | 255,785 B, `21964edd…` |
| 4th → PR #11 | 255,608 B, `83c878d2…` |
| 5th → PR #12 | 256,335 B, `09aca920…` ← **current** |

**All five uploads carry byte-identical visible text.** Every difference was formatting and
export metadata: an ampersand switching fonts, a background bar 6.6pt wider, "Servo
Controllers" bolded in PR #11 and un-bolded again in PR #12, plus new
`CreationDate`/`ModDate`/XMP timestamps each time. If the user expects wording changes to
appear, the edits are not making it into the exported PDF.

---

## 4. Environment gotchas (cost real time — worth knowing up front)

**Not installed in this sandbox:** `pandoc`, `pdftotext` / poppler (so `Read` on a PDF
fails with "pdftoppm is not installed"), `gh` CLI. Use the `mcp__github__*` MCP tools for
GitHub, and parse `.docx` by unzipping and reading `word/document.xml` directly.

**Network:** `krishpri-1811.github.io` is **not reachable** from the sandbox — all curl
attempts return connection failures. "Live" can only be confirmed from a green Pages deploy
plus verified content on `main`, never from fetching the page. `fonts.googleapis.com` is
also blocked, so local renders fall back to system fonts (Fraunces/Inter/JetBrains Mono are
absent) — line counts and text metrics differ slightly from production.

**Headless Chromium** at `/opt/pw-browsers/chromium-1194/chrome-linux/chrome`
(`--headless --disable-gpu --no-sandbox --hide-scrollbars --virtual-time-budget=5000`):

- `--dump-dom` works well; write measurements into an attribute and grep for it.
- **Viewport clamps to a 500px minimum.** `--window-size=390,...` still lays out at 500 CSS
  px and merely *crops* the screenshot — this produced a convincing false "content is
  clipped on mobile" reading. Phone widths below 500px could not be measured directly.
- `setInterval` does **not** advance under `--virtual-time-budget`, so polling probes return
  "pending". An iframe trick for narrow widths also failed (file:// cross-origin).
- Deep-page screenshots don't paint — anchors/scrolling produce blank images. Screenshot the
  top of the page, or build a small harness page that includes only the elements of interest.

**PDF text extraction (hand-rolled):** isolate the page content stream by picking the
decompressed stream with the highest count of `b' Tm\r\n'` — streams that merely contain
`Tj`/`TJ` include embedded font subsets and will pollute any diff (this produced a bogus
"~7,000 fewer characters" reading). Also, a regex over parenthesized operands breaks on
literal parentheses inside the text — "State of Health **(SOH)**" caused a false "content
was deleted" alarm. Capture *every* `(...)` operand in the page stream and compare the
concatenation.

**`git` on GitHub Actions listing:** `mcp__github__actions_list` with
`method: list_workflow_runs` returns ~410 KB and blows the token cap. Either add restrictive
`workflow_runs_filter` keys (`actor`, `branch`, `event`, `status` together kept it in range)
or parse the persisted result file with Python.

---

## 5. Conventions used this session

- **Never pushed directly to `main`.** All work went to `claude/github-pages-update-wqv9v8`,
  then a PR, then a merge. After each merge the branch was restarted from the new `main`
  (`git fetch origin main && git checkout -B claude/github-pages-update-wqv9v8 origin/main`)
  and pushed with `--force-with-lease`, since a merged PR can't track new work.
- Each change was verified before reporting: headless render + measurement for CSS, content
  diff for converted documents, MD5 + `git show origin/main:<path>` after every merge, and
  the Pages run's conclusion for every deploy.
- Commit messages end with the `Co-Authored-By` / `Claude-Session` trailers; PR bodies end
  with the Claude Code attribution block. No PR template exists in the repo.

---

## 6. Outstanding items (raised, not acted on — all await the user's call)

1. **The Class-E article is publicly listed while marked a draft.** Its meta table says
   "Draft for project selection — August 2026" and the lead says it was "Prepared as a
   technical concept document for discussion with Prof. Jungwon Choi". It is linked from the
   homepage writing section and live. Removing the `<li>` from
   `index.html`'s `.writing-list` would unlist it without deleting the page.
2. **"Bachelors Degree" in the hero intro** probably wants an apostrophe — "Bachelor's
   Degree". Left as the user wrote it; word-level edits are their call.
3. **Education entries in "latest updates"** still carry "focusing on Analog Integrated
   Circuits & Microwave Engineering" (Sep 2025) and "concentrating in Power Electronics &
   Electric Drives" (Jun 2025). The user's trim request covered internships only, so these
   were intentionally kept.

Also worth flagging to whoever picks this up: the hero intro drops "first-year" and the
"pursuing a focus in Analog Integrated Circuits & Microwave Engineering" clause that the
older copy had. That detail still survives in the Sep 2025 updates entry.

---

## 7. Quick reference — verifying a change

```bash
# restart branch after a merge
git fetch origin main && git checkout -B claude/github-pages-update-wqv9v8 origin/main

# render + measure locally (copy site into the scratchpad first; fonts will fall back)
CH=/opt/pw-browsers/chromium-1194/chrome-linux/chrome
$CH --headless --disable-gpu --no-sandbox --hide-scrollbars \
    --window-size=1400,900 --virtual-time-budget=5000 \
    --screenshot=out.png "file:///path/to/index.html"

# confirm what actually landed
git fetch -q origin main && git show origin/main:style.css | grep -n "avatar"
```

Deploy status: `mcp__github__actions_list` → `list_workflow_runs` with
`{"actor": "KRISHPRI-1811", "branch": "main", "event": "dynamic", "status": "completed"}`,
`per_page: 1`; match `head_sha` to the merge commit and check `conclusion`.
