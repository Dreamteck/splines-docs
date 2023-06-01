# Spline Computer Settings
Moving down in the Inspector, below the Edit panel, there is the Spline Computer panel. This is where the general settings for the spline are located as well as some operations which can be performed on the splines. 

![](./_images/041.png)

## Type
The type of spline interpolation to be used. Dreamteck Splines supports four types of spline interpolation:

- Hermite

![](./_images/042.png)

- Bezier

![](./_images/043.png)

- B-Spline

![](./_images/044.png)

- Linear

![](./_images/045.png)

For the Bezier spline type, each point gets additional tangent points which control the interpolation of the curve (connected with a dotted line).
## Space
The coordinate system the spline should use:

- World – the spline uses world coordinates regardless of how the spline object’s transform is positioned, oriented and scaled
- Local – the spline will transform with the object’s transform


## Sample Mode

Splines, by default are evaluated with a [0-1] percent which spans across all spline points. For example, if a spline, consists of 3 points and it’s evaluated at 0.5 percent this will always return the second point’s position because it is in the middle. 

![](./_images/046.png)

However, if the first and the second points are very close to each other and the third is very far apart, evaluating at 0.5 percent will not return the middle of the spline because it will still be returning point 2.

![](./_images/047.png)

A lot of people who are new to splines encounter this issue when they try to extrude meshes or place objects along splines as some regions are denser than others. To illustrate this, here is the spline, drawn with thickness on:

![](./_images/048.png)

Each vertical line represents a spline sample. In this case, there are 10 samples between points 1 and 2 but there are also 10 samples in between points 2 and 3.

This is how the **Default** sample mode works. It is the fastest one in terms of performance but if the points are not distributed evenly, some spline regions may stretch or squash. There are three sample modes in Dreamteck Splines:

- Default – the default sample mode

![](./_images/049.png)

- Uniform – distributes the samples evenly along the spline but is heavy especially for larger splines

![](./_images/050.png)

- Optimized – same as Default but performs an optimization operation which removes unnecessary samples. This also comes at the cost of performance.

![](./_images/051.png)

Note that in Default and Optimized modes, when moving a control point, the splines only update the samples in the region that is affected by the point. In Uniform mode, the entire spline is re-calculated.

In Optimized mode, an additional slider is presented to control the angle threshold for optimization.

![](./_images/052.png)

## Update Mode
This property concerns when the splines are updated in runtime when there is a change to the spline’s object transform or one of the spline points. In a single frame, a spline can be modified by a script multiple times. For example, the spline type can be changed and then some spline points can be modified:

```csharp
SplineComputer spline = GetComponent();

spline.type = Spline.Type.BSpline;

spline.space = Space.Local;

spline.SetPoint(0, new SplinePoint(new Vector3(10f, 24f, 1f)));
```

Here, there are three operations on the spline. If the spline updated immediately after each operation, at the end of the frame, the spline would have re-calculated three times while it only needs to perform calculations once at the end of the command set. This is why Dreamteck Splines delays updates for the next frame by default. The Update Mode field defines when the update takes place.

- Update
- FixedUpdate
- LateUpdate
- AllUpdate – all three
- None – the spline will never update automatically during runtime and needs to be manually updated by calling Rebuild()

## Sample Rate
The resolution at which the spline is sampled at. Higher sample rate means higher resolution. The number defines how many spline samples there must be between each two control points.

|**Sample Rate: 4**|
| :-: |

![](./_images/053.png)

|**Sample Rate: 10**|
| :-: |

![](./_images/054.png)

## Rebuild on Awake
Should the Spline Computer rebuild as soon as it is active in the scene? This is often a good idea when instantiating splines from prefabs.
## Multithreaded
Should the Spline Computer use a separate thread for calculating the samples? This is useful when either the spline is big, the sample rate is very high, the sample mode is set to Optimized or Uniform or all of the aforementioned. 

Multithreaded splines might not update on the same frame. They usually lag one or two frames behind but that is only noticeable if an in-game spline editing is presented to the player (if the player has control over some spline points or properties). 
## Custom Interpolation
The custom interpolation foldout contains two buttons which are used to create animation curves that control how some control point values are interpolated as the spline is evaluated.

![](./_images/055.png)

What can be interpolated?

Interpolation happens for the points’ sizes, colors and normals. By default, linear interpolation is used but sometimes it is useful to have other types of interpolation. For example, when following a spline, it might be a good idea to use an ease-in, ease-out curve for the normal interpolation if point normals point in different directions.
### Value Interpolation
Value interpolation refers to the interpolation of point sizes and point colors together. For example, here is a spline (thickness drawing is enabled), consisting of only two control points. The left one has a size of 0.5 and the right one has a size of 2.

![](./_images/056.png)

This is how the size interpolation looks like when custom interpolation is used:

![](./_images/057.png)
### Normal Interpolation
How spline normals are interpolated in between two control points.