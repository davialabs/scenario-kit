---
title: Map asset catalog
description: Use only the current map asset keys and parameter values for entities and landmarks.
---

The authoritative catalog is generated from Davia's current, listed asset rows:

[`https://cdn.davia.ai/tale/map-assets/catalog.json`](https://cdn.davia.ai/tale/map-assets/catalog.json)

This URL is stable. The asset publication pipeline regenerates its JSON
automatically whenever the current listed assets change. Do not maintain a
second static list of asset keys in the Scenario Kit.

Read that JSON file in full before assigning assets. Each catalog entry contains
only the fields needed to choose a valid asset:

```json
{
  "key": "entity:samurai",
  "description": "Map asset representing a samurai.",
  "defaultParameters": { "variant": "a", "palette": "earth" },
  "parameters": {
    "variant": { "a": "...", "b": "...", "c": "..." },
    "palette": { "earth": "...", "blue": "..." }
  }
}
```

Write the selected asset in the final file with this shape:

```json
"mapAsset": {
  "key": "entity:samurai",
  "parameters": { "variant": "b", "palette": "blue" }
}
```

Choose only a listed `key`. The key prefix must match the feature type:
`entity:*` for entities and `poi:*` for landmarks.

`parameters` contains the closed set of configurable values for that asset.
Choose only names and values listed under the entry's `parameters` object. Omit
the entire object when the documented defaults fit, or when the entry has no
parameters. Never copy `defaultParameters` into the final file merely to make
defaults explicit.

The catalog deliberately hides internal versions. Davia resolves the listed
current version during import and stores the exact version in the scenario so a
later asset release cannot change an existing game.

If no specialized asset fits, choose the closest generic listed asset. Never
invent, shorten, translate, or infer a key or parameter value.
