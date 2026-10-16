# Pre-reset Calculation Notes

Historical only; never import bulk output or treat examples as live coverage.

- Aquila separates ATK modifier from independent Physical proc; its healing clause remained incomplete.
- Cinnabar/Crescent Pike demonstrate attached target damage with a DEF/ATK reference, not a stat grant or independent proc.
- Ayaka demonstrates `hits` stores one identical-hit coefficient; Baizhu/Noelle demonstrate raw fixed `add` separate from scaling.
- Raiden/Furina demonstrate external consumer inputs are not precomputed totals; Razor’s stale display requires source/translation review.
- Freminet demonstrates separate elemental components, not multi-attribute splitting. Grouped multi-attribute results remain deferred: no `specification`/`multipliers` schema is approved for emission.
- The API serves static inputs only. Progression/stat composition/damage/reactions/consumer state remain consumer concerns; revalidate source revisions before any future work.

Historical source links are retained in `rule-decisions.md`.
