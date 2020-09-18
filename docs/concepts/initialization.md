# Initialization
During startup, Forge will call your mod several times to add new blocks, items, read configuration files, and integrate itself into the game by registering your classes in the appropriate location.

- PreInitialization - Run before anything else. Read your config, create blocks, items, etc, and register them within the GameRegistry.

- Initialization - Do your mod setup. Build whatever data structures you care about, and register recipes.

- PostInitialization - Handle interactions with other mods.

PreInitialization is performed for all the mods you installed, followed by Initialization for all mods, followed by PostInitialization for all mods. Initializing the mods in `init()` is particularly useful when there might be interactions between multiple mods
