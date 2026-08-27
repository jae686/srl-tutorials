# 2nd Interlude : putting everything together in a simple project

## Main Goals

The main goal is to consolidate the topics covered on the previous tutorial series into a *simple* mario kart clone :

- Use VDP1 for sprites and 3D models
- Use VDP2 for background ,  ground plane and GUI elements
- Use Gamepad Input
- Make extensive use of C++ features (OOP)

In order to make this project we will start with the previous chapter as a baseline.

We will have classes representing the major blocks of the project, and a simple FSM (Finite state machine), to manage the state of the entities on it.

## A Primer on state machines

TODO

## Milestone 1

### Goals

The Goal of this milestone is to get the code of the [previous chapter](../11_second_background/11_second_background.md) and add user input.

In the end out cube must appear to run along the VDP2, based on user input.

All this while keeping the complexity manageable.

### Design

At this point we have 2 elements in the project :

- The cube.
- The VDP2 RBG0 plane.

The cube will be representing our *kart* (a cube for now), that will be represented into its own class, and the VDP2 RBG plane will represent the ground / track.
~
The camera will be behind the kart, and the kart will be at the center of the screen. Up and Down will move our cube forward and backward, relative to the cube's orientation, and Left and Right will rotate the cube along the Z axis.

### Implementation

#### Baseline

In order to represent the Kart, we will define the following class, into its own header :

```cpp

#pragma once
#include <srl.hpp>
#include "modelObject.hpp"

using namespace SRL::Types;
using namespace SRL::Math::Types;

class kart
{
    public :
    
    ModelObject mesh;
    Vector3D position = Vector3D();
    
    kart(const char* filename)
    {
        mesh.LoadFile(filename);
    };

    void Draw()
    {
        mesh.Draw();
    }
};

```

And we re-write our main file to make use of this class :

```cpp

#include <srl.hpp>
#include "modelObject.hpp"
#include "kart.hpp"

// Using to shorten names for Vector and HighColor
using namespace SRL::Types;
using namespace SRL::Math::Types;

int main()
{
    // Initialize library
    SRL::Core::Initialize(HighColor(0x31, 0x14, 0x32));
    SRL::Debug::Print(1,1, "SRL Interlude : Milestone 1");
    kart k("CUBE_X.NYA");
    Vector3D cameraLocation = Vector3D(12.5, -5.5, 0.0);
    Vector3D lightDirection = Vector3D(0.2, 0.0, 0.2);
    SRL::Scene3D::SetDirectionalLight(lightDirection);
    SRL::Scene3D::SetDepthDisplayLevel(4);

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
    SRL::VDP2::RBG0::SetRotationMode(SRL::VDP2::RotationMode::ThreeAxis);
    SRL::VDP2::RBG0::ScrollEnable();

    // Main program loop
     while(1)
    {
        SRL::Scene3D::LoadIdentity();
        SRL::Scene3D::LookAt(cameraLocation, Vector3D(), Angle::FromDegrees(0.0));
        SRL::Scene3D::RotateX(Angle::FromDegrees(90.0));
        SRL::VDP2::RBG0::SetCurrentTransform();        
        SRL::Scene3D::Scale(Vector3D(2.0)); 
        k.Draw();
        // Refresh screen
        SRL::Core::Synchronize();
                   
    }
    return 0;
}


```

This is the result :

![](img/12_Interlude_01.png)


At this point , have we Kart at location (0.0 , 0.0 , 0.0) and the camera is at (0.0, -5.5, -12.5).

> [!NOTE]
> Remember that our UP is -Y


#### Input

We will take a 




## Milestone 2

The goal of this milestone is to set a NGB plane (or planes) on the background and make them rotate to give a parallax effect.

## Milestone 3

The goal of this milestone is to add objects on top of the VDP2 plane

## Milestone 4

The goal is to introduce a simple game logic

## Milestone 5

T.B.D.


