# Entities
Entities are one of the most interesting concept in minecraft, and it's so broad, that this doc can't fit every single bit of info into it. So instead, I picked out some of the most important concepts in this class.

`motionX`, `motionY`, `motionZ` - these three fields determines the motion/speed towards the corresponding axis. This is very useful for making vehicles such as cars or airplanes.

`rotationYaw`, `rotationPitch` - the yaw and pitch rotation angle

For `rotationYaw`, 0 degrees at north (Z axis, negative), clockwise is positive, counter-clockwise is negative. When the angle reach 360 degrees, minecraft will convert it into 0 degrees. So there will be no issues where `rotationYaw` gets bigger than 360 degrees.

For `rotationPitch`, 0 degrees at horizontal, look down is positive, look up is negative. The range is from 180 to -180 degrees.

`rotationRoll` - Minecraft does not have rotationRoll integrated into the game, so if one wants to create roll effect, you would have to done it by yourself by adding a few new variables to your entity class. Most of the time we use `rotationRoll` for rotating the model and the player's camera.

Rotating the player's model can be done using the `RenderPlayerEvent` (see more in [RenderPlayer](https://apo11o-m.github.io/MCAerial_Documentation/rendering/renderclass/#renderplayer)). As for rotating the player's camera, it can be done using the `CameraSetup` event which allows the mod to alter the angle of the player's camera in the yaw, pitch, and roll direction.

`ignoreFrustumCheck` - Since minecraft only render entities whom bounding box are in the player's view, for those whose model significantly exceeds the bounding box, we can set `ignoreFrustumCheck = true` in the constructor so that minecraft will always render the entity's model.
