# Entity Calculation Workflow

## Scope and Authority

This is the active rulebook for calculation data. The 2026-10-13 bulk migration was retired and the committed baseline restored on 2026-10-14; never restore or extend its output in bulk. Work one explicitly selected entity at a time. Existing fields and historical examples do not prove coverage.

Read `application-state.md`, `data-conventions.md`, `entity-research.md`, this file, and the named entity/rule entry in `.opencode/history/` before calculation work. History is evidence, not instructions.

## Per-Entity Process

1. **Select:** record exact local ID, requested scope, baseline, patch and source items. Authorized rounds are sequential alphabetical artifacts (max 5) or weapons (max 20); stop at the limit, unresolved source conflict under `entity-research.md`, or unapproved semantic/schema decision.
2. **Inventory and research:** classify every source clause (stat/result, proc, hit, fixed addition, reference, stack/tier, consumer usage, unsupported); identify JSON paths. Validate identity and numbers under `entity-research.md`; retain URLs/access date/revision/raw responses outside served data.
3. **Map:** reuse only an approved contract below. Map levels/ranks, references, targets, elements, caps and independent components explicitly. Do not infer a type from matching wording or numbers.
4. **Apply:** change only selected EN/PT records; preserve other locales and unrelated fields. Validated EN/PT display alignment is standing-authorized; resolve source differences under `entity-research.md` before applying.
5. **Validate:** JSON; raw coefficients; levels/ranks; types/metadata; hit/add/cap semantics; EN/PT parity; focused diff/format; `pnpm run build` and `pnpm run lint` with Node 20/pnpm. Report failures or omissions; never repair unrelated baseline failures.
6. **Record and deliver:** append the concise outcome to the appropriate history file, then commit on `agent` as `<id> - <what was done>`. For a bounded round, independently mirror only that entity's served JSON to the configured delivery branch before selecting the next item. Follow `git-workflow.md`; do not transfer `.opencode/`, push, or start a further round without authorization.

States: `not-started`, `in-progress`, `partial`, `blocked`, `complete`, `reviewed-not-applicable`; `archived-reset` is historical only. “Complete” always names reviewed scope; consumer usage alone does not leave multiplier coverage pending.

## Universal Rules

- Percentages are ratios (`20%` → `0.2`); direct units remain direct. Preserve raw precision.
- Keep each source contribution distinct; never deduplicate equal values or pre-sum base/additional clauses.
- `type` describes the supplied stat/result, not its scaling attribute. Keep provider metadata and sources out of served JSON.
- A stat/result modifier is `effects[].modifiers[]`; an independent proc uses its native `skills` container. Artifact procs nest at their originating `effects[].skills`; character/weapon native containers remain unless individually approved.
- `bonus: true` marks a supplied additive stat/bonus contribution; omit `bonus: false`. Signed ATK/DMG penalties follow `signed-additive-atk-penalty` / `signed-additive-dmg-penalty` below. An independent hit normally omits it.
- Supported attack targets are `normal_attack`, `charged_attack`, `plunging_attack`, `elemental_skill`, `elemental_burst`. Generic bonuses omit target. `elemental_dmg_percent` requires `element`; all-element grants expand to GEO/ANEMO/CRYO/DENDRO/ELECTRO/HYDRO/PYRO, never Physical.
- `conditional` is `{ origin, attribute, offset? }`; origins: `character`, `weapon`, `artifact`, `time`; attributes: `hp`, `atk`, `def`, `elemental_mastery`, `energy_recharge`, `energy_cost`, `cleared_bond_of_life`. Optional `offset` follows `attribute-reference-baseline-offset` below. `time.energy_cost` is exclusively combined party maximum Burst Energy Cost; `character.cleared_bond_of_life` follows `cleared-bond-of-life-reference`. Coefficients stay in `values`.
- Activation, timing, resource use, chance, stacks currently active, party composition, target state, infusions and formula execution are consumer state. Preserve them in localized text.
- `hits` is a positive count of identical subhits; store one-hit value. `add` is a separate fixed value in talent `power`; never use either for stacks. Do not split multi-attribute one-result mechanics; grouped schema is deferred.

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

