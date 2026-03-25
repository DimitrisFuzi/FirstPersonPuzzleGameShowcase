# Technical Notes — FirstPersonPuzzleGameShowcase (Unreal Blueprints)

**Purpose:** Brief technical overview of the gameplay systems implemented in Blueprints (interaction, inventory/pickup, inspect, and UI flow).  
**Controls:** Listed in `README.md` and displayed in-game (startup widget + inspect overlay).

---

## 0) System map (high level)

**Interaction**
- `BP_FirstPersonCharacter` → Line Trace
- If target implements `BPI_Interactable` → call `Interact(Instigator)`
- `BP_MasterItem` receives `Interact` → routes via `EInteractionType` → **Use / Inspect / Pickup**

**Inventory**
- Items described by `STR_ItemData`
- Player owns `AC_InventoryComponent` (`AddItem`, `RemoveItem`, `HasItem`)
- Inventory UI builds a grid by iterating the component’s current items

---

## 1) Interaction System

### 1.1 Player-side detection (`BP_FirstPersonCharacter`)
**Goal:** Detect the interactable under the crosshair and call a generic interaction entry point.

- Uses a **line trace** from camera/crosshair to find a hit actor.
- If the hit actor implements `BPI_Interactable`, the character calls `Interact` and passes itself as instigator.

**Figure 1 — Player trace + interface call**
![](Images/interaction_character_linetrace.png)

### 1.2 Item-side dispatch (`BP_MasterItem` + `BPI_Interactable`)
**Goal:** Centralize shared interaction logic while still allowing per-item unique behavior.

- All interactables inherit from `BP_MasterItem` and implement `BPI_Interactable`.
- Each item instance sets `EInteractionType` in the editor.
- `BP_MasterItem` routes `Interact` into:
  - **Pickup** (shared implementation)
  - **Inspect** (shared implementation)
  - **Use** (intended to be overridden per item: door, lightswitch, clock/code UI, etc.)

**Example:** the clock uses **Use** to open `WBP_ClockInput` and validate the entered code.

**Figure 2 — Dispatch by `EInteractionType`**
![](Images/interaction_masteritem_switch.png)

**Figure 3 — Per-instance configuration in editor**
![](Images/interaction_instance_details.png)

#### Extending the system (adding a new interactable)
1. Create a child Blueprint of `BP_MasterItem`
2. Set `InteractionType` and prompt text in the Details panel
3. If `Use`: override `OnUseAction` in the child Blueprint to implement unique behavior

#### Design tradeoff (why enum routing)
- ✅ Clear and fast for a small vertical slice
- ❌ Less scalable if many new interaction types are added (requires editing enum + central switch)
- **Next step (if expanding):** migrate toward a more data-driven “action strategy” approach

---

## 2) Pickup + Inventory

### 2.1 Data model (`STR_ItemData`)
Items are represented by a struct containing:
- `ItemName` (String)
- `ItemDescription` (Text)
- `ItemIcon` (Texture2D)
- `ItemMesh` (StaticMesh)
- `ItemType` (Byte)

### 2.2 Inventory logic (`AC_InventoryComponent`)
The player owns an `AC_InventoryComponent` with:
- `AddItem`
- `RemoveItem`
- `HasItem` (used for gating interactions like door unlocking)

### 2.3 UI update approach (dynamic grid)
**Goal:** Render the inventory UI based on current component state (not hard-coded slots).

Approach:
- The inventory widget clears the Wrap Box children.
- It reads the `Items` array from `AC_InventoryComponent`.
- For each item, it creates a slot widget (e.g. `WBP_ItemSlot`) and passes `ItemData`.
- Each created slot is added to the Wrap Box.

This keeps the UI consistent with gameplay state, and the layout automatically scales with item count.

**When it updates:** `WBP_InventoryHUD` calls `UpdateItemGrid` on `Event Construct`, rebuilding the grid when the inventory UI is created/opened.

**Figure 4 — Inventory grid rebuild (Wrap Box + create slot per item)**
![](Images/inventory_ui_grid_rebuild.png)

---

## 3) Inspect System

### 3.1 Overview (`BP_InspectItem` + `WBP_Inspect`)
**Goal:** Allow items to be inspected in a controlled preview space, similar to Resident Evil inspection.

- Inspect is initiated from `BP_MasterItem`, but handled by a dedicated Blueprint: `BP_InspectItem`.
- `BP_InspectItem` spawns the inspect UI (`WBP_Inspect`).

### 3.2 Rendering approach (SceneCapture preview)
- Uses a `SceneCaptureComponent2D` setup so the inspected mesh is rendered in a controlled area (consistent lighting/background).
- The widget displays the capture output.

**Figure 5 — Inspect lifecycle (start/end)**
![](Images/inspect_start_and_end.png)

**Figure 6 — Inspect controls logic (rotate/zoom/reset)**
![](Images/inspect_controls_rotation_zoom.png)

---

## 4) UI / Flow / Input (high level)

- Enhanced Input actions (examples): `IA_Interact`, `IA_GameMenu`
- HUD ownership: after the intro sequence finishes, the player creates `WBP_HUD`, stores a reference, and adds it to the viewport.
- Interaction feedback: `WBP_InteractPrompt` displays context prompts based on the current interact target.
  - The interact target is refreshed using a short repeating timer (0.1s) to avoid doing this work every frame on Tick; if scaling up, this would move to “update-on-change” (only refresh prompt when the hit actor changes).
- Inventory UI: `WBP_InventoryHUD` hosts the item grid (`WBP_ItemGrid`) and updates it from `AC_InventoryComponent`.
- “Use” override example: the clock interaction opens a dedicated UI (`WBP_ClockInput`) to enter the code.
- Game flow: fade/end widgets support clean transitions between gameplay beats.

---

## 5) Non-goals / limitations (intentional for this showcase)
- No save/checkpoint system (short vertical slice)
- No settings menu (graphics/audio)
- Keyboard/mouse only (no controller support)
- Not broadly hardware tested (tested on a single Windows 10 Pro machine)

---

## 6) Next improvements (if expanding this project)
- Replace enum routing with a more scalable, **data-driven action strategy** approach.
- Formalize gameplay “modes” (Gameplay / Inspect / Menu) so input/UI rules remain maintainable as scope grows.
- In a follow-up project, implement core systems (interaction/inventory) in **C++** for AA-style workflows.