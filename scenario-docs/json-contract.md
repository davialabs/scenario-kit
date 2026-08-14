---
title: Final JSON contract
description: Follow the exact scenario JSON structure consumed by Davia.
---

The final result is a single JSON object. It contains only the scenario data
consumed by the importer. The map and technical fields are already handled by
the website.

## Root structure

The six root keys are required and are the only ones allowed:

```json
{
  "story": {
    "name": "<text>",
    "subtitle": "<text>",
    "premise": "<text>",
    "world_description": "<text>",
    "simulation_rules": "<text>",
    "start_date": "YYYY-MM-DDTHH:mm:ss"
  },
  "story_stats": [
    {
      "stat_id": "example_stat",
      "label": "<text>",
      "description": "<text>",
      "value_kind": "categorical",
      "domain": [{ "key": "example_value", "value": "<text>" }],
      "applies_to": ["cell"],
      "default_value": "example_value",
      "allow_dynamic_values": false,
      "value_colors": { "example_value": "#808080" }
    }
  ],
  "world_stats": {},
  "cell_values": [
    { "cell_id": 123, "stat_defs": { "example_stat": "example_value" } }
  ],
  "entities": [
    {
      "name": "<text>",
      "description": "<text>",
      "coordinates": [-43.1729, -22.9068],
      "is_featured": true,
      "stat_defs": {},
      "assets": { "mapAssetKey": "entity:person" }
    }
  ],
  "landmarks": [
    {
      "name": "<text>",
      "coordinates": [-43.1729, -22.9068],
      "stat_defs": {},
      "assets": { "mapAssetKey": "poi:settlement" }
    }
  ]
}
```

Replace every placeholder with scenario data. Keep
every key exactly as written. Do not invent aliases: `value` or `stats` never
replaces `stat_defs`, and a landmark never accepts `description`. The result is
the JSON object itself, not a document that describes it; root keys such as
`title`, `preamble`, or `sections` are invalid.

Hard collection limits: `story_stats` contains at most 30 items, `entities`
contains between 1 and 100 items, and `landmarks` contains between 0 and 100
items. Across `story_stats`, no more than 10 definitions may apply to
`playthrough`, 3 to `cell`, 10 to `entity`, and 4 to `poi`. A definition that
lists several values in `applies_to` counts toward each corresponding limit.
These are importer limits, not targets.

The importer distinguishes two outcomes:

- a **blocking error** means that the file cannot be imported safely. This
  includes a malformed contract, an impossible date, an unknown cell, an
  invalid statistic reference, an invalid map asset, or coordinates outside
  the story board;
- a **warning** means that the scenario remains importable, but useful optional
  information is missing. Davia shows the warning and asks the person importing
  the file to confirm before continuing.

The AI must still produce the complete structure described below. Warning
tolerance is a recovery mechanism, not permission to omit information on
purpose.

## `story`

```json
{
  "name": "The Fall of the Empire of Brazil",
  "subtitle": "Court, army, and republicans on the brink of rupture.",
  "premise": "On November 15, 1889, the Empire of Brazil enters a decisive crisis while the court, officers, and republican movements still face several possible outcomes.",
  "world_description": "Imperial authority remains legally in place, but its military support is fragmenting in Rio de Janeiro. The provinces, economic elites, and abolitionist networks approach the crisis with different interests.",
  "simulation_rules": "Military orders require a credible chain of command. News between Rio de Janeiro and the provinces travels through the communication methods available in 1889.",
  "start_date": "1889-11-15T08:00:00"
}
```

For a complete result, provide every field shown above. `name` and `start_date`
are technically required for import. Missing optional narrative fields are
accepted when the scenario can still run; missing `premise` or
`world_description` produces a warning. Visual direction does not belong in
`story` and must not be added to the final file.

Respect these maximum lengths: `name` 150 characters, `subtitle` 350,
`premise` 1,200, `world_description` 150,000, and `simulation_rules` 12,000.
`start_date` is always present and uses `YYYY-MM-DDTHH:mm:ss` without a time
zone.

## `story_stats`

Each definition has exactly these fields:

```json
{
  "stat_id": "country",
  "label": "Country",
  "description": "Country displayed as the primary identity on cells.",
  "value_kind": "categorical",
  "domain": [
    { "key": "brazil", "value": "Brazil" },
    { "key": "argentina", "value": "Argentina" }
  ],
  "applies_to": ["cell"],
  "default_value": "brazil",
  "allow_dynamic_values": false,
  "value_colors": {
    "brazil": "#D6B64C",
    "argentina": "#6C8FC7"
  }
}
```

Allowed values for `value_kind`: `categorical`, `ordinal`, `scalar`, and `text`.

Allowed values in `applies_to`: `playthrough`, `cell`, `entity`, and `poi`. A
statistic may contain several, but each value may appear only once in that
statistic.

`stat_id` is 1 to 64 characters. It starts with a lowercase ASCII letter and
then contains only lowercase ASCII letters, digits, or underscores. `label` is
1 to 120 characters and `description` is at most 1,000 characters.

Structure of `domain`:

