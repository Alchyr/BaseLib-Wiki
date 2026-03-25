---
title: CustomOrbModel
parent: Custom Models
---

Orbs are the Defect's core mechanic. `CustomOrbModel` provides hooks for custom icons, sprites, sound effects, and optional inclusion in the random orb pool used by cards like Chaos and Rainbow.

## Icon and Sprite

Override `CustomIconPath` to provide a custom icon used in tooltips and the orb slot UI.

Override `CustomSpritePath` to point to a custom Godot scene for the orb's in-slot visual. The scene must have a `SpineSkeleton` node with an `idle_loop` animation.

Alternatively, override `CreateCustomSprite()` to build the orb visual programmatically. For example by compositing existing orb scenes with color tints and scale adjustments. If `CreateCustomSprite()` returns a non-null value, it takes precedence over `CustomSpritePath`.
```c#
public override Node2D? CreateCustomSprite()
{
    var container = new Node2D();
    string lightningPath = SceneHelper.GetScenePath("orbs/orb_visuals/lightning_orb");
    Node2D lightning = PreloadManager.Cache.GetScene(lightningPath)
        .Instantiate<Node2D>(PackedScene.GenEditState.Disabled);
    new MegaSprite(lightning.GetNode("SpineSkeleton"))
        .GetAnimationState().SetAnimation("idle_loop");
    lightning.Modulate = new Color(0.8f, 0.1f, 0.0f, 1.0f);
    lightning.Scale = new Vector2(1.1f, 1.1f);
    container.AddChild(lightning);
    return container;
}
```

## Sound Effects

Override `CustomPassiveSfx`, `CustomEvokeSfx`, and `CustomChannelSfx` to provide FMOD event paths for custom sounds. If not overridden, the debug audio system will look for `.mp3` files named `{id_lowercase}_passive.mp3`, `{id_lowercase}_evoke.mp3`, and `{id_lowercase}_channel.mp3`.

## Random Orb Pool

By default, custom orbs will not appear from cards like Chaos or Rainbow. Set `IncludeInRandomPool` to `true` to include your orb in the random pool alongside the five vanilla orbs, giving it equal weight.
```c#
public override bool IncludeInRandomPool => true;
```

## Example
```c#
public sealed class FireOrb : CustomOrbModel
{
    public override Color DarkenedColor => new Color("996510");
    public override string? CustomIconPath => "res://MyMod/images/orbs/fire_orb.png";
    public override bool IncludeInRandomPool => true;
    public override decimal PassiveVal => ModifyOrbValue(2m);
    public override decimal EvokeVal => ModifyOrbValue(4m);

    public override Node2D? CreateCustomSprite()
    {
        var container = new Node2D();
        // back layer
        string lightningPath = SceneHelper.GetScenePath("orbs/orb_visuals/lightning_orb");
        Node2D lightning = PreloadManager.Cache.GetScene(lightningPath)
            .Instantiate<Node2D>(PackedScene.GenEditState.Disabled);
        new MegaSprite(lightning.GetNode("SpineSkeleton"))
            .GetAnimationState().SetAnimation("idle_loop");
        lightning.Modulate = new Color(0.8f, 0.1f, 0.0f, 1.0f);
        lightning.Scale = new Vector2(1.1f, 1.1f);
        container.AddChild(lightning);
        // front layer
        string glassPath = SceneHelper.GetScenePath("orbs/orb_visuals/glass_orb");
        Node2D glass = PreloadManager.Cache.GetScene(glassPath)
            .Instantiate<Node2D>(PackedScene.GenEditState.Disabled);
        new MegaSprite(glass.GetNode("SpineSkeleton"))
            .GetAnimationState().SetAnimation("idle_loop");
        glass.Modulate = new Color(0.95f, 0.75f, 0.2f, 1.0f);
        container.AddChild(glass);
        return container;
    }

    public override async Task BeforeTurnEndOrbTrigger(PlayerChoiceContext choiceContext)
        => await Passive(choiceContext, null);

    public override async Task Passive(PlayerChoiceContext choiceContext, Creature? target)
    {
        Trigger();
        var enemies = CombatState.GetOpponentsOf(Owner.Creature)
            .Where(e => e.IsHittable).ToList();
        if (enemies.Count == 0) return;
        Creature passiveTarget = target ?? Owner.RunState.Rng.CombatTargets.NextItem(enemies)!;
        await CreatureCmd.Damage(choiceContext, new[] { passiveTarget }, PassiveVal, ValueProp.Unpowered, Owner.Creature);
        await PowerCmd.Apply<BurnPower>(passiveTarget, PassiveVal, Owner.Creature, null);
    }

    public override async Task<IEnumerable<Creature>> Evoke(PlayerChoiceContext choiceContext)
    {
        var enemies = CombatState.GetOpponentsOf(Owner.Creature)
            .Where(e => e.IsHittable).ToList();
        if (enemies.Count == 0) return enemies;
        Creature target = Owner.RunState.Rng.CombatTargets.NextItem(enemies)!;
        PlayEvokeSfx();
        await CreatureCmd.Damage(choiceContext, new[] { target }, EvokeVal, ValueProp.Unpowered, Owner.Creature);
        return enemies;
    }
}
```
