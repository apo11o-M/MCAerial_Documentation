# Sides
The minecraft code can be divided into two "sides" - Client and Server
  - The Server side is responsible for running the game logic (mob spawning, weather, updating inventories, health, AI, etc), maintaining the master copy of the world - updating the blocks and entities based on packets received from the client, and sending updated information to all the clients.
  - The Client side is primarily responsible for reading input from the player and for rendering the screen.

In a minecraft single player world, there is one server and one client both running in your computer. In multiplayer mode, there is one server but with multiple clients(players) connect to it.

In order to distinguish between client and server, you use the boolean check `world.isRemote` to check sides. If the field is `true`, the world is currently running on the logical client. If the field is `false`, then the world is running on the logical server.

## Now you may ask: *why do we need to check the sides?*

It's because we have to ensure that the game logic and other mechanics only runs on the server side, such as damaging the player every time they walk on your block, or have your machine process dirt into diamonds. Applying game logic to the client side could cause desynchronization (ghost entities, desynchronized stats, disappearing items, etc) in the lightest case, and crashes in the worst case.

More details can be found on [here](http://greyminecraftcoder.blogspot.com/2013/10/client-server-communication-using.html).
