
# A Primer on VDP2 RBG0 and RBG1

A reminder on VDP2 layers, covered on the [08_first_background](../08_first_background/08_first_background.md) :

In the table below there is an overview:

| Scroll Screen Name | Name | Notes |
| ------------------ | ---- | ----- |
| Normal Scroll Screen | NBG0 | Can move up/down/left/right. Can Scale |
| Normal Scroll Screen | NBG1 | Can move up/down/left/right. Can Scale |
| Normal Scroll Screen | NGB2 | Can move up/down/left/right. |
| Normal Scroll Screen | NGB3 | Can move up/down/left/right. |
| Rotation Scroll Screen | RBG0 | Can Rotate / Scale |
| Rotation Scroll Screen | RBG1 | Can Rotate / Scale |

What makes the `RBG0` and `RBG1` layers distinct is the ability to rotate.
This allows for effects such as the famous "MODE 7" effect on games like Mario Kart on the SNES.

### Diferences between Rotation scroll screen and normal scroll screens

While on the normal scroll screens, there are dedicated functions for translation, and scaling if applicable. For the rotation scroll screen, Scaling, Translation and Rotation are done by manipulation the SRL matrix stack, the same way you manipulate 3D scenes / objects, via `SRL::Scene3D::Scale()` , `SRL::Scene3D::Translate()` and `SRL::Scene3D::Rotate_()` functions.

You must call `SRL::VDP2::RBG0::SetCurrentTransform();` to apply the transform to rotation scroll screen.

## Baseline

Lets start with a simple scene:

```cpp

#include <srl.hpp>

// Using to shorten names for Vector and HighColor
using namespace SRL::Types;
using namespace SRL::Math::Types;

// Main program entry
int main()
{
    // Initialize library
    SRL::Core::Initialize(HighColor(0x31, 0x14, 0x32));
    SRL::Debug::Print(1,1, "VDP2 - RGB0 Tutorial");

    SRL::Bitmap::TGA* MyBmp = new SRL::Bitmap::TGA("CHK.TGA"); 
    SRL::Tilemap::Interfaces::Bmp2Tile* Tile = new SRL::Tilemap::Interfaces::Bmp2Tile(*MyBmp,1);
    delete MyBmp;
 
    SRL::VDP2::RBG0::LoadTilemap(*Tile);
    SRL::VDP2::RBG0::SetPriority(SRL::VDP2::Priority::Layer2);
    SRL::VDP2::RBG0::ScrollEnable();
   
   // Main program loop
	while(1)
	{
        SRL::Scene3D::LoadIdentity();
        SRL::VDP2::RBG0::SetCurrentTransform();
        
        // Refresh screen
        SRL::Core::Synchronize();

	}

	return 0;
}
```

This Loads a simple 16x16 tile into VDP2.

This is the result :

![](img/11_second_background_01.png)

Note that we loaded the Identity Matrix into the SRL matrix stack. Then we applied it to RBG0 rotating plane by using `SRL::VDP2::RBG0::SetCurrentTransform();`

## Filling the scroll screen

As you noticed, having just a single image on VDP2 is not very interesting. You can make your own tilemaps as shown on [08_first_background](../08_first_background/08_first_background.md), or we can do it programatically.

