# Render Class
The purpose of the rendering class is to render the model and the texture of the entity. The `doRender` method is mainly responsible for that.

The model's rotation is done by using `GlStatemanager.rotate(angle, x, y, z)`. Note the angle is in degrees, and the `x`, `y`, and `z` field indicates which axis the model rotates. Ex. `GlStatemanager.rotate(90F, 0.0F, 1.0F, 0.0F)` means rotate 90 degrees clockwise about the y axis.


## RenderPlayer
The pivot point of the player's model is located in the center of the feet, and if one want to change the pivot point, translate the model first, do the rotation, and then translate back. The player model dimension can be found [here](https://www.reddit.com/r/Minecraft/comments/143xdt/how_tall_is_steve/).

One thing to note is that when the program run `GlStateManager.pushMatrix()` in your `RenderPlayerEvent.Pre` event, all of the "virtual" axis minecraft did previously will be reseted ([Euler Angle Explained](https://en.wikipedia.org/wiki/Euler_angles)), so if we want to rotate the player's model in the pitch and roll direction, we will have to rotate the model in the corresponding yaw angle first, do the pitch and roll, and then undo the yaw rotation we did by rotating in the reverse direction.

Also, be sure to have a variable that indicates the program to pop the matrix in the `RenderPlayerEvent.Post` event. Popping the matrix in the `RenderPlayerEvent.Pre` event will cause GLStackUnderflow error.

So the entire player model rotation process in the `RenderPlayerEvent.Pre` event looks like this:
  1. pushMatrix
  2. rotate in the yaw direction for the virtual axis
  3. translate the model to its pivot point
  4. do your pitch & roll rotation
  5. translate the model back
  6. undo step two by applying the same rotation but in opposite direction  

[Bedrock_Miner](https://bedrockminer.jimdofree.com/modding-tutorials/advanced-modding/vanilla-rendering/) has a detailed guide on using `GlStateManager` class
