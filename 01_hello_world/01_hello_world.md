# A Simple Hello World

## Anatomy of a SRL project

> [!NOTE]
> It is recomended to out your project files under `SaturnRingLib/Projects`

> [!TIP]
> It is more simple to copy a sample into the `Projects` folder and then add and change files as needed.

### Folder and File Structure :

+ `Your Project Folder`
  - `.gitignore`
  - `Makefile` This makefile has configuration settings for SRL.
  - `compile.bat` compiles the project
  - `clean.bat` cleans the build directory
  - `run_with_mendafen.bat` runs the project inside mendafen emulator
  - `src/` contains your project source files
  - `cd/` files to be placed on the final, burned CD. File such as TGA files, NYA files, eyc
  - `BuildDrop/` contains the final build ISO

### Main.cxx

All programs using SRL must use the `srl.hpp` header at the start.

``` 
#include <srl.hpp>
```

Is is also recomended (but not mandatory), for legibility issues, to use the apropriate namespaces.
SRL provides many datatypes designed to help with saturn development.

```
using namespace SRL::Types;
using namespace SRL::Math::Types;
```

SRL program entry point is, as usual with C++ programs, the `int main()` function.
Before the use of any SRL function, it must be initialized via the ``SRL::Core::Initialize`` function. A ´´SRL::Types::HighColor´´ containing the color must be provided.

> [!TIP]
> SRL already defines some colors that the programmer can use, without having to manually define its RGB values. For example `SRL::Types::HighColor::Colors::Black` for the back color. Now you can see why we define namespaces in order to ease the use of SRL defined types.

> [!IMPORTANT]
> SRL *MUST* be initialized before the use of any SRL functionality.

The code, so far, looks like this

```
#include <srl.hpp>
using namespace SRL::Types;
using namespace SRL::Math::Types;

int main()
{
   SRL::Core::Initialize(HighColor::Colors::Black);
   
   return 0;
}

```

Now that we have the SRL initialized, we can start using it.

We will begin how using the `SRL::Debug` class.

This class and its member functions are used to print information to the screen, and it's meant to aid in debuging tasks.



