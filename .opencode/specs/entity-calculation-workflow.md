# Entity Calculation Workflow

## Authority and Restart Point

This is the single active calculation-data plan and rulebook. It consolidates the former calculation gaps, progression sources, structured effects plan and talent review notes. Approved rules below apply by mechanical meaning across every entity, not exclusively to talents, characters, weapons or artifacts. Container differences do not create different mathematical rules.

The owner retired the bulk migration on 2026-10-13. On 2026-10-14 all pending entity changes were restored to the last committed baseline, superseding the earlier removal of committed progression/effect data. Previously committed fields remain; uncommitted bulk results do not. Start again with one explicitly selected entity. Historical examples below are design references, not proof of current field presence or full entity certification; inspect the selected record.

Read `application-state.md`, `data-conventions.md`, `entity-research.md` and this file before work. For earlier attempts, consult `.opencode/history/entity-work-history.md`; its snapshot and old blocker report are history, not automatically loaded instructions or a current completion list. Never restore an old bulk result wholesale.

## Work in Stages, One Entity at a Time

1. **Select and inspect:** record the exact entity ID, source items, intended patch, current Git baseline and requested scope. Do not expand to other entities without permission. On 2026-10-14 the owner authorized consecutive artifact review in local-ID alphabetical order, one entity at a time, up to five entities per requested round; the current entity counts toward that limit. Stop earlier for a source conflict or schema/semantic decision requiring owner evaluation. This is bounded sequential maintenance, not a bulk migration or permission to add unsupported mechanics.
2. **Inventory:** list each source coefficient and classify it as progression, stat/result modifier, independent proc, attached damage, repeated hits, fixed addition, mixed element, grouped attributes, consumer usage or unsupported result. Name the affected paths before editing.
3. **Research:** verify identity and corresponding numerical rows/refinements using the entity research policy. Save source URLs, access dates, source revision/patch and raw responses outside served data. A JSON response does not mean every mechanic is normalized.
4. **Map and decide:** map names, categories, component identity, talent levels and refinement ranks explicitly. Reuse approved rules; when a text interpretation establishes or changes a general rule, perform the retrospective compatibility review below before calling the rule settled. Stop unsupported layouts or factual conflicts instead of inventing types or using coincidental coefficients.
5. **Apply:** edit only the selected entity's English and Portuguese records. Preserve profile fields, progression outside the requested scope and every other locale. On 2026-10-14 the owner gave standing authorization to align EN/PT display text with the corresponding validated source clauses/templates without asking separately for each correction; numerical identity/scope still follow the research policy, and accessible source conflicts must not be hidden by selecting arbitrary text. Record exact display changes and preserve other fields; missing display levels may be filled from validated localized source templates. Do not relabel or delete old rows to make an audit pass.
6. **Validate:** check JSON, all source levels/ranks, raw unrounded coefficients, supported types, required element metadata, hit counts, additions and bilingual mechanical parity. Inspect the diff for unrelated changes. Run focused formatting, `pnpm run build` and `pnpm run lint` with Node.js 20/pnpm; disclose failures or omitted checks. Do not format unrelated files to conceal baseline failures.
7. **Record, commit individually, advance within the limit and stop:** append the entity's scope, changes, examples, sources, decisions, checks and remaining blockers to the history. Mark complete only for explicitly reviewed coverage. On 2026-10-14 the owner restored the commit-before-advance requirement and gave standing authorization to commit source-validated work without separate per-commit approval in requested bounded rounds. Fully research/map/apply/validate/record and commit one entity with `<id> - <what was done>` (exact local ID, English description) before selecting the next. Preserve per-entity commit scope/directly affected specs/history and approved governance changes, following `git-workflow.md`. Stop at five entities or earlier for source conflicts or unapproved schema/semantic decisions, then report outcomes; do not commit unresolved invented mappings. Outside the requested round do not select another automatically. A commit does not certify unresolved coverage or authorize a push or another round. Do not run a bulk migration.

### Completion States

Use `not-started`, `in-progress`, `partial`, `blocked`, `complete` and `reviewed-not-applicable`. Historical reset entries are `archived-reset`, never current completion claims. A character cannot be declared entirely complete from core talents alone; passives and constellations require their own inventory. An empty modifier array means reviewed non-applicability, not an unreviewed source. Usage mechanics alone do not keep multiplier coverage pending.

### New Text Rules and Retrospective Compatibility

Whenever resolving a specific source text establishes or revises how a mechanical idea is represented, treat that decision as a general semantic rule, not an exception limited to the current entity or entity type. Apply the following process:

1. **Define the rule:** record a stable descriptive rule key, the source text/example, its mechanical meaning, intended representation, applicability limits and relevant counterexamples in this rulebook. Match the same idea across wording and language differences; shared words or equal coefficients alone do not prove equivalent mechanics.
2. **Find prior occurrences:** consult the entity work history and inspect previously worked entities for that idea across talents, passives, constellations, weapons and artifact effects. Include relevant partial/blocked work and earlier examples, not only completed entities. Archived reset records are candidate evidence only; inspect the current live records before judging compatibility or proposing a correction.
3. **Compare semantics and structure:** for each applicable occurrence, check the attribute/result reference, coefficient units, additions/hits, bonus/proc distinction, targets, elements, level/refinement mapping and English/Portuguese parity as relevant. Preserve intentional native-container differences; structural cohesion means equivalent semantics use the same approved contract, not identical JSON nesting for all entity types.
4. **Record the outcome:** link the rule key to the reviewed entity IDs and exact source-item/JSON paths in the history. Record each finding as compatible, not applicable, correction required or unresolved, with reasons and references. Separate entities actually reviewed from candidates still pending; never claim retrospective compatibility from a search alone.
5. **Resolve inconsistencies deliberately:** keep compatible records unchanged; explain legitimate differences instead of adding entity-specific exceptions. List incompatible records as individually scoped follow-ups. Read-only retrospective inspection is required even while working on one entity, but edits outside the selected entity still require an explicit scope decision and must be performed entity by entity, never by an unrequested bulk migration. Breaking changes, source conflicts or unsupported layouts remain blocked pending a decision.

