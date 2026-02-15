# Sprite Effects

The VDP1 supports a series of ways to render the quads into the screen.
The effects are set via the `SRL::Scene2D::SetEffect` function.

## Baseline

Lets add 2 distinct, overlapping sprites :

```cpp
int main()
{
    // Initialize library
	SRL::Core::Initialize(HighColor::Colors::Black);
    SRL::Debug::Print(1,1, "07_Tutorial");
   
    int32_t textureIndex = loadTGA("TEST.TGA");    // Loads TGA into VDP1
    int32_t chkTexture = loadTGA("CHK.TGA");       // Loads TGA into VDP1

    Vector2D center_sprite[4] = {Vector2D(0.0)}; // holds center sprite position.
    center_sprite[0] = Vector2D(-50, -50);
    center_sprite[1] = Vector2D( 50, -50);
    center_sprite[2] = Vector2D( 50,  50);
    center_sprite[3] = Vector2D(-50,  50); 
    
    Vector2D offset = Vector2D(25.0 , 25.0);
    Vector2D second_sprite[4] = {Vector2D(0.0)};

    for(int i = 0 ; i < 4 ; i++)
    {
        second_sprite[i] = center_sprite[i] + offset; // create the points for thew 2nd sprite
    }

    // Main program loop
	while(1)
	{       
        SRL::Scene2D::DrawSprite ( textureIndex,  center_sprite, 50.0 );             //draw the center sprite
        SRL::Scene2D::DrawSprite ( chkTexture,  second_sprite, 50.0 );               //draw the offset sprite                 
        SRL::Core::Synchronize();                                                   // Refresh screen
	}

	return 0;
}

```

And this is the result.

![](img/spriteEffects_01.png)

## Setting Effects

