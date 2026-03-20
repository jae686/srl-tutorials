# 3D Model pipeline in SRL

Before starting to use 3D into the sega saturn, there are some limitations to consider :

- There are no triangles - only quads (distorted sprites)
- There are no UV maps in the classical sense - each texture is fully mapped to the sprite / quad.
- Matrix operations are done on the CPU , drawing is done by VDP1

For using 3D models with SRL, there are 2 main tools we will be using in order to get 3D models into our saturn project :

- The `ModelObject.hpp` file that contains the `ModelObject` class. This can be found on the `SaturnRingLib\Samples\VDP1 - 3D - Flat teapot\src` folder.
- A file conversion utility from `.obj` format (Wavefront) into a binary `.NYA` file, that contains the model and textures when applicable. This tool can be found [here](https://github.com/ReyeMe/ModelConverter-linux). We will be using blender the generate and export the files.

## Preparing your first 3D Model

### Obtaining the converter tool

First we need the model converter tool. You can clone the repo and compile it your self, or download precompiled binary from the [releases section](https://github.com/ReyeMe/ModelConverter-linux/releases).

> [!NOTE]
> Despite the name of the repository, it does work on windows.

### Preparing the model

For the Model we will start with a simple, slightly deformed cube, created in blender:

![](img/09_3D_model_pipeline_01.png)



## `SRL::Scene3D` namespace

SRL::Scene3D namespace is responsible for the rendering of 3D objects. This includes the camera , and the matrix stack, etc.