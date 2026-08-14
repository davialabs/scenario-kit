---
title: Populate values for the existing map
description: Assign scenario statistics to the cells already provided.
---

The map is never generated or copied into the final JSON. This step only adds
the scenario's statistic values to the cells already provided.

## Build the bijection

Read the list of `cell_id` values from the source map. Create exactly one
`cell_values` entry for every cell in the scenario and no additional entries.

Each entry contains only:

```json
{
  "cell_id": 41,
  "stat_defs": {
    "country": "brazil",
    "political_stability": "fragile"
  }
}
```

Do not copy the cell's name, description, type, center, geometry, or any other
property.

## Paint identity and states

Every cell receives exactly one value from the first cell statistic in
`story_stats`. This value is the primary identity that takes precedence on the
map.

Then add the relevant secondary states without mixing their domains. A
contested cell retains its primary identity and, when defined, receives a
secondary control or stability value.

Example — Brazil: a cell may retain `country: "brazil"` while receiving
`political_stability: "fragile"`. Example — Troy: a cell may retain
`kingdom: "kingdom_of_troy"` while receiving `war_status: "besieged"`.

The first field answers, "Which country or kingdom is written on this area?"
The second answers, "What state is this area in?" Never swap these answers.

## Never rewrite the map

Do not invent any `cell_id`. Do not sort, merge, or renumber cells. Do not
produce GeoJSON, `bbox`, or geometry, even to simplify validation.

Source geometry may be read temporarily to verify entity and landmark
coordinates. It must never appear in the final JSON.
