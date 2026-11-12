# Entity Calculation Workflow

## Scope and Authority

This is the active rulebook for calculation data. The 2026-10-13 bulk migration was retired and the committed baseline restored on 2026-10-14; never restore or extend its output in bulk. Work one explicitly selected entity at a time. Existing fields and historical examples do not prove coverage.

Read `application-state.md`, `data-conventions.md`, `entity-research.md`, this file, and the named entity/rule entry in `.opencode/history/` before calculation work. History is evidence, not instructions.

## Per-Entity Process

1. **Select:** record exact local ID, requested scope, baseline, patch and source items. Authorized rounds are sequential alphabetical artifacts (max 5), weapons (max 20), or characters under the bounded authorization in `git-workflow.md`, after the completed `albedo` pilot. Stop at the authorized scope limit, unresolved source conflict under `entity-research.md`, or unapproved semantic/schema decision; do not skip the blocked entity or advance before its coordinated commits.
2. **Inventory and research:** classify every source clause (stat/result, proc, hit, fixed addition, reference, stack/tier, consumer usage, unsupported); identify JSON paths. Validate identity and numbers under `entity-research.md`; retain URLs/access date/revision/raw responses outside served data.
3. **Map:** reuse only an approved contract below. Map levels/ranks, references, targets, elements, caps and independent components explicitly. Do not infer a type from matching wording or numbers.
4. **Apply:** change only selected EN/PT records; preserve other locales and unrelated fields. Validated EN/PT display alignment is standing-authorized; resolve source differences under `entity-research.md` before applying.
5. **Validate:** JSON; raw coefficients; levels/ranks; types/metadata; hit/add/cap semantics; EN/PT parity; focused diff/format; `pnpm run build` and `pnpm run lint` with Node 20/pnpm. Report failures or omissions; never repair unrelated baseline failures.
6. **Record and deliver:** append the concise outcome to the appropriate history file, then commit on `agent` as `<id> - <what was done>`. For a bounded round, independently mirror only that entity's served JSON to the configured delivery branch before selecting the next item. Follow `git-workflow.md`; do not transfer `.opencode/`, push, or start a further round without authorization.

States: `not-started`, `in-progress`, `partial`, `blocked`, `complete`, `reviewed-not-applicable`; `archived-reset` is historical only. “Complete” always names reviewed scope; consumer usage alone does not leave multiplier coverage pending.

## Universal Rules

- Percentages are ratios (`20%` → `0.2`); direct units remain direct. Preserve raw precision.
- Keep each source contribution distinct; never deduplicate equal values or pre-sum base/additional clauses.
- Bonus composition follows `consumer-composed-source-contributions`; do not bake another source's bonuses into a derived coefficient.
- `type` describes the supplied stat/result, not its scaling attribute. Keep provider metadata and sources out of served JSON.
- A stat/result modifier uses the approved source effect's `modifiers`; an independent proc uses its approved `skills` container. Artifact procs nest at their originating `effects[].skills`; reviewed character source items follow the character rules below, and weapon containers retain their approved native layout.
- `bonus: true` marks a supplied additive stat/bonus contribution; omit `bonus: false`. Signed ATK/DMG/RES penalties follow `signed-additive-atk-penalty`, `signed-additive-dmg-penalty` and `signed-wearer-resistance-penalty` below. An independent hit normally omits it.
- Supported attack targets are `normal_attack`, `charged_attack`, `plunging_attack`, `elemental_skill`, `elemental_burst`. Generic bonuses omit target. `elemental_dmg_percent` requires `element`; all-element grants expand to GEO/ANEMO/CRYO/DENDRO/ELECTRO/HYDRO/PYRO, never Physical.
- Attribute `conditional` is one `{ origin, attribute, offset? }` reference or a nonempty array of references multiplied under `multiplicative-attribute-references`. Attribute origins: `character`, `weapon`, `artifact`, `time`; attributes: `hp`, `atk`, `base_atk`, `def`, `elemental_mastery`, `energy_recharge`, `energy_cost`, `cleared_bond_of_life`, `bond_of_life`. Optional `offset` follows `attribute-reference-baseline-offset` below. `character.base_atk` follows `character-base-atk-reference`; `time.energy_cost` is exclusively combined party maximum Burst Energy Cost; `character.cleared_bond_of_life` is cleared HP units, while `character.bond_of_life` is the current normalized ratio under `current-bond-of-life-reference`. Coefficients stay in `values` or talent component `scalings`. Healing modifiers additionally allow the narrowly defined single talent-result reference in `character-talent-healing-reference`; it is not an attribute-product factor.
- Activation, timing, resource use, chance, stacks currently active, party composition, target state, infusions and formula execution are consumer state. Preserve them in localized text.
- `hits` is a positive count of identical subhits; store one-hit value. `add` is a separate fixed value in talent `power`; never use either for stacks. Do not split multi-attribute one-result mechanics: reviewed character talent damage, healing and shield absorption group them under their respective component rules below. Shield fixed terms follow `character-grouped-shield-absorption-components` instead of `add`; other grouped results remain deferred.

## Approved Contracts

