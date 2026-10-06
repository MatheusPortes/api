# Character and Weapon Progression Data Plan

> **Status:** planning document; it is not an implementation contract.

## Purpose

Provide versioned static progression data for external consumers to calculate character and weapon base stats by level and ascension. The API only serves this data.

**Current target patch:** 7.1.

## Data Contract

Each entity supplies its unique values in `calculation`:

```ts
{
  initialStats: Record<Property, number>,
  growthCurves: Record<Property, CurveId>,
  ascensions: Array<{
    level: number,
    maxLevel: number,
    bonuses: Partial<Record<Property, number>>,
  }>,
}
```

`Property` includes base HP, ATK, DEF, weapon substats, and ascension attributes where applicable. `CurveId` identifies a table in `/calculation/curves`; the curve table itself is never duplicated in entity records.

The consumer resolves a base stat as:

```text
initial stat × curve multiplier at level + cumulative ascension bonus
```

No API calculation, rounding, or external request occurs at runtime.

## Sources

### Yatta API: entity-specific structured data

- Character: `https://gi.yatta.moe/api/v2/en/avatar/{avatarId}`
- Weapon: `https://gi.yatta.moe/api/v2/en/weapon/{weaponId}`
- Character index: `https://gi.yatta.moe/api/v2/en/avatar`

Relevant fields include `upgrade.prop[].initValue`, `upgrade.prop[].type`, `upgrade.promote[]`, and talent `params`. Yatta does not provide a validated curve endpoint; `https://gi.yatta.moe/api/v2/en/curve` returns HTTP 404.

### KQM TCL: shared growth tables

- Character curves: `https://raw.githubusercontent.com/KQM-git/TCL/master/src/data/character_curves.json`
- Weapon curves: `https://raw.githubusercontent.com/KQM-git/TCL/master/src/data/weapon_curves.json`

KQM is a versionable Git source. Persist a verified table in the API dataset rather than querying it during client use.

## Validation

Before adding entity data, validate its factual values under `entity-research.md`. Check the entity and curves against the declared patch, retain source responses during import work, and record unresolved discrepancies rather than approximating values.
