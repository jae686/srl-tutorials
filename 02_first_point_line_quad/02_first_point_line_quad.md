# Your First ~~triangle~~ point / line / quad / sprite (VDP1)

> [!NOTE]
> Since SRL is a SGL Wrapper , most of its concepts are interchangeable 


## 2D Coordinate system primer

On the sega Saturn, the screen has a 320 x 240.

For 2D Graphics, the sega saturn uses the following coordinate system :

![](img/Coordinate_System_2d.png)


## Vector Primer

TODO : add a small vector primer


## SRL::Scene2D

In order to draw things in 2D space, SRL supplies the `SRL::Scene2d` class.
This class allows you to draw, via the VDP 1, into the screen.

This class allows you to easily draw lines, quads and sprites.

### A simple line

To draw a simple line, we start with a simple SRL program, as described on the previous chapter.

```cpp
#include <srl.hpp>

// Using to shorten names for Vector and HighColor
using namespace SRL::Types;
using namespace SRL::Math::Types;

// Main program entry
int main()
{
  // Initialize library
  SRL::Core::Initialize(HighColor::Colors::Black);
  SRL::Debug::Print(1,1, "Tutorial 02");
  SRL::Core::Synchronize(); 
  return 0;
}
```

But before drawing the line we must describe the start and end points, its color and its Z coordinate. The Z coordinate is used for sorting.
For this we can use the `` SRL::Scene2D::DrawLine() `` (documentation)[https://srl.reye.me/classSRL_1_1Scene2D_ad028a771e97b80710000c3d46e2f4e50.html#ad028a771e97b80710000c3d46e2f4e50]

To define the start and end points we use 2D vectors, via the `` SRL::Math::Types::Vector2D`` . Since we use `using namespace SRL::Math::Types;` at the start, we can just declare the vector by `Vector2D`.

In this case, we want to draw a line from `(0,0)` to ``(319, 239)``









