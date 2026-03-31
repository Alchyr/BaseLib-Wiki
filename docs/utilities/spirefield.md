---
title: SpireField
parent: Utilities
---

SpireField is a simple way for mods to effectively add a new field to a preexisting class in the game.

To use it, define a SpireField like so:

`public static readonly SpireField<TargetType, FieldType> MyField = new(()=>default value);`

This field can be placed in any class; you are recommended to place it somewhere related to the field's purpose.

Getting and setting the field can be done like so, given the following example:

```c#
public class SpecialNumberThing {
    public static readonly SpireField<CardModel, int> SpecialNumber = new(() => 0); //Default value of 0
}

...
CardModel model = ModelDb.Card<Bash>();
int val = SpecialNumberThing.SpecialNumber.Get(model);

SpecialNumberThing.SpecialNumber.Set(model, 5);
```

You can also get or set the value using an indexer.

```c#
CardModel model = ModelDb.Card<Bash>();
int val = SpecialNumberThing.SpecialNumber[model];

SpecialNumberThing.SpecialNumber[model] = 5;
```

## SavedSpireField

SavedSpireField is a class inheriting from SpireField with the additional functionality of saving and loading its value when attached to a model.

It is used almost identically to a normal SpireField; the only difference is that you have to provide a name which will be used for the save data. SavedSpireField does not support ALL types, only the same types allowed by the basegame's `[SavedProperty]` attribute.

- int
- bool
- string
- int[]
- ModelId
- SerializableCard
- SerailizableCard[]
- List<SerailizableCard>

A SavedSpireField might look like the following:

```c#
public class SpecialNumberThing {
    public static readonly SavedSpireField<CardModel, int> SpecialNumber = new(() => 0, "MYMOD_SPECIAL_NUMBER");
}
```

Try to ensure that the name will be something unique to your mod to avoid conflicts.

### Technical

SpireField works by wrapping a ConditionalWeakTable. A ConditionalWeakTable can be used directly for similar results, but SpireField provides some utility to make it simpler to use, such as having a default value.