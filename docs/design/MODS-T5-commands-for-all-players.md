# MODS-T5 — Commands for all players

## Goal
Any connected coop player can issue lance commands (move, attack, hold, focus-fire, and the new
Bait/Power Down commands below) to any lancemate — not just the host/commander (Player 1).

## Current behavior (assumed, needs confirming in the editor)
The lance-command radial/menu and its execution are host-authoritative: only the host's input is
wired to the command-issuing system, and/or only the host's client has UI for it.

## Proposed design
- Every connected player gets the same command UI (radial menu / keybind) the host currently has.
- A player can target and command **any** lancemate, including ones piloted by other human
  players' lancemate AI slots or the host's own AI lancemates — command authority isn't restricted
  to "your own" mech's AI.
- Simultaneous/conflicting commands: if two players command the same lancemate within a short
  window, the later command wins (last-writer-wins) — simplest rule, revisit only if playtesting
  shows it's confusing.
- Command source should be visible: when a non-host player issues a command, show a brief
  attribution (e.g. a small "`<Player>` ordered `<Lancemate>`: Bait" toast) so nobody's confused
  about who's steering whose mech.

## Implementation notes (Mod Editor / Blueprint)
- This is the prerequisite for MODS-T6 (Bait) and MODS-T7 (Power Down/Up): both only make sense
  once any player can issue them to any lancemate.
- Needs to find wherever the base game currently gates the command system to "is this the host" (or
  equivalent) and either remove that gate or replicate the command-issuing RPC from every client
  instead of just the host's local input.

## Acceptance criteria
- Every connected player has the same command menu/keybind the host has.
- A non-host player can successfully command any lancemate (AI or otherwise), with the result
  visible to all players.
