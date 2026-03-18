---
title: Creature Visuals
parent: Utilities
---

BaseLib contains a few utilities to help with the creation of and increase the variety of options available to use for creature visuals. This page will explain those as well as some more general information regarding creating creature visuals.

## NCreatureVisuals

This is the script (effectively the base type) that all creature visuals must be. However, when creating a scene through Godot's editor, scripts from other assemblies are not accessible. There are a few options to work around this.

* Define a class inheriting from NCreatureVisuals marked with the `[GlobalClass]` attribute and use it as the script for your scene. You can add additional functionality in this class as well if you wish.
* Use `GodotUtils.CreatureVisualsFromScene` to convert a scene that isn't using NCreatureVisuals as its script into an NCreatureVisuals instance. `CustomCharacterModel` will do this by default if the scene provided for its visuals does not inherit from NCreatureVisuals.
* Define the visuals scene manually by editing a .tscn file in text, which allows you to set its script to NCreatureVisuals. You will not be able to edit this scene in the Godot editor.

An `NCreatureVisuals` follows a specific setup and has required named nodes.

## Spine

