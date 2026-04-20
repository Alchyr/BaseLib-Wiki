---
title: Hooks and Mechanics
nav_order: 3
---

Hooks and mechanics added by BaseLib that can be used by any mod.

## Hooks

* `IHealAmountModifier`: `ModifyHealAdditive` and `ModifyHealMultiplicative`
* `IMaxHandSizeModifier`: `ModifyMaxHandSize` and `ModifyMaxHandSizeLate`
* `IHealthBarForecastSource`: Supported directly by powers; other implementations must be registered with `HealthBarForecastRegistry`


## Keywords

Add these to your card's CanonicalKeywords for them to function.

| Name         | Description                                                                                                                    |
|:-------------|:-------------------------------------------------------------------------------------------------------------------------------|
| Purge        | Cards with this keyword will be removed from your deck when played.                                                            |


## Dynamic Variables

Add these to your card's CanonicalVariables and to their card text.

| Name         | Card Text                                    | Description                                                                              |
|:-------------|:---------------------------------------------|:-----------------------------------------------------------------------------------------|
| Persist      | [gold]Persist[/gold] {Persist:diff()}.       | Card returns to hand when played a certain number of times each turn.                    |
| Exhaustive   | [gold]Exhaustive[/gold] {Exhaustive:diff()}. | Card Exhausts after a certain number of uses each combat.                                |
| Refund       | [gold]Refund[/gold] {Refund:diff()}.         | Card returns some of its energy cost after play. Will not refund more than energy spent. |