- `categorical` or `ordinal`: ordered array of `{ "key", "value" }`;
- `scalar`: `{ "min": number, "max": number, "unit": "unit" }`;
- `text`: empty object `{}`.

For categorical and ordinal domains, every `key` and display `value` is 1 to
120 characters and every `key` is unique within that domain. For a scalar
domain, `max` is strictly greater than `min`, and an optional `unit` is at most
40 characters.

For a categorical or ordinal statistic, every `domain` item contains exactly
two fields: `key` and `value`. `key` is the technical value reused in
`default_value`, `stat_defs`, and `value_colors`; `value` is the label shown to
people. A `color` field inside a `domain` item is invalid. Put colors only in
the sibling `value_colors` object, keyed by the same `key` strings.

There is no 50-option limit. Include one option for every distinct value the
scenario needs, and do not merge confirmed territorial identities merely to
shorten the array.

`value_colors` contains one hexadecimal color for every categorical or ordinal
value that must be visualized. It is `{}` for a scalar or text statistic.

All initial values are strings, including numbers. Scalar bounds in `domain`
remain JSON numbers.

The array order is consumed. The first statistic whose `applies_to` contains
`cell` is the primary territorial identity displayed by default.

## Initial values

`world_stats` contains the world values:

```json
{
  "national_tension": "72"
}
```

The final result should make `cell_values` a bijection with the provided cells:

```json
[
  {
    "cell_id": 41,
    "stat_defs": {
      "country": "brazil",
      "political_stability": "fragile"
    }
  }
]
```

Every existing `cell_id` should appear exactly once. An unknown or duplicate
cell blocks the import. A missing cell produces a warning only when its values
can be completed safely from the statistic defaults. No other cell field is
allowed.

## `entities`

```json
{
  "name": "Deodoro da Fonseca",
  "description": "An influential marshal in Rio de Janeiro, ill and torn between his personal loyalty to the emperor and pressure from republican officers.",
  "coordinates": [-43.1729, -22.9068],
  "is_featured": true,
  "stat_defs": {
    "political_influence": "high",
    "health": "fragile"
  },
  "assets": {
    "mapAssetKey": "entity:military-commander"
  }
}
```

For a complete result, provide `name`, `description`, `is_featured`,
`stat_defs`, `assets.mapAssetKey`, and either `coordinates` or `cell_id`.
Coordinates are the preferred placement input. Missing optional values use safe
defaults and may produce a warning, notably when no entity is featured or an
asset has to be selected automatically. At least one entity is required. An entity `name` is 1 to 200
characters, its `description` is at most 500 characters, and
`assets.mapAssetKey` is 1 to 200 characters.

When the entity's starting position is knowable or can be authored deliberately,
provide `[longitude, latitude]` coordinates and omit `cell_id`; Davia derives
the containing cell. If no precise position can be justified, omit
`coordinates` and provide `cell_id` as a fallback. Copy that fallback from the
exact source field; never infer it from a numeric slug suffix. For example,
`{"cell_id":118,"slug":"germany-124"}` means the fallback is `118`. If both
fields are present, coordinates are authoritative.

## `landmarks`

```json
{
  "name": "Rio de Janeiro",
  "coordinates": [-43.1729, -22.9068],
  "stat_defs": {
    "urban_control": "contested"
  },
  "assets": {
    "mapAssetKey": "poi:capital"
  }
}
```

For a complete result, provide `name`, `coordinates`, `stat_defs`, and
`assets.mapAssetKey`. Use the actual
position of a known real place, never the supplied cell `center`, an arbitrary
point, or the center of its `bbox`. For an invented place, choose one deliberate
scenario-consistent point on the map; historical realism is not required. The
pair is always `[longitude, latitude]`. Omit `cell_id`; Davia derives the
containing cell from the point.

A landmark `name` is 1 to 200 characters and `assets.mapAssetKey` is 1 to 200
characters.

`coordinates` is required for every landmark. If even one landmark omits it,
the file is invalid and Davia rejects the import. This is a blocking error,
not a warning, and there is no cell-center fallback in the final-file contract.
An empty `landmarks` array is accepted with a warning.

The landmark list must also cover the inhabited and scenario-relevant map at
its displayed scale. Add density to the central action only after every major
relevant macroregion has enough meaningful, spatially separated landmarks to
avoid an accidental blank area. Do not use arbitrary filler to meet this
coverage requirement.

A landmark description is not allowed: the current product does not consume it.

## Prohibited fields

In particular, the JSON must not contain any:

- `id`, UUID, or generated identifier;
- `slug`;
- `map_id`, `bbox`, `basemap`, or artificially added time zone;
- GeoJSON, `geometry`, polygon coordinates, or copy of cells;
- `summary`, counter, or calculated distribution;
- `starts_on_map`, because every provided element starts on the map;
- `visual_elements`, `role`, `verbs`, or `opening_problem`;
- cover, portrait, image URL, or CDN manifest;
- `visual_style` or other image-generation direction;
- landmark description.

The importer creates UUIDs and slugs, associates the selected map, places
points, and calculates technical data without a second AI pass.

