---
title: Define and populate statistics
description: Define statistics and their initial values without duplication.
---

First define every row in `story_stats`. Then populate the values for the world,
cells, entities, and landmarks. Never redefine a statistic within each element.

## One definition per statistic

Every `stat_id` is unique across the scenario. Use a stable semantic identifier
in lowercase ASCII with underscores, such as `country`, `kingdom`, or
`military_strength`.

A statistic may apply to several types if it retains exactly the same meaning,
domain, and unit. In that case, list multiple values in `applies_to`.

## Order and primary identity

Place the cell statistic that carries the primary territorial identity first.
It must be categorical, have homogeneous nominal values, and assign a color to
each one.

Other cell statistics are secondary states. They cannot replace the primary
identity with values such as `strong`, `weak`, `stable`, or `contested`.

Before populating values, verify that the primary identity exactly matches the
wording confirmed by the user during brainstorming. Do not choose a new one at
this stage.

Example — Brazil: `Country` is the primary identity, with `Brazil`, `Argentina`,
and `Paraguay`. `Political stability`, with `Stable`, `Fragile`, and `In crisis`,
is a secondary state.

Example — Troy: `Kingdom` is the primary identity, with `Kingdom of Troy`,
`Kingdom of Mycenae`, and `Kingdom of Sparta`. `Military status`, with `Stable`,
`Mobilized`, and `Besieged`, is a secondary state.

## Types

- `categorical`: an identity or category with no natural order;
- `ordinal`: an ordered progression with clearly defined levels;
- `scalar`: a bounded quantity with a useful unit;
- `text`: a free-form note genuinely needed by the simulation.

Do not use a scalar simply because a number seems more precise. Its bounds and
unit must remain meaningful over time.

Examples: `Country` is categorical; `Political stability` is ordinal;
`National tension` from 0 to 100 is scalar; a text note is suitable only when
free-form information genuinely needs to be stored and simulated.

## Default and initial values

`default_value` is the value an applicable subject receives when no specific
value is provided. Use `null` when a statistic applies only to certain subjects
of a type.

In the final JSON:

- `world_stats` gives a value to every statistic applicable to `playthrough`
  that must exist at launch;
- every `cell_values` entry provides at least the primary territorial identity
  and relevant secondary states;
- every entity or landmark contains its specific values in `stat_defs`;
- a value equal to the default may be repeated explicitly;
- an absent value is acceptable only if the default produces exactly the
  intended state, or if the default is `null` and the statistic does not apply
  to that subject.

All initial values are serialized as strings.

Example: a tension of 72 is written as `"72"` in `world_stats`, while its domain
bounds remain the numbers `0` and `100`. An entity without a specific influence
value may inherit `"medium"` if that is the default; a statistic reserved for a
few commanders may use `null` as its default.

## Examples by owner

- world: `national_tension` in Brazil or `siege_duration` in Troy;
- cell — primary identity: `country` or `kingdom`;
- cell — secondary state: `political_stability` or `war_status`;
- entity: `political_influence` or `loyalty`;
- landmark: `urban_control` or `fortification_condition`.

Each example remains a distinct statistic. Do not put a loyalty value in
`country`, or a value such as `Besieged` in `kingdom`.

## Colors

For a categorical or ordinal statistic visualized on the map, map each domain
key to a distinct hexadecimal color in `value_colors`. Never use a color as a
domain value.

Each `domain` item is exactly `{ "key": "technical_key", "value": "Display label" }`.
Do not write `{ "value": "Display label", "color": "#808080" }`: `color` is
not an allowed field there. There is no 50-option limit for a categorical or
ordinal domain.

## Current limits

Do not exceed ten world stats, three cell stats, ten entity stats, and four
landmark stats. Use fewer statistics when the scenario remains clear without
them.
