# Render Class
The purpose of the rendering class is to render the model and the texture of the entity. The `doRender` method is mainly responsible for that.

The steps of properly render the entity is to:
1. `GlStateManager.pushMatrix` - This is required before we use other OpenGL functions. Fail to do so would result in GLStackOverflow error.
2. `interpolateRotation()` - interpolate the angle the model should rotate. See more on [Partial Ticks](https://apo11o-m.github.io/MCAerial_Documentation/rendering/partialticks/).
3. `GlStateManager.translate(x, y, z)` - Translate the entity's position. The arguments are the current x, y, and z coordinates plus the offsets you want to make.
4. `GlStateManager.rotate(angle, xAxis, yAxis, zAxis)` - rotate the model about a specific axis. The `angle` is in degrees, and the `x`, `y`, and `z` field indicates which axis the model rotates. Ex. `GlStatemanager.rotate(90F, 0.0F, 1.0F, 0.0F)` means rotate 90 degrees clockwise about the y axis.
5. `bindTexture(ResourceLocation)` - Bind the model with your texture.
6. `mainModel.render()` - this sets the models various rotation angles then renders the model.
7. `GlStateManager.popMatrix` - pop the matrix. Fail to do so would result in GLStackUnderflow error.

Bedrock_Miner has a detailed guide on using `GlStateManager` class, be sure to check it out! [Advanced Modding - Rendering](https://bedrockminer.jimdofree.com/modding-tutorials/advanced-modding/vanilla-rendering/)

## RenderPlayer
The pivot point of the player's model is located in the center of the feet, and if one want to change the pivot point, translate the model first, do the rotation, and then translate back. The player model dimension can be found on this thread. [How Tall is Steve](https://www.reddit.com/r/Minecraft/comments/143xdt/how_tall_is_steve/)

One thing to note is that when the program run `GlStateManager.pushMatrix()` in your `RenderPlayerEvent.Pre` event, all of the "virtual" axis minecraft did previously will be reseted ([Euler Angle Explained](https://en.wikipedia.org/wiki/Euler_angles)), so if we want to rotate the player's model in the pitch and roll direction, we will have to rotate the model in the corresponding yaw angle first, do the pitch and roll, and then undo the yaw rotation we did by rotating in the reverse direction.

Also, be sure to have a variable that indicates the program to pop the matrix in the `RenderPlayerEvent.Post` event. Popping the matrix in the `RenderPlayerEvent.Pre` event will cause GLStackUnderflow error.

So the entire player model rotation process in the `RenderPlayerEvent.Pre` event looks like this:
  1. pushMatrix
  2. rotate in the yaw direction for the virtual axis
  3. translate the model to its pivot point
  4. do your pitch & roll rotation
  5. translate the model back
  6. undo step two by applying the same rotation but in opposite direction  
