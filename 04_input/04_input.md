# Input Handling

For handling input the SRL library provides the `SRL::Input` namespace.

In order to keep the function calls short we will use ```cpp using namespace SRL::Input; ``` at the beginning of our file, similarly to what we've done with ```SRL::Types ``` and ```SRL::Math::Types``` .

## Digital Gamepad

### Quick start

First we must declare a ```cpp SRL::Input::Digital```. 
We must provide the port number to the constructor.
Since we already have declared at the beginning that we are also using the `SRL::Input` namespace, we can initiate it by :

```cpp
Digital port(0); // Initializes the controller port 0 (the first controller port)
```
This class stores the gamepad's button states during the execution of our program.

Once it is initialized, we must check its state inside the render loop.

Before reading the input from the gamepad, we must check if it's connected to the console.

This is done via `IsConnected()` method.

The code, to Initialize the gamepad port, and check if its connected, is shown below :

```cpp
#include <srl.hpp>

// Using to shorten names for Vector and HighColor
using namespace SRL::Types;
using namespace SRL::Math::Types;
using namespace SRL::Input; // Using to shorten names for input

int main()
{
    SRL::Core::Initialize(HighColor::Colors::Black); // Initialize library
    SRL::Debug::Print(1,1, "04_Tutorial");
    Digital port(0); // Initializes the controller port 0 (the first controller port)

    while(1) // Main program loop
	{   
      SRL::Debug::PrintClearLine(2);
      if(port.IsConnected())
      {
        SRL::Debug::Print(1,2, "Connected");

      }else
      {
        SRL::Debug::Print(1,2, "Not connected");
      }
      SRL::Core::Synchronize(); // Refresh screen
	}

	return 0;
}
```



