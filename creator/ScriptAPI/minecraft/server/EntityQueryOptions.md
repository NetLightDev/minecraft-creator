---
# DO NOT TOUCH — This file was automatically generated. See https://github.com/mojang/minecraftapidocsgenerator to modify descriptions, examples, etc.
author: jakeshirley
ms.author: jashir
ms.service: minecraft-bedrock-edition
title: minecraft/server.EntityQueryOptions Interface
description: Contents of the @minecraft/server.EntityQueryOptions class.
---
# EntityQueryOptions Interface

Contains options for selecting entities within an area.

#### Examples
##### ***testBlockConditional.ts***
```typescript
import { Dimension } from '@minecraft/server';

// Having this command:

// execute as @e[type=fox] positioned as @s if block ^ ^-1 ^ stone run summon salmon

// Equivalent scripting code would be:
function spawnFish(dimension: Dimension) {
    dimension
        .getEntities({
            type: 'fox',
        })
        .filter(entity => {
            const block = dimension.getBlock({
                x: entity.location.x,
                y: entity.location.y - 1,
                z: entity.location.z,
            });

            return block !== undefined && block.matches('minecraft:stone');
        })
        .forEach(entity => {
            dimension.spawnEntity('salmon', entity.location);
        });
}
```
##### ***testPlaySoundChained.ts***
```typescript
import { Dimension } from '@minecraft/server';

// Having this command:

// execute as @e[type=armor_stand,name=myArmorStand,tag=dummyTag1,tag=!dummyTag2] run playsound raid.horn @a

// Equivalent scripting code would be:
function playSounds(dimension: Dimension) {
    const targetPlayers = dimension.getPlayers();
    const originEntities = dimension.getEntities({
        type: 'armor_stand',
        name: 'myArmorStand',
        tags: ['dummyTag1'],
        excludeTags: ['dummyTag2'],
    });

    originEntities.forEach(entity => {
        targetPlayers.forEach(player => {
            player.playSound('raid.horn');
        });
    });
}
```
##### ***testSendMessageAllPlayers.ts***
```typescript
import { Dimension } from '@minecraft/server';

// Having this command:

// execute as @e[type=armor_stand,name=myArmorStand,tag=dummyTag1,tag=!dummyTag2] run tellraw @a { "rawtext": [{"translate": "hello.world" }] }

// Equivalent scripting code would be:
function sendMessagesToPlayers(dimension: Dimension) {
    const targetPlayers = dimension.getPlayers();
    const originEntities = dimension.getEntities({
        type: 'armor_stand',
        name: 'myArmorStand',
        tags: ['dummyTag1'],
        excludeTags: ['dummyTag2'],
    });

    originEntities.forEach(entity => {
        targetPlayers.forEach(player => {
            player.sendMessage({ rawtext: [{ translate: 'hello.world' }] });
        });
    });
}
```
##### ***testSetScoreBoardChained.ts***
```typescript
import { Dimension, world } from '@minecraft/server';

// Having these commands:

// scoreboard objectives add scoreObjective1 dummy
// scoreboard players set @e[type=armor_stand,name=myArmorStand] scoreObjective1 -1

// Equivalent scripting code would be:
function setScores(dimension: Dimension) {
    const objective = world.scoreboard.addObjective('scoreObjective1', 'dummy');
    dimension
        .getEntities({
            type: 'armor_stand',
            name: 'myArmorStand',
        })
        .forEach(entity => {
            if (entity.scoreboardIdentity !== undefined) {
                objective.setScore(entity.scoreboardIdentity, -1);
            }
        });
}
```
##### ***testSummonMobChained.ts***
```typescript
import { Dimension } from '@minecraft/server';

// Having this command:

// execute as @e[type=armor_stand] run execute as @a[x=0,y=-60,z=0,c=4,r=15] run summon pig ~1 ~ ~

// Equivalent scripting code would be:
function spawnPigs(dimension: Dimension) {
    const armorStandArray = dimension.getEntities({
        type: 'armor_stand',
    });
    const playerArray = dimension.getPlayers({
        location: { x: 0, y: -60, z: 0 },
        closest: 4,
        maxDistance: 15,
    });
    armorStandArray.forEach(entity => {
        playerArray.forEach(player => {
            dimension.spawnEntity('pig', {
                x: player.location.x + 1,
                y: player.location.y,
                z: player.location.z,
            });
        });
    });
}
```
##### ***checkFeatherNearby.ts***
```typescript
import { DimensionLocation, EntityComponentTypes } from "@minecraft/server";

// Returns true if a feather item entity is within 'distance' blocks of 'location'.
function isFeatherNear(location: DimensionLocation, distance: number): boolean {
    const items = location.dimension.getEntities({
        location: location,
        maxDistance: 20,
    });
    
    for (const item of items) {
        const itemComp = item.getComponent(EntityComponentTypes.Item);
    
        if (itemComp) {
            if (itemComp.itemStack.typeId.endsWith('feather')) {
                return true;
            }
        }
    }

    return false;
}
```

