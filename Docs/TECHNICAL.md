# Technical Documentation — FirstPersonPuzzleGameShowcase

> Unreal Engine · Blueprint-only · Windows · ~1 minute playtime  
> Last updated: 2026-03-25

---

## 1. Project Overview

Short first-person puzzle built to demonstrate Unreal Blueprint gameplay systems.

### High-level runtime flow

```
Player (BP_FirstPersonCharacter)
  └─ Line Trace (camera forward)
       └─ Hit Actor implements BPI_Interactable?
            └─ Call Event Interact (BPI_Interactable)
                 └─ BP_MasterItem checks EInteractionType
                      ├─ UseAction      → per-item override (BP_Door, BP_LightSwitch, BP_Clock…)
                      ├─ InspectAction  → BP_InspectItem (shared)
                      └─ PickupAction   → AC_InventoryComponent.AddItem (shared)
```

---

## 2. Interaction System

### 2.1 Interface: `BPI_Interactable`

Defines the `Event Interact` contract. All interactable actors implement this interface.

Also used: `BPI_Intro` (intro sequence binding).

### 2.2 Base class: `BP_MasterItem`

| Responsibility | Detail |
|---|---|
| Implements | `BPI_Interactable` |
| Exposes | `EInteractionType` (set per instance in editor) |
| Routes | Switch on `EInteractionType` → UseAction / InspectAction / PickupAction |
| Shared logic | `InspectAction` and `PickupAction` fully implemented here |
| Extensible | `UseAction` is empty by default — overridden per item |

### 2.3 Enum: `EInteractionType`

| Value | Handler | Example actors |
|---|---|---|
| `UseAction` | Per-item override | `BP_Door`, `BP_LightSwitch`, `BP_Clock` |
| `InspectAction` | `BP_MasterItem` shared | Inspectable props |
| `PickupAction` | `BP_MasterItem` → `AC_InventoryComponent` | Key, collectibles |

**Tradeoff note:** Enum routing is simple and predictable for a fixed action set. For larger projects this would migrate to a data-driven action strategy (per-item action object or Gameplay Tags) to avoid editing central switch logic when new types are added.

### 2.4 Adding a new interactable in 3 steps

1. Create a child Blueprint of `BP_MasterItem`.
2. Set `EInteractionType` in the instance defaults.
3. If `UseAction`: override the `UseAction` event and add custom logic.

---

## 3. Pickup / Inventory System

### 3.1 Struct: `STR_ItemData`

| Field | Type | Purpose |
|---|---|---|
| `ItemName` | String | Display name |
| `ItemDescription` | Text | Description (localisation-ready) |
| `ItemIcon` | Texture2D | Inventory grid icon |
| `ItemMesh` | StaticMesh | Mesh used in inspect preview |
| `ItemType` | Byte | Category/type identifier |

### 3.2 Component: `AC_InventoryComponent`

Attached to `BP_FirstPersonCharacter`. Holds the authoritative list of `STR_ItemData` structs.

| Function | Behaviour |
|---|---|
| `AddItem(STR_ItemData)` | Appends item; UI reflects automatically |
| `RemoveItem(…)` | Removes item from array |
| `HasItem(ItemType)` | Returns bool; used by `BP_Door` to check for key |

### 3.3 UI update mechanism

Inventory widget (`WBP_Inventory`) contains a grid panel. On open, it loops the current struct array in `AC_InventoryComponent` and dynamically creates one item-slot sub-widget per entry. No hard-coded slots — adding/removing items and reopening the inventory reflects the current state automatically.

> **Diagram placeholder:** `Docs/Images/inventory-flow.png` — add a draw.io diagram showing AddItem → Component Array → Widget loop → Grid slots.

---

## 4. Inspect System

### 4.1 Flow

```
BP_MasterItem (InspectAction)
  └─ Calls BP_InspectItem (passes: mesh, per-item rotation, scale defaults)
       ├─ Spawns item mesh at dedicated "preview space" in level
       ├─ SceneCaptureComponent2D captures that space → render target
       └─ Spawns WBP_Inspect
            └─ Displays render target in Image widget (what the camera sees)
            └─ Rotate / zoom input → modifies mesh transform in preview space
```

### 4.2 Key parameters per inspectable item

Stored per `BP_MasterItem` instance:
- Default rotation (Rotator)
- Default scale (Vector)

These are passed to `BP_InspectItem` on inspect activation.

### 4.3 Exit conditions

- Player presses the bound exit/inspect key.
- `WBP_Inspect` closes, mesh is removed from preview space, input mode returns to gameplay.

> **Screenshot placeholder:** `Docs/Images/inspect-widget.png` — crop of `WBP_Inspect` in editor or in-game.

---

## 5. UI / Game Flow

### 5.1 Input Actions (Enhanced Input)

| Action | Context |
|---|---|
| `IA_Interact` | Triggers line trace → interact |
| `IA_GameMenu` | Opens/closes in-game menu |
| _(others)_ | _Document remaining IA_ |

### 5.2 Game Mode: `GM_MainMenu`

Handles main menu state. Manages widget visibility and level transition to gameplay level.

### 5.3 Widgets summary

| Widget | Purpose |
|---|---|
| `WBP_MainMenu` | Title screen |
| `WBP_Inventory` | In-game inventory grid |
| `WBP_Inspect` | Inspect mode (render target display) |
| `WBP_FadeScreen` | Fade-in/out transitions |
| _(others)_ | _Add any remaining widgets_ |

### 5.4 Intro sequence

Level Sequence plays on level load (implements `BPI_Intro`). Handles camera pan before giving player control.

### 5.5 End screen

Triggered when player enters the final room. Door closes and locks, `WBP_FadeScreen` fades to "Thank You for Playing".

---

## 6. Known Issues & Planned Improvements

### Known issues

- [ ] _Add tested platform details (Windows version, GPU)_
- [ ] _Document any softlock edge cases_
- [ ] _Note any UI prompts that are placeholder text_

### Planned improvements

- Replace `EInteractionType` switch with a data-driven action strategy to scale without touching central routing.
- Introduce a formal gameplay state machine (`InMenu` / `InGameplay` / `InInspect` / `InPuzzleUI`) to enforce input exclusivity cleanly.
- Add in-editor debug overlay (on-screen key/current state) for faster iteration.
- Expose item descriptions to a Data Table for editor-friendly content management.
- Investigate a small C++ base layer for the interaction interface and inventory component (AA-studio workflow familiarity).

---

## 7. Diagrams & Screenshots

Place images in `Docs/Images/` and link from relevant sections above.

| File | Contents |
|---|---|
| `interaction-flow.png` | Player → Trace → BPI → MasterItem → enum routing |
| `inventory-flow.png` | AddItem → Component → Widget loop → Grid |
| `inspect-flow.png` | MasterItem → BP_InspectItem → SceneCapture → WBP_Inspect |
| `inspect-widget.png` | In-game or editor screenshot of inspect UI |
| `inventory-widget.png` | In-game or editor screenshot of inventory UI |
| `blueprint-masteritem.png` | Cropped graph: EInteractionType switch node |
| `blueprint-additem.png` | Cropped graph: TryAddToInventory → AC_InventoryComponent.AddItem |