| Rule key                                            | Representation and limit                                                                                                                                                                       |
| --------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `attribute-referenced-healing`                      | `healing` ratio + attribute `conditional`; no `bonus`. Fixed HP recovery is `healing` direct units without conditional/bonus.                                                                  |
| `flat-defense-bonus`                                | Direct DEF is `flat_def`, direct units, `bonus: true`.                                                                                                                                         |
| `artifact-source-effect-skills`                     | Artifact independent proc is `effects[].skills`, beside `label` and `modifiers`; no root duplicate.                                                                                            |
| `accumulated-healing-independent-damage`            | Independent scaling `accumulated_healing_percent`; `maxValues` caps recorded-healing HP input before ratio (Clam: `0.9`, `30000`).                                                             |
| `accumulated-healing-attached-damage`               | Attached recorded-healing damage is targeted `accumulated_healing_percent`, `bonus: true`; same input-cap meaning (Song: `0.08`, `15000`).                                                     |
| `consumer-only-status-duration-reduction`           | Native effect with `modifiers: []`; no duration/resistance substitute.                                                                                                                         |
| `consumer-only-fixed-energy-restoration`            | Native effect with `modifiers: []`; no Energy-result schema or ER/healing substitute.                                                                                                          |
| `target-state-crit-rate`                            | Separate `crit_rate`, `bonus: true`, condition-bearing label; no conditional/target. Keep independent condition contributions separate.                                                        |
| `attack-scoped-crit-rate`                           | `crit_rate`, positive ratio, `bonus: true`, validated benefiting attack target only.                                                                                                           |
| `reaction-scoped-damage-bonus`                      | `dmg_percent`, nonempty `reaction`, `bonus: true`; one modifier for one shared source multiplier.                                                                                              |
| `reaction-selected-elemental-bonus`                 | One explicit elemental modifier per eligible alternative; alternatives are not simultaneous and are not all-element grants.                                                                    |
| `equal-coefficient-reaction-alternatives`           | Equal nonstacking alternatives from one contribution may share a reaction list only when activation/reference/cap/stack semantics match.                                                       |
| `fixed-source-bonus-increase`                       | Store each derived absolute source contribution (e.g. base × stated fraction), separately; preserve derivation in history.                                                                     |
| `stackable-bonus-cap`                               | Uniform per-stack value + integer `accumulations > 1`; actual count is consumer state.                                                                                                         |
| `bounded-bonus-step-count`                          | Uniform bounded absolute bonus steps may use per-step value + cap; do not create a separate total/base grant.                                                                                  |
| `nonuniform-stack-tier-totals`                      | Separate condition-labeled mutually exclusive total modifiers; omit `accumulations`, never sum tiers.                                                                                          |
| `party-member-counted-bonus`                        | Per-eligible-member value + contributor cap; holder eligibility follows the source. Count each member once and preserve mutually constrained counts.                                           |
| `enemy-elemental-resistance-reduction`              | `elemental_res_reduction`, element, positive amount; omit `bonus`, target, conditional and reaction.                                                                                           |
| `nonstacking-same-element-res-alternative-triggers` | One same-element RES reduction for alternative nonstacking triggers; no duplicate summable entries.                                                                                            |
| `wearer-party-element-resistance-bonus`             | `elemental_res_percent`, element, positive value, `bonus: true`; represented elements may coexist, repeats do not stack.                                                                       |
| `attack-scoped-speed-bonus`                         | `atk_speed_percent`, positive value, `bonus: true`, validated benefiting target.                                                                                                               |
| `wearer-shield-strength-bonus`                      | `shield_strength_percent`, positive value, `bonus: true`; distinct from shield creation/absorption.                                                                                            |
| `combined-party-energy-cost-burst-bonus`            | Burst `dmg_percent`, target `elemental_burst`, `conditional: time.energy_cost`, per-point values and same-rank result `maxValues`; consumer applies `min(sum party Burst costs × value, cap)`. |

`maxValues` otherwise caps the supplied result, not the reference; the entity-specific `heretic-distance-atk-range` rule below is an explicit minimum–maximum interval exception. No rule authorizes formulas, timers, recipient selectors, state engines, unsupported scopes, or automatic migrations.

Rule `heretic-distance-atk-range`: the owner-approved Heretic's Molten Blade representation uses exactly one root `atk_percent` modifier with R1–R5 minimum bonuses in `values` [.18,.225,.27,.315,.36], maximum bonuses in `maxValues` [.36,.45,.54,.63,.72], and `bonus: true`. For this entity's modifier only, these are the inclusive lower/upper endpoints of a single distance-dependent ATK-bonus interval, not a scaling coefficient plus ordinary cap, two summable bonuses, or discrete stack tiers. Localized labels identify the minimum–maximum interval. Omit `conditional`, `target`, `accumulations` and skills; the consumer determines the qualifying bonus from distance traveled during the previous second and handles Skill activation, duration/cooldown and off-field removal. No distance unit, interpolation formula or new runtime field is inferred. Existing result caps and recorded-healing input caps keep their meanings; this exception does not authorize other range mappings or automatic migrations.

### Party-member eligibility and thresholds

Rule `wearer-movement-speed-bonus`: an individually source-validated Movement SPD stat bonus uses `movement_speed_percent`, positive ratio `values`, and `bonus: true`, without an attack target. Lost Prayer supplies [.1,.1,.1,.1,.1] at R1–R5 without conditional/accumulations; its separate elemental bonus retains per-stack coefficients/cap. Movement speed is not attack speed, stamina consumption, Energy, or damage. This static stat input does not encode locomotion, sprinting, current stacks or timers and does not authorize migrations of other entities.

Rule `signed-additive-atk-penalty`: an individually source-validated ATK-percent loss may use negative `atk_percent` values with `bonus: true`; the sign subtracts from the additive ATK-percent contribution, not final ATK or damage. Fruit of Fulfillment uses [-.05,-.05,-.05,-.05,-.05] per stack, `accumulations: 5`, separately from direct EM [24,27,30,33,36] with the same cap. Both use the same consumer-selected active stack count. Do not emit positive ATK, `bonus: false`, an invented reduction type, a precomputed cap total, or a timer/state engine. This approval does not redefine other result types or authorize automatic migrations.

Rule `signed-additive-dmg-penalty`: the owner-approved Rust mapping uses negative `dmg_percent` with `bonus: true` and the source-validated benefiting attack target to subtract from that attack's additive DMG-bonus contribution. Rust supplies Charged [-.1,-.1,-.1,-.1,-.1] separately from Normal [.4,.5,.6,.7,.8]; no conditional/accumulations. This is not a final-damage multiplier of .9, an enemy resistance reduction, incoming-damage result, `bonus: false`, or positive grant. Preserve each signed source component and validated scope; do not generalize to unreviewed damage-received mechanics or other result types. No automatic migration or runtime engine.

Rule `signed-wearer-resistance-penalty`: owner approval on 2026-10-27 permits individually validated wearer RES losses as negative ratios with `bonus: true`: `elemental_res_percent` with one explicit eligible element, and `physical_res_percent` without `element`. Itto's originating Burst supplies seven elemental entries and one Physical entry, each `values: [-.2]`, active only during Raging Oni King. Omit attack `target`, `conditional`, `reaction` and stacks; activation/removal stay localized text and consumer state. The negative contribution subtracts 20 percentage points of wearer RES, not 20% final incoming damage, enemy RES, DEF, or outgoing DMG. Existing positive wearer elemental grants and positive enemy `elemental_res_reduction` results keep their meanings; no automatic migration or unvalidated result/scope expansion.

