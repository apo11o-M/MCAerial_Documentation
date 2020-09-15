# Items
Basic items that do not need and special functionality or attributes do not need a custom class. It’s also recommended to create your mod’s general item class since you can put all your custom items and blocks into a dedicated tab in creative mode.

## Common issue(s):
You implemented the recipes but nothing shows up when you’re in the game. It’s possible that you did not specify the state of the blocks you're using (this mostly happens with stone, dirt, or colored wool as those blocks have different states). Simply add `"data": 0` to the end of the block id to and it should fix the issue. `"item": "minecraft:stone", "data": 0`
