# Getting Started
Making blocks with guis can be confusing because there are several things that have to work together. More importantly, you may not need all the complexity depending on what you are trying to achieve.

Before you start, you should know what kind of classes you would/might have to implement.

## Do you need an `IInventory`
This class holds the information about any stuffs (items or blocks) that are contained inside the block. For example when you put a stack of coal into the furnace it is going into the block's inventory.

## Do you need a `Container`
If your block have an `IInventory` or a custom gui then `Container` class is a must. It helps combining the inventories and sync them between the client and the server.

## Do you need a `GuiContainer`
This is where the actual "gui stuff" goes. This class handles the rendering of your custom gui textures, and handles the gui buttons on screen.

## Do you need an `IGuiHandler`
If you use a `Container` then `IGuiHandler` class is a must. It helps creating network packets to sync up the containers on the client and the server. The `IGuiHandler` also needs a unique GUI ID passed to it as a parameter, so I suggest creating an enum to help manage these IDs.

## Do you need a `TileEntity`
A tile entity is like a variation of a regular block with IInventory. It is particularly useful if you want to gui to continue to do something even when it isn't open. For example a furnace will continue to cook something even if you are not in the gui.

Examples where no tile entity is needed:

- A simple block that just displays some information in the GUI, like maybe a sign that displays your mod's credits or help info.
- A block where the processing happens instantaneously like a crafting table.

Examples where a tile entity is recommended:
- A block where processing happens over time like cooking in a furnace.
