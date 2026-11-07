# MODS-T6 — Bait command

## Goal
A new lance command, "Bait": the commanded mech approaches a designated target, engages briefly,
then automatically retreats back to the squad — used to pull aggro off a lone enemy toward the
baiting mech, typically to set up an ambush (see MODS-T7).

## Depends on
MODS-T5 (commands available to all players) — Bait is only useful once any player can issue it to
any lancemate, not just the commander's own AI.

## Mechanic
1. Player targets an enemy and issues "Bait."
2. The commanded mech moves to effective weapons range of the target and engages (fires its
   loadout normally) for a short window (suggest 5-10s, or "N engagements," whichever reads
   better in practice).
3. The mech then automatically disengages — stops firing, turns, and moves back toward the
   lance's current position/formation point, same as a "Return to squad" or "Hold" command would
   resolve movement.
4. While baiting, the mech should prioritize staying alive over damage output: if it drops below a
   configurable health/armor threshold (suggest 50%) it retreats immediately regardless of the
   engagement timer.
5. Issuing any other command cancels Bait early (same as canceling any other lance order).

## UI
- New radial-menu / command-list entry: "Bait," taking a target the same way existing attack-type
  commands already take one.
- Brief on-screen indicator while a mech is baiting (e.g. a status icon near its portrait/nameplate)
  so the rest of the lance can see the plan is in motion.

## Implementation notes (Mod Editor / Blueprint)
- Likely builds on whatever AI-behavior state machine already drives existing commands (e.g.
  "Attack" + "Hold position" style states) — Bait is essentially Attack-with-a-timer-and-an-
  automatic-retreat, so reusing that state machine rather than building pathing/combat AI from
  scratch is the efficient path.
- Needs the retreat/return-to-squad logic that other commands (e.g. "Hold"/"Follow") already use,
  so Bait's retreat behaves consistently with the rest of the command set.

## Acceptance criteria
- Any player can order any lancemate to Bait a target.
- The mech engages briefly, then reliably disengages and returns without further input.
- The mech breaks off early if it takes heavy damage, rather than following the timer into a kill.
