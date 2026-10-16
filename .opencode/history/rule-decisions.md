# Rule Decisions and Reusable Evidence

Read a section only for the matching active rule. Live records remain authoritative for compatibility.

## Owner-selected resolutions

- **Archaic Petra:** retain existing four ordinary Crystallize elemental branches; use KQM/Yatta Lunar-Crystallize text over Game8’s older text. No general source override.
- **Nighttime Whispers:** use KQM/Yatta nearby Lunar-Crystallize Moondrift alternative over Game8 shield-only wording; entity-only resolution.
- **Scholar:** fixed Energy is consumer-only; use KQM/Yatta Particle/Orb EN wording, not generic Energy-gain wording.
- **Traveling Doctor:** Max HP is the recovery reference; owner-confirmed because accessible clauses omitted the denominator.
- **Ocean-Hued Clam:** proc is Physical; owner-confirmed with KQM support. Detailed formula certification was dismissed.
- **Akuoumaru:** `time.energy_cost` means combined party maximum Burst Energy Cost only; never a timer/general state namespace.
- **Source consensus (2026-10-16):** owner replaced the blanket conflict stop with corroborated independent-source consensus; the current rule is in `entity-research.md`. Astral Vulture uses matching KQM/Yatta Swirl-or-Stellar-Swirl wording over Fandom's Swirl-only clause. Unlike earlier entity-specific resolutions above, this instruction is standing authorization for subsequent research.

## Compatibility findings

- Artifact independent proc nesting: Clam is the only found artifact root-skill correction; Aquila’s weapon `calculation.skills` is intentionally different.
- Consumer-empty effects: Gambler cooldown reset, Prayers status duration, Scholar/The Exile/Scroll fixed Energy are not stat/result coefficients. Lucky Dog fixed healing is a counterexample.
- Input caps: Clam independent and Song attached `accumulated_healing_percent` caps apply to recorded-healing HP input; Emblem’s cap remains a result cap.
- CRIT scopes: Berserker (holder HP), Blizzard (target state), Marechaussee (HP-change stacks), Resolution (Charged-only) remain distinct.
- Reaction scopes: Crimson Witch, Flower, Thundering Fury, Viridescent, Aubade and later sets retain source-specific reaction lists; equal values merge only under `equal-coefficient-reaction-alternatives`.
- Stacks/totals: Crimson/Flower/Husk/Marechaussee/Fragment are uniform-cap examples; Nymph is mutually exclusive tier totals; Gilded is party-contributor counting.
- RES: Deepwood (enemy) and Defender/Tiny Miracle (wearer) are distinct; Viridescent same-element alternative triggers do not double.
- Future candidates, not approved edits: Aquila healing; Calamity Queller stacking; Faruzan enemy RES; Wavebreaker’s Fin and Mouun’s Moon party Energy Cost.

## Reusable historical source links

- Aquila: `https://gi.yatta.moe/api/v2/en/weapon/11501`, `https://library.keqingmains.com/equipment/weapons/swords#aquila-favonia`
- Cinnabar / Crescent Pike: KQM sword/polearm equipment pages.
- Ayaka/Baizhu/Noelle/Faruzan/Raiden/Furina/Razor/Freminet: Yatta avatar IDs `10000002/82/34/76/52/89/20/85`.
- Curves: `https://raw.githubusercontent.com/KQM-git/TCL/master/src/data/character_curves.json` and `weapon_curves.json`.

These are historical evidence, never fresh validation or a permission to migrate.

## 2026-10-16 — attribute-referenced-independent-scaling

- Owner correction: Ash-Graven Drinking Horn must explicitly identify `character.hp` as the independent scaling reference. Extend `IndependentSkill.scalings[]` with the existing `conditional` shape; keep the selected scaling's native `hp_percent`, values [.4,.5,.6,.7,.8], native container and unknown damage type unchanged. This does not convert a proc to an HP stat grant.
- Read-only compatibility review of live EN/PT independent skills: `assets/data/weapons/ash-graven-drinking-horn/{en,pt}.json`, `calculation.skills[0].scalings[0]`: correction-required, corrected in this change; `assets/data/weapons/aquila-favonia/{en,pt}.json`, same path: correction-required for the missing explicit `character.atk` reference, not edited because this correction request selects Ash-Graven and Astral only; `assets/data/artifacts/ocean-hued-clam/{en,pt}.json`, `effects[1].skills[0].scalings[0]`: not-applicable, its recorded-healing input follows a separate approved contract rather than a character attribute. No other live EN independent-skill containers were found. Other locales remain outside maintenance scope.
- Evidence: owner instruction and the already inspected KQM/Yatta Max HP clauses at https://library.keqingmains.com/equipment/weapons/catalysts#ash-graven-drinking-horn and https://gi.yatta.moe/api/v2/en/weapon/14427 (PT: `/api/v2/pt/weapon/14427`). No claim of global reference consistency while Aquila remains uncorrected.
