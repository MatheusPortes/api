# Data Conventions

## Lookup Rules

- Active individual-entity maintenance targets `en.json` and `pt.json` only. Preserve every other locale. On 2026-10-14 entity records were restored exactly to Git HEAD, including previously committed calculation fields in all locales. The uncommitted bulk migration remains discarded. The runtime continues serving existing locales.
- Explicit exception on 2026-10-14: the owner requested removing the entire unreleased `artifacts/glacier-and-snowfield` entity, including its six locale files. This entity-specific deletion does not authorize removing other records, changing other locales or broadening ordinary EN/PT maintenance; its scope/outcome is recorded in history.

- API paths resolve directly from the directory tree under `assets/data/`; nested directories are part of the entity type path.
- Data lookup lowercases entity IDs, then reads the requested language file exactly. It adds or overwrites the top-level `id` with the directory ID at response time.
- Do not assume an image exists for every data record. Image coverage is incomplete for several types.

## Storage Layouts

| Data area         | Layout                                                   | Notes                                                                           |
| ----------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Standard entities | `<type>/<id>/en.json`                                    | Artifacts, characters, domains, elements, legacy enemies, nations, and weapons. |
| Bosses            | `boss/weekly-boss/<id>/en.json`                          | `weekly-boss` is part of the route and image path.                              |
| Living beings     | `living-being/{enemies,groups,families,types}/...`       | Individual enemies and classification records use different schemas.            |
| Materials         | Root category catalogs and `materials/drop/<id>/en.json` | Only `drop` records are individually addressable.                               |
| Consumables       | `consumables/{food,potions}/en.json`                     | Each file is a catalog keyed by item ID, not per-item directories.              |

## Record Shapes

| Area                 | Established shape                                                                                                              | Compatibility rule                                                                 |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------- |
| Characters           | Profile fields plus `skillTalents`, `passiveTalents`, `constellations`, `vision_key`, `weapon_type`, and `ascension_materials` | Optional profile fields and outfits exist; inspect a comparable record.            |
| Artifacts            | `name`, `max_rarity`, and piece-bonus fields                                                                                   | Most use `2-piece_bonus` and `4-piece_bonus`; one-piece sets use `1-piece_bonus`.  |
| Weapons              | `name`, `type`, `rarity`, `baseAttack`, `subStat`, and location/passive fields                                                 | Legacy `ascensionMaterial` and newer `baseDamage`/`description` variants coexist.  |
| Domains              | Location, requirements, recommended elements, and rewards                                                                      | Reward tiers inconsistently use `drops` or `items`; preserve the comparable shape. |
| Legacy enemies       | Region, type, family, elements, and drops                                                                                      | This schema is incompatible with `living-being/enemies`.                           |
| Living-being enemies | `id`, `name`, optional resistance, element, damage type, category, faction, and drop                                           | Groups, families, and types are separate classification records.                   |

## Calculation Work