A text rule is not settled merely because it works for the first example. Record the rule as pending compatibility review until the previously worked, applicable occurrences have been checked; track unresolved follow-ups explicitly and do not claim global structural consistency while they remain. If no prior occurrence is applicable, record the reviewed scope and that finding rather than inventing a migration. Repeat this review whenever an existing semantic rule changes. This governance rule itself adds no new mechanical interpretation and does not authorize modifying entity data now.

## General Mechanical Rules

### Units, Identity and Precision

- Normalize percentages to ratios: `20% -> 0.2`, `200% -> 2`. Direct flat values remain direct values.
- Keep numerical precision from the source. Rounding is presentation only; do not reverse-engineer an internal coefficient from rounded text when the raw parameter is available.
- `type` identifies a stat/result. An ATK-scaled proc does not grant ATK. A damage-reference ratio is not automatically an ATK coefficient.
- Keep separate source contributions separate, including clauses with equal coefficients. Deduplicating equal numbers or pre-summing base and additional bonuses destroys source coverage.
- Never import a coefficient just because that number occurs somewhere else on a source page.
- Keep provider IDs, cache metadata and source references out of served entity JSON. API entity IDs remain the local directory IDs.

### Modifier Versus Independent Proc

- A supplied stat/result modifier belongs to `effects[].modifiers[]`; an independent damage proc belongs to `skills[]`.
- Attached attack additions follow the approved targeted-modifier convention, not the independent-proc convention. Preserve their stat reference with `conditional`.
- `bonus: true` marks a stat/result bonus on a modifier or talent upgrade. Omit it on ordinary hits and independent procs; do not emit `bonus: false` placeholders.
- An attack-scoped damage modifier requires its supported `target`: `normal_attack`, `charged_attack`, `plunging_attack`, `elemental_skill`, or `elemental_burst`. Genuinely generic damage bonuses omit it; approved reaction-scoped bonuses use `reaction` instead. Unsupported reaction/scoped-CRIT results must not be disguised as generic damage or global CRIT. Reaction damage and target-state CRIT follow the explicitly approved rules below; other unsupported scopes remain blocked.
- `elemental_dmg_percent` requires `element`. Expand an all-element bonus into seven modifiers: GEO, ANEMO, CRYO, DENDRO, ELECTRO, HYDRO, PYRO; exclude physical damage.
- `maxValues` is an optional same-rank numeric cap. Do not pre-apply consumer inputs to stored coefficients.

#### Rule `reaction-selected-elemental-bonus`

A bonus for the element selected by a reaction is not an all-element bonus. Represent the source-validated eligible elements as separate `elemental_dmg_percent` modifiers with explicit `element`, the same source ratio and localized labels identifying the branch. These are activation alternatives, not simultaneous grants: the consumer selects the applicable branch using the preserved description and source mechanic. Do not add reaction state to `conditional`, which is only an attribute reference. No new target or selector field is introduced.

Archaic Petra's ordinary Crystallize branch has four eligible elements (CRYO, ELECTRO, HYDRO, PYRO), each with `values: [0.35]` and `bonus: true`; at most one 4-piece branch is active. Its separate 2-piece Geo bonus remains `0.15`. Do not expand the ordinary branch to Geo/Anemo/Dendro/physical or treat the four variants as four independent source contributions. This rule does not resolve the newer Lunar-Crystallize clause: its affected-element mapping and the differing source descriptions remain pending, so Archaic Petra is partial, not globally complete.

Retrospective review found no applicable earlier worked selector: Adventurer's flat HP and healing effects are not elemental selectors. Inspected live Calamity Queller's all-element passive and Faruzan's Anemo-only talent row; both are counterexamples, not candidates for this mutually exclusive expansion. The all-element seven-modifier rule remains unchanged. Exact paths, sources and limits are recorded in history; no other entity was edited.

#### Rule `target-state-crit-rate`

Owner-approved on 2026-10-14: CRIT Rate bonuses activated by the attacked enemy's state may use separate `crit_rate` modifiers, `bonus: true`, and localized labels explicitly naming each condition. Preserve the original description; the consumer evaluates target state for the eligible hit instead of adding these bonuses unconditionally to the character's global CRIT Rate. Enemy state is not an attribute reference, so do not encode it in `conditional` or invent `target` values. No new runtime or machine-readable predicate schema is introduced.

Blizzard Strayer's 4-piece bonus has two contributions: `values: [0.2]` against Cryo-affected enemies, and a separate additional `values: [0.2]` against Frozen enemies. Never deduplicate the equal coefficients or pre-sum them into a single `0.4`. Example with 30% base CRIT Rate: 30% against an unaffected enemy, 50% with only the Cryo condition, 70% when both conditions apply. This does not claim every Cryo/Frozen transition meets both conditions; source-specific timing and actual target eligibility remain consumer usage. Do not use the `element: "CRYO"` damage-bonus metadata to imply that only Cryo damage can benefit from these CRIT modifiers.

Limits and retrospective review: this approves target-state activation only, not an arbitrary attack-category CRIT scope or a reaction-only CRIT formula. Berserker's live EN/PT 4-piece modifier is compatible with the existing activation contract but is a counterexample to target-state scope: its condition is the holder's HP, not the attacked enemy. No prior worked target-state CRIT modifier was recorded; no other entity was edited or globally certified. Paths and source evidence are recorded in history.

#### Rule `reaction-scoped-damage-bonus`

