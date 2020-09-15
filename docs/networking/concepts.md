# Concepts
The client and server are always connected by a network connection(packets).

There are two primarily reasons for networking communication.

1. Making sure the client view is “in sync” with the server view
  - The flower at coordinates x, y, z, just grew.
2. Giving the client a way to tell the server that something has changed about the player
  - The player just pressed a key.

The most common way to accomplish these goals is through the packet system where the client and the server send messages to each other. Those messages are usually structured, containing data in a particular arrangement, for the ease of sending and receiving.

The simplest packet system would be [Simplelmpl]().
