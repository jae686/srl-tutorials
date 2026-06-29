
# A Primer on VDP2 RGB0 and RGB1

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


## Rotation axis

First me must set the rotation axis. This is done via the `SetRotationMode()`.

There are 3 modes available :

| Enumerator | Description | Notes |
| -------- | -------------- | ---------- |
| OneAxis | 2d rotation with only roll and zoom | No additional VRAM requirements |
| TwoAxis | 3d rotation with pitch and yaw, but no roll (modified per line) | Requires 0x2000-0x18000 bytes in arbitrary VRAM Bank (No cycles) |
| ThreeAxis | Full 3d rotation with pitch, yaw and roll (modified per pixel) | Requires 0x2000-0x18000 bytes in Reserved VRAM bank (8 cycles) |

For the rotation, we use the  [`SetScale()`](https://srl.reye.me/classSRL_1_1VDP2_1_1NBG1_adcbf7bf416f13ef79e4462f83cdbe5e3.html#adcbf7bf416f13ef79e4462f83cdbe5e3) function.

Lets begin with a `OneAxis` rotation.

> [!NOTE]
> Be mindfull of the `SetRotationMode()` selected, and the respective rotation axis!

| Rotation Mode | Rotate X | Rotate Y | Rotate Z |
| ------------- | -------- ! -------- | -------- |
| OneAxis      |  ?       | ?        | Rotates as expected |
| TwoAxis      | ?        | ?        | ? |
| ThreeAxis    | ?        | ?        | ? |

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


