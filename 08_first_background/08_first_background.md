# A Primer on VDP2

While the VDP1 is responsible to draw the sprites in the foreground, the VDP2 is, in general, responsible to handle the backgrounds.
On the sega saturn, after the VDP1 draws the frame buffer, the image is handled over to the VDP2 for further processing.

The images on the VDP2 are referred to as screens.

![](img/VDP1_VDP2_BlockDiagram.png)

The VDP2 supports 4 normal scroll screens and 2 rotation scroll screens.
The Rotation scroll screen will be dealt in a future tutorial.

In the table below there is an overview :

| Scroll Screen Name | Name | Notes |
| ------------------ | ---- | ----- |
| Normal Scroll Screen | NBG0 | Can move up/down/left/right. Can Scale |
| Normal Scroll Screen | NBG1 | Can move up/down/left/right. Can Scale |
| Normal Scroll Screen | NGB2 | Can move up/down/left/right. |
| Normal Scroll Screen | NGB3 | Can move up/down/left/right. |
| Rotation Scroll Screen | RBG0 | Can Rotate / Scale |
| Rotation Scroll Screen | RBG1 | Can Rotate / Scale |

> [!NOTE]
> RBG0 and RGB1 will be covered at a later tutorial, since they require use of the `SRL::Scene3D` namespace.

## Screen formats

There are 2 screen formats:

- Bitmap format
- Cell format

### Bitmap format

This is the most simple way to have a image into the VDP2.
The bitmap data is placed into a VDP2 screen.

SRL provides, through the `SRL::VDP2` namespace the methods to interact and set VDP2 scroll screens.

On SRL, we can do so by, for example, for the NBG1 screen:

```cpp
SRL::Bitmap::TGA* logo = new SRL::Bitmap::TGA("TITLE.TGA"); //Load Bitmap image to work RAM
SRL::VDP2::NBG1::LoadBitmap(logo);                          // Load Bitmap into NGB1                                        
delete logo;                                                //free original bitmap from work ram
```

