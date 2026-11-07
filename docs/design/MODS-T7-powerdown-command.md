# MODS-T7 — Power Down/Up command

## Goal
A new lance command, "Power Down" / "Power Up": powered-down mechs go sensor-invisible (engine and
sensors offline, same idea as a real mech shutdown) until they re-engage, enabling ambush setups —
classically combined with Bait (MODS-T6): one mech baits a lone enemy in while the rest of the
lance powers down nearby, then powers up together to spring the ambush.

## Depends on
MODS-T5 (commands available to all players).

## Mechanic
- "Power Down": the commanded mech(s) shut down in place — no movement, no weapons fire, engine
  and active sensors offline. While powered down:
  - Invisible to enemy sensor/detection checks (treated as if not present for AI target-acquisition
    purposes), same as the base game's own "mech shutdown" state should already behave, if one
    exists to build on.
  - Still physically present and can still take direct damage if an enemy happens to walk into / 
    visually spot it (stealth is about *sensor* detection, not full invincibility) — exact rule
    is a judgment call, flag it for confirmation before implementation.
- "Power Up": brings the mech back online — either on explicit player command (for a coordinated
  ambush) or automatically if the mech takes direct damage/is spotted while powered down (so a
  discovered ambusher isn't stuck helpless).
- Multiple mechs can be ordered to Power Down together and Power Up together (a group command), to
  support the "everyone ambushes at once" use case the Bait combo is built around.

## What breaks stealth (needs a decision before implementation)
- Taking any damage — always breaks it.
- Player manually canceling — always breaks it.
- Enemy proximity/line-of-sight — open question: does getting *close* to an alert enemy break
  stealth on its own (more realistic, harder to pull off safely), or only actual detection logic
  breaks it (simpler, more forgiving)? Flag for the user's call once this reaches implementation.

## UI
- New radial-menu / command entries: "Power Down" and "Power Up," usable on one or multiple
  selected lancemates.
- Clear status indicator (icon/outline) on a powered-down mech's portrait/nameplate so the lance
  always knows who's dark and who's active.

## Implementation notes (Mod Editor / Blueprint)
- Needs to find whatever existing "mech shutdown" state the base game already has (overheat
  shutdown is a near-certain existing state to repurpose/extend) rather than building a parallel
  invisible-to-sensors state from scratch.
- Needs to confirm how enemy AI target-acquisition actually reads "is this mech detectable" so the
  powered-down state can hook the same check overheat-shutdown already uses, if any.

## Acceptance criteria
- Any player can Power Down one or more lancemates; those mechs stop acting and become
  undetectable to enemy sensors.
- Power Up (manual or damage-triggered) reliably brings a mech back to normal combat state.
- Combined with Bait, a full "bait then ambush" sequence is playable: one mech baits, the rest
  power down nearby, then power up together once the target is drawn in.
