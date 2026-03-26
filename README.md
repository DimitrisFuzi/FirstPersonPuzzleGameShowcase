# FirstPersonPuzzleGameShowcase

A short **Unreal Engine Blueprint gameplay systems showcase** focused on interaction, inspection, pickup/inventory, and UI flow.

**Focus:** Gameplay systems implementation (Blueprints)  
**Playtime:** ~1-2 minutes (if solved quickly)

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
  - Supports rotate/zoom/reset and per-item default rotation/scale

### UI + flow
- Main menu + in-game UI widgets (inventory, inspect, fade screen, etc.)
- Enhanced Input actions (e.g. `IA_Interact`, `IA_GameMenu`)
- Intro sequence (Level Sequence) and end-screen fade

## Puzzle summary
The core puzzle is to find a wall shadow that suggests **21:12**, then input **0912** into the clock to open a box containing a key. The player uses the key to progress into a short hallway sequence and an end-screen.

## Controls

### Gameplay
- **WASD**: Move  
- **Mouse**: Look  
- **Space**: Jump  
- **E**: Interact  
- **I**: Open inventory  
- **Esc**: Open game menu  
- Menus are controlled with **mouse click**.

### Inspect mode
- **Rotate**: WASD *or* Left Mouse Button drag  
- **Zoom in / Zoom out**: Mouse Wheel  
- **Reset position**: Right Mouse Button  

## Build / download
- Download: https://github.com/DimitrisFuzi/FirstPersonPuzzleGameShowcase/releases/download/v1.1.1/FirstPersonPuzzleGameShowcase_Windows_v1.1.0.zip
- Platform: Windows
- Tested on: Windows 10 Pro

## Known issues / limitations
- Not optimized/tested on many hardware configurations (tested on a single Windows 10 Pro machine).
- No save/checkpoint system (short vertical slice).
- No controller support (keyboard/mouse only).
- No settings menu (graphics/audio).

## What I would improve next
- Replace `EInteractionType` enum routing with a more **data-driven action system** (e.g., per-item “action strategy” objects or gameplay tags) to scale without editing central switch logic.
- Formalize gameplay “modes” (Gameplay / Inspect / Menu) so input/UI rules stay easy to maintain as scope grows.
- Add a small C++ layer in a future project (e.g., interaction component + inventory core).

## Links
- Technical notes: [Docs/TECHNICAL.md](Docs/TECHNICAL.md)
