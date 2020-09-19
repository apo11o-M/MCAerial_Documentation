# EventHandlers
Minecraft Forge provides event buses that are extremely useful for modders. The general concept is that you create "event handling" methods that subscribe a particular event, and being fired every time that event happens.

## Create an Event Handler
General steps:

1. **Create am event handling class**, you can name it whatever you want.
2. **Give your class the `@EventBusSubscriber` annotation**. By doing this you don't need to actually register it, that will happen automatically!
3. Put `public static void [your_event_name](Event)` methods in the your event handling class. Each method needs to have the `@SubscribeEvent` annotation and the parameter passed to the event must be the type of event you are trying to handle. The methods should contain your custom code to whatever you want when the event happens.

Note: It is important to note that the event parameter passed usually has a number of *useful fields and methods* to aid your custom code.

For more information see Jabelar's guide on [Minecraft Event Handling](http://jabelarminecraft.blogspot.com/p/minecraft-forge-172-event-handling.html)

## Using Event Handling To Modify Vanilla Behavior

For your own custom classes, you often don't need event handling because you already have the means of controlling their behavior. So event handling is most useful for the purpose of changing the behavior of "vanilla" blocks, items, entities, world generation, etc.

Note: Before implementing event handling you should first check to see if there are already *public methods and fields* available to you.  For example, changing a texture of a block or changing the motion of an entity can be done with public methods. Even though events could be used for these examples, it is probably easiest to use the provided methods.

### (Preferable To Not Use) Event Handling In Custom Classes
If you make your own classes, especially as extending existing blocks, items or entities, you already have access to `@Override` several methods that are actually event handlers.  Many of these methods are named starting with "on", like `onUpdate()`. These methods are automatically called at the same time as the related events are posted and so don't need to be called or otherwise handled.

## Event Cancelation
It is especially important to understand is that certain events are "cancelable", which is usually annotated with `@Cancelable`. The cool thing about cancelable events is you can choose to prevent (i.e. cancel) the vanilla processing from happening at all: cancelable events allow you to fully replace the vanilla event handling with your own code!

**Warning**: One problem with event cancelation is that it will affect other mods as well and can cause event incompatibility. You can affect the order of handling using priority (described below) and for those at the same priority I believe the handlers will fire based on the order the mods are loaded (or more specifically in order they register the handlers which is usually during the FMLInitializationEvent so would be in order they are loaded).
