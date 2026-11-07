# MODS-T2 — Shared briefing audio

## Goal
Every connected coop player hears the campaign mission-briefing voice-over (commander/narrator
dialogue), not just the host.

## Current behavior (assumed, needs confirming in the editor)
Briefing dialogue is almost certainly host-authoritative: the briefing cutscene/dialogue sequence
plays as a local-only presentation on the host's client, with no network event telling clients to
also play it. Clients either hear nothing or get no equivalent cue.

## Proposed design
- When the host's briefing dialogue sequence starts, fire a replicated (host -> all clients)
  event carrying: the dialogue/cutscene asset ID (or line ID) and a start timestamp.
- Each client, on receiving the event, plays the same audio/dialogue asset locally in sync
  (subtitle UI included, if the base game shows one).
- If the briefing also blocks/pauses other actions for the host (e.g. can't launch until the
  briefing finishes), clients should get the same soft-lock so everyone stays in sync, rather than
  clients racing ahead while the host is still listening.
- Players who join mid-briefing (if that's possible) should at minimum get the audio from their
  join point, not a hard desync — exact behavior is a judgment call once we see what hook the
  editor exposes for "briefing started".

## Implementation notes (Mod Editor / Blueprint)
This needs an editor pass to confirm:
- The Blueprint/event that fires when a briefing's dialogue sequence starts (likely a dialogue
  system "play sequence" node or a mission-flow Blueprint state).
- Whether the existing coop netcode already has a generic "replicate this event to all clients"
  node/function we can hook into, or whether this needs a new custom replicated event.
- Whether dialogue assets are addressable by a stable ID client-side (they should be, since every
  client has the same game content installed) so we only need to replicate the ID + timestamp, not
  any audio data itself.

## Acceptance criteria
- In a coop session, every connected client hears the mission briefing dialogue at (or very near)
  the same time as the host, with no manual action required.
- No audio content is duplicated into this mod's own assets — only existing game content is
  triggered to play, same as any other mod that reacts to or triggers stock game events.