Rule `serpent-spine-consumer-only-incoming-damage`: the owner approved mapping Serpent Spine's outgoing `dmg_percent` [.06,.07,.08,.09,.1], `bonus: true`, `accumulations: 5`, without target/conditional, while retaining its wearer incoming-damage increase [.03,.027,.024,.022,.02] only in localized `passiveDesc` for consumer handling. Current shared count, on-field gains, off-field retention and one-stack loss remain consumer state. Do not encode incoming damage as outgoing bonus/penalty, DEF/RES reduction or a new result type. Completion names outgoing coefficient coverage plus the explicitly text-only incoming component; this entity-specific owner resolution does not certify a structured incoming-damage schema or authorize omissions on unrelated entities.

Rule `the-bell-consumer-only-shield-absorption`: the owner approved retaining The Bell's shield generation/absorption [.20,.23,.26,.29,.32] of wearer Max HP only in localized `passiveDesc`, while structuring its outgoing `dmg_percent` [.12,.15,.18,.21,.24], `bonus: true`, without target/conditional/accumulations. The damage bonus qualifies under any shield, not only the generated shield. Do not add `shield_absorption`, substitute shield strength/HP/healing/incoming reduction, or claim structured shield-capacity coverage. Shield duration, cooldown, remaining capacity/breakage and qualification remain consumer handling. This is an entity-specific text-only completion exception, not a general omission policy or new result schema.

Rule `tulaytullah-separated-increments-consumer-shared-cap`: the owner approved separate Normal-targeted `dmg_percent` bonuses for Tulaytullah's Remembrance: per-second [.048,.060,.072,.084,.096] with `accumulations: 10`, and per-qualifying-hit [.096,.120,.144,.168,.192] without accumulations. Keep its separate Normal `atk_speed_percent` [.10,.125,.15,.175,.20] unstacked. All use `bonus: true`, without conditional/maxValues/skills. The shared total DMG cap [.48,.60,.72,.84,.96] remains explicitly in localized passiveDesc and consumer handling, not independently per modifier or certified by accumulations10. Hit-history/ramp aggregation, 14s duration, .3s qualification, off-field removal and recast reset remain consumer state. Do not normalize the two source clauses into one equivalent-step modifier, add a cap-group schema or treat the hit increment as an unconditional permanent grant. This entity-specific owner-selected representation preserves raw distinct coefficients; it does not certify a structured shared-cap engine or authorize migrations/general omissions.

Under `party-member-counted-bonus`, the source determines whether the holder is eligible; inclusion is not universal. Chain Breaker counts a Natlan member OR a member whose Elemental Type differs from the wearer, counting each once, including a Natlan wearer. Its per-member ATK uses `accumulations: 4`; the separate EM grant requires at least three eligible members and is supplied once, without `accumulations`. Actual eligible-member count and party selection remain consumer state in localized labels/descriptions, not `conditional`. Gilded Dreams and A Thousand Floating Dreams still exclude the holder and retain their validated cap-3 constraints. This source-specific extension does not authorize automatic edits to other entities.

### Attribute references and independent skills

Rule `unqualified-hp-reference-default`: the owner's standing 2026-10-20 instruction treats an unqualified HP reference in a percentage recovery or HP-based scaling coefficient as Max HP unless the source explicitly specifies another reference. Reuse `character.hp` for the appropriate source-defined character; do not ask per entity solely because a validated clause says only HP. Explicit current/remaining/lost HP or other stated references take precedence. This default does not infer a recipient, convert HP-state thresholds into scaling, alter fixed-HP recovery, or reinterpret ATK/DEF references. Recurve Bow uses wearer-Max-HP `healing` [.08,.1,.12,.14,.16], without bonus/hits/accumulations. Record the default as owner-directed semantics rather than claiming the omitted denominator was independently explicit. Prior wearer/recipient Max HP mappings remain unchanged; no bulk migration.

