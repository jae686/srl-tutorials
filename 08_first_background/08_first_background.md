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

## Screen formats

There are 2 screen formats:

- Bitmap format
- Cell format

### Bitmap format

This is the most simple way to have a image into the VDP2.
The bitmap data is placed into a VDP2 screen.

SRL provides, through the `SRL::VDP2` namespace the methods to interact and set or VDP2 scroll screens.

On SRL, we can do so by , for example for the NBG1 screen:

```cpp
SRL::Bitmap::TGA* logo = new SRL::Bitmap::TGA("TITLE.TGA");                                         //Load Bitmap image to work RAM
SRL::Tilemap::Interfaces::Bmp2Tile* TestTilebmp = new SRL::Tilemap::Interfaces::Bmp2Tile(*logo);    //convert bitmap to tilemap
SRL::VDP2::NBG1::LoadTilemap(*TestTilebmp);             //Transfer tilemap from work RAM to VDP2 VRAM and register with NBG2
delete TestTilebmp;                                                                                 //free tilemap from work ram 
delete logo;                                                                                        //free original bitmap from work ram
```

And to Draw the VDP2 Scroll Screen we must call, on our render loop:

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
     
    //Demonstrate NBG1 loading with Tilemap converted from Bitmap:
    SRL::Bitmap::TGA* logo = new SRL::Bitmap::TGA("TITLE.TGA");                                         //Load Bitmap image to work RAM
    SRL::Tilemap::Interfaces::Bmp2Tile* TestTilebmp = new SRL::Tilemap::Interfaces::Bmp2Tile(*logo);    //convert bitmap to tilemap
    SRL::VDP2::NBG1::LoadTilemap(*TestTilebmp);             //Transfer tilemap from work RAM to VDP2 VRAM and register with NBG2
    delete TestTilebmp;                                                                                 //free tilemap from work ram 
    delete logo;                                                                                        //free original bitmap from work ram

    // Main program loop
	while(1)
	{       
        SRL::VDP2::NBG1::SetPriority(SRL::VDP2::Priority::Layer2);  //set NBG1 priority
        SRL::VDP2::NBG1::ScrollEnable();                            //enable display of NBG1
        SRL::Core::Synchronize();                                   //Refresh screen                                           
	}

	return 0;
}
```

And this is the result :

![](img/first_background_01.png)
