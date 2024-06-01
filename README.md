Interested in following along with development of any of my modules? Join the [Discord server](https://discord.gg/QAG8eWABGT). 

# Items with Spells DnD5e

This module aims to allow users to attach spells to items in such a way that when the parent item is added to an actor, the spells attached come with, pre-configured with consumption overrides and other small QOL changes.

It also adds a new display of spell-attached items to the Spellbook.

## Features

This module does not replace or augment any core item workflow logic, only allows a way to signify that some Items also add spells to actors when added to an actor.

Recommended Use Cases:
- Magic Items which have Spells attached to them.
- Features which specifically add spells that have their own limited uses, not meant to be used with spell slots.

Not Recommended for:
- Class Spell Lists
- Subclass Specific Spells (e.g. "Always Prepared" spells)

The scope of this module is intentionally limited and will not fully replace the edge-cases that Magic Items attempts to accommodate.

### Adding Spells to Items

Only works for Unowned Items, and only for non-spells.

A new Tab is added to the Item Sheet: "Spells"

![Drag and Drop the "Web" spell from the SRD Compendium to add it to a "Wand of Web" item.](https://user-images.githubusercontent.com/7644614/190871191-9255a1af-c784-41a3-a9f2-722fc90cef26.png)

This tab allows spells to be Dragged and Dropped into the Spell List from either the Items Tab or any loaded Compendium.

### Configuring Overrides

Once the Spell is added to the Item, clicking on the "Edit" button will open the spell override configuration.

![Demonstration of overrides for the Wand of Web](https://user-images.githubusercontent.com/7644614/190871004-22077815-bbcc-4348-b6d6-b8cb033813fc.png)

The override configuration allows an Item's spells to be configured in the two primary ways that RAW dnd5e makes Spells possible to use on Items:
#### 1. Independent Uses for each Spell

Configure the "Limited Uses" override as you would normally for an item.

E.g. `1` of `1` per `Day`.

> This module does not add the ability to define a spell as castable as either 1/day or with spell slots, useful for some newer feats as example.

#### 2. Spell consumes charges of the "parent"

Configure the "Charges" input to input how many parent item charges this spell uses when cast.

> This module does not add the ability to prompt the user how many charges to use when casting a spell, useful for the Wand of Fireballs as example.

#### Saving Throw Override

If an Item's spell has a specified DC calculation rules, change the "vs DC" input accordingly. Be sure to change the `Spellcasting` dropdown to `Flat` if the DC is a flat DC.

E.g. vs DC. `15` `Flat` for Wand of Web casting "Web"

### Using Spells on Actors

When an Item with spells attached is added to an Actor, the spells attached to the item are also added. Any overrides configured are applied at this moment as well.

Unless the Item requires Attunement, a new section will appear in the Actor's "Spellbook" tab which contains that Item's spells. Multiple sections will appear if multiple items with spells are on a given actor.

The spells listed can be used like normal. The Core System's consumption behavior takes over at this point. Editing a spell that is already on an actor can be done by editing the spell from the Spellbook Tab.

![Demonstration of the "Wand of Web" displayed in an Actor Sheet.](https://user-images.githubusercontent.com/7644614/190870987-ba49d749-47cb-49f7-91d7-1f869f2f8190.png)


https://user-images.githubusercontent.com/7644614/190871810-824db216-56fa-4411-9e03-ada2a9c0f251.mp4


## API

When programatically deleting items that have spells attached, it is possible to skip the prompt for the user by providing the following object in the [DocumentModificationContext](https://foundryvtt.com/api/classes/foundry.abstract.Document.html#delete) option of the `delete` method.

```
someItem.delete({
  itemsWithSpells5e: {alsoDeleteChildSpells: true} // or false
})
```

The logic within this module will respect the boolean provided, deleting the child item spells itself if `true`, or leaving them alone if `false`.
