# MODS-T3 — Vote-gated star jump

## Goal
Any connected player can propose jumping to the next system/contract; the jump only happens once
the other players confirm.

## Current behavior (assumed, needs confirming in the editor)
Star-map jump initiation is host-authoritative: only the host's UI exposes the "jump" action, and
it executes immediately.

## Proposed design
- Any player can select a destination and propose a jump from the star map.
- On proposing, every other connected player gets a confirmation prompt: "`<Player>` wants to jump
  to `<Destination>`. Confirm?" with Yes/No.
- Resolution rule: jump proceeds once **all** connected players have confirmed (or the proposer is
  the only one left in session). A single "No" cancels the proposal and returns everyone to the
  star map with a brief notice of who declined.
- Timeout: if a player doesn't respond within a set window (default 30s, make it configurable),
  treat that as a soft default (lean "Yes" with a visible countdown) rather than silently blocking
  forever on an AFK player — exact default is a judgment call for whoever implements it, call it
  out for the user to confirm before shipping.
- While a vote is open, show every player a shared "vote in progress" banner with live
  yes/no/pending state per player, so nobody is left wondering why the map froze.

## Implementation notes (Mod Editor / Blueprint)
- Needs the editor's multiplayer-RPC primitives: a "propose jump" event from the proposing client
  to the host (if the host remains authoritative for actually executing the jump) or to all
  clients (if jump execution can be made fully distributed).
- A new UI widget for the confirmation prompt and the shared vote-status banner.
- Needs to confirm whether star-map/jump state is currently host-only replicated data, or already
  visible to clients (affects how much new replication this needs).

## Acceptance criteria
- A non-host player can propose a jump.
- The jump executes for everyone only once all players confirm; any single decline cancels it for
  everyone, with a clear in-UI reason shown.
- No player can be silently overridden or left on a different star-map state than the rest of the
  lance.
