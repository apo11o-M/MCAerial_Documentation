# Blocks
For simple blocks (think of cobblestone, dirt, grass, wood, etc.) which do not need any special functionality or attributes, a custom class is not necessary. It’s also recommended to create a general block class for your mod for reasons mentioned above.

If you want more functionality (such as interactions with players) then a custom class is required. The `Block` class has many methods that you can implement to your custom block class.

One thing to note about blocks is the boundary boxes(hitbox). if one wants to create a block that has a smaller bounding box than a regular full block such as half slabs, then in the block class one would have to declare a new object called `AxisAlignedBB` and override the `getBoundingBox()` method to return the `AxisAlignedBB` field you just declared.

## ItemBlock
`ItemBlock` is a subclass of `Item` and has a field `block` that holds a reference to the block it presents.

A block with `Block` class but not `ItemBlock` class will be impossible to hold in inventory. (like `minecraft:water` exists as a block but can’t hold in an inventory)

A more detailed document can be found [here](https://mcforge.readthedocs.io/en/1.12.x/blocks/blocks/).
