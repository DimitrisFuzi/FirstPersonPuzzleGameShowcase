# Portfolio Packaging Roadmap — FirstPersonPuzzleGameShowcase

> Started: 2026-03-25  
> Goal: portfolio-ready in 3–6 months (target: Junior Gameplay / Systems, Unreal)  
> Single source of truth for all remaining packaging work.

---

## Status legend

- [ ] Not started
- [~] In progress
- [x] Done

---

## Phase 0 — Foundation (est. 2–3 hours total)

### 0.1 — Docs skeleton
**Exit criteria:** `Docs/ROADMAP.md`, `Docs/TECHNICAL.md`, and `Docs/Images/.gitkeep` exist in the repo and render correctly on GitHub.  
**Deliverables:** those three files.  
**Reused in:** all later steps reference these files.

- [ ] `Docs/ROADMAP.md` committed ← *this file*
- [ ] `Docs/TECHNICAL.md` skeleton committed
- [ ] `Docs/Images/.gitkeep` committed
- [ ] README links to `Docs/TECHNICAL.md` and `Docs/ROADMAP.md`

---

## Phase 1 — QA gate (est. 3–5 hours)

Do this before recording anything. A broken or confusing build invalidates all later content.

### 1.1 — Fresh-install test
**Exit criteria:** Unzip → run on a clean Windows user profile (or fresh machine) → game reaches main menu with no errors. All controls work as expected.  
**Deliverables:** nothing committed, but gate must pass before 1.2.

- [ ] Tested: unzip → run without Visual C++ / DirectX installer surprises
- [ ] Tested: main menu loads, gameplay level loads, end screen triggers
- [ ] Tested: interact prompt visible within 5 seconds of level start
- [ ] Tested: clock code input (0912) accepts correct entry and rejects wrong entry cleanly
- [ ] Tested: inspect mode opens, rotates/zooms, exits cleanly
- [ ] Tested: inventory opens and shows picked-up key

### 1.2 — Build notes (draft)
**Exit criteria:** 8–12 bullet "Release Notes" written, covering tested platform, known issues, and limitations.  
**Deliverables:** bullets ready to paste into README and itch.io page.  
**Reused in:** Phase 3 (README), Phase 5 (itch.io).

- [ ] Note: tested Windows version + GPU/CPU
- [ ] Note: estimated playtime range (first play, optimal)
- [ ] Note: any missing redistributables or launcher steps
- [ ] Note: controls list finalised (actual keybinds confirmed)
- [ ] Note: any known edge cases / soft-locks

---

## Phase 2 — Video capture (est. 3–5 hours)

### 2.1 — Shot list
**Exit criteria:** written list of 10–15 clips (name, what to show, target duration each).  
**Deliverables:** shot list kept as a personal note (no repo file needed).  
**Reused in:** Phase 2.2.

Minimum clips:
- [ ] Interaction framework: door + lightswitch + clock prompt (show `E` prompt appearing)
- [ ] Clock code: input 0912 → success; input wrong code → failure feedback
- [ ] Pickup: pick up key → inventory grid updates
- [ ] Inspect: open inspect mode, rotate mesh, exit
- [ ] Hallway trigger sequence (lights turning on)
- [ ] End screen fade

### 2.2 — Record raw footage
**Exit criteria:** all clips from the shot list recorded at 1080p/60 fps; footage reviewable.  
**Tools:** OBS Studio (recommended) or NVIDIA ShadowPlay.  
**Settings:** 1080p, 60 fps, NVENC if available, 20–40 Mbps.  
**Deliverables:** raw clip files (stored locally, not in repo).

- [ ] OBS / ShadowPlay configured
- [ ] All clips recorded
- [ ] Footage reviewed; re-record any unclear clips

### 2.3 — Edit highlight reel (60–90 seconds)
**Exit criteria:** exported `Highlight.mp4`, 60–90 seconds, with title card, system captions, and end card.  
**Tools:** DaVinci Resolve (free).  
**Deliverables:** `Highlight.mp4` (hosted externally; link added to README + itch).  
**Reused in:** Phase 5 (itch page embed), Phase 6 (portfolio site).

Structure:
- [ ] 0–3 s: title card ("FirstPersonPuzzleGameShowcase — Unreal Engine — Blueprint gameplay systems")
- [ ] 3–20 s: interaction framework clips with captions
- [ ] 20–35 s: pickup + inventory clip with caption
- [ ] 35–55 s: inspect mode clip with caption
- [ ] 55–75 s: puzzle loop + end screen
- [ ] 75–90 s: end card ("itch.io | GitHub")
- [ ] Exported: `Highlight.mp4`

### 2.4 — (Optional) Systems walkthrough (3–5 minutes)
**Exit criteria:** exported `SystemsWalkthrough.mp4` covering each system with narration or captions.  
**Reused in:** Phase 5 (itch page) and Phase 6 (portfolio).

