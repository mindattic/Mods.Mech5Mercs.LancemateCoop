# MODS-T10 — Flat UI rendering

## Goal
Turn off the slanted/perspective-angled rendering of UI panels so text and icons draw flat, facing
the screen, and are easier to read.

## Check first
Before building anything, check whether the stock game already has a setting for this, under
Settings > Display/Gameplay/HUD (for example a HUD tilt, curvature or "3D HUD" toggle), or in the
config `.ini` files under `%LOCALAPPDATA%\MW5Mercs\Saved\Config\WindowsNoEditor\`. If a stock
setting covers it, this ticket becomes "document the setting" instead of a mod change.

## Proposed design
- Every slanted UI panel draws flat by default with the mod installed:
  - The in-mission HUD, if its side panels are angled.
  - Any menu screens that are angled (mechlab, star map side panels, etc.).
- Add a toggle, "Slanted UI", in a MindAttic settings section, defaulting to Off, for players who
  prefer the stock look.
- Layout must still fit: flattening a panel can make it wider on screen, so check it doesn't overlap
  neighbouring panels at 16:9, 21:9 and lower resolutions.

## Implementation notes (Mod Editor / Blueprint)
- In Unreal, a slanted UI is usually done one of two ways:
  - A UMG widget with a render transform (shear or rotation) or a 3D projection. Fix: zero the
    transform on the affected widgets.
  - A widget drawn onto a 3D widget component/plane placed at an angle in the scene (common for
    cockpit holographic HUDs). Fix: rotate the component to face the camera, or switch it to
    screen-space rendering.
- Identify which approach each slanted panel uses before changing it. Mods that override base-game
  widget assets can conflict with other UI mods that override the same assets, so note any known
  conflicts on the mod page.

## Acceptance criteria
- With the mod installed, the HUD and menus that used to be slanted draw flat and readable.
- Nothing overlaps or clips at common resolutions and aspect ratios.
- The "Slanted UI" toggle brings the stock look back.
