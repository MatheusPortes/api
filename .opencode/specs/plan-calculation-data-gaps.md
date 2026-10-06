# Calculation Data Gaps

> **Status:** planning document; it is not an implementation contract.

## Scope

The API exposes static inputs for an external consumer to calculate progression, stats, and damage. It never runs those calculations. The current target patch is **7.1**.

## Data Placement

- Store values unique to a character or weapon in that entity's `en.json` under `calculation`.
- Store shared character and weapon growth tables once in `assets/data/calculation/curves/en.json`; expose them at `/calculation/curves` with a top-level `patch`.
- Do not duplicate shared curves, patch values, or upstream-provider IDs in entity responses.
- Add calculation fields to localized entity files only after the English shape is approved.

## Known Gaps

| Area      | Required structured data                                                                                                                                                                    |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Character | Initial HP/ATK/DEF, growth-curve identifiers, ascension bonuses, talent multipliers, scaling property, damage element, infusion/conversion, and structured passive and constellation rules. |
| Weapon    | Initial ATK, growth-curve identifiers, substat progression, ascension bonuses, refinement effects, and structured trigger, duration, limit, and proc rules.                                 |
| Artifact  | Element-specific goblet bonus and structured set-bonus rules.                                                                                                                               |
| Enemy     | Numeric resistance by phase, damage-type-to-resistance mapping, DEF and RES reductions, and DEF ignore.                                                                                     |

## Validation Rule

Text is not calculation data. Normalize and validate every numeric value, effect, and patch before exposing it.
