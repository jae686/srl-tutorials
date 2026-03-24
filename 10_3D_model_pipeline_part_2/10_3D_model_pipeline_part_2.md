# 3D Model pipeline in SRL - Part 2

## The Matrix Stack

As we described on the [previous chapter](../09_3D_model_pipeline_part_1/09_3D_model_pipeline_part_1.md), when you call `SRL::Scene3D::Translate` , `SRL::Scene3D::Scale` and `SRL::Scene3D::Rotate`, that SRL does behind the scenes is multiply the corresponding transform matrix with the SRL active matrix.

Then the mesh is transformed by this matrix when you call `Draw` : At this point SRL applies this matrix into the vertices of the model and then adds them to the VDP1 command table the transformed vertices for rendering.

However, as you draw multiple objects, it might be useful to store the active matrix for later. There is an example below:

Lets go back to our baseline from chapter 10 :

![](img/10_3D_model_pipeline_part_2_01.png)