Owner-approved on 2026-10-14: use `type: "dmg_percent"` with a nonempty `reaction: Reaction[]` and `bonus: true` for a supplied reaction-damage bonus. One modifier represents the shared multiplier of a source contribution and lists every reaction to which that contribution applies. Crimson Witch of Flames uses `["OVERLOADED", "BURNING", "BURGEON"]` with `values: [0.4]`, and `["VAPORIZE", "MELT"]` with `values: [0.15]`. Do not split either common multiplier into one modifier per reaction, treat the list as simultaneous damage events, or apply it as generic/elemental damage. Independent source contributions remain separate even when their coefficients match; this is not permission to deduplicate bonuses with different activation conditions.

`reaction` is optional elsewhere but required for reaction-scoped `dmg_percent`; omit it on generic or attack-scoped damage, not `reaction: []`. Use the canonical locale-independent enum below, without duplicates or provider IDs. The initial five values were extended by owner approval on 2026-10-14 with BLOOM/HYPERBLOOM/LUNAR_BLOOM after source validation for Flower of Paradise Lost; any further extension requires a deliberate source-backed contract. Reaction-formula placement, ownership, activation, base multipliers and EM terms remain consumer work: stored bonuses are not final reaction multipliers or a complete reaction engine. Do not add an attack `target`, damage `element` or attribute-reference `conditional` to these scalar bonuses without separately validating that combined scope.

Retrospective live EN/PT review: Brave Heart's generic damage and Bloodstained Chivalry's Charged Attack bonus correctly omit `reaction`. Archaic Petra grants an elemental bonus selected by a reaction, not a bonus to the reaction's damage; its `elemental_dmg_percent` alternatives remain unchanged, including the existing Lunar-Crystallize blocker. No earlier worked reaction-damage modifier was found in history; no other entity was edited. Paths and source evidence are recorded in history.

Flower of Paradise Lost's approved extension preserves one modifier per shared source multiplier: BLOOM/HYPERBLOOM/BURGEON share base `[0.4]` and a separate `[0.1]` per stack; LUNAR_BLOOM has base `[0.1]` and separate `[0.025]` per stack. The two additional modifiers carry `accumulations: 4`; they share source activation/count but have unequal coefficients and must not be merged into `[0.025]` for all reactions. Retrospective live EN/PT Crimson Witch reaction groups/per-stack Pyro bonus remain compatible and unchanged. Archaic Petra's reaction-selected damage and Emblem's ER conversion are counterexamples, not candidates for the new enum values. No new Lunar-Crystallize mapping or reaction engine is approved; exact paths/results are in history.

#### Rule `fixed-source-bonus-increase`

When a source increases a fixed, source-defined bonus by a ratio of its starting value, store the separate absolute bonus contribution per activation unit, validated against that source expression. Crimson Witch of Flames gives `0.15 * 0.5 = 0.075` additional Pyro damage bonus per stack, distinct from its base `0.15`. This is normalization of fixed source constants, not pre-application of a character stat or a guessed stack count. Keep the original 50%-of-starting-value expression in the description and record its derivation in history. Do not store `0.5` as 50 percentage points of Pyro bonus, pre-apply three stacks, combine the base/additional modifiers, or use a stack count as `hits`/refinement ranks. The validated maximum is represented by `accumulations` under the rule below; actual stack activation/count and duration remain consumer usage. This rule does not approve fixed normalization when the referenced base is a consumer-dependent stat/result.

Retrospective review found no previously worked fixed-base relative-bonus increase. Live Archaic Petra's separate base/activated elemental bonuses are not relative increases; they remain compatible with the separate-contribution contract. The archived Berserker partial-coverage lesson also describes an additional flat CRIT percentage, not an amplification of its starting bonus. No other entity was edited or globally certified.

Flower of Paradise Lost also applies this contract: each stack grants 25% of its respective starting bonus, so `0.4 * 0.25 = 0.1` for BLOOM/HYPERBLOOM/BURGEON and `0.1 * 0.25 = 0.025` for LUNAR_BLOOM. Preserve distinct base/additional contributions and unequal multipliers, even though the trigger and max 4 stacks are shared. Values are additive per-stack increments, not a recursive amplification of the current stack total. Live Crimson Witch's `0.15 * 0.5 = 0.075` example remains compatible; no prior record changed.

#### Rule `stackable-bonus-cap`

Owner-approved on 2026-10-14: a bonus that can accumulate uses optional `accumulations` on the affected modifier (or the native bonus upgrade), storing the source-validated maximum stack count as an integer greater than one. It is a static capability/cap, not the number currently active. Omit the field on non-stackable bonuses; do not emit 0/1 placeholders, infer an unspecified maximum, or attach the cap to unrelated bonuses in the same effect. General semantics apply across entity types without bulk edits.

The owner-approved `party-member-counted-bonus` rule below extends this cap to source-counted uniform party-member contributions. Do not interpret all party-dependent bonuses as stacking: element presence alone is not a member count. Labels/descriptions distinguish the contribution unit; actual eligible counts always remain consumer input.

For uniform additive stacks, keep `values` (or native scalar scaling) as one stack's coefficient and let the consumer choose an active count from zero through the cap. Crimson Witch's additional Pyro modifier uses `values: [0.075]`, `accumulations: 3`; neither its base Pyro bonus nor its reaction bonuses receive this field. Do not replace `hits`, `refinementLevels` or `maxValues` with stack counts; those fields have different meanings. This metadata does not approve collapsing tiered/nonlinear or differently valued stacks into a uniform coefficient; such sources still require an explicit mapping. Trigger, expiration, refresh and actual stack state remain consumer usage.

Retrospective live EN/PT review found no stackable bonus among the six previously worked artifact sets (Adventurer, Archaic Petra, Berserker, Blizzard Strayer, Bloodstained Chivalry, Brave Heart): omission is compatible. Historical Calamity Queller is an applicable future candidate, but its live EN/PT records contain only the stacking passive description and no structured `calculation.effects`; its missing stacking modifier/cap requires separately authorized entity work and fresh validation. No prior entity was modified, and no global stacking coverage claim is made; exact paths/outcomes are recorded in history.