## Properties

### **closest**
`closest?: number;`

Limits the number of entities to return, opting for the closest N entities as specified by this property. The location value must also be specified on the query options object.

Type: *number*

### **excludeFamilies**
`excludeFamilies?: string[];`

Excludes entities that match one or more of the specified families.

Type: *string*[]

### **excludeGameModes**
`excludeGameModes?: GameMode[];`

Excludes entities if have a specific gamemode that matches the specified gamemode.

Type: [*GameMode*](GameMode.md)[]

### **excludeNames**
`excludeNames?: string[];`

Excludes entities that have a name that match one of the specified values.

Type: *string*[]

### **excludeTags**
`excludeTags?: string[];`

Excludes entities with a tag that matches one of the specified values.

Type: *string*[]

### **excludeTypes**
`excludeTypes?: string[];`

Excludes entities if they are one of the specified types.

Type: *string*[]

### **families**
`families?: string[];`

If specified, includes entities that match all of the specified families.

Type: *string*[]

### **farthest**
`farthest?: number;`

Limits the number of entities to return, opting for the farthest N entities as specified by this property. The location value must also be specified on the query options object.

Type: *number*

### **gameMode**
`gameMode?: GameMode;`

If specified, includes entities with a gamemode that matches the specified gamemode.

Type: [*GameMode*](GameMode.md)

### **location**
`location?: Vector3;`

Adds a seed location to the query that is used in conjunction with closest, farthest, limit, volume, and distance properties.

Type: [*Vector3*](Vector3.md)

### **maxDistance**
`maxDistance?: number;`

If specified, includes entities that are less than this distance away from the location specified in the location property.

Type: *number*

### **maxHorizontalRotation**
`maxHorizontalRotation?: number;`

If specified, will only include entities that have at most this horizontal rotation.

Type: *number*

### **maxLevel**
`maxLevel?: number;`

If defined, only players that have at most this level are returned.

Type: *number*

### **maxVerticalRotation**
`maxVerticalRotation?: number;`

If specified, only entities that have at most this vertical rotation are returned.

Type: *number*

### **minDistance**
`minDistance?: number;`

If specified, includes entities that are least this distance away from the location specified in the location property.

Type: *number*

### **minHorizontalRotation**
`minHorizontalRotation?: number;`

If specified, will only include entities that have at a minimum this horizontal rotation.

Type: *number*

### **minLevel**
`minLevel?: number;`

If defined, only players that have at least this level are returned.

Type: *number*

### **minVerticalRotation**
`minVerticalRotation?: number;`

If specified, will only include entities that have at least this vertical rotation.

Type: *number*

### **name**
`name?: string;`

Includes entities with the specified name.

Type: *string*

### **scoreOptions**
`scoreOptions?: EntityQueryScoreOptions[];`

Gets/sets a collection of EntityQueryScoreOptions objects with filters for specific scoreboard objectives.

Type: [*EntityQueryScoreOptions*](EntityQueryScoreOptions.md)[]

### **tags**
`tags?: string[];`

Includes entities that match all of the specified tags.

Type: *string*[]

### **type**
`type?: string;`

If defined, entities that match this type are included.

Type: *string*

::: moniker range="=minecraft-bedrock-experimental"
### **volume**
`volume?: BlockAreaSize;`

In conjunction with location, specified a cuboid volume of entities to include.

Type: [*BlockAreaSize*](BlockAreaSize.md)

> [!CAUTION]
> This property is still in pre-release.  Its signature may change or it may be removed in future releases.
::: moniker-end

