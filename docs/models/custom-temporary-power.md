---
title: CustomTemporaryPowerModel
parent: Custom Models
---

The `CustomTemporaryPowerModel` is designed to allow you to create temporary versions of any power similar to the base game. It also provides some additional features.

The base game only has Temporary Power Models for `Strength`, `Dexterity` and `Focus`. If you intend to create a temporary power based on them, you can inherit either from the respective base game class or use this one.

Even if a temporary power is intended to be applied through various models, every Model should have its own temporary power version to properly show tooltips.

## Wrapper

For ease of use BaseLib provides a wrapper for the `CustomTemporaryPowerModel` called `CustomTemporaryPowerModelWrapper`. It will provide default implementations for most things. You can still manually override anything you want.

The wrapper takes two Types in the class definition:
 - **TModel**: The Model that will apply the power. Information from it will be displayed in the powers tooltip.
 - **TPower**: The Power that will be applied.

```c#
public class FooPower : CustomTemporaryPowerModelWrapper<Anticipate, DexterityPower> {}
```

## CustomTemporaryPowerModel

If you do not want to use the wrapper, you can directly inherit `CustomTemporaryPowerModel`.
The properties shown in the example must be overridden.
```c#
public class FooPower : CustomTemporaryPowerModel
{
    public override AbstractModel OriginModel => ModelDb.Card<Anticipate>();
    public override PowerModel InternallyAppliedPower => ModelDb.Power<StrengthPower>();
    protected override Func<PlayerChoiceContext, Creature, decimal, Creature?, CardModel?, bool, Task> ApplyPowerFunc=> PowerCmd.Apply<FooPower>;
}
```

## CustomTemporaryPowerModel Documentation

`CustomBigBetaIconPath`, `CustomPackedIconPath` and `CustomBigIconPath` should be overriden to load a custom filepath.<br>
*Wrapper provides default implementations*

`Title`, `Description` and `SmartDescriptionLockKey` should be overriden to point to localization keys<br>
*Wrapper provides default implementations*

`ExtraHoverTips` should be overridden to show information about the power and origin model.<br>
*Wrapper provides default implementation*

`InvertInternalPowerAmount` can be used to invert the applied power amount from the applied temporary power amount.

`CanonicalVars` should not be overriden. If you need to override this, copy the contents of the base implementation into your override.

### Additional Features

`LastForXExtraTurns` provides an option to have the power last for multiple turns.

`UntilEndOfOtherSideTurn` will change the removal of the power from the end of the creatures turn to the end of its enemies turn.

## Examples copying base game power behaviours

#### Focused Strike
```c#
public class FooPower : CustomTemporaryPowerModelWrapper<FocusedStrike, FocusPower> {}
```
#### Speed Potion
```c#
public class FooPower : CustomTemporaryPowerModelWrapper<SpeedPotion, DexterityPower> {}
```
#### Piercing Wail
```c#
public class FooPower : CustomTemporaryPowerModelWrapper<PiercingWail, StrengthPower>
{
    protected override bool InvertInternalPowerAmount => true;
    public override PowerType Type => PowerType.Debuff;
}
```