#### Rule `party-member-counted-bonus`

Owner-approved on 2026-10-14: a supplied bonus that adds one uniform contribution per eligible party member keeps the per-member `values` and may use `accumulations` for the source-validated maximum contributor count. This is not accumulation over repeated activations. Gilded Dreams uses `[0.14]` `atk_percent` per other same-element member and `[50]` `elemental_mastery` per other different-element member, each `bonus: true`, `accumulations: 3`; base 2-piece `[80]` EM has no count cap. Localized labels name the per-member category and reaction activation; preserve source descriptions for duration/eligibility.

Exclude the holder. Counts partition the eligible other members, so `same_count + different_count <= 3`; a consumer must not independently maximize both modifiers. Different-element membership compares each teammate to the holder, not distinct element identities among teammates: two Hydro teammates of a Pyro holder each contribute 50 EM. Do not invent a party origin/attribute in `conditional`, multiply stored values by a guessed team, encode reaction activation as reaction-damage scope, or use repeated reactions to add member-count stacks. Party composition, active buff, 8s timing/off-field activation and eligible counts remain consumer state. Nonuniform contributions or unspecified caps still need separate mapping.

Retrospective live EN/PT review: Defender's Will's element-presence RES branches are a counterexample, remain non-stacking and omit `accumulations` even with repeated same-element members. Crimson Witch, Flower of Paradise Lost and Fragment of Harmonic Whimsy are compatible uniform per-stack/cap examples; their contribution units remain activation stacks, not party members, and their caps/values are unchanged. Historical Calamity Queller still has no live normalized effect and requires future separately scoped validation; no incompatible emitted modifier was found. No other entity edited or global party-count/stack coverage certified; paths/results are in history.

#### Rule `enemy-elemental-resistance-reduction`

Owner-approved on 2026-10-14: enemy elemental resistance reductions in `effects[].modifiers[]` use `type: "elemental_res_reduction"`, required `element` (one of the seven elemental Vision identifiers), and a positive ratio in `values` representing the amount subtracted from that enemy's resistance. Deepwood Memories uses `element: "DENDRO"`, `values: [0.3]`: 30 percentage points of resistance reduction, not a 30% relative multiplication of existing resistance, 30% damage bonus, DEF reduction or an increase to the holder's resistance. Keep localized labels explicit about enemy scope and source activation.

Omit `bonus` on this enemy debuff; it is not a supplied character stat/damage bonus. Skill/Burst hits are activation triggers, not restrictions on which subsequent attack categories benefit, so do not emit attack `target` values. Do not invent enemy origins/attributes in `conditional`, precompute a resistance/damage multiplier, or add reaction/stack metadata without validating those mechanics separately. Trigger, affected enemies, 8s duration, off-field applicability and application state remain consumer usage in the preserved source description. This approves only enemy elemental resistance reduction; physical resistance, resistance increases and other DEF/RES mechanics remain outside this contract.

Retrospective live EN/PT review: none of the seven previously worked artifact sets has an elemental resistance-reduction clause; their existing damage/stat/reaction modifiers are not applicable and remain unchanged. The historical Faruzan example is a future candidate: its live Burst description and `skillTalents[2].upgrades[3]` contain an Anemo RES decrease in display-only EN/PT rows, not a normalized conflicting modifier. It requires separately scoped character work and source validation/native-container mapping, not an automatic migration. No other entity was edited; exact paths and outcomes are recorded in history and global resistance coverage is not certified.

#### Rule `wearer-party-element-resistance-bonus`

Owner-approved on 2026-10-14: wearer elemental resistance increases use `type: "elemental_res_percent"`, required elemental `element` and a positive ratio in `values`, with `bonus: true`. Defender's Will grants 30 percentage points of wearer resistance to each distinct corresponding element present in the party. Represent this with seven localized, party-condition-labeled modifiers (GEO/ANEMO/CRYO/DENDRO/ELECTRO/HYDRO/PYRO), each storing `[0.3]`; the consumer activates only elements actually represented. Multiple different elemental bonuses may coexist; repeated same-element party members do not multiply the bonus. This is not Archaic Petra's mutually exclusive selector or an unconditional all-element grant.

Do not use enemy `elemental_res_reduction`, precompute resistance/damage multipliers, multiply by party size, or emit `accumulations`, reaction/attack scope, physical resistance or an attribute-reference `conditional`. Party composition/eligibility is consumer state, not stored activation metadata. This approves scalar wearer elemental RES bonuses, not arbitrary party-wide grants, enemy resistance increases or physical RES. Defender's Will's separate 2-piece is ordinary `def_percent` `[0.3]`, not `base_def`; the owner authorized correcting the stale English "Base DEF" description.

Retrospective live EN/PT review of all eight earlier individual artifact sets found no wearer-resistance clause or conflicting modifier. Deepwood Memories's enemy reduction is a counterexample and retains its type and omission of `bonus`; Archaic Petra's reaction-selected damage alternatives retain their at-most-one constraint. History's archived examples do not identify an earlier wearer-resistance bonus. No other entity edited or globally certified; paths and outcomes are recorded in history.

#### Rule `attack-scoped-speed-bonus`

Owner-approved on 2026-10-14: supplied attack-speed increases use `type: "atk_speed_percent"`, a positive source ratio in `values`, `bonus: true`, and the supported attack `target` when the source restricts the affected category. Desert Pavilion Chronicle's 4-piece Normal Attack speed bonus uses `target: "normal_attack"`, `values: [0.1]`; the Charged Attack hit is its activation trigger, not the speed target. Localized labels identify both the supplied speed and trigger. Preserve the source description and leave activation, 15s duration, animation/hitlag handling and actual attack timing to the consumer.

