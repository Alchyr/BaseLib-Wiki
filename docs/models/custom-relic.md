---
title: CustomRelicModel
parent: Custom Models
---

Relics must exist in a relic pool. When creating a card using `CustomRelicModel`, mark it with the `[Pool]` annotation to determine what pool to add it to. The `[Pool]` annotation can be inherited.

If your class does not inherit `CustomRelicModel`, you will need to add a default constructor that calls `CustomContentDictionary.AddModel` for the `[Pool]` annotation to function.

```c#
[Pool(typeof(IroncladRelicPool))]
public class DummyRelic : CustomRelicModel
{

}
```

## RelicModel Documentation

`PackedIconPath`, `PackedIconOutlinePath`, and `BigIconPath` can be overridden to load a custom filepath.

Otherwise, they will look for images under `images/atlases/relic_atlas.sprites/`, `images/atlases/relic_outline_atlas.sprites/`, and `images/relics/` respectively.

The first two will look for a `.tres` using the `IconBaseName` method, while `images/relics` will be looking for a `.png`. When overriding these methods, you can return a `.png` for all three.

TODO