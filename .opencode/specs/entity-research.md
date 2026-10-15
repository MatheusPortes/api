# Entity Research Policy

## Scope

Apply this policy before using external factual information to create, correct, or edit an entity, translation, image label, relationship, or entity calculation data.

## Validation Sources

1. Prefer the three primary community sources for the exact entity:
   - Game8: `https://game8.co/games/Genshin-Impact`
   - HoYoWiki: `https://wiki.hoyolab.com/`
   - Genshin Impact Wiki: `https://genshin-impact.fandom.com/`
2. Approved supplementary sources for numerical mechanics, discovery, and cross-validation are:
   - KQM Theorycrafting Library: `https://library.keqingmains.com/`
   - Yatta: `https://gi.yatta.moe/`
   - Honey Hunter: `https://gensh.honeyhunterworld.com/`
   - Nanoka: `https://gi.nanoka.cc/`
   - GenshinDB: `https://genshindb.com/`
   - Akademiya: `https://genshin.akademiya.app/`
   - Genshin-Builds: `https://genshin-builds.com/`
3. Use entity-specific pages, not source homepages, and compare each factual field being changed. Dynamic pages may be validated from their server-rendered content or data exposed by their page requests; do not automate browser-only interactions that cannot be independently inspected.
4. Treat a mechanical fact as validated when two independent accessible sources agree, with at least one primary source when one is available. Record unavailable primary sources and any disagreement in the final report.
5. Do not edit factual data when accessible sources conflict without an official Genshin tie-breaker. Report the blocker instead.
   - Explicit owner decision on 2026-10-15 for `artifacts/nighttime-whispers-in-the-echoing-woods` only: use the matching KQM/Yatta description including nearby Lunar-Crystallize Moondrifts, despite Game8's older shield-only table. Record this as an owner-selected source resolution, not an official tie-breaker or permission to bypass future conflicts. This does not resolve Archaic Petra's separate affected-element question.
   - Explicit owner decision on 2026-10-15 for `artifacts/scholar`: after the trigger-wording difference was presented, the owner directed an empty 4-piece modifier list and authorized the text change. Align EN with matching KQM/Yatta "Gaining Elemental Particles or Orbs" rather than Game8/local EN's generic "Gaining Energy"; retain already matching PT. Record as owner-selected source resolution, not official verification or a blanket override for future differences.
   - Explicit owner confirmation on 2026-10-15 for `artifacts/traveling-doctor`: after reporting that accessible clauses only say "restores 20% HP", the owner confirmed recovery is based on the character's Max HP. Apply the existing HP-referenced healing contract and record the reference as owner-confirmed, not independently explicit in those source clauses. This resolves only that reference, not future ambiguous percentages or a complete healing formula.

## Calculation Data Exception

- Owner resolution on 2026-10-15 for `artifacts/archaic-petra`: existing EN/PT modifiers are accepted as correct and must remain unchanged. The owner authorized resolving the description difference using matching KQM/Yatta Lunar-Crystallize clauses despite Game8's older clause. Close the element-mapping follow-up by owner decision, not a newly independently certified Lunar element or blanket source override. Exact evidence and scope remain in history.
- Owner confirmation on 2026-10-15 for `artifacts/ocean-hued-clam`: the proc deals physical damage; set its existing independent skill's `damageType` to `PHYSICAL`. KQM also explicitly identifies physical damage, while the inspected Game8/Yatta set clauses do not classify it. Record the owner confirmation and that evidence limit, not two-source classification certification or a standard DEF/CRIT damage formula. Other previously reported artifact follow-ups were dismissed by the owner, not silently validated.

- Character and weapon calculation data may use Yatta as the structured entity source and KQM TCL as the shared growth-curve source and passive-multiplier reference.
- Validate that the Yatta entity name, type, and element or weapon type match the local record before importing values.
- Keep upstream IDs and source metadata out of public entity responses.
- This exception applies only to numerical progression and calculation fields. Other entity facts still require the validation sources above.

## References

Every response for a data change or factual research task must include a `References` section. List the source name, entity-specific URL, and the facts validated there. State disagreements, unavailable sources, and any official tie-breaker explicitly.

## Release Status

Do not add unreleased, preview, leaked, or speculative content. Confirm the entity is released in the global game version through the required sources.