Speed is distinct from ATK, damage bonuses and a damage-per-second multiplier: do not store it as `atk_percent`/`dmg_percent`, apply it to Charged/Plunging attacks merely because those share the damage clause, or precompute attack intervals/DPS. No element, reaction scope, attribute-reference `conditional`, stack count, rank or independent proc is implied. This source validates only Normal Attack speed; other speed categories and mixed/tiered speed mechanics require their own source mapping, not an assumed global speed grant. The separate 40% damage clause uses three `dmg_percent` modifiers with the supported Normal/Charged/Plunging targets; only the applicable category contributes once to a hit.

Retrospective live EN/PT review of all nine earlier individually worked artifacts found no attack-speed clause or conflicting speed representation. Bloodstained Chivalry's Charged Attack damage modifier is compatible with attack targeting but is not speed; its 10s/zero-Stamina usage is not a speed coefficient. The historical examples in the work history identify no prior speed representation. No other entity edited or globally certified; exact paths/outcomes are recorded in history.

### Stat References and Consumer Usage

`conditional` contains exactly `{ origin, attribute }`. Supported origins are `character`, `weapon`, `artifact`; attributes are `hp`, `atk`, `def`, `elemental_mastery`, `energy_recharge`. Coefficients remain in `values`, not in `conditional.coefficient`, `conditional.value` or a retired `per` field.

#### Rule `attribute-referenced-healing`

Owner-approved on 2026-10-14: stat-referenced HP recovery in an effect uses `type: "healing"`, its source attribute in `conditional`, and the unrounded ratio in `values`. Example: Adventurer's 4-piece "Opening a chest regenerates 30% Max HP over 5s" maps to `{ "label": "HP Recovery from Max HP", "type": "healing", "conditional": { "origin": "character", "attribute": "hp" }, "values": [0.3] }` in top-level `effects[].modifiers[]`. The ratio is the total described recovery, not a guessed per-tick coefficient; trigger and scheduling remain consumer usage.

This is recovery, not an increase to Max HP (`hp_percent`), healing effectiveness (`healing_bonus`) or damage. Omit `bonus`, `target` and damage-element metadata on this recovery coefficient. Do not add a new origin, infer tick counts or pre-apply HP. Fixed recovery follows the separately approved rule below; grouped recovery still requires its own layout. Native talent healing rows retain their attribute-scaling contracts; this rule does not migrate talent `power` to modifier `values`.

Retrospective live-record review: Aquila Favonia EN/PT has an unrepresented ATK-referenced healing clause, requiring an individually authorized follow-up; it has no conflicting emitted healing modifier. Baizhu and Noelle EN/PT native healing rows are display-only in the current baseline, so their historical scalar examples are not live coverage certificates and need separate future entity work. No existing healing modifier was rewritten. These findings do not certify global healing coverage; exact paths and outcomes are recorded in the work history.

Activation, actual active stacks, party state, timing, costs, consumption, cooldowns, chance, infusions and deciding whether a bonus applies belong to the consumer. A validated maximum stack count is static bonus metadata in `accumulations`, not persisted active state. Preserve source descriptions; excluding a timing row from multiplier coverage never authorizes deleting that row. Do not pre-multiply Raiden's coefficient by a fixed Energy cost, Furina's by a fixed Fanfare count, or party-scaled bonuses by a guessed party composition.

#### Rules `flat-defense-bonus` and `fixed-hp-recovery`

Owner-approved on 2026-10-14 for Lucky Dog: direct DEF grants use `flat_def`, direct unnormalized `values` and `bonus: true`; `[100]` is 100 DEF, not 100% or a base-DEF percentage. Fixed HP recovery uses `healing`, direct HP units in `values` (`[300]`), and no `conditional` or `bonus`. A `healing` modifier with a validated attribute reference stores a ratio; an approved fixed-healing modifier without it stores HP units. Do not invent a reference, convert fixed units to percentages, grant Max HP or disguise recovery as healing effectiveness. Trigger/delivery/timing remain consumer usage; this does not approve mixed fixed-plus-variable effect layouts, grouped healing, tick counts or new talent shapes.

Retrospective live EN/PT review: Adventurer's flat HP `[1000]` and HP-referenced healing `[0.3]` remain compatible and distinct; do not remove its `conditional`. Defender's Will/Husk DEF percentages remain `def_percent`, not `flat_def`. Aquila Favonia's ATK-referenced healing clause remains unrepresented and requires future selected work; it is not fixed healing. No incompatible prior emitted modifier found, no other entity edited and no global fixed-recovery/DEF coverage claim. Exact paths/outcomes are recorded in history.

### Repeated Hits, Fixed Additions and Components

- `hits` is an optional positive integer for repeated identical coefficients. Store one hit's coefficient, not a pre-summed total. Do not use it for unequal hit components.
- A fixed `add` is separate from the stat coefficient. In current talent tables it is `power[*].add`, with the unrounded level-specific number.
- These meanings are general. Non-talent skills/effects require an explicit component/refinement mapping before an `add` or `hits` layout can be emitted. A blanket `power` array on item modifiers was never approved; do not invent a rank-array layout without discussing it.
- Different damage elements may be separate components with their own elements and coefficients. This representation does not prove timing or simultaneity.
- Multiple attributes contributing to one result are not independent attacks. Their grouped schema remains deferred; see below. Do not split them further with the scalar schema.
- Never invent a damage element for healing or shield absorption. Incoming healing and outgoing healing are distinct semantics even when the current normalized type is `healing_bonus`; preserve the source label and do not silently interchange them.

### Supported Vocabulary

```ts
type EffectType =
  | 'hp_percent'
  | 'atk_percent'
  | 'atk_speed_percent'
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
  | 'healing'
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
```

`base_atk`/`base_def` are percentage changes to base attributes before stat composition, not ordinary ATK%/DEF%. Do not infer base-stat mechanics from legacy wording. Resistance changes other than the approved enemy elemental reduction and wearer elemental bonus, ignore/reduction of DEF, unsupported speed mechanics outside `attack-scoped-speed-bonus` and unsupported reaction/scoped-CRIT bonuses need an explicit supported contract before import. Flat DEF and fixed recovery follow the approved rules above. Healing coefficients can use approved stat-plus-fixed-addition talent rows; this does not approve every possible healing/result schema.