Rule `serpent-spine-consumer-only-incoming-damage`: the owner approved mapping Serpent Spine's outgoing `dmg_percent` [.06,.07,.08,.09,.1], `bonus: true`, `accumulations: 5`, without target/conditional, while retaining its wearer incoming-damage increase [.03,.027,.024,.022,.02] only in localized `passiveDesc` for consumer handling. Current shared count, on-field gains, off-field retention and one-stack loss remain consumer state. Do not encode incoming damage as outgoing bonus/penalty, DEF/RES reduction or a new result type. Completion names outgoing coefficient coverage plus the explicitly text-only incoming component; this entity-specific owner resolution does not certify a structured incoming-damage schema or authorize omissions on unrelated entities.

Rule `the-bell-consumer-only-shield-absorption`: the owner approved retaining The Bell's shield generation/absorption [.20,.23,.26,.29,.32] of wearer Max HP only in localized `passiveDesc`, while structuring its outgoing `dmg_percent` [.12,.15,.18,.21,.24], `bonus: true`, without target/conditional/accumulations. The damage bonus qualifies under any shield, not only the generated shield. Do not add `shield_absorption`, substitute shield strength/HP/healing/incoming reduction, or claim structured shield-capacity coverage. Shield duration, cooldown, remaining capacity/breakage and qualification remain consumer handling. This is an entity-specific text-only completion exception, not a general omission policy or new result schema.

Rule `tulaytullah-separated-increments-consumer-shared-cap`: the owner approved separate Normal-targeted `dmg_percent` bonuses for Tulaytullah's Remembrance: per-second [.048,.060,.072,.084,.096] with `accumulations: 10`, and per-qualifying-hit [.096,.120,.144,.168,.192] without accumulations. Keep its separate Normal `atk_speed_percent` [.10,.125,.15,.175,.20] unstacked. All use `bonus: true`, without conditional/maxValues/skills. The shared total DMG cap [.48,.60,.72,.84,.96] remains explicitly in localized passiveDesc and consumer handling, not independently per modifier or certified by accumulations10. Hit-history/ramp aggregation, 14s duration, .3s qualification, off-field removal and recast reset remain consumer state. Do not normalize the two source clauses into one equivalent-step modifier, add a cap-group schema or treat the hit increment as an unconditional permanent grant. This entity-specific owner-selected representation preserves raw distinct coefficients; it does not certify a structured shared-cap engine or authorize migrations/general omissions.

Under `party-member-counted-bonus`, the source determines whether the holder is eligible; inclusion is not universal. Chain Breaker counts a Natlan member OR a member whose Elemental Type differs from the wearer, counting each once, including a Natlan wearer. Its per-member ATK uses `accumulations: 4`; the separate EM grant requires at least three eligible members and is supplied once, without `accumulations`. Actual eligible-member count and party selection remain consumer state in localized labels/descriptions, not `conditional`. Gilded Dreams and A Thousand Floating Dreams still exclude the holder and retain their validated cap-3 constraints. This source-specific extension does not authorize automatic edits to other entities.

### Attribute references and independent skills

Rule `consumer-composed-source-contributions`: the owner's standing 2026-11-11 API policy keeps each source's raw contribution separate. When an independent attack is expressed as a fraction of another attack's damage, derive its raw attribute coefficients using that fraction and the effective source talent level, without incorporating the referenced attack's separate bonuses or post-bonus damage. Preserve those bonuses independently in their originating approved containers, with their eligibility in localized labels/descriptions. The API consumer combines applicable contributions and applies eligible bonuses once; the API neither automatically inherits nor categorically denies bonus eligibility merely because one attack references another. Do not block solely to ask whether to pre-compose those bonuses, introduce a post-bonus damage-result reference, or flatten separate contributions into a precomputed total. Chasca's A4 therefore uses1.5 times the ordinary/converted Shell raw ATK coefficient, independently of A1's Shining-Shell bonus tiers. This is owner-directed representation, not independent certification of undocumented in-game bonus inheritance. Continue validating raw multipliers, references, elements, attack classifications and explicit source restrictions; genuine conflicts on those facts still stop work. Existing grouped components, approved attribute products, derived raw coefficients, explicit constellation changes and raw healing-result references retain their contracts. No runtime evaluator or automatic migration.

Rule `character-base-atk-reference`: owner approval on 2026-11-11 adds `{ origin: 'character', attribute: 'base_atk' }` for the source character's Base ATK: character innate Base ATK plus the equipped weapon's Base ATK, excluding artifact flat ATK and all ATK buffs. Existing `character.atk` remains total ATK; do not replace it automatically, use weapon-only ATK or reinterpret the EffectType `base_atk` as an attribute reference. Bennett's originating Burst supplies recipient `flat_atk`, bonus true, with effective-Burst-level coefficients [.56,.602,.644,.7,.742,.784,.84,.896,.952,1.008,1.064,1.12,1.19,1.26,1.33] and this Bennett reference. C1 supplies its separate flat_atk[.2] with the same reference, not20% of the initial grant or a recipient Base ATK increase. Field/recipient/HP eligibility, C1 removal of the HP restriction and application/expiry remain localized text and consumer handling. Only character origin is approved for this attribute; no runtime stat evaluator or unrelated migration is added.

Rule `unqualified-hp-reference-default`: the owner's standing 2026-10-20 instruction treats an unqualified HP reference in a percentage recovery or HP-based scaling coefficient as Max HP unless the source explicitly specifies another reference. Reuse `character.hp` for the appropriate source-defined character; do not ask per entity solely because a validated clause says only HP. Explicit current/remaining/lost HP or other stated references take precedence. This default does not infer a recipient, convert HP-state thresholds into scaling, alter fixed-HP recovery, or reinterpret ATK/DEF references. Recurve Bow uses wearer-Max-HP `healing` [.08,.1,.12,.14,.16], without bonus/hits/accumulations. Record the default as owner-directed semantics rather than claiming the omitted denominator was independently explicit. Prior wearer/recipient Max HP mappings remain unchanged; no bulk migration.

