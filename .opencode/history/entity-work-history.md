# Entity Work History

## Purpose and Current State

Keep reusable representation examples, references and decisions for individual-entity work. The discarded bulk migration's entity inventory, completion counts, old pending lists and reset execution logs have been removed from this document. They are not a current work queue.

Entity data was restored to Git HEAD `0b2f887d6d2d2953d9a6896002c06044454434ff`; previously committed data remains, while uncommitted bulk extensions were discarded. No entity has been newly certified under the individual workflow. Historical examples are design references, not proof that the live records contain those fields or are complete.

The active rules, JSON examples, supported types and stage checklist are in `.opencode/specs/entity-calculation-workflow.md`. Consult that rulebook and inspect the selected live record before reusing a past representation. This history does not authorize a bulk migration.

## Reusable Examples and References

The following are lessons from earlier attempts, not a retained inventory of completed entities. Values and source interpretation must be revalidated for the selected patch and entity. References identify the earlier evidence; they do not constitute fresh validation.

### Separate Result Modifiers from Independent Damage

- **Aquila Favonia:** the passive ATK increase is a modifier; the independent physical damage proc is a skill. Their refinement arrays are separate. The earlier example did not fully represent the healing clause, so those two fields alone do not certify passive completeness.
  - References: `https://gi.yatta.moe/api/v2/en/weapon/11501`, `https://library.keqingmains.com/equipment/weapons/swords#aquila-favonia`.
- **Cinnabar Spindle:** attached skill damage uses targeted `dmg_percent`, `target: "elemental_skill"`, and `conditional: { origin: "character", attribute: "def" }`; it is not a DEF% stat grant or an independent proc.
  - Reference: `https://library.keqingmains.com/equipment/weapons/swords#cinnabar-spindle`.
- **Crescent Pike:** the approved attached-attack convention similarly uses a Normal Attack target and ATK reference, not an independent damage skill. Source wording and actual application must still be inspected.
  - Reference: `https://library.keqingmains.com/equipment/weapons/polearms#crescent-pike`.

### Repeated Hits and Fixed Additions

- **Ayaka:** repeated identical sub-hits use `hits: 3`, while `scalings` stores one hit's coefficient, not the total. Historical level-1 examples were `0.226464` for the 4-Hit row and `0.55126` for Charged Attack.
  - Reference: `https://gi.yatta.moe/api/v2/en/avatar/10000002`.
- **Baizhu:** keep the HP coefficient and unrounded fixed addition separate. Historical level-1 healing used `scalings: 0.08`, `add: 770.3755`; shield absorption used `0.008` and `77.03752`. Do not invent a damage element for healing or shields.
  - Reference: `https://gi.yatta.moe/api/v2/en/avatar/10000082`.
- **Noelle:** the same addition rule applies to DEF-based healing/shields. Historical healing used `scalings: 0.2128`, `add: 102.71802`, while preserving the rounded display `21.3% DEF + 103`.
  - Reference: `https://gi.yatta.moe/api/v2/en/avatar/10000034`.

### Bonus Meaning, Damage References and Consumer Inputs

- **Faruzan:** retain the exact owner-approved `type: "atk_percent"`, `damageType: "ANEMO"`, `bonus: true` convention as a historical decision. It distinguishes a supplied bonus from a hit; it does not grant ATK and is not a universal elemental-bonus template. Recheck the source and legacy display before reuse.
  - Reference: `https://gi.yatta.moe/api/v2/en/avatar/10000076`.
- **Raiden:** the skill's historical level-1 `0.0022` bonus coefficient is applied using the receiving burst's Energy cost. Never pre-multiply it by Raiden's own cost of 90 or introduce a `per` field.
  - Reference: `https://gi.yatta.moe/api/v2/en/avatar/10000052`.
- **Furina:** damage and incoming-healing conversions are distinct bonuses; Fanfare is consumer input. Do not treat incoming healing as outgoing healing or the damage bonus as Hydro-only.
  - Reference: `https://gi.yatta.moe/api/v2/en/avatar/10000089`.
- **Razor:** Soul Companion's coefficient references Normal Attack damage, not ATK: `type: "dmg_percent"`, `target: "normal_attack"`, `damageType: "ELECTRO"`. The earlier English display said 20% while the imported level-1 coefficient was `0.24`; this is evidence requiring translation review, not a validated display to copy.
  - Reference: `https://gi.yatta.moe/api/v2/en/avatar/10000020`.
