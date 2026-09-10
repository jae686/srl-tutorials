# Audio

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

This is provided through the `SRL::Sound::Cdda::Analysis` class.

We can get trough this class, in real time:

- The Volume Frequency Analysis (The volume of the High, Mid and Low Frequencies)
- The Volume on Right and Left channels.

To enable this, you must set the option `SRL_ENABLE_FREQ_ANALYSIS` to `1`.
This will load a DSP program to perform this analysis. Obviously it requires the SGL sound driver.

In you program, you must start the analysis program. is is done by :

```cpp
SRL::Sound::Cdda::Analysis::Start();
```

