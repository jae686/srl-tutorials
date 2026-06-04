
# Note

> [!NOTE] Chapter not complete. T.B.C. during the summer

## Rotation

Only `RGB0` and `RGB1` can rotate.

First me must set the rotation axis. This is done via the `SetRotationMode()`.

There are 3 modes available :

| Enumerator | Description | Notes |
| -------- | -------------- | ---------- |
| OneAxis | 2d rotation with only roll and zoom | No additional VRAM requirements |
| TwoAxis | 3d rotation with pitch and yaw, but no roll (modified per line) | Requires 0x2000-0x18000 bytes in arbitrary VRAM Bank (No cycles) |
| ThreeAxis | Full 3d rotation with pitch, yaw and roll (modified per pixel) | Requires 0x2000-0x18000 bytes in Reserved VRAM bank (8 cycles) |

For the rotation, we use the  [`SetScale()`](https://srl.reye.me/classSRL_1_1VDP2_1_1NBG1_adcbf7bf416f13ef79e4462f83cdbe5e3.html#adcbf7bf416f13ef79e4462f83cdbe5e3) function.

Lets begin with a `OneAxis` rotation.