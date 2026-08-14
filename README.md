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

English is the default language and lives directly in `scenario-docs/`. Future
translations can live in locale folders such as `scenario-docs/fr/` while
keeping the same filenames and links.

The repository may later add examples, validation packages, and contract tests
without mixing them into the scenario instructions.

Learn more about Davia at [davia.ai](https://davia.ai/).