Rule `recipient-max-hp-healing-context`: for individually reviewed recipient-Max-HP healing, the owner-approved Prototype Amber representation reuses `healing` with `conditional: character.hp` and explicit localized recipient-Max-HP labels. The consumer supplies each healed recipient's Max HP for that modifier, not the wearer's HP. Store per-tick R1–R5 [.04,.045,.05,.055,.06], without `bonus`, `hits`, `accumulations` or aggregated duration totals. Recipient selection, interval/duration and fixed Energy restoration remain consumer text; no recipient selector/origin or runtime engine is added. This context is explicit and entity-specific; other existing wearer HP references retain their meaning and are not migrated automatically.

Rule `attribute-reference-baseline-offset`: an individually source-validated above-baseline reference may supply optional finite nonnegative `conditional.offset`, in the referenced attribute's native units. The consumer uses `max(attributeValue - offset, 0)` before multiplying by `values`; `maxValues` retains its existing result-cap meaning. Omission preserves the original unmodified reference; never infer a baseline globally. Engulfing Lightning uses offset `1` for ER above 100% (ratio units), ATK coefficients [.28,.35,.42,.49,.56] and result caps [.8,.9,1,1.1,1.2]. Emblem of Severed Fate continues using full total ER without an offset. This supplies static inputs, not runtime formulas or automatic migrations.

Rule `cleared-bond-of-life-reference`: an individually source-validated modifier may reference `{ origin: 'character', attribute: 'cleared_bond_of_life' }`, the total Bond of Life cleared for the qualifying removal, in HP units supplied by the consumer. It is not Max HP, current Bond remaining, or arbitrary total healing. Keep raw coefficients in `values` and cap the supplied result with same-rank `maxValues`; removal tracking/qualification, calculation timing and buff duration remain consumer state. Finale of the Deep supplies `flat_atk` [.024,.03,.036,.042,.048], result caps [150,187.5,225,262.5,300], separately from its initial ATK-percent grant. Do not assume the cleared value is 25% of Max HP, add offset/stack metadata, alter prior Bond-state grants or implement a resource engine.

Rule `current-bond-of-life-reference`: the owner approved only the new name `bond_of_life` on 2026-10-28, not a second `bond_of_life_ratio` attribute. Use `character.bond_of_life` consistently as the current Bond divided by the character's Max HP, in ratio units (130% is 1.3). This single normalized representation supports Arlecchino's ATK × Bond contribution directly; a product of `character.hp` and `character.bond_of_life` supplies the current Bond amount in HP units for healing. Never reinterpret the same attribute as HP units in another path, alias it to `cleared_bond_of_life`, or add `bond_of_life_ratio`. The consumer supplies the source-appropriate current state before the qualifying hit/heal; generation, consumption, thresholds and absorption remain consumer state, not additional stat grants or a runtime engine.

Rule `multiplicative-attribute-references`: owner approval on 2026-10-28 extends `conditional` to a single existing reference object or a nonempty flat array of reference objects. Array entries are multiplicative factors, not alternatives or additive components. Evaluate each factor with its own approved offset, then multiply the factors and the raw scalar coefficient; existing result caps still apply after the contribution. Arlecchino's Masque/C1/C6 use ATK × current normalized Bond as attached damage, retaining separate source coefficients. Components continue summing contributions to one result; each component may itself reference a product. Keep existing single-object references unchanged, reject nested/empty reference arrays, and do not infer undocumented reference products or precompute consumer state. IndependentSkill.scalings[] retains its separate container/values contract and can use the same reference union without conversion to talent components.

Rule `wearer-physical-resistance-bonus`: an individually validated positive wearer Physical RES contribution reuses the approved `physical_res_percent` result with positive ratio `values` and `bonus: true`, without `element`, attack target or reaction. Arlecchino's C2 supplies [.2] for 15s; A4 uses [.0001] per ATK above1000 with `conditional: character.atk`, `offset: 1000` and result `maxValues: [.2]`, matching the separately scoped elemental grants. This is the positive form of the existing additive wearer stat, not enemy resistance reduction, a damage multiplier or a new state engine; Itto's approved negative contribution remains unchanged. No automatic migration.

Rule `attribute-referenced-independent-scaling`: an individually reviewed independent skill's attribute-referenced scaling carries `conditional` beside `type`/`values`. Use the source's reference, not activation state. Ash-Graven retains `hp_percent`, R1–R5 .4–.8, with `character.hp` (Max HP). Do not infer the reference from the type alone or migrate other entities automatically.

Rule `independent-em-referenced-damage`: owner approval on 2026-11-12 permits `IndependentSkill.scalings[]` with `type: dmg_percent`, ratio `values` and `conditional: { origin: 'character', attribute: 'elemental_mastery' }` for raw independent damage based on the source character's EM. Citlali C4 uses values[18] for one Cryo skull explosion, without a benefiting attack target because the source explicitly excludes Burst damage but does not establish another category. This scaling supplies raw damage units (18 times EM), not an EM stat grant, an additive1800% DMG Bonus or a contribution attached to the triggering hit; keep it in the originating constellation's independent `skills`, without `bonus`. Separate applicable bonuses remain consumer composition. Preserve fixed Energy/Nightsoul restoration, proc interval and source restrictions in localized text. Existing independent ATK/DEF/HP scaling types, attached EM modifiers and talent components retain their meanings; no new enum, runtime evaluator or automatic migration.

Rule `independent-triggered-weapon-damage`: a source-validated or explicitly owner-confirmed separate weapon proc uses native `calculation.skills`, even when Normal/Charged attacks trigger it. Keep its attribute reference explicit; trigger categories remain consumer text, not duplicated targeted modifiers. Crescent Pike's owner-approved Physical ATK-referenced proc is independent, unlike Echoes of an Offering's attached damage. Halberd's independent classification is owner-confirmed; use ATK [1.6,2,2.4,2.8,3.2] with `character.atk`, without inferring its damage type. This does not reclassify other weapons or restore archived mappings.

