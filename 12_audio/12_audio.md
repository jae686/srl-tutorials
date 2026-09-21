# Audio: A start

As for the audio options on the sega saturn, we can place it in two categories :

- CDDA audio (CD tracks)
- PCM Audio

## CDDA Audio

### Overview

CDDA audio is nothing more than music tracks stored on the CD. No CPU usage, very low memory requirements. Audio is streamed directly from the CD drive.

You put mp3, ogg or WAV files into the `cd/music` folder. If you used a sample project as recommended on [chapter 01](../01_hello_world/01_hello_world.md), the compile scripts will take care of generating the ISO with the corresponding audio tracks.

In order to interact with CDDA audio, SRL provides the [`SRL::Sound::Cdda`](https://srl.reye.me/classSRL_1_1Sound_1_1Cdda.html) class.

With this class we can :

- Play a single CD Audio track
- Play a range of CD audio tracks
- Set Volume
- Stop / Pause 

### CDDA usage

In order to use the CDDA audio tracks in your SRL project , you must:

- Set the option `SRL_USE_SGL_SOUND_DRIVER` to `1` in the `Makefile` of your project. Otherwise `SRL::Sound::Cdda` wont be available! (This will load the SGL sound driver).
- Add the audio files into the `cd/music` folder.
- Call `SRL::Sound::Cdda::PlaySingle(TrackNumber, false);`

> [!IMPORTANT]
> Track 1 is the DATA track of the CD !

> [!NOTE]
> Tracks are sorted by filename. In case of doubt , check the `.cue` file for track information.

Its that simple!

Below is the sample code using the CDDA functions:

```cpp

#include <srl.hpp>

// Using to shorten names for Vector and HighColor
using namespace SRL::Types;
using namespace SRL::Input;

int main()
{
    // Initialize library
    SRL::Core::Initialize(HighColor(0x31, 0x14, 0x32));
    SRL::Debug::Print(1,1, "Audio"); 

    Digital port(0);

    int track_nr = 2;

    // Main program loop
    while(1)
    {
        SRL::Debug::Print(1,2, "Track nr %d", track_nr);
        
        if(port.IsConnected())
        {
            if(port.WasPressed(SRL::Input::Digital::Button::A))
            {
                SRL::Sound::Cdda::PlaySingle(track_nr, false);
            }

            if(port.WasPressed(SRL::Input::Digital::Button::B))
            {
                SRL::Sound::Cdda::StopPause();
            }

            if(port.WasPressed(SRL::Input::Digital::Button::Up))
            {
                track_nr < 4 ? track_nr++ : track_nr = 4; 
            }

            if(port.WasPressed(SRL::Input::Digital::Button::Down))
            {
                track_nr > 2 ? track_nr-- : track_nr = 2;
            }
        }
        
        SRL::Core::Synchronize();      
    }

    return 0;
}

```

### CDDA Audio analysis

SRL also provides data analysis on the audio track being played.

This is provided through the [`SRL::Sound::Cdda::Analysis`](https://srl.reye.me/classSRL_1_1Sound_1_1Cdda_1_1Analysis.html) class.

We can get trough this class, in real time:

- The Volume Frequency Analysis (The volume of the High, Mid and Low Frequencies)
- The Volume on Right and Left channels.

To enable this, you must set the option `SRL_ENABLE_FREQ_ANALYSIS` to `1` in the `Makefile` of your project.
This will load a DSP program to perform this analysis. Obviously it requires the SGL sound driver.

In you program, you must start the analysis program. is is done by :

```cpp
SRL::Sound::Cdda::Analysis::Start();
```

Then, inside the render loop, we can obtain the volume of each frequency rage (Low, Mid, High) by using :

```cpp
SRL::Sound::Cdda::Analysis::GetFrequencyVolume();
```

Example :

```cpp
auto volume = SRL::Sound::Cdda::Analysis::GetFrequencyVolume();
auto highV = volume.Highs;
auto midV = volume.Mids;
auto lowV = volume.Lows;
```

> [!NOTE]
> We are using the `auto` keyword for convenience (this is modern c++). 
> The [`SRL::Sound::Cdda::Analysis::GetFrequencyVolume();`](https://srl.reye.me/classSRL_1_1Sound_1_1Cdda_1_1Analysis_ab2d7195e12d754ac5250584a3400fdf9.html#ab2d7195e12d754ac5250584a3400fdf9) returns a [`FrequencyVolume` struct](https://srl.reye.me/structSRL_1_1Sound_1_1Cdda_1_1Analysis_1_1FrequencyVolume.html). Inside this struck, resulting values for hig, mid and low have the `uint16_t` type.

For the whole volume we use :

```cpp
auto volumeT = SRL::Sound::Cdda::Analysis::GetTotalVolume();
```

Example :

```cpp
auto volumeT = SRL::Sound::Cdda::Analysis::GetTotalVolume();
auto leftC = volumeT.LeftChannel;
auto rightC = volumeT.RightChannel;
```

> [!NOTE]
> We are using the `auto` keyword for convenience (this is modern c++). The resulting values have the `uint16_t` type.

It might be useful to get those return values in the Fxp format (Fixed Point) so that we can use it for scaling
We can achieve this byte shifting to the left by 5 and creating a `Fxp` value from the result.

For example :

```cpp

Fxp volF = Fxp::BuildRaw(volumeT << 5);

```

Now we can use this to scale a quad according to the sound that is playing on CDDA.

We will get the sprite from [chapter 03](../03_first_sprite/03_sprites.md), and use the volume to scale the sprite:

First we load the sprite like we did on [chapter 03](../03_first_sprite/03_sprites.md#sprite-loading), on our `main()` before the render loop:

```cpp
//Load TGA
SRL::Bitmap::TGA *tga = new SRL::Bitmap::TGA("TEST.TGA"); // Loads TGA file into main RAM
int32_t textureIndex = SRL::VDP1::TryLoadTexture(tga);    // Loads TGA into VDP1
delete tga;
```

Then we get the volume from the `SRL::Sound::Cdda::Analysis`, get a fixed point value from it and use it to create a `Vector2D` with the scale factor:

```cpp
// get volume data
auto volumeT = SRL::Sound::Cdda::Analysis::GetTotalVolume();
auto leftC = volumeT.LeftChannel;
auto rightC = volumeT.RightChannel;           
Fxp VolF = Fxp::BuildRaw(leftC << 5);

//use the volume to create the scale vector, and pass it to DrawSprite
Vector2D scale = Vector2D(VolF + 1.0);
SRL::Scene2D::ZoomPoint zp = SRL::Scene2D::ZoomPoint::Center;
Angle SpriteAngle = Angle::FromDegrees(0.0);
SRL::Scene2D::DrawSprite(textureIndex, Vector3D(0.0, 0.0, 500), SpriteAngle, scale, zp);

```

https://github.com/user-attachments/assets/9d02f3dd-9917-4d5d-9c7d-63fd1bd442ae

## PCM Audio

PCM (Pulse Code Modulation) audio allows to play sound samples in real time on the saturn.
Currently SRL has support for 4 stereo channels. This means 4 stereo sounds can be played at the same time.

SRL provides the `SRL::Sound::Pcm` class to manage PCM sounds playback.
The samples are loaded into memory and then we play them as we need.

### File formats

The file formats supported are WAV and PCM.
The recommended sampling rate is 22Khz

In order to play any sound, we must first load them from the CD.

### WAV format

In order to load a WAV file, SRL provides the `SRL::Sound::Pcm::WaveSound` class.
Usage Example (code excerpt from SRL audio sample):

```cpp
SRL::Sound::Pcm::WaveSound* copter = lwnew SRL::Sound::Pcm::WaveSound("COPTER.WAV");
```

> [!NOTE]
> SRL provides the `lwnew` allocator. This allocator allocates the object into the low working ram (lwram) of the sega saturn.
> The `new` allocator allocates the new object into hwram.
> We will cover this in a later tutorial.

In order to play the sound, we invoke the [`PlayOnChannel`](https://srl.reye.me/classSRL_1_1Sound_1_1Pcm_1_1IPcmFile_a4b088aaafc2a0707a0306aae0ae3429e.html#a4b088aaafc2a0707a0306aae0ae3429e) or the [`Play`](https://srl.reye.me/classSRL_1_1Sound_1_1Pcm_1_1IPcmFile_a097c1192e9b51d15ea462e9eef033acf.html#a097c1192e9b51d15ea462e9eef033acf) method.

If you use the `Play` method, SRL will attempt to play on the first free channel.
If you use the `PlayOnChannel` method, you will have to specify the channel you wish to use. 

In both of these methods you can also define the volume (from 0 to 127) and the audio panning.

> [!NOTE]
> Audio panning will be covered at a late tutorial.

Example :

```cpp
copter->PlayOnChannel(0);
```

Using the audio from the SRL samples, we will modify our example to play a sound upon a keypress:

First we load our sound sample, inside our `main()` , before the render loop:

```cpp
 SRL::Sound::Pcm::WaveSound* copter = lwnew SRL::Sound::Pcm::WaveSound("COPTER.WAV"); // Stereo, 16bit
```

and inside our render loop, where we handle input we add:

```cpp
if(port.WasPressed(SRL::Input::Digital::Button::X))
{
    copter->Play();
}
```

Current example code for this tutorial:

```cpp
#include <srl.hpp>

// Using to shorten names for Vector and HighColor
using namespace SRL::Types;
using namespace SRL::Input;
using namespace SRL::Math::Types;

int main()
{
    // Initialize library
    SRL::Core::Initialize(HighColor(0x31, 0x14, 0x32));
    SRL::Debug::Print(1,1, "Audio"); 

    Digital port(0);

    int track_nr = 2;

    SRL::Sound::Cdda::Analysis::Start();

    Fxp min = 0.0;
    Fxp max = 0.0;

    //Load TGA
    SRL::Bitmap::TGA *tga = new SRL::Bitmap::TGA("TEST.TGA"); // Loads TGA file into main RAM
    int32_t textureIndex = SRL::VDP1::TryLoadTexture(tga);    // Loads TGA into VDP1
    delete tga;

    SRL::Sound::Pcm::WaveSound* copter = lwnew SRL::Sound::Pcm::WaveSound("COPTER.WAV"); // Stereo, 16bit

    // Main program loop
    while(1)
    {
        SRL::Debug::Print(1,2, "Track nr %d", track_nr);
        
        if(port.IsConnected())
        {
            if(port.WasPressed(SRL::Input::Digital::Button::A))
            {
                SRL::Sound::Cdda::PlaySingle(track_nr, false);
            }

            if(port.WasPressed(SRL::Input::Digital::Button::B))
            {
                SRL::Sound::Cdda::StopPause();
            }

            if(port.WasPressed(SRL::Input::Digital::Button::X))
            {
                copter->Play();
            }

            if(port.WasPressed(SRL::Input::Digital::Button::Up))
            {
                track_nr < 4 ? track_nr++ : track_nr = 4; 
                min = 0.0;
                max = 0.0;
            }

            if(port.WasPressed(SRL::Input::Digital::Button::Down))
            {
                track_nr > 2 ? track_nr-- : track_nr = 2;
                min = 0.0;
                max = 0.0;
            }
        }
        
        auto volumeT = SRL::Sound::Cdda::Analysis::GetTotalVolume();
        auto leftC = volumeT.LeftChannel;
        auto rightC = volumeT.RightChannel;           
              
        Fxp VolF = Fxp::BuildRaw(leftC << 5);

        if(VolF < min)
        {
            min = VolF;
        }

        if(VolF > max)
        {
          max = VolF;
        }

        SRL::Debug::Print(1,3, "curr %f , min %f, max %f", VolF, min, max);

        Vector2D scale = Vector2D(VolF + 1.0);
        SRL::Scene2D::ZoomPoint zp = SRL::Scene2D::ZoomPoint::Center;
        Angle SpriteAngle = Angle::FromDegrees(0.0);
        SRL::Scene2D::DrawSprite(textureIndex, Vector3D(0.0, 0.0, 500), SpriteAngle, scale, zp);

        SRL::Core::Synchronize();      
    }

    return 0;
}

```