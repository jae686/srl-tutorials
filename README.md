# A Tutorial series for Saturn Ring Library (SRL)

## Author note

> [!IMPORTANT]
> As of August 2025, this tutorial series is a work in progress. New chapters will be added and corrected as my personal time allows. 
> The Chapters as they are now may be changed as the tutorials are being written.

## Sega Saturn Primer

- [Hardware Description](hardware.md)

## Introduction to [SRL](https://github.com/ReyeMe/SaturnRingLib)

- [Description Of Library, Features , Alternatives](00_srl/srl_description.md)
- [SDK setup](https://github.com/ReyeMe/SaturnRingLib)
- [Best Practices](00_srl/srl_bestpractices.md)

## [01 - A simple hello world](01_hello_world/01_hello_world.md)

- Anatomy of a SRL project
- Introduction to `SRL::Debug`

## [02 - Your First ~~triangle~~ ~~point~~ / line / quad (VDP1)](02_line_quad/02_line_quad.md)

- 2D Screen Coordinates primer
  - Screen Coordinates
- `SRL::Scene2D`
  - A simple line
  - A simple quad

## [03 - Your First sprite (VDP1)](03_first_sprite/03_sprites.md)

- File System Primer
- Sprite constraints
- Sprite Loading
- Sprite Drawing
  - The Simplest Way
- `SRL::Math::Types::Angle` Introduction
- Sprite Rotation
- Working with Degrees and Radians
- Scaling the sprites
- The ZoomPoint

## [04 - Input Handling (Digital Gamepad)](04_input/04_input.md)

- ```SRL::Input::Digital``` class
- Is is Connected ?
- Is a Button being pressed ?
- Multiple buttons at the same time

## [05 - Interlude : Putting everything (so far) together](05_Interlude/05_interlude.md)

- Modularize code

## [06 - Your Second sprite (VDP1)](06_second_sprite/06_second_sprite.md)

- Distorted sprites

## [07 - Sprite effects](07_sprite_effects/07_sprite_effects.md)

- Half Transparency
- Screen doors
- Combining Effects
- Flip
- Clipping
- Gouraud
- High Speed Shrink
- Enable End-code-Disable  

## [08 - Backgrounds and tile maps (VDP2)](08_first_background/08_first_background.md)

- Screen Formats
  - Bitmap Format
    - Translate
    - Scale
  - Cell Format
    - Notes on SEGA's Editor
    - Using `SRL::Tilemap::Interfaces::Bmp2Tile` Interface
    - Using [buhan's saturn-aseprite](https://github.com/buhman/saturn-aseprite) [Aseprite](https://www.aseprite.org/) python script

## [09 - 3D Model pipeline](09_3D_model_pipeline_part_1/09_3D_model_pipeline_part_1.md)

- Caveats
- Description of the use of the tools for 3d mesh importing
- Obtaining the converter tool
- Preparing the our first model
- Getting the model into our saturn project
- Basic lightning
- Transforms
  - Rotation
  - Scaling
  - Translation
  - Be mindful of Transform order
  - Fixing the clipping issue

## [10 - 3D Model pipeline part 2](10_3D_model_pipeline_part_2/10_3D_model_pipeline_part_2.md)

- The matrix stack
- Textures and UV maps
- Face Properties

## [11 - A Primer on VDP2 Rotation Scroll Screen (VDP2)](11_second_background/11_second_background.md)

- A Primer on VDP2 Rotation Scroll Screen
  - Differences between rotation scroll screen and normal scroll screens
  - Filling the scroll screen
  - Rotation axis
  - OneAxis Rotation
    - Rotations on non-perpendicular axis
      - Rotation no X axis
      - Rotation on Y axis
      - Rotation on both axis
    - Two Axis Rotation
      - Applying transforms
      - Rotation effects
      - A note on translations
      - Scaling
  - Three Axis Rotation
    - A note on interaction with images from VDP1
    - Final Example code

## 12 - 3D Model Animation

- T.B.D.

## 13 - Input Handling - Revisited

- Peripheral management.
- Analogue Gamepad
- Light Pistol

## 14 - 2nd Interlude : putting everything together
