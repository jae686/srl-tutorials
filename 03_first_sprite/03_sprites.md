# Your First Sprite

The `SRL::Scene2D` class provides 6 different `DrawSprite` methods to draw sprites on the 2D plane.

However , before getting into the `DrawSprite` methods we must first know how to load the bitmap.
For this example, we will be loading a TGA file from the CD file system.

> [!NOTE]
> There are other ways to load bitmaps to use as sprites, but they are out of the scope of this tutorial.

## File system primer

When you compile a project in SLR, via the `compile.bat` , the files on the `cd\data` folder are written into the resulting CD image.
However the files must comply with the 8.3 format, case insensitive, like the old MS-DOS days. 
There is also a file count limit , that its set on the `makefile` at the root of the project, via the `SRL_MAX_CD_FILES` option.

## Sprite loading constraints

> [!WARNING]
> Be aware of the limitations of the sega saturn hardware.
> During TGA loading, some main ram is used to convert the TGA image into a format usable by VDP1 and VDP2.

Image width must be divisible by 8.


## Sprite loading

On this tutorial, the sprite loading into VDP1 ram is done in 2 steps :
- Create a `SRL::Bitmap::TGA` object to load from the CD file system into main RAM.
- Load it into VDP1 via the `SRL::VDP1::TryLoadTexture`
- Free the memory in main RAM occupied by the `SRL::Bitmap::TGA` object.
- Pass the loaded texture in into `SRL::Scene2D::DrawSprite`
- Profit.

```cpp

    SRL::Bitmap::TGA *tga = new SRL::Bitmap::TGA("TEST.TGA"); // Loads TGA file into main RAM
    int32_t textureIndex = SRL::VDP1::TryLoadTexture(tga);    // Loads TGA into VDP1
    delete tga;                                               // Frees main RAM

```

At the end, the `textureIndex` will contain the index of the texture. This value will then be passed into `SRL::Scene2D::DrawSprite`.


> [!TIP]
> It is very useful to test the return value of `SRL::VDP1::TryLoadTexture`. If its less then 0, the loading failed.


## Sprite Drawing

To actually draw the sprite into the screen SRL provides the `SRL::Scene2D::DrawSprite`.
Currently there are 6 overloads of this function.

### The simplest way

The most simple way is by using :
```cpp 
    static bool SRL::Scene2D::DrawSprite(const uint16_t	texture,  const SRL::Math::Types::Vector2D	points[4], const SRL::Math::Types::Fxp depth)
```

`texture` is the textureIndex returned by `SRL::VDP1::TryLoadTexture` , `points` is an array with 4 Vector2D with the points of the sprite. This array uses the same logic that was used on the previous chapter.
`depth` is used for Z sorting.



