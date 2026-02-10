# Sprite Effects

The VDP1 supports a series of ways to render the quads into the screen.
The effects are set via the `SRL::Scene2D::SetEffect` function.

## Baseline

Lets get back to a simple sprite :

```cpp
int main()
{
    // Initialize library
	SRL::Core::Initialize(HighColor::Colors::Black);
    SRL::Debug::Print(1,1, "07_Tutorial");
    int32_t textureIndex = loadTGA("TEST.TGA");    // Loads TGA into VDP1
    Vector2D center_sprite[4] = {Vector2D(0.0)}; // holds center sprite position.
    center_sprite[0] = Vector2D(-50, -50);
    center_sprite[1] = Vector2D( 50, -50);
    center_sprite[2] = Vector2D( 50,  50);
    center_sprite[3] = Vector2D(-50,  50); 

    // Main program loop
	while(1)
	{       
       //draw the center sprite
       SRL::Scene2D::DrawSprite ( textureIndex,  center_sprite, 50.0 );              
       // Refresh screen
       SRL::Core::Synchronize();
	}

	return 0;
}
```

And This is the result
![](../06_second_sprite/img/second_sprite_01.png)

## Setting Effects

In order to set effects it is done via the `static void SRL::Scene2D::SetEffect	(const SpriteEffect	effect, const int32_t data = -1 )` function [documentation](https://srl.reye.me/classSRL_1_1Scene2D_a2f702eaaf22b82345520cce09908f386.html#a2f702eaaf22b82345520cce09908f386). 


### Half Transparency

And now lets set a Half Transparency effect.
To do so, we must use the function `SRL::Scene2D::SetEffect(SRL::Scene2D::SpriteEffect::HalfTransparency, true);` before our `SRL::Scene2D::DrawSprite` call.

The code then becomes

### Screen doors

### Gouraud 

### Clipping 

### Flip

### OpacityBank 

### EnableHSS 

### EnableECD 

### DisablePreClip 