Messenger is owner-confirmed attached weak-point Charged damage, not an independent skill: use root targeted `dmg_percent`, `bonus: true`, ATK ratios [1,1.25,1.5,1.75,2] and `conditional: character.atk`. Weak-point/critical qualification and interval remain localized text/consumer handling; do not create `calculation.skills`, a CRIT DMG stat grant, a forced-CRIT state field, or a pre-applied CRIT multiplier. This reuses the existing targeted attribute-referenced bonus shape and does not reclassify Halberd or Crescent Pike.

Scion of the Blazing Sun's owner-selected split uses an independent native Sunfire Arrow skill, ATK [.6,.75,.9,1.05,1.2] with `character.atk`, and a separate Heartsearer root `dmg_percent` modifier, Charged target, [.28,.35,.42,.49,.56], `bonus: true`, no conditional/stacks. Affected-opponent/wearer-only qualification remains localized text/consumer handling. Do not introduce `dmg_taken_percent`, reinterpret all enemy "take more damage" wording globally, or combine the Arrow with the damage bonus. This is an entity-specific owner resolution, not a new vulnerability formula or damage-type certification.

Rule `required-independent-skill-label`: every `IndependentSkill` requires a nonempty localized `label`, including single-output skills and artifact-nested skills. Reuse established localized passive/proc names where applicable. This does not add labels to separate character talent-table schemas that already use names.

Rule `independent-proc-alternative-totals`: source-validated mutually exclusive proc totals use separate labeled native skills; each label identifies eligibility and replacement. Choose exactly one applicable variant per proc/target, never sum alternatives or multiply by trigger chance. Dragonspine Spear uses ordinary ATK [.8,.95,1.1,1.25,1.4] versus Cryo-affected replacement [2,2.4,2.8,3.2,3.6], with `character.atk`; timing/chance/target state remain consumer text, not attribute references or new state fields. Do not infer damage type from an icicle name or weapon substat.

### Reactions

Rule `direct-reaction-independent-damage`: owner approval on 2026-11-12 adds optional nonempty `IndependentSkill.reaction: Reaction[]` for an individually validated proc explicitly classified as direct reaction damage. This classifies the independent hit itself, not its activation trigger or a DMG Bonus. Cyno's Radiance Starsame uses damageType ELECTRO, reaction[STELLAR_CONDUCT], raw ATK scaling[2] and no attack target; it replaces the ordinary Electro/Skill Bolt for that trigger. Its raw EM addition remains separately source-local on A4: dmg_percent[6], conditional character.elemental_mastery, reaction[STELLAR_CONDUCT], bonus true, with a Starsame-only localized label. That attribute-referenced modifier supplies raw attached damage units, not a dimensionless600% reaction bonus or a second independent hit; unreferenced reaction dmg_percent (C2[.16] per stack) remains a dimensionless bonus. Consumers select the eligible ordinary/enhanced proc, combine its applicable raw contributions and apply separate bonuses under consumer-composed-source-contributions; no reaction evaluator, universal damage/CRIT/DEF formula or implicit inheritance is supplied. Preserve ordinary proc categories and prior independent ATK/DEF/HP/EM scaling meanings. Omit reaction on ordinary procs and activation-only mechanics; never auto-backfill or infer that an attack is reaction damage merely from its element. No new Reaction enum or unrelated migration.

`reaction` is required for reaction-scoped `dmg_percent` and approved reaction-scoped `crit_dmg` under `lunar-reaction-scoped-crit-damage` or `stellar-glimmer-scoped-crit-damage`; omit it on other results and on ordinary/global CRIT grants. Valid values:
`SWIRL`, `STELLAR_SWIRL`, `OVERLOADED`, `ELECTRO_CHARGED`, `SUPERCONDUCT`, `BURNING`, `BURGEON`, `BLOOM`, `HYPERBLOOM`, `LUNAR_BLOOM`, `AGGRAVATE`, `SPREAD`, `LUNAR_CHARGED`, `LUNAR_CRYSTALLIZE`, `STELLAR_CONDUCT`, `STELLAR_GLIMMER`, `VAPORIZE`, `MELT`.

Rule `spread-reaction-bonus`: Spread is the English name of Portuguese Propagação, distinct from Aggravate/Intensificação. The owner confirmed both source clauses in Baizhu's A4. Use `SPREAD` for individually source-validated Spread bonuses; retain reaction-specific formula placement as consumer context. Do not substitute Dendro elemental DMG or broaden existing Aggravate-only bonuses automatically.

Lunar-family bonuses name only source-eligible Lunar reactions; ordinary reaction names do not imply family-wide coverage. Reaction formulas and ownership remain consumer work.

Rule `lunar-reaction-scoped-crit-damage`: a source-validated Lunar Reaction-only CRIT DMG grant may use `crit_dmg`, positive ratio `values`, `bonus: true`, and an explicit nonempty `reaction` list identifying benefiting reactions, not the activation trigger. The owner-approved Nocturne's Curtain Call modifier names `LUNAR_CHARGED`, `LUNAR_BLOOM`, `LUNAR_CRYSTALLIZE` with R1–R5 [.6,.8,1,1.2,1.4], without `target`, `conditional` or stacks. This supplies CRIT DMG only for eligible Lunar Reaction damage, not global CRIT DMG or reaction `dmg_percent`; reaction eligibility/formula/CRIT execution,12s activation and Energy restoration remain consumer work. Do not add reaction scope to a global CRIT grant merely triggered by a reaction, broaden source-specific reaction lists, introduce reaction-scoped CRIT Rate, or migrate existing entities automatically.

Rule `stellar-glimmer-scoped-crit-damage`: the owner-approved Whitelake Frostfeather mapping uses one `crit_dmg` modifier with `reaction: ['STELLAR_GLIMMER']`, R1–R5 [.5,.65,.8,.95,1.1], and `bonus: true`, granted once only while the wearer has three Lake-Hued Lament stacks. Its localized label preserves the threshold; omit `accumulations`, `conditional`, `target` and skills on this CRIT grant. Keep separate per-stack `atk_percent` [.08,.1,.12,.14,.16] with `accumulations: 3`. Fixed Energy recovery remains consumer-only, as do stack duration/activation/off-field state and CRIT execution. This explicit benefiting-reaction extension does not classify Stellar Glimmer as Lunar, broaden Nocturne's reaction list, make CRIT global, grant it per stack, substitute reaction DMG%, authorize reaction CRIT Rate, or migrate unrelated records.

