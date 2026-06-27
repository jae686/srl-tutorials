
# A Primer on VDP2 RGB0 and RGB1

A reminder on VDP2 layers, covered on the [08_first_background](../08_first_background/08_first_background.md) :

In the table below there is an overview:

| Scroll Screen Name | Name | Notes |
| ------------------ | ---- | ----- |
| Normal Scroll Screen | NBG0 | Can move up/down/left/right. Can Scale |
| Normal Scroll Screen | NBG1 | Can move up/down/left/right. Can Scale |
| Normal Scroll Screen | NGB2 | Can move up/down/left/right. |
| Normal Scroll Screen | NGB3 | Can move up/down/left/right. |
| Rotation Scroll Screen | RBG0 | Can Rotate / Scale |
| Rotation Scroll Screen | RBG1 | Can Rotate / Scale |

What makes the `RBG0` and `RBG1` layers distinct is the ability to rotate.
This allows for effects such as the famous "MODE 7" effect on games like Mario Kart on the SNES.


## Rotation axis

First me must set the rotation axis. This is done via the `SetRotationMode()`.

There are 3 modes available :

| Enumerator | Description | Notes |
| -------- | -------------- | ---------- |
| OneAxis | 2d rotation with only roll and zoom | No additional VRAM requirements |
| TwoAxis | 3d rotation with pitch and yaw, but no roll (modified per line) | Requires 0x2000-0x18000 bytes in arbitrary VRAM Bank (No cycles) |
| ThreeAxis | Full 3d rotation with pitch, yaw and roll (modified per pixel) | Requires 0x2000-0x18000 bytes in Reserved VRAM bank (8 cycles) |

For the rotation, we use the  [`SetScale()`](https://srl.reye.me/classSRL_1_1VDP2_1_1NBG1_adcbf7bf416f13ef79e4462f83cdbe5e3.html#adcbf7bf416f13ef79e4462f83cdbe5e3) function.

Lets begin with a `OneAxis` rotation.