- Entity records restarted from the last committed baseline; subsequent individual changes are recorded in the work history. Previously committed progression snapshots, shared curves and earlier effect examples remain available; uncommitted bulk calculation additions are discarded. Neither field presence nor presentation tables certify full calculation coverage.
- `.opencode/specs/entity-calculation-workflow.md` is the single authoritative calculation plan and general rulebook: schemas, coefficients, modifiers/procs, `hits`, `add`, `bonus`, `target`, references, supported vocabulary, deferred grouped contributions and source-backed examples are defined there rather than duplicated here.
- Select one entity and work in explicit research/mapping/application/validation/history stages. Do not run a bulk migration or infer completion from field presence. Keep approved mechanical fields aligned between English and Portuguese, with localized labels and preserved source descriptions.
- Use `.opencode/history/entity-work-history.md` for restart context and prior attempts; the old blocker report/archive is historical, not a current pending list. Record scope, patch, source URLs, actual examples, checks and remaining blockers after each stage.
- This API serves static inputs only; runtime stat/damage calculation and consumer state remain outside it. Do not expose provider IDs or maintenance metadata in entity responses.
- Individually reviewed artifact set modifiers use optional top-level `effects`, with localized effect/modifier labels and one coefficient in `values` for an unranked bonus; do not invent refinement ranks. `artifacts/adventurer` represents its 2-piece `flat_hp` bonus (`1000`, `bonus: true`) and 4-piece `healing` coefficient (`0.3`, `conditional` referencing `character.hp`, without `bonus`). Preserve both display descriptions. The semantic contract is rule `attribute-referenced-healing` in the calculation workflow.
- Approved reaction-damage modifiers retain `type: "dmg_percent"` and add a nonempty `reaction` enum array, grouping the reactions that share one source multiplier. Canonical enum values, scope limits and fixed-source bonus normalization are defined only in the calculation workflow. `crimson-witch-of-flames` is the first individually reviewed example; do not add empty reaction arrays or reaction scope to ordinary damage bonuses.
- Stackable bonuses use optional `accumulations` for a source-validated maximum count, not active consumer state; retain per-stack coefficients. Apply rule `stackable-bonus-cap` from the calculation workflow, omitting the field on non-stackable bonuses and preserving EN/PT mechanical parity.
- Nonuniform stack tiers may use source-validated, condition-labeled total modifiers under `nonuniform-stack-tier-totals` in the calculation workflow. Nymph's Dream's 1/2/3+ alternatives omit `accumulations`; select one total per stat, never sum tiers or treat them as refinement ranks/uniform contributions. Consumer state chooses the eligible tier.
- Source-counted uniform party-member bonuses may use `accumulations` as the maximum contributor count under approved rule `party-member-counted-bonus`; `values` remain per member, not a precomputed team total. Gilded Dreams's two 4-piece counts exclude the holder and together count at most three other members. Mere party-element presence (Defender's Will) does not gain stacks by repeated members.
- Flower of Paradise Lost extends approved reaction scope with BLOOM/HYPERBLOOM/LUNAR_BLOOM, preserving base and additional reaction contributions separately. Shared activation does not justify merging unequal per-stack values (`0.1` versus `0.025`); both additional modifiers use `accumulations: 4`. Enum authority and fixed-source normalization remain in the calculation workflow.
- Approved enemy elemental resistance reductions use `elemental_res_reduction` with explicit `element` and a positive reduction amount in `values`; omit `bonus` and do not disguise the result as a damage bonus. The authority and limits are rule `enemy-elemental-resistance-reduction` in the calculation workflow; Deepwood Memories is the first individually reviewed example.
- Approved wearer elemental resistance bonuses use `elemental_res_percent`, explicit `element`, positive `values` and `bonus: true`. Defender's Will uses seven party-element-conditioned alternatives that may coexist only for represented elements; no stacking by repeated elements or unconditional all-element grant. Apply rule `wearer-party-element-resistance-bonus` from the calculation workflow.
- Approved attack-speed bonuses use `atk_speed_percent`, a source-validated attack `target`, positive `values` and `bonus: true`, distinct from attack/damage stat bonuses. Desert Pavilion Chronicle's Normal Attack speed is `[0.1]`, separately from its three attack-targeted damage modifiers `[0.4]`. Apply rule `attack-scoped-speed-bonus` from the calculation workflow.
- Independent damage from recorded healing uses native `skills[].scalings[]` with `accumulated_healing_percent`, `[0.9]` and owner-approved `maxValues: [30000]` on Ocean-Hued Clam. This skill cap is the HP-unit input limit before the ratio, not damage output; existing modifier caps (Emblem) still cap supplied results. The 4-piece effect has `modifiers: []` with its damage represented by `skills`. No `conditional`, `bonus` or new input-cap field is emitted; see `accumulated-healing-independent-damage` in the rulebook.

## Translations

- Lucky Dog's owner-approved `flat_def` uses direct DEF units; fixed `healing` uses direct HP units without `conditional`/`bonus`. Attribute-referenced recovery retains its ratio and `conditional` (Adventurer); apply the calculation workflow's `flat-defense-bonus` and `fixed-hp-recovery` rules, not guessed percentages.

- Language coverage and casing are inconsistent across types. Use the exact local filename convention of the comparable entity.
- For individual character talent work, a complete `pt.json` is the structural reference for `upgrades` and `power`. Validate numerical mechanics against an approved source before aligning English/Portuguese fields; preserve localized names/descriptions. An incomplete Portuguese record is not a license to guess missing mechanics or change other locales.
- A translation is a complete served response, not a field-level overlay. Retain every field required by the intended response shape.
- Standing owner authorization on 2026-10-14 permits EN/PT display-text alignment with corresponding validated source clauses/templates without per-correction approval. Record each correction, preserve other locales/fields and follow source-conflict rules. Commit/advance authorization follows the separately clarified bounded-round Git workflow; text authorization alone does not approve new mechanical schemas.
- Validate JSON before completing work. Existing translation files can be partial or invalid; do not copy their omissions blindly.

## Images and Generation

- Store source image files without filename extensions. The API emits WebP by default and can emit PNG, JPG, or JPEG on request.
- Character images commonly use `card`, `portrait`, `icon-big`, `icon-side`, `gacha-card`, `gacha-splash`, constellation, talent, and namecard names.
- `icon-big` should be 256x256. Run `pnpm run gen` after changing it; generated 128px `icon` files are ignored.

## Legacy Data

- Preserve established keys exactly, including misspellings such as `descrition`, `enimies`, and `namequality`, unless a deliberate schema migration is requested.
- Scripts other than `pnpm run gen` are one-off historical migrations, not current validators or generators.