Reaction bonuses covered by `reaction-scoped-damage-bonus` use this standardized enum (uppercase serialized identifiers are identical across locales):

```ts
enum Reaction {
  OVERLOADED = 'OVERLOADED',
  BURNING = 'BURNING',
  BURGEON = 'BURGEON',
  BLOOM = 'BLOOM',
  HYPERBLOOM = 'HYPERBLOOM',
  LUNAR_BLOOM = 'LUNAR_BLOOM',
  VAPORIZE = 'VAPORIZE',
  MELT = 'MELT',
}
```

## Native Containers and Shapes

| Source location                       | Structured destination when individually approved               |
| ------------------------------------- | --------------------------------------------------------------- |
| Entity progression                    | Character/weapon top-level `calculation`                        |
| Character talent level tables         | `skillTalents[*].upgrades[*].power[*]`                          |
| Character passive or constellation    | That source item's `calculation.effects` / `calculation.skills` |
| Weapon passive / independent proc     | `calculation.effects` / `calculation.skills`                    |
| Artifact set bonus / independent proc | Top-level `effects` / `skills`                                  |
| Shared progression curves             | Dedicated shared dataset, not duplicated inside entities        |

```ts
type Modifier = {
  label: string;
  type: EffectType;
  values: number[];
  bonus?: true;
  accumulations?: number;
  element?: Vision;
  reaction?: Reaction[];
  target?:
    | 'normal_attack'
    | 'charged_attack'
    | 'plunging_attack'
    | 'elemental_skill'
    | 'elemental_burst';
  maxValues?: number[];
  conditional?: {
    origin: 'character' | 'weapon' | 'artifact';
    attribute: 'hp' | 'atk' | 'def' | 'elemental_mastery' | 'energy_recharge';
  };
};
type Effect = {
  label: string;
  refinementLevels?: number[];
  modifiers: Modifier[];
};
type IndependentSkill = {
  refinementLevels?: number[];
  damageType?: Vision;
  scalings: Array<{ type: EffectType; values: number[] }>;
};
type ScalarUpgrade = {
  name: string;
  type: EffectType;
  damageType?: Vision;
  hits?: number;
  bonus?: true;
  accumulations?: number;
  target?: Modifier['target'];
  power: Array<{
    level: number;
    value: string;
    scalings: number;
    add?: number;
  }>;
};
```

`refinementLevels` explicitly maps each coefficient position to a rank. Talent `power[*].scalings` is always scalar, never an array. An independent skill's `scalings` array holds attribute contributions, not talent-level entries. Omit unknown proc damage type instead of guessing it. Labels identify the source and supplied result in the served locale; use a concise localized label when the source has no name.

Map normal/skill/burst by category, not total talent count; alternate sprint/passive display blocks must not shift indexing. A complete Portuguese table is a structural reference, not numerical authority. Verify source name, weapon type, element, corresponding locale labels and every source parameter. Traveler stays `traveler-<element>`; prior tooling used the element-specific male Yatta variant and did not guess unmapped gender-specific rows.

## Examples Captured Before the Reset

These are shortened historical snippets from the pre-reset implementation. Some earlier examples remain in the committed baseline, while bulk-only additions were discarded. Omitted levels/ranks must be validated when an entity is restarted. Examples demonstrate a rule, not an entity-specific exemption or full coverage certificate.

### Result Bonus and Independent Damage: Aquila Favonia

```json
{
  "calculation": {
    "effects": [
      {
        "label": "Falcon's Defiance",
        "refinementLevels": [1, 2, 3, 4, 5],
        "modifiers": [
          {
            "label": "ATK Increase",
            "type": "atk_percent",
            "bonus": true,
            "values": [0.2, 0.25, 0.3, 0.35, 0.4]
          }
        ]
      }
    ],
    "skills": [
      {
        "refinementLevels": [1, 2, 3, 4, 5],
        "damageType": "PHYSICAL",
        "scalings": [
          { "type": "atk_percent", "values": [2, 2.3, 2.6, 2.9, 3.2] }
        ]
      }
    ]
  }
}
```

The healing clause was not fully represented by this example. Do not call the entire passive complete from its ATK buff and damage proc alone.

### Attribute-Referenced Attached Addition: Cinnabar Spindle

```json
{
  "label": "Elemental Skill DMG from DEF",
  "type": "dmg_percent",
  "target": "elemental_skill",
  "conditional": { "origin": "character", "attribute": "def" },
  "values": [0.4, 0.5, 0.6, 0.7, 0.8]
}
```

This archived modifier demonstrates reference/target placement; annotation coverage was incomplete and it did not yet contain `bonus`. Crescent Pike used the same reference pattern with `target: "normal_attack"`, `attribute: "atk"`, and values `[0.2, 0.25, 0.3, 0.35, 0.4]`. Do not turn these into independent procs or percent increases to DEF/ATK.

### One-Hit Coefficient and Hit Count: Ayaka

```json
{
  "name": "4-Hit DMG",
  "type": "atk_percent",
  "damageType": "PHYSICAL",
  "hits": 3,
  "power": [{ "level": 1, "value": "22.6% x 3", "scalings": 0.226464 }]
}
```

The named normal-attack row contains three identical sub-hits. Its coefficient is not multiplied by three in storage. The historical Charged Attack row likewise used `hits: 3` and level-1 `scalings: 0.55126`.

### Unrounded Fixed Addition: Baizhu and Noelle

```json
{
  "name": "Healing",
  "type": "hp_percent",
  "power": [
    {
      "level": 1,
      "value": "8% Max HP + 770.3755",
      "scalings": 0.08,
      "add": 770.3755
    }
  ]
}
```

