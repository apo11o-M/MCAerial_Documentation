# Partial Ticks

\>\> Partial ticks is a fractional value representing the *amount of time that has passed since the last tick.*

Another important concept about rendering is partial ticks. Minecraft updates it's game logic every ticks, or 0.05 seconds, but the rendering class updates every frame, which is about 60fps for a normal computer. Now if we only change the rotation for the model every 0.05 seconds then it will have an unpleasant stuttering/choppy rotation. (The reason it rotates every 0.05 seconds is because we are using the rotation angle from our custom entity class, which, only updates every ticks).

To fix this, we have to *interpolate* the rotating angle between the first frame and the second frame and rotate the model by that amount as the `doRender()` method is called upon every frame in minecraft.

So here's the basic formula on how to calculate the rotation angle using partial ticks:

\>\> prevAngle + (angle - prevAngle) x partialTicks
