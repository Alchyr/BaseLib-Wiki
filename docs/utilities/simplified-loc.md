---
title: Simplified Localization
parent: Utilities
---

Mods can use simplified localization shortcuts by either opting their entire mod's localization in or by starting a localization entry with `#`.

To opt-in your mod's entire localization, call `SimpleLoc.EnableSimpleLoc(ModId);` in your mod's initializer.

## Formatting

`!Var!` will be replaced with `{Var:diff()}`.

`?Var?` will be replaced with `{Var:inverseDiff()}`.

Certain names have shortened versions that can be used.

```c#
        { "D", "Damage" },
        { "CD", "CalculatedDamage" },
        { "B", "Block" },
        { "CB", "CalculatedBlock" },
        { "C", "Cards" },
        { "E", "Energy" },
        { "H", "Heal" }
```

`*Word` will be replaced with `[gold]Word[/gold].
Highlighting will end either on whitespace or end of "word" (symbol). You can surround text like `*Wo*rd` -> `[gold]Wo[/gold]rd` to explicitly control what is highlighted.

`(plural)` will be displayed if the most recently referenced variable is plural.
e.g. `Draw !C! card(s).` will become `Draw {Cards:diff()} card{Cards:plural:|s}.`