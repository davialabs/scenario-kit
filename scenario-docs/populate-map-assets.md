---
title: Assign 3D assets
description: Match every entity and landmark to an existing 3D key.
---

Complete this step after designing the entities and landmarks. The choice of a
model must not artificially influence their selection.

Read [the asset catalog](https://raw.githubusercontent.com/davialabs/scenario-kit/main/scenario-docs/map-assets-catalog.md) in full. For every
entity and landmark:

1. choose the existing key whose representation is the best match;
2. always use an `entity:*` key for an entity;
3. always use a `poi:*` key for a landmark;
4. copy the exact key into `assets.mapAssetKey`;
5. do not invent, shorten, or translate any key.

If no specialized model is suitable, choose the closest generic model, such as
`entity:person`, `entity:group`, `poi:city`, or `poi:settlement`.

Examples:

- Deodoro da Fonseca → `entity:military-commander` ;
- Hector → `entity:general` ;
- Rio de Janeiro → `poi:capital` ;
- Achaean camp → `poi:camp`;
- Troy, when no more precise model fits → `poi:fortress`.

These matches illustrate how to choose a representation. They do not make
these keys mandatory for another scenario.

The link is final in the JSON. No other AI model should be called after import
to choose or correct the assets.
