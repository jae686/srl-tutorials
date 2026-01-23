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

To start lets specify a simple quad. This Quad is where our sprite will be mapped into. Bear in mind that there are no UV coordiates : the whole sprite is fully mapped into the quad.

```cpp

 Vector2D points[4] = {Vector2D(0.0)};
    points[0] = Vector2D(-50, -50);
    points[1] = Vector2D(50,  -50);
    points[2] = Vector2D( 50,  50);
    points[3] = Vector2D(-50,  50);

```

and use our loader function as specified on [chapter 5](05_interlude/05_interlude.md):

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
        SRL::Scene2D::DrawSprite(textureIndex,  points, 50.0 );
        // Refresh screen
        SRL::Core::Synchronize();
	}

	return 0;
}
```

The Result :

![](img/second_sprite_01.png)

From this, we can now start to experiment with the coordiates.
Lets, for example, make the quad larger at the top :

```cpp
    points[0] = Vector2D(-75, -50);
    points[1] = Vector2D(75,  -50);
    points[2] = Vector2D( 50,  50);
    points[3] = Vector2D(-50,  50);   
```

The resulting quad :

![](img/second_sprite_02.png)

## Manual Transforms

We can scale , rotate and transform the points of our quad.
For this, SRL provides matrices to help.

### Rotation

Since we are on the XY plane, we must rotatate along the Z axis.

Therefore, using SRL we can first declare our rotation matrix.
We must provide to the `CreateRotationZ` function the rotation angle.
SRL also provides functions to create rotation in X and Y as well.

In SRL is really simple :

```cpp
    //Declare and initialize our 3x3 matrix
    Matrix33 transform = Matrix33::Identity();
    //Lets create a rotation matrix
    transform = transform.CreateRotationZ(SRL::Math::Angle::FromDegrees(45.0));
```

Our sprite point coordinates are in 2 component vectors. We must create a 3 component vectors in order to multiply the vertex position by the rotation matrix. Then we multiply each point and copy the X and Y values into the `Vector2D` array that will be provided to `SRL::Scene2D::DrawSprite`.

```cpp
Vector3D vec3_points[4] = {Vector3D(0.0)};
    vec3_points[0] = Vector3D(points[0], 1.0);
    vec3_points[1] = Vector3D(points[1], 1.0);
    vec3_points[2] = Vector3D(points[2], 1.0);
    vec3_points[3] = Vector3D(points[3], 1.0);

    for(int i = 0 ; i < 4 ; i++)
    {
       //multiply matrix
       vec3_points[i] = transform *  vec3_points[i];
       // get back to vector2D type that  SRL::Scene2D::DrawSprite accepts
       points[i].X = vec3_points[i].X;
       points[i].Y = vec3_points[i].Y;
    }
```

The result:

![](img/second_sprite_03.png)

> [!WARNING]
> A quick note on animations
> One might be tempted to , in order to make for example, make an animation of a sprite rotating , by multiplying the rotated points from a previous frame by the rotation matrix.
> This will *compound* the rounding errors due to precision loss.

![](mov/error_compounds.gif)