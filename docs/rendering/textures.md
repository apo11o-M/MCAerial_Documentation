# Textures (.png files)
The texture size must be a square and a power of 2, such as 16x16, 32x32, 64x64 etc.

Also, for entities be sure to add the textures location to the render class so minecraft knows where your texture are.

## Blockbench
[Blockbench](https://blockbench.net/) also suppports making blocks and entities. Once you are finish making your model, export your project to .java models and a .png texture. 

## GIMP
The [GNU Image Manipulation Program (GIMP)](https://www.gimp.org/) is a very powerful free image editor that can even compete with photoshop. Be sure to check it out!!

## Common issues:

  - If one of the textures is broken, look at the json file. 90% of times a broken texture is because of a faulty json file.

  - If the texture works in the inventory but not on the ground, check the spelling and run the json validator on the json file in `assets.[youmodname].models.block`.

  - If the texture works when placed but not in the inventory, check the spelling and run the json validator on the json file in `assets.[yourmodname].models.item`.

  - If the texture doesn’t work at all, check the spelling and run the json validator on the json file in `assets.[yourmodname].blockstates`.

  - If the texture still doesn’t work at all, check your image file, make sure it’s in png format and square size
