# Archived Calculation Workflow Reference Material

This is the preserved reference material removed from the active calculation workflow to keep routine context focused. Read it only when its specific topic is relevant. It does not override the active rulebook.

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
