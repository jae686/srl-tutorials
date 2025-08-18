#SRL
## Description
SRL is a wrapper , written in C++23 , that sits on top of SGL. SRL is opensource.

The main goal is to simplify the development process on the SEGA Saturn.

The library includes a class to deal with fixed point math, as efficently as possible on the sega saturn hardware, via the [SaturnMathPP](https://github.com/robertoduarte/SaturnMathPP) library, as well as classes to access resources such as the VDP1 and VDP2.

## Features
+ Has full support for TGA image format (including RLE compression) for loading textures/sprites.
+ Mesh and SmoothMesh simplify the process of handling 3D model draw calls. With the NYA exporter (not included,) it’s never been easier to draw a 3D scene on the Saturn.
+ Sega Saturn optimized math routives and types via [SaturnMathPP](https://github.com/robertoduarte/SaturnMathPP) 
+ CDDA playback with volume and 3 band analyzer.
+ Builds BIN/CUE images with embedded CDDA tracks.
+ VDP2 wrapper functions make it fast and easy to set up NBG and RGB layers with tilemap and bitmap data. A custom CRAM manager allows for easier management of 16, 64, 128, and 256 color palettes.
+ Custom memory allocation with support for allocating in LWRAM and with ability to automatically determine size of the available dynamic memory.


## Alternatives
+ [libyaul](https://github.com/yaul-org/libyaul). This is a opensource C library





