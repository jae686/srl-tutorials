# 3D Model pipeline in SRL

Before starting to use 3D into the sega saturn, there are some limitations to consider :

- There are no triangles - only quads (distorted sprites)
- There are no UV maps in the classical sense - each texture is fully mapped to the sprite / quad.
- Matrix operations are done on the CPU

For using 3D models with SRL, there are 2 main tools we will be using in order to get 3D models into our saturn project :

- The `ModelObject.hpp` file that contains the `ModelObject` class. This can be found on the `SaturnRingLib\Samples\VDP1 - 3D - Flat teapot\src` folder.
- A file conversion utility from `.obj` format (Wavefront) into a binary `.NYA` file, that contains the model and textures when applicable. This tool can be found [here](https://github.com/ReyeMe/ModelConverter-linux)

