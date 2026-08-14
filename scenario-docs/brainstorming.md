---
title: Scenario brainstorming
description: Gather the decisions needed before preparing the final file.
---

This document describes the only visible phase before the final file. Its
purpose is to surface the necessary decisions, not to immediately populate a
technical structure.

## Start with what already exists

First analyze the idea and the provided map. The map already exists: its extent
and cells are input constraints.

Automatically use the language of the user's first message. Do not ask the user
to choose a language. If the first message mixes languages, use the dominant
language.

Keep that same language consistently throughout the entire interview,
validation brief, and all visible scenario content. Do not switch languages
because a source, the map, a proper name, or an example uses another language.
The final file's technical keys, `stat_id` values, domain keys, and
`map_asset_key` values remain unchanged.

If the idea is very brief, begin with its essence. Ask what situation should
exist at the start, what tensions make it compelling, and what kinds of actors
should be playable. Do not turn this step into a promise aimed at a single
character.

## Direct-generation exception

If the user explicitly tells you to make every necessary decision, ask no
questions, and produce the complete final file immediately, that instruction
overrides the interview and confirmation requirements below. Treat it as
permission to decide every missing blocking requirement and as explicit
validation of the resulting scenario.

In this case, do not show a brief, ask for confirmation, or stop because the
date or primary territorial identity has not been separately approved. Research
and choose those values, complete the brief internally, then proceed directly
to the final-file step in the same response. All quality, map, contract, and
mechanical validation rules still apply.

## Conduct the interview

- Ask one to three closely related questions at a time.
- Do not ask for information already provided.
- When a topic is specific enough, move to the next one.
- If the user is unsure, offer two or three concrete options and state which
  one you recommend.
- Verify accessible facts yourself. Cite important research in the validation
  brief without turning the interview into a source review.
- Do not silently make a decision that would change the essence, date, primary
  territorial identity, or initial distribution.

## Blocking requirements

### Essence

The scenario must define an initial situation that is compelling to experience
from several possible entities. It must not impose a future plot or victory
condition unless the user requests one.

### Start date

Obtain a precise date and time in the logical format `YYYY-MM-DDTHH:MM:SS`. If
the user provides only a period, research and propose a precise moment, then
ask the user to validate it.

Example — Brazil: if the user says only "at the fall of the Empire," propose
`1889-11-15T08:00:00`, briefly explain the choice, then ask for confirmation.
Do not silently turn a period into a date.

### Existing map

Verify only:

- what the existing map covers;
- which existing cells will be used;
- whether the planned entities and landmarks can be placed there.

Do not ask for a `bbox`, projection, tiling, cell count, or new geometry.

### Primary territorial identity

One cell statistic must take visual precedence. It describes the identity or
affiliation of each area: country, kingdom, territorial faction, culture,
religion, sphere of influence, or another homogeneous category that genuinely
occupies an area.

Ask specifically which names should appear on the map's areas. Every value must
answer the same question.

Explicitly tell the user that this decision determines **what will primarily be
labeled and colored on the map**. Before continuing, always restate the
decision as follows:

> The map's primary identity will be **[type]**. The visible names will be
> **[values]**. Information such as **[secondary states]** will remain separate
> states. Is this what you primarily want to see on the map?

This confirmation is required. Do not infer the primary identity from other
statistics, and do not continue until the user confirms the restatement.

Correct: `France`, `Spain`, `Italy` for a country identity.

Example — Brazil: primary identity `Country`, with `Brazil`, `Argentina`, and
`Paraguay`. Example — Troy: primary identity `Kingdom`, with `Kingdom of Troy`,
`Kingdom of Mycenae`, and `Kingdom of Sparta`.

Incorrect: `France`, `Catholicism`, `Contested`, `Napoleon`. This list mixes a
country, religion, state, and person.

`Strong`, `weak`, `stable`, `contested`, `wealthy`, or a percentage are
secondary states, never the primary identity.

Every cell must receive exactly one primary identity value.

### Secondary territorial states

Every secondary state answers one question and has homogeneous values. Several
states may coexist, including stability, dominant religion, degree of control,
wealth, or crisis intensity. None replaces the primary identity.

Example — Brazil: a cell remains identified as `Brazil` and separately receives
the political state `Fragile`. Example — Troy: a cell remains identified as
`Kingdom of Troy` and separately receives the military state `Besieged`.

The current contract allows no more than three cell statistics. Count the
primary identity among those three statistics.

### Entities

An entity is a named actor that can move: a person, specific group, army, fleet,
vehicle, creature, or another identifiable mobile subject.

All initial entities must be physically present on the map at the start. Do not
invent an off-map entity because it is famous or might become involved later.
An abstract concept, profession, or generic category is not automatically an
entity.

