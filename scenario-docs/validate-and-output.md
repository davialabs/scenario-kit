---
title: Validate and produce the final JSON
description: Run mechanical checks before returning a single JSON object.
---

Before producing any output, run a small validation with Python or an
equivalent tool. Do not rely on visually reviewing the JSON.

## Mechanical checks

At a minimum, the program must verify:

1. the JSON serializes, parses, and remains identical;
2. all six root keys are present and no others exist; `story_stats` has at most
   30 items, `entities` has 1 to 100, and `landmarks` has 0 to 100; no more
   than 10 stat definitions apply to `playthrough`, 3 to `cell`, 10 to
   `entity`, and 4 to `poi`;
3. no prohibited technical field has been added;
4. `start_date` is a valid calendar date and time;
5. all `stat_id` values are unique, contain at most 64 characters, start with a
   lowercase ASCII letter, and otherwise contain only lowercase ASCII letters,
   digits, or underscores;
6. every `value_kind`, `domain`, `default_value`, and `applies_to` is valid;
   every subject in `applies_to` is unique; every categorical or ordinal domain
   item contains exactly `key` and `value`, its keys are unique, and colors
   appear only in the sibling `value_colors` object; every scalar has
   `max > min`;
7. all categorical and ordinal values belong to their domain unless
   `allow_dynamic_values` genuinely permits otherwise;
8. all scalar values fall within their bounds;
9. each value is used only on a type listed in `applies_to`;
10. the first cell statistic is categorical and serves as the primary
    identity;
11. the `cell_id` values in `cell_values` form exactly the same set as the
    source cells, with no duplicates or omissions;
12. every cell has a resolved value for the primary identity;
13. every entity provides coordinates when a precise position is available;
    otherwise it provides an existing source `cell_id` copied from the exact
    field rather than inferred from a slug suffix, name, list position,
    `center`, or `bbox`; every landmark omits `cell_id`;
14. every landmark has a distinct, deliberate `[longitude, latitude]` pair;
    known real places use their actual geographic positions rather than cell
    centers or `bbox` centers, while fictional places use deliberate points
    consistent with the scenario; every supplied entity or landmark coordinate
    falls within the story board when source geometry is available;
15. every entity with a knowable or deliberately authored starting position has
    coordinates appropriate to its situation at `start_date`; people, armies,
    fleets, governments, and groups were not bulk-placed at generic cell points;
16. entity names are unique, and landmark names are unique within their own
    list;
17. every `mapAssetKey` exists in the catalog and has the correct prefix;
18. at least one entity is marked `is_featured`;
19. no geometry, UUID, `bbox`, `map_id`, slug, cover, visual direction, or
    calculated data is present.

The validator does not generate UUIDs or reconstruct the map. It uses source
geometry only as read-only input when necessary.

If a check fails, correct the working object and rerun every check. Never show
partial or invalid JSON.

When correcting a file after an import attempt, require the original complete
board context alongside the reported issues. Preserve the validated scenario
and the current file; do not regenerate from a generic skeleton. Never copy a
template coordinate such as `[0, 0]`, guess replacement coordinates, or fill
missing cells from their numeric IDs. A missing-cell warning may be accepted
unchanged when the applicable statistic defaults already express the intended
state. After any repair, rerun this entire checklist because schema validation
may reveal additional independent issues only after an earlier malformed field
is corrected.

## Spatial coverage checks

Use the landmark coordinates to review the whole map, not only the core action.
Group the inhabited and scenario-relevant extent into meaningful macroregions
from the source landmasses and cells, then count and inspect the landmarks in
each one. Flag any major relevant macroregion with no landmark, any large
inhabited empty area, any unjustified isolated token point, and any extreme
cluster that consumed the coverage needed elsewhere. When the scale and
collection limit allow it, prefer at least two separated landmarks in each
major macroregion.

Correct accidental gaps with meaningful places at the map's scale, then rerun
the coordinate and cell checks. Do not add arbitrary filler, and do not force
equal density where the scenario genuinely concentrates activity.

Examples of failures to correct automatically before delivery:

- the source cells are `[41, 42, 43]`, but `cell_values` contains only `41` and
  `42`;
- an entity uses `poi:city` instead of an `entity:*` key;
- the first cell statistic contains `Strong`, `Weak`, and `Contested` even
  though the confirmed identity was `Country`;
- a cell in Troy receives `Besieged` as its `kingdom` value instead of in
  `war_status`.

## Editorial checks

Also verify the following without adding text to the result:

- one language for all human-readable content;
- a premise independent of the chosen entity;
- a `world_description` limited to world context;
- `simulation_rules` limited to scenario-specific constraints;
- no repetition of Davia's native principles;
- every entity and landmark within the map;
- no landmark left at a generic cell center when its real or authored position
  can be supplied;
- no large inhabited or scenario-relevant map region left accidentally empty;
- no unjustified landmark pileup caused by concentrating almost every point in
  one theater;
- landmarks appropriate to the map's scale;
- a nominal, homogeneous primary identity distinct from secondary states.

## Deliver to the user

When speaking with the user, always call the result **the scenario's final
file**, not "the JSON," unless the user asks for the format name.

After every check passes:

- if you can create an attachment, create one downloadable `.json` file and
  write only: "Download this file: it is the scenario's final file.";
- otherwise, write only: "Copy the entire text below: it is the scenario's
  final file.", then provide one complete `json` block.

The attachment itself ends in `.json`. Never create a `.md` or `.txt` file and
never wrap the JSON inside a document, explanation, or second JSON envelope.

Do not add a summary, comment, or second version. Never truncate, split, or
replace cells with ellipses.
