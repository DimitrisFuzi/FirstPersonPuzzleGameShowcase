# FirstPersonPuzzleGameShowcase

A short **Unreal Engine Blueprint gameplay systems showcase** focused on interaction, inspection, pickup/inventory, and UI flow.

**Target role:** Junior Gameplay / Systems (Unreal)  
**Playtime:** ~1 minute (if solved instantly)

## What this project demonstrates
### Interaction framework (interface + base item class)
- Player interaction uses a **line trace** from `BP_FirstPersonCharacter` to find an interactable actor.
- Interactables are children of `BP_MasterItem` and implement `BPI_Interactable`.
- `BP_MasterItem` routes interaction through `EInteractionType`:
  - **Use** (per-item override: door, lightswitch, clock code, etc.)
  - **Inspect** (shared implementation)
  - **Pickup** (shared implementation)

### Inventory (component + struct-driven UI)
- Inventory logic is contained in `AC_InventoryComponent`:
  - `AddItem`, `RemoveItem`, `HasItem`
- Items are described by `STR_ItemData`:
  - `ItemName` (String), `ItemDescription` (Text), `ItemIcon` (Texture2D), `ItemMesh` (StaticMesh), `ItemType` (Byte)
- Inventory UI dynamically populates a grid by iterating the inventory data (no hard-coded slots).

### Inspect system (Resident Evil style)
- Inspect is handled through a dedicated blueprint (`BP_InspectItem`) that:
  - Spawns the inspect UI (`WBP_Inspect`)
  - Uses a `SceneCaptureComponent2D` setup to render the inspected mesh in a controlled “preview space”
  - Supports rotate/zoom and per-item default rotation/scale

### UI + flow
- Main menu + in-game UI widgets (inventory, inspect, fade screen, etc.)
- Enhanced Input actions (e.g. `IA_Interact`, `IA_GameMenu`)
- Intro sequence (Level Sequence) and end-screen fade

## Puzzle summary (context)
The core puzzle is to find a wall shadow that suggests **21:12**, then input **0912** into the clock to open a box containing a key. The player uses the key to progress into a short hallway sequence and an end-screen.

## Controls
- **WASD**: Move  
- **Mouse**: Look  
- **[E]**: Interact  
- **[Esc]**: Game menu  
- **Inspect mode**: rotate/zoom + exit via **[key here]** (document your actual bindings)

## Build / download
- Download: **[itch.io link here]**
- Platform: Windows
- Tested on: **[your Windows version + GPU/CPU if you want]**

## Known issues / limitations
- **[Add 5–10 bullets here]** (example: “No save system”, “No subtitles”, “Some UI prompts are placeholder”, etc.)

## What I would improve next
- Replace `EInteractionType` enum routing with a more **data-driven action system** (e.g., per-item “action strategy” objects or gameplay tags) to scale without editing central switch logic.
- Add clearer **gameplay state management** (Menu / Inspect / Gameplay) via a small state machine to prevent input conflicts.
- Add a small C++ layer in a future project (e.g., interaction component + inventory core) for AA-style workflows.