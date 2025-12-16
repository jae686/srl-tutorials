# A Simple Hello World

## Anatomy of a SRL project

> [!NOTE]
> It is recommended to out your project files under `SaturnRingLib/Projects`

> [!TIP]
> It is more simple to copy a sample into the `Projects` folder and then add or change files as needed.

### Folder and File Structure :

+ `Your Project Folder`
  - `.gitignore`
  - `Makefile` This makefile has configuration settings for SRL.
  - `compile.bat` compiles the project
  - `clean.bat` cleans the build directory
  - `run_with_mendafen.bat` runs the project inside mendafen emulator
  - `src/` contains your project source files
  - `cd/` files to be placed on the final, burned CD. File such as TGA files, NYA files, etc
  - `BuildDrop/` contains the final build ISO

### src/main.cxx

> [!NOTE]
> This tutorial assumes that a empty `main.cxx` file exists on the `<project folder>/src` folder.

All programs using SRL must use the `srl.hpp` header at the start.

```cpp 
#include <srl.hpp>
```

Is is also recommended (but not mandatory), for legibility issues, to use the appropriate namespaces.
SRL provides many datatypes designed to help with saturn development.

```cpp
using namespace SRL::Types;
using namespace SRL::Math::Types;
```

SRL program entry point is, as usual with C++ programs, the `int main()` function.
Before the use of any SRL function, it must be initialized via the `SRL::Core::Initialize` function. A [`SRL::Types::HighColor`](https://srl.reye.me/structSRL_1_1Types_1_1HighColor.html) containing the color must be provided.

> [!TIP]
> SRL already defines some [colors that the programmer can use](https://srl.reye.me/classSRL_1_1Types_1_1HighColor_1_1Colors.html), without having to manually define its RGB values. For example `SRL::Types::HighColor::Colors::Black` for the black color. Now you can see why we define namespaces in order to ease the use of SRL defined types.

> [!IMPORTANT]
> SRL *MUST* be initialized before the use of any SRL functionality.

The code, so far, looks like this

```cpp
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

We will begin how using the [`SRL::Debug`](https://srl.reye.me/classSRL_1_1Debug.html) class.

This class and its member functions are used to print information to the screen, and it's meant to aid in debugging tasks. In other words, you will be using it a lot :) 

To print a text to the screen, the [`SRL::Debug::Print`](https://srl.reye.me/classSRL_1_1Debug_afa892baf3e31d364ffe07350c916696f.html#afa892baf3e31d364ffe07350c916696f) function is used. There are 2 overloads : one that allows the print a simple string, and another that is a [variadic function](https://srl.reye.me/classSRL_1_1Debug_a4ab210527af751fedbbc8877a019252f.html#a4ab210527af751fedbbc8877a019252f) that takes arguments in the same way as the classic `printf()` function.

Using this function can simply be done with :

```cpp
SRL::Debug::Print(1,1, "Hello World");
```

This should print the "Hello World" string at the x=1 ,y= 1 coordinates.

This will be the resulting program :

```cpp
#include <srl.hpp>

// Using to shorten names for Vector and HighColor
using namespace SRL::Types;
using namespace SRL::Math::Types;

// Main program entry
int main()
{
  SRL::Core::Initialize(HighColor::Colors::Black);
  SRL::Debug::Print(1,1, "Hello World");
  return 0;
}
```

**However this will not print anything on the screen.**

> [!IMPORTANT]
> At the end of each frame the function [`SRL::Core::Synchronize()`](https://srl.reye.me/classSRL_1_1Core_a06c60715afe1f84b01286b5d7bc269e7.html#a06c60715afe1f84b01286b5d7bc269e7) must be called. Otherwise nothing will be drawn at the end of the frame.
>

The correct, working program looks like :

```cpp
#include <srl.hpp>

// Using to shorten names for Vector and HighColor
using namespace SRL::Types;
using namespace SRL::Math::Types;

// Main program entry
int main()
{
  // Initialize library
  SRL::Core::Initialize(HighColor::Colors::Black);
  SRL::Debug::Print(1,1, "Hello World");
  SRL::Core::Synchronize(); 
  return 0;
}
```

To compile the project , in windows, you simple run the `compile.bat` in the project folder.
To run the project inside an emulator with `run_with_mednafen.bat` in the project folder.

## Sumary

On this short tutorial you learned that :
- To start using the library its recommended that you copy a sample program in the `samples` folder into the `Projects` folder, and modify and add from there. This ensures the helper scripts and makefile are already in place.
- Every SRL project must:
  - Include `srl.hpp` header
  - Initialize the SRL library before using any `SRL` function
  - Call `SRL::Core::Synchronize();` at the end of each frame
-  `SRL::Debug::Print(int x, int y, char* text)` can be used to print text into the screen.