- **General lesson:** the word "Bonus", similar wording or equal coefficients never establishes whether a result is generic damage, a scoped bonus, a stat conversion or an additive attack coefficient. Inspect the corresponding source mechanic before selecting a type/target.

### Components, Timing and Coverage

- **Albedo:** preserve distinct hit components and verify the raw DEF-based coefficients. A split-hit example does not authorize splitting different attributes contributing to one result.
  - Reference: `https://gi.yatta.moe/api/v2/en/avatar/10000038`.
- **Freminet:** separate Cryo and physical Pressure components with their own elements and coefficients. The earlier Pressure 1-3 split did not certify Pressure 4 or imply simultaneous application/timing.
  - Reference: `https://gi.yatta.moe/api/v2/en/avatar/10000085`.
- **Chevreuse:** Healing Duration is presentation/usage data, not a missing healing coefficient. Excluding that row from multiplier diagnostics never permits deleting it or declaring healing coverage complete.
  - Reference: `https://gi.yatta.moe/api/v2/en/avatar/10000090`.
- **Berserker:** a 2-piece CRIT Rate modifier does not cover the additional 4-piece coefficient. The HP threshold belongs to consumer state; the supplied additional bonus still needs its own representation.
  - References: `https://game8.co/games/Genshin-Impact/archives/305480`, `https://library.keqingmains.com/equipment/artifacts#berserker`.
- **All-element bonuses:** expand to seven elemental modifiers, excluding physical damage. Preserve separate base/additional coefficients and do not pre-apply stacks or party inputs.
  - Reference example: `https://library.keqingmains.com/equipment/weapons/polearms#calamity-queller`.
- **Grouped attributes:** ATK+EM, ATK+DEF or ATK+HP contributing to one result must not be disguised as separate attacks. The UUID-based `specification`/`multipliers` direction remains deferred; consult the rulebook before any structural change.

## General Source and Validation Lessons

- Yatta level parameters can provide raw coefficients, but many passives, constellations, weapons and set effects remain prose inside JSON. An accessible API is not a complete calculation schema.
- KQM provides mechanical explanations and versionable curve tables; use entity-specific source clauses and source revisions rather than matching numbers anywhere on a page.
- Verify identity, locale labels, source rows, all levels/refinements and coefficient meanings. Profile mismatches require factual validation, not an importer assumption.
- A complete Portuguese table can guide structure, but it does not override validated source coefficients. Locales are complete responses, not overlays.
- Keep progression, stat composition, hit scaling and final damage as separate layers. Consumer activation/timing/state does not become entity coefficients.
- Passing JSON, formatting or idempotence checks does not prove full source/semantic coverage. Record actual checks and remaining gaps for each entity.

## Semantic Rule Consistency

When a text interpretation establishes or changes a general rule:

1. Record a stable rule key, mechanical meaning, intended representation and applicability limits in the rulebook.
2. Review equivalent occurrences in entities actually worked under the individual process, including relevant partial work and prior reference examples. Inspect live records; do not resurrect the discarded bulk inventory as a completion list.
3. Record each reviewed source-item/path as compatible, not applicable, correction required or unresolved, with its reasons/references.
4. Preserve intentional native-container differences. Correct genuine inconsistencies individually with an explicit scope decision; do not run a bulk migration.
5. Do not claim global consistency while applicable prior occurrences remain unchecked or structural follow-ups remain unresolved.

## Future Individual Entries

No individual entity has yet been completed under this workflow. Append a dated entry only for work actually performed, using the template in `.opencode/specs/entity-calculation-workflow.md`: selected entity/scope, source URLs and patch, rules/actual snippets, compatibility review, validations, blockers and next approved action.

## Optional Historical Evidence

The existing `.opencode/history/calculation-reset-2026-10-13.tar.gz` backup and `.opencode/history/structured-effect-data-blockers-2026-10-13.md` frozen report were preserved. They contain discarded experimental output, not current entity status or permission to restore data. Consult only when a specific example requires additional evidence; never extract/apply them over the worktree without an explicitly scoped request.
