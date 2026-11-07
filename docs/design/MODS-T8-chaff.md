# MODS-T8 — Chaff countermeasure

## Goal
Every mech gets a "Chaff" countermeasure. When homing missiles (LRMs, Streak SRMs, ATMs, or any
other lock-on weapon) are inbound, pressing the bound key makes those missiles lose their lock on
the mech and home onto a decoy instead.

"Chaff" is the right term for decoying radar/sensor-guided missiles, which is how MW5's lock-on
weapons work. Flares are the heat-seeker equivalent, and nothing in MW5 guides on heat, so Chaff
is the better fit.

## Mechanic
- Limited charges per mission (suggest 3, tune by playtesting). They refill at mission start, the
  same way Flush Coolant (MODS-T4) does.
- On activation:
  - Spawns a short-lived decoy point just behind/beside the mech (suggest a few seconds' lifetime).
  - Every homing missile currently locked on this mech and still in flight retargets to the decoy.
    Missiles already in the terminal phase (very close to impact) may be excluded so chaff can't
    trivially negate everything. Exact cutoff is a tuning call.
  - Enemy lock-on against this mech is broken for a short window (suggest 2-3s), so the enemy can't
    instantly re-lock and fire a fresh volley.
- Cooldown between uses (suggest 5s) so it can't be chained to keep a permanent lock-break.
- Dumb-fire missiles (SRMs, MRMs, unlocked LRMs) are unaffected because they aren't homing.
- Counterplay balance: this overlaps with Anti-Missile System (AMS) in purpose. Chaff is a burst
  answer on a short supply; AMS is passive and continuous. Both should stay worth having.

## UI
- New entry in the "MindAttic" Controls section (introduced by MODS-T4), "Chaff," with a suggested
  default key that doesn't clash with stock binds (to be picked once the stock bind list is checked
  in the editor).
- HUD: a charge counter next to the Flush Coolant counter, and an incoming-missile warning cue
  if the base game doesn't already make it obvious when to press it.

## Implementation notes (Mod Editor / Blueprint)
- Needs the projectile Blueprint for homing missiles and how it stores its target (a target actor
  reference, a lock component, etc.). Retargeting means overwriting that reference with the decoy.
- The decoy can be a lightweight invisible actor with whatever component the lock-on system checks
  for, plus a visual/particle burst so players can see it was used.
- Multiplayer: the activating client asks the host to spawn the decoy and retarget, so every client
  sees the same missiles veer off.

## Acceptance criteria
- Every mech has Chaff with a visible, limited charge count, bound under the MindAttic section.
- Using it while homing missiles are inbound reliably makes them miss the mech.
- Dumb-fire weapons are unaffected, and it can't be chained to make a mech permanently unlockable.