#### Examples
##### ***testBlockConditional.ts***
```typescript
import { Dimension } from '@minecraft/server';

// Having this command:

// execute as @e[type=fox] positioned as @s if block ^ ^-1 ^ stone run summon salmon

// Equivalent scripting code would be:
function spawnFish(dimension: Dimension) {
    dimension
        .getEntities({
            type: 'fox',
        })
        .filter(entity => {
            const block = dimension.getBlock({
                x: entity.location.x,
                y: entity.location.y - 1,
                z: entity.location.z,
            });

            return block !== undefined && block.matches('minecraft:stone');
        })
        .forEach(entity => {
            dimension.spawnEntity('salmon', entity.location);
        });
}
```
##### ***testPlaySoundChained.ts***
```typescript
import { Dimension } from '@minecraft/server';

// Having this command:

// execute as @e[type=armor_stand,name=myArmorStand,tag=dummyTag1,tag=!dummyTag2] run playsound raid.horn @a

// Equivalent scripting code would be:
function playSounds(dimension: Dimension) {
    const targetPlayers = dimension.getPlayers();
    const originEntities = dimension.getEntities({
        type: 'armor_stand',
        name: 'myArmorStand',
        tags: ['dummyTag1'],
        excludeTags: ['dummyTag2'],
    });

    originEntities.forEach(entity => {
        targetPlayers.forEach(player => {
            player.playSound('raid.horn');
        });
    });
}
```
##### ***testSendMessageAllPlayers.ts***
```typescript
import { Dimension } from '@minecraft/server';

// Having this command:

// execute as @e[type=armor_stand,name=myArmorStand,tag=dummyTag1,tag=!dummyTag2] run tellraw @a { "rawtext": [{"translate": "hello.world" }] }

// Equivalent scripting code would be:
function sendMessagesToPlayers(dimension: Dimension) {
    const targetPlayers = dimension.getPlayers();
    const originEntities = dimension.getEntities({
        type: 'armor_stand',
        name: 'myArmorStand',
        tags: ['dummyTag1'],
        excludeTags: ['dummyTag2'],
    });

    originEntities.forEach(entity => {
        targetPlayers.forEach(player => {
            player.sendMessage({ rawtext: [{ translate: 'hello.world' }] });
        });
    });
}
```
##### ***testSetScoreBoardChained.ts***
```typescript
import { Dimension, world } from '@minecraft/server';

// Having these commands:

// scoreboard objectives add scoreObjective1 dummy
// scoreboard players set @e[type=armor_stand,name=myArmorStand] scoreObjective1 -1

// Equivalent scripting code would be:
function setScores(dimension: Dimension) {
    const objective = world.scoreboard.addObjective('scoreObjective1', 'dummy');
    dimension
        .getEntities({
            type: 'armor_stand',
            name: 'myArmorStand',
        })
        .forEach(entity => {
            if (entity.scoreboardIdentity !== undefined) {
                objective.setScore(entity.scoreboardIdentity, -1);
            }
        });
}
```
##### ***testSummonMobChained.ts***
```typescript
import { Dimension } from '@minecraft/server';

// Having this command:

// execute as @e[type=armor_stand] run execute as @a[x=0,y=-60,z=0,c=4,r=15] run summon pig ~1 ~ ~

// Equivalent scripting code would be:
function spawnPigs(dimension: Dimension) {
    const armorStandArray = dimension.getEntities({
        type: 'armor_stand',
    });
    const playerArray = dimension.getPlayers({
        location: { x: 0, y: -60, z: 0 },
        closest: 4,
        maxDistance: 15,
    });
    armorStandArray.forEach(entity => {
        playerArray.forEach(player => {
            dimension.spawnEntity('pig', {
                x: player.location.x + 1,
                y: player.location.y,
                z: player.location.z,
            });
        });
    });
}
```
##### ***checkFeatherNearby.ts***
```typescript
import { DimensionLocation, EntityComponentTypes } from "@minecraft/server";

// Returns true if a feather item entity is within 'distance' blocks of 'location'.
function isFeatherNear(location: DimensionLocation, distance: number): boolean {
    const items = location.dimension.getEntities({
        location: location,
        maxDistance: 20,
    });
    
    for (const item of items) {
        const itemComp = item.getComponent(EntityComponentTypes.Item);
    
        if (itemComp) {
            if (itemComp.itemStack.typeId.endsWith('feather')) {
                return true;
            }
        }
    }

    return false;
}
```
