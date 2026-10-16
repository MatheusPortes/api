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

- **Crescent Pike independent proc (2026-10-16):** owner approved the displayed `calculation.skills` mapping, Physical, `atk_percent` [.2,.25,.3,.35,.4] with `character.atk`, superseding its archived attached-damage note. Rule `independent-triggered-weapon-damage` distinguishes separate proc output from trigger categories. Read-only compatibility: Crescent Pike EN/PT had no live passive mapping and are mapped individually; Ash-Graven EN/PT independent HP-reference shape compatible, unchanged; Aquila EN/PT independent-container shape compatible but previously missing ATK reference remains correction-required/outside scope, unchanged; Echoes EN attached Normal damage remains compatible/unchanged, not migrated. Evidence and source/translation limits are in `entity-records-weapons.md` under this ID. No global completeness claim.

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

## 2026-10-16 — party-member-counted-bonus: source-specific holder eligibility

- Owner approved the displayed Chain Breaker example: per-eligible-member ATK [.048,.06,.072,.084,.096], cap four; direct EM [24,30,36,42,48] once at three or more eligible members. Eligibility is Natlan OR different Elemental Type from the wearer, counting each member once; a Natlan wearer qualifies. The stable rule now follows source-specific holder eligibility instead of globally excluding the holder. No new response fields or party-state engine.
- Read-only compatibility review of live EN/PT paths: `assets/data/artifacts/gilded-dreams/{en,pt}.json`, `effects[1].modifiers[0..1]`: compatible, source text explicitly refers to other members and combined cap three; `assets/data/weapons/a-thousand-floating-dreams/{en,pt}.json`, `effects[0].modifiers[0..7]`: compatible, other-member exclusion and per-source cap-three constraints remain unchanged. Its unstacked party EM grant is not-applicable to contributor counting.
- Nearby shapes reviewed to avoid false migrations: `assets/data/weapons/alley-hunter/{en,pt}.json`, `effects[0].modifiers[0]`: not-applicable, time-based units rather than party contributors; `assets/data/weapons/astral-vulture-s-crimson-plumage/{en,pt}.json`, `effects[0].modifiers[1..4]`: not-applicable, mutually exclusive tier totals; `assets/data/weapons/ballad-of-the-fjords/{en,pt}.json`, `effects[0].modifiers[0]`: not-applicable, party-diversity threshold rather than per-member multiplication. All these prior paths remain untouched. Chain Breaker is the only selected application; no unresolved compatibility correction found in these directly relevant records.
- Evidence: matching KQM/Yatta/Fandom clauses already inspected in this round: https://library.keqingmains.com/equipment/weapons/bows#chain-breaker ; https://gi.yatta.moe/api/v2/en/weapon/15431 ; https://genshin-impact.fandom.com/wiki/Chain_Breaker#Gameplay_Notes (explicit Natlan-wearer inclusion). Owner-approved representation, not a source conflict or automatic authorization to edit future count mechanics.
