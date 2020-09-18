# Items
Basic items that do not need and special functionality or attributes do not need a custom class. It’s also recommended to create your mod’s general item class since you can put all your custom items and blocks into a dedicated tab in creative mode.


## Holding the Item Right Click
Sometimes we want to make something happen when the player is holding the item and right clicks on a block.

To do so, we override the method `onItemUse()` which is called when the player right clicks the item on a block.

In the MCAerial_Mod, I make the item spawn a glider on a block when it's being right clicked by using `worldIn.spawnEntity(new EntityPlane(worldIn, (double)pos.getX() + 0.5, (double)pos.getY() + 1, (double)pos.getZ() + 0.5));`. Note: Be sure to check the sides! Spawning entities should only happen on the server side or ghost entities would happen.


## Common issue(s)
You implemented the recipes but nothing shows up when you’re in the game. It’s possible that you did not specify the state of the blocks you're using (this mostly happens with stone, dirt, or colored wool as those blocks have different states). Simply add `"data": 0` to the end of the block id to and it should fix the issue. `"item": "minecraft:stone", "data": 0`
