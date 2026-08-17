---
title: Create a Davia scenario with AI
description: Lead the brainstorming, validate the scenario, then prepare its final file.
---

This page contains instructions for the AI. Follow them directly: do not
summarize the documentation or ask the user what they want to do with it.

You must help the user design a Davia scenario, then produce a single final
file that follows the contract. The deliverable is one downloadable `.json`
file whose entire contents are the JSON object defined by the contract. It is
never a Markdown (`.md`) document, a plain-text (`.txt`) document, or JSON that
describes a document with keys such as `title`, `preamble`, or `sections`.
When speaking with the user, simply call it **the scenario's final file**.
[Davia](https://davia.ai/) is the website for the product in question.

The examples in this documentation illustrate structure and reasoning. Never
reuse their names, values, or elements unless they match the user's scenario
and map.

The user must provide:

- their starting idea, however brief;
- a description of the extent of the existing map;
- the map's cell data, copied exactly from the website. The user does not need
  to understand or modify it.

The map is an input. Do not redraw its extent, create new cells, or copy its
geometry into the result.

## Understand the provided map data

The prompt contains one map-context object followed by one compact JSON line per
cell. The board gives the current scenario name and map metadata. Each cell may
contain `cell_id`, `slug`, `name`, `kind`, `description`, `bbox`,
`center`, and `neighbors`.

Use all of this as read-only context. Coordinates are the preferred placement
input. Davia derives the containing cell from every supplied position. Build a
lookup from the exact `cell_id` field on each JSON line only for entities whose
precise position is intentionally unknown. A fallback `cell_id` is never a
number inferred from the cell's `slug`, name, order, `center`, or `bbox`. For
example, if a source line contains `{"cell_id":118,"slug":"germany-124"}`,
the only valid fallback is `118`, never `124`.

Never copy the board object, slugs, bounds, centers, neighbors, or cell
descriptions into the final file. Give every landmark its own
`[longitude, latitude]` coordinates. For a known real place, use its actual
geographic position. For a fictional place, choose a deliberate point
consistent with the scenario. This is a positioning requirement, not a demand
for historical realism. Never use the cell's `center` or `bbox`, and never
reuse one generic point for several places. A missing or generic landmark
position makes the final file invalid and Davia rejects it.

## Non-negotiable output gate

The final JSON has exactly these six root keys: `story`, `story_stats`,
`world_stats`, `cell_values`, `entities`, and `landmarks`. Before delivering
the file, inspect every landmark object. Each one must have exactly this shape:

```json
{
  "name": "<landmark name>",
  "coordinates": [-43.1729, -22.9068],
  "stat_defs": {},
  "assets": { "mapAssetKey": "poi:settlement" }
}
```

`coordinates` is required for every landmark, without exception. Omit
`cell_id`; Davia derives it from the point. Missing coordinates is a blocking
error, not a warning. There is no cell-center fallback in the final-file
contract. Do not create a landmark unless you can provide its deliberate
position on the map.

When an entity has a knowable or deliberately authored starting point, include
its coordinates and omit `cell_id`. People use their actual location at
`start_date`; armies, fleets, and groups use their headquarters, base, or
concentration point. Only when no precise position can be justified, omit
`coordinates` and provide the exact source `cell_id` as a fallback. If both
fields are present, coordinates are authoritative. Do not leave an entire
entity list at generic cell positions.

Run a map-wide coverage pass for landmarks. Spread meaningful places across
the inhabited and scenario-relevant extent before adding extra density to the
core action. No large relevant landmass or macroregion may remain empty merely
because another region contains most of the action. Do not fill gaps with
arbitrary points: use important cities, ports, hubs, chokepoints, or deliberate
fictional places that belong in the scenario.

## Repair a rejected or paused import

When the user returns validation errors or warnings, keep the validated
scenario and repair the current final file. Do not restart brainstorming and do
not replace the file with a generic example.

The correction request must include the original creation instructions and the
complete board context. An error list by itself is not enough to reconstruct
cell IDs or verify coordinates. Rebuild the exact source-cell lookup from that
context before changing `cell_values`, entity fallbacks, or feature positions.

Never copy coordinates, cell IDs, names, or values from a contract example or
repair template. In particular, `[0, 0]` is not a neutral placeholder: it is a
real geographic position and is invalid unless the authored feature genuinely
belongs there and the point lies inside the current board. Never replace an
invalid position by trying arbitrary pairs such as `[1, 1]`, `[2, 2]`, or
`[5, 5]`.

Warnings that say omitted cells will use defaults do not require invented
explicit values. If those defaults express the validated scenario exactly,
keep the file unchanged and tell the user it can be imported with the warning.
Otherwise, use the complete board context to add only the deliberate values
that differ from the defaults. Never generate terrain, ownership, or another
cell state from the numeric `cell_id`, array order, or an arbitrary formula.

After a correction, rerun every mechanical and editorial check, not only the
check named by the latest error. Return one complete final file without
ellipsis, placeholder prose, or truncated collections.

Hard collection limits: at most 30 `story_stats`, between 1 and 100 `entities`,
and between 0 and 100 `landmarks`. Across `story_stats`, no more than 10
definitions may apply to `playthrough`, 3 to `cell`, 10 to `entity`, and 4 to
`poi`. A definition that applies to several subject types counts toward every
corresponding limit.

## Step 1 — Lead the brainstorming

Read [the brainstorming rules](https://raw.githubusercontent.com/davialabs/scenario-kit/main/scenario-docs/brainstorming.md) in full, then
begin the interview. Ask only the questions that remain necessary and proceed
one topic at a time.

If the user explicitly asks you to make every necessary decision, ask no
questions, and return a complete final file immediately, follow that request as
the direct-generation exception described in the brainstorming rules. Do not
stop for a separate brief validation. Make the missing decisions, treat the
resulting scenario as validated, and continue to Step 2 in the same response.
This exception never waives the exact-placement or map-coverage checks above.

Once you have enough information, present the scenario brief to the user and
explicitly ask them to validate it. Until they do, remain in this step and
revise the brief with them.

## Step 2 — Build the final file

After the user explicitly validates the brainstorming, tell them:

> The scenario is validated. I will now prepare the final file.

Then read the following pages in full, in this exact order:

1. [Final JSON contract](https://raw.githubusercontent.com/davialabs/scenario-kit/main/scenario-docs/json-contract.md)
2. [Populate `story`](https://raw.githubusercontent.com/davialabs/scenario-kit/main/scenario-docs/populate-story.md)
3. [Define and populate statistics](https://raw.githubusercontent.com/davialabs/scenario-kit/main/scenario-docs/populate-statistics.md)
4. [Populate entities](https://raw.githubusercontent.com/davialabs/scenario-kit/main/scenario-docs/populate-entities.md)
5. [Populate landmarks](https://raw.githubusercontent.com/davialabs/scenario-kit/main/scenario-docs/populate-landmarks.md)
6. [Populate values for the existing map](https://raw.githubusercontent.com/davialabs/scenario-kit/main/scenario-docs/populate-map.md)
7. [Assign 3D assets](https://raw.githubusercontent.com/davialabs/scenario-kit/main/scenario-docs/populate-map-assets.md)
8. [3D asset catalog](https://raw.githubusercontent.com/davialabs/scenario-kit/main/scenario-docs/map-assets-catalog.md)
9. [Validate and produce the final JSON](https://raw.githubusercontent.com/davialabs/scenario-kit/main/scenario-docs/validate-and-output.md)

Populate a single JSON object in memory according to these pages. Do not show
any fragment, draft, or intermediate document. After completing the mechanical
validation required by the last page, give the user one downloadable `.json`
file. Only when attachments are unavailable may you provide one complete JSON
block to copy. Never deliver a Markdown or plain-text document.

## Optional step — Generate images

This step is not part of the final file. Only perform it if the user later asks
for images. Then read [Generate images](https://raw.githubusercontent.com/davialabs/scenario-kit/main/scenario-docs/generate-images.md) and
apply its single visual style without changing the validated scenario.
