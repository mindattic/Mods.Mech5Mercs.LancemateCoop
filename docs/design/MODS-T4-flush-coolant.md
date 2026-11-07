# MODS-T4 — Flush Coolant ability

## Goal
An original "Flush Coolant" mech ability, in the spirit of MechWarrior 4's coolant flush (the
*mechanic* — instant heat dump from a limited per-mission resource — not any of MW4's code,
assets, or text), bound to `/` by default, with a new "MindAttic" section in the Controls menu to
hold it and any future MindAttic-added binds.

## Mechanic
- Every mech gets a coolant pool for the mission: a fixed number of charges (suggest 3, tune by
  playtesting) that does **not** regenerate mid-mission and refills at mission start/mechbay.
- Pressing the bound key with at least one charge available:
  - Instantly removes a fixed amount of heat (suggest 30-40% of max heat capacity, tune by
    playtesting — should meaningfully reset a near-shutdown mech, not fully zero it every time).
  - Consumes one charge.
  - Has a short cooldown (suggest 2-3s) so it can't be spammed as crowd control against the shutdown
    threshold itself.
- At zero charges, the key does nothing (play a short "empty" cue so the player knows why).
- Multiplayer: each mech's coolant pool is personal (per-player), not a shared/lance resource.

## UI
- A new "MindAttic" category in the in-game Controls/Keybinds menu, separate from the game's own
  categories, so every MindAttic-added bind lands in one predictable place instead of being
  scattered through the base menu.
- "Flush Coolant" is the first entry in that category, default bind `/`.
- HUD: a small charge counter (e.g. "x3") near the heat gauge, and a brief visual/audio cue on use
  and on cooldown.

## Implementation notes (Mod Editor / Blueprint)
- Needs the editor's heat-system hook (whatever Blueprint/function currently adjusts mech heat) and
  per-mech ability-slot system (if mechs already have an ability-slot framework, e.g. for existing
  consumables/CBill-bought equipment, reuse that rather than inventing a parallel one).
- Custom keybind registration: confirm whether the editor/engine exposes a generic "register a new
  keybind action, with a category label" node, or whether a custom Controls-menu section has to be
  built from scratch as its own UI widget. The "Mod Options" community mod is the one confirmed
  precedent for mods adding to the Controls menu — worth checking its public behavior description
  (not its code) for what's achievable without digging into another mod's implementation.

## Acceptance criteria
- Every mech has a working "Flush Coolant" ability with a visible, limited charge count.
- The bind appears under a new "MindAttic" Controls section, defaulted to `/`, and is rebindable
  like any other game action.
- No MW4 code, assets, text, or audio is copied — this is an original implementation of a
  commonly-understood mechanic.
