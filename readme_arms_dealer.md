# A3UExtender - Arms Dealer

Adding stuff to the arms dealer requires a dedicated addon _you must write
yourself._

DO NOT extract A3U .pbos and repack them or create workshop addons that
replicate the whole `hals_store` addon, or worse, the whole mod.

## Requirements

- Arma 3 - The game. Duh.
- [Arma 3 Tools][arma-3-tools] - For publishing your extender.
- [HEMTT][hemtt] - Quite opinionated build system - to build your extender.

## Setup

### Initialize project

Follow project initialization instructions from the
[HEMTT book][hemtt-command-new]. That should leave you with a project structure
like this:

```
<my-arms-dealer-project> /
   | .git
   | .hemtt
   +---- project.toml
   | addons
   | .gitignore
   | LICENSE
```

> [!TIP]
> `hemtt check` is your friend. Run this command often; especially after bigger
> changes. A good project is an error- and warning-free project!

[arma-3-tools]: https://store.steampowered.com/app/233800/Arma_3_Tools/
[hemtt]: https://hemtt.dev/
[hemtt-command-new]: https://hemtt.dev/commands/new.html