All entities are potentially playable. Do not create a privileged point of view
called "the player." If the user mentions a specific player, turn that idea
into a concrete, named person, group, or other entity.

Flexible guidelines:

- intimate scenario: about 10 to 20 entities;
- national or ensemble scenario: about 20 to 40;
- large geopolitical simulation: about 40 to 80.

The relevance of the list always matters more than the quota.

Example — Brazil: an officer present in Rio de Janeiro may be created; a
diplomat stationed in London may not if London is outside the map. Example —
Troy: Hector in Troy and Achilles in the Achaean camp may be created if both
positions are on the map; an ally who remains outside its extent may not.

### Landmarks

A landmark is a fixed place displayed as a named point. Its granularity must
match the map's scale.

- world map: major cities, capitals, metropolitan centers, major hubs, and rare
  sites of global importance;
- national map: important cities, ports, regional capitals, and major sites;
- regional map: cities, towns, and regional sites;
- city map: neighborhoods, buildings, and distinct local places.

On a world map, Paris may be a landmark; a student bedroom or small studio may
not. A small-scale place remains context in an entity description or the world.

Flexible guidelines:

- city map: about 10 to 30 landmarks;
- regional map: about 15 to 40;
- national map: about 20 to 60;
- continental map: about 30 to 100;
- world map: about 60 to 100.

Example — on a world map for the Brazil scenario, Rio de Janeiro and Buenos
Aires may be landmarks, but a private bedroom may not. On a map limited to Troy
and its surroundings, the city of Troy, the Achaean camp, and a fortified gate
may be visible; an individual tent remains context.

#### Cover the map before increasing density

Design landmarks in two passes. First, divide the inhabited and
scenario-relevant map into meaningful macroregions based on its landmasses,
cells, and the scenario. Give every macroregion enough spatially separated
landmarks to remain readable at the map's full extent. On a world map, no major
inhabited continent or relevant theater should be left with a large accidental
blank area. When the scale and limit allow it, use at least two separated
landmarks in each major macroregion rather than one isolated token point.

Second, add more landmarks where the action is denser. The result need not be
mathematically uniform: the scenario's core region may be richer. It must not
become one crowded cluster surrounded by empty relevant regions.

Every landmark must still earn its place. Fill coverage gaps with meaningful
capitals, cities, ports, transport hubs, chokepoints, strongholds, or other
scenario-relevant places at the map's scale. Never add an arbitrary or generic
point only to satisfy distribution. For a fictional setting, invent deliberate
places with distinct roles and positions rather than placeholder locations.

Before presenting the brief, review the landmark list as if viewing the whole
map. Identify its densest cluster and its largest inhabited empty area. Revise
the list until the cluster is justified and no empty area is accidental.

### Statistics

Define only persistent information that affects interpretation or simulation.
A statistic may apply to one or more types among the world, cells, entities,
and landmarks if its meaning and domain remain identical.

Clearly separate:

- world stats;
- cell stats, including the primary identity;
- entity stats;
- landmark stats.

The current contract allows up to ten world stats, three cell stats, ten entity
stats, and four landmark stats. These limits are not targets.

For each statistic, clarify its question, type, domain, owners, default value,
and useful evolution.

Examples of correct separation:

| Owner                   | Brazil              | Troy                    |
| ----------------------- | ------------------- | ----------------------- |
| World                   | National tension    | Siege duration          |
| Cell — identity         | Country             | Kingdom                 |
| Cell — state            | Political stability | Military status         |
| Entity                  | Political influence | Loyalty                 |
| Landmark                | Urban control       | Fortification condition |

### Simulation rules

Keep only scenario-specific practices not already expressed by descriptions
and statistics. A rule may specify a delay, cost, or historical, technological,
magical, legal, or logistical constraint unique to the world.

Do not repeat:

- that the world or other actors evolve;
- that no future is fixed;
- that entities and landmarks must be on the map;
- the date, lists of elements, or their initial values;
- Davia's general principles.

Example — Brazil: "Military orders require a credible chain of command" is
specific and useful. Example — Troy: "An army cannot cross the walls without a
breach, ruse, or surrender" genuinely guides the simulation. By contrast,
"actors evolve autonomously" repeats a general principle.

## Brief to validate

Once every requirement is met, produce a readable brief containing:

1. the provisional name and subtitle;
2. the scenario's essence;
3. the date and initial situation;
4. a very short reminder of the map already provided;
5. the primary territorial identity and its distribution;
6. secondary territorial states;
7. the selected entities and their positions;
8. the selected landmarks and their positions;
9. statistics by type;
10. initial tensions;
11. only the specific simulation rules;
12. the selected scale estimates.

This brief is for human validation, not the final file. End by explicitly
asking whether the scenario is validated and whether to prepare the final file.