- [ ] Walkthrough recorded and edited
- [ ] Exported: `SystemsWalkthrough.mp4`

---

## Phase 3 — README finalisation (est. 1–2 hours)

**Exit criteria:** README has no placeholder text (`[…here]`, `[key here]`, etc.), all links are real, and the page is self-contained for a recruiter skimming it in 30–60 seconds.  
**Deliverables:** updated `README.md`.  
**Reused in:** itch page description (partial copy-paste).

- [ ] Fill in itch.io link
- [ ] Fill in tested platform details (Windows version, hardware)
- [ ] Fill in inspect exit keybind (`[key here]` placeholder)
- [ ] Replace "Known issues" placeholders with bullets from Phase 1.2
- [ ] Add link to `Highlight.mp4` (video section or badge)
- [ ] Verify links to `Docs/TECHNICAL.md` and `Docs/ROADMAP.md` work on GitHub

---

## Phase 4 — Technical doc completion (est. 4–6 hours)

**Exit criteria:** `Docs/TECHNICAL.md` has no `_placeholder_` text; all diagrams are committed under `Docs/Images/`; the document is accurate to the actual implementation.  
**Deliverables:** completed `Docs/TECHNICAL.md`, diagram PNGs in `Docs/Images/`.  
**Reused in:** portfolio site "Technical depth" section.

### 4.1 — Diagrams
- [ ] `Docs/Images/interaction-flow.png` — Player → Trace → BPI → MasterItem → enum routing
- [ ] `Docs/Images/inventory-flow.png` — AddItem → Component → Widget loop → Grid slots
- [ ] `Docs/Images/inspect-flow.png` — MasterItem → BP_InspectItem → SceneCapture → WBP_Inspect
- [ ] Diagrams made in draw.io and exported as PNG

### 4.2 — Blueprint screenshots
- [ ] `Docs/Images/blueprint-masteritem.png` — cropped EInteractionType switch node
- [ ] `Docs/Images/blueprint-additem.png` — TryAddToInventory → AC_InventoryComponent.AddItem
- [ ] `Docs/Images/inspect-widget.png` — in-game or editor screenshot of inspect UI
- [ ] `Docs/Images/inventory-widget.png` — in-game or editor screenshot of inventory UI

### 4.3 — Fill placeholders in TECHNICAL.md
- [ ] Section 5.1: document remaining input actions beyond `IA_Interact` / `IA_GameMenu`
- [ ] Section 5.3: add any widgets not already listed
- [ ] Section 6: fill in known issues bullets
- [ ] Link all diagram images from the relevant sections

---

## Phase 5 — itch.io publishing (est. 1–2 hours)

**Exit criteria:** itch.io page is live, download works, description and cover image are present.  
**Deliverables:** live itch.io URL (paste into README and portfolio).  
**Reused in:** README, portfolio site.

- [ ] Page description drafted (reuse README intro + features list)
- [ ] Cover image / screenshots uploaded (minimum 1 gameplay screenshot)
- [ ] `Highlight.mp4` embedded or linked
- [ ] Download button active; build tested via itch launcher
- [ ] Controls + known issues section visible on itch page
- [ ] URL finalised and added to README

---

## Phase 6 — Portfolio site integration (est. 2–4 hours)

**Exit criteria:** project has its own page/section on portfolio site with: title, one-liner, highlight video embed, itch link, GitHub link, and a "systems" bullet list.  
**Deliverables:** updated portfolio site.  
**Reused in:** job applications.

- [ ] Project page created on portfolio site
- [ ] Highlight video embedded
- [ ] itch.io and GitHub repo links present
- [ ] Systems summary bullet list (4 systems, consistent with README)
- [ ] "What I'd improve next" blurb present (shows engineering judgment)

---

## Output map (what gets reused where)

| Output | Created in | Reused in |
|---|---|---|
| Build notes (bullets) | Phase 1.2 | Phase 3 (README), Phase 5 (itch) |
| Shot list | Phase 2.1 | Phase 2.2 |
| `Highlight.mp4` | Phase 2.3 | Phase 3, 5, 6 |
| README intro + features | Phase 3 | Phase 5 (itch description), Phase 6 |
| `Docs/TECHNICAL.md` diagrams | Phase 4 | Phase 6 (portfolio "depth" section) |
| itch.io URL | Phase 5 | README, Phase 6 |

---

## Time budget summary

| Phase | Estimate |
|---|---|
| Phase 0 — Foundation | 2–3 h |
| Phase 1 — QA gate | 3–5 h |
| Phase 2 — Video | 3–5 h |
| Phase 3 — README | 1–2 h |
| Phase 4 — Technical doc | 4–6 h |
| Phase 5 — itch.io | 1–2 h |
| Phase 6 — Portfolio site | 2–4 h |
| **Total** | **16–27 h** |

Spread over 3–6 months alongside job applications, targeting ~2–4 hours per week.
