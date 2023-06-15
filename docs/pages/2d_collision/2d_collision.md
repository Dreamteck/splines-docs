# 2D Collision
Dreamteck Splines has means of generating 2D colliders.
## Polygon Collider Generator
As the name suggests, this component generates a 2D Polygon Collider to be used with 2D physics. The collider is immediately updated on change in the editor and has an update interval property used to restrict the number of updates during runtime.

The Polygon Collider Generator has two modes:

- Path: creates a path along the spline, much like the Path Generator
- Surface: creates a solid shape, much like the surface generator

![](./_images/113.png)![](./_images/114.png)
## Edge Collider Generator
A very simple component that generates an edge collider. It has no custom settings.

Note: The edge collider will be generated at the position of the spline and therefore the spline rendering will cover the collider visualization making it seem like nothing is happening.