Rule `recipient-max-hp-healing-context`: for individually reviewed recipient-Max-HP healing, the owner-approved Prototype Amber representation reuses `healing` with `conditional: character.hp` and explicit localized recipient-Max-HP labels. The consumer supplies each healed recipient's Max HP for that modifier, not the wearer's HP. Store per-tick R1–R5 [.04,.045,.05,.055,.06], without `bonus`, `hits`, `accumulations` or aggregated duration totals. Recipient selection, interval/duration and fixed Energy restoration remain consumer text; no recipient selector/origin or runtime engine is added. This context is explicit and entity-specific; other existing wearer HP references retain their meaning and are not migrated automatically.

Rule `attribute-reference-baseline-offset`: an individually source-validated above-baseline reference may supply optional finite nonnegative `conditional.offset`, in the referenced attribute's native units. The consumer uses `max(attributeValue - offset, 0)` before multiplying by `values`; `maxValues` retains its existing result-cap meaning. Omission preserves the original unmodified reference; never infer a baseline globally. Engulfing Lightning uses offset `1` for ER above 100% (ratio units), ATK coefficients [.28,.35,.42,.49,.56] and result caps [.8,.9,1,1.1,1.2]. Emblem of Severed Fate continues using full total ER without an offset. This supplies static inputs, not runtime formulas or automatic migrations.

Rule `cleared-bond-of-life-reference`: an individually source-validated modifier may reference `{ origin: 'character', attribute: 'cleared_bond_of_life' }`, the total Bond of Life cleared for the qualifying removal, in HP units supplied by the consumer. It is not Max HP, current Bond remaining, or arbitrary total healing. Keep raw coefficients in `values` and cap the supplied result with same-rank `maxValues`; removal tracking/qualification, calculation timing and buff duration remain consumer state. Finale of the Deep supplies `flat_atk` [.024,.03,.036,.042,.048], result caps [150,187.5,225,262.5,300], separately from its initial ATK-percent grant. Do not assume the cleared value is 25% of Max HP, add offset/stack metadata, alter prior Bond-state grants or implement a resource engine.

Rule `attribute-referenced-independent-scaling`: an individually reviewed independent skill's attribute-referenced scaling carries `conditional` beside `type`/`values`. Use the source's reference, not activation state. Ash-Graven retains `hp_percent`, R1–R5 .4–.8, with `character.hp` (Max HP). Do not infer the reference from the type alone or migrate other entities automatically.

Rule `independent-triggered-weapon-damage`: a source-validated or explicitly owner-confirmed separate weapon proc uses native `calculation.skills`, even when Normal/Charged attacks trigger it. Keep its attribute reference explicit; trigger categories remain consumer text, not duplicated targeted modifiers. Crescent Pike's owner-approved Physical ATK-referenced proc is independent, unlike Echoes of an Offering's attached damage. Halberd's independent classification is owner-confirmed; use ATK [1.6,2,2.4,2.8,3.2] with `character.atk`, without inferring its damage type. This does not reclassify other weapons or restore archived mappings.

Messenger is owner-confirmed attached weak-point Charged damage, not an independent skill: use root targeted `dmg_percent`, `bonus: true`, ATK ratios [1,1.25,1.5,1.75,2] and `conditional: character.atk`. Weak-point/critical qualification and interval remain localized text/consumer handling; do not create `calculation.skills`, a CRIT DMG stat grant, a forced-CRIT state field, or a pre-applied CRIT multiplier. This reuses the existing targeted attribute-referenced bonus shape and does not reclassify Halberd or Crescent Pike.

Scion of the Blazing Sun's owner-selected split uses an independent native Sunfire Arrow skill, ATK [.6,.75,.9,1.05,1.2] with `character.atk`, and a separate Heartsearer root `dmg_percent` modifier, Charged target, [.28,.35,.42,.49,.56], `bonus: true`, no conditional/stacks. Affected-opponent/wearer-only qualification remains localized text/consumer handling. Do not introduce `dmg_taken_percent`, reinterpret all enemy "take more damage" wording globally, or combine the Arrow with the damage bonus. This is an entity-specific owner resolution, not a new vulnerability formula or damage-type certification.

