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



