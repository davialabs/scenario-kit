# Davia Scenario Kit

This public repository contains the instructions an AI assistant follows to
help a creator design a Davia game and prepare its final scenario file.

## Use the scenario instructions

Send this entry point to the AI assistant together with the map context copied
from Davia:

https://raw.githubusercontent.com/davialabs/scenario-kit/main/scenario-docs/README.md

The files in [`scenario-docs/`](scenario-docs/) form one coherent contract.
The assistant starts with `scenario-docs/README.md` and follows the linked pages
in their specified order.

English is the canonical language and lives directly in `scenario-docs/`.
The calling prompt tells the assistant which language to use with the creator;
the JSON field names and contract identifiers remain unchanged.

The repository may later add examples, validation packages, and contract tests
without mixing them into the scenario instructions.

Learn more about Davia at [davia.ai](https://davia.ai/).