## Shapes and Vocabulary

```ts
type EffectType =
  | 'hp_percent'
  | 'atk_percent'
  | 'atk_speed_percent'
  | 'movement_speed_percent'
  | 'base_atk'
  | 'base_def'
  | 'elemental_dmg_percent'
  | 'elemental_res_reduction'
  | 'enemy_atk_reduction'
  | 'enemy_def_reduction'
  | 'elemental_res_percent'
  | 'physical_res_percent'
  | 'physical_dmg_percent'
  | 'dmg_percent'
  | 'dmg_reduction'
  | 'def_percent'
  | 'crit_rate'
  | 'crit_dmg'
  | 'energy_recharge'
  | 'healing_bonus'
  | 'shield_strength_percent'
  | 'healing'
  | 'shield_absorption'
  | 'accumulated_healing_percent'
  | 'elemental_mastery'
  | 'flat_hp'
  | 'flat_atk'
  | 'flat_def';
type Vision =
  | 'GEO'
  | 'ANEMO'
  | 'CRYO'
  | 'DENDRO'
  | 'ELECTRO'
  | 'HYDRO'
  | 'PYRO'
  | 'NON-ELEMENTAL'
  | 'PHYSICAL';
type AttackTarget =
  | 'normal_attack'
  | 'charged_attack'
  | 'plunging_attack'
  | 'elemental_skill'
  | 'elemental_burst';
type Reaction =
  | 'SWIRL'
  | 'STELLAR_SWIRL'
  | 'OVERLOADED'
  | 'ELECTRO_CHARGED'
  | 'SUPERCONDUCT'
  | 'BURNING'
  | 'BURGEON'
  | 'BLOOM'
  | 'HYPERBLOOM'
  | 'LUNAR_BLOOM'
  | 'AGGRAVATE'
  | 'SPREAD'
  | 'LUNAR_CHARGED'
  | 'LUNAR_CRYSTALLIZE'
  | 'STELLAR_CONDUCT'
  | 'STELLAR_GLIMMER'
  | 'VAPORIZE'
  | 'MELT';
type AttributeReference = {
  origin: 'character' | 'weapon' | 'artifact' | 'time';
  attribute:
    | 'hp'
    | 'atk'
    | 'base_atk'
    | 'def'
    | 'elemental_mastery'
    | 'energy_recharge'
    | 'energy_cost'
    | 'cleared_bond_of_life'
    | 'bond_of_life';
  offset?: number;
};
type TalentHealingReference = {
  origin: 'skillTalents';
  skillTalents: string;
  attribute: 'healing';
};
type AttributeConditional =
  | AttributeReference
  | [AttributeReference, ...AttributeReference[]]
  | TalentHealingReference;
type Modifier = {
  label: string;
  type: EffectType;
  values: number[];
  bonus?: true;
  accumulations?: number;
  element?: Vision;
  reaction?: Reaction[];
  target?: AttackTarget;
  maxValues?: number[];
  conditional?: AttributeConditional;
};
type Effect = {
  label: string;
  refinementLevels?: number[];
  modifiers: Modifier[];
  skills?: IndependentSkill[];
};
type IndependentSkill = {
  label: string;
  refinementLevels?: number[];
  damageType?: Vision;
  target?: AttackTarget;
  reaction?: Reaction[];
  scalings: Array<{
    type: EffectType;
    values: number[];
    maxValues?: number[];
    conditional?: Modifier['conditional'];
  }>;
};
type CharacterConstellationChange =
  | {
      skillTalents: string;
      level: true;
      add: number;
      modifiers?: never;
      accumulations?: never;
    }
  | {
      skillTalents: string;
      modifiers: string;
      accumulations: true;
      add: number;
      level?: never;
    };
// Optional on an individually reviewed constellation:
// changes?: CharacterConstellationChange[];
type CharacterTalentUpgrade = {
  name: string;
  type?: EffectType;
  damageType?: Vision;
  hits?: number;
  bonus?: true;
  accumulations?: number;
  target?: AttackTarget;
  power: Array<{
    level: number;
    value: string;
    components?: Array<{
      scalings: number;
      conditional?:
        | AttributeReference
        | [AttributeReference, ...AttributeReference[]];
    }>;
    add?: number;
  }>;
};
```

Entity progression belongs to top-level `calculation`; talent tables use `skillTalents[*].upgrades[*].power[*]`; character stat/result modifiers follow `character-source-local-modifiers` and independent procs follow `character-source-local-independent-skills` below; weapon passive modifiers use top-level `effects`; weapon independent procs remain `calculation.skills`; artifact effects use top-level `effects`. `refinementLevels` maps array positions; reviewed character talent coefficients use scalar `components[].scalings`, never a duplicate scalar at `power[].scalings`. Omit unknown proc damage type.

Rule `character-source-local-modifiers`: owner approval on 2026-10-23 places individually reviewed character stat/result contributions directly in optional `skillTalents[*].modifiers`, `passiveTalents[*].modifiers`, or `constellations[*].modifiers`. Reuse the existing localized `Modifier` contract; do not duplicate the contribution in `calculation.effects`. Native attack coefficients remain in talent `upgrades[*].power[*]` under `character-talent-attribute-components`, with upgrade-level damageType/target as validated, not bonus modifiers. Use Portuguese `upgrades`/level-indexed `power` as the structural reference when harmonizing EN/PT, but independently validate numbers. Preserve source conditions/recipients/duration in localized descriptions and labels; unlock `level` is not a coefficient-array index. Source scopes narrower than the attack category must remain explicit in labels/text and consumer eligibility, never applied globally to that category. Enhanced-kit additions remain explicitly condition-labeled alongside their originating base effects, with both base and conditional enhanced descriptions preserved in `description`; do not invent variant/activation fields or make enhancements unconditional. A validated resultless utility may have `modifiers: []`. Constellation-driven talent-level and modifier stack-cap increments follow `character-constellation-changes`; fixed Energy, crafting chances, timers and activation remain source text/consumer usage, without new structured results. No automatic migration.

