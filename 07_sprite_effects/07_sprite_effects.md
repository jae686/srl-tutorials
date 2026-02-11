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

In order to set effects it is done via the `static void SRL::Scene2D::SetEffect	(const SpriteEffect	effect, const int32_t data = -1 )` function [documentation](https://srl.reye.me/classSRL_1_1Scene2D_a2f702eaaf22b82345520cce09908f386.html#a2f702eaaf22b82345520cce09908f386).

### Half Transparency

And now lets set a Half Transparency effect.
To do so, we must use the function `SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::HalfTransparency, true);` before our `SRL::Scene2D::DrawSprite` call.

The code then becomes :

```cpp
while(1)
	{       
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::HalfTransparency, true); //Enable the effect
        SRL::Scene2D::DrawSprite(textureIndex,  center_sprite, 50.0);                //draw the center sprite
        SRL::Scene2D::DrawSprite(chkTexture,  second_sprite, 50.0);                  //draw the offset sprite   
        SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::HalfTransparency);       //Disable the effect          
        SRL::Core::Synchronize();                                                    //Refresh screen
	}
```

![](img/spriteEffects_02.png)

### Screen doors

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

### Gouraud

### Clipping

### Flip

### OpacityBank

### EnableHSS

### EnableECD

### DisablePreClip
