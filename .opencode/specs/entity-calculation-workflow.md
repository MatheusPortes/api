# Entity Calculation Workflow

## Authority and Restart Point

This is the single active calculation-data plan and rulebook. It consolidates the former calculation gaps, progression sources, structured effects plan and talent review notes. Approved rules below apply by mechanical meaning across every entity, not exclusively to talents, characters, weapons or artifacts. Container differences do not create different mathematical rules.

The owner retired the bulk migration on 2026-10-13. On 2026-10-14 all pending entity changes were restored to the last committed baseline, superseding the earlier removal of committed progression/effect data. Previously committed fields remain; uncommitted bulk results do not. Start again with one explicitly selected entity. Historical examples below are design references, not proof of current field presence or full entity certification; inspect the selected record.

Read `application-state.md`, `data-conventions.md`, `entity-research.md` and this file before work. For earlier attempts, consult `.opencode/history/entity-work-history.md`; its snapshot and old blocker report are history, not automatically loaded instructions or a current completion list. Never restore an old bulk result wholesale.

## Work in Stages, One Entity at a Time

1. **Select and inspect:** record the exact entity ID, source items, intended patch, current Git baseline and requested scope. Do not expand to other entities without permission.
2. **Inventory:** list each source coefficient and classify it as progression, stat/result modifier, independent proc, attached damage, repeated hits, fixed addition, mixed element, grouped attributes, consumer usage or unsupported result. Name the affected paths before editing.
3. **Research:** verify identity and corresponding numerical rows/refinements using the entity research policy. Save source URLs, access dates, source revision/patch and raw responses outside served data. A JSON response does not mean every mechanic is normalized.
4. **Map and decide:** map names, categories, component identity, talent levels and refinement ranks explicitly. Reuse approved rules; when a text interpretation establishes or changes a general rule, perform the retrospective compatibility review below before calling the rule settled. Stop unsupported layouts or factual conflicts instead of inventing types or using coincidental coefficients.
5. **Apply:** edit only the selected entity's English and Portuguese records. Preserve existing display text, profile fields, progression outside the requested scope and every other locale. Missing display levels may be filled from validated localized source templates; do not relabel or delete old rows to make an audit pass.
6. **Validate:** check JSON, all source levels/ranks, raw unrounded coefficients, supported types, required element metadata, hit counts, additions and bilingual mechanical parity. Inspect the diff for unrelated changes. Run focused formatting, `pnpm run build` and `pnpm run lint` with Node.js 20/pnpm; disclose failures or omitted checks. Do not format unrelated files to conceal baseline failures.
7. **Record and stop:** append the entity's scope, changes, examples, sources, decisions, checks and remaining blockers to the history. Mark complete only for explicitly reviewed coverage; then wait for the next entity. Do not run a bulk migration or select the next entity automatically.

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
- `target` is optional only for genuinely generic damage bonuses. Supported targets are `normal_attack`, `charged_attack`, `plunging_attack`, `elemental_skill`, and `elemental_burst`. Unsupported reaction/scoped-CRIT results must not be disguised as generic damage or global CRIT.
- `elemental_dmg_percent` requires `element`. Expand an all-element bonus into seven modifiers: GEO, ANEMO, CRYO, DENDRO, ELECTRO, HYDRO, PYRO; exclude physical damage.
- `maxValues` is an optional same-rank numeric cap. Do not pre-apply consumer inputs to stored coefficients.

### Stat References and Consumer Usage

`conditional` contains exactly `{ origin, attribute }`. Supported origins are `character`, `weapon`, `artifact`; attributes are `hp`, `atk`, `def`, `elemental_mastery`, `energy_recharge`. Coefficients remain in `values`, not in `conditional.coefficient`, `conditional.value` or a retired `per` field.

Activation, stacks, party state, timing, costs, consumption, cooldowns, chance, infusions and deciding whether a bonus applies belong to the consumer. Preserve source descriptions; excluding a timing row from multiplier coverage never authorizes deleting that row. Do not pre-multiply Raiden's coefficient by a fixed Energy cost, Furina's by a fixed Fanfare count, or party-scaled bonuses by a guessed party composition.

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
  | 'base_atk'
  | 'base_def'
  | 'elemental_dmg_percent'
  | 'physical_dmg_percent'
  | 'dmg_percent'
  | 'def_percent'
  | 'crit_rate'
  | 'crit_dmg'
  | 'energy_recharge'
  | 'healing_bonus'
  | 'elemental_mastery'
  | 'flat_hp'
  | 'flat_atk';

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

`base_atk`/`base_def` are percentage changes to base attributes before stat composition, not ordinary ATK%/DEF%. Do not infer base-stat mechanics from legacy wording. Resistance changes, ignore/reduction of DEF, attack speed, reaction bonuses, scoped CRIT and flat DEF need an explicit supported contract before import. Healing coefficients can use approved stat-plus-fixed-addition talent rows; this does not approve every possible healing/result schema.

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
  element?: Vision;
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
