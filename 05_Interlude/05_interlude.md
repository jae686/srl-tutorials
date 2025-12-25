# Interlude : Putting everything (so far) together

The goal of this chapter is making a simple program that uses all the concepts discussed in chapters 01 - 04 :
- Load Sprites from the CD
- Use the gamepad 1 for input - to move the sprite around
- Keep the code *manageable*

For starters , lets get back to the initial baseline :

```cpp
#include <srl.hpp>

// Using to shorten names for Vector , HighColor and Input
using namespace SRL::Types;
using namespace SRL::Math::Types;
using namespace SRL::Input;

// Main program entry
int main()
{
  // Initialize library
  SRL::Core::Initialize(HighColor::Colors::Black);
  SRL::Debug::Print(1,1, "05 - Interlude");
  while(1)
  {
    SRL::Core::Synchronize(); 
  }
  return 0;
}
```

If you recall the sprite loading code, from chapter 03 , it is something like this :

```cpp
SRL::Bitmap::TGA *tga = new SRL::Bitmap::TGA("TEST.TGA"); // Loads TGA file from cd into main RAM
int32_t textureIndex = SRL::VDP1::TryLoadTexture(tga);    // Loads Bitmap from main RAM into VDP1 RAM
if(textureIndex < 0) // Check if texture was properly loaded into VDP1 RAM
    {
         SRL::Debug::Print(1,2, "Loading Failed");
    }else
    {
        SRL::Debug::Print(1,2, "Loading OK , index : %d", textureIndex);
    }  
    delete tga;  // Free memory used by Bitmap in main RAM
```

## Modularize code

But if you use an arbitrary number of sprites, copy pasting code for each sprite we wish to load is **not** the way to go, specially if you value your sanity.
And since the only thing we care about, in our example, is to have the Sprite loaded into VDP1 ram and have its ID, we will wrap the sprite loading code into a function.

```cpp
int32_t loadTGA(char* filename)
    {
        SRL::Bitmap::TGA *tga = new SRL::Bitmap::TGA(filename); // Loads TGA file into main RAM
        int32_t textureIndex = SRL::VDP1::TryLoadTexture(tga);  // Loads TGA into VDP1
        delete tga;  
        return textureIndex;
    }
```

Now we have a simple function that I can call whenever I wish to load a sprite into VDP1. We can add a check to ensure the texture was properly loaded later.

So the code to draw a sprite, in the center of the screen becomes :

```cpp
#include <srl.hpp>

// Using to shorten names for Vector , HighColor and Input
using namespace SRL::Types;
using namespace SRL::Math::Types;
using namespace SRL::Input;

int32_t loadTGA(char* filename) //texture loading function
{
    SRL::Bitmap::TGA *tga = new SRL::Bitmap::TGA(filename); // Loads TGA file into main RAM
    int32_t textureIndex = SRL::VDP1::TryLoadTexture(tga);  // Loads TGA into VDP1
    delete tga;  
    return textureIndex;
}

int main() // Main program entry
{
  // Initialize library
  SRL::Core::Initialize(HighColor::Colors::Black);
  SRL::Debug::Print(1,1, "05 - Interlude");

  int32_t SpriteId = 0;
  SpriteId = loadTGA("TEST.TGA");

  while(1)
  {
    SRL::Scene2D::DrawSprite(SpriteId, Vector3D(0.0, 0.0, 500));
    SRL::Core::Synchronize(); 
  }
  return 0;
}
```