## Minimal complete example

This example shows the structure of a complete final result. Cell IDs in
`cell_values` and entity fallbacks come from the provided map. Davia derives
the cell for every supplied coordinate.

```json
{
  "story": {
    "name": "The Fall of the Empire of Brazil",
    "subtitle": "Court, army, and republicans on the brink of rupture.",
    "premise": "On November 15, 1889, the Empire of Brazil enters a decisive crisis while the court, officers, and republican movements still face several possible outcomes.",
    "world_description": "Imperial authority remains legally in place, but its military support is fragmenting in Rio de Janeiro. The provinces, economic elites, and abolitionist networks approach the crisis with different interests.",
    "simulation_rules": "Military orders require a credible chain of command. News between Rio de Janeiro and the provinces travels through the communication methods available in 1889.",
    "start_date": "1889-11-15T08:00:00"
  },
  "story_stats": [
    {
      "stat_id": "country",
      "label": "Country",
      "description": "Country displayed as the primary identity on cells.",
      "value_kind": "categorical",
      "domain": [
        { "key": "brazil", "value": "Brazil" },
        { "key": "argentina", "value": "Argentina" }
      ],
      "applies_to": ["cell"],
      "default_value": "brazil",
      "allow_dynamic_values": false,
      "value_colors": {
        "brazil": "#D6B64C",
        "argentina": "#6C8FC7"
      }
    },
    {
      "stat_id": "political_stability",
      "label": "Political stability",
      "description": "Immediate strength of the political order in a cell.",
      "value_kind": "ordinal",
      "domain": [
        { "key": "collapsed", "value": "Collapsed" },
        { "key": "fragile", "value": "Fragile" },
        { "key": "stable", "value": "Stable" }
      ],
      "applies_to": ["cell"],
      "default_value": "stable",
      "allow_dynamic_values": false,
      "value_colors": {
        "collapsed": "#9F3A38",
        "fragile": "#C98B3C",
        "stable": "#4F7F59"
      }
    },
    {
      "stat_id": "political_influence",
      "label": "Political influence",
      "description": "An entity's ability to influence political decisions.",
      "value_kind": "ordinal",
      "domain": [
        { "key": "low", "value": "Low" },
        { "key": "medium", "value": "Medium" },
        { "key": "high", "value": "High" }
      ],
      "applies_to": ["entity"],
      "default_value": "medium",
      "allow_dynamic_values": false,
      "value_colors": {
        "low": "#7C8798",
        "medium": "#C49A45",
        "high": "#8E4A3B"
      }
    },
    {
      "stat_id": "urban_control",
      "label": "Urban control",
      "description": "Degree of operational control over an urban landmark.",
      "value_kind": "ordinal",
      "domain": [
        { "key": "uncertain", "value": "Uncertain" },
        { "key": "secured", "value": "Secured" }
      ],
      "applies_to": ["poi"],
      "default_value": "uncertain",
      "allow_dynamic_values": false,
      "value_colors": {
        "uncertain": "#B9843F",
        "secured": "#4F7654"
      }
    },
    {
      "stat_id": "national_tension",
      "label": "National tension",
      "description": "Overall level of political tension in the country.",
      "value_kind": "scalar",
      "domain": { "min": 0, "max": 100, "unit": "%" },
      "applies_to": ["playthrough"],
      "default_value": "50",
      "allow_dynamic_values": false,
      "value_colors": {}
    }
  ],
  "world_stats": {
    "national_tension": "72"
  },
  "cell_values": [
    {
      "cell_id": 41,
      "stat_defs": {
        "country": "brazil",
        "political_stability": "fragile"
      }
    },
    {
      "cell_id": 42,
      "stat_defs": {
        "country": "argentina",
        "political_stability": "stable"
      }
    }
  ],
  "entities": [
    {
      "name": "Deodoro da Fonseca",
      "description": "An influential marshal in Rio de Janeiro, ill and torn between his personal loyalty to the emperor and pressure from republican officers.",
      "coordinates": [-43.1729, -22.9068],
      "is_featured": true,
      "stat_defs": {
        "political_influence": "high"
      },
      "assets": {
        "mapAssetKey": "entity:military-commander"
      }
    },
    {
      "name": "Pedro II",
      "description": "An aging emperor, personally respected but without political support organized enough to guarantee the regime's survival.",
      "cell_id": 41,
      "is_featured": true,
      "stat_defs": {
        "political_influence": "high"
      },
      "assets": {
        "mapAssetKey": "entity:head-of-state"
      }
    }
  ],
  "landmarks": [
    {
      "name": "Rio de Janeiro",
      "coordinates": [-43.1729, -22.9068],
      "stat_defs": {
        "urban_control": "uncertain"
      },
      "assets": {
        "mapAssetKey": "poi:capital"
      }
    },
    {
      "name": "Buenos Aires",
      "coordinates": [-58.3816, -34.6037],
      "stat_defs": {
        "urban_control": "secured"
      },
      "assets": {
        "mapAssetKey": "poi:city"
      }
    }
  ]
}
```
