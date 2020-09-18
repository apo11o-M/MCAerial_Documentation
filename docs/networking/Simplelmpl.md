# Simplelmpl (Packet System)
The client and server are always connected by a network connection(packets).

Simplelmpl is the name of the packet system we will be using. It's the easiest way to send custom data between the server and the client.

## PacketHandler Class
The first step is to create the `PacketHandler` class. And inside the class we declare a `SimpleNetworkWrapper` object as a static field.

~~~java
public class PacketHandler {
	public static final SimpleNetworkWrapper INSTANCE = NetworkRegistry.INSTANCE.newSimpleChannel("yourModId".toUpperCase());
	private static int nextPacketId = 0;

	public static void initPackets() {
		registerMessage(MyMessage.class, MyMessage.MyPacket.class);
	}

	private static void registerMessage(Class message, Class packet) {
		INSTANCE.registerMessage(packet, message, nextPacketId, Side.CLIENT);
		INSTANCE.registerMessage(packet, message, nextPacketId, Side.SERVER);
		nextPacketId++;
	}
}
~~~

Notice that we also registered our classes using the `registerMessage` method. The `initPackets` method is being called during the `init` stage.

To register our packet, simply call `INSTANCE.registerMessage(MyMessageHandler.class, MyMessage.class, 0, Side.Server);`

Let's breakdown the parameters of this method.

- The first parameter `messagehandler` is the class that handles your packet. Note that *this class must always have a default constructor*.
- The second parameter `requestMessageType` is your actual packet class. Note that *This class must also have a default constructor*.
- The third parameter `id` is the discriminator for the packet. This is a per-channel unique ID for the packet.
- The final parameter `side` is the side that your packet will ***received*** on. If you are planning to send the packet on both sides, then the packet have to be registered twice, once on each side.


## IMessages (Packets)
Now the next step is to create the packet itself. A packet is defined by using the `IMessage` interface. This interface defines 2 methods, `toBytes` and `fromBytes`. These methods, respectively, write and read the data in your packet to and from a `ByteBuf` object, which is an object used to hold a stream (array) of bytes which are sent through the network.

Here's an example packet that's designed to send an int and a boolean through the network.
~~~java
public class MyMessage implements IMessage {
    private int simpleInt;
    private boolean simpleBool;

    // a default constructor that's always required otherwise you'll get errors
    public MyMessage() {}

    public MyMessage(int simpleInt, boolean simpleBool) {
        this.simpleInt = simpleInt;
        this.simpleBool = simpleBool;
    }

    @Override
    public void fromBytes(ByteBuf buf) {
        // reads the int back from the buf. Note if you have multiple values, you must read in the same order you wrote.
        this.simpleInt = buf.getInt(0);
        this.SimpleBool = buf.getBoolean(1);
    }

    @Override
    public void toBytes(ByteBuf buf) {
        // writes the int into the buf
        buf.writeInt(this.simpleInt);
        buf.writeBoolean(this.simpleBool);
    }
}
~~~

## IMessageHandler
Now, how do we use the packet? Well, first we have to have a class that *handles* the packet. This is created with the IMessageHandler interface.

It's recommended to make the `IMessageHandler` class inside the `IMessage` class. Note that if this is done, the inner class has to be declared as `static`.

In this example we want to give a certain amount of diamonds to the player based on the int we received.

~~~java
public static class MyIMessageHandler implements IMessageHandler<MyMessage, IMessage> {

    public MyPacket() {}
    // do whatever stuff you want based on the parameters you recieved
    // in this case I'm using the forge's documentation example, giving the player some diamonds
    @Override
    public IMessage onMessage(MyMessage message, MessageContext ctx) {
      // This is the player the packet was sent to the server from
      EntityPlayerMP serverPlayer = ctx.getServerHandler().player;
      // the value that was sent
      int amount = message.toSend;
      // Execute the action on the main server thread by adding it as a scheduled task
      Minecraft.getMinecraft().addScheduledTask(() -> {
              serverPlayer.inventory.addItemStackToInventory(new ItemStack(Items.DIAMOND, amount));
      });
      // no response packet
      return null;
    }
}
~~~

## okay, so now how do we use the packets?
### Sending to the Server
There is only one way to send packets to server, since there will only be one server to send to.

To do so, simply call `PacketHandler.INSTANCE.sendToServer(new MyMessage(toSend))`. The message will then be sent to the Side.SERVER IMessageHandler for its type.

### Sending to the Client
There are four ways of sending packets to clients:

1. `sendToAll` - This will send the packet to every single player on the server. No matter what location or dimension they are in.
2. `sendToDimension` - This method takes two arguments: `IMessage`, and an int. The dimension int can be obtained through `world.provider.getDimension()`. The packet will be sent to all players in that dimension.
3. `sendToAllAround` - This method takes the `IMessage` and `NetworkRegistry.TargetPoint` object. All players within the `TargetPoint` will have the packet be sent to them. A `TargetPoint` object requires a dimension, x/y/z coordinates, and a range. It represents a sphere with radius of range, centered at (x, y, z) in a world.
4. `sendTo` - This send the packet to the specific player. Note that the player object has to be casted into `EntityPlayerMP`.
5. `sendToAllTracking` - Last but not least, there is also `INSTANCE.sendToAllTracking` which sends packets to all players on the server who are “tracking” the target. There are two variants of `sendToAllTracking`: one accepts a `TargetPoint`, and another one accepts an `Entity`. For the former, all clients that has the block located at `TargetPoint` loaded will receive the packet; for the latter, all clients that are within the tracking range of supplied `Entity` will receive the packet. Do note that `EntityPlayers` will not track themselves, so this cannot replace the use of `sendTo`; if the `EntityPlayer` that is used as target also requires syncing, you need to call `sendTo` on that before calling the `Entity` version of `sendToAllTracking`.

## Example

***[So basically the result networking stuff would look something like this.](https://gist.github.com/apo11o-M/bec16b08a7cfa43e99820bbe625fec9b)***
