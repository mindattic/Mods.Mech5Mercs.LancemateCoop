# MODS-T9 — Hand weapons for every mech with hands

## Goal
Any mech chassis with hand actuators can equip hand/melee weapons, whatever its weight class, with
damage scaled up or down by the mech's tonnage.

## Current behavior (assumed, needs confirming in the editor)
Hand/melee weapon equipment is restricted by chassis (specific variants or weight classes) through
hardpoint/slot rules or per-item allowed-chassis lists.

## Proposed design
- Eligibility: a mech can mount a hand weapon if its arm has a hand actuator. Weight class and
  variant no longer matter.
- Damage scaling: each hand weapon keeps its stock damage as the value at a reference tonnage.
  Final damage = base damage x (mech tonnage / reference tonnage).
  - Reference tonnage per weapon should be whatever chassis it was originally balanced for (for
    example, if a weapon was tuned for 70-ton mechs, a 35-tonner does half, a 100-tonner about 1.4x).
  - Clamp the multiplier (suggest 0.4x to 1.6x) so a 20-ton mech can still do something and a
    100-ton mech doesn't one-shot everything. Tune by playtesting.
- Tonnage/slot cost of the weapon itself stays stock, so light mechs still pay a real price to
  carry one.
- The mechlab tooltip shows the scaled damage for the current mech, not just the base number, so
  players aren't misled.

## Implementation notes (Mod Editor / Blueprint)
- Find where hand-weapon equip eligibility is decided (item data asset allowed-chassis list,
  hardpoint type, or a mechlab validation Blueprint) and replace it with a hand-actuator check.
- Apply the tonnage multiplier where melee damage is calculated at hit time, reading the attacking
  mech's tonnage, so it works without duplicating every weapon item per weight class.
- Check animations: hand weapons on chassis that never had them may need the generic melee
  animation set. Visual clipping on very small or very large mechs is a known risk to look at.

## Acceptance criteria
- Every chassis with a hand actuator can equip hand weapons in the mechlab.
- Damage dealt matches the tonnage formula, within the clamp, and the tooltip shows it.
- Mechs without hand actuators still can't equip them.
