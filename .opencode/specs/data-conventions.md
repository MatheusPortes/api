# Data Conventions

## Lookup Rules

- API paths resolve directly from the directory tree under `assets/data/`; nested directories are part of the entity type path.
- Data lookup lowercases entity IDs, then reads the requested language file exactly. It adds or overwrites the top-level `id` with the directory ID at response time.
- Do not assume an image exists for every data record. Image coverage is incomplete for several types.

## Storage Layouts

| Data area         | Layout                                                   | Notes                                                                                   |
| ----------------- | -------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| Standard entities | `<type>/<id>/en.json`                                    | Artifacts, characters, domains, elements, legacy enemies, nations, and weapons.         |
| Bosses            | `boss/weekly-boss/<id>/en.json`                          | `weekly-boss` is part of the route and image path.                                      |
| Living beings     | `living-being/{enemies,groups,families,types}/...`       | Individual enemies and classification records use different schemas.                    |
| Materials         | Root category catalogs and `materials/drop/<id>/en.json` | Only `drop` records are individually addressable.                                       |
| Consumables       | `consumables/{food,potions}/en.json`                     | Each file is a catalog keyed by item ID, not per-item directories.                      |
| Growth curves     | `calculation/curves/en.json`                             | One shared dataset with a top-level `patch`; do not duplicate curve tables in entities. |

## Record Shapes

| Area                 | Established shape                                                                                                              | Compatibility rule                                                                 |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------- |
| Characters           | Profile fields plus `skillTalents`, `passiveTalents`, `constellations`, `vision_key`, `weapon_type`, and `ascension_materials` | Optional profile fields and outfits exist; inspect a comparable record.            |
| Artifacts            | `name`, `max_rarity`, and piece-bonus fields                                                                                   | Most use `2-piece_bonus` and `4-piece_bonus`; one-piece sets use `1-piece_bonus`.  |
| Weapons              | `name`, `type`, `rarity`, `baseAttack`, `subStat`, and location/passive fields                                                 | Legacy `ascensionMaterial` and newer `baseDamage`/`description` variants coexist.  |
| Domains              | Location, requirements, recommended elements, and rewards                                                                      | Reward tiers inconsistently use `drops` or `items`; preserve the comparable shape. |
| Legacy enemies       | Region, type, family, elements, and drops                                                                                      | This schema is incompatible with `living-being/enemies`.                           |
| Living-being enemies | `id`, `name`, optional resistance, element, damage type, category, faction, and drop                                           | Groups, families, and types are separate classification records.                   |

## Calculation Data

- This API exposes calculation inputs only; it does not calculate character stats or damage.
- Add validated character and weapon mechanical data under a top-level `calculation` field in `en.json` first. After the English contract is approved, copy its exact mechanical fields to every existing localized record for that entity; localize human-readable labels alongside the served locale.
- Entity calculation data contains only values unique to that entity, such as initial properties, curve identifiers, ascension bonuses, talent multipliers, and structured effects.
- Structured passive modifiers are stored in `calculation.effects`. An effect can map its values to refinement levels with `refinementLevels` and contains `modifiers`.
- Weapon damage procs are stored separately in `calculation.skills`. A weapon skill has optional `refinementLevels`, a `damageType` using the uppercase `Vision` enum, and `scalings`. Each scaling identifies its normalized status `type` and decimal-ratio `values` (`2` for 200% of ATK).
- Each effect modifier has a `label`, a normalized `type`, and decimal-ratio `values` (`0.2` for 20%). Use a name supplied by the effect text when available; otherwise create a concise, meaningful label in the record's locale. `elemental_dmg_percent` requires `element` using the uppercase `Vision` value.
- The supported normalized effect types are `hp_percent`, `atk_percent`, `elemental_dmg_percent`, `physical_dmg_percent`, `def_percent`, `crit_rate`, `crit_dmg`, `energy_recharge`, `healing_bonus`, `elemental_mastery`, `flat_hp`, and `flat_atk`. Keep the source text even when a number does not yet have a supported type.
- Shared curve tables belong only in `calculation/curves/en.json`. Its top-level `patch` identifies the game patch for every included table.
- Do not expose upstream provider IDs in entity responses. They may be used in import and validation tooling only.

## Translations

- Language coverage and casing are inconsistent across types. Use the exact local filename convention of the comparable entity.
- A translation is a complete served response, not a field-level overlay. Retain every field required by the intended response shape.
- Validate JSON before completing work. Existing translation files can be partial or invalid; do not copy their omissions blindly.

## Images and Generation

- Store source image files without filename extensions. The API emits WebP by default and can emit PNG, JPG, or JPEG on request.
- Character images commonly use `card`, `portrait`, `icon-big`, `icon-side`, `gacha-card`, `gacha-splash`, constellation, talent, and namecard names.
- `icon-big` should be 256x256. Run `pnpm run gen` after changing it; generated 128px `icon` files are ignored.

## Legacy Data

- Preserve established keys exactly, including misspellings such as `descrition`, `enimies`, and `namequality`, unless a deliberate schema migration is requested.
- Scripts other than `pnpm run gen` are one-off historical migrations, not current validators or generators.
