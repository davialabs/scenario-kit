---
title: Populate story
description: Describe the initial situation without imposing a playable point of view.
---

Populate `story` from the validated brainstorming. Do not add facts merely to
make the text more dramatic.

## Name and subtitle

The name identifies the scenario. The subtitle describes its central pressure
without promising the experience of a specific character.

## Premise

The premise describes the initial situation shared by all playable entities. It
does not address "you" or assume a protagonist, resources, or objective that
belongs to only one entity.

Avoid:

> Paris, 1 January 2026. You have no audience, amateur equipment and €2,000.

This wording imposes a character before the user chooses one.

Prefer wording that describes the moment, pressures, and shared possibilities
of the world.

Example — Brazil:

> On the morning of November 15, 1889, the imperial government still holds Rio
> de Janeiro as officers, political leaders, and the imperial family face a
> rupture whose outcome remains open.

Example — Troy:

> After years of siege, Troy still stands as the Trojan and Achaean commands
> face exhaustion, internal rivalries, and pressure to force a decision.

## World description

`world_description` explains only the context needed to understand the
situation: institutions, power relationships, material conditions, common
knowledge, and current tensions.

Do not repeat Davia's native principles there. In particular, remove sentences
such as:

> Every actor evolves autonomously; no future event is fixed.

This sentence does not describe this specific world.

Do not restate the list of entities, landmarks, and values already stored
elsewhere.

Example — Brazil: describe the fragility of military support, the institutions
still operating, and the slow movement of news between the capital and the
provinces. Example — Troy: describe the walls, supply lines, alliances, and toll
of the siege. Do not turn these paragraphs into lists of people or places.

## Simulation rules

`simulation_rules` is a short text containing only scenario-specific
constraints. A rule must concretely change how an action is resolved or a
value evolves.

Do not write:

> All created entities and POIs must remain on the displayed world map.

This is a contract requirement, not a rule of the simulated world.

Do not repeat the general autonomy of actors, the absence of a fixed timeline,
the date, the map, lists, or statistics. A general realism principle is useful
only if it specifies an expected tone beyond the native rules.

Valid examples:

- Brazil: military orders depend on a credible chain of command, and news
  travels through the means available in 1889;
- Troy: the walls require a breach, ruse, or surrender, and large armies depend
  on their supply lines.

## Date

`start_date` uses the exact validated date, including seconds and without an
additional time zone field.

Do not add visual direction to `story`. Image generation is a separate optional
step performed only if the user requests it after the final file is created.
