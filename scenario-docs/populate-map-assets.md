---
title: Assign map assets
description: Match every entity and landmark to an existing map asset key and its allowed parameters.
---

Complete this step after designing the entities and landmarks. The choice of a
model must not artificially influence their selection.

Read [the asset instructions](https://raw.githubusercontent.com/davialabs/scenario-kit/main/scenario-docs/map-assets-catalog.md)
and [the current generated catalog](https://cdn.davia.ai/tale/map-assets/catalog.json)
in full. For every entity and landmark:

1. choose the existing key whose representation is the best match;
2. always use an `entity:*` key for an entity;
3. always use a `poi:*` key for a landmark;
4. always use the `mapAsset` JSON shape;
5. choose only listed parameter values and omit parameters
   that should keep their documented default;
6. do not invent, shorten, or translate any key or parameter value.

Every catalog entry uses `mapAsset` directly on the entity or landmark:

```json
"mapAsset": {
  "key": "entity:samurai",
  "parameters": { "variant": "b", "palette": "blue" }
}
```

If no specialized model is suitable, choose the closest generic model, such as
`entity:person`, `entity:group`, `poi:city`, or `poi:settlement`.

Examples:

- Deodoro da Fonseca → `entity:military-commander` ;
- Hector → `entity:general` ;
- Rio de Janeiro → `poi:capital` ;
- Achaean camp → `poi:camp`;
- Troy, when no more precise model fits → `poi:fortress`.
- a Sengoku samurai → `entity:samurai`, with the closest listed `variant` and
  `palette` values.

These matches illustrate how to choose a representation. They do not make
these keys mandatory for another scenario.

The link is final in the JSON. No other AI model should be called after import
to choose or correct the assets.
