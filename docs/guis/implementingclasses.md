# Implementing Classes
## `IInventory`
This is fairly simple as we typically just need to extend the existing IInventory classes such as the `InventoryCraftingResult` or the `InventoryEnderChest`. The `IInventory` class is essentially a list that stores a bunch of `ItemStacks` and with many other helper methods that manipulates the list.

## `Container`
As stated above, this is the class than handles the interactions between the player(client) and the server.

In the constructor we creates `Slots` using the `addSlotToContainer()` method and link the slot to the block's and player's inventory. The slots are the translucent boxes you see when you hover your mouse over some of the container block's guis. It is capable to connect with both the player's and the block's inventory. Note that we also have to specify the on-screen position of the slot.

One thing to note is the `transferStackInSlot()` method. It has to be implemented by the modders since the inherited method doesn't do anything. This method is called when the player wants to move an itemstack from one slot to another. This includes left clicks and shift-clicks. Failure to consider the possibilities would result in weird inventory behaviors such as itemstacks disappearing or even mc stuck in a infinite for loop.

## `GuiContainer`
This class specifies the layout of the gui, render the background images, retrives items and progress bars for display, amd more.

There are multiple gui drawing methods in the `GUI` class, and each of them have different usages and parameters. For my texture I am using the `drawModalRectWithCustomSizedTexture()` since my texture size is not the minecraft default 176x166 pixels texture size.

I also suggest using GNU Image Manipulation Program (GIMP) or Adobe Photoshop to create your gui textures. You can also go into minecraft's source code, find the original png textures and start from there. It would save you tons of time if you have a template to start with.

To add buttons to the gui, we have to initialize it in the `initGui()` method, create an if block with the button's id, and perform the actions for each buttons.

It's also important to not offset the textures and buttons by absolute pixels, but rather use proportions based on the screen size. This is because each person has different monitor/screen sizes, and different UI size settings. If we uses absolute pixels to off set their position it's very easy to mess up the entire layout for a different window size setting.

Warning: This class only runs on the CLIENT SIDE, so if you want to do stuff that would relate to the server side such as adding items into certain slots, you would have to send a packet to the server side and add the item over there or else your gui would have desyncing issues. I have an example of setting up packets and IMessages in [Networking](https://github.com/apo11o-M/MCAerial_Mod/tree/master/src#networking).

## `IGuiHandler`
The IGuiHandler provides the Synchronization of the slot contents between the server and client and lets you avoid having to create custom packets. Basically it provides an association between the Container on the server side and the GuiScreen on the client side.

The class check the enumerated ID that is passed into the methods and return the associated element (Container on the server side, or GuiScreen on the client side).
