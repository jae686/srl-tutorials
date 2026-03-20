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

### Preparing the our first model

For the Model we will start with a simple, slightly deformed cube, created in blender:

![](img/09_3D_model_pipeline_01.png)

And we export it to `.obj` with the following settings :

![](img/09_3D_model_pipeline_02.png)

To convert our `.obj` to `.NYA` we use the following command :

```bash
PS D:\Development\Saturn\ModelConverter> .\ModelConverter.exe -i "..\SaturnRingLib\Projects\09_3D model_pipeline\assets\cube_01.obj" -o "..\SaturnRingLib\Projects\09_3D model_pipeline\assets\cube_01.NYA"
Input files:
cube_01.obj
Output file: D:\Development\Saturn\SaturnRingLib\Projects\09_3D model_pipeline\assets\cube_01.NYA
Using import plugin 'Wavefront' for 'cube_01.obj'
Using export plugin: NyaExport
Exporting type: 'NoLight'
UV mapping enabled: 'True'
UV mapping generated 0 texture
Texture data size 0 bytes
Export done.
 ```

### Getting the model into our saturn project

We copy the resulting `.NYA` to the `cd/data` folder of our saturn project.

First we must include the `modelObject.hpp` at the top of our source file, and in our main function we instantiate the class.

```cpp
ModelObject teapot = ModelObject("cube_01.NYA");
```

We will also need coordinates for out camera. For this we use the `SRL::Math::Types::Vector3D` :

```cpp
Vector3D cameraLocation = Vector3D(12.5);
```

Our file becomes :

```cpp
#include <srl.hpp>
#include "modelObject.hpp"

// Using to shorten names for Vector and HighColor
using namespace SRL::Types;
using namespace SRL::Math::Types;

int main()
{
    // Initialize library
    SRL::Core::Initialize(HighColor::Colors::Black);
    SRL::Debug::Print(1,1, "09_Tutorial"); 
  
    ModelObject cube = ModelObject("CUBE01.NYA");
    Vector3D cameraLocation = Vector3D(12.5);
  
    // Main program loop
    while(1)
    {       
       // Load identity matrix
       SRL::Scene3D::LoadIdentity();

       // Set camera location and direction
       SRL::Scene3D::LookAt(cameraLocation, Vector3D(), Angle::FromDegrees(0.0));

       cube.Draw();
       SRL::Core::Synchronize();                                                
    }

    return 0;
}
```

And this is the result :

![](img/09_3D_model_pipeline_03.png)

But its not very interesting, isn't it ?

### Basic lightning

## `SRL::Scene3D` namespace

SRL::Scene3D namespace is responsible for the rendering of 3D objects. This includes the camera , and the matrix stack, etc.