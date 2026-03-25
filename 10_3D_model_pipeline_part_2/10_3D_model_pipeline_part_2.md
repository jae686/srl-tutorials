# 3D Model pipeline in SRL - Part 2

## The Matrix Stack

As we described on the [previous chapter](../09_3D_model_pipeline_part_1/09_3D_model_pipeline_part_1.md), when you call `SRL::Scene3D::Translate` , `SRL::Scene3D::Scale` and `SRL::Scene3D::Rotate`, that SRL does behind the scenes is multiply the corresponding transform matrix with the SRL active matrix.

Then the mesh is transformed by this matrix when you call `Draw` : At this point SRL applies this matrix into the vertices of the model and then adds them to the VDP1 command table the transformed vertices for rendering.

However, as you draw multiple objects, it might be useful to store the active matrix for later. There is an example below:

Lets go back to our baseline from chapter 10 :

![](img/10_3D_model_pipeline_part_2_01.png)

Currently our model sits at `(0.0, 0.0, 0.0)`.

Lets draw another mesh at `(10.0, 0.0, 0.0)` , and another at `(-10, 0.0, 0.0)`.

```cpp
int main()
{
    // Initialize library
    SRL::Core::Initialize(HighColor::Colors::Black);
    SRL::Debug::Print(1,1, "10_Tutorial"); 
  
    ModelObject cube = ModelObject("CUBE01.NYA");
    Vector3D cameraLocation = Vector3D(12.5, -12.5, 12.5);
  
    // Setup light, we can use scale of the vector to manipulate light intensity
    Vector3D lightDirection = Vector3D(0.2, 0.0, 0.2);
    SRL::Scene3D::SetDirectionalLight(lightDirection);
    SRL::Scene3D::SetDepthDisplayLevel(4);

    // Main program loop
    while(1)
    {       
       // Load identity matrix
       SRL::Scene3D::LoadIdentity();
       // Set camera location and direction
       SRL::Scene3D::LookAt(cameraLocation, Vector3D(), Angle::FromDegrees(0.0));  
       cube.Draw();
       SRL::Scene3D::Translate(Vector3D(10.0, 0.0, 0.0));
       cube.Draw();
       SRL::Scene3D::Translate(Vector3D(-10.0, 0.0, 0.0));
       cube.Draw();
       SRL::Core::Synchronize();       
    }

    return 0;
}
```

![](img/10_3D_model_pipeline_part_2_02.png)

We have only 2 meshes despite having 3 `Draw()` calls.
This is because all transform operations are done on top of all previous ones.
On this example, by doing `SRL::Scene3D::Translate(Vector3D(-10.0, 0.0, 0.0));` from `(10.0, 0.0, 0.0)`, we get back to `(0.0, 0.0, 0.0)`.

In order to work around this we save the current active matrix into a matrix stack. Think of a matrix stack as a stack where we save our current transform, and allows us to easily revert back to it.

We can only do 2 operations with SRL's matrix stack : we can push a copy of the current active matrix into it, or retrieve it (pop).

When you pop a matrix from the stack, you remove the matrix from the top of the stack and set it as the active matrix.

You can push and pop the matrix stack using the `SRL::Scene3D::PushMatrix();` and `SRL::Scene3D::PopMatrix();`.

```cpp
int main()
{
    // Initialize library
    SRL::Core::Initialize(HighColor::Colors::Black);
    SRL::Debug::Print(1,1, "10_Tutorial"); 
  
    ModelObject cube = ModelObject("CUBE01.NYA");
    Vector3D cameraLocation = Vector3D(12.5, -12.5, 12.5);
  
    // Setup light, we can use scale of the vector to manipulate light intensity
    Vector3D lightDirection = Vector3D(0.2, 0.0, 0.2);
    SRL::Scene3D::SetDirectionalLight(lightDirection);    
    SRL::Scene3D::SetDepthDisplayLevel(4);

    // Main program loop
    while(1)
    {       
       // Load identity matrix
       SRL::Scene3D::LoadIdentity();
       // Set camera location and direction
       SRL::Scene3D::LookAt(cameraLocation, Vector3D(), Angle::FromDegrees(0.0));  
       cube.Draw();
       SRL::Scene3D::PushMatrix();  // Save the current transforms into the matrix stack
       SRL::Scene3D::Translate(Vector3D(10.0, 0.0, 0.0));
       cube.Draw();
       SRL::Scene3D::PopMatrix();   // Restored the current transforms from the matrix stack
       SRL::Scene3D::Translate(Vector3D(-10.0, 0.0, 0.0));
       cube.Draw();
       SRL::Core::Synchronize();       
    }

    return 0;
}
```



![](img/10_3D_model_pipeline_part_2_03.png)
