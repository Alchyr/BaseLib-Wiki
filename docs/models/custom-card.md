---
title: CustomCardModel
parent: Custom Models
---

Cards must exist in a card pool. When creating a card using `CustomCardModel`, mark it with the `[Pool]` annotation to determine what pool to add it to. The `[Pool]` annotation can be inherited.

Using `CustomCardModel` is not required, as you can just mark your classes with the `ICustomModel` interface instead. However, if your class does not inherit `CustomCardModel`, you will need to add a default constructor that calls `CustomContentDictionary.AddModel` for the `[Pool]` annotation to function.

```c#
[Pool(typeof(IroncladCardPool))]
public class StrikeOddmelt() : CustomCardModel(1, CardType.Attack, CardRarity.Basic, TargetType.AnyEnemy)
{
    protected override HashSet<CardTag> CanonicalTags => [CardTag.Strike];
    protected override IEnumerable<DynamicVar> CanonicalVars => [new DamageVar(6, ValueProp.Move)];

    protected override async Task OnPlay(MegaCrit.Sts2.Core.GameActions.Multiplayer.PlayerChoiceContext choiceContext, MegaCrit.Sts2.Core.Entities.Cards.CardPlay play)
    {
        //CommonActions is a utility class provided by BaseLib to shorten some common card/relic effects.
        await CommonActions.CardAttack(this, play.Target).Execute(choiceContext);
    }

    protected override void OnUpgrade()
    {
        DynamicVars.Damage.UpgradeValueBy(3m);
    }
}
```

## CardModel Documentation

TODO