Baizhu's level-1 shield used `0.008` and `77.03752`. Noelle's healing used `type: "def_percent"`, `scalings: 0.2128`, `add: 102.71802`, while preserving the rounded display `21.3% DEF + 103`. Healing/shield rows do not acquire a fabricated damage element.

### External Inputs Are Not Stored Totals: Raiden and Furina

```json
{
  "name": "Elemental Burst DMG Bonus",
  "type": "dmg_percent",
  "target": "elemental_burst",
  "bonus": true,
  "power": [{ "level": 1, "value": "0.22% Per Energy", "scalings": 0.0022 }]
}
```

Use the receiving burst's Energy cost as consumer input; Raiden's own cost of 90 is not universal. Furina used `bonus: true` with level-1 damage conversion `0.0007` (`dmg_percent`) and incoming-healing conversion `0.0001` (`healing_bonus`). Neither is pre-multiplied by Fanfare; neither is a Hydro-only bonus.

### Supplied Bonus Versus Attack: Faruzan

```json
{
  "name": "Anemo DMG Bonus",
  "type": "atk_percent",
  "damageType": "ANEMO",
  "bonus": true,
  "power": [{ "level": 1, "value": "18", "scalings": 0.18 }]
}
```

This exact owner-approved convention was retained historically. `bonus` distinguishes the result from an ATK-scaled hit; it does not grant ATK. The legacy display `18` and source interpretation must be rechecked before reuse; do not generalize `atk_percent` to unrelated elemental bonuses.

### Damage Reference and Stale Display: Razor

```json
{
  "name": "Soul Companion DMG",
  "type": "dmg_percent",
  "damageType": "ELECTRO",
  "target": "normal_attack",
  "power": [{ "level": 1, "value": "20% normal attack dmg", "scalings": 0.24 }]
}
```

The coefficient refers to Normal Attack damage, not ATK. This archived snippet deliberately shows the known mismatch between stale English display and the imported parameter. Preserve the evidence and require a separate translation review; do not silently copy it as a newly validated display.

### Mixed Elements Are Not Mixed Attributes: Freminet

```json
[
  {
    "name": "Level 1 Shattering Pressure DMG (Cryo)",
    "type": "atk_percent",
    "damageType": "CRYO",
    "power": [{ "level": 1, "value": "100.24% Cryo", "scalings": 1.0024 }]
  },
  {
    "name": "Level 1 Shattering Pressure DMG (Physical)",
    "type": "atk_percent",
    "damageType": "PHYSICAL",
    "power": [{ "level": 1, "value": "48.69% Phys", "scalings": 0.48688 }]
  }
]
```

The prior split covered Pressure levels 1-3, not a certification of Pressure level 4's element. Preserve components without asserting timing or simultaneous application.

### Partial Set Coverage: Berserker

```json
{
  "effects": [
    {
      "label": "2-Piece Bonus",
      "modifiers": [
        {
          "label": "CRIT Rate Increase",
          "type": "crit_rate",
          "values": [0.12],
          "bonus": true
        }
      ]
    }
  ]
}
```

The prior record still lacked the separate 4-piece additional `24%` coefficient. The HP threshold is consumer usage; that threshold must not hide the missing result coefficient. Chevreuse's Healing Duration is the complementary example: a timing row is not a missing healing multiplier and must stay available for presentation.

## Deferred Grouped Multi-Attribute Schema

Approved direction, **not approved for emission yet**. Before implementing it, audit every existing upgrade and every consumer, not just previously pending entities. Generate UUID v4 IDs once; keep them stable across locales. References are local to the containing upgrade. Preserve original display text and keep each numeric scaling scalar.

```json
{
  "name": "Rush Attack DMG",
  "specification": [
    {
      "id": "61421c4c-b08b-49f8-9c2e-efcc747d44a0",
      "name": "Rush Attack — ATK",
      "damageType": "DENDRO",
      "type": "atk_percent"
    },
    {
      "id": "972e7157-a5aa-4b55-a3ef-0e1f587f6323",
      "name": "Rush Attack — Elemental Mastery",
      "damageType": "DENDRO",
      "type": "elemental_mastery"
    }
  ],
  "power": [
    {
      "level": 1,
      "value": "193.6% ATK + 154.88% Elemental Mastery",
      "multipliers": [
        {
          "specificationId": "61421c4c-b08b-49f8-9c2e-efcc747d44a0",
          "scalings": 1.936,
          "value": "193.6% ATK"
        },
        {
          "specificationId": "972e7157-a5aa-4b55-a3ef-0e1f587f6323",
          "scalings": 1.5488,
          "value": "154.88% Elemental Mastery"
        }
      ]
    }
  ]
}
```

The UUIDs above are illustrative. The prior core-template scan found Alhaitham (ATK+EM, five source rows), Chiori (ATK+DEF, three), Dehya (ATK+HP, three), Nahida (ATK+EM, one), Sethos (ATK+EM, one): 13 grouped rows, with two repeated-hit checks dependent on grouping. This inventory is historical structural evidence, not exhaustive passive/constellation certification.

## Consolidated Progression and Damage Roadmap

The former plans described an external consumer's `src/control/stats`, `src/control/progression` and `src/control/damage`; these are not runtime entrypoints in this API repository. Their implementation/production-status claims are historical and must be rechecked in that consumer. The former target was patch 7.1; do not label fresh sources as supporting that patch without verification.

