---
title: CustomSingletonModel
parent: Custom Models
---

`CustomSingletonModel` is a singleton class (only one instance) that is used to passively receive hooks. The constructor parameters determine if it will receive combat hooks, run hooks, or both.

```cs
public class HealOnPlaySingleton() : CustomSingletonModel(true, false)
{
    public override async Task AfterCardPlayed(PlayerChoiceContext choiceContext, CardPlay cardPlay)
    {
        await CreatureCmd.Heal(cardPlay.Card.Owner.Creature, 1);
    }
}
```