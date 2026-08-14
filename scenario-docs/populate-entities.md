---
title: Populate entities
description: Create the mobile actors present on the map when the scenario begins.
---

Create only the mobile actors validated during brainstorming.

## Consumed fields

Each entry in `entities` contains:

- `name`: a unique, concrete name;
- `description`: identity, initial situation, and useful distinguishing details;
- `cell_id`: an existing cell copied from the provided map;
- `coordinates`: an optional `[longitude, latitude]` position;
- `is_featured`: prominence in the interface, with no effect on playability;
- `stat_defs`: applicable initial values;
- `assets.mapAssetKey`: the exact key assigned during the asset step.

Do not add any other field.

Copy `cell_id` only from the exact `cell_id` field of the selected source-cell
line. Ignore every number embedded in its `slug`. A cell described by
`{"cell_id":118,"slug":"germany-124"}` must be referenced as `118`.

## Presence on the map

Every entity must be physically present in its `cell_id` at
`story.start_date`. Remove actors outside the map. Do not retain them with a
fictional position or nearby cell.

Coordinates are technically optional, but a complete one-shot result should
provide them whenever the starting position is knowable or can be authored
deliberately. Place a known person where they are at `story.start_date`. Place
an army, fleet, government, or group at its headquarters, base, assembly area,
or another concrete starting point supported by its description. Do not place
every actor at a capital or generic cell position merely because they share a
country.

Omit `coordinates` only when cell-level uncertainty is intentional and no
precise starting point can be justified. When coordinates are present, use
`[longitude, latitude]`, keep the real or authored point authoritative, and
select the exact source `cell_id` that contains that point. Never substitute
the cell's `center` or the center of its `bbox`.

Example — Brazil: Deodoro da Fonseca may be included if he is in Rio de Janeiro
at the start and that city is on the map. A diplomat stationed in London at
that time must be excluded if London is outside the map.

Example — Troy: Hector in Troy and Achilles in the Achaean camp may be included
if their cells appear on the map. An ally who remains outside its extent must
not receive a fictional cell.

## No "player" entity

All entities are potentially playable. Do not create an abstract entity named
"The Player," "Player," or "Protagonist." Do not write descriptions from a
privileged point of view.

`is_featured` only selects the entities highlighted in the interface.

## Description

The description must remain factual and useful regardless of the entity chosen
to start. It must not contain instructions, second person, or a predetermined
future. It must be no more than 500 characters.

Correct example:

> Hector commands the Trojan forces and defends the city while facing pressure
> from his family, his allies, and a prolonged war.

Incorrect example:

> You are Hector and must defeat Achilles before the end of the day.

Do not add `role`, `verbs`, `opening_problem`, or `visual_elements`. If a detail
is essential, write it naturally in `description` or in a statistic the
product actually consumes.