| Layer            | Required inputs and unresolved work after reset                                                                                                     |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| Progression      | Initial HP/ATK/DEF, weapon ATK/substat, shared curves, cumulative ascension bonuses, selected level/ascension, patch                                |
| Stat composition | Ascension/weapon/artifact attributes, main/substat tables and configured rolls, set/passive/constellation/buff modifiers, elemental goblet identity |
| Hit base         | Owner/level, exact talent coefficients, attribute references, additions, repeated hits, mixed attributes/elements and infusion/conversion mapping   |
| Direct damage    | Applicable damage bonuses, CRIT, target damage reduction, special multipliers, enemy level/DEF/RES and reduction/ignore inputs                      |
| Reactions        | Patch-specific formulas, coefficients, ownership, EM-dependent terms and applicable reaction modifiers; not guaranteed by an entity API             |
| Consumer state   | Configuration, party, activation, timing/stacks and input values; not persisted as entity coefficients                                              |

Progression uses `initialStat * curve[level] + cumulativeAscensionBonus`. The prior KQM arrays were indexed with `level - 1`; respect the selected dataset's indexing. Ascension is explicit because boundary levels have pre/post-ascension states. Do not interpolate linearly, round internally or confuse growth curves with hit scaling.

The baseline progression shape is `calculation.initialStats`, `growthCurves`, `ascensions[{ ascension, maxLevel, bonuses }]`. The earlier consumer plan used a different ascension-key example; align the adapter explicitly rather than assuming schemas are interchangeable. Shared curve datasets declare their patch and are versioned once, not copied into entities. The committed shared curves were restored with the entity baseline; revalidate patch/source coverage before extending or consuming them.

Recommended stages after the first entity's rules are settled: validate/version progression snapshots; build consumer adapters; represent hit coefficients/results; validate enemy DEF/RES inputs; then cover reactions and special mechanics. Do not turn this roadmap into permission for another bulk migration or unrequested runtime/frontend changes.

## Sources and API Limitations

- Yatta character/weapon indices and details expose JSON. Character level tables contain `talent.*.promote.*.params` and formula templates; many passive/constellation/weapon/set effects remain prose inside JSON.
- Yatta progression fields previously inspected were `upgrade.prop[].initValue`, curve `type`, `upgrade.promote[]`, and cumulative `addProps`. Recheck field layout and source identity for each selected entity.
- Yatta `.../en/reliquary` exposes artifact entries and textual set bonuses; it does not guarantee an executable effect schema.
- KQM pages supply mechanical descriptions/notes; its Git repository supplies versionable raw curve tables, not a confirmed general REST API. Persist the source commit hash in maintenance history/snapshots, not entity responses.
- Game8 pages are HTML validation sources, not a confirmed public calculation API. An occurrence of a number anywhere on the page is not clause-specific validation.
- The previously attempted Yatta `.../en/curve` endpoint returned 404; do not invent an alternative endpoint. No source is guaranteed to provide every input for complete damage calculation.
- Fetch and normalize deliberately during maintenance, not on every frontend calculation. Prefer patch-associated snapshots; do not mix source revisions silently. Unavailable sources and disagreements remain blockers under the research policy.

### Historical Entity References

These URLs identify evidence used before reset, not fresh validation of every entity:

- Baizhu: `https://gi.yatta.moe/api/v2/en/avatar/10000082` — level coefficients/additions.
- Ayaka: `https://gi.yatta.moe/api/v2/en/avatar/10000002` — repeated hits.
- Noelle: `https://gi.yatta.moe/api/v2/en/avatar/10000034` — DEF-based healing/shields.
- Faruzan: `https://gi.yatta.moe/api/v2/en/avatar/10000076` — bonus row.
- Raiden: `https://gi.yatta.moe/api/v2/en/avatar/10000052` — receiving-burst Energy coefficient.
- Furina: `https://gi.yatta.moe/api/v2/en/avatar/10000089` — Fanfare conversions.
- Razor: `https://gi.yatta.moe/api/v2/en/avatar/10000020` — Normal Attack damage reference.
- Freminet: `https://gi.yatta.moe/api/v2/en/avatar/10000085` — elemental/physical components.
- Aquila Favonia: `https://gi.yatta.moe/api/v2/en/weapon/11501`, `https://library.keqingmains.com/equipment/weapons/swords#aquila-favonia` — refinement coefficients and mechanics.
- Cinnabar Spindle: `https://library.keqingmains.com/equipment/weapons/swords#cinnabar-spindle` — DEF-referenced addition.
- Crescent Pike: `https://library.keqingmains.com/equipment/weapons/polearms#crescent-pike` — attached attack addition.
- Berserker: `https://game8.co/games/Genshin-Impact/archives/305480`, `https://library.keqingmains.com/equipment/artifacts#berserker` — separate set bonuses.
- Mixed-attribute templates: `https://gi.yatta.moe/api/v2/en/avatar/10000078`, `/10000094`, `/10000079`, `/10000073`, `/10000097` — prior structural inventory.
- KQM character/weapon curves: `https://raw.githubusercontent.com/KQM-git/TCL/master/src/data/character_curves.json`, `https://raw.githubusercontent.com/KQM-git/TCL/master/src/data/weapon_curves.json`.
- KQM progression reference: `https://raw.githubusercontent.com/KQM-git/TCL/master/src/utils/stats/charstats.ts`.

## History Entry Template

```md
### YYYY-MM-DD — <type>/<id>

- Status: <in-progress|partial|blocked|complete|reviewed-not-applicable>
- Scope and stage: <source items, locales, progression/effects/hits reviewed>
- Baseline and resulting files: <commit, exact paths, change reference>
- Sources: <entity-specific URLs, access dates, patch/revision, validated facts>
- Rules applied: <coefficient units, type/reference, target, hits/add/bonus, components>
- New/revised text rule: <stable rule key, semantic pattern, representation, limits/counterexamples, or none>
- Retrospective compatibility: <rule key; reviewed entity IDs/source-item paths and outcomes; unreviewed candidates; individually scoped follow-ups>
- Example: <short actual snippet, explicitly state omitted levels/ranks>
- Checks: <JSON, source tables, bilingual parity, formatting, build/lint results>
- Remaining blockers: <mapping, unsupported result, conflict, deferred schema>
- Next approved action: <resume this entity; do not select another automatically>
```
