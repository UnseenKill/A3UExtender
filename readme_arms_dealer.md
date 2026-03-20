# A3UExtender - Arms Dealer

Adding stuff to the arms dealer requires a dedicated mod _you must write
yourself._

DO NOT extract A3U .pbos and repack them or create workshop mods that
replicate the whole `hals_store` addon, or worse, the whole mod.

## Terminology

- **Addon** - a functionally segregated part of a mod; always has a `config.cpp`
and _may_ result in a `.pbo` file.
- **Mod** - a collection of _addons_ that can be uploaded to the workshop.

## Requirements

- Arma 3 - The game. Duh.
- [Arma 3 Tools][arma-3-tools] - For publishing your extender.
- [HEMTT][hemtt] - Quite opinionated build system - to build your extender.
- [CBA][cba] - Community Based Addons for Arma 3

## Setup

### Initialize project

We'll crate an extender that adds stuff from the Reaction Forces DLC to the
store.

Follow project initialization instructions from the
[HEMTT book][hemtt-command-new]. For demonstration purposes, we'll create a
project called `my-arms-dealer-extension`.

![HEMTT new command](images/arms_dealer_hemtt_new.png)

That should leave you with a project structure
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
> `git commit` is your friend. Regularly commit changes made to the project;
> especially _before_ bigger changes to the project.
>
> The initial project structure is uncommitted. Time for your first commit!

### Main addon

The main addon should define the mod itself, dependencies and globally used
stuff for your mod; _ideally_, it's a collection of meta-data, no code.

Let's set up our main addon.

> [!IMPORTANT]
> Please suppress the urge to simply copy/paste stuff from below. Try to
> understand what each file does and especially, where you'll need to make
> adjustments for _your_ mod.

1. Create `addons/main/$PBOPREFIX$` with this content (`z\a3uemade` is a 
   combination of what you've entered [above](#initialize-project) as "prefix"
   and "main prefix" during initialization):
    ```
    z\a3uemade\addons\main
    ```

2. Create `addons/main/script_component.hpp` with this content:
    ```sqf
    #define COMPONENT main
    #include "script_mod.hpp"
    #include "script_macros.hpp"
    ```

3. Create `addons/main/script_mod.hpp`:
    ```sqf
    // COMPONENT should be defined in the script_component.hpp and included BEFORE this hpp
    #define PREFIX a3uemade

    #include "script_version.hpp"

    #define VERSION     MAJOR.MINOR
    #define VERSION_STR MAJOR.MINOR.PATCHLVL.BUILD
    #define VERSION_AR  MAJOR,MINOR,PATCHLVL,BUILD

    // MINIMAL required Arma version for the Mod. Components can specify others..
    #define REQUIRED_VERSION 2.20
    ```

4. Create `addons/main/script_version.hpp`:
    ```sqf
    #define MAJOR 1
    #define MINOR 0
    #define PATCHLVL 0
    #define BUILD 000000
    ```

5. Create `addons/main/script_macros.hpp`:
    ```sqf
    // The mod's main prefix as defined as "main prefix" during initialization
    #define MAINPREFIX z
    // Community Based Addons - common macros
    #include "\x\cba\addons\main\script_macros_common.hpp"

    // All your global mod-specific macros go after this line
    /* ... */
    ```

6. Create `addons/main/config.cpp` with this content:

    ```sqf
    #include "script_component.hpp"

    class CfgPatches {
        class ADDON {
            name = CSTRING(component);
            units[] = {};
            weapons[] = {};
            requiredVersion = REQUIRED_VERSION;
            requiredAddons[] = {
                // Add dependency to Antistasi Ultimate
                "A3A_ultimate",
                // We're going to be adding stuff from RF
                "RF_Weapons"
            };
            // Make this mod load _only_ if all the above dependencies are satisfied
            skipWhenMissingDependencies = 1;
            author = CSTRING(Author);
            authors[] = {};
            url = CSTRING(URL);
            VERSION_CONFIG;
        };
    };
   ```

7. Finally, create `addons/main/stringtable.xml`:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project name="A3UEMADE">
        <Package name="Main">
            <Key ID="STR_A3UEMADE_Main_Author">
                <Original>UnseenKill/goreSplatter</Original>
                <German>UnseenKill/goreSplatter</German>
            </Key>
            <Key ID="STR_A3UEMADE_Main_Component">
                <Original>[A3UE] MADE - Main component</Original>
                <German>[A3UE] MADE - Hauptkomponente</German>
            </Key>
            <Key ID="STR_A3UEMADE_Main_URL">
                <Original>https://github.com/</English>
            </Key>
        </Package>
    </Project>
    ```

> [!TIP]
> `hemtt check` is your friend. Run this command often; especially after bigger
> changes. A good project is an error- and warning-free project! Run it now.

[arma-3-tools]: https://store.steampowered.com/app/233800/Arma_3_Tools/
[cba]: https://steamcommunity.com/sharedfiles/filedetails/?id=450814997
[hemtt]: https://hemtt.dev/
[hemtt-command-new]: https://hemtt.dev/commands/new.html