Rule `required-independent-skill-label`: every `IndependentSkill` requires a nonempty localized `label`, including single-output skills and artifact-nested skills. Reuse established localized passive/proc names where applicable. This does not add labels to separate character talent-table schemas that already use names.

Rule `independent-proc-alternative-totals`: source-validated mutually exclusive proc totals use separate labeled native skills; each label identifies eligibility and replacement. Choose exactly one applicable variant per proc/target, never sum alternatives or multiply by trigger chance. Dragonspine Spear uses ordinary ATK [.8,.95,1.1,1.25,1.4] versus Cryo-affected replacement [2,2.4,2.8,3.2,3.6], with `character.atk`; timing/chance/target state remain consumer text, not attribute references or new state fields. Do not infer damage type from an icicle name or weapon substat.

### Reactions

`reaction` is required for reaction-scoped `dmg_percent` and approved reaction-scoped `crit_dmg` under `lunar-reaction-scoped-crit-damage` or `stellar-glimmer-scoped-crit-damage`; omit it on other results and on ordinary/global CRIT grants. Valid values:
`SWIRL`, `STELLAR_SWIRL`, `OVERLOADED`, `ELECTRO_CHARGED`, `SUPERCONDUCT`, `BURNING`, `BURGEON`, `BLOOM`, `HYPERBLOOM`, `LUNAR_BLOOM`, `AGGRAVATE`, `LUNAR_CHARGED`, `LUNAR_CRYSTALLIZE`, `STELLAR_CONDUCT`, `STELLAR_GLIMMER`, `VAPORIZE`, `MELT`.

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
  | 'elemental_res_percent'
  | 'physical_dmg_percent'
  | 'dmg_percent'
  | 'def_percent'
  | 'crit_rate'
  | 'crit_dmg'
  | 'energy_recharge'
  | 'healing_bonus'
  | 'shield_strength_percent'
  | 'healing'
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
  | 'LUNAR_CHARGED'
  | 'LUNAR_CRYSTALLIZE'
  | 'STELLAR_CONDUCT'
  | 'STELLAR_GLIMMER'
  | 'VAPORIZE'
  | 'MELT';
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
  conditional?: {
    origin: 'character' | 'weapon' | 'artifact' | 'time';
    attribute:
      | 'hp'
      | 'atk'
      | 'def'
      | 'elemental_mastery'
      | 'energy_recharge'
      | 'energy_cost'
      | 'cleared_bond_of_life';
    offset?: number;
  };
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
  scalings: Array<{
    type: EffectType;
    values: number[];
    maxValues?: number[];
    conditional?: Modifier['conditional'];
  }>;
};
type ScalarUpgrade = {
  name: string;
  type: EffectType;
  damageType?: Vision;
  hits?: number;
  bonus?: true;
  accumulations?: number;
  target?: AttackTarget;
  power: Array<{
    level: number;
    value: string;
    scalings: number;
    add?: number;
  }>;
};
```

Entity progression belongs to top-level `calculation`; talent tables use `skillTalents[*].upgrades[*].power[*]`; character passive/constellation uses native `calculation.effects/skills`; weapon passive modifiers use top-level `effects`; weapon independent procs remain `calculation.skills`; artifact effects use top-level `effects`. `refinementLevels` maps array positions; talent `scalings` is scalar. Omit unknown proc damage type.

## Semantic Changes and History

A new/revised interpretation needs a stable rule key here and a read-only compatibility review of actually relevant prior live records. Record each reviewed path as compatible/not-applicable/correction-required/unresolved in `.opencode/history/rule-decisions.md`. Do not edit other entities without scope authorization or claim global consistency while follow-ups remain.

For an entity entry, retain only: date/ID/status, reviewed scope and resulting paths, applied rule keys/essential values, source URLs or source bundle location, compatibility result, checks, blocker/next authorized action. Put artifact records in `entity-records-artifacts.md`, weapon records in `entity-records-weapons.md`, and closed owner/release/archive decisions in `closed-decisions.md`.
