# Entity Research Policy

## Scope

Apply this policy before using external factual information to create, correct, or edit an entity, translation, image label, relationship, or entity calculation data.

## Required Validation

1. Research the exact entity in all three required community sources:
   - Game8: `https://game8.co/games/Genshin-Impact`
   - HoYoWiki: `https://wiki.hoyolab.com/`
   - Genshin Impact Wiki: `https://genshin-impact.fandom.com/`
2. Use entity-specific pages, not source homepages, and compare each factual field being changed.
3. Treat a fact as validated only when all three sources agree or an official Genshin source resolves a documented disagreement.
4. Do not edit factual data when any required source is unavailable, lacks the entity, or has an unresolved conflict. Report the blocker instead.

## Calculation Data Exception

- Character and weapon calculation data may use Yatta as the structured entity source and KQM TCL as the shared growth-curve source.
- Validate that the Yatta entity name, type, and element or weapon type match the local record before importing values.
- Keep upstream IDs and source metadata out of public entity responses.
- This exception applies only to numerical progression and calculation fields. The required community-source validation remains in effect for all other entity facts.

## References

Every response for a data change or factual research task must include a `References` section. List the source name, entity-specific URL, and the facts validated there. State disagreements, unavailable sources, and any official tie-breaker explicitly.

## Release Status

Do not add unreleased, preview, leaked, or speculative content. Confirm the entity is released in the global game version through the required sources.
