# Your Second Sprite

On [Chapter 3](../03_first_sprite/03_sprites.md) we covered the simplest way to display, rotate and resize sprites.

However there might be situations where more control on the sprites is needed : for example to apply custom transforms to the sprite's vertices.

## Sprite Drawing - Redux

On [Chapter 3](../03_first_sprite/03_sprites.md) we've drawn sprites only by specifing the `bitmap ID`, sprite `position`, and optionally `scale`, `rotation` and `zoompoint`.

Although the functions themselves are quite simple to use they do not alow for us to deform the sprite.

For this there are 2 overloads that allow for us to specify the vertices of the quad where the sprite will be mapped to.
This vertices can be transformed as we wish.

```cpp
static bool SRL::Scene2D::DrawSprite ( const uint16_t texture,
    const SRL::Math::Types::Vector2D points[4],
    const SRL::Math::Types::Fxp depth )

```

and

```cpp
static bool SRL::Scene2D::DrawSprite ( const uint16_t texture,
    SRL::CRAM::Palette * texturePalette,
    const SRL::Math::Types::Vector2D points[4],
    const SRL::Math::Types::Fxp depth )
```

> [!NOTE]
> Notice that these functions do not have rotation, scale or ZoomPoint. This is because, with these functions, you are expected to transform the sprite's edge coordinates yourself.

The use of `SRL::CRAM::Palette` will be covered at a late tutorial.

## A Distorted sprite

To start lets specify a simple quad. This Quad is where out sprite will be mapped into. Bear in mind that there are no UV coordiates : the whole sprite is fully mapped into the quad.

```cpp

 Vector2D points[4] = {Vector2D(0.0)};
    points[0] = Vector2D(-50, -50);
    points[1] = Vector2D(50,  -50);
    points[2] = Vector2D( 50,  50);
    points[3] = Vector2D(-50,  50);

```

and use out loader function as specified on [chapter 5](05_interlude/05_interlude.md):

```cpp
int32_t loadTGA(char* filename)
    {
        SRL::Bitmap::TGA *tga = new SRL::Bitmap::TGA(filename); // Loads TGA file into main RAM
        int32_t textureIndex = SRL::VDP1::TryLoadTexture(tga);  // Loads TGA into VDP1
        delete tga;  
        return textureIndex;
    }
```

Now the finished source code looks like this :

```cpp
#include <srl.hpp>

// Using to shorten names for Vector and HighColor
using namespace SRL::Types;
using namespace SRL::Math::Types;

int32_t loadTGA(char* filename) //texture loading function
    {
        SRL::Bitmap::TGA *tga = new SRL::Bitmap::TGA(filename); // Loads TGA file into main RAM
        int32_t textureIndex = SRL::VDP1::TryLoadTexture(tga);  // Loads TGA into VDP1
        delete tga;  
        return textureIndex;
    }


int main()
{
    // Initialize library
	SRL::Core::Initialize(HighColor::Colors::Black);
    SRL::Debug::Print(1,1, "06_Tutorial");

    int32_t textureIndex = loadTGA("TEST.TGA");    // Loads TGA into VDP1

    Vector2D points[4] = {Vector2D(0.0)};
    points[0] = Vector2D(-50, -50);
    points[1] = Vector2D(50,  -50);
    points[2] = Vector2D( 50,  50);
    points[3] = Vector2D(-50,  50);   
        
    // Main program loop
	while(1)
	{   
        SRL::Scene2D::DrawSprite ( textureIndex,  points, 50.0 );
        // Refresh screen
        SRL::Core::Synchronize();
	}

	return 0;
}
```

The Result :

![](img/second_sprite_01.png)

From this, we can now start to experiment with the coordiates.
Lets, for example, make the quad larger in the top :

```cpp
    points[0] = Vector2D(-75, -50);
    points[1] = Vector2D(75,  -50);
    points[2] = Vector2D( 50,  50);
    points[3] = Vector2D(-50,  50);   
```

The resulting quad :

![](img/second_sprite_02.png)


