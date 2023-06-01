# Using Splines (Spline Users)
Splines by themselves are intangible and invisible during runtime so to make use of a spline, the spline needs to be used. For that, Dreamteck Splines comes with a group of components called Spline Users. The Spline User components take a spline reference and utilize it to generate geometry, make objects follow splines, control particles/objects and much more. The Spline Users are all components in Unity and can be added to an object either through the editor or in runtime by calling the **AddComponent** method.

When an object with a Spline User component is selected in the editor, the referenced spline is automatically drawn.

Note that the Spline User component itself is rarely directly used since it doesn’t do anything. The Spline User component is used as a base for the rest of the components that utilize the splines. In the following chapters, various Spline User components are described.
## General Spline User Properties
![](./_images/072.png)

All Spline User components have several base properties. Starting with the top, the first and most important property is the Spline field which defines which spline this user is using. If a spline isn’t referenced, an error will be displayed in the inspector and the Spline User will not work.

Example in code:

```csharp
SplineUser genericUser = GetComponent();

genericUser.spline = mySpline;
```

### Clip Range
The segment of the spline that this user will be using. A Spline User could operate on just a fraction of the spline.

![](./_images/073.png)

The clip range is defined by the Clip From and Clip To values. By default, the editor displays the range with a MinMax slider:

This type of control prevents the “clipFrom” value from being bigger than the “clipTo” value. However, if the sampled spline is closed, the clip range can be looped, meaning that “clipFrom” can be bigger than “clipTo” and the samples will be looped across the spline’s connected start and end.

![](./_images/074.png)

Example in code:

```csharp
SplineUser genericUser = GetComponent();

genericUser.clipFrom = 0.25;

genericUser.clipTo = 0.75;
```

### Update Method
Similarly to the Spline Computer’s Update Method property – it defines in which update cycle the logic for this Spline User will be executed – Update, LateUpdate or FixedUpdate.
### Auto Rebuild
Should the Spline User automatically re-calculate if there is a change to the spline? If set to false, RebuildImmediate should be manually called to force the Spline User to update.

```csharp
SplineUser genericUser = GetComponent();

genericUser.RebuildImmediate();
```

### Multithreaded
Enables multithreading for the Spline User’s calculations. 

Note: For mesh generating Spline Users, only the vertex calculation operations are performed on the separate thread – the information still needs to be written to the Mesh object in the main Unity thread.
### Build on Awake
Forces the Spline User to re-calculate on Awake.
### Build on Enable
Forces the Spline User to re-calculate each time it is enabled.