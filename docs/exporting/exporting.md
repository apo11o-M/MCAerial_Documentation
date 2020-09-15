# Exporting your mod
So you just finished building you mod, and the next step is to export it as .jar file that people can use.
  1. Open you command prompt inside your mod folder
  2. Run `gradlew build`, or `bash gradlew build` if you're running on macOS
  3. Once the build is done, go to your mod folder > build > libs and there will be two files in there. The one with the word "source" is the source code of your mod. KEEP IT! It's the other one that you post online or share to others.

For sharing your mod online, I suggest create an account and upload your file to [Mediafire](https://www.mediafire.com/), and it will generate a link that anyone will be able to access and download.

## Common issues
The command prompt shows “Build fail” when you are trying to export your mod.
* What went wrong:
`Execution failed for task ':processResources'`
This is due to an issue with forge 1.12.2 and your mcmod.info file. Change the mcversion from 1.12.2 to mcversion (...yeah) and it should fix the crash.
`"mcversion": "${mcversion}"`
