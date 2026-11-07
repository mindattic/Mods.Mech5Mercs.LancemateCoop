# MODS-T11 — Lancemate auto-eject

## Goal
When a lancemate's cockpit (head) is destroyed, the pilot ejects and survives instead of being
killed outright. Losing a long-trained pilot to one unlucky headshot is the worst outcome in the
campaign, and this makes it a setback rather than a permanent loss.

## Current behavior (assumed, needs confirming in the editor)
Cockpit/head destruction kills the pilot (or rolls for death) and destroys the mech. The pilot is
removed from the roster.

## Proposed design
- When a lancemate's cockpit is destroyed, the pilot ejects instead of dying:
  - The mech is out of the fight for the rest of the mission, the same as it is today.
  - The pilot survives with a serious injury, so they're out of action for a recovery period.
    Use the game's existing injury system and its longest stock recovery time, rather than adding
    a new one.
  - The pilot keeps all their skills and experience.
- Visual: an ejection seat/canopy effect plays if the game already has one (it does for the
  player's own ejection), so it's clear the pilot got out.
- After the mission, the mech is treated however a cockpit-destroyed mech is treated today
  (salvageable wreck, repair bill, or written off). This mod doesn't change that part.
- Scope:
  - Applies to AI lancemates on the player's side.
  - Human coop players' own mechs follow the same rule, if the base game doesn't already let them
    eject or respawn.
  - Enemy pilots are unaffected.
- Optional setting: "Lancemate auto-eject" in the MindAttic settings section, on by default. Turn it
  off for the stock permadeath risk.

## Edge cases
- Other ways of dying stay as they are: a full mech destruction (core/engine blown) and death from
  repeated injuries are not covered by this ticket. If you want those covered too, they need a
  follow-up decision.
- If the pilot is already injured when the cockpit goes, eject them at the longest recovery time,
  not a death roll.

## Implementation notes (Mod Editor / Blueprint)
- Find where head/cockpit destruction is handled for a lancemate (a damage-component event or a
  pilot-death Blueprint) and route it to the eject path instead.
- The player's own ejection logic is the best model to reuse for the effect and the "mech out,
  pilot alive" state.

## Acceptance criteria
- A lancemate whose cockpit is destroyed ejects, survives the mission, and comes back to the roster
  injured, with all their skills and experience.
- Enemy pilots and other causes of death behave as in the stock game.
- The setting toggle brings back stock behavior.
