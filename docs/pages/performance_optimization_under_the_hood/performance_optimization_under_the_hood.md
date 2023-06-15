# Performance, Oprimization and Under the Hood
Dreamteck Splines is optimized for speed and runs smoothly on all mobile devices, including older ones. This chapter will go over the main concepts behind the system’s design and shed some light on how the main components – the Spline Computer and the Spline User work.
## What impacts performance and what doesn’t?
Dreamteck Splines uses caching in order to minimize calculations. When a spline is created, it is being immediately cached based on its sample rate setting. The bigger the spline and the bigger the sample rate, the more samples will be generated. 

After the initial caching calculation, no other calculations will be performed unless the spline is changed. 

What counts as a spline change?

- Moving spline points
- Adding / Removing spline points
- Changing spline settings like sample rate, spline type and space
- Moving the spline’s transform when the Space is set to Local

The computational complexity and scale depend greatly on the spline settings. For example, if the Sample Mode of the spline is set to Default, moving a single spline point through code or inside the editor will only update the spline’s samples around that point and will not recalculate the entire spline. However, if the Sample Mode is set to Uniform or Optimized, the entire spline will be re-calculated. 

For splines, set to Local space, an additional Transform operation is run after the spline samples are calculated. If the Spline Computer’s Transform is changed, the Transform operation will run on all samples but the spline calculation will not be performed. This is different from the legacy Dreamteck Splines (version 1.1) where both the sample calculation and the transform are performed regardless of what is happening. 

Having many static Spline Computers in the scene costs nothing but memory since nothing is updated until it needs to update. Even if a lot of Spline Users are attached to the splines, no spline calculation operations will be performed as long as the spline objects are not changed.
## How Spline Users Work?
When a Spline User is added to a Spline, it runs its own logic using the already cached spline samples. This means that no spline calculations are performed. 

Similar to the Spline Computer, most Spline Users do not perform operations when not needed. For example, all Mesh Generators with the exception of the Spline Renderer, perform the mesh generation operation only when the spline changes or some property of the generators themselves is changed.

For the Spline Follower – the follow logic runs every frame on the cached spline and so many followers can be added to the same scene without performance drop. On mobile this number varies between 50 and 200 running simultaneously. However, if that many objects need to follow a single spline, consider using an Object Controller instead as it is meant exactly for this – to position lots of object on a spline and incrementing the “Evaluate Offset” property could give a similar result. 

The Particle Controller runs every frame and for every particle but it is still pretty speedy and suitable for mobile.

Probably the most taxing Spline User is the Spline Renderer component as it runs once for each camera, every frame. If the game has many cameras running simultaneously or there are many/big splines in the scene, using a Spline Renderer, the framerate could drop fast. Always consider using the Path Generator component first before jumping onto the Spline Renderer, especially for 2D games. 2D games cannot benefit from the Spline Renderer at all – there will not be any visual difference between using a Spline Renderer and a Path Generator in a 2D game. 
### What about the sample modifiers?
If a Spline User has sample modifiers, they are calculated each time the Spline User is updated so they do add a small overhead. However, each modifier has a range in which it operates and so calculations are not done outside this range.
## Does having many SplineComputer components in the scene hurt performance?
As mentioned above, the Spline Computer by itself doesn’t re-calculate the spline so there will be no Spline calculations. However, in order to ensure that the splines will be updated properly when the Spline Computer’s transform changes, there will be a couple of if-checks during update. This means that having several thousand SplineComputers in the scene will indeed take up some performance.How to optimize if a thousand SplineComputers are present?

The best way to optimize would be to disable all Spline Computer components. This will not disable the splines but will disable the checks every frame. Then whenever a spline needs updating, either enable while its needed or call ResampleTransform() on it.
## What happens when a scene, full of SplineComputers and SplineUsers loads?
Spline Computers and Users cache their values into the memory and everything gets serialized into the scene. By default, when a scene loads, all values will be de-serialized and no calculations will be performed. 

However, if a Spline Computer is set to rebuild on Awake (by toggling “Rebuild On Awake “in the inspector), as soon as the Spline becomes active, it will be re-calculated and all connected Spline Users will run their update logic.
## Common Methods
In Dreamteck Splines, a couple of methods with the same name can be found in several classes. These methods are:

- **Evaluate** – Evaluate the spline at a given percent [0-1] and return an entire object with values like position, direction, color, size, etc.
- **EvaluatePosition** – Evaluate the spline at a given percent [0-1] and return just a Vector3 object for the position
- **Travel** – Move along a spline in world units starting at a given percent. The returned result is a new percent that can be used to Evaluate the spline.
- **CalculateLength** – Calculates the distance in world units along the spline between two percentages
- **Project** – Returns the spline percent [0-1] that is closest to a given point in world space

They al can be found in the Spline class, as well as in SplineComputer, SplineUser and SampleCollection. 

“So what is the difference between all of them?”
### Methods in Spline
The Spline class represents a spline in world coordinates. It isn’t transformed by any transform and it isn’t used directly by any Spline User component. Instead, it is wrapped by the SplineComputer class. 
### Methods in SampleCollection
The SampleCollection is an object that holds a spline cache in the form of a list of samples. This class is used by the SplineComputer in order to store the cached samples. 

When calling the methods on a SampleCollection object, they get performed on the cached samples and no spline calculation operations are run.
### Methods in SplineComputer
Since the SplineComputer uses a SampleCollection object for its samples, calling one of these methods on a SplineComputer translates to calling the methods on a SampleCollection object – the methods are run on the cached samples. 

However, there is more to that. The SplineComputer versions of the methods all have an additional, optional parameter called “mode”. If the mode parameter is passed as SplineComputer.EvaluateMode.Calculate, then the spline will be calculated and the spline result will be transformed on the spot. This costs more performance but if pinpoint accuracy is needed, it can be very useful.
### Methods in SplineUser
Calling the methods on a SplineUser component results in calling them on the SplineComputer. The catch here is that the percentages are automatically mapped to the SplineUser’s clip range (clip from – clip to). 

For example, if the clip range is set to [0.0 – 0.5], calling **EvaluatePosition(1.0)** on the Spline User will return the position at 0.5.

This also applies for the returned percent for **Travel** and **Project**.