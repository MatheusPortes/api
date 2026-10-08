# Structured Effect Data Plan

> **Status:** planning document; it is not an implementation contract.

## Goal

Convert supported numeric modifiers and damage scalings currently embedded in entity text into locale-aware, calculation-ready data. Preserve every existing text field for presentation and conditions that are not yet modeled.

## Validated Reference

`weapons/aquila-favonia` is the reference implementation for weapon passive data:

- `calculation.effects` stores the passive ATK percentage modifier.
- `calculation.skills` stores the weapon's separate physical-damage proc, even though its scaling uses the wielder's ATK.
- `values` use decimal ratios: `0.2` is 20%, and `2` is 200% of the relevant stat.
- Mechanical fields are identical across locales; human-readable `label` values are localized in each served record.

## Current Contracts

### Passive modifiers

```ts
{
  refinementLevels?: number[],
  modifiers: Array<{
    label: string,
    type: EffectType,
    values: number[],
    element?: Vision,
  }>,
}
```

- Use the source effect's own name as `label` when it exists. Otherwise create a concise label in the record locale.
- `refinementLevels` maps each array position to a weapon refinement rank.
- `element` is required for `elemental_dmg_percent`.

### Damage skills

```ts
{
  refinementLevels?: number[],
  damageType: Vision,
  scalings: Array<{
    type: EffectType,
    values: number[],
  }>,
}
```

- A damage proc supplied by a weapon, artifact, passive, or constellation is its own skill, not a passive stat modifier.
- A scaling `type` identifies the owner stat used to produce the damage; it does not grant that stat to the character.
- Add an identifier or label only when multiple skills in the same source cannot otherwise be distinguished.

### Supported effect types

```ts
type EffectType =
  | 'hp_percent'
  | 'atk_percent'
  | 'elemental_dmg_percent'
  | 'physical_dmg_percent'
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

Do not force numbers that are outside this type set into an incorrect type. Keep them in the text until the contract is expanded. Examples include durations, cooldowns, resistance changes, healing that scales from another stat, and generic damage bonuses without a known damage type.

## Expansion Scope

| Entity area | Text source                                       | Structured destination                                                       |
| ----------- | ------------------------------------------------- | ---------------------------------------------------------------------------- |
| Artifacts   | `1-piece_bonus`, `2-piece_bonus`, `4-piece_bonus` | `calculation.effects` and `calculation.skills` when the bonus creates damage |
| Weapons     | `passiveDesc`                                     | `calculation.effects` and `calculation.skills`                               |
| Characters  | `skillTalents[*].upgrades[*]`                     | talent multiplier and damage-skill data                                      |
| Characters  | `skillTalents[*]['attribute-scaling'][*]`         | talent multiplier and damage-skill data by talent level                      |
| Characters  | `passiveTalents[*]`                               | `calculation.effects` and `calculation.skills`                               |
| Characters  | `constellations[*]`                               | `calculation.effects` and `calculation.skills`                               |

## Delivery Sequence

1. Review the contract for artifacts and characters before editing them, especially how to represent talent level arrays, damage element, conditions, and multiple skills from one text source.
2. Inventory each source field and classify values as a supported stat modifier, damage scaling, unsupported value, or no numeric effect.
3. Validate each entity's changed mechanical values in Game8, HoYoWiki, and Genshin Impact Wiki. Do not edit an entity when a required source is unavailable or conflicts without an official resolution.
4. Add the English calculation data first, then copy mechanical fields to every existing localized record while translating labels for those locales.
5. Validate JSON, locale mechanical-field parity excluding localized labels, supported types, required `element` and `damageType` fields, and formatting.
6. Update `data-conventions.md` whenever the approved contract changes; do not silently broaden the schema during a batch.

## Open Discussion Items

- Which additional types should model healing scaling, flat DEF, generic damage bonuses, resistance changes, duration, cooldown, energy cost, stack limits, and trigger conditions?
- How should character skill entries map a value to its talent level, especially when the legacy `upgrades` and `attribute-scaling` shapes differ between locales?
- Should a character damage skill always declare its element or physical type, including infusions and conversions?
- What conditions must be structured before an effect is eligible for automatic calculation?