Rule `character-constellation-changes`: owner authorization on 2026-11-06 adds optional source-local `constellations[*].changes`, not `talentChanges`. Each entry identifies one exact, uniquely resolving same-locale `skillTalents[*].name`, explicitly selects exactly one changed property with literal `true`, and supplies a positive integer `add`. A talent-level increment uses `{ skillTalents, level: true, add }`, without a modifier reference. A stack-cap increment uses `{ skillTalents, modifiers, accumulations: true, add }`, with `modifiers` exactly matching one unique label in that talent's own modifiers. Omitted selectors, strings such as `level: 'Talents'`, false selectors, multiple selected properties, dangling/ambiguous references and empty arrays are invalid. References are localized with their targets; rename both together. Keep the base talent levels, coefficient arrays and modifier accumulations unchanged; the consumer applies each unlocked constellation change once to compute the effective level/cap, before selecting the existing source-level coefficient. Retain source limits (including maximum talent level15), activation and descriptions; do not pre-shift or duplicate arrays, encode unlock rank as coefficient level, treat charges as modifier accumulations, or add a runtime evaluator. Ayato Namisen retains base cap4, with C2 add1; C3 supplies a separate Skill level add3. This narrowly approves level/accumulations changes, not arbitrary property mutations or replacement of independent additive damage/stat contributions. The owner explicitly requires an individual retrospective review of previously completed Albedo, Alhaitham, Aloy, Amber, Arataki Itto, Arlecchino and Ayaka before resuming Ayato; record compatibility and complete coordinated per-entity follow-up commits for changed EN/PT records. Preserve unavailable Aloy constellations and all other locales; no unreviewed-character migration.

Rule `enemy-attack-reduction`: owner approval on 2026-10-26 adds `enemy_atk_reduction` for an individually validated reduction of an affected opponent's ATK. Store positive reduction ratios in `values`; omit `bonus`, attack `target` and `conditional`. Aloy's Frozen Wilds uses .12 at source levels 1–3, .13 at 4–6, .14 at 7–9 and .15 at 10–15, with hit eligibility and 6s duration in localized text. This is not a signed wearer `atk_percent`, enemy RES reduction, outgoing DMG bonus or an incoming-damage formula. No automatic migration.

Rule `enemy-defense-reduction`: owner approval on 2026-11-03 adds `enemy_def_reduction` for an individually validated reduction of affected opponents' DEF. Store positive reduction ratios in `values`; omit `bonus`, benefiting attack `target`, `conditional`, `element` and reaction. Ayaka C4 uses [.3] for opponents damaged by Soumetsu's Frostflake Seki no To, lasting6s, with trigger/affected opponents/duration in localized labels and descriptions. Burst damage activates the reduction; it does not limit the benefiting damage to Burst. This is not negative wearer `def_percent`, enemy RES reduction, DEF ignore or a direct outgoing/incoming damage multiplier. Consumer handles activation/expiry and the DEF formula; no automatic migration.

Rule `character-source-level-modifier-values`: the owner rejected a separate `talentLevels` field on 2026-10-26. For individually reviewed level-varying `skillTalents[*].modifiers`, `values[0]` corresponds to source talent level 1, `values[1]` to level 2, and so on, without gaps; any level-indexed `maxValues` follows the same positions. The index is the containing skill's effective level, not the benefiting attack talent's level. Single-value constants remain constants without backfill. Aloy's Skill-origin enemy ATK reduction and Normal DMG bonuses use this convention. Raw source levels do not certify ordinary attainability. Do not add `talentLevels`, reinterpret passive/constellation unlock levels as indices, or change weapon refinement indexing.

Rule `attribute-referenced-summon-hp`: for Amber's individually reviewed Baron Bunny HP, the owner directed reuse of `hp_percent` with `conditional: { origin: 'character', attribute: 'hp' }` on 2026-10-26, rather than a new `summon_hp` type. Place the modifier on the originating Skill, supply raw level-varying ratios under `character-source-level-modifier-values`, and explicitly identify the summoned recipient and Amber's Max HP reference in localized labels. Omit `bonus` and attack `target`: this supplies the summon HP result, not an increase to Amber's HP, healing, shield strength or damage. The talent's inherited-HP row remains a display table, without fabricated damage components. Existing wearer HP bonuses and independent HP-based damage retain their distinct containers/meaning; no automatic migration or summon-state engine.

Rule `character-separated-talent-components`: owner approval on 2026-10-23 represents distinct unequal subhits as separate named `upgrades`, preserving individual raw coefficients at each source level under `character-talent-attribute-components`. Albedo's Charged Attack becomes Hit 1 (.473 at level 1) and Hit 2 (.602), both Charged/Physical/ATK. Mutually exclusive Low/High Plunge results likewise use separate explicitly named upgrades (1.278377 / 1.596762 at level 1), but the consumer selects only one impact variant. Do not pre-sum, use identical-hit `hits` for unequal coefficients, treat the second percentage as fixed `add`, or emit combined-row duplicates. Non-damage resource/duration/cooldown rows keep localized `power[].value`/`level` without fake damage type/components. This does not permit splitting multi-attribute contributions to one result into separate hits.

Rule `character-talent-attribute-components`: owner approval on 2026-10-23 adopts level-local `power[].components` for every reviewed character talent damage row, including single-attribute rows. Each nonempty component carries finite scalar `scalings` in ratio units plus explicit existing attribute `conditional`; preserve every raw source term separately. One component denotes one attribute contribution, not another hit: the consumer sums the qualifying components (and a source-validated fixed `add`, if present) for one hit before applying shared hit mechanics once. Identical composite hits may use upgrade-level `hits` with one-hit coefficients; mutually exclusive variants remain distinct named upgrades. Do not retain a duplicate `power[].scalings`, infer a shared source from matching numbers, or use upgrade-level `type` as a competing scaling-attribute reference; preserve only independently meaningful result types where approved. Plain damage rows need damageType/target plus explicit component references, not an ATK/DEF type tag. Non-damage display-only rows omit components. The owner explicitly extends this alignment to already reviewed characters: Albedo is the sole prior processed character and receives one-component ATK/DEF rows in EN/PT on both maintenance branches, preserving coefficients and unrelated data. Alhaitham uses ATK+EM components on each composite hit. Existing `Modifier.values` and `IndependentSkill.scalings[]` contracts are unchanged; this does not authorize a bulk character migration or reclassify independent skills.