In order to set effects it is done via the `static void SRL::Scene2D::SetEffect(const SpriteEffect	effect, const int32_t data = -1)` function [documentation](https://srl.reye.me/classSRL_1_1Scene2D_a2f702eaaf22b82345520cce09908f386.html#a2f702eaaf22b82345520cce09908f386).

### Half Transparency

And now lets set a Half Transparency effect.
To do so, we must use the function `SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::HalfTransparency, true);` before our `SRL::Scene2D::DrawSprite` call.

The code then becomes :

```cpp
while(1)
	{       
        SRL::Scene2D::DrawSprite(textureIndex,  center_sprite, 50.0);                //draw the center sprite
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::HalfTransparency, true); //Enable the effect
        SRL::Scene2D::DrawSprite(chkTexture,  second_sprite, 50.0);                  //draw the offset sprite   
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::HalfTransparency);       //Disable the effect          
        SRL::Core::Synchronize();                                                    //Refresh screen
	}
```
The Result:

![](img/spriteEffects_02.png)

### Screen doors

For that Saturn look, look no further than the `ScreenDoors` effect.
For the `ScreenDoors` effect, you use the `SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::ScreenDoors, true);` function.

```cpp
while(1)
	{       
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::ScreenDoors, true); //Enable the effect
        SRL::Scene2D::DrawSprite (textureIndex,  center_sprite, 50.0);        //draw the center sprite
        SRL::Scene2D::DrawSprite (chkTexture,  second_sprite, 50.0);          //draw the offset sprite   
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::ScreenDoors);       //Disable the effect          
        SRL::Core::Synchronize();                                               //Refresh screen
	}
```
The Result:

![](img/spriteEffects_03.png)

### Combining effects

You can also combine `ScreenDoors` and `HalfTransparency`.

```cpp
while(1)
	{       
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::HalfTransparency, true); //Enable the effect
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::ScreenDoors, true);      //Enable the effect
        SRL::Scene2D::DrawSprite(textureIndex,  center_sprite, 50.0);             //draw the center sprite
        SRL::Scene2D::DrawSprite(chkTexture,  second_sprite, 50.0);               //draw the offset sprite   
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::ScreenDoors);            //Disable the effect
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::HalfTransparency);       //Disable the effect            
        SRL::Core::Synchronize();                                                    //Refresh screen
	}
```

![](img/spriteEffects_04.png)

### Flip

The `Flip` effect is exactly what it says. This will flip the sprite on X, Y or both axis.

For example :
#### `SRL::Scene2D::FlipEffect::HorizontalFlip` 

```cpp
while(1)
	{       
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::Flip, SRL::Scene2D::FlipEffect::HorizontalFlip); //Enable the effect    
        SRL::Scene2D::DrawSprite ( textureIndex,  center_sprite, 50.0 );             //draw the center sprite 
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::Flip);                   //Disable the effect            
        SRL::Core::Synchronize();                                                    // Refresh screen
	}
```

![](img/spriteEffects_05.png)

#### `SRL::Scene2D::FlipEffect::VerticalFlip`

```cpp
while(1)
	{       
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::Flip, SRL::Scene2D::FlipEffect::VerticalFlip); //Enable the effect    
        SRL::Scene2D::DrawSprite ( textureIndex,  center_sprite, 50.0 );                                   //draw the center sprite   
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::Flip);                                         //Disable the effect            
        SRL::Core::Synchronize();                                                                          // Refresh screen
	}
```

![](img/spriteEffects_06.png)

#### Combining both `SRL::Scene2D::FlipEffect::HorizontalFlip` and `SRL::Scene2D::FlipEffect::VerticalFlip`

   
```cpp
while(1)
	{       
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::Flip, SRL::Scene2D::FlipEffect::VerticalFlip | SRL::Scene2D::FlipEffect::HorizontalFlip);   
        SRL::Scene2D::DrawSprite ( textureIndex,  center_sprite, 50.0 );             //draw the center sprite   
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::Flip);       //Disable the effect            
        SRL::Core::Synchronize();                                                    // Refresh screen
	}
```

![](img/spriteEffects_07.png)


### Clipping

The clipping effect allows you to define a "clipping window", where you can set if the affected sprite is drawn inside or outside of it.

First we must define the clipping window.
This is done via the `static bool SRL::Scene2D::SetClippingRectangle(const SRL::Math::Types::Vector3D &location, const SRL::Math::Types::Vector2D &size )` function. [documentation](https://srl.reye.me/classSRL_1_1Scene2D_a73100f8cb69e1a46092bd997bc9aed3b.html#a73100f8cb69e1a46092bd997bc9aed3b).

We must provide the location of our clipping window and its size.

> [!WARNING]
> The top left corner of the screen is (0,0)!

> [!WARNING]
> If no Clipping Rectangle is specified, the sprites are not drawn.

After our clipping rectangle is defined, we can enable our clipping.
And there are 3 clipping modes to choose from :

### `SRL::Scene2D::ClippingEffect::ClipInside`

It will clip top sprite inside of the clipping window.
In our example, it will show the sprite underneath the sprite that was drawn on top.

The code becomes :

```cpp
Vector3D clip_square = Vector3D(160.0 , 120.0, 50); // holds center sprite position.
Vector2D square_size = Vector2D(50,50);

// Main program loop
while(1)
        {       
                SRL::Scene2D::SetClippingRectangle(clip_square, square_size);
                SRL::Scene2D::DrawSprite ( textureIndex,  center_sprite, 50.0 );             //draw the center sprite 
                SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::Clipping, SRL::Scene2D::ClippingEffect::ClipInside);
                SRL::Scene2D::DrawSprite ( chkTexture,  second_sprite, 50.0 );               //draw the offset sprite   
                SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::Clipping);                
                SRL::Core::Synchronize();                                                    // Refresh screen
	}
```
The result :

![](img/spriteEffects_08.png)

### `SRL::Scene2D::ClippingEffect::ClipOutside`

The `SRL::Scene2D::ClippingEffect::ClipOutside` option will clip everything that is outside of the clipping window.

The code below:

```cpp
Vector3D clip_square = Vector3D(160.0 , 120.0, 50); // holds center sprite position.
Vector2D square_size = Vector2D(50,50);

// Main program loop
while(1)
	{       
                SRL::Scene2D::SetClippingRectangle(clip_square, square_size);
                SRL::Scene2D::DrawSprite ( textureIndex,  center_sprite, 50.0 );             //draw the center sprite 
                SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::Clipping, SRL::Scene2D::ClippingEffect::ClipOutside);
                SRL::Scene2D::DrawSprite ( chkTexture,  second_sprite, 50.0 );               //draw the offset sprite   
                SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::Clipping);                
                SRL::Core::Synchronize();                                                    // Refresh screen
	}
```

Result:

![](img/spriteEffects_09.png)

### Gouraud

Gouraud Shading is a shading technique where colors are interpolated along vertices.
Before using the gouraud effect, we must first create a gouraud table.
This table contains color values to be used.
And then, when enabling the effect, we refer the table entry index for the first color to be applied.
To every vertex of the quad will applied the next color on the Gouraud Table.


This is done by means of :
```cpp
// Define Gouraud Table
HighColor* table = SRL::VDP1::GetGouraudTable();
table[0] = HighColor::Colors::Blue;
table[1] = HighColor::Colors::Green;
table[2] = HighColor::Colors::Yellow;
table[3] = HighColor::Colors::Magenta;
```

And we enable the effect by :

```cpp
while(1)
{       
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::Gouraud, 0);
        SRL::Scene2D::DrawSprite ( textureIndex,  center_sprite, 50.0 );             //draw the center sprite  
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::Gouraud);               
        SRL::Core::Synchronize();                                                    // Refresh screen
}
```

This is the result :

![](img/spriteEffects_10.png)

Notice that the table has `Blue` , `Green` , `Yellow` and `Magenta` values. And notice that the values are applied clockwise on the quad.

### OpacityBank

### EnableHSS

### EnableECD

### DisablePreClip
