# Partial Ticks
Another important concept about rendering is partial ticks. Entities update every 0.05 seconds(1 tick), but the rendering class updates every frame, which is about 60fps for a normal computer. If we only change the rotation for the model every 0.05 seconds then it will have an unpleasant stuttering/choppy effect. To fix this, we have to interpolate the rotating angle between the first frame and the second frame and rotate the model by that amount as the `doRender` method is called upon every frame in minecraft.

// show how to do interpolateAngle() in the render class
