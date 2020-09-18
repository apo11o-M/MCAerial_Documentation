# Blocks
For simple blocks (think of cobblestone, dirt, grass, wood, etc.) which do not need any special functionality or attributes, a custom class is not necessary. It’s also recommended to create a general block class for your mod for reasons mentioned above.

If you want more functionality (such as interactions with players) then a custom class is required. The `Block` class has many methods that you can implement to your custom block class.


## Not-Full Blocks
Sometimes we want to create blocks that are smaller and doesn't take the full-block space, such as half slabs or fences.

To do so, we would have to declare a new object and override a few methods.

1. First is to declare an `AxisAlignedBB` object like so: `public static final AxisAlignedBB CUSTOM_BLOCK = new AxisAlignedBB(minX, minY, minZ, maxX, maxY, maxZ);`. The object takes six arguments, the first three are the minimum coordinates (x, y, z) of the bounding box, and the last three are the maximum coordinates (x, y, z) of the bounding box. All of them are guaranteed to be less than 1 (if the argument is greater than 1 then it means the bounding box is bigger than a full block and weird things would happen).
2. The second is to override the `getBoundingBox()` method to return the `CUSTOM_BLOCK` object we just created.
3. And at last override the `isFullBlock()` method to return false.


## Transparent blocks
So, how do we create transparent blocks? One way is to extend the minecraft `BlockGlass` class and call it done. Another way is to just override two methods, and we'll demonstrate that here.

- Override the `isOpaqueCube()` method and return false
- Override the `getBlockLayer()` method and return `BlockRenderLayer.CUTOUT`

And that's it. The player will now be able to see the stuff behind the block through the part of the texture that's *transparent*. It's very important for you to not color the part of the texture where you want to see through, or else it won't work.


## ItemBlock Class
`ItemBlock` is a subclass of `Item` and has a field `block` that holds a reference to the block it presents.

A block with `Block` class but not `ItemBlock` class will be impossible to hold in inventory. (like `minecraft:water` exists as a block but can’t hold in an inventory)
