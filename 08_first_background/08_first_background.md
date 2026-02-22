# A Primer on VDP2

While the VDP1 is responsible to draw the sprites in the foreground, the VDP2 is, in general, responsible to handle the backgrounds.
On the sega saturn, after the VDP1 draws the frame buffer, the image is handled over to the VDP2 for further processing.

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

## Our First Background

SRL provides, through the `SRL::VDP2` namespace the methods to interact and set or VDP2 scroll screens.