There are some caveats when using [`LoadBitmap`](https://srl.reye.me/classSRL_1_1VDP2_1_1BmpScreen_a731038a2273de70dafaf30a3c5a9f7c2.html#a731038a2273de70dafaf30a3c5a9f7c2) function:

- The bitmap can only have the following sizes :  512x256, 512x512, 1024x256, or 1024x512
- The maximum supported loading size is 2 VRAM banks (262,144 bytes)

The relation between bit depth and VRAM usage is shown below.

| Bitdepth | Max Image Size | VRAM Usage |
| -------- | -------------- | ---------- |
| 4bpp | 1024x512 | 1/2, 1, or 2 banks |
| 8bpp | 512x512 or 1024x256 | 1 or 2 banks |
| 16bpp | 512x256 | Always 2 banks |

And to draw the VDP2 Scroll Screen we must call, on our render loop:

```cpp
SRL::VDP2::NBG1::SetPriority(SRL::VDP2::Priority::Layer2);  //set NBG1 priority
SRL::VDP2::NBG1::ScrollEnable();                            //enable display of NBG1
SRL::Core::Synchronize();                                   //Refresh screen      
```

As we use more scroll screens, we must set on which priority they are drawn. The higher the layer, the higher the priority.
And we must enable the display of our scroll screen.

The Example Code becomes :

```cpp
int main()
{
    // Initialize library
    SRL::Core::Initialize(HighColor::Colors::Black);
    SRL::Debug::Print(1,1, "08_Tutorial");
     
    SRL::Bitmap::TGA* logo = new SRL::Bitmap::TGA("TITLE.TGA"); //Load Bitmap image to work RAM
    SRL::VDP2::NBG1::LoadBitmap(logo);                          // Load Bitmap into NGB1                                        
    delete logo;                                                //free original bitmap from work ram                                                                                   

    SRL::VDP2::NBG1::SetPriority(SRL::VDP2::Priority::Layer2);  //set NBG1 priority
    SRL::VDP2::NBG1::ScrollEnable();                            //enable display of NBG1

    // Main program loop
 while(1)
 {       
        SRL::Core::Synchronize();                                   //Refresh screen                                           
 }

 return 0;
}
```

And this is the result:

![](img/first_background_01.png)

However we will want to manipulate out scroll screen (move, rotate, scale..)

### Translation

For translation we use the [`SetPosition()`](https://srl.reye.me/classSRL_1_1VDP2_1_1NBG1_af78d57a49bd7ccc3f04e3b6769bacc20.html#af78d57a49bd7ccc3f04e3b6769bacc20) function.

For example, move our screen along the Y axis, this is what our `main()` function looks like:

```cpp
int main()
{
    // Initialize library
	SRL::Core::Initialize(HighColor::Colors::Black);
    SRL::Debug::Print(1,1, "08_Tutorial");
     
    SRL::Bitmap::TGA* logo = new SRL::Bitmap::TGA("TITLE.TGA"); //Load Bitmap image to work RAM
    SRL::VDP2::NBG1::LoadBitmap(logo);                          // Load Bitmap into NGB1                                        
    delete logo;                                                //free original bitmap from work ram                                                                                   

    Vector2D offset = Vector2D(0.0, -50.0);                     //offset vector
    SRL::VDP2::NBG1::SetPriority(SRL::VDP2::Priority::Layer2);  //set NBG1 priority
    SRL::VDP2::NBG1::ScrollEnable();                            //enable display of NBG1

    // Main program loop
	while(1)
	{       
        SRL::VDP2::NBG1::SetPosition(offset);
        SRL::Core::Synchronize();                                   //Refresh screen                                           
        offset.Y = offset.Y - 1.0;
	}

	return 0;
}

```
And this is the result:

![](img/first_background_02.gif)

## Scale

Only `NBG0` and `NBG1` can scale.

To scale the layer, we use the  [`SetScale()`](https://srl.reye.me/classSRL_1_1VDP2_1_1NBG1_adcbf7bf416f13ef79e4462f83cdbe5e3.html#adcbf7bf416f13ef79e4462f83cdbe5e3) function.

To test our scaling , below there is an example of our `main` function:

```cpp

int main()
{
    // Initialize library
	SRL::Core::Initialize(HighColor::Colors::Black);
    SRL::Debug::Print(1,1, "08_Tutorial");
     
    SRL::Bitmap::TGA* logo = new SRL::Bitmap::TGA("TITLE.TGA"); //Load Bitmap image to work RAM
    SRL::VDP2::NBG1::LoadBitmap(logo);                          // Load Bitmap into NGB1                                        
    delete logo;                                                //free original bitmap from work ram                                                                                   

    Vector2D scale = Vector2D(0.1);
    SRL::VDP2::NBG1::SetPriority(SRL::VDP2::Priority::Layer2);  //set NBG1 priority
    SRL::VDP2::NBG1::ScrollEnable();                            //enable display of NBG1

    Fxp Cnt = 0.0;

    // Main program loop
	while(1)
	{       
        SRL::VDP2::NBG1::SetScale(scale);
        SRL::Core::Synchronize();                                   //Refresh screen                                           
        scale =  SRL::Math::Trigonometry::Sin(SRL::Math::Angle::FromDegrees(Cnt)*0.5);
        Cnt = Cnt + 0.5;       
	}

	return 0;
}
```

And this is the result (note that some artifacts in scaling are due to the .gif framerate):

![](img/first_background_03.gif)

## Cell Format

When using the cell format , the image information on the scroll screen is arranged as follows :

- Data is arranged into "cells", that consist on a 8x8 pixel image.
- Then those "cells" are arranged into a "character pattern" that consists on 1x1 or 2x2 cells.
- Then the "character patterns" are arranged into a "page" that consists of 32x32 or 64x64 character patterns.
- Then the "pages" are arranged into a plane, that can consists of a 1x1, 2x2, 1x2 or 2x1 pages.
- And finally (!) the planes are arranged into a "map" that , on a normal scroll surface consists of 2x2 planes, or, in a rotating plane, 4x4 planes.

The following picture makes it easier to understand :

![](img/first_background_cell_format.png)

This in practice means that one can, from a limited set of cells, make more intricate scrolls screens in less space that an equivalent bitmap scroll screen.

There are several was to create cell format scroll screens :

- Using `SRL::Tilemap::Interfaces::Bmp2Tile`
- Using [buhan's saturn-aseprite](https://github.com/buhman/saturn-aseprite) [Aseprite](https://www.aseprite.org/) plug-in
- Using sega's map editor for windows 95. (no , we wont be covering that)

### Notes on Sega's map editor

Due to existence of alternatives, we wont cover the official sega MapEdtor. However, documentation is provided below.
Documentation regarding sega map editor :

- [Mapeditor 1.81E Readme](https://antime.kapsi.fi/sega/files/MapEdit.pdf)
- [SS-SDK Win95 Graphic Tools - ver 1.0j](https://antime.kapsi.fi/sega/files/WGT_MAN.pdf)

### Using [`SRL::Tilemap::Interfaces::Bmp2Tile`](https://srl.reye.me/structSRL_1_1Tilemap_1_1Interfaces_1_1Bmp2Tile.html)

The `SRL::Tilemap::Interfaces::Bmp2Tile` creates a tilemap from a bitmap.

Some notes, from the [documentation](https://srl.reye.me/structSRL_1_1Tilemap_1_1Interfaces_1_1Bmp2Tile.html):

- Maximum Size of bitmap to convert is 0x20000 bytes (512x512 @ 4bpp, 512x256 @ 8bpp, or 256x256 @ 16bpp).
- Empty tiles in the source image are detected and removed from the tileset, but duplicate and mirrored tiles are not.
- In cases where bitmap is below maximum size or contains empty tiles, a default empty tile is written at start of tileset.

Is is done by, for example :

```cpp
SRL::Bitmap::TGA* MyBmp = new SRL::Bitmap::TGA("TITLE.TGA"); 
SRL::Tilemap::Interfaces::Bmp2Tile* Tile = new SRL::Tilemap::Interfaces::Bmp2Tile(*MyBmp,1);
delete MyBmp;//no longer need original bitmap in memory
```

And now we can Load the resulting tilemap into VDP2. In this case we can use `NGB0`.

```cpp
SRL::VDP2::NBG0::LoadTilemap(*Tile);
```

The resulting code is:

```cpp
int main()
{
    // Initialize library
	SRL::Core::Initialize(HighColor::Colors::Black);
    SRL::Debug::Print(1,1, "08_Tutorial"); 
    SRL::Bitmap::TGA* MyBmp = new SRL::Bitmap::TGA("TITLE.TGA"); 
    SRL::Tilemap::Interfaces::Bmp2Tile* Tile = new SRL::Tilemap::Interfaces::Bmp2Tile(*MyBmp,1);
    delete MyBmp;//no longer need original bitmap in memory
    
    //We will display right hand on NBG1 Layer in this example:
    SRL::VDP2::NBG0::LoadTilemap(*Tile);
    SRL::VDP2::NBG0::SetPriority(SRL::VDP2::Priority::Layer3);
    SRL::VDP2::NBG0::ScrollEnable();
  
    // Main program loop
	while(1)
	{       
       SRL::Core::Synchronize();                                                
	}

	return 0;
}
```

![](img/first_background_06.png)

The Tile interface allows for, programmatically, create several pages associated with a given tilemap, and copy data between pages.
This will be covered in a future tutorial covering the VDP2.


### Using [buhan's saturn-aseprite](https://github.com/buhman/saturn-aseprite) [Aseprite](https://www.aseprite.org/) script

> [!NOTE]
> As of 15.03.2026 only this [fork](https://github.com/seven-shades/saturn-aseprite/tree/patch-1) works.

As an example, we will have a tilemap created in [Aseprite](https://www.aseprite.org/).

![](img/first_background_04.png)

The example tilemap used has 128*128 resolution , with tiles with a 16x16 pixel size.

Once you have the scripts, in order to convert them to a format compatible with `SRL` you must use the following command :

```bash
PS D:\Development\Saturn\saturn-aseprite> python.exe .\background.py .\sprite_map.aseprite teste.bin
palette_size 512
8 8
pattern_name_table_size 4096
character_patterns_size 1280
7shades:
  file_type_id: 5
  size_of_cel_data: 1280
  size_of_map_data: 4096
  tile_character_size: 1
  tile_color_mode: 16
  plane_size: 0
  map_data: 0
  width: 1
  height: 1
```

The command syntax is as follows :

```bash
python background.py source_data.aseprite converted_file.bin
```
The output file `converted_file.bin` , can be loaded directly into SRL.

#### Loading the tilemap

Once you placed the `.bin` file under the `cd/data` folder , we can load it by using the `SRL::Tilemap::Interfaces::CubeTile` interface :

```cpp
auto tile = new SRL::Tilemap::Interfaces::CubeTile("TILEMAP1.BIN");     // load the tilemap into main ram
SRL::VDP2::NBG0::LoadTilemap(*tile);                                    // load tilemap into NGB0
delete tile;                                                            // free the tilemap copy in main ramm
SRL::VDP2::NBG0::SetPriority(SRL::VDP2::Priority::Layer3);              // set priority
SRL::VDP2::NBG0::ScrollEnable();                                        // enable NGB0
```

The resulting code is :

```cpp
int main()
{
    // Initialize library
	SRL::Core::Initialize(HighColor::Colors::Blue);
    SRL::Debug::Print(1,1, "08_Tutorial"); 

    auto tile = new SRL::Tilemap::Interfaces::CubeTile("TILEMAP1.BIN");
    SRL::VDP2::NBG0::LoadTilemap(*tile);
    delete tile;

    SRL::VDP2::NBG0::SetPriority(SRL::VDP2::Priority::Layer3);
    SRL::VDP2::NBG0::ScrollEnable();
  
    // Main program loop
	while(1)
	{       
       SRL::Core::Synchronize();                                                 
	}

	return 0;
}

```

And this is the result :

![](img/first_background_05.png)