SRL provides the [`CopyMap()`](https://srl.reye.me/structSRL_1_1Tilemap_1_1Interfaces_1_1Bmp2Tile_aadc8863cc9ea5ae0c4f1bfc663f06cde.html#aadc8863cc9ea5ae0c4f1bfc663f06cde) function to copy tilemap data *before* we load it into the corresponding VDP2 scroll screen.

For example :

```cpp
for(int i = 0 ; i < 16 ; i++)
    {
        for(int j = 0 ; j < 16 ; j++)
        {
           Tile->CopyMap(0,SRL::Tilemap::Coord(0,0), SRL::Tilemap::Coord(1,1), 0,SRL::Tilemap::Coord(i,j) );
        }
    }
```

Will copy our initial image along the tilemap : 16 times along x and 16 times along y.

The result :

![](img/11_second_background_02.png)

However, where will be situations where you might want the image to tile along the *whole* scroll screen.

To do so, the total tile size must be 512 * 512.

Since our original tile map is 16*16, to cover 512x512 we must copy the original tile 32 times (512/16).

So if we modify our code to :

```cpp
for(int i = 0 ; i < 32 ; i++)
{
    for(int j = 0 ; j < 32 ; j++)
    {
        Tile->CopyMap(0,SRL::Tilemap::Coord(0,0), SRL::Tilemap::Coord(1,1), 0,SRL::Tilemap::Coord(i,j) );
    }
}
```

We now get :

![](img/11_second_background_03.png)



## Rotation axis

First me must set the rotation axis. This is done via the `SetRotationMode()`.

There are 3 modes available :

| Enumerator | Description | Notes |
| -------- | -------------- | ---------- |
| OneAxis | 2d rotation with only roll and zoom | No additional VRAM requirements |
| TwoAxis | 3d rotation with pitch and yaw, but no roll (modified per line) | Requires 0x2000-0x18000 bytes in arbitrary VRAM Bank (No cycles) |
| ThreeAxis | Full 3d rotation with pitch, yaw and roll (modified per pixel) | Requires 0x2000-0x18000 bytes in Reserved VRAM bank (8 cycles) |


> [!NOTE]
> Be mindfull of the `SetRotationMode()` selected, and the respective rotation axis!

| Rotation Mode | Rotate X | Rotate Y | Rotate Z |
| ------------- | --------|  -------- | -------- |
| OneAxis      |  Scales in Y       | Scales in X        | Rotates as expected |
| TwoAxis      | ?        | ?        | ? |
| ThreeAxis    | ?        | ?        | ? |

### OneAxis Rotation

The plane is on the XY plane.
Therefore, to perform the expected rotation, we must rotate-it to the axis perpendicular to the XY plane : the Z axis.
In order to make the `Scene3D` transforms affect the RBG plane, we must call `SRL::VDP2::RBG0::SetCurrentTransform();`.

In this example, we declare an angle variable and increment it at each frame.

The resulting code is :

```cpp
#include <srl.hpp>

// Using to shorten names for Vector and HighColor
using namespace SRL::Types;
using namespace SRL::Math::Types;

// Main program entry
int main()
{
    // Initialize library
	SRL::Core::Initialize(HighColor(0x31, 0x14, 0x32));
    SRL::Debug::Print(1,1, "VDP2 - RGB0 Tutorial");

    SRL::Bitmap::TGA* MyBmp = new SRL::Bitmap::TGA("CHK.TGA"); 
    SRL::Tilemap::Interfaces::Bmp2Tile* Tile = new SRL::Tilemap::Interfaces::Bmp2Tile(*MyBmp,1);
    delete MyBmp;



    for(int i = 0 ; i < 32 ; i++)
    {
        for(int j = 0 ; j < 32 ; j++)
        {
           Tile->CopyMap(0,SRL::Tilemap::Coord(0,0), SRL::Tilemap::Coord(1,1), 0,SRL::Tilemap::Coord(i,j) );
        }
    }
    
    SRL::VDP2::RBG0::LoadTilemap(*Tile);
    SRL::VDP2::RBG0::SetPriority(SRL::VDP2::Priority::Layer2);
    SRL::VDP2::RBG0::SetRotationMode(SRL::VDP2::RotationMode::OneAxis);
    SRL::VDP2::RBG0::ScrollEnable();


    Angle angle = 0;

    // Main program loop
	while(1)
	{
        SRL::Scene3D::LoadIdentity();
        SRL::Scene3D::RotateZ(angle);
        SRL::VDP2::RBG0::SetCurrentTransform();
        
        // Refresh screen
        SRL::Core::Synchronize();
        angle += Angle::FromDegrees(0.5);
	}

	return 0;
}
```

And this is the resulting image :

![](img/11_second_background_04.gif)

#### Rotations on non-perpendicular axis

What would happen if we rotated on a different axis than Z ?

##### Rotation no X

Causes distortion on Y axis.

![](img/11_second_background_05.gif)

##### Rotation on Y axis

Causes distortion on X axis.

![](img/11_second_background_06.gif)

##### Rotation on both axis

Combination of the both distortions above. Apparent rotation direction depends on transform order.

![](img/11_second_background_07.gif)

### Two Axis Rotation

If you just take the code from the One Axis rotation, and set the rotation mode to  `SRL::VDP2::RotationMode::TwoAxis` , you will simply get a distorted image, in this case red (due to the sprite used).

![](img/11_second_background_08.png)

#### Applying transforms

In order to see the plane properly , we need to transform the plane.

> [!NOTE]
> We are assuming -Y is up.
>
> We are assuming the standard rotation order : RotX , Rot Y and Rot Z.

Since the `SRL::Scene3D` transforms can affect the RBG plane, we can simply set a camera like we would on a 3D scene.

Out main loop now looks like this :

```cpp
while(1)
{
    SRL::Scene3D::LoadIdentity();
    SRL::Scene3D::LookAt(cameraLocation, Vector3D(), Angle::FromDegrees(0.0));
    SRL::Scene3D::RotateX(Angle::FromDegrees(90.0)); // go from XY to XZ plane!
    SRL::VDP2::RBG0::SetCurrentTransform();        
      
    // Refresh screen
    SRL::Core::Synchronize();           
}

```

And this is the result :

![](img/11_second_background_09.png)

If we now rotate on the Z axis, we get the expected rotation :

![](img/11_second_background_10.gif)

What happens if we rotate by Y ?

![](img/11_second_background_11.gif)

Furthermore, since we are using the same functions to transform both the RBG planes and 3D Space we can draw 3D models and they will share the same transforms.

![](img/11_second_background_12.gif)

However, one must be mindful of the side effects of the sega saturn architecture : the VDP2 image is layered behind the VDP1 frame. This can lead to some interesting inconsistencies as seen below on a X rotation with a 3D mesh on the same scene.

![](img/11_second_background_13.gif)