Rule `character-grouped-healing-components`: owner approval on 2026-10-28 extends the existing level-local `power[].components` contract to individually reviewed talent healing with upgrade `type: healing`. Sum the component contributions into one recovery; omit damageType, benefiting attack target, hits and bonus on that row. Arlecchino's Burst keeps raw 1.5 × Max HP × normalized current Bond and 1.5 × ATK as two components, using a reference product only for the first. This is raw healing before Bond absorption, not two independent heals or effective .5-Bond recovery; preserve absorption/restrictions in localized text for consumer handling. Damage components, modifier values, independent skills, fixed recovery and display-only utility rows retain their existing contracts. No unreviewed healing migration or general grouped-result approval.

Rule `character-grouped-shield-absorption-components`: owner approval on 2026-11-10 adds talent-row `type: shield_absorption` with level-local `power[].components`, explicitly following Alhaitham's additive component structure. For this result, an attribute-referenced component uses raw scalar `scalings` and its reference; a fixed component uses `scalings` in direct absorption units and omits `conditional`. Sum the separate terms once. Baizhu's Burst level1 uses HP .008 plus fixed77.03752 as two components, not `add`, damage or healing. Omit damageType, target, hits and bonus; preserve Dendro250% effectiveness, shield creation/refresh/breakage/duration in localized text and consumer handling. Missing conditional is permitted only for a fixed shield component; existing damage/healing components still require their explicit references. Existing `add` contracts, Shield Strength and The Bell's entity-specific text-only exception remain unchanged. No automatic migration or runtime shield engine.

Rule `character-source-local-shield-absorption`: owner approval on 2026-11-10 extends `shield_absorption` to an individually validated source-local character modifier for a single-attribute shield capacity. Beidou C1 uses `constellations[0].modifiers`, values[.16], conditional character.hp (Beidou Max HP), without bonus, target, element, hits or accumulations. This is capacity, not Shield Strength, HP bonus, healing or damage. Shield recipient/creation/duration and element-specific effectiveness remain localized text and consumer state. Grouped talent shield rows continue using separate components; this approval adds no grouped modifier or constellation-upgrade schema, does not move C1 into the base Skill, and does not override The Bell's entity-specific text-only exception or authorize unrelated migration.

Rule `character-incoming-damage-reduction`: owner approval on 2026-11-10 adds `dmg_reduction` for an individually validated reduction of incoming damage. Use positive ratio `values`, without bonus, benefiting attack target, conditional, element or reaction. Beidou's originating Burst modifier uses contiguous effective Burst levels1–15 under `character-source-level-modifier-values`: [.2,.21,.22,.24,.25,.26,.28,.3,.32,.34,.35,.36,.37,.38,.39]. Preserve its protected recipient and15s duration in localized labels/descriptions; the talent table remains display-only for this result. It is not outgoing dmg_percent, DEF/RES, shield capacity, enemy damage, or an independently certified rule for combining multiple reductions. Consumer handles activation and the incoming-damage formula; do not reinterpret Serpent Spine's entity-specific text-only incoming increase, emit a negative outgoing bonus, add an incoming-damage engine or migrate other entities automatically.

Rule `character-talent-healing-reference`: owner approval on 2026-11-10 permits a healing modifier's single `conditional` object `{ origin: 'skillTalents', skillTalents: exactLocalizedTalentName, attribute: 'healing' }`. It must resolve uniquely to one same-locale talent and exactly one `upgrades` row with `type: healing`. Use that row's raw grouped recovery, including any fixed term, at its effective source talent level (including unlocked `changes`); multiply once by the modifier's `values` coefficient, then apply applicable healing bonuses once. Baizhu C2 supplies source-local constellation healing `values: [.2]`, without bonus, target, duplicated coefficient arrays or `talentLevels`. Do not reference post-bonus/effective healing, use constellation unlock level as talent level, permit recursive/cyclic references or mix talent-result references into attribute-product arrays. Existing character/weapon/artifact/time references keep their original meanings; this narrowly approves a healing-result dependency, not arbitrary talent parameter references or a runtime evaluator.

Rule `character-source-local-independent-skills`: owner approval on 2026-10-23 places an individually validated independent character proc in optional `skills` beside `modifiers` on the originating `skillTalents`, `passiveTalents`, or `constellations` item, not duplicated at the root. Use labeled `IndependentSkill`, explicit attribute references and source-validated `damageType`; optional `target` identifies the proc's benefiting attack classification, not the triggering attack. Albedo's enhanced C2 uses `constellations[1].skills[0]`, Geo, Burst target, DEF-referenced `def_percent` [3] for one Fatal Blossom. The owner separately and explicitly confirmed the 300%-DEF-per-flower denominator after the source-wording limitation was presented; do not claim independent source certification of that unit. Three flowers per qualifying proc and A4 activation remain localized source text/consumer handling, without pre-summed totals or duplicate attached modifiers. Optional target omission preserves existing weapon/artifact independent-skill meaning; no automatic target backfill or container migration.

## Semantic Changes and History

A new/revised interpretation needs a stable rule key here and a read-only compatibility review of actually relevant prior live records. Record each reviewed path as compatible/not-applicable/correction-required/unresolved in `.opencode/history/rule-decisions.md`. Do not edit other entities without scope authorization or claim global consistency while follow-ups remain.

For an entity entry, retain only: date/ID/status, reviewed scope and resulting paths, applied rule keys/essential values, source URLs or source bundle location, compatibility result, checks, blocker/next authorized action. Put artifact records in `entity-records-artifacts.md`, weapon records in `entity-records-weapons.md`, character records in `entity-records-characters.md`, and closed owner/release/archive decisions in `closed-decisions.md`.
