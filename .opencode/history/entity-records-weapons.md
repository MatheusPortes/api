# Weapon Entity Records

Each complete entry is EN/PT passive scope only, committed/mirrored individually in the authorized 2026-10-16 weapon round. KQM/Yatta validated values; inaccessible primary pages are recorded as unavailable, not inferred.

| ID                         | Status / compact result                                                                                                                                                                  |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| a-thousand-blazing-suns    | complete: root `effects`; five-rank CRIT DMG .2–.4 and ATK .28–.56, plus 75%-of-base Nightsoul increments. PT base stats corrected to 49/2.4%.                                           |
| a-thousand-floating-dreams | complete: root `effects`; same-element EM and different-element seven-element DMG per-member values, caps 3; party EM delivery.                                                          |
| absolution                 | complete: root `effects`; CRIT DMG .2–.4; generic DMG .16–.32×3.                                                                                                                         |
| akuoumaru                  | complete: root Burst DMG .0012–.0024 per combined party Burst Energy Cost, caps .4–.8; `time.energy_cost` owner-approved. Wavebreaker’s Fin/Mouun’s Moon are unedited future candidates. |
| alley-hunter               | complete: root generic DMG .02–.04 per off-field second, cap 10; EN stale passive corrected.                                                                                             |
| amber-catalyst             | removed: owner-authorized deletion of the unreleased CBT3 record/icon; delivery removes only the served EN JSON.                                                                         |
| amenoma-kageuchi           | blocked: fixed 6/7.5/9/10.5/12 Energy per consumed Seed (cap 3) is consumer-only; no approved weapon container exists for an empty fixed-Energy effect.                                  |

For completed records, JSON/parity/source/diff/format checks passed. `pnpm run build` failed on baseline TypeScript 4.9/current Node declarations; lint had unrelated Prettier failures. No push was authorized.

Amenoma Kageuchi research (2026-10-16): KQM and Yatta EN/PT agree on its released four-star Sword identity, 41 Base ATK, 12% ATK, five fixed Energy values per Seed, Elemental Skill trigger, three-Seed cap, Burst consumption and two-second delay. Fandom was inaccessible (403), Game8's attempted entity route returned 404, and HoYoWiki returned no usable content. Existing `consumer-only-fixed-energy-restoration` is limited to native artifact effects with `modifiers: []`; approving an equivalent root weapon `effects` entry would be a new container/semantic decision. Sources: https://library.keqingmains.com/equipment/weapons/swords#amenoma-kageuchi, https://gi.yatta.moe/api/v2/en/weapon/11414, https://gi.yatta.moe/api/v2/pt/weapon/11414.
