# Unreal Portfolio Slice Roadmap (2026) — MyFPProjectX

**Target role:** Junior Gameplay / Systems (AA / Indie / mid-tier)  
**Goal:** Apply in **3–6 months** with a playable, reviewable slice  
**Execution Focus:** Core gameplay loop first, polish/improvements after core complete (“ship, then refactor”).

---

## Progress Tracker

| Milestone    | Status      | ETA/Notes         |
|--------------|------------|-------------------|
| A – Prompt + Targeting      | **✅ Complete** | Prompt reliably shows/hides for each interactable. |
| B – Inspection        | Not Started | Elephant + note inspect UX must be robust.     |
| C – Puzzle State + Door Unlock | Not Started | Read note, toggle desk lamp, wall light puzzle, unlock door. |
| D – Packaging + Video + Docs   | Not Started | Build, playtest, video demo, README.           |
| E – C++ Upgrade (optional)     | Backlog     | Add if time after D.                           |

---

## Current Slice State *(updated: 2026-02-28)*

- **Core map:** House with interactables: bed/desk assets, desk lamp, elephant statue, door, wall light switch, note.
- **Player (BP_FirstPersonCharacter):** LineTrace, Enhanced Input, stable HUD prompt.
- **Prompt system:** (`W_HUD`, `WBP_InteractPrompt`) works reliably for all items.
- **Interaction architecture:** (`BPI_Interactable` interface, `BP_MasterItem` with overridable `InteractionText`)
- **Inspection:** Initial Blueprint, not robust, next up.
- **Puzzle logic:** Not implemented yet (todo: clean puzzle flow controller).
- **Packaging, docs, video:** Pending after core complete.

---

## Next Up — Core Milestones ("must-have before shipping")

### **B – Inspection (Simplified, Reliable)**
1. Decide inspection scope: which items, behavior (hold key, click, etc.).
2. Implement:
    - Camera zoom/focus.
    - Lock movement; modal UI overlay.
    - Smooth exit (ESC/E restores movement).
3. QA: No lockups, inspect always works.

### **C – Puzzle & Door Unlock Loop**
1. Add `BP_HouseFlowController` (not in Level Blueprint).
2. Rules:
    - Door unlocks if: *Note read + Desk lamp ON + Ceiling light OFF*
3. Wire events:
    - Interact/inspect note → set `bReadNote`.
    - Desk lamp/wall switch → update their booleans.
    - Door opens if unlocked; otherwise shows “Locked”.
4. QA: Loop always completable, no bugs.

### **D – Packaging, Docs, Video**
1. Set default map; build for Windows.
2. Record 2–3 min demo video (prompt, inspect, puzzle, door).
3. Update README:
   - Feature summary
   - Controls
   - Known issues
   - Improvement plan

---

## Optional/Improvements (Do *only after* core is playable and reviewable)

- **C++ component for interaction targeting.**
- Refactor references (avoid BeginPlay casts in interactables; use player/controller references cleanly).
- Add designer-friendly edits for notes/messages.
- More advanced camera/system polish for inspection.
- Lighting, VFX, sound polish
- Multiplayer/network handling (only if applying to relevant roles).

---

## Execution Philosophy

**Core first.**  
- Complete gameplay loop -> Slice works end-to-end.
- Only then, iterate QA/polish/features for reviewer impact.

**No “half-feature creep.”**  
- Improvements only once the main loop is stable.

---

## Next Steps

**B – Inspection:**
- [ ] Choose interaction inputs (key/mouse, modal flow)
- [ ] Implement UI/camera logic for inspection
- [ ] QA for reliability

**C – Puzzle & Door Unlock:**
- [ ] Build puzzle controller Blueprint
- [ ] Connect interactables to state
- [ ] QA: Full puzzle and unlock loop works

**D – Packaging & Docs:**
- [ ] Build playable Windows package
- [ ] Record video demo
- [ ] Write README/portfolio docs

---

**Whenever you complete a step or want to plan a sub-feature, update this tracker.  
This approach signals studio-readiness: disciplined shipping, clear QA strategy, no generic filler, visible progress.**  